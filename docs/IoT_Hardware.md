# IoT-Hardware – Lernnotizen

> Ziel: Die vielen Einzelinformationen als zusammenhängenden Weg verstehen:  
> **physikalische Umwelt → Sensor → Mikrocontroller/Firmware → Bus/WLAN → Protokoll → Server/Broker → Speicherung/UI → Aktor**

---

# Inhaltsverzeichnis

1. [Sensoren, Aktoren und Gateway](#kapitel-2)
2. [Umgebungssensoren aus dem Kurs](#kapitel-3)
3. [ESP8266](#kapitel-4)
4. [ESP32](#kapitel-5)
5. [GPIO](#kapitel-6)
6. [I²C-Bus](#kapitel-7)
7. [SPI-Bus](#kapitel-8)
8. [UART](#kapitel-9)


---

---

<a id="kapitel-1"></a>
## 1. Sensoren, Aktoren und Gateway

## 1.1 Sensor

Ein **Sensor** wandelt eine physikalische Grösse in ein elektrisches bzw. maschinenlesbares Signal um.

Beispiele:

- Temperatur
- Luftfeuchtigkeit
- Luftdruck
- Licht
- Schall
- Bewegung und Beschleunigung
- Abstand und Näherung
- Füllstand
- Rauch oder Gas
- Bild
- Magnetfeld

Wichtige Auswahlkriterien:

- Messbereich
- Genauigkeit
- Auflösung
- Zuverlässigkeit
- Reaktionszeit
- Leistungsaufnahme
- Versorgungsspannung
- Schnittstelle, z. B. analog, I²C oder SPI
- Eignung für Innen-/Aussenbereich

Ein Sensor ist ein **Data Endpoint**, weil er Daten erzeugt.

## 1.2 Aktor

Ein **Aktor** wandelt ein elektrisches Signal in eine physikalische Wirkung um.

Beispiele:

- LED → Licht
- Lautsprecher → Schall
- Motor → Bewegung
- Heizung → Wärme
- Relais → schaltet einen anderen Stromkreis
- Elektromagnet → magnetische Kraft

**Merksatz:**

- Sensor: **physikalisch → elektrisch/digital**
- Aktor: **elektrisch/digital → physikalisch**

## 1.3 Gateway/Mikrocontroller

Im Kurs übernehmen ESP8266 oder ESP32 die Rolle des Controllers beziehungsweise Gateways. Sie können:

- Sensoren auslesen,
- lokale Logik ausführen,
- Aktoren ansteuern,
- Daten über WLAN weiterleiten,
- MQTT oder andere Netzwerkprotokolle verwenden.

### Edge oder zentraler Server?

**Edge-Verarbeitung:** Der ESP entscheidet lokal, z. B. „wenn Gaswert zu hoch, sofort Alarm auslösen“.

Vorteile:

- kurze Reaktionszeit,
- funktioniert teilweise ohne Internet,
- weniger Netzwerkverkehr,
- sensible Daten können lokal bleiben.

**Zentrale Verarbeitung:** Der ESP sendet Rohdaten an Home Assistant oder einen Cloud-Service; dort wird entschieden.

Vorteile:

- komplexere Regeln,
- zentrale Verwaltung,
- Langzeitspeicherung,
- komfortable UI.

In realen Systemen wird oft beides kombiniert: **sicherheitskritische Reaktion lokal**, Komfort und Auswertung zentral.

---

<a id="kapitel-2"></a>
## 2. Umgebungssensoren aus dem Kurs

## 2.1 BMP280

Misst:

- Temperatur
- Luftdruck

Eigenschaften:

- Schnittstellen: I²C und SPI
- Betrieb typischerweise mit 3,3 V
- geringe Leistungsaufnahme
- kann aus Luftdruckänderungen auch Höhenunterschiede ableiten
- `forced mode`: genau eine Messung durchführen und danach wieder in den Sleep-Modus wechseln

Einsatz:

- Wetterstation
- Höhenmessung
- Wearables
- mobile Geräte

## 2.2 BME280

Misst:

- Temperatur
- Luftdruck
- relative Luftfeuchtigkeit

Das **E** steht für **Environmental**.

Wichtig: Ein konkretes Breakout-Board kann weniger Schnittstellen herausführen als der eigentliche Sensorchip. Laut Kurs-Breakout ist dort nur I²C verwendbar, obwohl der Chip grundsätzlich auch SPI unterstützt.

Erkennung laut Unterlagen:

- BMP280-Gehäuse eher rechteckig
- BME280-Gehäuse eher quadratisch

Beim Kauf trotzdem immer Beschriftung und Datenblatt prüfen, weil Module teilweise falsch angeboten werden.

## 2.3 BME680

Misst:

- Temperatur
- Luftdruck
- Luftfeuchtigkeit
- Gaswiderstand als Hinweis auf Luftqualität

Schnittstellen:

- I²C
- SPI

Besonderheit:

- Eine kleine interne Heizung wird für die Gasmessung verwendet.
- Häufige Messungen können die Temperaturmessung erwärmen und verfälschen.
- Der Kurs nennt bis zu ungefähr 1,5 °C Abweichung bei vielen schnellen Messungen.

> **Wichtige Korrektur:** Ein BME680 ist **kein echter CO₂-Sensor**. Er misst den Widerstand einer beheizten Gassensorschicht und kann daraus Luftqualitäts-/VOC-Trends ableiten. Einen exakten CO₂-Wert erhält man nur mit einem dafür vorgesehenen CO₂-Sensor.

---

<a id="kapitel-3"></a>
## 3. ESP8266

Der ESP8266 ist ein günstiger 32-Bit-Mikrocontroller von Espressif mit integriertem 2,4-GHz-WLAN. Er eignet sich für kleinere IoT-Aufgaben.

## 3.1 ESP-01

Eigenschaften:

- sehr klein
- nur zwei allgemein gut nutzbare GPIO-Pins
- UART vorhanden
- geeignet für einfache Aufgaben, z. B. einen Sensor oder ein Relais
- zum komfortablen Flashen meist USB-Programmer nötig

## 3.2 ESP-12 / Wemos D1 mini / NodeMCU

Der ESP-12 stellt deutlich mehr Pins bereit. Das nackte Modul hat ein Raster von ungefähr 2 mm und passt daher nicht direkt in ein Breadboard mit 2,54-mm-Raster. Entwicklerboards wie **Wemos D1 mini** oder **NodeMCU** lösen dieses Problem und ergänzen:

- USB-Anschluss
- USB-UART-Wandler
- Spannungsregler
- breadboardfreundliche Pins

Kursangaben zum Wemos D1 mini:

- Controller: ESP8266/ESP-12E
- Betriebsspannung des Chips: 3,3 V
- CPU: 80 MHz
- Flash: 4 MB
- ein analoger Eingang
- WLAN als Client oder Access Point
- Micro-USB am Entwicklungsboard

## 3.3 ESP8266 und Deep Sleep

WLAN benötigt im aktiven Betrieb relativ viel Strom. Für Batteriebetrieb soll der ESP deshalb nur kurz aufwachen:

```text
aufwachen → Sensor versorgen/messen → WLAN verbinden → Daten senden → Deep Sleep
```

In den Unterlagen genannte Betriebsmodi:

- **Active mode:** CPU und WLAN aktiv; Daten können verarbeitet und übertragen werden.
- **Modem sleep:** CPU läuft, WLAN sendet gerade nicht.
- **Light sleep:** CPU und angeschlossene Funktionen weitgehend pausiert.
- **Deep sleep:** fast alles ausgeschaltet; RTC bleibt für das zeitgesteuerte Aufwachen aktiv.

Aufwecken:

- nach einer definierten Zeit
- über `RST`, z. B. mit einem Taster
- beim ESP8266 wird für zeitgesteuertes Aufwachen häufig `GPIO16` mit `RST` verbunden

> Die reale Stromaufnahme hängt nicht nur vom ESP-Chip ab. Spannungsregler, Power-LED, USB-UART-Chip und Sensoren auf einem Development Board können im Deep Sleep weiterhin Strom verbrauchen.

## 3.4 Wichtige ESP8266-Pins

| Pin | Typische Funktion / Einschränkung |
|---|---|
| `GPIO0` | Boot-Strapping-Pin; für Flash-Modus LOW; falscher Pegel verhindert normalen Boot |
| `GPIO1` | UART `TX`; serielle Ausgaben beim Booten |
| `GPIO2` | Boot-Strapping-Pin; muss für normalen Boot passend HIGH sein |
| `GPIO3` | UART `RX` |
| `GPIO4` | oft I²C `SDA` |
| `GPIO5` | oft I²C `SCL` |
| `GPIO12` | häufig SPI `MISO` |
| `GPIO13` | häufig SPI `MOSI` |
| `GPIO14` | häufig SPI `SCLK` |
| `GPIO15` | Boot-Strapping-Pin; typischerweise Pull-down; häufig SPI `CS` |
| `GPIO16` | Deep-Sleep-Wakeup; besondere Einschränkungen bei Interrupt/PWM/I²C |
| `ADC/A0` | analoger Eingang; erlaubte Maximalspannung ist boardabhängig |

> **Achtung:** Beim nackten ESP8266-ADC gilt typischerweise ein kleinerer Spannungsbereich als bei manchen Development Boards. Das Kursmaterial nennt für das Wemos-Board maximal 3,2 V, an anderer Stelle 0–1 V. Das ist kein Widerspruch des Chips, sondern hängt vom eingebauten Spannungsteiler des Boards ab. **Immer Datenblatt und Schaltplan des konkreten Boards prüfen.**

---

<a id="kapitel-4"></a>
## 4. ESP32

Der klassische ESP32 ist der leistungsfähigere Nachfolger des ESP8266.

Eigenschaften aus den Kursunterlagen:

- Dual-Core Tensilica LX6
- bis 240 MHz
- 520 KB internes SRAM
- typischerweise 4 MB Flash auf dem Board
- 2,4-GHz-WLAN, IEEE 802.11 b/g/n
- Bluetooth Classic und BLE 4.2 beim klassischen ESP32
- USB-UART-Wandler `CP2104` beim ESP32 Wemos MiniKit
- mehrere GPIO-, ADC-, SPI-, I²C- und UART-Funktionen
- kapazitive Touch-Eingänge
- zwei 8-Bit-DAC-Ausgänge beim klassischen ESP32
- Hall-Sensor beim klassischen ESP32
- Logik/Betrieb des Chips: 3,3 V

> Der Chip besitzt viele interne GPIO-Signale, aber **nicht alle sind auf jedem Board herausgeführt oder frei verwendbar**. Board-Pinout und ESP32-Variante müssen zum Programm passen.

## 4.1 ESP32 gegenüber ESP8266

| Merkmal | ESP8266 | klassischer ESP32 |
|---|---|---|
| WLAN | ja | ja |
| Bluetooth | nein | Classic + BLE |
| CPU | meist Single-Core, 80 MHz | Dual-Core, bis 240 MHz |
| GPIO/Peripherie | weniger | mehr |
| ADC | weniger, typischerweise 10 Bit | mehrere ADC-Pins, 12 Bit nominal |
| DAC | kein echter DAC | zwei 8-Bit-DACs (`GPIO25`, `GPIO26`) |
| Touch | nein | mehrere kapazitive Touch-Pins |
| typische Nutzung | kleine/günstige WLAN-Aufgaben | komplexere IoT-Aufgaben, BLE, mehr Sensoren |

## 4.2 Wichtige Sicherheit

- ESP32-GPIO arbeitet mit **3,3-V-Logik**.
- Ein GPIO darf nicht einfach mit 5 V belastet werden.
- ESP32-Pins gelten im Allgemeinen **nicht als 5-V-tolerant**.
- Für 5-V-Signale sind Spannungsteiler oder Pegelwandler nötig.
- Stromintensive Verbraucher wie Motoren, Relais, starke LEDs oder Kameras dürfen nicht direkt aus einem GPIO versorgt werden.
- Externe Aktoren benötigen Treiberstufe, Transistor/MOSFET und bei induktiven Lasten eine Freilaufdiode.
- Alle verbundenen Schaltungsteile benötigen normalerweise ein gemeinsames `GND`.

---

<a id="kapitel-5"></a>
## 5. GPIO

**GPIO** bedeutet **General Purpose Input/Output**. Ein GPIO-Pin kann durch die Firmware für unterschiedliche Aufgaben konfiguriert werden.

Mögliche Rollen:

- `DigitalIn`
- `DigitalOut`
- `AnalogIn` über ADC-fähige Pins
- analogähnlicher Ausgang über PWM
- echter Analogausgang über DAC-fähige Pins
- Interrupt-Eingang
- alternative Busfunktion wie I²C, SPI oder UART

Nicht jeder Pin unterstützt jede Funktion.

## 5.1 Digital und analog

### Digital

Ein digitales Signal kennt diskrete logische Zustände:

- LOW / `0` / `False`
- HIGH / `1` / `True`

Ein digitaler Eingang misst nicht den genauen Spannungswert, sondern ordnet ihn einem Logikzustand zu. Zwischen garantiertem LOW und HIGH kann ein undefinierter Bereich liegen.

### Analog

Ein analoges Signal kann innerhalb eines Bereichs viele kontinuierliche Spannungswerte annehmen. Ein ADC übersetzt die Spannung in eine Zahl.

## 5.2 Pull-up und Pull-down

Ein unbeschalteter Eingang kann **floaten** und zufällige Werte liefern.

- **Pull-up:** Widerstand zieht den Eingang im Ruhezustand auf HIGH.
- **Pull-down:** Widerstand zieht den Eingang im Ruhezustand auf LOW.

Beispiel Taster mit Pull-up:

```text
nicht gedrückt → HIGH
Taster gedrückt und mit GND verbunden → LOW
```

Die Logik ist dann **active LOW**.

## 5.3 ADC – Analog to Digital Converter

Der klassische ESP32 besitzt laut Kurs 18 ADC-fähige Eingänge mit nominell 12 Bit Auflösung.

Idealisierte Zuordnung aus dem Kurs:

- 0 V → Messwert `0`
- 3,3 V → Messwert `4095`

ADC-Gruppen:

- **ADC1:** auch bei aktivem WLAN verwendbar
- **ADC2:** beim klassischen ESP32 mit WLAN-Ressourcen geteilt; bei aktivem WLAN problematisch bzw. nicht verfügbar

Wichtig:

- niemals mehr als die erlaubte Pinspannung anlegen
- ADC-Werte können rauschen
- ESP32-ADC ist nicht perfekt linear
- bei genauer Messung kalibrieren und mehrere Werte mitteln
- konkrete Eingangsbereiche hängen von ADC-Attenuation und Chipvariante ab

## 5.4 DAC – Digital to Analog Converter

Beim klassischen ESP32:

- `GPIO25` = `DAC1`
- `GPIO26` = `DAC2`
- 8 Bit, Werte `0` bis `255`

Ein DAC erzeugt eine echte abgestufte Ausgangsspannung. Er kann beispielsweise einfache Audiosignale oder Sollspannungen liefern, aber nur mit begrenzter Ausgangsleistung.

## 5.5 PWM – Pulse Width Modulation

PWM schaltet einen digitalen Ausgang sehr schnell zwischen LOW und HIGH. Über das Verhältnis von Ein-Zeit zu Gesamtperiode entsteht eine mittlere Wirkung.

**Duty Cycle / Tastgrad:**

- 0 % → immer LOW
- 50 % → gleich lange HIGH und LOW
- 100 % → immer HIGH

Anwendungen:

- LED dimmen
- Motordrehzahl steuern
- Servoimpulse erzeugen
- mit Filter eine analogähnliche Spannung erzeugen

PWM ist **kein echter DAC**: Das Signal bleibt digital und pulsiert.

Beim ESP32 sind die reinen Eingangspins `GPIO34` bis `GPIO39` nicht als PWM-Ausgänge verwendbar.

## 5.6 Interrupt

Ein Interrupt reagiert auf ein Ereignis am Eingang, ohne dass die Hauptschleife den Pin ständig abfragen muss.

Mögliche Auslöser:

- steigende Flanke (`RISING`)
- fallende Flanke (`FALLING`)
- jede Änderung (`CHANGE`)
- bestimmter Pegel

Vorteil: schnelle und effiziente Reaktion.

Regeln für Interrupt Service Routines:

- möglichst kurz halten
- keine langen Wartezeiten
- keine aufwendige Netzwerkkommunikation
- meist nur Flag setzen oder Zähler erhöhen
- gemeinsam verwendete Daten korrekt schützen
- mechanische Taster entprellen

## 5.7 Touch-Pins

Der klassische ESP32 besitzt kapazitive Touch-Eingänge. Sie erkennen Änderungen der elektrischen Kapazität, zum Beispiel durch einen Finger.

Einsatz:

- Touch-Taste
- berührungsloser Näherungseffekt
- Wake-up aus Deep Sleep

Fehlerquellen:

- Feuchtigkeit
- lange Leitungen
- elektrische Störungen
- falscher Schwellwert
- unbeabsichtigtes Auslösen

## 5.8 Hall-Sensor

Der klassische ESP32 enthält laut Unterlagen einen Hall-Sensor zur Erkennung von Magnetfeldern.

Mögliche Anwendungen:

- Tür-/Fensterposition
- Umdrehungszähler
- magnetischer Näherungssensor

> Nicht jede neuere ESP32-Variante besitzt dieselben internen Sensoren. Immer das Datenblatt der Variante prüfen.

---

<a id="kapitel-6"></a>
## 6. I²C-Bus

**I²C** steht für **Inter-Integrated Circuit** und wurde von Philips entwickelt.

I²C verwendet zwei Signalleitungen:

- `SDA` – Serial Data
- `SCL` – Serial Clock

Zusätzlich benötigt die Schaltung:

- gemeinsame Masse `GND`
- passende Versorgung `VCC`

## 6.1 Eigenschaften

- synchron: `SCL` liefert den Takt
- adressierter Bus
- mehrere Geräte teilen `SDA` und `SCL`
- typische Geschwindigkeiten: 100 kbit/s und 400 kbit/s
- High-Speed-Mode laut Unterlagen bis 3,4 Mbit/s
- 7-Bit-Adressierung: theoretisch 128 Kombinationen, aber einige sind reserviert; häufig werden 112 nutzbare Adressen genannt
- Bosch-Sensoren verwenden häufig `0x76` oder `0x77`

## 6.2 Pull-up-Widerstände

`SDA` und `SCL` sind Open-Drain/Open-Collector-Leitungen. Teilnehmer ziehen die Leitung aktiv auf LOW; HIGH entsteht über Pull-up-Widerstände.

Typischer Kurswert:

- ungefähr 4,7 kΩ bis 10 kΩ nach `VCC`

> **Wichtige Korrektur zur Folie:** I²C benötigt elektrisch Pull-ups. Interne Pull-ups können vorhanden sein, sind aber oft zu schwach. Viele Breakout-Boards besitzen bereits externe Pull-ups. Zu viele parallele Pull-ups können den Gesamtwiderstand wiederum zu klein machen.

## 6.3 Ablauf einer Übertragung

1. Bus ist frei: `SDA` und `SCL` HIGH.
2. Controller erzeugt **START**.
3. Controller sendet die 7-Bit-Adresse und das Read/Write-Bit.
4. Adressierter Teilnehmer bestätigt mit **ACK**.
5. Datenbytes werden übertragen.
6. Empfänger bestätigt Bytes mit ACK oder beendet mit NACK.
7. Controller erzeugt **STOP**.

## 6.4 Typische Fehler

- `SDA` und `SCL` vertauscht
- kein gemeinsames `GND`
- Pull-ups fehlen
- falsche I²C-Adresse
- zwei Geräte besitzen dieselbe feste Adresse
- 5-V-Pull-up an einem 3,3-V-ESP
- zu lange Kabel oder zu hohe Taktrate
- Bibliothek verwendet andere Pins als die Verdrahtung
- Sensor bekommt falsche Versorgungsspannung

**Test:** Mit einem I²C-Scanner alle antwortenden Adressen anzeigen. Erwartet wird beispielsweise `0x76` oder `0x77`; die tatsächlich beobachtete Adresse muss aus dem Scan stammen.

---

<a id="kapitel-7"></a>
## 7. SPI-Bus

**SPI** steht für **Serial Peripheral Interface** und wurde von Motorola entwickelt.

Signale:

- `SCLK`/`SCK` – Serial Clock vom Controller
- `MOSI` – Master Out, Slave In
- `MISO` – Master In, Slave Out
- `CS`/`SS` – Chip Select bzw. Slave Select

> Im Kurs steht teilweise `SCL`; bei SPI ist die übliche Bezeichnung **`SCLK` oder `SCK`**. `SCL` wird normalerweise für I²C verwendet.

## 7.1 Eigenschaften

- synchron
- **Full Duplex** durch getrennte Leitungen `MOSI` und `MISO`
- laut Kurs bis ungefähr 20 Mbit/s; das reale Maximum hängt von Controller, Slave, Leitung und Signalqualität ab
- kein Start-/Stop-Bit wie bei UART
- ein Controller/Master im vereinfachten Kursmodell
- jeder Slave benötigt normalerweise eine eigene `CS`-Leitung
- keine Busadresse wie bei I²C

## 7.2 Ablauf

1. Controller setzt `CS` des gewünschten Slaves auf LOW.
2. Controller erzeugt den Takt auf `SCLK`.
3. Controller sendet Bits über `MOSI`.
4. Slave kann gleichzeitig Bits über `MISO` zurücksenden.
5. Controller setzt `CS` wieder auf HIGH.

Typische Anwendungen:

- Display
- SD-/Flash-Speicher
- schnelle ADCs
- Sensoren mit hoher Datenrate

## 7.3 Typische Fehler

- `MOSI` und `MISO` vertauscht
- falsche `CS`-Leitung
- falscher SPI-Modus (`CPOL`/`CPHA`)
- zu hohe Clock-Frequenz
- Slave bleibt wegen falscher Logik dauerhaft ausgewählt
- mehrere Slaves treiben gleichzeitig `MISO`
- lange Leitungen verursachen Signalreflexionen/Störungen
- kein gemeinsames `GND`

---

<a id="kapitel-8"></a>
## 8. UART

**UART** steht für **Universal Asynchronous Receiver/Transmitter**.

Der ESP verwendet UART unter anderem zum:

- Flashen von Programmen über einen USB-UART-Wandler
- Ausgeben serieller Debug-Nachrichten
- Kommunizieren mit GPS-, GSM- oder anderen seriellen Modulen

## 8.1 Leitungen

Für die eigentliche Datenkommunikation:

- `TX` – Transmit
- `RX` – Receive
- gemeinsames `GND`

Verdrahtung:

```text
Gerät A TX → Gerät B RX
Gerät A RX ← Gerät B TX
GND        ↔ GND
```

`TX` wird also **gekreuzt** mit `RX`. Eine Versorgungsleitung kann zusätzlich nötig sein, zählt aber nicht zum UART-Datensignal selbst.

## 8.2 Asynchron

UART besitzt keine separate Clock-Leitung. Beide Seiten müssen dieselben Parameter verwenden:

- Baudrate, z. B. `9600` oder `115200`
- Datenbits
- Parität
- Stopbits

Typische Schreibweise:

- `8N1` = 8 Datenbits, No parity, 1 Stopbit

Ein UART-Frame enthält typischerweise:

1. Startbit
2. Datenbits
3. optionales Paritätsbit
4. Stopbit(s)

## 8.3 Kommunikationsrichtungen

- **Simplex:** nur eine Richtung
- **Half Duplex:** beide Richtungen, aber nicht gleichzeitig
- **Full Duplex:** beide Richtungen gleichzeitig; mit getrennten `TX`- und `RX`-Leitungen möglich

## 8.4 Typische Fehler

- `TX` mit `TX` statt mit `RX` verbunden
- gemeinsames `GND` fehlt
- unterschiedliche Baudraten
- unterschiedliche Frame-Einstellungen
- 5-V-UART direkt an 3,3-V-GPIO
- falscher COM-Port
- UART-Pins werden gleichzeitig von anderer Hardware benutzt
- serielle Bootmeldungen werden mit Sensordaten verwechselt

---
