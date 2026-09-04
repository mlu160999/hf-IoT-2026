# Eigenes ESPHome-Beispiel: 4-Bit-Binärzähler mit LEDs

## Projektidee

In diesem eigenen Beispiel verwende ich vier LEDs des ACEBOTT-Boards als **4-Bit-Binäranzeige**. Der aktuelle Zählerwert wird gleichzeitig als Dezimalzahl auf dem TM1637-Display und in der ESPHome-Weboberfläche angezeigt.

Der Zähler kann auf zwei Arten bedient werden:

- **Taster K1:** Wert um 1 erhöhen
- **Taster K2:** Wert um 1 vermindern
- **Webbutton `Binary Counter +1`:** Wert um 1 erhöhen
- **Webbutton `Binary Counter -1`:** Wert um 1 vermindern
- **Webbutton `Binary Counter Reset`:** Wert auf 0 zurücksetzen

Der Wertebereich reicht von **0 bis 15**. Nach 15 springt der Zähler wieder auf 0. Beim Herunterzählen springt er nach 0 auf 15.

[Demo-Video ansehen](MicrosoftTeams-video.mp4)

---

## Verwendete Hardware

- ACEBOTT ESP32-MAX-V1.0
- vier LEDs auf dem Lernboard
- Taster K1 und K2
- 4-stelliges TM1637-Display
- USB-C-Datenkabel
- Computer mit ESPHome

---

## GPIO-Belegung

| Komponente | GPIO | Funktion |
|---|---:|---|
| LED1 | GPIO25 | Bitwert 1 = $2^0$ |
| LED2 | GPIO14 | Bitwert 2 = $2^1$ |
| LED3 | GPIO13 | Bitwert 4 = $2^2$ |
| LED4 | GPIO12 | Bitwert 8 = $2^3$ |
| Taster K1 | GPIO23 | Zähler um 1 erhöhen |
| Taster K2 | GPIO19 | Zähler um 1 vermindern |
| TM1637 `CLK` | GPIO18 | Taktsignal des Displays |
| TM1637 `DIO` | GPIO17 | Datenleitung des Displays |

---

## Funktionsprinzip

Vier LEDs können $2^4 = 16$ verschiedene Zustände darstellen. Deshalb kann der Zähler die Werte **0 bis 15** anzeigen.

Die Binärzahl wird in der Reihenfolge `LED4 LED3 LED2 LED1` gelesen:

```text
LED4  LED3  LED2  LED1
  8     4     2     1
```

Eine leuchtende LED entspricht `1`, eine ausgeschaltete LED entspricht `0`.

Beispiele:

```text
Dezimal 0 = Binär 0000 → keine LED leuchtet
Dezimal 1 = Binär 0001 → LED1 leuchtet
Dezimal 2 = Binär 0010 → LED2 leuchtet
Dezimal 3 = Binär 0011 → LED1 und LED2 leuchten
Dezimal 4 = Binär 0100 → LED3 leuchtet
Dezimal 15 = Binär 1111 → alle vier LEDs leuchten
```

---

## Binärtabelle

| Dezimal | Binär | LED4 (8) | LED3 (4) | LED2 (2) | LED1 (1) |
|---:|:---:|:---:|:---:|:---:|:---:|
| 0 | 0000 | 0 | 0 | 0 | 0 |
| 1 | 0001 | 0 | 0 | 0 | 1 |
| 2 | 0010 | 0 | 0 | 1 | 0 |
| 3 | 0011 | 0 | 0 | 1 | 1 |
| 4 | 0100 | 0 | 1 | 0 | 0 |
| 5 | 0101 | 0 | 1 | 0 | 1 |
| 6 | 0110 | 0 | 1 | 1 | 0 |
| 7 | 0111 | 0 | 1 | 1 | 1 |
| 8 | 1000 | 1 | 0 | 0 | 0 |
| 9 | 1001 | 1 | 0 | 0 | 1 |
| 10 | 1010 | 1 | 0 | 1 | 0 |
| 11 | 1011 | 1 | 0 | 1 | 1 |
| 12 | 1100 | 1 | 1 | 0 | 0 |
| 13 | 1101 | 1 | 1 | 0 | 1 |
| 14 | 1110 | 1 | 1 | 1 | 0 |
| 15 | 1111 | 1 | 1 | 1 | 1 |

---

## 1. Zählervariable in `main.yaml`

Der aktuelle Wert wird in der globalen Ganzzahlvariable `binary_counter` gespeichert.

```yaml
globals:
  - id: binary_counter
    type: int
    restore_value: no
    initial_value: '0'
```

Bedeutung:

- `type: int` definiert eine Ganzzahl.
- `initial_value: '0'` setzt den Startwert auf 0.
- `restore_value: no` bedeutet, dass nach einem Neustart wieder bei 0 begonnen wird.

---

## 2. Binärwert auf den LEDs ausgeben

Das Skript `update_binary_leds` prüft die vier Bits des Zählerwerts mit dem bitweisen AND-Operator `&`.

