# Nützliche Tools für AVR

*Hier soll eine kleine Übersicht enstehen, welche Tools und Tricks die Entwicklung von Software für AVR-µController erleichtern.*

* Mit `avr-nm --size-sort --print-size ct-Bot.elf` erhält man eine sortierte Liste, welche Funktionen und Variablen wieviel Platz in Flash und RAM belegen. In der ersten Spalte steht die Adresse, in der Zweiten die Größe (hexadezimal). T: Globale Funktionen, t: Statische Funktionen und D oder d: Globale bzw. statische Daten. All diese liegen im Flash. B und b hingegen belegen nur Platz im RAM, da sie beim Start mit 0 initialisiert werden.

* Float-Ausgaben mit printf für Display und Log: In den Linkereinstellungen (Miscellaneous) folgende Linker-Flags ergänzen: `-Wl,-u,vfprintf -lprintf_flt`

* Vom Compiler generierten Assembler-Code als .s-Dateien erhalten: In den Compilereinstellungen (Miscellaneous) folgendes unter Other Flags ergänzen: `-save-temps -fverbose-asm -dA`

* Assembler-Listing des gesamten Codes generieren: `avr-objdump -h -S ct-Bot.elf > ct-Bot.s`

* Erzeugtes Binary zu Debug-Zwecken disassemblieren: `avr-objdump -d -m avr5 ct-Bot.elf`

* Automatisch alle nicht verwendeten Funktionen aus dem Binary entfernen:
  1. Dem Compiler mit `-ffunction-sections -fdata-sections` sagen, dass er jede Funktion in eine eigene Section stecken soll.
  1. Den Linker mit `-Wl,--gc-sections` anweisen, nicht verwendete Sections zu verwerfen (*gc* für *Garbage Collection*) - damit werden dann allerdings auch alle zurzeit nicht benutzten EEPROM-Daten entfernt.

[![License: CC BY-SA 4.0](../license.svg)](https://creativecommons.org/licenses/by-sa/4.0/)
