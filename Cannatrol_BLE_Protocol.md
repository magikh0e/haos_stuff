# Cannatrol BLE Protocol — Reverse Engineered from HCI Log
By: magikh0e

**Date:** February 2026
**Source:** btsnoop_hci.log captured from Pixel 10 Pro XL
**Device:** Cannatrol unit named "TropicalRootsMaui"
**Device MAC:** 

---

## Overview

The Cannatrol unit communicates over Bluetooth Low Energy (BLE) using a custom GATT service with text-based commands. 
The protocol follows a simple request/response pattern over a single characteristic pair (one for writes, one for notifications).

---

## Device Identification

### BLE Advertisement
- **Advertising name:** `CANN`
- **MAC Address:** `x:x:x:x:x:x` (public address)
- **Advertisement payload:** 53 bytes, broadcasts continuously even when not connected
- **RSSI range observed:** -81 to -87 dBm

### GATT Service Discovery

After connection, the phone discovers the following service structure:

| Handle | Type | UUID / Value |
|--------|------|-------------|
| 0x0001–0x0005 | GAP Service | Standard (includes device name) |
| 0x0006–0x000D | GATT Service | Standard |
| 0x000E | Custom Service | `9ecadc24-0ee5-a9e0-93f3-a3b50100406e` |
| 0x000F–0x0010 | Characteristic | UUID `9ecadc24-0ee5-a9e0-93f3-a3b50300406e` (NOTIFY — device→phone) |
| 0x0011 | CCCD | Notification descriptor |
| 0x0012–0x0013 | Characteristic | UUID `9ecadc24-0ee5-a9e0-93f3-a3b50200406e` (WRITE — phone→device) |

**Custom Service Base UUID:** `9ecadc24-0ee5-a9e0-93f3-a3b50X00406e`
- `0100` = Service declaration
- `0200` = Write characteristic (phone → Cannatrol)
- `0300` = Notify characteristic (Cannatrol → phone)

### Device Name (GATT Read)
- **Handle 0x0003:** Returns `TropicalRootsMaui` (user-assigned device name)

---

## Authentication Flow

The Cannatrol uses a challenge-response authentication mechanism. Both sessions in the capture follow the identical sequence:

### Step 1: Phone requests challenge
```
WRITE → Handle 0x0013: REQUEST_CHALLENGE\n
```
Note: The command is terminated with a newline (`0x0a`).

### Step 2: Device returns random challenge
```
NOTIFY ← Handle 0x0010: CHALLENGE:<16-char-alphanumeric>
```
**Session 1:** `CHALLENGE:blahlbah`
**Session 2:** `blehbleh`

The challenge is a 16-character random alphanumeric string, different each session.

### Step 3: Phone sends computed response
```
WRITE → Handle 0x0013: RESPONSE:<58-char-hex>
```
**Session 1:** `RESPONSE:d4f410a830a00c94fc982149d57184a07870c05c1818b844c8512d08fd29`
**Session 2:** `RESPONSE:d5b0f4d810e0ac5c6884e8899989f8607870c05c1818b844c8512d08fd29`

### Response Analysis
```
Session 1: d4f410a830a00c94fc982149d57184a0 | 7870c05c1818b844c8512d08fd29
Session 2: d5b0f4d810e0ac5c6884e8899989f860 | 7870c05c1818b844c8512d08fd29
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
           Variable (challenge-dependent)      Static (device key / secret)
```

**Key observations:**
- Response is 58 hex characters = 29 bytes
- Last 15 bytes (`7870c05c1818b844c8512d08fd29`) are identical across sessions — likely derived from a static device secret or pairing key
- First 14 bytes vary with each challenge
- Likely hash: `HMAC-SHA256(device_secret, challenge)` truncated, or a similar construction

### Step 4: Device confirms
```
NOTIFY ← Handle 0x0010: AUTH_OK
```

---

## Post-Authentication Commands

After `AUTH_OK`, the app immediately checks WiFi status:

### WiFi Status Check
```
WRITE → Handle 0x0013: IS_WIFI_CONNECTED
NOTIFY ← Handle 0x0010: WIFI_CONNECTED: TRUE
```

### WiFi Credential Retrieval
```
WRITE → Handle 0x0013: GET_WIFI_CREDENTIALS
NOTIFY ← Handle 0x0010: WIFI_SSID:cDc
```

**Note:** Only the SSID is returned, not the password. The WiFi password is likely stored on-device but not exposed over BLE.

---

## Known Command Summary

| Command (Phone → Device) | Response (Device → Phone) | Purpose |
|--------------------------|--------------------------|---------|
| `REQUEST_CHALLENGE\n` | `CHALLENGE:<16-char>` | Initiate auth |
| `RESPONSE:<58-hex>` | `AUTH_OK` | Complete auth |
| `IS_WIFI_CONNECTED` | `WIFI_CONNECTED: TRUE/FALSE` | Check WiFi state |
| `GET_WIFI_CREDENTIALS` | `WIFI_SSID:<name>` | Get saved SSID |

---

## Commands NOT Observed (Likely Exist)

Based on Cannatrol app functionality, these commands likely exist but were not captured because the app only performed WiFi checks in this session:

| Probable Command | Expected Response | Purpose |
|-----------------|-------------------|---------|
| `GET_TEMP` | Temperature reading | Current chamber temp |
| `GET_HUMIDITY` | Humidity reading | Current chamber humidity |
| `GET_STATUS` or `GET_STATE` | Cycle state | Dry/Cure/Hold/Idle |
| `START_DRY` | Acknowledgment | Begin dry cycle |
| `START_CURE` | Acknowledgment | Begin cure cycle |
| `STOP` | Acknowledgment | Stop current cycle |
| `SET_TEMP:<value>` | Acknowledgment | Set target temp |
| `SET_HUMIDITY:<value>` | Acknowledgment | Set target humidity |
| `SET_WIFI_CREDENTIALS` | Acknowledgment | Configure WiFi |
| `GET_FIRMWARE_VERSION` | Version string | Firmware info |
| `GET_SERIAL` | Serial number | Device serial |

---

## Connection Sequence (Full HCI Detail)

1. **Stop scanning** → HCI LE Set Scan Enable (disable)
2. **Set scan parameters** → HCI LE Set Scan Parameters
3. **Start scanning** → HCI LE Set Scan Enable (enable)
4. **Receive CANN advertisement** → HCI LE Advertising Report (MAC `9425357D7850`)
5. **Stop scanning** → HCI LE Set Scan Enable (disable)
6. **Initiate connection** → HCI LE Create Connection to `9425357D7850`
7. **Connection complete** → HCI LE Connection Complete (handle 0x0041)
8. **Set MTU** → Exchange MTU Request (proposed 517 bytes, negotiated 244)
9. **Service discovery** → ATT Read By Group Type (primary services)
10. **Characteristic discovery** → ATT Read By Type (characteristics per service)
11. **Enable notifications** → ATT Write Request on CCCD (handle 0x0011, value 0x0001)
12. **Read device name** → ATT Read Request on handle 0x0003 → `TropicalRootsMaui`
13. **Begin auth flow** → Write `REQUEST_CHALLENGE` to handle 0x0013

---
