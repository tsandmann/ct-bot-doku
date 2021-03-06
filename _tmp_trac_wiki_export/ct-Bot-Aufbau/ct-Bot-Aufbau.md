# Aufbauanleitung für den c't-Bot

>> **Trac-2-Markdown Konvertierung:** *unchecked*

Da zwischenzeitlich einige veschiedene Aufbauanleitungen im Umlauf waren, haben wir hier nun nochmals die offizielle zusammengestellt. Sie entspricht der in c't 04/06. Lediglich zwei kleine Tipps sowie eine ganze Reihe von Fotos sind dazugekommen. Alle anderen Anleitungen bitte nicht mehr beachten. Auf der Hardware-Seite finden sich alle aktuellen Schalt- und Bestückungspläne, sowie ein überarbeitete Stückliste. *Stand: 13.02.2006*

## Schritt für Schritt

Der Reflexkoppler U1 auf der Hauptplatine soll später den Schwenkarm zur Verriegelung des Transportfachs detektieren und wird als einziges Bauteil von unten mit der Platine verlötet (Bauteilestempelung in Richtung IC10). Alle anderen Bauteile kommen auf die Oberseite (Bestückungsdruck). Die Kabelbäume werden durch die Schlitze in der Platine nach unten geführt. Das vereinfacht die Montage. Es folgen die Dioden (D1 bis D3); ihr Kennring muss in die vom Bestückungsdruck angegebene Richtung zeigen. Die IC-Sockel besitzen eine Kerbe, die später die korrekte Polung der ICs anzeigt. Mit dem Einsetzen der ICs wartet man jedoch bis ganz zum Schluss.

Die Polung der Keramikkondensatoren C1 bis C4 spielt keine Rolle, ebenso wenig die der Spule (L1) oder des Quarzes (Q1). Der erhält eine Isolierscheibe als Unterlage. Bei den acht Leuchtdioden kennzeichnet das längere Beinchen die Anode (Pluspol). Den Spannungswandler (IC10) knickt man so ab, dass die Kühlfahne rund 1 mm über der Platine schwebt. So bekommt der Wandler auch ein wenig Luft von unten und berührt keine Durchkontaktierungen.

Die lichtempfindliche abgerundete Seite des IR-Empfängers (IC9) braucht freie Sicht nach hinten. Danach lötet man die Stiftleisten J1 bis J8, SW1, SW2 und BR1 sowie die Transistoren (TR1 bis TR6) ein. Ein Jumper auf BR1 aktiviert später die Hintergrundbeleuchtung des Displays.

Den Platz des Widerstandes R29 nimmt eine Zenerdiode mit 2,4 V ein. Ihr Kennring zeigt in Richtung IC10. Bei den restlichen Widerständen spielt die Ausrichtung keine Rolle, sie haben jedoch nur stehend genug Platz. In welche Richtung die zwei Fotowiderstände (LDR1, LDR2) Licht messen sollen, muss jeder selbst entscheiden. Bei den Steckverbindern ST1 bis ST9 und dem Elektrolytkondensator C5 muss man wieder auf die Polung achten. Nun fehlen nur noch das Potenziometer für den Display-Kontrast POT1 und die DC-Buchse P1.

## Erster Test

Bevor man die empfindlichen ICs einsteckt, empfiehlt sich ein Funktionstest der Spannungsversorgung. Dazu hängt man entweder einen Akkupack aus fünf Mignon-Zellen an ST1 oder ein Netzteil mit 6 Volt Gleichspannung an P1. Zeigt ein Voltmeter zwischen Pin 1 und Pin 3 der Leiste J3 5,0 Volt an, arbeitet der Spannungsregler korrekt. Zwischen Pin 2 und Pin 3 liegt hingegen direkt die Eingangsspannung. Achtung, die Belegung des DC-Steckers hat sich gegenüber dem ersten Schaltplan geändert: Masse muss nun außen und nicht mehr innen liegen. Eine Schutzdiode (D3) bewahrt jedoch die Schaltung vor Schäden durch Verpolung. Nach erfolgreichem Spannungstest und Abklemmen der Stromversorgung kann man die verbleibenden ICs einsetzen.

