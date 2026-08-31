# IoT-Hardware – Lernnotizen

> Ziel: Die vielen Einzelinformationen als zusammenhängenden Weg verstehen:  
> **physikalische Umwelt → Sensor → Mikrocontroller/Firmware → Bus/WLAN → Protokoll → Server/Broker → Speicherung/UI → Aktor**

---

# Inhaltsverzeichnis

1. [Was ist ein IoT-Element?](#kapitel-1)
2. [Sensoren, Aktoren und Gateway](#kapitel-2)
3. [Umgebungssensoren aus dem Kurs](#kapitel-3)
4. [ESP8266](#kapitel-4)
5. [ESP32](#kapitel-5)
6. [GPIO](#kapitel-6)
7. [I²C-Bus](#kapitel-7)
8. [SPI-Bus](#kapitel-8)
9. [UART](#kapitel-9)
10. [Vergleich I²C, SPI und UART](#kapitel-10)
11. [Controllerboards sinnvoll auswählen](#kapitel-11)
12. [Stromversorgung und Zuverlässigkeit](#kapitel-12)
13. [Netzwerk, Protokolle und kompletter Datenfluss](#kapitel-13)
14. [IoT-Sicherheit](#kapitel-14)
15. [Praktischer Testplan](#kapitel-15)
16. [Häufige Prüfungsfallen](#kapitel-16)
17. [Exam Notes / Prüfungsnotizen](#exam-notes)

---

<a id="kapitel-1"></a>
## 1. Was ist ein IoT-Element?

Ein **IoT-Element** ist ein physisches Gerät, das Daten aus seiner Umgebung erfasst oder etwas in der Umgebung bewirkt und über ein Netzwerk kommunizieren kann.

Typische Bestandteile:

1. **Sensor (Input)** – misst eine physikalische Grösse.
2. **Mikrocontroller/Gateway** – liest Messwerte, verarbeitet sie und stellt die Netzwerkverbindung her.
3. **Firmware** – das Programm auf dem Mikrocontroller.
4. **Kommunikationsweg** – z. B. GPIO, I²C, SPI oder UART innerhalb des Geräts und WLAN ausserhalb des Geräts.
5. **Netzwerkprotokoll** – z. B. TCP/IP, MQTT, HTTP oder NTP.
6. **Server/Broker/Cloud/Edge-Service** – empfängt und verarbeitet Daten.
7. **Speicherung und Benutzeroberfläche** – z. B. Datenbank und Home-Assistant-Dashboard.
8. **Aktor (Output)** – führt eine physische Aktion aus.

### Beispiel: Temperaturüberwachung

```text
Temperatur
   ↓
BME280-Sensor
   ↓  I²C
ESP32 + Firmware
   ↓  WLAN / TCP-IP / MQTT
MQTT-Broker oder Home Assistant
   ↓
Datenbank + Dashboard + Regel
   ↓  MQTT
ESP32
   ↓  GPIO
LED, Lüfter oder Relais
```

**Erfolgreiches Ergebnis:** Der Sensorwert erscheint nachvollziehbar im Dashboard. Wird ein definierter Grenzwert überschritten, erhält der Aktor einen Steuerbefehl und sein Zustand ist beobachtbar.

---

<a id="kapitel-2"></a>
## 2. Sensoren, Aktoren und Gateway

## 2.1 Sensor

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

## 2.2 Aktor

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

## 2.3 Gateway/Mikrocontroller

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

<a id="kapitel-3"></a>
## 3. Umgebungssensoren aus dem Kurs

## 3.1 BMP280

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

## 3.2 BME280

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

## 3.3 BME680

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

<a id="kapitel-4"></a>
## 4. ESP8266

Der ESP8266 ist ein günstiger 32-Bit-Mikrocontroller von Espressif mit integriertem 2,4-GHz-WLAN. Er eignet sich für kleinere IoT-Aufgaben.

## 4.1 ESP-01

Eigenschaften:

- sehr klein
- nur zwei allgemein gut nutzbare GPIO-Pins
- UART vorhanden
- geeignet für einfache Aufgaben, z. B. einen Sensor oder ein Relais
- zum komfortablen Flashen meist USB-Programmer nötig

## 4.2 ESP-12 / Wemos D1 mini / NodeMCU

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

## 4.3 ESP8266 und Deep Sleep

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

## 4.4 Wichtige ESP8266-Pins

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

<a id="kapitel-5"></a>
## 5. ESP32

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

## 5.1 ESP32 gegenüber ESP8266

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

## 5.2 Wichtige Sicherheit

- ESP32-GPIO arbeitet mit **3,3-V-Logik**.
- Ein GPIO darf nicht einfach mit 5 V belastet werden.
- ESP32-Pins gelten im Allgemeinen **nicht als 5-V-tolerant**.
- Für 5-V-Signale sind Spannungsteiler oder Pegelwandler nötig.
- Stromintensive Verbraucher wie Motoren, Relais, starke LEDs oder Kameras dürfen nicht direkt aus einem GPIO versorgt werden.
- Externe Aktoren benötigen Treiberstufe, Transistor/MOSFET und bei induktiven Lasten eine Freilaufdiode.
- Alle verbundenen Schaltungsteile benötigen normalerweise ein gemeinsames `GND`.

---

<a id="kapitel-6"></a>
## 6. GPIO

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

## 6.1 Digital und analog

### Digital

Ein digitales Signal kennt diskrete logische Zustände:

- LOW / `0` / `False`
- HIGH / `1` / `True`

Ein digitaler Eingang misst nicht den genauen Spannungswert, sondern ordnet ihn einem Logikzustand zu. Zwischen garantiertem LOW und HIGH kann ein undefinierter Bereich liegen.

### Analog

Ein analoges Signal kann innerhalb eines Bereichs viele kontinuierliche Spannungswerte annehmen. Ein ADC übersetzt die Spannung in eine Zahl.

## 6.2 Pull-up und Pull-down

Ein unbeschalteter Eingang kann **floaten** und zufällige Werte liefern.

- **Pull-up:** Widerstand zieht den Eingang im Ruhezustand auf HIGH.
- **Pull-down:** Widerstand zieht den Eingang im Ruhezustand auf LOW.

Beispiel Taster mit Pull-up:

```text
nicht gedrückt → HIGH
Taster gedrückt und mit GND verbunden → LOW
```

Die Logik ist dann **active LOW**.

## 6.3 ADC – Analog to Digital Converter

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

## 6.4 DAC – Digital to Analog Converter

Beim klassischen ESP32:

- `GPIO25` = `DAC1`
- `GPIO26` = `DAC2`
- 8 Bit, Werte `0` bis `255`

Ein DAC erzeugt eine echte abgestufte Ausgangsspannung. Er kann beispielsweise einfache Audiosignale oder Sollspannungen liefern, aber nur mit begrenzter Ausgangsleistung.

## 6.5 PWM – Pulse Width Modulation

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

## 6.6 Interrupt

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

## 6.7 Touch-Pins

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

## 6.8 Hall-Sensor

Der klassische ESP32 enthält laut Unterlagen einen Hall-Sensor zur Erkennung von Magnetfeldern.

Mögliche Anwendungen:

- Tür-/Fensterposition
- Umdrehungszähler
- magnetischer Näherungssensor

> Nicht jede neuere ESP32-Variante besitzt dieselben internen Sensoren. Immer das Datenblatt der Variante prüfen.

---

<a id="kapitel-7"></a>
## 7. I²C-Bus

**I²C** steht für **Inter-Integrated Circuit** und wurde von Philips entwickelt.

I²C verwendet zwei Signalleitungen:

- `SDA` – Serial Data
- `SCL` – Serial Clock

Zusätzlich benötigt die Schaltung:

- gemeinsame Masse `GND`
- passende Versorgung `VCC`

## 7.1 Eigenschaften

- synchron: `SCL` liefert den Takt
- adressierter Bus
- mehrere Geräte teilen `SDA` und `SCL`
- typische Geschwindigkeiten: 100 kbit/s und 400 kbit/s
- High-Speed-Mode laut Unterlagen bis 3,4 Mbit/s
- 7-Bit-Adressierung: theoretisch 128 Kombinationen, aber einige sind reserviert; häufig werden 112 nutzbare Adressen genannt
- Bosch-Sensoren verwenden häufig `0x76` oder `0x77`

## 7.2 Pull-up-Widerstände

`SDA` und `SCL` sind Open-Drain/Open-Collector-Leitungen. Teilnehmer ziehen die Leitung aktiv auf LOW; HIGH entsteht über Pull-up-Widerstände.

Typischer Kurswert:

- ungefähr 4,7 kΩ bis 10 kΩ nach `VCC`

> **Wichtige Korrektur zur Folie:** I²C benötigt elektrisch Pull-ups. Interne Pull-ups können vorhanden sein, sind aber oft zu schwach. Viele Breakout-Boards besitzen bereits externe Pull-ups. Zu viele parallele Pull-ups können den Gesamtwiderstand wiederum zu klein machen.

## 7.3 Ablauf einer Übertragung

1. Bus ist frei: `SDA` und `SCL` HIGH.
2. Controller erzeugt **START**.
3. Controller sendet die 7-Bit-Adresse und das Read/Write-Bit.
4. Adressierter Teilnehmer bestätigt mit **ACK**.
5. Datenbytes werden übertragen.
6. Empfänger bestätigt Bytes mit ACK oder beendet mit NACK.
7. Controller erzeugt **STOP**.

## 7.4 Typische Fehler

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

<a id="kapitel-8"></a>
## 8. SPI-Bus

**SPI** steht für **Serial Peripheral Interface** und wurde von Motorola entwickelt.

Signale:

- `SCLK`/`SCK` – Serial Clock vom Controller
- `MOSI` – Master Out, Slave In
- `MISO` – Master In, Slave Out
- `CS`/`SS` – Chip Select bzw. Slave Select

> Im Kurs steht teilweise `SCL`; bei SPI ist die übliche Bezeichnung **`SCLK` oder `SCK`**. `SCL` wird normalerweise für I²C verwendet.

## 8.1 Eigenschaften

- synchron
- **Full Duplex** durch getrennte Leitungen `MOSI` und `MISO`
- laut Kurs bis ungefähr 20 Mbit/s; das reale Maximum hängt von Controller, Slave, Leitung und Signalqualität ab
- kein Start-/Stop-Bit wie bei UART
- ein Controller/Master im vereinfachten Kursmodell
- jeder Slave benötigt normalerweise eine eigene `CS`-Leitung
- keine Busadresse wie bei I²C

## 8.2 Ablauf

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

## 8.3 Typische Fehler

- `MOSI` und `MISO` vertauscht
- falsche `CS`-Leitung
- falscher SPI-Modus (`CPOL`/`CPHA`)
- zu hohe Clock-Frequenz
- Slave bleibt wegen falscher Logik dauerhaft ausgewählt
- mehrere Slaves treiben gleichzeitig `MISO`
- lange Leitungen verursachen Signalreflexionen/Störungen
- kein gemeinsames `GND`

---

<a id="kapitel-9"></a>
## 9. UART

**UART** steht für **Universal Asynchronous Receiver/Transmitter**.

Der ESP verwendet UART unter anderem zum:

- Flashen von Programmen über einen USB-UART-Wandler
- Ausgeben serieller Debug-Nachrichten
- Kommunizieren mit GPS-, GSM- oder anderen seriellen Modulen

## 9.1 Leitungen

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

## 9.2 Asynchron

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

## 9.3 Kommunikationsrichtungen

- **Simplex:** nur eine Richtung
- **Half Duplex:** beide Richtungen, aber nicht gleichzeitig
- **Full Duplex:** beide Richtungen gleichzeitig; mit getrennten `TX`- und `RX`-Leitungen möglich

## 9.4 Typische Fehler

- `TX` mit `TX` statt mit `RX` verbunden
- gemeinsames `GND` fehlt
- unterschiedliche Baudraten
- unterschiedliche Frame-Einstellungen
- 5-V-UART direkt an 3,3-V-GPIO
- falscher COM-Port
- UART-Pins werden gleichzeitig von anderer Hardware benutzt
- serielle Bootmeldungen werden mit Sensordaten verwechselt

---

<a id="kapitel-10"></a>
## 10. Vergleich I²C, SPI und UART

| Merkmal | I²C | SPI | UART |
|---|---|---|---|
| Bedeutung | Inter-Integrated Circuit | Serial Peripheral Interface | Universal Asynchronous Receiver/Transmitter |
| Synchron? | ja | ja | nein |
| Clock | `SCL` | `SCLK`/`SCK` | keine separate Clock |
| Datenleitungen | eine bidirektionale `SDA` | `MOSI` und `MISO` | `TX` und `RX` |
| Auswahl Gerät | Adresse | eigene `CS`-Leitung | direkte Punkt-zu-Punkt-Verbindung |
| Standard-Signalleitungen | 2 | 4 für einen Slave | 2 plus gemeinsames GND |
| Duplex | nicht gleichzeitig auf einer SDA-Leitung | Full Duplex | Full Duplex mit TX und RX möglich |
| Geschwindigkeit | eher mittel | hoch | abhängig von Baudrate |
| Mehrere Geräte | einfach über Adressen | zusätzliche CS-Pins nötig | normalerweise Punkt zu Punkt |
| Pull-ups | für SDA/SCL erforderlich | normalerweise nicht | normalerweise nicht |
| Typischer Einsatz | viele langsame Sensoren | Display/SD/schnelle Sensoren | Debugging, Flashen, serielle Module |

### Entscheidung

- Viele Sensoren, wenige Leitungen → **I²C**
- Hohe Geschwindigkeit → **SPI**
- Einfache direkte serielle Verbindung → **UART**

---

<a id="kapitel-11"></a>
## 11. Controllerboards sinnvoll auswählen

## 11.1 Auswahlkriterien

1. Brauche ich WLAN?
2. Brauche ich Bluetooth/BLE?
3. Wie viele GPIO-, ADC-, DAC- oder Touch-Pins brauche ich?
4. Welche Busse braucht das Projekt: I²C, SPI, UART, I²S?
5. Wie gross darf das Board sein?
6. Läuft es über Netzteil oder Akku?
7. Wie hoch ist der Verbrauch im Deep Sleep des **gesamten Boards**?
8. Brauche ich Kamera, Mikrofon, Lautsprecher oder SD-Karte?
9. Gibt es USB und einen USB-UART-Wandler?
10. Ist ein externer Programmer nötig?
11. Brauche ich eine externe Antenne?
12. Unterstützt die Firmware, z. B. ESPHome, das Board korrekt?
13. Sind Pinbelegung und Spannungspegel dokumentiert?

## 11.2 Genannte Boards und sinnvoller Einsatz

### ESP32 NodeMCU

- universelles ESP32-Board
- viele GPIOs
- WLAN und Bluetooth
- für Sensoren, Displays und Steueraufgaben
- beim Kauf auf konkrete Pinzahl und Pinout achten

### Wemos D1 mini ESP32

- kompakt
- viele Schnittstellen
- teilweise mit D1-mini-Shields kompatibel
- praktisch bei wenig Platz
- Funkreichweite kann je nach Bauform geringer sein

### Lolin32

- ESP32 mit Anschluss und Ladeelektronik für 3,7-V-Lithiumakku
- gut für mobile Projekte
- Zusatzhardware auf dem Board kann trotz Deep Sleep Strom verbrauchen

### ESP32-CAM

- Kamera, SD-Karten-Slot und helle LED
- geeignet für Türspion, Drucker- oder Tierbeobachtung
- hoher Spitzenstrom, laut Artikel bis ungefähr 700 mA
- stabile Stromversorgung nötig
- lange/dünne USB-Kabel können Spannungsabfall und Abstürze verursachen
- Kamera bedeutet zusätzlich Datenschutz- und Sicherheitsverantwortung

### ESP-WROOM-32 als Minimalmodul

- sehr klein
- wenige Zusatzverbraucher
- alle benötigten Funktionen müssen selbst beschaltet werden
- externer Programmer und stabile 3,3-V-Versorgung nötig
- geeignet, wenn Platz und Energieverbrauch wichtiger als Komfort sind

### M5 Atom Echo

- ESP32, Mikrofon, Lautsprecher, Verstärker, RGB-LED und Taster
- geeignet für Sprachprojekte/Home Assistant
- Datenschutz bei Mikrofon beachten

### TTGO T-Higrow

- Bodenfeuchtesensor mit ESP32
- kapazitive Messung reduziert Elektrodenkorrosion
- Gehäuse-/Feuchtigkeitsschutz ist kritisch
- vollständig luftdichte Gehäuse können Temperatur-/Feuchtemessungen verfälschen

### ESP8266 NodeMCU

- universelles, preiswertes WLAN-Board
- kein Bluetooth
- weniger RAM/Leistung als ESP32
- für viele einfache IoT-Projekte ausreichend
- Pinout verschiedener Varianten prüfen

### Wemos D1 mini ESP8266

- kompakt, gut dokumentiert, viele Shields
- beliebt für kleine Sensor-/Relaisprojekte
- Stromversorgung eines Shield-Stapels nicht überlasten

### ESP-01

- nur wenige Pins
- gut für genau eine einfache Aufgabe
- Programmer meist erforderlich

### ESP8266-12F

- sehr kleines Modul
- viele Signale als Lötpads
- kein komfortables USB/Development-Board
- Variantenqualität und Antennenausführung beachten

### ESP IR TR

- ESP8285 mit IR-Sender und IR-Empfänger
- kann Fernbedienungscodes lernen und Geräte per Infrarot steuern
- laut Artikel 5-V-Versorgung für ausreichende IR-Leistung

### ESP32-H2

- im Artikel für Matter/Thread-Erfahrungen erwähnt
- wichtig: ESP32-H2 besitzt kein klassisches WLAN; Einsatzkonzept und Gateway/Border Router prüfen

### Arduino Nano RP2040 Connect

- RP2040 plus WLAN/Bluetooth und zusätzliche Sensorik
- mehr Rechenleistung für bestimmte Aufgaben
- höherer Preis als einfache ESP-Boards

### Raspberry Pi Pico W

- RP2040 mit WLAN
- viele GPIOs
- gut für rechenintensivere lokale Aufgaben
- Versorgung laut Artikel in einem relativ breiten Bereich möglich
- konkrete Funk- und Bluetooth-Funktionen hängen von Firmware/Supportstand ab

---

<a id="kapitel-12"></a>
## 12. Stromversorgung und Zuverlässigkeit

Viele scheinbare Softwarefehler sind Stromversorgungsfehler.

## 12.1 Typische Ursachen

- Netzteil liefert zu wenig Spitzenstrom
- USB-Kabel ist zu lang oder zu dünn
- Spannung fällt beim WLAN-Senden ab
- Kamera, Display, Relais oder Motor erzeugt Lastspitzen
- fehlende Abblockkondensatoren
- Verbraucher wird direkt über GPIO gespeist
- 3,3-V- und 5-V-Pegel werden verwechselt
- Akku-Ladeelektronik passt nicht zum Akku

## 12.2 Verbesserung

- stabiles Netzteil mit Reserve verwenden
- kurze, ausreichend dicke Leitungen
- Kondensatoren nahe am Board/Verbraucher
- Aktoren separat versorgen, aber Masse verbinden
- Brownout-/Reset-Meldungen im seriellen Log prüfen
- Stromaufnahme in Active- und Sleep-Modus tatsächlich messen
- Sensoren nur bei Bedarf einschalten
- Messung und Sendung bündeln

**Beobachtbare Verifikation:** Das Gerät läuft auch während WLAN-Senden, Kameraaufnahme oder Relais-Schalten stabil; keine unerwarteten Resets im seriellen Log.

---

<a id="kapitel-13"></a>
## 13. Netzwerk, Protokolle und kompletter Datenfluss

Die internen Hardwarebusse enden am Mikrocontroller. Danach folgt die Netzwerkkommunikation.

## 13.1 WLAN und TCP/IP

Der ESP verbindet sich mit einem Access Point und erhält typischerweise per DHCP:

- IP-Adresse
- Subnetzmaske
- Default Gateway
- DNS-Server

TCP/IP transportiert darauf höhere Protokolle.

## 13.2 MQTT

MQTT arbeitet nach Publish/Subscribe:

- Gerät veröffentlicht Daten mit `publish` auf einem Topic.
- MQTT-Broker verteilt sie an alle `subscriber`.
- Ein Server oder anderes Gerät kann Steuerbefehle auf ein Command-Topic publizieren.

Beispiel:

```text
home/livingroom/bme280/temperature   → 22.4
home/livingroom/fan/command          → ON
home/livingroom/fan/state            → ON
```

Ein Payload sollte Einheit, Datentyp und Format eindeutig machen. Möglich ist JSON:

```json
{
  "temperature_c": 22.4,
  "humidity_percent": 47.1,
  "pressure_hpa": 1008.6
}
```

Für zuverlässige Systeme beachten:

- eindeutige Topics
- QoS passend auswählen
- Retain nur bewusst verwenden
- Last Will and Testament für Offline-Erkennung
- Reconnect-Logik
- Zeitstempel
- Plausibilitätsprüfung
- Zustand (`state`) und Befehl (`command`) trennen

## 13.3 HTTP

Bei HTTP sendet der ESP Requests direkt an einen Webserver/API-Endpunkt. Das ist einfach für einzelne Abfragen, aber weniger entkoppelt als MQTT.

## 13.4 NTP

NTP synchronisiert die Uhr. Das ist wichtig für:

- korrekte Zeitstempel
- Zertifikatsprüfung bei TLS
- zeitabhängige Regeln
- geordnete Messreihen

## 13.5 Speicherung und UI

Der Broker speichert Daten normalerweise nicht automatisch als langfristige Messreihe. Eine Anwendung muss sie abonnieren und speichern, z. B.:

```text
ESP → MQTT-Broker → Home Assistant/Node-RED → Datenbank → Dashboard
```

---

<a id="kapitel-14"></a>
## 14. IoT-Sicherheit

## 14.1 Risiken

- Standardpasswörter
- WLAN- oder MQTT-Zugangsdaten im öffentlichen Repository
- unverschlüsseltes MQTT/HTTP
- offene Broker ohne Authentisierung
- unsichere OTA-Updates
- veraltete Firmware
- zu grosse Netzwerkrechte
- Kameras/Mikrofone ohne Datenschutzkonzept
- Aktoren können physische Schäden verursachen

## 14.2 Schutzmassnahmen

- pro Gerät eindeutige Zugangsdaten
- Geheimnisse nicht in Git committen
- TLS verwenden, wo möglich
- Broker-ACLs: Gerät darf nur benötigte Topics lesen/schreiben
- IoT-Geräte in eigenes VLAN/Netzsegment
- Firewall-Regeln nach Minimalprinzip
- signierte bzw. authentisierte Updates
- sichere Wiederverbindung ohne endlose schnelle Schleife
- Eingabedaten validieren
- lokale sichere Defaults bei Netzwerkausfall
- Debug-Schnittstellen und unnötige Dienste deaktivieren

---

<a id="kapitel-15"></a>
## 15. Praktischer Testplan

## 15.1 Hardware vor dem Einschalten

- Boardtyp und Pinout stimmen.
- Versorgungsspannung stimmt.
- Kein 5-V-Signal liegt direkt an einem 3,3-V-GPIO.
- `GND` ist gemeinsam.
- I²C-Pull-ups ziehen auf den richtigen Pegel.
- Aktoren besitzen eine Treiberstufe.
- Keine Boot-Strapping-Pins werden falsch belastet.

## 15.2 Firmware

- richtige Boarddefinition gewählt
- richtiger COM-Port
- serielles Log auf erwartete Baudrate eingestellt
- Sensorinitialisierung meldet Erfolg
- Fehler werden explizit ausgegeben
- keine echten Passwörter im Code/Repository

## 15.3 Bus

- I²C-Scanner findet die tatsächlich vorhandene Adresse
- SPI-Gerät antwortet mit passendem Modus und Takt
- UART-Empfang funktioniert bei gleicher Baudrate und gekreuztem TX/RX

## 15.4 Netzwerk

- ESP erhält eine IP-Adresse
- Gateway/Broker ist erreichbar
- MQTT-Client verbindet sich
- Test-Subscriber sieht das reale Topic und Payload
- Reconnect nach WLAN-Unterbruch funktioniert

## 15.5 End-to-End

- Sensorwert ändert sich bei einer kontrollierten Umweltänderung.
- Der neue reale Messwert erscheint im Broker/Server.
- Speicherung enthält einen plausiblen Zeitstempel.
- UI zeigt den Wert und die Einheit korrekt.
- Aktorbefehl bewirkt eine sichtbare Aktion.
- Rückmelde-Topic bestätigt den realen Aktorzustand.

> Erwartete Werte sind keine gemessenen Werte. Erst serielles Log, Bus-Scan, Broker-Nachricht oder sichtbare Aktorreaktion gelten als beobachteter Nachweis.

---

<a id="kapitel-16"></a>
## 16. Häufige Prüfungsfallen

1. **ESP32 ist 3,3-V-Logik, nicht automatisch 5-V-tolerant.**
2. **ADC** wandelt analog → digital; **DAC** digital → analog.
3. **PWM ist kein echter Analogausgang.**
4. I²C: `SDA` + `SCL`, Adressen und Pull-ups.
5. SPI: `MOSI`, `MISO`, `SCLK`, `CS`; schnell und Full Duplex.
6. UART: asynchron, TX wird mit RX gekreuzt, Baudrate muss stimmen.
7. ESP8266 besitzt WLAN, aber kein Bluetooth.
8. Klassischer ESP32 besitzt WLAN und Bluetooth.
9. `GPIO0`, `GPIO2` und `GPIO15` des ESP8266 beeinflussen den Bootvorgang.
10. ESP32-`ADC2` kollidiert beim klassischen ESP32 mit aktivem WLAN.
11. Sensor erzeugt Daten; Aktor erzeugt eine physische Wirkung.
12. Gateway/Controller verbindet lokale Hardware mit dem Netzwerk.
13. Ein MQTT-Broker verteilt Nachrichten; Langzeitspeicherung braucht zusätzlich eine Anwendung/Datenbank.
14. Deep Sleep spart nur dann viel Energie, wenn auch die restliche Board-Hardware wenig verbraucht.
15. Boarddaten, Pinout und Spannungsbereich immer für die konkrete Variante prüfen.

---

<a id="exam-notes"></a>
# Exam Notes / Prüfungsnotizen

## Unbedingt merken

- **IoT-Kette:** Sensor → Mikrocontroller → lokaler Bus → WLAN/TCP-IP → MQTT/HTTP → Server/Datenbank/UI → Aktor.
- **Sensor:** physikalische Grösse wird zu einem Signal.
- **Aktor:** elektrisches Signal wird zu einer physischen Wirkung.
- **GPIO:** frei programmierbarer digitaler Ein-/Ausgang; Sonderfunktionen sind pinabhängig.
- **ESP32/ESP8266 arbeiten mit 3,3-V-Logik.**
- **I²C:** 2 Signalleitungen, `SDA` und `SCL`, Geräteadressen, Pull-ups erforderlich.
- **SPI:** `MOSI`, `MISO`, `SCLK`, `CS`; synchron, schnell, Full Duplex.
- **UART:** `TX ↔ RX`, gemeinsames `GND`, asynchron, gleiche Baudrate.
- **ADC:** Spannung einlesen; beim 12-Bit-ADC nominell Werte `0…4095`.
- **DAC:** echte Ausgangsspannungsstufen; klassischer ESP32: `GPIO25`/`GPIO26`, 8 Bit.
- **PWM:** digitales Pulssignal; der Duty Cycle steuert die mittlere Wirkung.
- **Interrupt:** reagiert auf Ereignisse; ISR kurz halten.
- **ESP8266:** WLAN, kein Bluetooth, weniger Ressourcen.
- **klassischer ESP32:** WLAN + Bluetooth, mehr GPIO/ADC/Peripherie.
- **MQTT:** Publish/Subscribe über einen Broker; Topics und Payloads sauber definieren.
- **Verifikation:** Nie erfundene Messwerte verwenden – serielles Log, I²C-Scan, MQTT-Subscriber und sichtbare Aktorreaktion prüfen.
