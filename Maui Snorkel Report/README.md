# Maui Snorkel Report

Pulls [The Snorkel Store's](https://thesnorkelstore.com/maui-snorkeling-conditions-reports/)
daily Maui snorkeling-conditions report onto a Home Assistant dashboard,
auto-updating every few hours.

## The challenge

The conditions page is behind Cloudflare bot protection — `curl`/`wget` get a
challenge page, and HA's scrape integration is blocked too. But:

- The **image host** (a Cloudflare Worker) serves the report JPG without the
  challenge.
- The upload **folder path is static**.
- The **filename is date-based**: `<month>-<day>-<year>-email.jpg`.

## The approach

Instead of scraping the protected page, a `command_line` sensor **probes the
open image host directly** for the last ~10 days of date-based filenames and
returns the first that exists (HTTP 200). This sidesteps Cloudflare entirely and
naturally handles the daily publish lag (and skipped days).

## Files

- [snorkelReport.yaml](snorkelReport.yaml) — the `command_line` sensor,
  plus inline instructions for the Generic Camera (UI setup) and the dashboard
  card with an auto date caption.

## Setup

1. Add the `command_line:` sensor to `configuration.yaml`, restart, and confirm
   `sensor.maui_snorkel_report_url` populates with the latest report URL.
2. Add a Generic Camera via the UI with still-image URL
   `{{ states('sensor.maui_snorkel_report_url') }}`.
3. Add the dashboard card (in the file comments).

## Notes

- `command_line` runs in the HA Core container, which ships BusyBox `wget`, not
  `curl` — hence `wget --spider` for the existence check.
- To extract the conditions as text/data (not just the image), you'd need to get
  past Cloudflare (e.g. FlareSolverr) or run an AI/vision pass on the image — the
  `media_content_id` for `ai_task.generate_data` is the same sensor URL.
