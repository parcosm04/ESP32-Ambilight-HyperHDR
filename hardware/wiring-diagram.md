# Wiring Diagram

## Connections

```text
ESP32 GPIO18 ──────► Ring 1 DI
Ring 1 DO    ──────► Ring 2 DI

5V ────────────────► Ring 1 5V
5V ────────────────► Ring 2 5V

GND ───────────────► Ring 1 GND
GND ───────────────► Ring 2 GND
```

## Data Chain

```text
ESP32
  │
  ▼
Ring 1
LED 1 → LED 2 → ... → LED 16
                         │
                         ▼
Ring 2
LED 17 → LED 18 → ... → LED 32
```

> Replace this file with the actual `wiring-diagram.png` when the physical wiring diagram is uploaded.
