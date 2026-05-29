# Dog Medication Reminders

Multi-dose daily medication reminders with actionable phone notifications that
**nag until marked given**, a **missed-dose escalation**, and shared "given or
not" state **synced across every Home Assistant Companion app** — so anyone in
the household can mark a dose given and everyone sees it, with logbook
accountability for who did it.

No external service needed: Home Assistant itself is the sync layer.

## How it works

- One `input_boolean` per dose represents "given today"; a daily reset clears
  them at midnight.
- A `time_pattern` automation re-checks every 15 minutes and sends an actionable
  reminder ("✅ Mark given") for any dose that's past its time, within the nag
  window, and not yet given. Because it re-evaluates each tick rather than
  running a long loop, **it survives HA restarts**.
- Tapping "Mark given" (from any phone) flips the dose on and clears the
  notification everywhere. The dashboard and all Companion apps reflect it
  instantly; the logbook records who marked it.
- If a dose is never marked given by its window-end (dose time + nag window), a
  **missed-dose escalation** fires: a time-sensitive push to everyone in the
  notify group plus a spoken TTS backstop.

## Files

- [configuration.yaml](configuration.yaml) — `input_boolean` helpers (one per
  dose) + a `dog_caretakers` notify group.
- [automations.yaml](automations.yaml) — four automations: daily reset,
  reminder/nag, mark-given handler, and missed-dose escalation.
- [dashboard-card.yaml](dashboard-card.yaml) — an entities card with per-dose
  "given X ago" readouts; also lets you mark a dose manually.

## Setup

1. Add the helpers + notify group to `configuration.yaml` (replace
   `mobile_app_phone` with your real notify service; add a second caretaker to
   the group if desired). Restart HA.
2. Add the four automations to `automations.yaml`. **Adjust the dose times** to
   your schedule — keep them on `:00/:15/:30/:45` boundaries so the first
   reminder lands on time, and set the missed-dose times to dose time +
   `nag_minutes`.
3. Add the dashboard card.

## Customizing

- **More/fewer doses** — add or remove an `input_boolean`, a line in the
  reminder's `doses:` list, an entry in each `bool_map:`, a trigger in the
  missed-dose automation, and a card row.
- **Nag window / interval** — change `nag_minutes` and the `time_pattern`
  minutes.
- **Fixed course (e.g. 10-day antibiotic)** — add a `condition:` gating the
  automations on a date range, or an `input_datetime` end date.

## Why not a to-do list or CalDAV?

A to-do list tracks *what* to do; this tracks *whether a dose was given* with
actionable reminders and household accountability. HA's own state sync across
Companion apps is exactly the right tool — CalDAV would only help if you needed
the status inside an external task app, at the cost of weaker actionable
reminders.
