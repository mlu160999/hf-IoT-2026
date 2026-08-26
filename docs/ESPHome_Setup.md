# ESPHome – Installation und Einrichtung

## 1. Ziel

Ziel dieser Übung war es, **ESPHome unter Windows einzurichten**, den ESP über USB mit dem Computer zu verbinden und eine ESPHome-Konfiguration über eine YAML-Datei auf das Gerät zu übertragen.

---

## 2. Voraussetzungen

Für die Einrichtung wurden folgende Komponenten verwendet:

- Windows-PC
- Python
- ESPHome
- ESP-Mikrocontroller
- USB-Kabel
- USB-Treiber für den ESP
- Visual Studio Code
- YAML-Konfigurationsdatei

---

## 3. Python überprüfen

Zuerst wurde überprüft, ob Python auf dem System installiert ist.

```powershell
python --version
```

Alternativ:

```powershell
py --version
```

Der Installationspfad von Python kann mit folgendem Befehl überprüft werden:

```powershell
where python
```

---

## 4. ESPHome installieren

ESPHome wird über `pip` installiert.

```powershell
py -m pip install esphome
```

Danach kann überprüft werden, ob ESPHome korrekt installiert wurde:

```powershell
esphome version
```

---

## 5. Projektverzeichnis öffnen

In PowerShell wird zuerst in das Verzeichnis gewechselt, in dem sich die ESPHome-Konfiguration befindet.

Beispiel:

```powershell
cd D:\ESPHome
```

Mit folgendem Befehl kann der Inhalt des aktuellen Ordners angezeigt werden:

```powershell
dir
```

Die Datei

```text
main.yaml
```

sollte hier sichtbar sein.

---

## 6. ESPHome-Konfiguration

ESPHome verwendet YAML-Dateien zur Konfiguration des Mikrocontrollers.

Die verwendete Konfigurationsdatei lautet:

```text
main.yaml
```

Vor dem Upload kann die Konfiguration überprüft werden:

```powershell
esphome config .\main.yaml
```

---

## 7. ESP über USB verbinden

Der ESP wurde über USB mit dem Windows-PC verbunden.

Anschliessend wurde der **Geräte-Manager** geöffnet.

Unter:

**Anschlüsse (COM & LPT)**

sollte der ESP bzw. der USB-Serial-Adapter angezeigt werden.

Falls Windows das Gerät nicht korrekt erkennt, muss der entsprechende USB-Treiber installiert werden.

---

## 8. USB-Treiber installieren

Der benötigte Treiber wurde als ZIP-Datei heruntergeladen.

Die ZIP-Datei wurde entpackt und der entsprechende Windows-Treiber installiert.

Danach wurde im Geräte-Manager erneut kontrolliert, ob das Gerät korrekt erkannt wird.

> Der COM-Port ist wichtig, da ESPHome diesen für die Kommunikation mit dem Mikrocontroller verwendet.

---

## 9. Firmware kompilieren und übertragen

Nachdem ESPHome installiert und der ESP erkannt wurde, kann die Konfiguration kompiliert und übertragen werden.

```powershell
esphome run .\main.yaml
```

ESPHome führt dabei mehrere Schritte durch:

1. YAML-Konfiguration überprüfen
2. Firmware generieren
3. Firmware kompilieren
4. ESP-Gerät erkennen
5. Firmware auf den ESP übertragen
6. Log-Ausgabe starten

---

## 10. Aufgetretene Probleme

### Problem 1 – ESPHome-Befehl funktioniert nicht

Beim Ausführen von:

```powershell
esphome run .\main.yaml
```

konnte ESPHome zunächst nicht korrekt gestartet werden.

Zur Kontrolle wurden Python und ESPHome überprüft.

```powershell
python --version
py --version
where python
esphome version
```

---

### Problem 2 – Laufwerk C: war voll

Während der Installation von ESPHome traten mehrere Warnungen und Fehler auf.

Eine Ursache war, dass auf dem Laufwerk `C:` kaum noch freier Speicherplatz vorhanden war.

Python und `pip` verwenden unter Windows unter anderem Verzeichnisse innerhalb von:

```text
C:\Users\<Benutzer>\AppData
```

Dadurch konnte die Installation bzw. das Erstellen benötigter Dateien fehlschlagen.

Nicht benötigte Dateien wurden deshalb entfernt bzw. auf Laufwerk `D:` verschoben.

---

### Problem 3 – ESP wurde nicht korrekt erkannt

Im Windows-Geräte-Manager wurde überprüft, ob der angeschlossene ESP erkannt wird.

Da das Gerät zunächst nicht korrekt erkannt wurde, wurde der passende USB-Serial-Treiber installiert.

Danach wurde der Geräte-Manager erneut überprüft.

---

## 11. Wichtige Befehle

| Befehl | Funktion |
|---|---|
| `py --version` | Installierte Python-Version anzeigen |
| `py --version` | Python Launcher überprüfen |
| `where python` | Python-Installationspfad anzeigen |
| `py -m pip install esphome` | ESPHome installieren |
| `esphome version` | ESPHome-Version überprüfen |
| `dir` | Dateien im aktuellen Verzeichnis anzeigen |
| `cd <Pfad>` | Verzeichnis wechseln |
| `esphome config .\main.yaml` | YAML-Konfiguration überprüfen |
| `esphome run .\main.yaml` | Firmware kompilieren und auf den ESP übertragen |

---

## 12. Erkenntnisse

Bei der Einrichtung von ESPHome müssen mehrere Komponenten zusammenspielen:

**Windows → Python → ESPHome → YAML-Konfiguration → USB-Treiber → COM-Port → ESP**

Ein Fehler bei einer dieser Komponenten kann dazu führen, dass die Firmware nicht auf den ESP übertragen werden kann.

Besonders hilfreich bei der Fehlersuche waren:

- Überprüfung der Python-Installation
- Überprüfung der ESPHome-Installation
- Kontrolle des freien Speicherplatzes
- Kontrolle des Geräte-Managers
- Kontrolle des COM-Ports
- Überprüfung des USB-Treibers
- Lesen der Fehlermeldungen in PowerShell