```yaml
script:
  - id: update_binary_leds
    then:
      - if:
          condition:
            lambda: 'return id(binary_counter) & 1;'
          then:
            - switch.turn_on: LED1
          else:
            - switch.turn_off: LED1

      - if:
          condition:
            lambda: 'return id(binary_counter) & 2;'
          then:
            - switch.turn_on: LED2
          else:
            - switch.turn_off: LED2

      - if:
          condition:
            lambda: 'return id(binary_counter) & 4;'
          then:
            - switch.turn_on: LED3
          else:
            - switch.turn_off: LED3

      - if:
          condition:
            lambda: 'return id(binary_counter) & 8;'
          then:
            - switch.turn_on: LED4
          else:
            - switch.turn_off: LED4
```

Beispiel für den Wert `3`:

```text
3 & 1 → wahr → LED1 ein
3 & 2 → wahr → LED2 ein
3 & 4 → falsch → LED3 aus
3 & 8 → falsch → LED4 aus

Ergebnis: 0011
```

---

## 3. Bedienung über die ESPHome-Weboberfläche

### Wert erhöhen

```yaml
button:
  - platform: template
    name: "Binary Counter +1"
    on_press:
      then:
        - lambda: |-
            id(binary_counter)++;

            if (id(binary_counter) > 15) {
              id(binary_counter) = 0;
            }

            ESP_LOGI(
              "binary_counter",
              "Current counter value: %d",
              id(binary_counter)
            );

        - script.execute: update_binary_leds
```

### Wert vermindern

```yaml
  - platform: template
    name: "Binary Counter -1"
    on_press:
      then:
        - lambda: |-
            id(binary_counter)--;

            if (id(binary_counter) < 0) {
              id(binary_counter) = 15;
            }

            ESP_LOGI(
              "binary_counter",
              "Current counter value: %d",
              id(binary_counter)
            );

        - script.execute: update_binary_leds
```

### Zähler zurücksetzen

```yaml
  - platform: template
    name: "Binary Counter Reset"
    on_press:
      then:
        - lambda: |-
            id(binary_counter) = 0;

            ESP_LOGI(
              "binary_counter",
              "Counter reset to: %d",
              id(binary_counter)
            );

        - script.execute: update_binary_leds
```

---

## 4. Bedienung mit den physischen Tastern

Die Eingänge sind als `INPUT_PULLUP` und `inverted: true` konfiguriert. Im Ruhezustand ist der Eingang HIGH. Beim Drücken wird er gegen GND gezogen und von ESPHome als aktiv erkannt.

```yaml
binary_sensor:
  - platform: gpio
    id: ButtonK1
    name: "Button K1"
    pin:
      number: GPIO23
      mode: INPUT_PULLUP
      inverted: true
    on_press:
      then:
        - lambda: |-
            id(binary_counter)++;

            if (id(binary_counter) > 15) {
              id(binary_counter) = 0;
            }

            ESP_LOGI(
              "binary_counter",
              "Current counter value: %d",
              id(binary_counter)
            );

        - script.execute: update_binary_leds

  - platform: gpio
    id: ButtonK2
    name: "Button K2"
    pin:
      number: GPIO19
      mode: INPUT_PULLUP
      inverted: true
    on_press:
      then:
        - lambda: |-
            id(binary_counter)--;

            if (id(binary_counter) < 0) {
              id(binary_counter) = 15;
            }

            ESP_LOGI(
              "binary_counter",
              "Current counter value: %d",
              id(binary_counter)
            );

        - script.execute: update_binary_leds
```

---

## 5. GPIO-Ausgänge in `led.yaml`

```yaml
switch:
  - platform: gpio
    id: LED1
    name: "LED1"
    icon: "mdi:led-on"
    pin: GPIO25

  - platform: gpio
    id: LED2
    name: "LED2"
    icon: "mdi:led-on"
    pin: GPIO14

  - platform: gpio
    id: LED3
    name: "LED3"
    icon: "mdi:led-on"
    pin: GPIO13

  - platform: gpio
    id: LED4
    name: "LED4"
    icon: "mdi:led-on"
    pin: GPIO12

  - platform: restart
    name: "Restart"
```

---

## 6. Dezimalwert auf dem TM1637-Display

Das Display zeigt den Zählerwert **dezimal** an. Die LEDs zeigen denselben Wert **binär**.

```yaml
display:
  - platform: tm1637
    id: tm1637_display
    clk_pin: GPIO18
    dio_pin: GPIO17
    length: 4
    update_interval: 500ms
    intensity: 7

    lambda: |-
      it.printf(
        0,
        "%4d",
        id(binary_counter)
      );
```

---

## 7. Zählerwert in der Weboberfläche

Der Template-Textsensor aktualisiert die Anzeige jede Sekunde.

```yaml
text_sensor:
  - platform: template
    name: "Binary Counter Value"
    id: binary_counter_display
    update_interval: 1s

    lambda: |-
      char buffer[20];

      snprintf(
        buffer,
        sizeof(buffer),
        "%d",
        id(binary_counter)
      );

      return {buffer};
```

