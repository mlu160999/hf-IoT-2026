# Geschichte des Internet of Things (IoT) – Lernnotizen

**Quelle:** `geschichte von iot.pdf`, Smartlearn – Modul HFI_IOT, Kapitel 2.2

---

# Inhaltsverzeichnis

1. [Was behandelt dieses Kapitel?](#kapitel-1)
2. [Die vier Entwicklungsschritte](#kapitel-2)
3. [Kein eindeutiges Geburtsjahr](#kapitel-3)
4. [Der Internet-Toaster von 1990](#kapitel-4)
5. [Kevin Ashton und der Begriff „Internet of Things“](#kapitel-5)
6. [Grundidee des IoT](#kapitel-6)
7. [Entwicklung vom verbundenen Gerät zum intelligenten System](#kapitel-7)
8. [Die zukünftige Entwicklung](#kapitel-8)
9. [Chancen des IoT](#kapitel-9)
10. [Risiken und Schattenseiten](#kapitel-10)
11. [Einordnung der Geschichte](#kapitel-11)
12. [Mögliche Portfolio-Antwort](#kapitel-12)
13. [Häufige Verständnisfehler](#kapitel-13)
14. [Exam Notes / Prüfungsnotizen](#exam-notes)

---

<a id="kapitel-1"></a>
## 1. Was behandelt dieses Kapitel?

Das Kapitel erklärt, wie das **Internet of Things (IoT)** entstanden ist und wohin sich diese Technologie entwickeln könnte.

IoT besitzt **kein einzelnes, genaues Geburtsjahr**. Es entstand über einen langen Zeitraum aus mehreren technischen Entwicklungen. Die Entstehung wird deshalb als schleichender Prozess beschrieben, der sich zunehmend beschleunigt.

Vier wichtige Entwicklungsschritte führten zum IoT:

1. **Mechanisierung**
2. **Elektrifizierung**
3. **Automatisierung**
4. **Vernetzung**

---

<a id="kapitel-2"></a>
# 2. Die vier Entwicklungsschritte

## 2.1 Mechanisierung

Bei der Mechanisierung werden menschliche oder tierische Arbeiten durch Maschinen unterstützt oder ersetzt.

Beispiele:

- mechanische Webstühle
- Dampfmaschinen
- Maschinen in Fabriken

Die Maschinen arbeiteten zunächst weitgehend isoliert und mussten direkt von Menschen bedient werden.

**Bedeutung für IoT:** Mechanisierung schuf die physischen Maschinen, die später elektrifiziert, automatisiert und vernetzt werden konnten.

## 2.2 Elektrifizierung

Elektrische Energie ermöglichte leistungsfähigere, flexiblere und besser steuerbare Maschinen.

Beispiele:

- Elektromotoren
- elektrische Beleuchtung
- elektrische Haushaltsgeräte
- elektrische Sensor- und Steuerschaltungen

**Bedeutung für IoT:** Ohne elektrische Energie könnten Sensoren, Mikrocontroller, Netzwerkmodule und Aktoren nicht betrieben werden.

## 2.3 Automatisierung

Automatisierung bedeutet, dass Maschinen Abläufe teilweise oder vollständig selbstständig ausführen.

Ermöglicht wurde dies unter anderem durch:

- Sensoren
- Steuerungen
- Computer
- Mikrocontroller
- Software

Beispiel:

```text
Sensor erkennt hohen Druck
        ↓
Steuerung verarbeitet den Messwert
        ↓
Ventil wird automatisch geöffnet
```

**Bedeutung für IoT:** Ein IoT-Gerät soll nicht nur verbunden sein, sondern Daten erfassen, verarbeiten und auf Ereignisse reagieren können.

## 2.4 Vernetzung

Bei der Vernetzung tauschen Geräte und Systeme Informationen miteinander aus.

Mögliche Verbindungen:

- lokale Netzwerke
- WLAN
- Mobilfunk
- Internet
- industrielle Netzwerke

Durch Vernetzung können Geräte:

- Daten an andere Systeme senden,
- Steuerbefehle empfangen,
- zentral überwacht werden,
- miteinander zusammenarbeiten.

**Bedeutung für IoT:** Erst durch die Verbindung von physischen Dingen mit digitalen Netzwerken entsteht das eigentliche Internet of Things.

---

<a id="kapitel-3"></a>
# 3. Kein eindeutiges Geburtsjahr

IoT entstand nicht durch eine einzige Erfindung. Viele Voraussetzungen mussten zuerst vorhanden sein:

- elektrische Geräte
- Sensoren und Aktoren
- Mikrocontroller
- Computer
- Netzwerke und Internet
- kleine und günstige Funkmodule
- geeignete Software und Protokolle

Deshalb beschreibt man die Geschichte des IoT besser als **kontinuierliche Entwicklung** und nicht als einzelnes Ereignis.

---

<a id="kapitel-4"></a>
# 4. Der Internet-Toaster von 1990

Ein bekannter früher IoT-Meilenstein ist ein mit dem Internet verbundener Toaster.

Im Jahr **1990** verbanden:

- **John Romkey**, US-amerikanischer Software- und Netzwerkexperte
- **Simon Hackett**, australischer Computerwissenschaftler

während einer Konferenz einen Toaster mit dem Internet.

Der Toaster konnte über das Netzwerk:

- eingeschaltet werden,
- ausgeschaltet werden.

## Warum ist der Toaster wichtig?

Er zeigte bereits das zentrale Prinzip des IoT:

```text
physisches Gerät
      ↕
Netzwerk/Internet
      ↕
entfernte Steuerung
```

Der Toaster war allerdings hauptsächlich ein Experiment beziehungsweise Forschungsgegenstand. Er wurde damals noch nicht breit in Alltag oder Industrie eingesetzt.

> **Merksatz:** Der Internet-Toaster von 1990 gilt als eines der frühen bekannten Beispiele eines über das Internet steuerbaren physischen Geräts.

---

<a id="kapitel-5"></a>
# 5. Kevin Ashton und der Begriff „Internet of Things“

Der Begriff **Internet of Things** wurde im Jahr **1999** durch **Kevin Ashton** bekannt.

Ashton arbeitete bei **Procter & Gamble**. Dort bemerkte er ein Problem mit einem braunen Lippenstift:

- Der Lippenstift war im Lager vorhanden.
- Im Verkaufsregal war er offenbar häufig ausverkauft.
- Zwischen Verkaufsfläche, Lager und Lieferkette fehlte ein automatischer Informationsaustausch.

## 5.1 Ashtons Idee

Ashton wollte Produkte und Regale mit elektronischer Identifikation ausstatten. Dazu dachte er an:

- einen Chip,
- eine RFID-Antenne,
- eine Verbindung zur Lieferkette.

Das System sollte automatisch erkennen, wenn im Regal kein Lippenstift mehr vorhanden war. Anschliessend sollte elektronisch Nachschub organisiert werden.

Vereinfachter Datenfluss:

```text
Produkt wird aus Regal genommen
            ↓
RFID/System erkennt neuen Bestand
            ↓
Bestandsdaten werden elektronisch übertragen
            ↓
Lager oder Lieferkette reagiert
            ↓
Regal wird nachgefüllt
```

## 5.2 Smart Packaging

Procter & Gamble untersuchte 1999 die Idee des **Smart Packaging**. Produkte oder Verpackungen sollten digital identifizierbar und mit Informationssystemen verbunden werden.

Ashton verwendete für eine Präsentation den Titel **Internet of Things**. Dadurch wurde der Begriff bekannt.

## 5.3 Bedeutung von RFID

**RFID** steht für **Radio-Frequency Identification**.

Ein RFID-System besteht typischerweise aus:

- RFID-Tag beziehungsweise Transponder
- Antenne
- Lesegerät
- Anwendung oder Datenbank

RFID erlaubt die automatische Identifikation von Gegenständen über Funk.

Wichtig:

- RFID bedeutet nicht automatisch, dass jeder einzelne Tag direkt mit dem Internet verbunden ist.
- Meist liest ein RFID-Lesegerät den Tag.
- Das Lesegerät oder ein angeschlossenes System überträgt die Daten weiter.

---

<a id="kapitel-6"></a>
# 6. Grundidee des IoT

Das IoT verbindet physische Gegenstände mit digitalen Informationssystemen.

Ein vereinfachtes IoT-System:

```text
physische Umwelt
      ↓
Sensor oder Identifikation
      ↓
Mikrocontroller/Gateway
      ↓
Netzwerk und Internet
      ↓
Server, Cloud oder Edge-System
      ↓
Datenauswertung und Entscheidung
      ↓
Information für Menschen oder Befehl an einen Aktor
```

IoT-Geräte können:

- ihre Umgebung erfassen,
- ihren Zustand melden,
- Daten austauschen,
- aus der Ferne gesteuert werden,
- automatisch auf Situationen reagieren.

---

<a id="kapitel-7"></a>
# 7. Entwicklung vom verbundenen Gerät zum intelligenten System

Ein Gerät wird nicht allein dadurch intelligent, dass es mit dem Internet verbunden ist.

Die Entwicklung kann in mehreren Stufen betrachtet werden:

1. **Erfassen:** Ein Sensor misst Daten.
2. **Übertragen:** Die Daten werden über ein Netzwerk gesendet.
3. **Speichern:** Ein Server oder eine Datenbank speichert die Messungen.
4. **Auswerten:** Software erkennt Zustände und Muster.
5. **Entscheiden:** Eine Regel oder KI leitet eine Handlung ab.
6. **Handeln:** Ein Mensch wird informiert oder ein Aktor wird gesteuert.
7. **Lernen/Optimieren:** Das System verbessert Entscheidungen anhand historischer Daten.

Beispiel:

```text
Temperatursensor misst 31 °C
          ↓
ESP32 sendet Messwert
          ↓
Server erkennt Grenzwertüberschreitung
          ↓
Lüfter wird eingeschaltet
          ↓
Temperatur sinkt
          ↓
System dokumentiert den Verlauf
```

---

<a id="kapitel-8"></a>
# 8. Die zukünftige Entwicklung

Laut Kursunterlagen umfasst das IoT – abhängig von Studie und Schätzung – sehr viele Milliarden Maschinen, Smartphones, Computer und andere Geräte. Solche Zahlen ändern sich schnell und hängen davon ab, was eine Studie als IoT-Gerät zählt.

Für die Prüfung ist deshalb wichtiger:

- Die Zahl vernetzter Geräte wächst.
- Dadurch entstehen sehr grosse Datenmengen.
- Der Nutzen entsteht nicht nur durch das Sammeln, sondern vor allem durch die sinnvolle Auswertung der Daten.

## 8.1 Big Data

**Big Data** beschreibt Datenmengen, die wegen ihrer Grösse, Geschwindigkeit oder Vielfalt nicht mehr einfach mit traditionellen Verfahren verarbeitet werden können.

IoT erzeugt beispielsweise:

- kontinuierliche Sensormessungen,
- Standortdaten,
- Maschinenzustände,
- Energieverbrauchsdaten,
- Verkehrs- und Umweltdaten.

Der Wert entsteht erst, wenn aus diesen Daten relevante Informationen gewonnen werden.

## 8.2 Künstliche Intelligenz

KI kann IoT-Daten verwenden, um:

- Muster zu erkennen,
- Fehler vorherzusagen,
- Anomalien zu finden,
- Entscheidungen zu unterstützen,
- Prozesse zu optimieren.

Beispiel Predictive Maintenance:

```text
Maschinensensoren
      ↓
Vibrations- und Temperaturdaten
      ↓
KI erkennt ungewöhnliches Muster
      ↓
Wartung wird vor dem Ausfall geplant
```

## 8.3 Entscheidungsunterstützung

Die zukünftige Bedeutung des IoT liegt besonders darin, aus sehr vielen Daten die entscheidenden Informationen zu ermitteln.

Menschen sollen dadurch:

- Probleme früher erkennen,
- bessere Entscheidungen treffen,
- Ressourcen effizienter verwenden,
- Prozesse automatisieren.

---

<a id="kapitel-9"></a>
# 9. Chancen des IoT

## Alltag und Smart Home

- automatische Beleuchtung
- intelligente Heizungssteuerung
- Energieüberwachung
- Alarm- und Sicherheitssysteme
- Unterstützung älterer oder beeinträchtigter Menschen

## Industrie

- Maschinenüberwachung
- Predictive Maintenance
- automatische Qualitätskontrolle
- effizientere Produktion
- transparente Lieferketten

## Umwelt und Landwirtschaft

- Bodenfeuchtemessung
- gezielte Bewässerung
- Luftqualitätsmessung
- Wetterstationen
- Überwachung von Wasserverbrauch

## Verkehr und Städte

- Verkehrsfluss analysieren
- Parkplätze erkennen
- Strassenbeleuchtung steuern
- öffentlichen Verkehr optimieren

## Medizin

- Wearables
- Überwachung von Vitalwerten
- Medikamentenerinnerungen
- Fernbetreuung

---

<a id="kapitel-10"></a>
# 10. Risiken und Schattenseiten

Die Kursunterlagen erwähnen, dass der IoT-Trend neben Vorteilen auch gesellschaftliche Probleme verursachen kann.

## 10.1 Veränderung der Arbeitswelt

Automatisierte und vernetzte Maschinen können bestimmte menschliche Tätigkeiten übernehmen.

Mögliche Folgen:

- bestimmte Berufe verändern sich,
- repetitive Tätigkeiten werden automatisiert,
- neue Qualifikationen werden benötigt,
- einige Arbeitsplätze können verschwinden,
- neue Arbeitsplätze in Entwicklung, Betrieb und Sicherheit entstehen.

Die Unterlagen beziehen sich auf die Befürchtung, dass Maschinen Menschen in Produktion und Fertigung teilweise ersetzen könnten, weil sie günstiger oder effizienter arbeiten.

## 10.2 Datenschutz

IoT-Geräte können sehr persönliche Daten sammeln:

- Anwesenheit zu Hause
- Bewegungsmuster
- Gesundheitsdaten
- Sprachaufnahmen
- Kamerabilder
- Verbrauchsgewohnheiten

Wichtige Fragen:

- Welche Daten werden gesammelt?
- Wer besitzt die Daten?
- Wo werden sie gespeichert?
- Wer darf sie verwenden?
- Wie lange bleiben sie gespeichert?

## 10.3 IT-Sicherheit

Unsichere IoT-Geräte können angegriffen oder übernommen werden.

Risiken:

- Standardpasswörter
- fehlende Updates
- unverschlüsselte Kommunikation
- unsichere Cloud-Dienste
- schlecht geschützte Schnittstellen
- Geräte als Teil eines Botnetzes
- Manipulation physischer Aktoren

## 10.4 Abhängigkeit

Wenn Cloud, Internet oder Herstellerdienst ausfallen, kann ein Gerät Funktionen verlieren.

Ein robustes System sollte deshalb:

- wichtige Funktionen lokal ausführen können,
- sichere Standardzustände besitzen,
- Netzwerkausfälle erkennen,
- Daten wenn möglich zwischenspeichern.

## 10.5 Ressourcen und Elektroschrott

Milliarden Geräte benötigen:

- Rohstoffe,
- Energie,
- Batterien,
- Herstellung und Transport.

Kurze Produktlebensdauer oder fehlende Softwareupdates erhöhen den Elektroschrott.

---

<a id="kapitel-11"></a>
# 11. Einordnung der Geschichte

Die Geschichte des IoT kann als Entwicklung von isolierten Maschinen zu vernetzten, datenbasierten Systemen verstanden werden:

```text
Mechanisierung
      ↓
Elektrifizierung
      ↓
Automatisierung
      ↓
Vernetzung
      ↓
Internet of Things
      ↓
Big Data und künstliche Intelligenz
```

Dabei baut jede Stufe auf früheren technischen Entwicklungen auf.

IoT ist gleichzeitig:

- das Ergebnis früherer Erfindungen,
- eine Plattform für neue Anwendungen,
- ein Auslöser weiterer technischer und gesellschaftlicher Veränderungen.

---

<a id="kapitel-12"></a>
# 12. Mögliche Portfolio-Antwort

## Wohin geht die IoT-Reise?

Das Internet of Things wird sich wahrscheinlich weiter ausbreiten. Immer mehr Alltagsgegenstände, Fahrzeuge, Maschinen und Infrastrukturen werden Sensoren, Software und Netzwerkverbindungen enthalten. Der wichtigste Nutzen wird nicht allein darin liegen, möglichst viele Geräte zu verbinden. Entscheidend wird sein, aus den entstehenden Daten zuverlässige und nützliche Informationen zu gewinnen.

Big Data und künstliche Intelligenz können dabei helfen, Fehler vorherzusagen, Energie zu sparen, Verkehrsflüsse zu optimieren oder medizinische Veränderungen frühzeitig zu erkennen. Gleichzeitig steigen die Anforderungen an Datenschutz, IT-Sicherheit und Nachhaltigkeit. Ein IoT-System sollte deshalb nicht nur technisch funktionieren, sondern auch sicher, transparent, reparierbar und langfristig wartbar sein.

Meiner Meinung nach wird sich IoT besonders dort durchsetzen, wo es ein konkretes Problem löst. Ein vernetztes Gerät ohne erkennbaren Nutzen erzeugt lediglich zusätzliche Kosten, Daten und Sicherheitsrisiken. Erfolgreiches IoT muss den Menschen unterstützen und darf ihn nicht unnötig überwachen oder vollständig von einem einzelnen Cloud-Anbieter abhängig machen.

---

<a id="kapitel-13"></a>
# 13. Häufige Verständnisfehler

1. **IoT wurde nicht an einem einzigen Tag erfunden.** Es entstand aus vielen Entwicklungen.
2. Der Internet-Toaster war ein früher Demonstrator, aber noch kein verbreitetes Alltagsprodukt.
3. **Kevin Ashton** machte den Begriff **Internet of Things** im Jahr **1999** bekannt.
4. RFID-Tags sind nicht automatisch selbst direkt mit dem Internet verbunden.
5. Mehr gesammelte Daten bedeuten nicht automatisch bessere Entscheidungen.
6. Vernetzung allein macht ein Gerät nicht intelligent.
7. IoT besteht nicht nur aus Smart-Home-Geräten; es umfasst auch Industrie, Medizin, Landwirtschaft und Infrastruktur.
8. Sicherheit und Datenschutz müssen schon beim Entwurf berücksichtigt werden.
9. Historische Gerätezahlen aus Präsentationen sind zeitabhängige Schätzungen, keine dauerhaften Fakten.

---

<a id="exam-notes"></a>
# Exam Notes / Prüfungsnotizen

## Unbedingt merken

- IoT besitzt **kein genaues Geburtsjahr**.
- Vier zentrale Meilensteine: **Mechanisierung, Elektrifizierung, Automatisierung, Vernetzung**.
- **1990:** John Romkey und Simon Hackett verbinden einen Toaster mit dem Internet.
- **1999:** Kevin Ashton macht den Begriff **Internet of Things** bekannt.
- Ausgangsproblem bei Procter & Gamble: Ein Lippenstift war im Lager vorhanden, aber im Regal nicht verfügbar.
- Ashtons Lösungsidee: Produktbestand mithilfe von **RFID** elektronisch erkennen und mit der Lieferkette verbinden.
- **RFID:** Radio-Frequency Identification.
- IoT verbindet **physische Dinge, Sensoren/Aktoren, Software und Netzwerke**.
- Zukunft: mehr vernetzte Geräte, Big Data, KI und bessere Entscheidungsunterstützung.
- Chancen: Automatisierung, Effizienz, Komfort, frühzeitige Fehlererkennung.
- Risiken: Datenschutz, Cyberangriffe, Abhängigkeit, Arbeitsplatzveränderung und Elektroschrott.
- Der eigentliche Nutzen entsteht nicht durch möglichst viele Daten, sondern durch **zuverlässige Auswertung und sinnvolle Handlungen**.
