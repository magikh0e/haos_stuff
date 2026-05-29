# Home Assistant DIY Alarm System

A self-hosted alarm built entirely in Home Assistant — **no cloud monitoring service**. Contact + motion sensors, TTS announcements, RGB light feedback, PTZ camera tracking, privacy mode, and smart-lock integration, with full YAML.

📄 **Full writeup:** [alarm-system.html](alarm-system.html) — also published at [magikh0e.pl](https://magikh0e.pl/pubHomeAutomation/alarm-system.html)

## What's covered

1. **The alarm panel** — a `manual` alarm_control_panel with arm-home / arm-away / arm-night states and entry/exit delays
2. **Detection automations** — perimeter (doors) + interior (motion) triggers, mode-aware so interior sensors only arm when away
3. **Response automations** — what fires on trigger: notifications, TTS, lights
4. **RGB light feedback** — color-coded states (armed / triggered / disarmed) for at-a-glance status
5. **PTZ camera + presence-driven tracking** — camera follows motion during an alarm
6. **Privacy mode** — cameras auto-off when you're home
7. **Smart door locks** — lock/unlock tied to arm state
- **Optional auto-arm** — arm on a schedule or when everyone leaves
- **⚠️ Caveats** — honest limitations of a DIY/self-hosted alarm versus a professionally monitored system

## Philosophy

Local-first, no subscription, no cloud dependency — you own the hardware and the logic. The tradeoff (detailed in the writeup's caveats) is that there's no third-party monitoring center: it alerts *you*, not a dispatcher.
