# ESP32 Bluetooth Proxy for Home Assistant

A no-code guide to flashing an ESP32 as a Home Assistant **Bluetooth proxy** — extends HA's BLE range across the house to reach sensors the host's built-in Bluetooth can't.

📄 **Full guide:** [esp32-bluetooth-proxy.md.txt](esp32-bluetooth-proxy.md.txt)

## What it covers

- **Flashing** — via the [ESPHome web flasher](https://web.esphome.io) (Chrome/Edge), no YAML or coding required (~10 min per proxy)
- **Adoption** — auto-discovery into HA, or manual ESPHome integration by IP
- **Verification** — confirming the proxy picks up nearby BLE devices (Govee, Xiaomi, Mijia, Inkbird, etc.)
- **Placement tips** — power, mounting height, WiFi/BLE spectrum separation, avoiding Faraday cages
- **Board recommendations** — ESP32-C3 Mini, ESP32-S3, M5Stack Atom Lite, and when each makes sense
- **Troubleshooting** — serial port / USB driver issues, discovery failures, VLAN/mDNS gotchas, rarely-broadcasting devices
- **Scaling** — running multiple proxies in parallel (one per floor / BLE-heavy room)

## Why it matters here

Several devices in this repo are BLE — the **Volcano Hybrid**, the **Cannatrol**, and various temp/humidity sensors. A proxy or two extends HA's reach so those devices stay connected from anywhere in the house, not just within range of the HA host.
