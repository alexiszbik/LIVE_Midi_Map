# LIVE Midi Map

Map of all MIDI messages used across Daisy modules, organized by channel.

---

## Global (all channels)

Messages received regardless of channel.

| Message | Source | Effect |
|---|---|---|
| MIDI Clock (0xF8) | **MIDI to CV** | Drives the synced clock (24 ppqn) |
| MIDI Clock (0xF8) | **FX for Modular** | Tempo detection (averaged over 5 quarter notes). Drives synced delay time when sync switch is disengaged. |
| Start (0xFA) | **MIDI to CV** | Resets and starts the synced clock |
| Start (0xFA) | **FX for Modular** | Resets tick counter and clock timer |
| Stop (0xFC) | **MIDI to CV** | Stops the synced clock and closes the gate |

---

## Channel 1

*No assignments yet.*

---

## Channel 2

### MIDI to CV Module

| Message | Number | Effect |
|---|---|---|
| Note On | C2–C7 (36–96) | Pitch CV (1V/oct, 0–5V) + Gate high |
| Note Off | — | Gate low |
| CC | 13 | Extra CV output (0–5V, scaled from CC value 0–127). Only active when the Extra CV switch is in CC mode. |
| CC | 60 | Mute: value > 60 mutes the clock output, value <= 60 unmutes |

### FX for Modular

Stereo multi-effect (bit crusher, ping-pong delay, frequency shifter, HPF).

| Message | Number | Effect |
|---|---|---|
| CC | 80 | Global Mute: value > 60 = muted, value <= 60 = unmuted (smooth fade) |

**Synced Delay Time**: when the sync switch is disengaged, the delay time locks to a **dotted eighth note** (3/16 of a 4/4 bar) at the detected BPM, clamped between 20 ms and 2000 ms.

---

## Channel 3

*No assignments yet.*

---

## Channel 4

*No assignments yet.*

---

## Channel 5

*No assignments yet.*

---

## Channel 6

### MIDI2CVR4

