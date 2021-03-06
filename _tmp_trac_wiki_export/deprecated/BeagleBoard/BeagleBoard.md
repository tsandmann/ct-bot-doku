# CPU-Erweiterung für den c't-Bot mit einem BeagleBoard

>> **Trac-2-Markdown Konvertierung:** *unchecked*

> **Trac-2-Markdown Konvertierung:** *deprecated*

## Einleitung

Die Standard-Hardware des c't-Bots lässt sich mit dem umfangreichen Software-Framework inzwischen nahezu vollständig ausreizen. Um weitere Features implementieren zu können, entstand deshalb die Idee, den c't-Bot um eine CPU-Erweiterung zu ergänzen, die auch im c't Sonderheft [c't Hardware Hacks](https://shop.heise.de/katalog/c-t-bot-roboter-selbst-bauen) vorgestellt wird. Eine Raspberry Pi Version ist [hier](../RaspberryPi/RaspberryPi.md) zu finden.

Diese Wiki-Seite soll einen Einblick in die neuen Möglichkeiten bieten und grundlegende Informationen liefern, wie sich ein c't-Bot mit einem BeagleBoard erweitern lässt und welche Anpassungen an Hard- und Software dafür nötig sind.
Sie enthält im Allgemeinen keine einsteigerfreundlichen und vollständigen Schritt-für-Schritt-Anleitungen, da die CPU-Erweiterung den Bastel-Aspekt des c't-Bot Projekts in den Vordergrund stellt und aktuell keine abgeschlossene Baugruppe oder fertige Software umfasst. Vielmehr geht es darum, Ideen und Lösungsansätze zusammenzutragen, um die Erweiterung auf diese Weise auszubauen und zu optimieren. *Die Seite befindet sich daher erst im Aufbau und ist dementsprechend teilweise noch etwas unvollständig -- sie wird aber stetig ergänzt.*

*Herzlich willkommen sind neben Feedback und Fragen auch jederzeit *'eigene Ideen** und weitere **Vorschläge** oder **Verbesserungen***: ***.

## Idee

* Zusätzlich zur c't-Bot Hauptplatine ein BeagleBoard (BB) montieren (anstelle der offiziellen WLAN- / MMC-Erweiterung).
* ATmega des Bots führt low-level Code aus und kommuniziert mit BB über eine serielle Schnittstelle (UART, SPI).
* BB führt eine Linux-Distribution (z.B. Ubuntu für ARM) aus, ermöglicht SSH Login und Filetransfer auf dem / zum Bot, NFS mounts, Ausführung fast jeder Linux-Software.
* BB bietet weitere Schnittstellen (USB, LAN, I2C, Audio, GPIOs) für zusätzliche Hardware (WLAN USB-Stick, weitere Sensoren, Kamera).
* Funktionalität des WiPorts wird mit einem einfachen WLAN USB-Stick (einzige Voraussetzung: Treiber in Linux ab 2.6.39 enthalten) und socat bereitgestellt.
* BB GPIO-Pins steuern Reset-Leitung des ATmegas und fragen Fehler- / Batteriestatus ab.
* Audio-Ports des BBs ermöglichen Sound- und Sprachausgabe (Verstärkerschaltung nötig) sowie Mikrofon (Verstärkerschaltung nötig) oder Line-In Anschluss.
* Erweiterung des c't-Bots um eine Kamera, die über dem Kamera-Port des BB xM oder per USB angeschlossen wird.

## Voraussetzungen

* Bastelinteresse in Hard- und Software.
* c't-Bot (oder kompatiblen Roboter / ähnliche Hardware).
* Linux-PC / Linux-VM (getestet: Ubuntu ab 12.04, auf anderen Distributionen vermutlich auch möglich), um Kernel und Bootloader für ARMv7-A (hardfloat) zu übersetzen.
* Cross-Toolchain nach ARM-Linux: verfügbar für Linux (getestet unter Ubuntu ab 12.04, auf anderen Distributionen vermutlich auch möglich) und Windows (mit Linaro Toolchain). Damit lässt sich der Bot-Code für ARM-Linux übersetzen.
* c't-Bot [Toolchain](../Installationsanleitung/Installationsanleitung.md) installiert.

## Hardware

Die CPU-Erweiterung für den c't-Bot umfasst nicht nur das CPU-Board, sondern auch ein notwendiges Interface-Board, das die Verbindung zur c't-Bot Hardware herstellt und insbesondere eine Konvertierung der Signal-Spannungspegel von 1.8 V (BeagleBoard) auf 5.0 V TTL (c't-Bot Hardware) vornimmt.

### BeagleBoard

Um den c't-Bot mit einer leistungsfähigeren CPU auszustatten, fiel die Wahl auf das BeagleBoard von Texas Instruments. Diese Seite umfasst keine detaillierte Anleitung zur Einrichtung eines BeagleBoards, sondern verweist im Folgenden auf die entsprechenden (meinst englischsprachigen) Informationen der BeagleBoard-Community.

#### Technische Daten / Versionen

Es gibt derzeit (Stand Januar 2012) drei Versionen des BeagleBoards:

1. Das originale (ursprüngliche) **!BeagleBoard Rev. C4**, im Folgenden als *!BeagleBoard* bezeichnet:
    * Super-scalar ARM Cortex-A8 ([OMAP3530](http://www.ti.com/lit/ds/symlink/omap3530.pdf)), CPU 720 MHz, DSP 520 MHz
    * 256 MB LPDDR RAM
    * 256 MB NAND Flash
    * High-speed USB 2.0 OTG port
    * High-speed (only) USB 2.0 host port
    * Stereo audio out / in
    * DVI-D video out
    * S-video out
    * SD/MMC slot
    * Preis: 125 $ (zzgl. EUSt, US-Import) / 119 € (in Deutschland)

1. Das **!BeagleBoard-xM Rev. C**, welches sich im Wesentlichen durch einen schnelleren Prozessor, mehr Speicher und einen integrierten USB-Hub auszeichnet (wo der Unterschied von Bedeutung ist, ist es als *!BeagleBoard-xM* bezeichnet):
    * Super-scalar ARM Cortex-A8 ([DM3730](http://www.ti.com/lit/ds/symlink/dm3730.pdf)), CPU 1 GHz, DSP 800 MHz
    * 512 MB LPDDR RAM
    * High-speed USB 2.0 OTG port
    * On-board four-port high-speed USB 2.0 hub with 10/100 MBit Ethernet
    * Stereo audio out / in
    * DVI-D video out
    * S-video out
    * microSD slot and 4 GB microSD card
    * Camera port
    * Overvoltage Protection
    * Preis: 149 $ (zzgl. EUSt, US-Import) / 143 € (in Deutschland)

1. Das **!BeagleBone**-Board ist das neueste Board aus der BeagleBoard-Familie. Gegenüber dem BeagleBoard C4 besitzt es keinen Video- und Soundausgang, verwendet DDR2 statt LPDDR Speicher (vermutlich höherer Energieverbrauch), besitzt keinen DSP und eine andere Pin-Belegung der Erweiterungsschnittstelle (das [Interface-Board](/BeagleBoard.md#Interface-Board) passt also derzeit nicht).
    * Super-scalar ARM Cortex-A8 ([AM3358](http://www.ti.com/lit/ds/symlink/am3358.pdf)), CPU 720 MHz
    * 256 MB DDR2 RAM
    * Board size: 3.4" (86,4 mm) x 2.1" (53,3 mm)
    * USB 2.0 flexible device port with ability to supply power
    * On-board USB-to-serial/JTAG over shared USB device port
    * 1-port USB 2.0 host
    * On-chip 10/100 Mbit Ethernet
    * microSD slot and 2 GB microSD card
    * 3.3 V I/Os
    * Preis: 89 $ (zzgl. EUSt, US-Import) / 78 € (in Deutschland)

[Vergleichsübersicht](http://beagleboard.org/static/flyer_latest.pdf) von *!BeagleBoard-xM* und *!BeagleBone*.

#### Reference Manuals

* [BeagleBoard Reference Manual](http://beagleboard.org/static/BBSRM_latest.pdf)
* [BeagleBoard-xM Reference Manual](http://beagleboard.org/static/BBxMSRM_latest.pdf)
* [BeagleBone Reference Manual](http://beagleboard.org/static/BONESRM_latest.pdf)

#### Links

* [Offizielle Webseite](http://beagleboard.org) zum BeagleBoard
* [BeagleBoard im eLinux-Wiki](http://elinux.org/BeagleBoard)
* [Einsteiger-Infos](http://elinux.org/BeagleBoardBeginners) im eLinux-Wiki
* [Ubuntu Betriebssystem](http://elinux.org/BeagleBoardUbuntu) für das BeagleBoard
* [Ubuntu-Installation](https://wiki.ubuntu.com/ARM/Server/Install#Installing_pre-installed_OMAP3.2BAC8-4_Precise_.2812.04.29_Server_Images)
* [SPI-Interface](http://elinux.org/BeagleBoard/SPI) des BeagleBoards
* [Mailingliste](http://groups.google.com/group/beagleboard/topics) zum BeagleBoard
* [BeagleBoard FAQ 1](http://beagleboard.org/support/faq) und [BeagleBoard FAQ 2](http://elinux.org/BeagleBoardFAQ)
* [Deutschsprachiges Wiki und Forum](http://www.beagleboard.de) zum BeagleBoard
* [Cortex-A Series Programmer's Guide](https://silver.arm.com/browse/BX100-DA-98001-r0p0-01rel1) (Registrierung bei arm.com erforderlich)
* [Interessantes Paper](http://caxapa.ru/thumbs/229665/armcortexa8vsintelatomarchitecturalandbe.pdf), welches ein älteres (600 MHz) BeagleBoard mit einem System auf Basis von Intels Atom CPU (N330) vergleicht und ein paar Hintergründe zur ARM Cortex-A8 Architektur bietet.

### WLAN

Für die kabellose Verbindung zur Außenwelt wird ein handelsüblicher WLAN-USB-Stick verwendet. Wichtig ist nur, dass Linux einen Treiber für den verwendeten Chipsatz mitbringt.

#### Getestete Geräte

* D-Link Wireless N Nano USB Adapter (DWA-131):
  * \+ kompakte Abmessungen (18 mm x 34 mm x 7 mm inkl. USB-Stecker)
  * \+ [relativ günstig erhältlich](http://www.heise.de/preisvergleich/467738)
  * \+ Chipsatz von Linux unterstützt
  * \- eher geringe WLAN-Reichweite auf Grund der (Antennen-) Abmessungen

* D-Link Wireless N 150 Pico USB Adapter (DWA-121)
  * \+ sehr kompakte Abmessungen (14 mm x 20 mm x 6 mm inkl. USB-Stecker)
  * \+ ragt eingesteckt nicht über den Bot-Durchmesser heraus
  * \+ [relativ günstig erhältlich](http://www.heise.de/preisvergleich/634322)
  * \+ Chipsatz von Linux gut unterstützt
  * \- eher geringe WLAN-Reichweite auf Grund der (Antennen-) Abmessungen

* FRITZWLAN USB Stick N
  * \+ Unterstützt das 5 GHz Band
  * \+ Sehr gute Übertragungsleistung (ca. 5 MB/s auf BeagleBoard xM)
  * \+ [halbwegs günstig erhältlich](http://www.heise.de/preisvergleich/288068)
  * \+ Chipsatz von Linux sehr gut unterstützt
  * \- recht große Gehäuse-Abmessungen (73 mm x 24,5 mm x 10,5 mm)
  * \- Anschluss über USB-Verlängerung sinnvoll

* Hinweise über weitere kompatible Geräte willkommen
* Konfiguration
  * Siehe [Installationsanleitung für Ubuntu 12.04 LTS](../BeagleBoardUbuntu1204Install/BeagleBoardUbuntu1204Install.md#WLANkonfigurieren) für die WLAN Konfiguration unter Ubuntu -- sollte für andere Distributionen analog funktionieren.

### Interface-Board

Das Interface-Board bindet das BeagleBoard (BeagleBoard und BeagleBoard-xM) an die Hardware des c't-Bots an. Derzeit umfasst es die folgenden Subsysteme:

* Pegelanpassung 1.8 V <-> 5.0 V (TTL) der **UART2-Signale**
* Pegelanpassung 1.8 V <-> 5.0 V (TTL) für **GPIO-Pins** (Reset-Leitung zum ATmega, Fehler-Signal)
* Pegelanpassung 1.8 V <-> 5.0 V (TTL) der **McSPI4.0-Signale**
* **ATmega1284P**, der den originalen Prozessor des c't-Bots (IC1 @ Schaltplan c't-Bot) ersetzt. Unterschiede:
  * 20 MHz Quarz / Takt
  * Hardware-SPI Patch
  * Anschluss für [BPS-Sensor](/Localization.md)
  * Pullup für Servo
  * Pulldowns für Motoren (geplant)
* SPI-Anschluss für ISP oder externen MMC / SD-Slot
* Stereo **Audio-Out Verstärker** für zwei Lautsprecher (8 Ohm, 0.5 W)

Hinweise zur derzeitigen Interface-Board Version:

* Die Anbindung des Maus-Sensors des Bots fehlt. Der Maussensor kann aktuell also nicht verwendet werden, wenn das Interface-Board installiert ist. Der Hintergrund ist, dass der Maus-Sensor den SPI-Port des ATMegas belegen würde. Alternativ ist die Anbindung des Maussensors an einen SPI-Port des BeagleBoards denkbar, erfordert aber zusätzliche Hardware.
* Es werden  zwei SMD-Bauteile verwendet: *IC3* im SOIC-14 Package und *IC2* im TSSOP-16 Package. Auf die Bestückung des per Hand schwieriger zu lötenden *IC2* kann auch verzichtet werden, dann steht allerdings keine Verbindung per SPI-Bus zwischen BeagleBoard und ATmega1284P zur Verfügung.
* Das Interface-Board lässt sich auch noch auf einer Lochraster-Platine aufbauen, auf der es dann allerdings recht gedrängt zugeht, damit sie die Abmessungen des BeagleBoards nicht stark überschreitet und auf den Bot passt.

#### Komponenten

|**Bezeichner**|**Typ**|
|---|---|
|C1|100nF|
|C2|100nF|
|C3|100nF|
|C4|100nF|
|C5|100nF|
|C6|100nF|
|C7|22pF|
|C8|22pF|
|C9|100nF|
|C10|22µF|
|C12|100µF|
|C13|100µF|
|C14|100nF|
|C15|100nF|
|C16|220µF|
|C17|220µF|
|C18|100nF|
|C19|100nF|
|D1|1N4148|
|D2|1N4148|
|D3|1N4148|
|D4|1N4148|
|IC1|[ATmega1284P](http://www.atmel.com/dyn/products/product_card.asp?part_id=4331)|
|IC2|[SN74AVC4T774](http://www.ti.com/product/sn74avc4t774)|
|IC3|[TXB0104](http://www.ti.com/product/txb0104)|
|IC4|TDA2822M|
|J4|1x8|
|J5|1x8|
|J6|1x8|
|J7|1x8|
|J8|1x8|
|JP1|BB-Extension|
|JP2|Audio-In 1x3|
|JP3|Speaker 1x4|
|JP4|ISP 2x5|
|JP5|LDDR/BPS 1x3|
|L1|100µH|
|L2|100µH|
|Q1|BS170|
|Q2|20MHz|
|R1|10k|
|R2|10k|
|R3|10k|
|R4|4R7|
|R5|4R7|
|R6|10k|
|R7|220|
|R8|220|
|R9|2k7|
|R10|4k7|
|R11|2k7|
|R12|4k7|
|R13|10k|

#### Schaltplan

![Image: 'Schaltplan-0.1.png'](Schaltplan-0.1.png)

### Montage

* ATmega auf der Bot-Hauptplatine entfernen.
* Interface-Board anstelle des [originalen Erweiterungsmoduls](../ct-Bot-Erweiterung/ct-Bot-Erweiterung.md) montieren, längere Abstandsbolzen verwenden, Kabel auf J4 bis J8 verbinden.
* BeagleBoard auf Interface-Board montieren, elektrische Verbindung über BB-Extension-Header.
* Display mit zusätzlichen Abstandsbolzen oberhalb des BeagleBoards anbringen.

## Toolchain

*Die Anleitung für die Toolchain wurde für Ubuntu 12.04 LTS / hardfloat-ABI aktualisiert. Die alte Version für Ubuntu 11.04 ist [wiki:BeagleBoard?version=23 hier] zu finden.*

Um den c't-Bot Code (oder andere Programme) für die ARM-Architektur des BeagleBoards zu übersetzen, wird ein Cross-Compiler benötigt. Alternativ lässt sich der entsprechende Code auch direkt auf dem BeagleBoard übersetzen, wenn dort gcc & Co. installiert sind, was allerdings deutlich umständlicher und langsamer ist.

### Cross-Compiler Linux

Unter Ubuntu (getestet 12.04 LTS oder neuer) wird die nötige Cross-Toolchain für die BeagleBoard-Plattform *arm-linux-gnueabihf* durch das Paket **g++-arm-linux-gnueabihf** installiert: `sudo apt-get install g++-arm-linux-gnueabihf`.

Andere Linux-Distributionen bieten vermutlich ähnliche Pakete an, *Hinweise willkommen*. Wichtig ist dabei, dass die *hardflot*-ABI verwendet wird.

Zum Compilieren des Bot-Codes mit dem Cross-Compiler der Distribution als Target *Debug-ARM-Linux-BB* auswählen und in den Projekteinstellungen für Compiler, Assembler und Linker `arm-linux-gnueabihf-gcc` eintragen.

### Cross-Compiler Windows

Die [Linaro Toolchain](https://launchpad.net/linaro-toolchain-binaries) stellt einen kostenlosen Cross-Compiler für ARM-Linux (hardfloat) unter Windows zur Verfügung.

Als Installer [hier](https://launchpad.net/linaro-toolchain-binaries/+download) herunterladbar. Getestete Version: *2012.05-20120523_win32.exe*.

Zum Compilieren des Bot-Codes unter Windows als Target *Debug-ARM-Linux-BB* auswählen und in den Projekteinstellungen für Compiler, Assembler und Linker `"arm-linux-gnueabihf-gcc"` eintragen (Installationspfad sollte im PATH eingetragen sein, ansonsten absoluten Pfad angeben).

### Libraries

Derzeit werden die folgenden Libraries sowohl auf der Linux-Distribution des BeagleBoards, als auch auf dem Rechner zum Cross-Compilieren benötigt:

#### Flite, Alsa

Die Sprachsynthese-Bibliothek *Flite* kann optional zur Sprachausgabe benutzt werden. Dazu muss auf dem BeagleBoard die Bibliothek *Alsa* installiert sein (`sudo apt-get install alsa-base libasound2-dev`).

* Cross-Compilieren aus dem Quelltext. Zuvor die Datei */usr/lib/arm-linux-gnueabihf/libasound.so.2.0.0* und das Verzeichnis */usr/include/alsa* der *Alsa-Bibliothek* vom BeagleBoard nach */usr/local/arm-linux/arm-cortex_a8-linux-gnueabi/sysroot/usr/lib* bzw. *usr/local/arm-linux/arm-cortex_a8-linux-gnueabi/sysroot/include/* kopieren. Außerdem nach `cd /usr/local/arm-linux/arm-cortex_a8-linux-gnueabi/sysroot/usr/lib` mit `sudo ln -s libasound.so.2.0.0 libasound.so` einen Link anlegen. Unter Linux oder Mac OS X sind dann diese Schritte nötig:
* Archiv herunterladen und entpacken (*!ToDo: Link*)
* `./configure --with-audio=alsa --host=arm-linux --enable-shared`
* `make`
* Dateien aus *lib* nach *ct-Bot/contrib/flite/lib/arm-linux-gnueabihf/* kopieren

## Software

Auf dem BeagleBoard wird eine Linux-Distribution ausgeführt, die auf der SD-Karte installiert ist.

### Ubuntu (ARM)

Getestete und empfohlene Version: **12.04 LTS** (*Precise Pangolin*).

#### Installation

Siehe [Installation von Ubuntu 12.04 LTS für ARM auf einem BeagleBoard](/BeagleBoardUbuntu1204Install.md).

#### Anpassungen

Damit das BeagleBoard mit dem c't-Bot kommunizieren kann, sind ein paar Anpassungen am Linux-Kernel erforderlich. Sie sorgen dafür, dass die nötigen Schnittstellen auf die Erweiterungspins geroutet werden und konfigurieren die Schnittstellen entsprechend.

Zur Installation siehe [Ubuntu 12.04 Installationsanleitung](/BeagleBoardUbuntu1204Install.md#AngepasstenKernelinstallieren).

#### Konfiguration

Siehe [Ubuntu 12.04 Installationsanleitung](../BeagleBoardUbuntu1204Install/BeagleBoardUbuntu1204Install.md#WLANkonfigurieren).

### Low-level Bot-Code für ATmega

Der ATmega1284P auf dem Interface-Board führt den Low-level Code aus und stellt somit die Schnittstelle zur c't-Bot-Hardware bereit. Der Vorteil durch die Verwendung eines zusätzlichen Mikrocontrollers neben dem BeagleBoard besteht in der Kompatibilität zum originalen c't-Bot und der Wiederverwendbarkeit sämtlicher Treiber für die Bot-Hardware. Der Low-level Code besteht aus einer leicht modifizierten Untermenge des originalen c't-Bot Codes. Verzichtet wird dabei auf sämtlichen Verhaltenscode und weitere Features wie Kartographie oder Positionsspeicher, all das übernimmt der High-level Code, den die BeagleBoard-CPU ausführt. Zusätzlich implementiert ist eine CRC16-Prüfsumme, welche die Kommunikation mit dem BeagleBoard gegen Übertragungsfehler absichert. Um den Low-level Code zu aktivieren, muss in *ct-Bot.h* der Schalter `BOT_2_RPI_AVAILABLE` aktiviert werden.

### High-level Bot-Code für ARM

Das BeagleBoard führt den für die ARM-Linux Architektur compilierten High-level Code aus und übernimmt somit die Ausführung von Verhalten, Kartographie usw.

Um den c't-Bot Code für einen Bot mit BeagleBoard-Erweiterung zu übersetzen als Build-Konfiguration in Eclipse *Debug-ARM-Linux* wählen und den Schalter `ARM_LINUX_BOARD` in *ct-Bot.h* aktivieren.

Die wesentlichen Unterschiede zum originalen ct-Bot-Code:

* CRC16-Prüfsumme für Kommunikation
* Anpassungen für Kommunikation (Synchronisation, Anmeldung als realer Bot)
* Anpassungen für Auswertung der Distanzsensoren (Umrechnung gemäß Kennlinie erfolgt bereits im Low-level Code)
* Anpassungen für Motorsteuerung (1:1 Weitergabe der Geschwindigkeit)
* Anpassungen für RC5-Code (Toggle-Bit Auswertung)

### BeagleBoard-Emulation

Ein BeagleBoard lässt sich (inkl. Linux Installation) mit Qemu emulieren. Eine Installationsanleitung für Ubuntu Hosts ist [hier](http://www.cnx-software.com/2011/09/26/beagleboard-emulator-in-ubuntu-with-qemu) zu finden.

[![License: CC BY-SA 4.0](../license.svg)](https://creativecommons.org/licenses/by-sa/4.0/)
