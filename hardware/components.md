# Hardware Components

## Core Components

| Component | Specification | Quantity |
|---|---|---:|
| Microcontroller | ESP32-WROOM-32 | 1 |
| LED Ring | WS2812B, 16 LEDs | 2 |
| Host | Windows laptop | 1 |
| Power | 5V regulated supply | 1 |
| Jumper wires | As required | — |

## LED Configuration

- Total LEDs: 32
- Ring 1: LEDs 1–16
- Ring 2: LEDs 17–32
- LED type: WS2812B
- Data GPIO: ESP32 GPIO18
- WLED color order: GRB

## Power

At a theoretical 60 mA per LED:

`32 × 60 mA ≈ 1.92 A`

Actual current depends on brightness, color, LED model, and current limiting. For sustained high brightness, use an appropriately rated regulated 5V supply and common ground.