Arduino Nano R4 MIDI to CV module — pitch, gate, and CC outputs. [GitHub repository](https://github.com/alexiszbik/midi2cvr4).

| Message | Number | Effect |
|---|---|---|
| Note On | C2–C7 (36–96) | Pitch CV (1 V/oct, 0–5 V) + Gate high |
| Note Off | — | Gate low |
| CC | 31 | CC CV output (0–10 V, scaled from CC value 0–127) |

### Synth Bass FX (MultiFX2)

Mono multi-effect (HPF, bit crusher, frequency shifter, reverb send, input metering). [GitHub repository](https://github.com/alexiszbik/multifx2).

| Message | Number | Effect |
|---|---|---|
| CC | 10 | HPF cutoff (0–127 → parameter 0–1) |
| CC | 11 | HPF resonance (0–127 → parameter 0–1) |
| CC | 12 | Bit crusher rate (0–127 → parameter 0–1) |
| CC | 13 | Frequency shifter amount / direction (0–127 → parameter 0–1) |
| CC | 14 | Frequency shifter dry/wet (0–127 → parameter 0–1) |
| CC | 15 | Stutter depth (0–127 → parameter 0–1, `pow³` curve in firmware) |
| CC | 16 | Reverb send (0–127 → parameter 0–1, `value²` curve in firmware) |
| CC | 80 | Global mute: value > 60 = muted, value <= 60 = unmuted (smooth fade) |
| Note On | 0 (C-1) | Stutter gate open (velocity > 0) — drives MIDI LED; stutter FX bypassed in current firmware |
| Note Off | 0 (C-1) | Stutter gate closed |

**Hardware knobs (mirror CC 10–16):**

| Pin | Control |
|-----|---------|
| D15 | HPF cutoff |
| D21 | HPF resonance |
| D17 | Bit crusher rate |
| D19 | Frequency shifter amount / direction |
| D18 | Frequency shifter dry/wet |
| D22 | Stutter depth |
| D16 | Reverb send |

**Notes:**
- CC values 10–16 are scaled to `0.0–1.0` in firmware (`value / 127`).
- Reverb is a parallel send (200 Hz HPF on send, LFO-modulated room size, ~8 s tail).
- Mute switch on hardware pin D11 mirrors CC 80 mute state.

---

## Channel 7

### VocoderSynth

Two-oscillator synth for driving a vocoder pedal. [GitHub repository](https://github.com/alexiszbik/VocoderSynth).

| Message | Number | Effect |
|---|---|---|
| Note On / Note Off | — | Trigger voices (poly: up to 4 voices) |
| CC | 10 | Play Mode: 0 = Mono, 127 = Poly |
| CC | 11 | Glide (0–127 → parameter 0–1, squared in seconds) |
| CC | 12 | Release (0–127 → parameter 0–1, `pow³` curve mapped to 0.005–8 s) |
| CC | 13 | Osc mix: balance osc A / osc B (0 = osc B, 127 = osc A, sqrt dry/wet) |
| CC | 14 | Osc A waveform: 0–63 = saw, 64–127 = square |
| CC | 15 | Osc B waveform: 0–63 = saw, 64–127 = square |
| CC | 16 | Osc A pulse width: duty cycle in square mode (0–127 → 0–1, 64 = 50 %) |
| CC | 17 | Osc B pulse width: duty cycle in square mode (0–127 → 0–1, 64 = 50 %) |

**Hardware knobs (mirror CC 11–12):**

| Pin | Control |
|-----|---------|
| 15 | Glide |
| 16 | Release |

**Hardware button (mirrors CC 10):**

| Pin | Control |
|-----|---------|
| 7 | Poly mode toggle |

**Notes:**
- CC values 10–17 are scaled to `0.0–1.0` in firmware (`value / 127`).
- Osc A plays at pitch; osc B plays one octave below.
- Osc mix, waveforms, and pulse width (CC 13–17) are MIDI-only — no front-panel control.
- Pulse width only affects an oscillator when it is in square mode (CC 14 / 15 ≥ 64).
- Defaults at boot: osc A square, osc B saw, osc mix 0.7, pulse width 50 %, mono mode.

---

## Channel 8

*No assignments yet.*

---

## Channel 9

*No assignments yet.*

---

## Channel 10

*No assignments yet.*

---

## Channel 11

*No assignments yet.*

---

## Channel 12

*No assignments yet.*

---

## Channel 13

*No assignments yet.*

---

## Channel 14

*No assignments yet.*

---

## Channel 15

### MIDI LED Strip Controller (megaLedStrips)

Arduino Mega — controls 4 RGB LED strips (12 PWM outputs). [GitHub repository](https://github.com/alexiszbik/MIDI-LED-Strip-Controller).

| Message | Number | Effect |
|---|---|---|
| Note On / Note Off | 58 | Rainbow mode toggle (on while held, returns to Normal on release; clears colors on release) |
| Note On / Note Off | 59 | Explode mode toggle (on while held, returns to Normal on release; clears colors on release) |
| Note On / Note Off | 60 | All strips — RED (velocity controls brightness) |
| Note On / Note Off | 61 | All strips — GREEN |
| Note On / Note Off | 62 | All strips — BLUE |
| Note On / Note Off | 63 | All strips — WHITE (R+G+B simultaneously) |
| Note On / Note Off | 64–66 | Strip 1 — RED / GREEN / BLUE |
| Note On / Note Off | 67 | Strip 1 — WHITE (R+G+B simultaneously) |
| Note On / Note Off | 68–70 | Strip 2 — RED / GREEN / BLUE |
| Note On / Note Off | 71 | Strip 2 — WHITE |
| Note On / Note Off | 72–74 | Strip 3 — RED / GREEN / BLUE |
| Note On / Note Off | 75 | Strip 3 — WHITE |
| Note On / Note Off | 76–78 | Strip 4 — RED / GREEN / BLUE |
| Note On / Note Off | 79 | Strip 4 — WHITE |
| CC | 3 | Decay time: 0–127 → 1 ms – ~3001 ms (`1 + (value/127)² × 3000`) |
| CC | 4 | Rainbow speed: hue step interval in ms (`1 + value`) |
| CC | 5 | Explode level — reserved (handler present, no effect in current firmware) |
| CC | 6 | CC mode: value > 60 = manual CC override (notes ignored), value ≤ 60 = note mode |
| CC | 7 | Global brightness in CC mode: 0–127 → PWM level (`(value/127)² × 255`, 0–1 = off) |

**Notes:**
- Note velocity is scaled in firmware: `velocity ≤ 1 → 0`, else `velocity × 2 + 1` (max 255).
- In Normal mode, colors decay after note off according to CC 3.
- In CC mode, all LEDs (R+G+B on every strip) share the same level from CC 7; switching CC mode clears all outputs.
- Rainbow mode cycles hue across all strips; Explode mode randomly flashes one strip in white every ~40 ms.

---

## Channel 16

*No assignments yet.*
