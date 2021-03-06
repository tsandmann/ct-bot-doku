# c't-Bot Remote-Calls

>> **Trac-2-Markdown Konvertierung:** *unchecked*

Stand: SVN Rev. 1887

## API der Remote-Calls

Auf das Kommando `CMD_REMOTE_CALL:SUB_REMOTE_CALL_LIST` schickt der Bot eine Liste mit den verfügbaren Remote-Calls an den c't-Sim. Jeder Listeneintrag kommt dabei als eigenes Kommando `CMD_REMOTE_CALL:SUB_REMOTE_CALL_ENTRY`. In der Payload steht neben dem Namen der Funktion auch, wie sie aufzurufen
ist und welche Parameter sie braucht. Das geschieht, indem die ganze interne Datenstruktur (siehe `remotecall_entry_t` in [behaviour_remotecall.h](https://github.com/tsandmann/ct-bot/blob/master/include/bot-logic/behaviour_remotecall.h)) übertragen wird.

Der PC kann jederzeit einen Remote-Call starten. Dazu schickt es das Kommando `CMD_REMOTE_CALL:SUB_REMOTE_CALL_ORDER`. In der Payload steht zuerst der Name der Funktion (null terminierter String). Danach kommen die Parameter. Jeder Parameter muss dabei (unabhängig von seiner tatsächlichen Länge) 32 Bit belegen.

Ist der Bot fertig, antwortet er mit `CMD_REMOTE_CALL:SUB_REMOTE_CALL_DONE` im DataL Feld steht eine 0, wenn das Verhalten nicht erfolgreich ausgeführt wurde. Steht dort eine 1, ist alles ok.

Um eigene Verhalten remote-aufrufbar zu machen, muss man ihre Botenfunktion nur in die calls-Struktur in [behaviour_remotecall.c](https://github.com/tsandmann/ct-bot/blob/master/bot-logic/behaviour_remotecall.c) eintragen. Wie das geht ist dort ausführlich beschrieben. Alles andere übernimmt das Framework.

## Implementierung der Remote-Calls

Hier gibt es eine Erklärung, wie die [Remote-Calls](https://github.com/tsandmann/ct-bot/blob/master/bot-logic/behaviour_remotecall.c) *intern* funktionieren. Es geht also in erster Linie um die Implementierung inkl. einiger Details und nicht darum, wie man die Remote-Calls benutzt.

### Der Kern

Die wesentliche Funktionalität der Remote-Calls ist als Bot-Verhalten implementiert, das mit der höchstens Priorität läuft und auf Anforderung andere Verhalten starten kann.

Wir betrachten nun zunächst schrittweise die Implementierung dieses Verhaltens und damit den Kern der Remote-Calls, nämlich die Funktion `void bot_remotecall_behaviour(Behaviour_t* data)`:

```C
void bot_remotecall_behaviour(Behaviour_t * data) {
 LOG_DEBUG("Enter bot_remotecall_behaviour");

 switch (running_behaviour) {
  case REMOTE_CALL_SCHEDULED: // Es laueft kein Auftrag, aber es steht ein neuer an
   LOG_DEBUG("REMOTE_CALL_SCHEDULED");
```

Für den Fall, dass ein neuer Remote-Call ansteht, wird dieses Codestück ausgeführt. Zum jetzigen Zeitpunkt sind durch den vorherigen Aufruf der Botenfunktion `bot_remotecall()` bereits einige Variablen initialisiert worden:

* `function_id` enthält die Remote-Call-ID des zu startenden Verhaltens (im Folgenden **X** genannt).
* `parameter_count` enthält die Anzahl der Parameter, die die Botenfunktion des Verhaltens X erwartet.
* `parameter_length` ist ein Array, das für jeden Parameter dessen Größe in Byte enhält.
* `parameter_data` ist ein Array, das die bereits korrekt formatierten Daten der Parameter für Verhalten X enthält.

Der folgende Code speichert und prüft die Remote-Call-ID von X und lädt anschließend die Adresse der Botenfunktion von X in den Funktionszeiger `func`. Im Falle des realen Bots liegt Letztere im Flash, um RAM zu sparen.

```C
   if (function_id >= STORED_CALLS) {
    LOG_DEBUG("keine Funktion gefunden. Exit");
    running_behaviour=REMOTE_CALL_IDLE;
    return;
   }

   #ifdef PC
    void (* func) (struct _Behaviour_t * data, ...);
    // Auf dem PC liegt die calls-Struktur im RAM
    func = (void *) calls[function_id].func;
   #else // MCU
    void (* func) (struct _Behaviour_t * data, remote_call_data_t dword1,
     remote_call_data_t dword2);
    // Auf dem MCU liegt die calls-Struktur im Flash und muss erst geholt werden
    func = (void *) pgm_read_word(&calls[function_id].func);
   #endif // PC
```

Für PC bekommt der Funktionszeiger eine variable Anzahl an Parametern, um auf Architekturen, die die Funktionsparameter nicht per Stack sondern in Registern übergeben, sowohl Integer- als auch Float-Parameter abzudecken (auf PPC werden die Parameter dann sowohl in die GP-Register als auch in die FP-Register kopiert - hierzu wird die Funktion `bot_remotecall_fl_dummy` aufgerufen).
Für MCU landen alle Parameter einer variablen Parameterliste auf dem Stack, unsere Botenfunktionen erwarten sie aber in Registern. Daher belegen wir hier acht Byte mit Parametern, "überschüssige" Parameter werden zwar in Register kopiert, von dort aber nie gelesen und stören deshalb auch nicht.

Nun gilt es drei Fälle zu unterscheiden:

* a) Die Botenfunktion von X erwartet außer dem obligatorischen Zeiger `*caller` auf den Aufrufer keine Parameter. Der Zeiger des Aufrufers wird hier grundsätzlich nicht mitgezählt.
* b) Die Botenfunktion von X erwartet Parameter, aber maximal `REMOTE_CALL_MAX_PARAM` viele.
* c) Die Botenfunktion von X erwartet mehr Parameter, als von den Remote-Calls unterstützt werden.

Im Fall c) passiert nichts, da wir die Parameter nicht korrekt übergeben können:

