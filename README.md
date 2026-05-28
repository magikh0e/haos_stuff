## Home Assistant pack for the Storz & Bickel Volcano Hybrid.
Scripts, automations, helper templates, voice commands, and dashboards for the [SavageNL/home-assistant-volcano-hybrid](https://github.com/SavageNL/home-assistant-volcano-hybrid) BLE integration. Session controls implement the [Vapesuvius temperature ladder](https://www.reddit.com/user/Vapesuvius/comments/zuwcs7/) (179 → 185 → 191 → 199 → 205 °C). Stock Lovelace cards only — no HACS dependencies.

### <ins>Dashboards</ins>
[volcanoHybridLovelace.yaml.txt](https://github.com/magikh0e/haos_stuff/blob/main/volcanoHybridLovelace.yaml.txt) - Compact control-surface tile: live/target temp, heater state, runtime, thermostat slider, ±step buttons, fan on/off, and a one-press "Fill a bag" button.  
[volcanoHybridLovelaceSession.yaml.txt](https://github.com/magikh0e/haos_stuff/blob/main/volcanoHybridLovelaceSession.yaml.txt) - Full session dashboard: thermostat + fan, the 5-rung ladder as direct-jump buttons, auto-progress toggle with "next step in X min" countdown, and a confirmation-guarded fill-a-bag CTA.  
[volcanoHybridLovelaceSessionLite.yaml.txt](https://github.com/magikh0e/haos_stuff/blob/main/volcanoHybridLovelaceSessionLite.yaml.txt) - Minimal-dependency variant of the session dashboard; needs only the climate entity, BLE connection sensor, and auto-off-time sensor (no runtime sensor, auto-progress, or companion scripts).  

### <ins>Scripts & automations</ins>
[volcanoHybridFillBag.yaml](https://github.com/magikh0e/haos_stuff/blob/main/volcanoHybridFillBag.yaml) - Named script (`script.volcano_fill_bag`) running the full bag-fill sequence: heat → reach temp → fan ~41s → stop → heater off. Callable from automations, dashboard buttons, voice, or REST.  
[volcanoHybridProgress.yaml.txt](https://github.com/magikh0e/haos_stuff/blob/main/volcanoHybridProgress.yaml.txt) - Auto-progress automation that walks temperature up the Vapesuvius ladder at 0/5/10/15/20 min of accumulated runtime.  
[volcanoHybridStepTemp.yaml.txt](https://github.com/magikh0e/haos_stuff/blob/main/volcanoHybridStepTemp.yaml.txt) - Manual ±temperature stepping (snap-to-next-rung) for a dimmer remote; companion override to the auto-progress automation.  

### <ins>Helpers & voice</ins>
[volcanoHybridRuntimeTemplate.yaml.txt](https://github.com/magikh0e/haos_stuff/blob/main/volcanoHybridRuntimeTemplate.yaml.txt) - Template sensor (`sensor.volcano_runtime`) tracking minutes since the climate entity entered `heat`. Fallback for integration versions that don't expose a runtime/on-time sensor.  
[volcanoHybridVoice.yaml.txt](https://github.com/magikh0e/haos_stuff/blob/main/volcanoHybridVoice.yaml.txt) - HA Assist voice control: `intent_script` + `custom_sentences` mapping natural-language phrases to VolcanoFillBag / VolcanoStartAutoProgress intents. Works with any Assist surface (mobile app, voice satellites, local Piper).  