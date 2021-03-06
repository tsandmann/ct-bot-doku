# Dokumentation des Protokolls zwischen ct-Bot und ct-Sim

>> **Trac-2-Markdown Konvertierung:** *unchecked*

<P>
<p>
 Hauptteil des Protokolls zwischen c't-Sim und Bot-Steuercode (sog.
 <em>c't-Bot-Protokoll</em>). Eingesetzt wird diese Klasse vorwiegend in
 zwei F&auml;llen:
 <ol>
 <li>Ein realer (in Hardware existierender) Bot, der per USB oder TCP
 verbunden ist, sendet laufend Messwerte der Sensoren und andere
 Statusinformationen. 12 Byte auf dem Draht stellen die Messwerte eines
 Sensorpaars dar, z.B. linker und rechter Distanzsensor; die Aufgabe dieser
 Klasse ist das Lesen und Repr&auml;sentieren dieser 12 Byte innerhalb des
 Sim.</li>
 <li>Ein simulierter Bot (Bot-Steuercode, auf einem PC l&auml;uft) bekommt
 &uuml;ber seine TCP-Verbindung vom Sim Sensorwerte gef&uuml;ttert. Der Sim
 speichert die Werte in einem Command-Objekt, das er anschlie&szlig;end ins
 TCP schreibt.</li>
 </ol>
 </p>
 <p>
 Diese Klasse behandelt das Dekodieren eines Commands aus einem Haufen Bytes
 (siehe Konstruktor) und das Enkodieren
 eines Commands (siehe <CODE>getCommandBytes()</CODE>). Verwendet und
 interpretiert werden die Commands in den Bot-Komponenten. Die betreffenden
 Komponenten stehen in der Liste der Command-Codes.
 </p>
 <p>

</P>

## Beispiel eines Command

Die genaue Definition des Datentyps findet sich in der Datei `include/command.h` respektive `ctSim/model/Command.java`.

<P>
 <pre>
 Byte#         Wert            Bedeutung

   0           '&gt;' (Ascii 62)  Startcode, markiert Beginn des Command, ist
                               immer '&gt;'
   1           'H' (Ascii 72)  Command-Code, hier der f&uuml;r die
                               Helligkeitssensoren
   2 Bit 0     0               Richtungsangabe, 0 = Anfrage, 1 = Antwort. Ist
                               historisch und steht immer auf 0.
   2 Bits 1-7  'N' (Ascii 78)  Sub-Command-Code: Nur von einigen Command-Codes
                               verwendet, ist normalerweise N (&quot;normal&quot;)
   3           0               L&auml;nge der Nutzlast in Byte, hier: keine
                               Nutzlast
   4           42              Datenfeld &quot;dataL&quot; LSB: niederwertiges Byte des
                               Messwerts des linken Helligkeitssensors
   5           1               dataL MSB: h&ouml;herwertiges Byte des Messwerts
   6           37              dataR LSB: niederwertiges Byte rechter
                               Helligkeitssensor
   7           0               dataR MSB: zugeh&ouml;riges h&ouml;herwertiges Byte
   8           61              Sequenznummer LSB, bei aufeinanderfolgenden
                               Commands erh&ouml;ht sich die Sequenznummer immer
                               um eins
   9           0               Absender-Id des Paketes
  10           0               Empaenger-Id des Paketes
  11           '&lt;' (Ascii 60)  CRC-Code, markiert Command-Ende, ist immer '&lt;'
                               (Name &quot;CRC&quot; irref&uuml;hrend)
  12 und folgende              Nutzlast falls vorhanden. Wird z.B. verwendet,
                               wenn der Bot den Inhalt des LCD &uuml;bertr&auml;gt oder
                               die Bilddaten, was der Maussensor sieht
 </pre>

 </p>
</P>

## Verfügbare Kommandos

Eine Liste mit allen Verfügbarenn Kommandos befindet sich im C-Code in der Datei include/command.h

## Ablauf der Kommunikation