```c
   if (parameter_count > REMOTE_CALL_MAX_PARAM) {
    LOG_DEBUG("Parameteranzahl unzulaessig!");
    running_behaviour=REMOTE_CALL_IDLE;
    return;
   }
```

Der Fall a) ist sehr einfach, wird im Code aber nicht extra behandelt, s.u.

Interessant wird es nun im Fall b). Vorweg ein paar Worte, was hier grundsätzlich zu tun ist:

Die Parameterdaten stehen im Array `parameter_data` und `func` zeigt auf die Botenfunktion. Wenn wir den Typ der Parameter einfach mal außer Acht lassen, müssen wir jetzt folgendes tun:

```C
func(data, parameter_data[0], ..., parameter_data[3]);
running_behaviour = REMOTE_CALL_RUNNING;
return;
```

Also die Botenfunktion mit der Anzahl der zugehörigen Parameter aufrufen und anschließend auf den Status `REMOTE_CALL_RUNNING` umschalten.

Das Problem ist hier nur, dass die Paramter unterschiedlich groß und unter Umständen größer als die Maschinenwortbreite der Zielarchtitektur sein können. Es reicht also nicht, je nach Anzahl der Parameter einen Funktionszeiger mit n Parametern anzulegen und diesen aufzurufen.
Stattdessen kommt hier der oben bereits kurz genannte Trick zum Einsatz: Wir übergeben dem Funktionszeiger einfach soviel Parameter, wie es maximal geben kann. Sind das mehr, als für das eigentliche Verhalten nötig, liegen diese zwar auf dem Stack oder in Registern, stören dort aber auch nicht weiter.
Für PC ist der Fall sehr einfach, da die maximale Parameterbreite kleinergleich der Maschninenwortbreite und somit der Register- oder Stackbreite ist. Ein uint8_t Parameter belegt auf einer 32 Bit Architektur z.B. immer 4 Byte auf dem Stack bzw. ein ganzes 32 Bit Register. Hier haben wir also genau das Format, in dem die Parameter auch vom Sim kommen.
Für MCU ist es etwas komplizierter, hier werden alle Parameter in 8 Bit-Registern übergeben, aber 16 Bit aligned:

1. Die Größe in Byte wird zur nächsten geraden Zahl aufgerundet, falls sie ungerade ist.
1. Der Registerort fängt mit 26 an.
1. Vom Registerort wird die berechete Größe abgezogen und das Argument in diesen Registern übergeben (LSB first).

Diese Konvertierung erledigt die Funktion `remotecall_convert_params`.

Der folgende Code führt nun zum gewünschten Ergebnis:

