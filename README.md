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

*No assignments yet.*

---

## Channel 16

*No assignments yet.*