## Ausführlicherer Test der Hauptplatine

Nachdem die Hauptplatine fertig bestückt und der [erste Test](#Erster-Test) bestanden wurde, empfiehlt es sich, die Spannungsversorgung an den Bauteilen der Hauptplatine zu überprüfen, bevor der Bot weiter aufgebaut wird. Kaum ein Bot wird auf Anhieb fehlerfrei funktionieren und die Fehlersuche bei späteren Problemen gestaltet sich sehr viel einfacher, wenn davon ausgegangen werden kann, dass die Hauptplatine weitesgehend fehlerfrei funktioniert (frustrierend dagegen ist, den Bot bei Problemen wieder komplett zerlegen zu müssen).

Eine Hilfe zum Testen der korrekten Spannungsversorgung der Hauptplatine ist dieses Messprotokoll, das in der ODT-Version auch leicht an eigene Bedürfnisse angepasst werden kann: [ct-bot_messprotokoll.pdf](ct-bot_messprotokoll.pdf) und [ct-bot_messprotokoll.odt](ct-bot_messprotokoll.odt).

Eine Orientierungshilfe für die Bauteile der Hauptplatine bietet diese Kombination aus Bestückungs- und Schaltplan: [ct-bot_bestueckungs-schaltplan.pdf](ct-bot_bestueckungs-schaltplan.pdf), [ct-bot_bestueckungs-schaltplan.odt](ct-bot_bestueckungs-schaltplan.odt).

## Fühler

![Image: 'ir-led.jpg'](ir-led.jpg)

**Die IR-LED für die Lichtschranke hat ein leicht bläulich gefärbtes Gehäuse.**

Auf den beiden kleinen Sensorplatinen zeigen Bauteile in drei verschiedene Richtungen. Je eine Reflexlichtschranke (U103, U104) beobachtet den Boden und bekommt die Platine zwischen ihre Beinchen gesteckt. Die Stempelung zeigt bei U104 zur Platine hin, bei U103 weg ? Achtung, an dieser Stelle stimmt der Bestückungsdruck auf der Platine nicht, der hier abgedruckte jedoch schon. Die anderen beiden (U101, U102) beobachten die Räder (siehe Foto). Die Leuchtdiode (LED101, rechte Platine) kommt auf die Unterseite, die Lichtschranke (U105, linke Platine) sitzt auf der Seite mit Bestückungsdruck. Die Lichtschranke muss dazu so abgewinkelt werden, dass ihre flache Seite später zur Leuchtdiode blickt. Das Verlöten aller Kabel hebt man sich bis zur Endmontage auf.

## Mausbau

![Image: 'mausled.jpg'](mausled.jpg)

**Die transparente LED für den Maussensor strahlt rotes Licht aus.**

Damit der Roboter seine Position verfolgen kann, bekommt er seine eigene optische Maus. Dieses Sandwich aus einer Platine und drei FR4-Platten in drei Ebenen meldet Daten über Bewegungen in X- und Y-Richtung direkt an den Mikrocontroller. Die Kondensatoren (C1, C2) und Widerstände (Rxx) platziert man wie fast alle Bauteile auf der Seite ohne Leiterbahnen. Der Quarz Q1 bekommt wieder eine Isolierscheibe, der Transistor Q2 wird abgewinkelt, sodass seine flache Seite auf der Platine aufliegt. Die Stempelungen der beiden Liniensensoren (U1, U2) muss — anders als auf der Platine aufgedruckt — zu der Seite zeigen, an der auch der Quarz sitzt.

Der Sensor-Chip (U3) kommt — nachdem man die Schutzfolie entfernt hat — von der Leiterbahnseite auf die Platine. Das für die Detektion benötigte Licht liefert die Leuchtdiode (LED1) aus einem Kunststoffträger heraus. Dieser sitzt zusammen mit dem Chip auf einer Seite. Die Linsenplatte kommt auf die andere Seite. Die zwei kleinen Platinenstreifen fixieren sie. Je eine PVCMutter und eine Unterlegscheibe sorgen für den richtigen Abstand und verhindern zu hohen Druck auf die Kunststoffteile.

## Schrauber

Vier Platinen sind bestückt, fast alle Teile haben ihren Platz gefunden. Einer Endmontage des Roboters steht nun nichts mehr im Wege. Es lohnt sich, den ganzen Roboter erst einmal lose zu montieren. Dann kann man die Längen der einzelnen Verbindungskabel bequem ausmessen. Sie sollten dabei möglichst dicht an den Alu-Trägern entlang verlaufen, um die Mechanik nicht zu behindern. Einlöten lassen sie sich jedoch leichter in zerlegtem Zustand.

Die Alu-Grundplatte besitzt bereits alle nötigen Bohrungen und Schlitze für Räder, das Maus-Sandwich, die Motorflansche und die Alu-Träger. Die U-förmige Aussparung in Vorausrichtung dient später dem Roboter als Transportfach. Legt man die Alu-Platte so vor sich, dass die Aussparung von einem weg weist, sollte nur links unten eine Bohrung für den Alu-Träger sein. Rechts fehlt sie, die Grundplatte ist nicht symmetrisch.

## Endmontage

![Image: 'gp2d12.gif'](gp2d12.gif)

**Die GP2D12-Sensoren verzeihen keine Verpolung, daher ist beim Anlöten der Kabel unbedingt auf die korrekte Polung zu achten.**

Zunächst befestigt man den Gleitpin mit einer Kreuzschlitzschraube, dann kommt das vorbereitete Maus-Sandwich von unten in die Aussparung. Die beiden Lichtschranken zeigen nach vorne; je zwei Unterlegscheiben halten die Platine auf Abstand zum Aluminium. Zwei Kunststoffmuttern pro Schraube fixieren die Konstruktion von oben. Darauf kommt zum Schutz das letzte unbenutzte Stück Platinenmaterial. Damit der Roboter später seine Räder überwachen kann, klebt man auf die Innenseite der Räder Encoder-Scheiben mit abwechselnd hellen und dunklen Feldern. Bei jedem Übergang zwischen diesen Feldern liefern später die Lichtschranken U101 und U102 ein Signal. Am einfachsten kopiert man die hier abgedruckten Vorlagen auf eine transparente Folie. Wer sie lieber ausdruckt, findet eine Bilddatei auf der Projektseite. Der Schwärzungsgrad eines üblichen Laser-Druckers sollte für die CNY70-Sensoren ausreichen. Zur Not malt man die dunklen Streifen mit einem Folienstift nach. Hat man die Motoren mit M2-Schrauben an ihren Flanschen befestigt und die Räder aufgesteckt - bitte keine Gewalt anwenden - , sichert man diese mit je einer Madenschraube. M3-Schrauben von oben und Muttern von unten verbinden diese Motorblöcke mit der Grundplatte.

Die beiden vorderen Alu-Träger halten die Sensorplatinen - eine Unterlegscheibe zwischen Schraubenkopf und Sensorplatine verhindert Kurzschlüsse - und die Abstandssensoren in Position. Deren dreipolige Stecker zeigen nach links beziehungsweise rechts außen, sodass die Kabel nicht in das Transportfach hineinragen. Kurze Kabel verbinden sie mit den Sensorplatinen. Achtung: Die GP2D12-Sensoren verzeihen keine Verpolung. Schaut man direkt in den Stecker hinein und zeigen die Sensoren nach oben, so liegt links Pin 1. Beim linken Sensor ist die rote Ader oben, beim rechten unten. Die rote Ader lötet man dann an dem Pad der Platine fest, das am weitesten vom Alu-Träger entfernt ist. Nach Möglichkeit sollte man vermeiden, dass der GP2D12 in direkten Kontakt mit den Lötstellen der Platinen kommt. Das Gehäuse ist nicht perfekt isolierend. Zuoberst thront dann die Hauptplatine.

## Kabelbaum

Beim Verlegen der einseitig vorkonfektionierten Kabel für ST1 bis ST3 und ST7 bis ST9 sollte man oberhalb der Hauptplatine genug Spiel lassen, um die Stecker nachher abziehen zu können. Kabelschellen und -binder halten die Leitungen im Zaum. Dank der Schlitze in der Hauptplatine muss kein Kabel über den Umfang der Grundplatte hinausragen — der Roboter kann sich später nicht verheddern. Die Pinbelegung der doppelreihigen Lötfelder auf den Sensorplatinen entspricht der üblichen Zählweise — im Zickzack. Pin 1 (schwarzes Kabel) hat ein eckiges Löt-Pad. Bei beiden Motoren hängt das braune Kabel am Pluspol. Damit laufen sie gegensinnig, aber das korrigiert die Firmware des Mikrocontrollers.
Die beiden Batteriehalter verklebt man zu einem Block, der später auf dem Maus-Sandwich ruht; Moosgummi-Pads und Klettband halten ihn in Position. Elektrisch hängen alle fünf Batterien in Reihe; dazu verbindet man den Pluspol des einen Päckchens mit dem Minuspol des anderen.

## Fotos

Die hochauflösenden Fotos von den fertig bestückten Platinen sollten alle Zweifel über die Platzierung und Ausrichtung der Bauteile in Luft auflösen. Weitere finden sich in c't 04/06:

* Oberseite Hauptplatine von vorne gesehen:
  ![Image: 'hauptplatine_vorne.jpg'](hauptplatine_vorne.jpg)
* Oberseite Hauptplatine von hinten gesehen:
  ![Image: 'hauptplatine_hinten.jpg'](hauptplatine_hinten.jpg)
* Oberseite Hauptplatine von links gesehen:
  ![Image: 'hauptplatine_links.jpg](hauptplatine_links.jpg)
* Oberseite Hauptplatine von rechts gesehen:
  ![Image: 'hauptplatine_rechts.jpg'](hauptplatine_rechts.jpg)
* Oberseite unbestückte Hauptplatine mit IC-Sockeln:
  ![Image: 'hauptplatine_unten.jpg'](hauptplatine_unten.jpg)
* Oberseite Mausplatine:
  ![Image: 'maussensor.jpg'](maussensor.jpg)
* Innenseite linke Sensorplatine von hinten gesehen:
  ![Image: 'sensorplatine_links_hinten_innen.jpg'](sensorplatine_links_hinten_innen.jpg)
* Innenseite linke Sensorplatine von schräg vorne bzw. unten gesehen:
  ![Image: 'sensorplatine_links_vorne_innen.jpg'](sensorplatine_links_vorne_innen.jpg)
* Außenseite rechte Sensorplatine von hinten gesehen:
  ![Image: 'sensorplatine_rechts_aussen_hinten.jpg'](sensorplatine_rechts_aussen_hinten.jpg)
* Unterseite Mausplatine:
  ![Image: 'platine_a.jpg'](platine_a.jpg)
* Oberseite Mausplatine:
  ![Image: 'platine_b.jpg'](platine_b.jpg)
* Liniensensoren auf der Mausplatine:
  * Der Bestückungsdruck ist falsch, beide Stempelungen der CNY70-Sensoren müssen in diesem Bild nach oben zeigen:
    ![Image: 'det01.jpg'](det01.jpg)
  * Beide CNY-70-Sensoren auf der Mausplatine müssen in die selbe Richtung orientiert sein:
    ![Image: 'det02.jpg'](det02.jpg)

[![License: CC BY-SA 4.0](../license.svg)](https://creativecommons.org/licenses/by-sa/4.0/)
