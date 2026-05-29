# Dog TV

Plays a random relaxing/ambient YouTube video on a TV — built to keep a dog calm
while home alone. One shared video list, two TV-specific launchers.

## Files

- [dogTV.yaml.txt](dogTV.yaml.txt) — three scripts:
  - `get_dog_tv_url` — returns a random video from the list as `{url, video_id}`
    via a response variable (single source of truth for the playlist).
  - `dog_tv_bedroom` — Android TV. Wakes the TV, then launches the URL with an
    ADB `am start` view intent.
  - `dog_tv_living_room` — LG webOS TV. Wakes via the WoL helper, then casts the
    video to the YouTube app via `media_player.play_media` (cast).

## Why two different launchers

Android TV and LG webOS handle app launching completely differently — ADB
intents vs. Google Cast — so each platform gets its own approach. The shared
`get_dog_tv_url` keeps the playlist in one place regardless of target.

## Setup

1. Add the scripts to `scripts.yaml`.
2. Swap entity IDs for your own TVs.
3. Living Room depends on the WoL helper in
   [../Universal Media Player/turn-on-living-room-tv.yaml.txt](../Universal%20Media%20Player/turn-on-living-room-tv.yaml.txt).
4. Edit the `videos:` list to your preferred channels/videos.
5. Optionally expose via `intent_script` for voice ("put on Dog TV in the
   bedroom").
