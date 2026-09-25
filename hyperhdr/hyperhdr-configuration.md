# HyperHDR Configuration

## Screen Capture

| Setting | Value |
|---|---|
| Capture source | System Capture |
| USB Capture | Disabled |
| Device | Automatic |
| Maximum stream width | 512 px |
| FPS | 30 |
| Hardware acceleration | ON |
| HDR → SDR | OFF |

## WLED Output

| Setting | Value |
|---|---|
| Controller | WLED |
| Target | ESP32/WLED device |
| RGB byte order | RGB |
| Refresh time | 20 ms |
| Override WLED brightness | OFF |
| Restore lights' original state | OFF |
| Max retries | 60 |

## Smoothing

| Setting | Value |
|---|---|
| Activate | ON |
| Interpolator | Hybrid / Infinite Interpolator |
| Time | 80 ms |
| Update frequency | 50 Hz |
| Smoothing factor | 0 |
| Stiffness | 150 |
| Damping | 26 |
| Anti-flicker | ON |
| Continuous output | ON |

## Processing

- Black-border / letterbox detection: enabled.
- Custom LED layout: 32 LEDs.
- The layout is stored in `led-layout.json`.
