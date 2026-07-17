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

Mono multi-effect (HPF, bit crusher, frequency shifter, stutter gate, input metering). [GitHub repository](https://github.com/alexiszbik/multifx2).

| Message | Number | Effect |
|---|---|---|
| CC | 10 | HPF cutoff |
| CC | 11 | HPF resonance |
| CC | 12 | Bit crusher rate |
| CC | 13 | Frequency shifter frequency |
| CC | 14 | Frequency shifter dry/wet |
| CC | 80 | Global mute: value > 60 = muted, value <= 60 = unmuted (smooth fade) |

---

## Channel 7

### VocoderSynth

Carrier synth for vocoder pedal. [GitHub repository](https://github.com/alexiszbik/VocoderSynth).

| Message | Number | Effect |
|---|---|---|
| Note On | — | Triggers a voice (mono: retrigger / last-note priority; poly: up to 4 voices) |
| Note Off | — | Releases the matching note |
| CC | 10 | **Play Mode**: 0 = Mono, 127 = Poly (linear 0–1) |
| CC | 11 | **Glide**: 0–127 (linear 0–1, glide time = glide² × sample rate) |
| CC | 12 | **Release**: 0–127 → 5 ms–8 s (pow³ curve) |

**Fixed envelope** (not CC-mapped): Attack 0.02 s, Decay 0.02 s, Sustain 1.0.


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