---

## Anpassungen gegenüber dem Temperaturbeispiel

Folgende Packages wurden für den Binärzähler deaktiviert:

```yaml
# dht: !include dht.yaml
# startup: !include startup.yaml
# rtttl: !include buzzer.yaml
# fastled: !include fastled.yaml
```

Gründe:

- `dht.yaml` steuerte dieselben LEDs anhand der Temperatur und würde die Binärdarstellung überschreiben.
- `buzzer.yaml` verwendete K2, der beim Binärzähler zum Herunterzählen benötigt wird.
- `startup.yaml` war für den Zählerversuch nicht notwendig.
- `fastled.yaml` war für dieses Beispiel nicht notwendig.

Aktiv bleiben die für das Beispiel wichtigen Packages:

```yaml
packages:
  button_inputs: !include button.yaml
  led: !include led.yaml
  tm1637: !include tm1637.yaml
  wifi_signal: !include wifi_signal.yaml
```

---

## Kompilieren und auf den ESP32 übertragen

```powershell
py -m esphome run .\main.yaml
```

ESPHome führt dabei folgende Schritte aus:

1. YAML-Konfiguration validieren
2. Firmware kompilieren
3. Firmware über USB oder OTA übertragen
4. serielle beziehungsweise drahtlose Logs anzeigen

Nach dem Start ist die Weboberfläche erreichbar unter:

```text
http://maddox.local
```

---

## Beobachtetes Testergebnis

Im Testvideo wurden folgende Zustände tatsächlich gezeigt:

| Dezimalwert | Binär | Beobachteter LED-Zustand |
|---:|:---:|---|
| 0 | `0000` | keine Zähler-LED aktiv |
| 1 | `0001` | LED1 aktiv |
| 2 | `0010` | LED2 aktiv |
| 3 | `0011` | LED1 und LED2 aktiv |
| 4 | `0100` | LED3 aktiv |

Zusätzlich waren im Video sichtbar:

- die lokale Weboberfläche unter `maddox.local`
- der Wert `Binary Counter Value`
- die laufenden ESPHome-Logs
- der Dezimalwert auf dem TM1637-Display
- die passenden binären LED-Zustände
- die Rückkehr des Zählers auf 0

Die Übergänge `15 → 0` und `0 → 15` sind im YAML-Code implementiert, wurden im Video jedoch nicht vollständig bis zum Grenzwert demonstriert.

---

## Datenfluss

```text
K1, K2 oder Webbutton
          ↓
binary_counter verändern
          ↓
update_binary_leds ausführen
          ├──→ LED1 bis LED4: Binärdarstellung
          ├──→ TM1637: Dezimaldarstellung
          ├──→ Weboberfläche: aktueller Wert
          └──→ ESPHome-Log: Current counter value
```

---

## Mögliche Fehler

- Falsche GPIO-Zuordnung führt zu einer falschen Bitreihenfolge.
- Wenn `dht.yaml` aktiv bleibt, kann die Temperaturautomatik die LEDs überschreiben.
- Wenn `buzzer.yaml` aktiv bleibt, besitzt K2 zwei unterschiedliche Funktionen.
- Einzelnes manuelles Schalten der LED-Switches in der Weboberfläche kann die Binäranzeige vorübergehend verfälschen. Beim nächsten Zählschritt setzt `update_binary_leds` wieder den korrekten Zustand.
- Wegen `restore_value: no` beginnt der Zähler nach einem Neustart wieder bei 0.
- Ohne gemeinsamen GND oder mit falscher Versorgung funktionieren Taster, LEDs oder Display nicht zuverlässig.

---

## Fazit

Der Versuch zeigt, wie eine Dezimalzahl mit vier digitalen Ausgängen binär dargestellt werden kann. Dazu wurden eine globale Variable, physische GPIO-Eingänge, Template-Buttons, ein ESPHome-Skript, bitweise Operatoren, GPIO-Schalter, Logmeldungen und ein TM1637-Display kombiniert.

Der Zähler funktioniert lokal auf dem ESP32 und kann sowohl direkt am ACEBOTT-Board als auch über die ESPHome-Weboberfläche bedient werden. Das Display zeigt den Dezimalwert, während die LEDs denselben Wert als vierstellige Binärzahl darstellen.

---

## Prüfungsnotizen

- Vier Bits ergeben $2^4 = 16$ Zustände: `0` bis `15`.
- `LED1 = 1`, `LED2 = 2`, `LED3 = 4`, `LED4 = 8`.
- Der bitweise Operator `&` prüft, ob ein bestimmtes Bit gesetzt ist.
- K1 erhöht und K2 vermindert den Zähler.
- Der Dezimalwert wird auf dem TM1637 angezeigt.
- Die vier LEDs zeigen denselben Wert binär.
- `restore_value: no` setzt den Zähler nach einem Neustart wieder auf 0.
- Implementierter Wrap-around: `15 → 0` und `0 → 15`.
