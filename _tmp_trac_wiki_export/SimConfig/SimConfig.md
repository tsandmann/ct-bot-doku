# Übersicht der in ct-sim.xml konfigurierbaren Optionen

>> **Trac-2-Markdown Konvertierung:** *unchecked*

Stand: Release 23

Jeder Parameter kann bei Bedarf ein Attribut "os" haben. So kann man verschiedene Werte für verschiedene Betriebssysteme angeben. Hat ein Parameter kein os-Attribut, gilt er für alle Systeme.

* **ctSimTickRate**: Initialer Wert für Simulationsgeschwindigkeit des Simulators in 10 ticks pro Sekunde, 0 für maximale Geschwindigkeit. *default=0*
* **ctSimTimeout**: Die Zeit in ms, die der Controller maximal auf einen Bot wartet. *default=500*
* **simBotErrorHandling**: Nur Simulierte Bots: Was passiert, wenn ein schwerer Fehler auftritt (z.B. die TCP-Verbindung abbricht weil der Bot-Code abgestürzt ist)? "kill" = Bot aus Simulation entfernen (Standardverhalten), "halt" = Bot in Simulation lassen, auch wenn er sich nicht mehr bewegen kann. *default=kill*
* **parcours**: Die Welt, die per default geladen wird. Angabe unabhängig von worlddir. *default=parcours/testparcours2.xml*
* **worlddir**: Das Verzeichnis, in dem alle Weltdateien liegen. *default=./parcours*
* **botport**: Der TCP-Port, auf dem der Sim auf Bots lauscht. *default=10001*
* **BotAutoStart**: Aktiviert (*true*) oder deaktiviert (*false*) den Autostart eines Bots, wie in botbinary (siehe unten) angegeben. *default=false*
* **botbinary**: Die Bot-Datei, die beim Sim-Start gestartet wird. *default=../ct-!Bot/Debug-W32/ct-Bot.exe*
* **botdir**: Verzeichnis, das der Dialog "Bot-Datei öffnen" anfangs anzeigt. *default=../ct-!Bot/Debug-W32*
* **judge**: Der Klassenname des Judges, den der Sim per default lädt. *default=ctSim.model.rules.!DefaultJudge*
* **TimeLogger**: Aktiviert/deaktiviert den TimeLogger, der während einer Simulation einige Performance-Daten auf die Konsole schreibt. *default=false*
* **LogLevel**: Stellt das Log-Level ein. Werte: SEVERE, WARNING, INFO, CONFIG, FINE, FINER, FINEST. *default=INFO*
* **RC5-type**: Fernbedienung, die verwendet werden soll. Namen wie in rc5-codes.h beim Bot-Code. *default=RC_HAVE_HQ_RC_UNIVERS29_334*
* **rcStartCode**: Fernbedienungscode, der an simulierte Bots gesendet wird, wenn man "Play" drückt. "" = nichts senden, "5" = wie Knopf "5", weitere Codes in ctSim/model/bots/components/RemoteControlCodes.java. *default=""*
* **LightSensors**: Schalter für Lichtsensoren. *default=true*
* **BPSSensors**: Schalter für BPS-Sensoren. *default=false*
* **GP2Y0A60**: Schalter für alternativen Distanzsensor-Typ *GP2Y0A60*. *default=false*
* **WheelLag**: Schalter für  Simulation des Nachlaufs der Räder. *default=false*
* **MapUpdateIntervall**: Update-Intervall der Map-Anzeige in ms. *default=500*
* **serialport**: Serieller Anschluss. Der Sim verbindet sich mit ihm und erwartet dort einen USB-2-Bot-Adapter. *default=COM3*
* **serialportBaudrate**: Geschwindigkeit des 'serialport' in Baud. *default=115200*
* **ipForConnectByTcp**: IP, die standardmäßig im Textfeld steht, wenn man "Verbinden / Per TCP" wählt. *default=192.168.1.30*
* **portForConnectByTcp**: Zugehoeriger Port. *default=10002*
* **mapSize**: Parameter der Map-Größe (für Export). *default=12.288f*
* **mapResolution**: Parameter der Map-Auflösung (für Export). *default=*125''
* **mapSectionSize**: Parameter der Map-Section-Größe (für Export). *default=16*
* **mapMacroblockSize**: Parameter der Map-Makroblock-Größe (für Export). *default=512*

[![License: CC BY-SA 4.0](../license.svg)](https://creativecommons.org/licenses/by-sa/4.0/)
