# Morning Briefing

A spoken TTS briefing played on a chosen media player — greeting, date, outdoor
weather, indoor temps, and a snorkel-report nudge. The target speaker is a
script parameter, so the same script works for any Assist satellite, Nest
display, or speaker group.

## Files

- [morningBriefing.yaml](morningBriefing.yaml) — the `morning_briefing`
  script. Fields: `target_player` (required), `volume` (optional).

## Behavior

- Greeting adapts to the time of day.
- **Defensive templating** — every sensor is read with `float(none)` and each
  line only speaks if its value is valid, so an unavailable sensor is skipped
  rather than crashing the script.
- Dynamic "open a window" line when it's cooler outside than the bedroom.
- Announces the daily snorkel report if it's posted (see the Maui Snorkel Report
  pack).

## Setup

1. Add to `scripts.yaml`.
2. Replace entity IDs (`sensor.openweathermap_*`, indoor temp sensors,
   `sensor.maui_snorkel_report_url`, `tts.home_assistant_cloud`) with yours.
3. Call it manually, from a time trigger, or on first morning motion:

   ```yaml
   action: script.morning_briefing
   data:
     target_player: media_player.home_assistant_voice
     volume: 0.6
   ```

## Requires

- A TTS engine (example uses Home Assistant Cloud / `tts.home_assistant_cloud`).
- OpenWeatherMap (or swap for any weather sensors).