<P>
 <p>
 F&uuml;r einen <strong>realen Bot</strong> besteht das Protokoll aus
 folgenden Regeln:
 <ul>
 <li>Der Bot-Steuercode sendet laufend Commands mit Sensor-Messwerten und
 anderen Statusinformationen, die der Sim auswertet und dem Benutzer anzeigt.
 Welche einzelnen Commands behandelt werden, und wie sie im Detail
 interpretiert werden, ist Sache der Bot-Komponenten wie in der
  Command-Code-Liste beschrieben. </li>
 <li>Beim Start des Sim &uuml;bertr&auml;gt er ein Command mit dem
 Command-Code <CODE>WELCOME</CODE>, das einen Handshake anfordert.
 Der Bot antwortet mit einem Command, das ihn als realen Bot ausweist
 (Command-Code WELCOME, Sub-Command-Code
<CODE>WELCOME_REAL</CODE>). Falls der Bot schon l&auml;uft,
 wenn der Sim die Verbindung aufbaut, sendet der Sim trotzdem ein
 <code>WELCOME</code>; bei USB-Verbindungen (COM-Verbindungen) schickt der Sim
 kein <code>WELCOME</code>, da von vornherein klar ist, dass es ein Real-Bot sein
 muss. Aus Sicht des Bot kann ein Handshake also jederzeit kommen oder
 &uuml;berhaupt nie.</li>
 <li>Jederzeit w&auml;hrend der Verbindung kann der Sim ein Command senden,
 das das Bild anfordert, das der Maussensor auf der Botunterseite sieht
 (Command-Code <CODE>SENS_MOUSE_PICTURE</CODE>). Der Bot
 beantwortet das mit einer Serie von Commands mit dem Aufbau, der in
 <CODE>MousePictureComponent</CODE> beschrieben ist</li>
 <li>Jederzeit w&auml;hrend der Verbindung kann der Sim ein Command senden,
 das einen Befehl der RC5-Fernbedienung repr&auml;sentiert (Command-Code
 <CODE>SENS_RC5</CODE>)</li>
 </ul>
 </p>
 <p>
 F&uuml;r einen <strong>simulierten Bot</strong> ist das Protokoll:
 <ul>
 <li>Der Sim lauscht auf dem TCP-Port, der in der Konfigdatei angegeben ist
 (Parameter "botport").</li>
 <li>Bot-Steuercode verbindet sich mit dem TCP-Port. Der Sim sendet ein
 Command mit dem <CODE>WELCOME</CODE>, das einen Handshake anfordert.
 Der Bot antwortet mit einem Command, das ihn als simulierten Bot ausweist
 (Command-Code WELCOME, Sub-Command-Code
 <CODE>WELCOME_SIM</CODE>).</li>
 <li>Der Sim sendet einen Block von Commands, die Sensorwerte beschreiben. Er
 ist abgeschlossen mit einem Command, was den Command-Code
 <CODE>DONE</CODE> hat. Beispiel:

 <pre>
 Richtung  Command-Code         dataL dataR

 Sim->Bot: SENS_MOUSE           L   0 R   0 Payload=''
 Sim->Bot: SENS_DOOR            L   0 R   0 Payload=''
 Sim->Bot: SENS_BORDER          L 690 R 690 Payload=''
 Sim->Bot: SENS_LDR             L 965 R 965 Payload=''
 Sim->Bot: SENS_IR              L 100 R  80 Payload=''
 Sim->Bot: SENS_ERROR           L   0 R   0 Payload=''
 Sim->Bot: SENS_RC5             L   0 R   0 Payload=''
 Sim->Bot: SENS_TRANS           L   0 R   0 Payload=''
 Sim->Bot: SENS_LINE            L 690 R 690 Payload=''
 Sim->Bot: SENS_ENC             L   0 R   0 Payload=''
 Sim->Bot: DONE                 L  30 R   0 Payload=''</pre>

 Wenn ein Bot andere Sensoren h&auml;tte als der normale c't-Bot, s&auml;he
 die Liste anders aus. Manche Sensoren senden nur, wenn sie etwas zu senden
 haben (Beispiel f&uuml;r dieses Verhalten: SENS_MOUSE_PICTURE, also die
 <CODE>MousePictureComponent</CODE>). Das DONE-Command enth&auml;lt die aktuelle
 Simulatorzeit in Millisekunden, falls der Bot zeitabh&auml;ngige Sachen
 rechnen will. </li>
 <li>Der Bot berechnet auf Basis der simulierten Sensorwerte seine
 n&auml;chsten Aktionen. Dann sendet er einen Block von Commands mit
 Aktuatorwerten. Er ist abgeschlossen mit einem Command, was den Command-Code
 <CODE>DONE</CODE> hat. Beispiel:

 <pre>
 Richtung  CmdCode/SubCmdCode   dataL dataR

 Bot->Sim: ACT_LED              L 128 R 128 Payload=''
 Bot->Sim: ACT_MOT              L -24 R  24 Payload=''
 Bot->Sim: ACT_LCD/LCD_CURSOR   L   0 R   0 Payload=''
 Bot->Sim: ACT_LCD/LCD_DATA     L   0 R   0 Payload='P=3C5 3C5 D=999 999 '
 Bot->Sim: ACT_LCD/LCD_CURSOR   L   0 R   1 Payload=''
 Bot->Sim: ACT_LCD/LCD_DATA     L   0 R   0 Payload='B=2B2 2B2 L=2B2 2B2 '
 Bot->Sim: ACT_LCD/LCD_CURSOR   L   0 R   2 Payload=''
 Bot->Sim: ACT_LCD/LCD_DATA     L   0 R   0 Payload='R= 0  0 F=0 K=0 T=0 '
 Bot->Sim: ACT_LCD/LCD_CURSOR   L   0 R   3 Payload=''
 Bot->Sim: ACT_LCD/LCD_DATA     L   0 R   0 Payload='I=1005 M=00000 00000'
 Bot->Sim: DONE                 L  30 R   0 Payload=''</pre>

 Das DONE-Command enth&auml;lt zur Best&auml;tigung die Simulatorzeit, die vom
 c't-Sim zuletzt empfangen wurde. </li>
 </li>
 Falls der Sim vom Benutzer pausiert wird, kann beliebig viel Armbanduhrenzeit
 zwischen einem Block und dem n&auml;chsten liegen. Die Simulatorzeit
 (DONE-Command) wird davon jedoch nicht beeinflusst. Bot-Steuercode sollte
 sich also auf die Simulatorzeit verlassen, nicht auf Armbanduhrenzeit.
 <li>
 Die Endianness auf dem Draht bei uns ist <a
 href="http://de.wikipedia.org/wiki/Little_endian">Little-Endian</a>. Java
 verwendet intern Big-Endian. Die Konvertierung erfolgt zu Fu&szlig; in der Command-Klasse.
 </ul>
 </p>

[![License: CC BY-SA 4.0](../license.svg)](https://creativecommons.org/licenses/by-sa/4.0/)
