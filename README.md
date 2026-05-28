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
[EcoFlow Delta 3 Max Plus Dashboard](https://github.com/magikh0e/haos_stuff/blob/main/delta3_max_plus_dashboard.yaml) - For the [tolwi/hassio-ecoflow-cloud](https://github.com/tolwi/hassio-ecoflow-cloud) integration on the official Developer API (accessKey/secretKey) path.

Shows battery SoC, charge/discharge remaining time, real-time power flow (total in/out, AC input, 2x solar, 12V DC), per-port USB-C/USB-A output, and a 24h energy history chart.

### <ins>Custom Cards used by dashboard, install via HACS..</ins>
None - uses only built-in Home Assistant cards (gauge, tile, statistics-graph, sections view). No HACS dependencies.

### <ins>Setup</ins>
Entity IDs use a generic `sensor.delta_3_max_plus_` prefix. Find your real prefix via Developer Tools -> States (filter "battery"), then find-and-replace `delta_3_max_plus` throughout the YAML with your device's prefix (e.g. `delta_3_max_plus_1753`). Paste via Edit dashboard -> Raw configuration editor.