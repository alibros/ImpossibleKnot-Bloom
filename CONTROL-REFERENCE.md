# BLOOM — control reference

Quick reference for the current 20 HP VCV Rack module.

## Main controls

| Control | Range | What it does |
|---|---:|---|
| **DECAY** | 0–1 | Sets the reverb tail length. Higher values make the plate ring longer. Use FREEZE for a true held tail. |
| **SIZE** | 0.25–4× | Scales the reverb tank. Larger values sound bigger and make the tail's pitch glide when changed while running. |
| **PREDELAY** | 0–500 ms | Delays the reverb relative to the dry input, keeping the source clearer. |
| **DIFFUSE** | 0–100% | Controls input diffusion. High values make a smoother wash; low values give a sharper, gated/reverse-like attack. |
| **LOW CUT** | 20–500 Hz | Removes low frequencies from the feedback tank. Raise it to reduce mud, especially with downward pitch shifts. |
| **TONE** | 500 Hz–16 kHz | Sets the tank's high-frequency damping. Lower values make the reverb darker and limit the number of shimmer octaves. |
| **SHIMMER** | 0–100% | Sets the amount of pitch-shifted feedback. At 0, BLOOM behaves as a plate reverb and ONSET/RISE have no audible shifted content to shape. |
| **ONSET** | 0–4 passes | Holds the shifted feedback back for a number of tank circulations before it enters. It scales with SIZE. |
| **RISE** | 0–8 s | Controls how gradually the shifted feedback grows after ONSET. At 0 it arrives immediately; longer values create a slow bloom. |
| **PITCH A** | −24 to +24 semitones | Sets the interval of shimmer voice A. +12 is one octave up; negative values create descending shimmer. |
| **PITCH B** | −24 to +24 semitones | Sets the interval of shimmer voice B. Use it with PITCH A for fifths, octaves, clusters, or contrasting motion. |
| **WEAVE** | A–B | Crossfades between pitch voices A and B. Centre blends them equally. |
| **GRAIN** | 20–150 ms | Sets the pitch-shifter window. Short values are grainier and more textured; long values are smoother. |
| **DRIFT** | 0–100% | Adds smooth-random movement to the reverb tank, reducing static metallic resonances. It is not a tempo-synced LFO. |
| **MIX** | 0–100% | Equal-power dry/wet balance. At 0 the output is dry; at 100 it is fully processed. |
| **FREEZE** | Push/latch | Holds the current reverb tail. The amber light shows when it is active. By default, pitch shifting is locked out while frozen so the held tail keeps its pitch. |

## CV attenuverters

The small bipolar controls above five CV jacks set the amount and polarity of
their corresponding CV input. Centre is zero; clockwise adds CV; counter-
clockwise inverts it.

| Attenuverter | Controls |
|---|---|
| **DECAY** | DECAY CV amount |
| **SIZE** | SIZE CV amount |
| **SHIMMER** | SHIMMER CV amount |
| **RISE** | RISE CV amount |
| **TONE** | TONE CV amount |

All ordinary CV inputs use ±5 V. `V/OCT` and `FRZ` are intentionally not
attenuverted.

## Audio jacks

| Jack | Type | What it does |
|---|---|---|
| **IN L** | Audio input | Left input. If IN R is empty, this mono signal is copied to both sides of the stereo reverb. |
| **IN R** | Audio input | Right input for stereo sources. Leave empty for mono-to-stereo operation. |
| **OUT L** | Audio output | Left processed output. |
| **OUT R** | Audio output | Right processed output. |

## CV and gate jacks

| Jack | Signal | What it does |
|---|---|---|
| **DECAY** | ±5 V CV | Modulates DECAY. Amount and polarity are set by the DECAY attenuverter. |
| **SIZE** | ±5 V CV | Modulates SIZE. The response is exponential, so voltage changes act as scale factors. Amount and polarity are set by the SIZE attenuverter. |
| **SHIMMER** | ±5 V CV | Modulates the amount of pitch-shifted feedback. Amount and polarity are set by the SHIMMER attenuverter. |
| **RISE** | ±5 V CV | Modulates the bloom time. The response is exponential across the useful range. Amount and polarity are set by the RISE attenuverter. |
| **TONE** | ±5 V CV | Modulates the tank's tone cutoff by octaves. Amount and polarity are set by the TONE attenuverter. |
| **V/OCT** | 1 V/oct | Tracks PITCH A. Each +1 V raises voice A by one octave; each −1 V lowers it by one octave. The input is clamped to the PITCH A range. |
| **FRZ** | Gate | Holds FREEZE while high. It turns on above 1 V and off below 0.1 V, so it can be driven by a sequencer or envelope gate. |

## Right-click options

- **Display** — choose Classic plate or Organic bloom.
- **Input while frozen** — mute, −20 dB, or unity input level.
- **Freeze locks out pitch shift** — keeps the held tail at its captured pitch
  when enabled.
- **Randomise grain window** — spreads pitch-shifter splice timing for a less
  obvious periodic grain artifact.