```C
   LOG_DEBUG("function_id=%u", function_id);
   LOG_DEBUG("parameter_count=%u", parameter_count);
   #ifdef PC
    bot_remotecall_fl_dummy(data, (*(remote_call_data_t*)parameter_data).fl32,
     (*(remote_call_data_t*)(parameter_data+4)).fl32,
     (*(remote_call_data_t*)(parameter_data+8)).fl32);
    func(data, *(remote_call_data_t*)parameter_data,
     *(remote_call_data_t*)(parameter_data+4),
     *(remote_call_data_t*)(parameter_data+8));
   #else // MCU
    func(data, *(remote_call_data_t*)(parameter_data+4),
     *(remote_call_data_t*)parameter_data);
   #endif // PC
```

Die Dummy-Funktion ist nur für die PowerPC-Architektur wichtig (s.o.), der eigentliche Funktionsaufruf im PC-Fall entspricht dem ersten Ansatz.
Für MCU bleibt lediglich noch zu beachten, dass die Parameterdaten hier in umgekehrter Reihenfolge übergeben werden müssen, weil mit aufsteigenden Parametern absteigende Register verwendet werden (s.o.).

Anschließend aktualisieren wir noch den Status und verlassen die Verhaltens-Funktion:

```C
   running_behaviour=REMOTE_CALL_RUNNING;
   return;
```

Ist das aufgerufene Verhalten fertig, wird wieder das Remote-Call-Verhalten aktiv und erledigt mit folgendem Code den Rest:

```C
  case REMOTE_CALL_RUNNING: // Es lief ein Verhalten und ist nun zuende
  {
   // Antwort schicken
   char * function_name;
   #ifdef PC
    function_name=(char*)&calls[function_id].name;
   #else
    // Auf dem MCU muessen wir die Daten erstmal aus dem Flash holen
    char tmp[REMOTE_CALL_FUNCTION_NAME_LEN+1];
    memcpy_P(tmp, &calls[function_id].name, REMOTE_CALL_FUNCTION_NAME_LEN+1);
    function_name=(char*)&tmp;
   #endif // PC

   #ifdef COMMAND_AVAILABLE
    int16 result = data->subResult;
    command_write_data(CMD_REMOTE_CALL,SUB_REMOTE_CALL_DONE,&result,&result,function_name);
    LOG_DEBUG("Remote-call %s beendet (%d)",function_name,result);
   #endif

   // Aufrauemen
   function_id=255;
   running_behaviour=REMOTE_CALL_IDLE;
   return_from_behaviour(data); // und Verhalten auch aus
   break;
  }
  default:
   return_from_behaviour(data); // und Verhalten auch aus
   break;
 }
}
```

Das beschränkt sich auf bekannte Funktionen: Daten aus dem Flash laden, Ergebnis über die Command-Schnittstelle an den Sim senden und den eigenen Status aktualisieren. Daraufhin können wir das Verhalten verlassen.

### Hilfsfunktionen

