# Beispiel 2 -- Start- und Laufzeitanzeige

## Ziel

Der ESP32 zeigt nach einem Neustart seine Laufzeit über vier LEDs an.\
Je länger der ESP32 läuft, desto weiter wechselt die aktive LED.

  Laufzeit        Anzeige
  --------------- ---------
  0--3 Sekunden   LED1
  3--6 Sekunden   LED2
  6--9 Sekunden   LED3
  ab 9 Sekunden   LED4

Der Test wird über **Restart** in der ESPHome-Weboberfläche gestartet.

## Änderungen

### `main.yaml`

Im Abschnitt `esphome` wurde `on_boot` ergänzt. Beim Start wird LED1
eingeschaltet und LED2--LED4 werden ausgeschaltet.

``` yaml
esphome:
  name: ${name}
  friendly_name: ${name}

  on_boot:
    priority: -100
    then:
      - switch.turn_on: LED1
      - switch.turn_off: LED2
      - switch.turn_off: LED3
      - switch.turn_off: LED4
```

Zusätzlich wird `startup.yaml` eingebunden:

``` yaml
startup: !include startup.yaml
```

Andere Funktionen, die ebenfalls die LEDs steuern, wurden für dieses
Beispiel deaktiviert, damit keine Konflikte entstehen.

### `startup.yaml`

Der `uptime`-Sensor misst die Laufzeit des ESP32. Bei 3, 6 und 9
Sekunden wird jeweils auf die nächste LED gewechselt.

``` yaml
sensor:
  - platform: uptime
    id: uptime_sensor
    name: "Uptime Sensor"
    update_interval: 1s

    on_value_range:
      - above: 3
        below: 4
        then:
          - switch.turn_off: LED1
          - switch.turn_on: LED2
          - switch.turn_off: LED3
          - switch.turn_off: LED4

      - above: 6
        below: 7
        then:
          - switch.turn_off: LED1
          - switch.turn_off: LED2
          - switch.turn_on: LED3
          - switch.turn_off: LED4

      - above: 9
        below: 10
        then:
          - switch.turn_off: LED1
          - switch.turn_off: LED2
          - switch.turn_off: LED3
          - switch.turn_on: LED4
```

## Test

1.  ESPHome-Weboberfläche über `maddox.local` öffnen.
2.  **Restart** ausführen.
3.  LED-Sequenz beobachten: **LED1 → LED2 → LED3 → LED4**.
4.  Der **Uptime Sensor** in der Weboberfläche zeigt gleichzeitig die
    aktuelle Laufzeit an.

## Ergebnis

Der ESP32 wertet seine Laufzeit automatisch aus und steuert abhängig
davon verschiedene Ausgänge. Das Beispiel demonstriert die Verwendung
eines internen Sensors, zeitabhängiger Bedingungen und automatischer
Aktionen in ESPHome.
