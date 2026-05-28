# haos_stuff
Home assistant automations, dashboards, scripts etc...

## Custom dashboard for the AC Infinity integration for Home Assistant.
[Custom HAOS AC Infinity Dashboard](https://github.com/magikh0e/haos_stuff/blob/main/acinfinity_integration_dashboard) - Using AC Infinity wifi/ble controllers and sensorPush Hygrometers.

![AC Infinity HAOS Integration dashboard](https://github.com/magikh0e/haos_stuff/blob/main/ACInfinity%20Dashboard%20%E2%80%93%20Home%20Assistant.png?raw=true)


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

## Custom dashboard for the EcoFlow Delta 3 Max Plus.
[EcoFlow Delta 3 Max Plus Dashboard](https://github.com/magikh0e/haos_stuff/blob/main/Ecoflow%20Delta%203%20Max%20Plus%20Dashboard.yaml) - For the [tolwi/hassio-ecoflow-cloud](https://github.com/tolwi/hassio-ecoflow-cloud) integration on the official Developer API (accessKey/secretKey) path.

![EcoFlow Delta 3 Max Plus dashboard](https://github.com/magikh0e/haos_stuff/blob/main/Ecoflow%20Delta%203%20Max%20Plus%20Dashboard.png?raw=true)

Battery SoC + charge/discharge state, output controls (AC / AC2 / DC switches, X-Boost, beeper, backup reserve, bypass), SoC limit + backup-reserve sliders, real-time power flow (total in/out, AC input, 2x solar, 12V DC), per-port USB-C/USB-A output, and a 24h energy history chart.

### <ins>Custom Cards used by dashboard, install via HACS..</ins>
None - uses only built-in Home Assistant cards (gauge, tile, entities, statistics-graph, sections view). No HACS dependencies.

### <ins>Setup</ins>
Entity IDs use a generic `delta_3_max_plus_` prefix across the sensor/switch/number domains. Find your real prefix via Developer Tools -> States (filter "battery"), then find-and-replace `delta_3_max_plus` throughout the YAML with your device's prefix (e.g. `delta_3_max_plus_1753`). Paste via Edit dashboard -> Raw configuration editor.

## Home Assistant pack for the Storz & Bickel Volcano Hybrid.
Scripts, automations, helper templates, voice commands, and dashboards for the [SavageNL/home-assistant-volcano-hybrid](https://github.com/SavageNL/home-assistant-volcano-hybrid) BLE integration. Session controls implement the [Vapesuvius temperature ladder](https://www.reddit.com/user/Vapesuvius/comments/zuwcs7/) (179 → 185 → 191 → 199 → 205 °C). Stock Lovelace cards only — no HACS dependencies.

### <ins>Dashboards</ins>
[volcanoHybridLovelace.yaml.txt](https://github.com/magikh0e/haos_stuff/blob/main/Volcano%20Hybrid/volcanoHybridLovelace.yaml.txt) - Compact control-surface tile: live/target temp, heater state, runtime, thermostat slider, ±step buttons, fan on/off, and a one-press "Fill a bag" button.  
[volcanoHybridLovelaceSession.yaml.txt](https://github.com/magikh0e/haos_stuff/blob/main/Volcano%20Hybrid/volcanoHybridLovelaceSession.yaml.txt) - Full session dashboard: thermostat + fan, the 5-rung ladder as direct-jump buttons, auto-progress toggle with "next step in X min" countdown, and a confirmation-guarded fill-a-bag CTA.  
[volcanoHybridLovelaceSessionLite.yaml.txt](https://github.com/magikh0e/haos_stuff/blob/main/Volcano%20Hybrid/volcanoHybridLovelaceSessionLite.yaml.txt) - Minimal-dependency variant of the session dashboard; needs only the climate entity, BLE connection sensor, and auto-off-time sensor (no runtime sensor, auto-progress, or companion scripts).  

### <ins>Scripts & automations</ins>
[volcanoHybridFillBag.yaml](https://github.com/magikh0e/haos_stuff/blob/main/Volcano%20Hybrid/volcanoHybridFillBag.yaml) - Named script (`script.volcano_fill_bag`) running the full bag-fill sequence: heat → reach temp → fan ~41s → stop → heater off. Callable from automations, dashboard buttons, voice, or REST.  
[volcanoHybridProgress.yaml.txt](https://github.com/magikh0e/haos_stuff/blob/main/Volcano%20Hybrid/volcanoHybridProgress.yaml.txt) - Auto-progress automation that walks temperature up the Vapesuvius ladder at 0/5/10/15/20 min of accumulated runtime.  
[volcanoHybridStepTemp.yaml.txt](https://github.com/magikh0e/haos_stuff/blob/main/Volcano%20Hybrid/volcanoHybridStepTemp.yaml.txt) - Manual ±temperature stepping (snap-to-next-rung) for a dimmer remote; companion override to the auto-progress automation.  

### <ins>Helpers & voice</ins>
[volcanoHybridRuntimeTemplate.yaml.txt](https://github.com/magikh0e/haos_stuff/blob/main/Volcano%20Hybrid/volcanoHybridRuntimeTemplate.yaml.txt) - Template sensor (`sensor.volcano_runtime`) tracking minutes since the climate entity entered `heat`. Fallback for integration versions that don't expose a runtime/on-time sensor.  
[volcanoHybridVoice.yaml.txt](https://github.com/magikh0e/haos_stuff/blob/main/Volcano%20Hybrid/volcanoHybridVoice.yaml.txt) - HA Assist voice control: `intent_script` + `custom_sentences` mapping natural-language phrases to VolcanoFillBag / VolcanoStartAutoProgress intents. Works with any Assist surface (mobile app, voice satellites, local Piper).