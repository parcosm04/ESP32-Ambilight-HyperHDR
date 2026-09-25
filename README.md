# 🎬 ESP32 Ambilight with HyperHDR

> A custom 32-LED real-time ambient lighting system for laptop displays using ESP32, WS2812B, WLED and HyperHDR.

![Project Status](https://img.shields.io/badge/Status-Working-success)
![Platform](https://img.shields.io/badge/Platform-ESP32-blue)
![LEDs](https://img.shields.io/badge/LEDs-32%20WS2812B-orange)
![WLED](https://img.shields.io/badge/Firmware-WLED-purple)
![HyperHDR](https://img.shields.io/badge/Software-HyperHDR-red)

## 📌 Overview

This project is a custom-built **Ambilight system for a laptop display**.

The system captures the laptop screen in real time, analyzes the displayed colors using **HyperHDR**, and sends the corresponding LED colors through **WLED and ESP32** to control 32 individually addressable WS2812B LEDs.

Unlike conventional Ambilight systems that use LED strips around the rectangular edges of a display, this project uses **two 16-LED circular rings** positioned side-by-side near the top of the laptop display.

A custom LED-to-screen mapping was created in HyperHDR so that each physical LED samples the appropriate region of the screen.

## ✨ Features

- 🎬 Real-time screen-synchronized ambient lighting
- 💡 32 individually addressable RGB LEDs
- 🔵 2 × 16-LED WS2812B circular rings
- ⚡ ESP32-based LED control
- 📡 Wi-Fi / UDP communication
- 🖥️ HyperHDR real-time screen capture
- 📐 Custom circular LED mapping
- 🎞️ Black-border / letterbox detection
- 🌊 Smooth color transitions
- 🛡️ Anti-flicker processing
- 🔧 WLED-based LED control

## 🧠 System Architecture

```text
┌───────────────────────┐
│     Laptop Display    │
│    Movie / Video      │
└───────────┬───────────┘
            │
            ▼
┌───────────────────────┐
│       HyperHDR        │
│                       │
│ • Screen Capture      │
│ • Color Analysis      │
│ • Border Detection    │
│ • LED Mapping         │
│ • Smoothing           │
└───────────┬───────────┘
            │
        Wi-Fi / UDP
            │
            ▼
┌───────────────────────┐
│         WLED          │
│    LED Controller     │
└───────────┬───────────┘
            │
            ▼
┌───────────────────────┐
│         ESP32         │
│       GPIO 18         │
└───────────┬───────────┘
            │ DATA
            ▼
     ┌─────────────┐
     │   Ring 1    │
     │  16 LEDs    │
     └──────┬──────┘
            │ DO → DI
            ▼
     ┌─────────────┐
     │   Ring 2    │
     │  16 LEDs    │
     └─────────────┘
```

## 🔧 Hardware

| Component | Specification | Quantity |
|---|---|---:|
| Microcontroller | ESP32-WROOM-32 | 1 |
| LED Ring | WS2812B, 16 LEDs | 2 |
| Power Supply | 5V | 1 |
| Host System | Laptop | 1 |
| Jumper Wires | — | As required |

## 🔌 Wiring

```text
ESP32 GPIO18 ──────► Ring 1 DI
Ring 1 DO    ──────► Ring 2 DI

5V ────────────────► Ring 1 5V
5V ────────────────► Ring 2 5V

GND ───────────────► Ring 1 GND
GND ───────────────► Ring 2 GND
```

### LED Data Chain

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

Therefore:

```text
LED 1–16   → Ring 1
LED 17–32  → Ring 2
```

Both physical rings are oriented clockwise.

## 📐 Physical Configuration

### Display

```text
Width  ≈ 34 cm
Height ≈ 19 cm
```

### LED Rings

```text
Ring diameter ≈ 6.5 cm
LEDs per ring  = 16
Total LEDs     = 32
```

The two rings are positioned side-by-side near the upper portion of the laptop display.

```text
┌────────────────────────────────────────┐
│                                        │
│       ◯                    ◯           │
│     Ring 1              Ring 2         │
│                                        │
│                                        │
│                                        │
└────────────────────────────────────────┘
```

## 🧩 Custom LED Mapping

The main technical challenge is mapping a **non-standard circular LED geometry** to a rectangular laptop display.

A conventional Ambilight system normally assumes LEDs positioned around the edges of the display.

This project instead uses two circular LED rings.

Therefore, HyperHDR uses a **custom 32-LED sampling layout**, with each LED assigned to a specific region of the screen.

The LED positions are represented using normalized horizontal and vertical coordinates.

```text
Horizontal:
0.0 ───────────────────────── 1.0
Left                          Right

Vertical:
0.0
│
│
│
1.0
Top                         Bottom
```

The final mapping can be stored in:

```text
hyperhdr/led-layout.json
```

## 🔄 Processing Pipeline

```text
Screen Content
      │
      ▼
System Screen Capture
      │
      ▼
Black-Border Detection
      │
      ▼
Custom 32-LED Geometry
      │
      ▼
Region Color Sampling
      │
      ▼
Color Processing
      │
      ▼
Smoothing / Interpolation
      │
      ▼
HyperHDR
      │
      ▼
Wi-Fi / UDP
      │
      ▼
WLED
      │
      ▼
ESP32
      │
      ▼
WS2812B Rings
```

## 💻 Software Stack

| Layer | Technology |
|---|---|
| Microcontroller | ESP32 |
| LED Firmware | WLED |
| Screen Processing | HyperHDR |
| LED Type | WS2812B |
| Network | Wi-Fi / UDP |
| Configuration | JSON |
| Host | Windows Laptop |

## ⚙️ WLED Configuration

```text
LED Type       : WS281x
LED Count      : 32
Data GPIO      : 18
Color Order    : GRB
Start LED      : 0
Reverse        : OFF
Skip First     : 0
Gamma          : ON
```

WLED acts as the network-controlled LED interface between HyperHDR and the ESP32.

## 🖥️ HyperHDR Configuration

### Screen Capture

```text
Capture Source      : System Screen Capture
USB Capture         : OFF
Maximum Width       : 512 px
Frame Rate          : 30 FPS
Hardware Accel.     : ON
HDR → SDR           : OFF
```

### WLED Output

```text
Controller          : WLED
RGB Byte Order      : RGB
Refresh Time        : 20 ms
Override Brightness : OFF
Continuous Output   : ON
Anti-Flicker        : ON
```

## 🌊 Smoothing

HyperHDR smoothing is used to reduce flickering and abrupt color transitions.

```text
Smoothing            : ON
Interpolator         : Hybrid Physics
Smoothing Time       : 80 ms
Update Frequency     : 50 Hz
Smoothing Factor     : 0
Stiffness            : 150
Damping              : 26
Anti-Flicker         : ON
Continuous Output    : ON
```

This provides a balance between response speed and smooth visual transitions.

## 🎞️ Black-Border Detection

Movies and videos can contain black bars when their aspect ratio differs from the laptop display.

Without detection, the black regions can influence the sampled LED colors.

HyperHDR's **black-border / letterbox detection** is therefore enabled to focus the color sampling on the active video region.

## 📡 Network Architecture

The laptop and ESP32 communicate through the same local Wi-Fi network.

```text
┌──────────────┐
│    Laptop    │
│  HyperHDR    │
└──────┬───────┘
       │
       │ Wi-Fi / UDP
       │
┌──────▼───────┐
│    ESP32     │
│     WLED     │
└──────┬───────┘
       │
       ▼
   WS2812B LEDs
```

Internet access is not required for the local LED communication once the system is configured.

## ⚡ Power Considerations

WS2812B LEDs can consume significant current at high brightness.

For 32 LEDs, using an approximate 60 mA maximum per LED:

```text
32 × 60 mA ≈ 1.92 A
```

Actual consumption depends on brightness, color, LED model and current limiting.

For a permanent installation:

- Use a regulated 5V power supply.
- Provide adequate current capacity.
- Maintain a common ground.
- Keep WLED current limiting enabled during development.
- Avoid sustained maximum brightness when using limited USB power.

## 🛠️ Setup

### 1. Flash WLED

Install WLED on the ESP32 and connect it to the local Wi-Fi network.

### 2. Configure LEDs

```text
LED Count : 32
GPIO      : 18
Type      : WS281x
Color     : GRB
```

### 3. Connect the Rings

```text
ESP32 GPIO18 → Ring 1 DI
Ring 1 DO    → Ring 2 DI
```

### 4. Configure HyperHDR

Select:

```text
System Screen Capture
```

Enable hardware acceleration.

### 5. Configure WLED Output

Select the ESP32/WLED device as the HyperHDR output.

### 6. Configure Custom LED Layout

Set:

```text
Total LEDs = 32
```

Use the custom circular LED mapping.

### 7. Enable Processing Features

Enable:

- Black-border detection
- Smoothing
- Anti-flicker
- Continuous output

### 8. Test

Play a video and verify that the LED colors follow the corresponding regions of the screen.

## 📊 Technical Specifications

| Parameter | Value |
|---|---|
| Microcontroller | ESP32-WROOM-32 |
| LED Type | WS2812B |
| Number of Rings | 2 |
| LEDs per Ring | 16 |
| Total LEDs | 32 |
| Data GPIO | GPIO 18 |
| WLED Color Order | GRB |
| HyperHDR Output | RGB |
| Capture FPS | 30 |
| Capture Width | 512 px |
| Smoothing | 80 ms |
| Update Frequency | 50 Hz |
| WLED Refresh | 20 ms |
| Hardware Acceleration | Enabled |
| Black-Border Detection | Enabled |
| Anti-Flicker | Enabled |

## 🧪 Engineering Challenges

### 1. Custom LED Geometry

Circular LED rings cannot be represented accurately using a standard rectangular Ambilight layout.

A custom mapping was therefore created in HyperHDR.

### 2. LED Data Ordering

The rings form one serial LED chain:

```text
LED 1–16  → Ring 1
LED 17–32 → Ring 2
```

### 3. Screen Aspect Ratio

Movie letterboxing can cause black regions to be sampled.

Black-border detection was incorporated to handle this.

### 4. Response vs Smoothness

Too much smoothing introduces latency, while too little smoothing can produce flicker.

The final configuration uses:

```text
80 ms smoothing
50 Hz update frequency
```

### 5. Power Management

A 32-LED WS2812B system can approach approximately 1.92 A under a theoretical full-load assumption, requiring appropriate power management.

## 🚀 Future Improvements

- Automatic LED geometry calibration
- Higher LED density
- Custom ESP32 firmware
- Audio-reactive lighting
- Mobile control interface
- Automatic Wi-Fi configuration
- Custom PCB
- 3D-printed LED mounting
- Dedicated power distribution
- Advanced color calibration

## 📁 Repository Structure

```text
ESP32-Ambilight-HyperHDR/
│
├── README.md
│
├── hardware/
│   ├── wiring-diagram.png
│   └── components.md
│
├── esp32/
│   └── wled-configuration.md
│
├── hyperhdr/
│   ├── led-layout.json
│   └── hyperhdr-configuration.md
│
├── images/
│   ├── hardware-setup.jpg
│   ├── led-rings.jpg
│   └── hyperhdr-layout.png
│
└── demo/
    └── demo-video.md
```

## 📸 Project Media

Recommended media:

- Final laptop Ambilight setup
- Close-up of both LED rings
- ESP32 and wiring
- HyperHDR custom LED layout
- Working movie demonstration

After uploading images:

```markdown
![Hardware Setup](images/hardware-setup.jpg)

![LED Rings](images/led-rings.jpg)

![HyperHDR Layout](images/hyperhdr-layout.png)
```

## 🎥 Demonstration

The demonstration should show:

1. Laptop playing a movie/video
2. HyperHDR processing the screen
3. Custom 32-LED layout
4. Physical LED rings responding to screen colors
5. Smooth color transitions

## 📌 Project Status

| Module | Status |
|---|---|
| ESP32 | ✅ Working |
| WS2812B Rings | ✅ Working |
| WLED | ✅ Working |
| HyperHDR | ✅ Working |
| Wi-Fi Communication | ✅ Working |
| Custom LED Mapping | ✅ Working |
| Black-Border Detection | ✅ Working |
| Smoothing | ✅ Working |
| Real-Time Synchronization | ✅ Working |

## 👨‍💻 Author

**Pankaj Pandit**

Electronics & Telecommunication Engineering  
PCCOE, Pune

### Interests

`Embedded Systems` · `IoT` · `Digital Electronics` · `VLSI` · `Signal Processing` · `Hardware Development`

## ⭐ Technologies

`ESP32` `WS2812B` `WLED` `HyperHDR` `Wi-Fi` `UDP` `Embedded Systems` `Signal Processing` `RGB LEDs`

---

<p align="center">
  <b>Built with ESP32 + WS2812B + WLED + HyperHDR ⚡</b>
</p>

<p align="center">
  🎬 Turn your screen into an immersive lighting experience.
</p>