Um die vom c't-Sim übergebenen Parameter für die verschiedenen Zielarchitekturen korrekt aufzubereiten, gibt es die Hilfsfunktion **`remotecall_convert_params`**.
Falls die Zielarchitektur ein x86-System ist, gibt es für `remotecall_convert_params` nicht viel zu tun, die Daten werden einfach 1 zu 1 in den Zielpuffer kopiert (Datenformat *Little-Endian*, *32-Bit alignment* - genau so kommen die Daten vom c't-Sim über die Leitung).
Für die AVR-Architektur sieht das Ganze etwas umfangreicher aus:

```C
static void remotecall_convert_params(uint8_t * dest, uint8_t count, uint8_t * len,
 uint8_t * data) {
#ifdef MCU
 dest += 8; // ans Ende springen
```

* `dest` ist ein Zeiger auf den Zielpuffer, der anschließend die konvertierten Daten enthalten wird.
* `count` gibt die Anzahl der im Datenstrom enthaltenen Parameter an.
* `len` ist ein Zeiger auf ein Array, das für jeden Parameter dessen Größe in Byte enthält.
* `data` ist ein Zeiger auf den Quelldatenstrom.

Da wir die Parameter rückwärts in den Puffer schreiben (Grund ist die Registerzuordnung des gcc auf der AVR-Architektur), lassen wir `dest` zunächst auf den letzten Eintrag zeigen.

```C
 uint8_t i;
 /* Parameter rueckwaerts einlesen */
 for (i=0; i<count; i++) {
  uint8_t pos = len[i] > 1 ? len[i] : 2; // alle Parameter sind 16-Bit-aligned
  dest -= pos;
  memcpy(dest, data, len[i]);
  data += sizeof(remote_call_data_t);
 }
```

Hier geschieht nun das eigentliche Konvertieren. Pro Parameter wird in der for-Schleife als erstes dessen Länge ermittelt. Für den Fall, dass es sich um einen 8-Bit-Datentyp handelt, nehmen wir trotzdem 2 Byte an, wie es uns die ABI vorschreibt. 24 Bit-Datentypen werden nicht unterstützt.
Der Zielpuffer wird um die Länge des Parameters runtergezählt, anschließend werden die zum Parameter gehörenden Daten per `memcpy()` einfach kopiert.
Der Zeiger auf den Quelldatenstrom wird immer um 4 Byte erhöht, denn die Daten kommen ungepackt (32-Bit alignment) vom Sim.

Wie bereits erwähnt, gibt es im Fall einer Little-Endian-PC-Architektur nicht viel zu tun:

```C
#else // PC
#if BYTE_ORDER == LITTLE_ENDIAN
 /* Daten einfach kopieren */
 memcpy(dest, data, count*sizeof(remote_call_data_t));
```

Für den Fall, dass wir ein Big-Endian-System haben, müssen wir sämtliche Parameter "umdrehen", da die 32 Bit große Struktur `remote_call_data_t` hier genau entgegengesetzt im Speicher liegt:

```C
#else // BIG_EDIAN
 uint8_t i;
 for (i=0; i<count; i++) {
  /* Parameter i von little-endian nach big-endian konvertieren*/
  remote_call_data_t in, out;
  in = *((remote_call_data_t *)data);
  out.u32 = ((in.u32 & 0xff) << 24) | ((in.u32 & 0xff00) << 8) |
   ((in.u32 & 0xff0000) >> 8) | ((in.u32 & 0xff000000) >> 24);
  memcpy(dest, &out, sizeof(remote_call_data_t));
  dest += sizeof(remote_call_data_t);
  data += sizeof(remote_call_data_t);
 }
#endif // LITTLE_EDIAN
#endif // MCU
```

Eine zweite Hilfsfunktion ist **`getRemoteCall`**, die in der Remote-Call-Tabelle nach einem Funktionsnamen sucht und die zugehörige ID zurückliefert:

```C
static uint8 getRemoteCall(char * call) {
 LOG_DEBUG("Suche nach Funktion: %s",call);
 uint8 i;
 for (i=0; i< (STORED_CALLS); i++) {
  if (!strcmp_P (call, calls[i].name)) {
   LOG_DEBUG("calls[%d].name=%s passt",i,call);
   return i;
  }
 }
 return 255;
}
```

Auf dem echten Bot liegen diese Daten im Flash-Speicher, die Funktion `strcmp_P` berücksichtigt das. Im PC-Fall wird sie per #define einfach durch `strcmp` ersetzt.

### Botenfunktion

Zum Starten eines Remote-Calls wird wie bei allen Verhalten die *Botenfunktion* aufgerufen. Sie bekommt den Namen des zu startenden Verhaltens und die Parameterdaten als Zeiger übergeben:

```C
void bot_remotecall(Behaviour_t* caller, char* func, remote_call_data_t* data) {
```

Zunächst wird überprüft, ob das gewünschte Verhalten in der Remote-Call-Tabelle existiert und seine ID ermittelt:

```C
 function_id = getRemoteCall(func);
 if (function_id >= STORED_CALLS){
  LOG_ERROR("Funktion %s nicht gefunden. Exit!", func);
  return;
 }
```

Nun können wir das Verhalten aktivieren und müssen nur noch die restlichen der anfangs erwähnten Voraussetzungen schaffen, also die Parameterdaten holen und konvertieren:

```C
 switch_to_behaviour(caller, bot_remotecall_behaviour, NOOVERRIDE);
 #ifdef PC
  parameter_count = calls[function_id].param_count;
  parameter_length = (uint8*)calls[function_id].param_len;
 #else
  // Auf dem MCU muessen wir die Daten erstmal aus dem Flash holen
  parameter_count = pgm_read_byte(&calls[function_id].param_count);
  memcpy_P(parameter_length, &calls[function_id].param_len, parameter_count);
 #endif // PC

remotecall_convert_params(parameter_data, parameter_count, parameter_length, (uint8*)data);
```

Abschließend aktualisieren wir lediglich noch den internen Status und lassen das Verhalten arbeiten: :-)

```C
 running_behaviour = REMOTE_CALL_SCHEDULED;
}
```

[![License: CC BY-SA 4.0](../license.svg)](https://creativecommons.org/licenses/by-sa/4.0/)
