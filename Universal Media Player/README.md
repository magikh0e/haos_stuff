# Universal Media Player — unified TV control

Combine the multiple Home Assistant integrations that each TV exposes into a
single controllable `media_player` entity, so dashboards, voice, and automations
target one entity instead of guessing between several identically-named ones.

## The problem

A single physical TV often shows up under several integrations at once — e.g.
Android Debug Bridge, Android TV Remote, Google Cast, Music Assistant, and Plex,
all named "Bedroom TV". Voice commands and area cards then pick one arbitrarily,
and "now playing" metadata only lives on whichever child is actually active.

## The fix

A [Universal Media Player](https://www.home-assistant.io/integrations/universal/)
wraps the children, routes each command to the integration best suited for it,
and surfaces the active child's state + metadata as one entity.

## Files

- [bedroom-tv.yaml.txt](bedroom-tv.yaml.txt) — Hisense Google TV. Routes power-on
  to Android TV Remote (wakes from sleep), control/apps/volume to Android Debug
  Bridge, and pulls now-playing metadata from the Cast child.
- [living-room-tv.yaml.txt](living-room-tv.yaml.txt) — LG webOS TV. Routes
  power-on to a Wake-on-LAN helper script, everything else to the webOS native
  entity, metadata from webOS.
- [turn-on-living-room-tv.yaml.txt](turn-on-living-room-tv.yaml.txt) — the WoL
  helper script the LG config uses for power-on.

## Setup

1. Add the `media_player:` blocks to `configuration.yaml` (merge under one
   `media_player:` key if you have multiple).
2. Replace child `entity_id`s with your own (Developer Tools -> States). Suffixes
   like `_3` are install-specific.
3. For the LG: enable `wake_on_lan:` in config, set the real MAC in the helper
   script, and turn on "Mobile TV On" on the TV.
4. Restart HA. Two new entities appear: `media_player.bedroom_tv` and
   `media_player.living_room_tv`.
5. Rename the child entities (e.g. "Bedroom TV (Cast)") so only the Universal MP
   is plainly "Bedroom TV", and hide the children from Assist so voice targets
   the unified entity.

## Notes

- Universal MP `commands:` use the legacy `service:` + `data:` syntax (not
  `action:` / `target:`) — the integration's schema requires it.
- `attributes:` map one child per attribute; metadata shows from that child. For
  full active-child metadata on dashboards, the `custom:mini-media-player` card
  follows the active child automatically.
