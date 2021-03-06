# Projekthistorie von c't-Bot und c't-Sim

**09.07.2016** **Release 23** von Bot- und Sim-Code (für Änderungen siehe Changelogs)

**26.10.2015** **Release 22** von Bot- und Sim-Code (für Änderungen siehe Changelogs)

**29.02.2012** **Release 21** von Bot- und Sim-Code (für Änderungen siehe Changelogs)

**03.02.2012** Dokumentation im Wiki stark überarbeitet und aktualisiert

**18.08.2011** **Release 20** von Bot- und Sim-Code (für Änderungen siehe Changelogs)

**12.04.2011** **Release 19** von Bot- und Sim-Code (für Änderungen siehe Changelogs)

**16.10.2010** **Release 18** von Bot- und Sim-Code (für Änderungen siehe Changelogs)

**23.05.2010** **Release 17** von Bot- und Sim-Code (für Änderungen siehe Changelogs)

**29.01.2010** Einige Code-Optimierungen (vor allem Positions- und Map-Code), Fehlerabschätzung bei der Positionsberechnung über die Option MEASURE_POSITION_ERRORS_AVAILABLE

**02.12.2009** PDF-Dokumentation des Bot-Codes verfügbar

**17.11.2009** Der Bot spielt Schach! Neues Schachverhalten behaviour_drive_chess() von Frank Menzel

**18.10.2009** Positionsspeicher können mit individueller Größe angelegt werden und sind außerdem per Bot-2-Bot-Kommunikation übertragbar

**28.09.2009** Aktualisierung der Eclipse-Anleitung auf Version 3.5

**17.09.2009** Beta-Code zur Selbstlokalisierung des ct-Bots

**06.07.2009** **Release 16** von Bot- und Sim-Code

**29.05.2009** Verbesserung des Verhaltens *bot_drive_area()*

**19.04.2009** Neues Video in der Galerie

**17.04.2009** Dokumentation zum BotOS

**13.03.2009** Per Bot-2-Bot-Kommunikation lassen sich nun auch RemoteCalls an einen anderen Bot schicken

**06.03.2009** Aktualisierung und Neugestaltung des Wikis inkl. der Aufbauanleitungen

**02.03.2009** ct-Sim kann nun live die vom Bot erstellte Map anzeigen, sowohl von simulierten als auch von echten Bots

**27.02.2009** Zwei neue Videos in der Galerie zum neuen Verhalten *bot_follow_line_enh()*

**26.02.2009** Neues Verhalten *bot_follow_line_enh()* von Frank Menzel (devel-Zweig)

**25.02.2009** Mit SVN Rev. 1554 gibt es etliche Verbesserungen der ct-Bot-Firmware

**16.01.2009** Bot-2-Bot-Kommunikation unterstützt Payload-Versand

**13.01.2009** Neues Verhalten bot_line_shortest_way() von Frank Menzel (devel-Zweig)

**21.11.2008** ct-Sim kann die Map des Bots zur Laufzeit anzeigen (devel-Zweig)

**29.10.2008** ct-Bot / ct-Sim **Release 15**

**05.04.2008** Kleinere Bugfixes im stable- und im devel-Zweig

**30.03.2008** SP03-Treiber Update (devel-Zweig)

**29.03.2008** Framework um Bot-2-Bot-Kommunikation ergänzt (devel-Zweig)

**29.02.2008** ct-Bot / ct-Sim **Release 14**

