# WLED Configuration

## Device

- Controller: ESP32-WROOM-32
- WLED version used: 16.0.1
- LED count: 32
- Data GPIO: 18
- LED type: WS281x
- Color order: GRB
- Start LED: 0
- Reverse: OFF
- Skip first: 0
- Gamma correction: ON

## Network

The ESP32 and laptop must be connected to the same local Wi-Fi/hotspot network so HyperHDR can send LED data to WLED.

## Power

Keep WLED current limiting enabled while using USB power or other limited supplies. For high brightness, use a regulated 5V supply with sufficient current capacity.
