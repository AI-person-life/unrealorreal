# Tooling

These came out of the project because nothing off-the-shelf did the job. All run
locally, all produce Hungarian output.

🇭🇺 [Magyarul](../hu/eszkozok.md)

## Wi-Fi survey aggregation

Turns a Kismet log or a WiGLE export into aggregate statistics. **Deliberately
incapable of identifying an individual network:** it never emits a MAC address, an
SSID or a coordinate — only counts and ratios. That makes its output directly
publishable.

A 3,416-network survey was summarised with it.

## WiGLE client

WiGLE's daily query allowance is a sliding scale, and very tight on new accounts.
So the client:

- caches every response to disk (never fetches the same box twice),
- stops at a configurable page count when paginating,
- emits aggregates only.

## Local Hungarian dictation

The development tool we use has built-in dictation that doesn't support Hungarian,
and the language list can't be extended client-side. The workaround is a local
`faster-whisper large-v3`: flawless in Hungarian, sends nothing off the machine,
and in exchange runs at roughly 4× real time without a GPU.

## Offline Hungarian speech synthesis

`piper` with local voices. A morning announcement (time + weather) and the
website's greeting both use it — same voice, same tuning values.

---

## Two hardware traps that ate hours

**The microphone preamp can sit at zero decibels by default.** On an ALC269VB
codec, `Front Mic Boost` shipped at 0, which made recordings peak at two percent
of scale — effectively silent. One command fixes it:

```bash
amixer -c 0 sset 'Front Mic Boost' 2   # ~24 dB
```

**`arecord -D plughw:...` steals the microphone from the sound server.** PipeWire
drops the capture node, the tray icon disappears, and it does not come back on its
own.

```bash
# wrong: claims the device exclusively
arecord -D plughw:0,0 ...
# right: goes through the sound server
arecord -D default ...
# if it already happened:
systemctl --user restart wireplumber
```
