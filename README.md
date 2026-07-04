# haos_stuff

My Home Assistant OS setup — dashboards, automations, scripts, helper templates, voice control, custom blueprints, and a reverse-engineered BLE protocol. Most of it is retargetable to your own entities by swapping `entity_id`s; each section calls out its dependencies (HACS cards, integrations) up front.

Several of these have longer write-ups published at **[magikh0e.pl/pubHomeAutomation](https://magikh0e.pl/pubHomeAutomation/)** — linked inline where they exist.

## Contents

**Climate / grow**
- [AC Infinity — Grow-Tent Dashboard](#ac-infinity--grow-tent-dashboard)
- [Cannatrol — Reverse-Engineered BLE Protocol](#cannatrol--reverse-engineered-ble-protocol)
- [Volcano Hybrid — Scripts, Dashboards & Voice](#volcano-hybrid--scripts-dashboards--voice)

**Power**
- [EcoFlow Delta 3 Max Plus — Dashboard](#ecoflow-delta-3-max-plus--dashboard)

**3D printing**
- [Creality K2 Plus — Dashboard](#creality-k2-plus--dashboard)

**Media / voice**
- [Universal Media Player — Unified TV Control](#universal-media-player--unified-tv-control)
- [Dog TV](#dog-tv)
- [Morning Briefing](#morning-briefing)
- [Maui Snorkel Report](#maui-snorkel-report)

**Security**
- [DIY Alarm System](#diy-alarm-system)
- [ESP32 Bluetooth Proxy](#esp32-bluetooth-proxy)

**Other**
- [Medication Reminders (moved)](#medication-reminders-moved)
- [Misc Automations](#misc-automations)

---

## AC Infinity — Grow-Tent Dashboard
[Custom HAOS AC Infinity Dashboard](https://github.com/magikh0e/haos_stuff/blob/main/AC%20Infinity/acinfinity_integration_dashboard) - Using AC Infinity wifi/ble controllers and sensorPush Hygrometers.

![AC Infinity HAOS Integration dashboard](https://github.com/magikh0e/haos_stuff/blob/main/AC%20Infinity/ACInfinity%20Dashboard%20%E2%80%93%20Home%20Assistant.png?raw=true)

### <ins>Custom Cards used by dashboard, install via HACS..</ins>
Custom Button Card: https://github.com/custom-cards/button-card  
Custom Stack In Card: https://github.com/custom-cards/stack-in-card  
Custom Declutter Card: https://github.com/custom-cards/decluttering-card  
Custom Flex Horseshoe Card: https://github.com/AmoebeLabs/flex-horseshoe-card  
Custom Mod Card: https://github.com/thomasloven/lovelace-card-mod  
Custom Vertical Stack In Card: https://github.com/ofekashery/vertical-stack-in-card  
Custom Browser Mod Card: https://github.com/thomasloven/hass-browser_mod  
Custom Gap Card: https://github.com/thomasloven/lovelace-gap-card  
Custom Slider Button Card: https://github.com/mattieha/slider-button-card  
Custom Tabbed Card: https://github.com/kinghat/tabbed-card  
Custom Number Box Card: https://github.com/junkfix/numberbox-card

## EcoFlow Delta 3 Max Plus — Dashboard
[EcoFlow Delta 3 Max Plus Dashboard](https://github.com/magikh0e/haos_stuff/blob/main/EcoFlow/Ecoflow%20Delta%203%20Max%20Plus%20Dashboard.yaml) - For the [tolwi/hassio-ecoflow-cloud](https://github.com/tolwi/hassio-ecoflow-cloud) integration on the official Developer API (accessKey/secretKey) path.

![EcoFlow Delta 3 Max Plus dashboard](https://github.com/magikh0e/haos_stuff/blob/main/EcoFlow/Ecoflow%20Delta%203%20Max%20Plus%20Dashboard.png?raw=true)

Battery SoC + charge/discharge state, output controls (AC / AC2 / DC switches, X-Boost, beeper, backup reserve, bypass), SoC limit + backup-reserve sliders, real-time power flow (total in/out, AC input, 2x solar, 12V DC), per-port USB-C/USB-A output, and a 24h energy history chart.

### <ins>Custom Cards used by dashboard, install via HACS..</ins>
None - uses only built-in Home Assistant cards (gauge, tile, entities, statistics-graph, sections view). No HACS dependencies.

### <ins>Setup</ins>
Entity IDs use a generic `delta_3_max_plus_` prefix across the sensor/switch/number domains. Find your real prefix via Developer Tools -> States (filter "battery"), then find-and-replace `delta_3_max_plus` throughout the YAML with your device's prefix (e.g. `delta_3_max_plus_1753`). Paste via Edit dashboard -> Raw configuration editor.

## Creality K2 Plus — Dashboard
[Creality K2 Plus Dashboard](https://github.com/magikh0e/haos_stuff/blob/main/Creality%20K2%20Plus/Creality%20K2%20Plus%20Dashboard.yaml) - For the [3dg1luk43/ha_creality_ws](https://github.com/3dg1luk43/ha_creality_ws) integration (local WebSocket). Find your entity prefix in Developer Tools → Template and swap `k2plus_1516` throughout.

> 📖 Full write-up: [magikh0e.pl/pubHomeAutomation/creality-k2-plus-dashboard.html](https://magikh0e.pl/pubHomeAutomation/creality-k2-plus-dashboard.html)

![Creality K2 Plus Home Assistant dashboard](https://github.com/magikh0e/haos_stuff/blob/main/Creality%20K2%20Plus/Creality%20K2%20Plus%20Dashboard.jpg?raw=true)

Live camera, print status with ETA, nozzle/bed/chamber gauges + target sliders, XYZ position, print controls (pause/resume/stop/home), print tuning + flow, chamber light, fans, dual CFS filament tables (type/color/remaining), external filament, and a 2-hour temperature history graph. Progress + model-preview cards auto-hide when idle; an attention banner appears on error or stopped.

### <ins>Custom Cards used by dashboard, install via HACS..</ins>
None - uses only built-in Home Assistant cards (markdown, picture-entity, gauge, tile, entity, button, history-graph). No HACS dependencies.

## Volcano Hybrid — Scripts, Dashboards & Voice
Scripts, automations, helper templates, voice commands, and dashboards for the [SavageNL/home-assistant-volcano-hybrid](https://github.com/SavageNL/home-assistant-volcano-hybrid) BLE integration. Session controls implement the [Vapesuvius temperature ladder](https://www.reddit.com/user/Vapesuvius/comments/zuwcs7/) (179 → 185 → 191 → 199 → 205 °C). Stock Lovelace cards only — no HACS dependencies.

> 📖 Full write-up with the reasoning behind each file: [magikh0e.pl/pubHomeAutomation/volcano-hybrid.html](https://magikh0e.pl/pubHomeAutomation/volcano-hybrid.html)

### <ins>Dashboards</ins>
[volcanoHybridLovelace.yaml](https://github.com/magikh0e/haos_stuff/blob/main/Volcano%20Hybrid/volcanoHybridLovelace.yaml) - Compact control-surface tile: live/target temp, heater state, runtime, thermostat slider, ±step buttons, fan on/off, and a one-press "Fill a bag" button.  
[volcanoHybridLovelaceSession.yaml](https://github.com/magikh0e/haos_stuff/blob/main/Volcano%20Hybrid/volcanoHybridLovelaceSession.yaml) - Full session dashboard: thermostat + fan, the 5-rung ladder as direct-jump buttons, auto-progress toggle with "next step in X min" countdown, and a confirmation-guarded fill-a-bag CTA.  
[volcanoHybridLovelaceSessionLite.yaml](https://github.com/magikh0e/haos_stuff/blob/main/Volcano%20Hybrid/volcanoHybridLovelaceSessionLite.yaml) - Minimal-dependency variant of the session dashboard; needs only the climate entity, BLE connection sensor, and auto-off-time sensor (no runtime sensor, auto-progress, or companion scripts).  

### <ins>Scripts & automations</ins>
[volcanoHybridFillBag.yaml](https://github.com/magikh0e/haos_stuff/blob/main/Volcano%20Hybrid/volcanoHybridFillBag.yaml) - Named script (`script.volcano_fill_bag`) running the full bag-fill sequence: heat → reach temp → fan ~41s → stop → heater off. Callable from automations, dashboard buttons, voice, or REST.  
[volcanoHybridProgress.yaml](https://github.com/magikh0e/haos_stuff/blob/main/Volcano%20Hybrid/volcanoHybridProgress.yaml) - Auto-progress automation that walks temperature up the Vapesuvius ladder at 0/5/10/15/20 min of accumulated runtime.  
[volcanoHybridStepTemp.yaml](https://github.com/magikh0e/haos_stuff/blob/main/Volcano%20Hybrid/volcanoHybridStepTemp.yaml) - Manual ±temperature stepping (snap-to-next-rung) for a dimmer remote; companion override to the auto-progress automation.  

### <ins>Helpers & voice</ins>
[volcanoHybridRuntimeTemplate.yaml](https://github.com/magikh0e/haos_stuff/blob/main/Volcano%20Hybrid/volcanoHybridRuntimeTemplate.yaml) - Template sensor (`sensor.volcano_runtime`) tracking minutes since the climate entity entered `heat`. Fallback for integration versions that don't expose a runtime/on-time sensor.  
[volcanoHybridVoice.yaml](https://github.com/magikh0e/haos_stuff/blob/main/Volcano%20Hybrid/volcanoHybridVoice.yaml) - HA Assist voice control: `intent_script` + `custom_sentences` mapping natural-language phrases to VolcanoFillBag / VolcanoStartAutoProgress intents. Works with any Assist surface (mobile app, voice satellites, local Piper).

## Cannatrol — Reverse-Engineered BLE Protocol
[Cannatrol BLE Protocol](https://github.com/magikh0e/haos_stuff/blob/main/Cannatrol/Cannatrol_BLE_Protocol.md) - Reverse-engineered Bluetooth Low Energy / GATT protocol for the Cannatrol curing unit, captured and decoded from an Android `btsnoop_hci.log`. Documents the custom GATT service, the write/notify characteristic pair, and the text-based request/response command format — the groundwork for a future native Home Assistant integration.

## DIY Alarm System
[DIY Alarm System](https://github.com/magikh0e/haos_stuff/blob/main/DIY%20Alarm%20System/alarm-system.html) - Self-hosted alarm built entirely in Home Assistant: contact + motion sensor triggers, TTS announcements, RGB light feedback, PTZ camera tracking, privacy mode, and smart-lock integration. No cloud monitoring service. Full writeup with YAML — also published at [magikh0e.pl](https://magikh0e.pl/pubHomeAutomation/alarm-system.html).

## ESP32 Bluetooth Proxy
[ESP32 Bluetooth Proxy](https://github.com/magikh0e/haos_stuff/blob/main/ESP32/esp32-bluetooth-proxy.md.txt) - No-code guide to flashing an ESP32 as a Home Assistant Bluetooth proxy, extending HA's BLE range across the house to reach Govee / Xiaomi / Inkbird sensors (and BLE devices like the Volcano and Cannatrol) in rooms the host can't cover. Uses the ESPHome web flasher — ~10 minutes per proxy. Also published at [magikh0e.pl](https://magikh0e.pl/pubHomeAutomation/esp32-bluetooth-proxy.html).

## Universal Media Player — Unified TV Control
[Universal Media Player](https://github.com/magikh0e/haos_stuff/tree/main/Universal%20Media%20Player) - Combine the multiple integrations a single TV exposes (Android Debug Bridge, Android TV Remote, Google Cast, webOS, Plex) into one controllable media_player entity, with per-integration command routing and Wake-on-LAN power-on. Bedroom (Hisense Google TV) + Living Room (LG webOS) configs and a WoL helper script.

## Dog TV
[Dog TV](https://github.com/magikh0e/haos_stuff/tree/main/Dog%20TV) - Plays a random relaxing YouTube video on a TV to keep a dog calm while home alone. Shared playlist via a response-variable helper; Android TV launches via ADB intent, LG webOS via Google Cast. Full writeup also published at [magikh0e.pl](https://magikh0e.pl/pubHomeAutomation/dog-tv-voice-command.html).

## Morning Briefing
[Morning Briefing](https://github.com/magikh0e/haos_stuff/tree/main/Morning%20Briefing) - Spoken TTS briefing (greeting, date, outdoor weather, indoor temps, snorkel-report nudge) played on a media player passed as a parameter. Defensive templating skips any unavailable sensor instead of erroring.

## Maui Snorkel Report
[Maui Snorkel Report](https://github.com/magikh0e/haos_stuff/tree/main/Maui%20Snorkel%20Report) - Daily Maui snorkeling-conditions report on a dashboard. Sidesteps the source page's Cloudflare protection by probing the open image host directly for date-based filenames. command_line sensor + Generic Camera + dashboard card.

## Medication Reminders (moved)
Multi-dose medication reminders — actionable notifications that nag until marked given, missed-dose escalation (time-sensitive push + spoken TTS), and household-synced "given or not" state with logbook accountability — have moved to their own dedicated repositories:
- **[ha-medication-reminder](https://github.com/magikh0e/ha-medication-reminder)** — YAML package version (stable, no dependencies).
- **[ha-medication-reminder-hacs](https://github.com/magikh0e/ha-medication-reminder-hacs)** — HACS custom integration with UI config, supply tracking, and flexible scheduling (alpha).

## Misc Automations
[Contact Sensor Issue-State Notification (Blueprint)](https://github.com/magikh0e/haos_stuff/blob/main/contactSensorIssueState.yaml) - Reusable HA blueprint that fires push + persistent notifications when a binary_sensor (or group) stays in an "issue state" (open / triggered / unavailable) longer than a configurable duration. Handles debounce, an optional gating condition, auto-clear, repeat reminders, multi-target notify, and custom enter/leave actions. Instantiate once per door/window/motion sensor.  
[Front Door Open Alert](https://github.com/magikh0e/haos_stuff/blob/main/frontDoorAlert.yaml) - Automation pair: alerts via phone push + Nest TTS when the front door is left open 30s+, repeating every ~10 min; plus a "door closed" announcement that skips quick in-and-out trips. Retarget by swapping three entity_ids.
