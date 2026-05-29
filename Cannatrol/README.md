# Cannatrol BLE Protocol

Reverse-engineered Bluetooth Low Energy (BLE) / GATT protocol for the **Cannatrol** curing unit, decoded from an Android `btsnoop_hci.log` capture. Groundwork for a future local Home Assistant integration.

📄 **Full protocol writeup:** [Cannatrol_BLE_Protocol.md](Cannatrol_BLE_Protocol.md)

## What's documented

- **Device identification** — advertises as `CANN`; custom GATT service `9ecadc24-0ee5-a9e0-93f3-a3b50100406e`
- **Characteristic pair** — write (phone → device) + notify (device → phone), text-based commands terminated with `\n`
- **Authentication** — challenge/response handshake: `REQUEST_CHALLENGE` → `CHALLENGE:<16-char>` → `RESPONSE:<58-hex>` → `AUTH_OK`. The trailing 15 bytes of the response are identical across sessions (likely a static device key), suggesting an HMAC-style construction over the challenge.
- **Post-auth commands** — WiFi status check + saved SSID retrieval
- **Full HCI connection sequence** — scan → connect → MTU negotiation → service/characteristic discovery → enable notifications → auth

## Status

⚠️ **Partial.** The capture only exercised WiFi-status checks, so the auth flow and WiFi commands are *confirmed*, but the control commands (temp/humidity reads, dry/cure cycle start/stop, setpoints) are *inferred* — listed as "likely exist," pending a capture that drives those features.

## Goal

Decode enough of the protocol to build a **local-only** Home Assistant integration (ESPHome BLE client or a custom component) that reads chamber temp/humidity and controls dry/cure cycles — no cloud dependency.

## Contributing captures

To fill in the missing commands: capture a `btsnoop_hci.log` on Android (Developer Options → enable Bluetooth HCI snoop log) while using the Cannatrol app to start a cycle or change setpoints, then decode the writes to handle `0x0013` and the notifications from handle `0x0010`.