**30.11.2007** Der Treiber für das Sprachausgabemodul [SP03](http://www.robot-electronics.co.uk/htm/Sp03doc.shtml) von Harald W. Leschner befindet sich jetzt im SVN. Damit lassen sich Strings als Sprache ausgeben, zurzeit nur auf dem echten Bot. Für weitere Informationen siehe mcu/sp03.c.

**12.11.2007** Frank Menzel hat ein neues Verhalten "behaviour_transport_pillar" entwickelt: Bot fährt zwischen Start- und Zielposition hin und her und kann dabei Dosen fangen und transportieren.

**11.11.2007** Unter contrib/tools sind zwei Scripte von Harald W. Leschner vorhanden, die eine einfache Speedlog-Auswertung mit Gnuplot und LaTeX ermöglichen.

**01.11.2007** Es gibt jetzt aktualisierte Test-Binaries, die auf dem aktuellen Code basieren.

**30.08.2007** Es gibt ein neues Verhalten *bot_follow_wall_behaviour* von Frank Menzel.

**13.07.2007** Die PC-EEPROM-Emulation von Achim Pankalla ist nun im SVN integriert.

**12.07.2007** Es gibt nun auch Delay_Routinen als Bot-Verhalten.

**26.06.2007** Das Pfadplanungsverhalten von Frank Menzel ist nun komplett mit Dokumentation im SVN zu finden.

**26.06.2007** Die Leserpatches sind nun auch ins Wiki umgezogen. Man kann daher selbst neue Patches hochladen.

**04.05.2007** Zur Webseite des c't-Projektes gehört nun auch ein Trac. Der Code ist nun offiziell ins SVN umgezogen.

**30.03.2007** Es gibt eine Wiki-Seite für Diskussionen, Ideen und Anregungen rund um neue Verhalten für den c't-Bot.

**29.03.2007** Ab sofort ist der Code aus dem alten Public-CVS im Subversion.

**07.03.2007** Ab sofort ist der Code aus dem alten (internen) CVS im Subversion.

**22.02.2007** Release 13 -- c't-Bot: Wichtigste Änderung dürfte das neue User-Interface sein. Wir haben Ausgaberoutinen und Fernbedienungsabfragen in einem Verzeichnis ui/ gesammelt. Dort kann man nun recht bequem nene Screens definieren und für jede Screen selbst die RC5-Kommandos abarbeiten. Das entschlackt sowohl die Datei rc5.c als auch ct-Bot.c deutlich. Welche screens aktiv sein sollen steht in available_sreens.h. Die Definition der Screens kann entweder in eigenen Dateien in ui/ (gemacht in misc.c) erfolgen, oder aber an Ort und Stelle, wo sie inhaltlich hingehört (siehe bot-logik.c). Wichtig ist nur, das man die neue Screen auch am System anmeldet (siehe gui.c).

**22.02.2007** Release 13 -- c't-Sim:

* Erste Version eines Applet, das man auf WLAN-Bot laden kann (siehe documentation/applet-howto/applet-howto.html)
* Reale Bots können sich wieder verbinden (per USB und per TCP für WLAN-Bots)
* Wettbewerbs-System, so wie es im Oktober verwendet wurde (ctSim.view.contestConductor)
* Remote-Calls
* Diverse Verbesserungen und Umstellungen

Achtung: Die main()-Methode des ctSim ist umgezogen. Sie wohnt nicht mehr in ctSim.controller.Controller, sondern in ctSim.controller.Main. Man muss zum Starten des c't-Sim also Main laufenlassen, nicht Controller. Alte "Run"-Profile in Eclipse müssen dementsprechend angepasst werden.

----
*Diese Seite wurde am 23.2.2007 von http://www.heise.de/ct/ftp/projekte/ct-bot/news.shtml hierher kopiert und ist deshalb in HTML.*

<p><b>21.02.2007:</b> Die Java Virtual Machine (<a href="http://sourceforge.net/projects/nanovm">NanoVM</a>) unterstützt seit der <a href="http://sourceforge.net/forum/forum.php?forum_id=666094">Version 1.5</a> auch den c't-Bot. Wer will, kann seinen Robotersteuercode nun auch in Java schreiben.</p>

<p><b>18.01.2007:</b> Die bebilderte Aufbauanleitung für das Erweiterungsmodul ist <a href="http://www.heise.de/ct/ftp/projekte/ct-bot/erweiterung.shtml">online</a>. Die Auslieferung der Teilesätze beginnt in Kürze.</p>

<p><b>17.01.2007:</b> Wir haben dem Code-Archiv des c't-Bot nun einen
Bootloader hinzugefügt. Mit ihm kann man den Bot sehr schnell und bequem
flashen. Das Übertragen eines Hex-Files dauert nun nur noch 14 Sekunden.
Der Bootloader funktioniert sowohl mit RS-232-, USB-2-Bot-, LAN-, als
auch WLAN-Verbindungen. Sobald der Bootloader einmal auf dem Bot
installiert ist braucht man den Programmieradapter nicht mehr.</p>

<p><b>15.01.2007:</b> Wir haben den c't-Bot-Code im CVS deutlich
überarbeitet. Der Code unbterstützt nun auch die Fähigkeiten des Erweiterungsmoduls.
Zu den wichtigsten Neuerungen  gehören:</p>

<ul>
<li>Ein Verhalten, um einen Gegenstand einzufangen</li>
<li>Ein Verhalten, um den Klappenservo zu kontrollieren</li>
<li>Code zum Umgang mit MMC- und SD-Speicherkarten</li>
<li>Kommunikation jetzt per Default mit 57600 Baud</li>
<li>Mini-Speichermanager für den bequemen Zugriff auf Speicherkarten</li>
<li>Mini-Fat Dateisystem für Speicherkarten</li>
<li>Erweiterung der Kartographieroutinen auf dem Bot</li>
<li>Remote-calls um von außen Verhalten auf dem Bot anzusprechen</li>
</ul>
Über weitere Details gibt die Changelog-Datei im Code Aufschluß.
</p>

<p><b>08.01.2006:</b> Mit dem Erweiterungsmodul lernt der Bot nun auch die Kommunikation per LAN und WLAN. Des Weiteren lässt sich der Speicher des Bots nun über MMC- oder SD-Karten stark erweitern. Eine Klappe kann das Transportfach verriegeln. Dazu kommen noch diverse Erweiterungen des Source-Codes für den Mikrocontroller. Der überarbeitete c't-Sim kommuniziert nun einerseits wieder per USB-2-Bot mit dem Roboter, kann aber auch per LAN und WLAn die Statusinformationen von einem echten Bot empfangen. Eine Erweiterung im Kommunikationsprotokoll schafft die Grundlagen, um dem c't-Bot auch aus der Ferne komplexe Aufgaben zu übermitteln. </p>

<p><b>24.11.2006:</b> In der Linkliste finden sich nun auch Einträge auf Seiten von Lesern sowie einige Videos zum Bot. Wer seine Seite dort vermisst, schickt uns einfach eine kurze Mail  und wir nehmen sie auf.</p>

<p><b>06.11.2006:</b> Alle Verhalten aus bot-logik.c haben nun eigene
Dateien erhalten, so dass das Framework nun deutlich übersichtlicher
ist. Des Weiteren haben wir eine Referenzimplementation des
Kartographiealgorithmus mit in den c't-Bot-Code gepackt.</p>


<p><b>13.10.2006:</b> Der c't-Sim-Wettbewerb wird ab dem 18.10.06
live online ausgetragen. Detailliertere Informationen zum Spielplan
veröffentlichen wir kurz vorher. Wie alles andere wird der Spielplan
direkt über den eigenen Bereich in der zusätzlichen Navigation rechts zu
erreichen sein, die wir der inzwischen ziemlich umfangreichen Webseite
zusammen mit einigen Aktualisierungen spendiert haben.
</p>

<p><b>18.09.2006:</b> Der IRC-Kanal zum c't-Bot ist umgezogen und nun so erreichbar: Server: pizza.de.eu.blitzed.org, Port:  6667 Kanal: #ct-bot</p>

<p>
<b>08.09.2006:</b> Die Anmeldefrist für den c't-Sim-Wettbewerb ist verstrichen -- über 100 Teilnehmer und Teams haben sich angemeldet! </p>

<p>Zum Anmeldeschluss wurde die bereits vor längerer Zeit im Forum diskutierte Änderung im Wettbewerbsablauf in die Wettbewerbsbedingungen aufgenommen:</p>

<p>Das Turnier wird jetzt in zwei Phasen abgewickelt. Zunächst wird in einer Vorrunde jeder Bot einzeln durch einen Parcours geschickt und die benötigte Zeit gestoppt, was eine Rangliste ergibt. In der Hauptphase kommt dann das K.o.-System zum Einsatz, d.h. dass sich jeweils der Gewinner eines Duells für das nächste qualifiziert. Nur unter den vier besten werden alle Plätze ausgespielt.</p>

<p>Die Rangliste aus der Vorrunde ermöglicht Ausgewogenheit im Turnierplan der Hauptphase: Die Spieler werden so platziert, dass sich der schnellste und der zweitschnellste aus der Vorrunde erst im Finale begegnen können, der schnellste und der drittschnellste erst im Halbfinale usw. So werden allzu verzerrte Wettbewerbsergebnisse vermieden -- gäbe es z.B. eine einfache Auslosung statt einer Vorrunde, könnten zufällig die beiden besten Implementierungen in der ersten Runde aufeinandertreffen. Das würde heißen, dass einer der beiden Schnellsten schon im ersten Durchgang ausscheidet, was seine tatsächliche Stärke verfälscht widerspiegelt. Die Vorrunde soll das vermeiden und helfen, die Spannung bis zuletzt aufrechtzuerhalten.</p>

<p>Für die Hauptphase gilt: Treten nicht genügend Teams an, um alle Zweikämpfe des ersten Durchgangs zu füllen, erhalten möglichst viele Bots ein Freilos, das sie automatisch für die nächste Runde qualifiziert. Im Extremfall findet somit in der ersten Runde des Turniers nur ein einziges Duell statt -- dafür sind alle anschließenden Durchgänge voll besetzt.</p>

<p>Gleichzeitig mit dem Anmeldeschluss erfolgt der Feature Freeze an der Wettbewerbsversion des c't-Sim. Einige besondere Eigenheiten des Simulators sollen an dieser Stelle noch einmal hervorgehoben werden:</p>
<ul>
<li>Der Simulator schickt bei Start eines Duells an beide Bots auf den Startfeldern jeweils das Fernbedienungssignal "5". Das wird auch im Wettbewerb der Fall sein. Die Referenzimplementierung aus dem CVS aktiviert auf dieses Signal hin das Wandfolgeverhalten. Dieses kann man für den Wettbewerb durch einen eigenen Algorithmus ersetzen. Alternativ kann Ihr Bot auch Signale der Fernbedienung ignorieren und einfach sofort bei Erzeugung im Simulator das von Ihnen programmierte Verhalten starten, das den virtuellen Roboter möglichst schnell durchs Labyrinth bringen soll. </li>

<li>Das Licht von den vier Eckleuchten im Parcours hat eine Tragweite von einem Meter, was etwa 4 Rasterfeldern des Labyrinths entspricht. Der Messwert der "Lichtsensoren" steigt linear mit abnehmender Entfernung zur nächsten Lichtquelle im Sichtbereich an. Befinden sich zwei Lichtquellen im Sichtbereich, wird die weiter entfernte komplett ignoriert. Die Sensoren haben einen Öffnungswinkel von 180 Grad und "schauen" auch durch Wände hindurch.  </li>

<li>Die Implementierung des Wandfolgers im CVS vermeidet keine Löcher. Enthält ein Parcours auf dem Weg an der Wand entlang Fallgruben, erreicht unsere Referenzimplementierung ihr Ziel nicht. </li>
 </ul>
<p>
<b>01.09.2006:</b> Nur noch eine Woche bis zum Anmeldeschluss des c't-Sim-Wettbewerbs! Nähere Informationen gibt es auf der Wettbewerbsseite, seinen Steuercode für den virtuellen Bot kann man noch bis zum 6. Oktober auf den c't-Server hochladen.</p>

<p>
<b>29.08.2006:</b> Einige Bugfixes sind in den letzten Tagen zum c't-Bot-Code
hinzugekommen. Wir raten daher allen, die aktuelle Version auszuchecken.</p>

<p>
<b>15.08.2006:</b> Es steht ein neues Release von ct-Bot und ct-Sim im CVS zur Verfügung (identisch in den Versionen, die mit 'HEAD' und 'Wettbewerb' markiert sind). Zahlreiche Eigenheiten und Bugs des Sim sollten damit behoben sein, Details entnehmen Sie bitte dem Changelog. Der Sim hat deutlich an Performance gewonnn und ist jetzt wieder mit detaillierteren Anzeigen und der lange vermissten Fernbedienung ausgestattet.</p>
<p>
Die im Forum diskutierte Möglichkeit, den realistischen Kennlinien der Sensoren auf Seiten des Bot mit einem einfachen lookup-table zu begegnen, wird (leider ?) nicht mehr funktionieren -- in der neuen Version des Sim extrapolieren die Sensoren auch beliebige Zwischenwerte. Im Wettbewerb muss der Steuercode des Bot also mit zweideutigen Werten umgehen können. </p>
<p>
<b>20.07.2006:</b> Achim Pankalla, Peter Jonas und Frank Menzel haben weitere Patches für den c't-Bot beigesteuert, die unter anderem auch simulierte Bots mit einem virtuellen EEPROM zur Kalibrierung der Distanzsensoren ausstatten. </p>

<p>
<b>18.07.2006:</b> Christian Isensee hat einen Fehler im Simulator gefunden und per Patch behoben: Versehentlich wurden den Distanzsensoren des Bot - rechts wie links - der gleiche Abstandswert übergeben (der eigentlich nur für den linken Sensor gedacht war). Das CVS enthält jetzt die korrigierte Version; wir bitten um Entschuldigung für diesen Fehler.</p>

<p>In diversen Foren-Threads wurden Stimmen laut, die zu bedenken gaben, dass beim K.o.-System im Wettbewerb die Gefahr besteht, dass "gute" Bots zu früh ausscheiden und "schlechte" weit kommen können. Aus verschiedenen bereits diskutierten Gründen möchten wir weiterhin an dem gewählten Ausscheidungssystem festhalten. Wir haben aber über die Anregungen unserer Leser nachgedacht und wollen daher eine Ergänzung des Systems wie folgt vornehmen:</p>

<p>Statt der ursprünglichen geplanten zufälligen Einsortierung in den Spielbaum des K.o.-Systems muss nun jeder Bot im Vorfeld des eigentlichen Wettbewerbs ein unbekanntes Qualifikationslabyrinth durchqueren. In diesem starten die Bots einzeln und der c't-Sim stoppt die Zeit, die der Bot vom Start zum Ziel braucht. Auch hier kann es -- wie später in den Ausscheidungsrunden -- zu einem Timeout kommen, wenn der Bot zu lange braucht.</p>

<p>Anhand der Ergebnisse dieser Vorrunde erfolgt dann die Verteilung der Bots in den eigentlichen Spielbaum: Alle Bots werden so auf die Matches verteilt, dass sich der nach der Qualifikation schnellste und zweitschnellste Bot erst im Finale begegnen können. Der erste/zweite kann auf den dritten/vierten erst im Halbfinale treffen. </p>

<p>Die eventuell vorhanden Freilose gehen an die Bots mit den kürzesten Zeiten. Die restlichen Bots treten in solchen Paarungen gegeneinader an, dass zwischen zwei Konkurrenten in der ersten Runde immer gleich viele Plätze liegen. </p>

<p>Auch bei diesem Modus kann man natürlich mal Pech mit dem Labyrinth (und dem Gegner) haben, aber dieses Risiko gehört zu solchen Wettbewerben dazu und macht das Turnier spannend. Guter Robotercode zeichnet sich unter anderem dadurch aus, dass er mit vielen verschiedenen Randbedingungen klarkommt.</p>

<p>
<b>07.07.2006:</b> Die Redaktion schreibt das erste virtuelle Bot-Turnier aus! Wer dort siegen will, muss im c't-Sim einen flotten Weg durch unbekanntes Gelände finden. Einen realen Roboter braucht man nicht für die Teilnahme -- den gibt es zu gewinnen. Die Anmeldung läuft bis zum 8. September, ausgefochten wird der Wettkampf in der Woche vom 16. bis 20. Oktober. Der Roboterwettbewerb der c't findet im Rahmen des <a href="http://www.informatikjahr.de/">Informatikjahrs</a> statt.</p>

<p>Im Zuge der Wettbewerbsvorbereitung wurde der c't-Sim gründlich überarbeitet und mit einer neuen grafischen Oberfläche versehen. Seine virtuellen c't-Bot muss aber niemand komplett umprogrammieren -- an den Schnittstellen zwischen Java- und C-Teil des Simulators hat sich wenig geändert. Mit einer Ausnahme: Die Distanzsensoren der neuen Version des c't-Sim besitzen Kennlinien, die an jene der GP2D12-Sensoren beim realen Bot angenähert sind. Dadurch fallen Entfernungsmessungen möglicherweise zweideutig aus. Details hierzu beschreibt der aktuelle c't-Artikel zum c't-Bot-Projekt.</p>

<p>Derzeit ist der aktuelle Code von c't-Bot und c't-Sim identisch mit den Programmversionen, die im Wettbewerb zum Einsatz kommen. Wie man später gezielt von den Entwicklerversionen des Codes zu den Turniervarianten wechseln kann, beschreibt ein gesonderter Abschnitt der Installationsanleitung.</p>

<p>Die neuesten Versionen von c't-Bot und c't-Sim sind aktuell nur über das CVS erhältlich -- fertige ausführbare Dateien und Zip-Archive mit dem Code werden in Kürze auf dieser Seite veröffentlicht. </p>

<p>
<b>28.06.2006: </b>Achim Pankalla hat einen Patch
eingereicht, der für realitätsnahe Messwerte bei den Abstandssensoren im c't-Sim
sorgt. Ein weiterer Patch sorgt dafür, dass der simulierte Bot mit Sensordaten
dieser Art umgehen kann.
</p>
<p>
<b>14.06.2006:</b> Torsten Evers hat einen Patch beigesteuert, der im
Display auf Screen 5 (nur für Target MCU) den noch freien
Arbeitsspeicher des Mikrocontrollers anzeigt.
</p>

<p>
<b>10.06.2006:</b> Der c't-Bot-Artikel in c't 13/06 zeigt, wie man den
Maussensor auswertet und aus seinen Daten eine Position errechnet. Der
Code im CVS-Archiv kennt nun auch eine Funktion bot_gotoxy(), die eine
genaue Position anfährt. Die ausgewerteten Daten der Sensoren und die
Position stehen als globale Variablen zur Verfügung. Ein überarbeitere
Hardware-Mod beschreibt, wie man den Maussensor optimal montiert. Des Weiteren hat
der Linienfolger von Torsten Evers Einzug in den offiziellen Code gehalten.
</p>

<p><b>27.05.2006:</b> Die RX/TX-Bibliothek für serielle Kommunikation erlaubt
dem c't-Sim nun, über beliebige serielle Adapter mit dem realen Bot zu
kommunizieren.</p>


<p><b>22.05.2006:</b> Die Roadmap ist wieder aktuell.</p>


<p><b>19.05.2006: </b>Eine zusätzliche Hardware-Modifikation
beschreibt, wie man Einstreuungen auf der seriellen Schnittstelle
vermeidet. Spendiert man den Abstandssensoren zusätzlich zu den schon
vorgestellten Pufferkondensatoren noch jeweils einen großen Elko,
verbessert das die Messgenauigkeit.</p>

<p><b>13.05.2006:</b> Der c't-Sim-Artikel in c't 11/06 beschreibt, wie man eigene Welten für die Bots baut. Außerdem können sich nun auch Zuschauer über das Internet auf einem c't-Sim einloggen und den Bots beim Rennen zuschauen. Für die Verfeinerung der ClientViews brauchen wir noch tatkräftige Unterstützung. Das Netzwerkprotokoll dürfte noch etwas Tuning vertragen und auch die Anzeige ist noch spartanisch.</p>

<p>
<b>02.05.2006:</b> In der FAQ gibt es nun auch ein paar Tipps zum Einstieg in die Programmierung eigener Verhalten. Segor hat nun auch eine zum Bot passende RC5-Infrarotfernbedienung im Angebot, die wir demnaechst als Standardmodell auch im c't-Sim nachbauen werden.</p>

<p>
<b>29.04.2006:</b> Passend zum Tutorial in c't 10/06 steht nun auch der
Code zum Durchqueren eines Labyrinthes bereit. Der c't-Sim hat diverse
interne Änderungen erfahren. Details dazu stehen im Changelog und werden
ausführlich Thema in c't 11/06 sein. So ist unter anderem die
Möglichkeit neu, simulierte C-Bots per Knopfdruck zu starten, oder das
Bild des Maussensors von einem realen Bot auszulesen und darzustellen.
</p>

<p>
<b>25.04.2006:</b> Die Aufbauanleitung enthält nun auch Fotos der LEDs. In der FAQ und der Link-Liste gibt es nun ein paar Tipps zur Netikette. Im CVS befindet sich mittlerweile auch eine Batch-Datei zum Flashen mit dem STK200 uner Windows und es sind mal wieder ein paar neue Fernbedienungsdefinitionen hinzugekommen.
</p>

<p>
<b>21.04.2006:</b> Auf der Patchsseite ist eine Code-Erweiterung von Carsten Giesen zu finden, die den c't-Sim mit allen Tasten der Fernbedienung Hauppauge MediaMPV ausstattet.
</p>

<p>
<b>13.04.2006:</b> Mit der Drehzahlregelung fährt der c't-Bot nun mit
definierten Geschwindigkeit in mm/s, die er auch ziemlich genau einhält.
Damit drehen sich die Motoren ebenfalls annähernd gleich, was den
Geradeauslauf stark verbessert.
</p>

<p>
<b>11.04.2006:</b> Dank Andreas Merkle zeigt der c't-Sim die Log-Ausgaben des Roboters in einem eigenen Log-Fenster an. Es gibt nun eine Checkliste zum Einreichen eigener Patches. In der FAQ gibt es neuerdings einen Tipp zum SMD-Löten.
</p>

<p><b>10.04.2006:</b> Die Roadmap ist wieder aktuell.</p>

<p><b>05.04.2006:</b> Segor bietet nun auch die von Viktor Dirks entworfenen Rad-Encoder-Scheiben an. Wer seinen Bot bereits erhalten hat, findet <a href="http://www.segor.de/L1Bausaetze/ct-robot.shtml">hier</a> Informationen, wie er günstig an seine Scheiben kommt. Es gibt jetzt eine Aufbauanleitung für den USB-2-BOT-Adapter. Des Weiteren stehen alle bisherigen c't-Bot-Artikel nun auch online in HTML-Form zur Verfügung. Darüber hinaus haben wir eine neue Seite mit Code-Richtlinien für eigene Erweiterungen erstellt. Ein neuer Patch von Johannes Dörnte ermöglicht es, im Simulator zwischen zwei Welten umzuschalten.</p>

<p><b>04.04.2006:</b> Eine ganze Reihe von Leser-Patches hat Eingang ins CVS gefunden. Zu den deutlichsten Änderungen gehört der Patch von Carsten Giesen und Frank Menzel, der es erlaubt einzelne Verhalten per Fernbedienung zu aktivieren. Außerdem gibt es jetzt ein Unterverzeichnis Documentation. Dort liegt bereits eine erste Datei, die die Bedienung der oben genannten Features beschreibt. In Zukunft würden wir hier gerne lieber HTML- als TXT-Content sehen. Doku-Patches nehmen wir genauso gerne entgegen wie Code-Patches.</p>

<p><b>21.03.2006:</b>  Dank eines Patches von Markus Lang zeigt der c't-Sim das Kontrollfenster und den Blick auf die Roboterwelt jetzt in einem gemeinsamen Rahmen (SplitPane) an. </p>
<p><b>20.03.2006:</b> Dank Michail Brzitwa kann man nun mehrere Fernbedienungen erfassen und muss nur noch per #define-Schalter eine auswählen.  Patches mit weiteren RC5-Codes nehmen wir gerne entgegen. Der reale c't-Bot nimmt nun auch FB-Kommandos vom c't-Sim an. Der simulierte Bot erkennt nun auch den Maussensor. </p>

<p><b>17.03.2006:</b> c't-Bots trainieren jetzt Slalomfahrten – ganz ohne vorher wissen zu müssen, wie der Kurs sich drehen und wenden wird. Am Beispiel dieser Aufgabe zeigt der neue Code für den c't-Bot, wie sich komplexes Verhalten in Anlehnung an das Subsumptionsprinzip des Robotik-Pioniers Rodney Brooks realisieren lässt. </p>

<p>An vielen Stellen und Parametern im Programm lässt sich noch herumschrauben, um das Verhalten des Bot weiter auszufeilen und an den eigenen echten c't-Bot anzupassen. In den Kommentaren zum Code finden sich an einigen Stellen Hinweise darauf, wo es sich anbietet, Verhaltensweisen zu erweitern und zu verbessern. Der Sourcecode steht wie immer im CVS und als ZIP-Verzeichnis zum Download bereit.</p>

<p>Auch der Simulator hat sich in wichtigen Details verändert, er liegt jetzt in Version 0.3 vor und ist für den Einsatz der neuen USB-2-Bot-Verbindung (Artikel dazu in c't 7/06) gerüstet. Weitere Änderungen betreffen die innere Struktur des Simulators und die Modellierung des Bot: So arbeiteten die vorigen Versionen des c't-Sim versehentlich mit der Zahl von 40 Encoder-Markierungen pro Radumdrehung (in der Realität sind es dagegen 30 helle und 30 dunkle Felder, also 60 Wechsel pro Rotation).</p>

<p>WICHTIG: Der c't-Sim verwendet jetzt Konstrukte aus dem Java-Standard 5.0 &mdash; Voraussetzung für den Betrieb ist also ein JDK, das diese Java-Version unterstützt. In der Entwicklungsumgebung Eclipse muss zusätzlich im Menü Window/Preferences unter Java/Compiler/JDK Compliance die Standardeinstellung 1.4 auf 5.0 umgeschaltet werden.

<p>Für den c't-Sim gibt es jetzt im CVS eine öffentlich zugängliche ToDo-Liste.</p>

<p><b>10.03.2006:</b> Für alle, die am Code von c't-Bot oder c't-Sim entwickeln, haben wir eine Mailingliste eröffnet. An diese kann man sich auch mit Patches oder Problemen wenden. Unter anderem soll diese Liste dazu dienen Patches mit einer breiteren Gruppe von Entwicklern zu diskutieren, bevor sie ins CVS einfließen.</p>


<p><b>09.03.2006:</b> Andreas Merkle hat den c't-Bot um diverse Logging-Funktionen erweitert. Auch das Ausgeben von Strings auf das Display ist nun deutlich komfortabler.</p>

<p><b>08.03.2006:</b> Wir haben eine eigene Seite für Hardware-Modifikationen eingerichtet. Dort sammeln wir sinnvolle Veränderungen am c't-Bot sowie Problemlösungen für widerspenstige Bots.</p>

<p><b>04.03.2006:</b> Im Forum und in c't 06/06 haben wir es bereits angekündigt: Auf der CeBIT werden wir dem c't-Bot eine kleine Veranstaltung widmen. Am Samstag den 11.03.2006 sind ab 17:00 Uhr alle Interessierten herzlich eingeladen, sich mit uns am Heise-Stand zu treffen. Nach einer kleinen Einführung in das Projekt wollen wir über Programmierung, Hardware-Erweiterungen, Einsatzmöglichkeiten virtueller und realer Roboter und vieles mehr mit Ihnen diskutieren.</p>

<p><b>03.03.2006:</b> Wie in c't 06/06 beschrieben geht der c't-Bot nun sparsamer mit den Akkus um und linearisiert die Werte der Abstandssensoren.</p>

<p><b>01.03.2006:</b> Der c't-Bot kann nun alle Sensor- und Aktuatordaten per serieller Schnittstelle an den PC übertragen. Eine geeignete Adapterschaltung (TTL-UART auf USB) werden wir voraussichtlich in einer der kommenden Ausgaben von c't vorzustellen. Die FAQ haben wir ebenfalls erweitert.</p>

<p><b>28.02.2006:</b> Um Messungen von Geschwindigkeiten zu ermöglichen, haben wir globale Zeitvariablen in den Code hinzugefügt. Wir nutzen sie zum Beispiel für die neue Geschwindigkeitsmessung. Für den c't-Bot-Code gibt es im CVS nun eine öffentlich zugängliche Todo-Liste. Alle Anpassungen des Roboters bzw. seines Verhaltenssystems sind nun in bot-local.h gesammelt. Dort kann man sie per .cvsignore besser schützen.</p>

<p><b>27.02.2006:</b> Im CVS steht eine aktualisierte Firmware: Dank Markus Hennecke kann man jetzt dem c't-Bot auf der Kommandozeile die IP-Adresse des Simualtors übergeben. Torsten Evers hat sich des Linux-Flash-Skriptes angenommen. Es prüft nun die Fuse-Bits vor dem Schreiben. Ausserdem gibt es eine neue Variable, die die letzte Drehrichtung des Motors speichert sowie ein wenig mehr Infos in sensor.h.</p>

<p><b>23.02.2006:</b> Es gibt ein neues Testprogramm im Download-Bereich und eine Erklärung dazu bei den FAQs. Wir haben die Motordrehrichtung im Code jetzt so verändert, dass sie zur Aufbauanleitung passt. Des Weiteren haben wir den Endian-Patch von Ansgar Esztermann und Markus Hennecke aufgenommen. c't-Bots auf Macs, Sparcs, usw. sollten nun auch mit dem c't-Sim kommunizieren können. Ein weiterer Leser-Patch findet sich auf der Patch-Seite. Ein neuer Schaltplan der Maussensorplatine zeigt nun den selben Wert für R5 wie die Stückliste.</p>

<p><b>22.02.2006:</b> Carsten Giesen hat einen Patch beigesteuert, mit dem sich der Inhalt des Reset-Registers auf dem Display anzeigen lässt. Im Schaltplan war der Widerstandswert von R6 fälschlicherweise mit 4k7 angegeben. 47k, wie es in der Stückliste steht, ist hingegen korrekt. Ein neuer Plan steht online.</p>

<p><b>21.02.2006</b>: Wir haben eine eigene Seite für Code-Patches von Lesern zum Download eingerichtet. Dort findet sich bereits eine Programmverfeinerung von Fabian Recktenwald für den c't-Sim. Damit bestimmt der Simulator die neue Position eines Bot geometrisch korrekt und nicht &mdash; wie bisher &mdash; lediglich in ausreichender Näherung.

<p>Im CVS finden sich dank Torsten Evers jetzt auch Linux-Skripte zum Flashen und Fusen des Mikrocontrollers.</p>

<p><b>20.02.2006</b>: Weitere Leser-Patches haben Eingang ins CVS gefunden. Rolf Ebert steuerte die Möglichkeit bei, das Display des Bot auch remote im Simulator anzuzeigen. Andreas Staudenmayer hat Windows-Batch-Files zum Flashen und Setzen der Fuse-Bits eingeschickt. Von Carsten Giesen stammt die Erweiterung, zwischen mehreren Display-Screens umschalten zu können. Dank Fabian Recktenwald konnten wir einen kleinen Bug im Goto-System beheben.</p>

<p>Viele L&ouml;sungen f&uuml;r Probleme, die in den letzten Tagen
im Forum auftauchten, haben wir in der FAQ gesammelt.</p>


<p><b>18.02.2006:</b> Version 0.2 des c't-Sim ist da! Der Simulator kommt jetzt mit mehreren virtuellen Robotern gleichzeitig klar, die sich gegenseitig als Hindernisse erkennen. Die simulierten Bots verfügen über neue Sensoren, um Licht, Linien, Abgründe oder die eigene Bewegung über Grund zu erkennen. Damit diese Sinne auch alle gefordert werden, enthält die Welt mehr Hindernisse als bisher, hinzugekommen sind Lampen, Linien und ein Loch im Boden. Der Code für den c't-Sim und den c't-Bot ist wie immer im CVS erhältlich, vorkompilierte ausführbare Dateien gibt es auch auch als ZIP-Datei. </p>
<p>WICHTIG: Kommende Versionen des c't-Sim werden Methoden benutzen, die erst ab Java 5.0 verfügbar sind. Meldet Eclipse für den neuen Code Kompilierfehler, ist zu prüfen, ob die Java-Version innerhalb der Entwicklungsumgebung richtig gesetzt ist. Für Eclipse muss im Menü Window/Preferences bei Java/Compiler/JDK Compliance die Version 5.0 eingeschaltet sein. </p>

<p><b>13.02.2006:</b> Eine korrigierte Aufbauanleitung, aktuelle Schalt- und Bestückungspläne sowie eine überarbeitete Stückliste stehen online.</p>

<p><b>04.02.2006</b>: Ab dem 6.2.06 ist der c't-Bot verfügbar. eMedia bietet einen Platinensatz an, Segor Electronics des Weiteren alle Bauteile sowie die Mechanik. Der c't-Bot-Code ist nun um die Mikrocontroller-Routinen erweitert. Die neueste Version findet sich wie immer im CVS, einige vorkompilierte Testprogramme gibt es auch auch als ZIP-Datei.</p>

<p><b>26.01.2006</b>: Der erste von einem Leser eingesandte Patch für den c't-Sim
hat Eingang in die offizielle Codebasis gefunden. Der Simulator kann
jetzt mittels eines neuen Schiebereglers auch in Zeitlupe betrieben
werden. Die neueste Version des Codes ist im CVS erhältlich, die
Änderungen beschreibt ein Eintrag in der Datei Changelog.txt.
</p>

[![License: CC BY-SA 4.0](../license.svg)](https://creativecommons.org/licenses/by-sa/4.0/)
