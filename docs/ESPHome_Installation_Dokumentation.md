# ESPHome – Installation und Konfiguration

## Inhaltsverzeichnis

- [1. Ziel](#1-ziel)
- [2. Python und ESPHome installieren](#2-python-und-esphome-installieren)
- [3. ESPHome-Version überprüfen](#3-esphome-version-überprüfen)
- [4. Firmware kompilieren und hochladen](#4-firmware-kompilieren-und-hochladen)
- [5. Problem mit COM4](#5-problem-mit-com4)
- [6. CH341-Treiber installieren](#6-ch341-treiber-installieren)
- [7. USB-Port wechseln](#7-usb-port-wechseln)
- [8. Verbindung über COM6 überprüfen](#8-verbindung-über-com6-überprüfen)
- [9. Firmware über COM6 hochladen](#9-firmware-über-com6-hochladen)
- [10. Wrong Boot Mode](#10-wrong-boot-mode)
- [11. Möglicher Lösungsweg](#11-möglicher-lösungsweg)
- [12. Zusammenfassung](#12-zusammenfassung)

---

## 1. Ziel

Ziel war es, **ESPHome unter Windows einzurichten** und eine ESPHome-Konfiguration aus der Datei `main.yaml` auf ein **ACEBOTT ESP32 Max V1.0 Board** zu übertragen.

Während der Einrichtung traten mehrere Probleme mit der seriellen USB-Verbindung und dem Boot-Modus des ESP32 auf.

Diese Dokumentation beschreibt die durchgeführten Schritte sowie die aufgetretenen Fehler und Lösungsversuche.

---

## 2. Python und ESPHome installieren

Zuerst wurde **Python unter Windows installiert**.

Danach wurde PowerShell geöffnet und `pip` aktualisiert:

```powershell
py -m pip install --upgrade pip
```

Anschliessend wurde ESPHome installiert:

```powershell
py -m pip install esphome
```

ESPHome wird damit als Python-Modul installiert und kann anschliessend über `py -m esphome` ausgeführt werden.

---

## 3. ESPHome-Version überprüfen

Nach der Installation wurde überprüft, ob ESPHome korrekt installiert wurde:

```powershell
py -m esphome version
```

Wenn eine ESPHome-Version angezeigt wird, wurde ESPHome erfolgreich installiert.

---

## 4. Firmware kompilieren und hochladen

Die ESPHome-Konfiguration befindet sich in:

```text
main.yaml
```

Der erste Versuch, die Konfiguration zu kompilieren und auf den ESP32 zu übertragen, wurde mit folgendem Befehl durchgeführt:

```powershell
py -m esphome run .\main.yaml
```

ESPHome konnte die Konfiguration kompilieren, beim Upload trat jedoch ein Fehler mit dem seriellen Port auf.

---

## 5. Problem mit COM4

ESPHome erkannte zunächst den seriellen Port:

```text
COM4
```

Beim Upload erschien unter anderem die Fehlermeldung:

```text
Could not open COM4, the port is busy or doesn't exist.
```

Zusätzlich meldete ESPHome:

```text
WARNING Failed to upload to ['COM4']
```

Daraufhin wurde versucht, COM4 explizit als Upload-Port anzugeben:

```powershell
py -m esphome run .\main.yaml --device COM4
```

Der gleiche Fehler trat weiterhin auf.

Das Problem lag daher nicht an der automatischen Auswahl des Ports durch ESPHome.

---

## 6. CH341-Treiber installieren

Als möglicher Grund für das Problem mit dem seriellen Port wurde der USB-Serial-Treiber überprüft.

Für die Kommunikation wurde der **CH341-Treiber** installiert.

Die Installation erfolgte über Windows `pnputil`:

```powershell
pnputil /add-driver "D:\4_GIBB_CH\Sem IV\IoT\W1\ch341ser\CH341SER\CH341SER.INF" /install
```

`pnputil` ist ein Windows-Werkzeug zur Verwaltung und Installation von Gerätetreibern.

Der verwendete Treiber befand sich im Verzeichnis:

```text
ch341ser\CH341SER\
```

Die eigentliche Treiberdatei war:

```text
CH341SER.INF
```

---

## 7. USB-Port wechseln

Nachdem der Treiber installiert wurde, wurde der ESP32 an einen **anderen USB-Port des Computers** angeschlossen.

Dadurch änderte sich auch der von Windows verwendete COM-Port.

Der ESP32 wurde danach über:

```text
COM6
```

erkannt.

---

## 8. Verbindung über COM6 überprüfen

Zuerst wurden die aktuell verfügbaren seriellen Ports unter Windows überprüft.

```powershell
[System.IO.Ports.SerialPort]::GetPortNames()
```

Damit kann überprüft werden, welche COM-Ports Windows aktuell erkennt.

Anschliessend wurde getestet, ob Python COM6 öffnen kann:

```powershell
py -c "import serial; s=serial.Serial('COM6',115200,timeout=1); print('PORT WORKS'); s.close()"
```

Wenn der Port erfolgreich geöffnet werden kann, erscheint:

```text
PORT WORKS
```

Dieser Test ist hilfreich, um zwischen einem **Windows-/Treiberproblem** und einem **ESPHome-/ESP32-Problem** zu unterscheiden.

---

## 9. Firmware über COM6 hochladen

Nachdem COM6 erkannt wurde, wurde erneut versucht, die Firmware hochzuladen:

```powershell
py -m esphome run .\main.yaml --device COM6
```

Die Firmware wurde erfolgreich kompiliert.

ESPHome bzw. `esptool` konnte auch eine Verbindung zum ESP32 herstellen.

In der Ausgabe wurde das Gerät als:

```text
ESP32-D0WD-V3 (revision v3.1)
```

erkannt.

Der Flash-Speicher wurde vorbereitet und der Schreibvorgang gestartet.

Bei weiteren Upload-Versuchen trat jedoch erneut ein Problem mit der Kommunikation bzw. dem Boot-Modus auf.

---

## 10. Wrong Boot Mode

Bei einem weiteren Upload-Versuch erschien die Fehlermeldung:

```text
Wrong boot mode detected (0x13)!
The chip needs to be in download mode.
```

Damit war der COM-Port nicht mehr das eigentliche Hauptproblem.

Der ESP32 wurde über die serielle Verbindung erkannt, befand sich aber nicht im benötigten **Download Mode / Flash Mode**.

Der ESP32 muss sich für das Schreiben einer neuen Firmware im Download-Modus befinden.

Nach einem erfolgreichen Upload startete das Board und ESPHome zeigte Log-Ausgaben an.

Unter anderem war zu sehen:

```text
Running through setup()
```

Danach blieb die Ausgabe bei:

```text
Performing bus recovery
```

stehen.

Der Vorgang wurde mit:

```text
Ctrl + C
```

abgebrochen und erneut gestartet.

Das gleiche Problem trat erneut auf.

---

## 11. Möglicher Lösungsweg

Als nächster Lösungsversuch soll das Board manuell in den **Download Mode** versetzt werden.

Beim verwendeten **ACEBOTT ESP32 Max V1.0** kann dafür GPIO0 während eines Resets mit GND verbunden werden.

### Benötigt

- ACEBOTT ESP32 Max V1.0
- Jumper Wire
- GPIO0 / `00`
- GND-Pin

### Vorgehen

1. Das Board über USB mit dem Computer verbinden.
2. Einen **Jumper Wire** zwischen dem Pin `00 (GPIO0)` und einem `GND`-Pin verbinden.
3. Während GPIO0 mit GND verbunden ist, den **RST-/Reset-Taster** des Boards drücken und wieder loslassen.
4. Anschliessend den Upload erneut starten:

```powershell
py -m esphome run .\main.yaml --device COM6
```

5. Sobald ESPHome bzw. `esptool` erfolgreich beginnt, eine Verbindung mit dem ESP32 herzustellen und die Firmware zu übertragen, kann die Verbindung zwischen GPIO0 und GND wieder entfernt werden.

> **Hinweis:** Dieser Schritt wurde noch nicht erfolgreich durchgeführt und soll als nächster Lösungsversuch durchgeführt werden.

---

## 12. Zusammenfassung

| Schritt | Ergebnis |
|---|---|
| Python installieren | Erfolgreich |
| `pip` aktualisieren | Erfolgreich |
| ESPHome installieren | Erfolgreich |
| ESPHome-Version überprüfen | Erfolgreich |
| `main.yaml` kompilieren | Erfolgreich |
| Upload über COM4 | Fehlgeschlagen |
| CH341-Treiber installieren | Durchgeführt |
| USB-Port wechseln | Durchgeführt |
| COM6 erkennen | Erfolgreich |
| COM6 mit Python testen | Durchgeführt |
| ESP32 über ESPHome erkennen | Erfolgreich |
| Firmware kompilieren | Erfolgreich |
| Firmware übertragen | Teilweise erfolgreich |
| ESPHome Logs starten | Erfolgreich |
| `Performing bus recovery` | Problem besteht weiterhin |
| Manueller Download Mode | Noch zu testen |

### Wichtigste Erkenntnis

Die Fehlersuche wurde schrittweise durchgeführt:

```text
Python
   ↓
ESPHome
   ↓
main.yaml
   ↓
USB-Treiber
   ↓
COM-Port
   ↓
ESP32-Verbindung
   ↓
Boot-/Download-Modus
   ↓
Firmware Upload
   ↓
ESPHome Logs
```

Der ESP32 und ESPHome können grundsätzlich miteinander kommunizieren. Das verbleibende Problem betrifft den Boot-/Download-Modus beziehungsweise das Verhalten des Boards nach dem Start.

Die manuelle Verbindung von **GPIO0 mit GND während eines Resets** ist der nächste geplante Troubleshooting-Schritt.
