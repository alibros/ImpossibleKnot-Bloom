# BLOOM — user manual

A shimmer reverb for VCV Rack 2, from Impossible Knot. The current build is
20 HP and requires VCV Rack 2.4 or later.

A shimmer reverb feeds a pitch-shifted copy of its own tail back into itself,
so the reverb keeps climbing as it decays. BLOOM's particular concern is
**when that shifted content arrives, and how fast it builds** — the difference
between a harmoniser bolted to a reverb and something that opens.

The panel calls the swell control **RISE**. In the DSP and code it is also
referred to as bloom time; these are the same control.

## At a glance

| Section | Controls | What it shapes |
|---|---|---|
| SPACE | DECAY, SIZE, PREDELAY, DIFFUSE, LOW CUT, TONE | Room length, diffusion, and spectral damping |
| SHIMMER | SHIMMER, PITCH A, PITCH B, WEAVE, GRAIN, DRIFT | Harmonic feedback and movement |
| UNFOLD | ONSET, RISE | When the shimmer begins and how quickly it grows |
| OUTPUT | FREEZE, MIX | Tail capture and dry/wet level |

The factory defaults are intentionally demonstrative: `DECAY` 0.88,
`SHIMMER` 60%, `PITCH A` +12, `PITCH B` +19, `ONSET` 1.5 passes, and `RISE`
4 seconds.

---

## Quick start

Patch audio into **IN L** (mono is fine — it feeds both channels), take
**OUT L / OUT R**, and turn **MIX** up.

The factory defaults are set to show what the module is for rather than to be
neutral: a long decay, a fair amount of shimmer, and a slow rise. Play a chord
and let it ring for ten seconds before judging anything. BLOOM is not a control
you hear immediately, by design.

If you want the sound most reverbs give you, set **RISE** and **ONSET** to
zero. The shifted content then enters as soon as the tank can supply it.

---

## The signature pair: ONSET and RISE

These sit together under **UNFOLD**, and they are the reason this module
exists. Both shape *the arrival of the shifted content*, not its amount.

**ONSET** — dead time before any shifted content enters at all, from 0 to 4
tank passes. It is measured in tank circulations rather than seconds, so it
scales with `SIZE`: a bigger space takes proportionally longer to bloom.

**RISE** — how gradually the shifted content swells in once it starts, from 0
to 8 seconds. At 0 it arrives immediately. At 4–8 seconds it emerges so slowly
that the reverb appears to be growing rather than decaying.

The two are independent and it's worth hearing them separately:

- **ONSET alone**, RISE at 0 → clean reverb for a moment, then the octave
  steps in. This is what a fixed pitch pre-delay does.
- **RISE alone**, ONSET at 0 → the octave starts immediately but takes seconds
  to reach full strength.
- **Both** → the tail stays clean, then opens gradually. This is the sound the
  module is named for.

### One bloom per phrase

The bloom re-arms when you *stop playing* for about a second, and fires again
on your next phrase. It deliberately does **not** retrigger on every note —
that would make pads stutter — and it deliberately does not cut the octave out
of a tail that is still ringing when you stop.

So: play a phrase, let it ring, pause, play again. Each new phrase blooms from
scratch. If you play continuously the bloom happens once, at the start, and
the tail then behaves like a conventional shimmer. That is intended.

The display shows this: watch the front sweep outward, then reset when you
start a new phrase.

---

## SPACE

The reverb itself — a Dattorro figure-of-eight plate.

**DECAY** — tail length. Above about 0.85 the tail lives long enough for a
slow RISE to develop inside it, which is why the default sits there. Below
that, a long RISE will still be climbing when the tail has gone.

**SIZE** — scales the whole tank, 0.25× to 4×. Changing it while audio is
running bends the pitch of the tail, which is musical and intended. A full
sweep takes about a second and a half; that glide is deliberate.

**PREDELAY** — delay before the reverb starts, 0–500 ms. Keeps the source
distinct from its own tail.

**DIFFUSE** — how much the input is smeared before it enters the tank. At 1 it
is a smooth wash. At 0 the input diffusers are bypassed entirely, which gives
a gated, almost reverse-sounding attack.

**LOW CUT** — high-pass inside the tank, 20–500 Hz. Essential when PITCH is
negative, or the down-shifted content piles up into mud.

**TONE** — low-pass inside the tank, 500 Hz–16 kHz. **The most important
control for shimmer character**: it decides how many octaves the cascade
climbs before it runs out of top end and dies. Low TONE gives a dark reverb
that shimmers once and settles; high TONE lets it keep climbing.

---

## VOICES

Two independent pitch-shift voices read the tank's own delay lines, so the
shimmer costs almost no extra memory.

**PITCH A** and **PITCH B** — the interval of each voice, −24 to +24
semitones. +12 is the classic octave-up shimmer; +19 (an octave and a fifth)
adds sparkle; negative values give a descending, subterranean version that
needs LOW CUT to stay clean.

**WEAVE** — the balance between the two voices. Centre is both equally.

**GRAIN** — the pitch-shifter's window length, 20–150 ms. Short is grainy and
choral; long is smooth and pad-like. There is no correct setting.

**DRIFT** — movement inside the tank. Not an LFO: it is smooth-random, so
there is no rate for the ear to lock onto. At 0 the tail can ring metallically;
this is what stops that. It is not an optional flourish.

**SHIMMER** — how much shifted content is fed back. This is the module's main
axis: at 0 it is a plate reverb, at 1 the tail is almost entirely climbing
octaves. Note that **ONSET and RISE do nothing at SHIMMER 0** — they shape the
arrival of something that isn't there.

---

## FREEZE, MIX

**FREEZE** — holds the tail indefinitely. Decay goes to exactly 1.0 *and* the
damping filters are crossfaded out of the loop, because decay alone cannot
hold a tail: the filters bleed enough per circulation to lose it over minutes.

By default the shifters are locked out while frozen, so a held tail keeps the
pitch it was frozen at. Without that, frozen content transposes up on every
circulation and climbs out of the passband in a few seconds. Turn it off from
the right-click menu if you want that effect — it is dramatic, and it is
not what "freeze" normally means.

**MIX** — dry/wet, equal power.

---

## CV and connections

Audio is nominally ±5 V at the jacks. Patch mono audio to **IN L** and it is
copied to both tank channels; patch **IN R** as well for stereo input. Take
**OUT L** and **OUT R** as the stereo result.

The modulation inputs accept ±5 V. **DECAY**, **SIZE**, **RISE**, **SHIMMER**
and **TONE** have bipolar attenuverters directly above them: at centre the
input does nothing, and turning either way scales or inverts it.

The CV inputs are:

- **DECAY** — varies tail length.
- **SIZE** — varies tank size exponentially, so CV moves by musical scale
  factors rather than fixed linear amounts.
- **SHIMMER** — varies the amount of shifted feedback.
- **RISE** — varies the bloom time exponentially across its useful range.
- **TONE** — shifts the tank's low-pass cutoff by octaves.

**V/OCT** tracks **PITCH A** at 1 V per octave, so a quantizer or sequencer can
play the shimmer interval. It has no attenuverter on purpose — a 1 V/oct input
that needs trimming is not 1 V/oct.

**FRZ** holds freeze while the gate is high: it turns on above 1 V and turns
off below 0.1 V. A sequencer can therefore freeze for an exact number of
beats. It has no attenuverter because a gate either crosses its threshold or
it does not.

The module is stereo, not polyphonic. If a polyphonic cable is patched, Rack's
input voltage sum is used for that port.

---

## The display

The window is a living celestial garden. Two pitch rosettes interweave
into the central mandala, a branching canopy and root system spread to either
side, open vapor filaments drift behind them, and the branch tips join into
stereo constellations. Audio travels through and illuminates that structure.
Once both the input and tail are silent, the surrounding ecosystem dissolves
and leaves only the central flower turning almost imperceptibly.

The input softly kindles the central seed and the existing organism brightens;
it does not emit a separate particle burst or hard circular ripple.
The measured left and right tail levels light the corresponding canopy, while
**WEAVE** transfers both sonic and visual emphasis between them. **PITCH A**
and **PITCH B** set the petal counts of the interwoven mandalas and their two
satellite blooms. Every filled membrane, vein, and lace contour cross-fades
between adjacent petal counts, and its rotation has continuous phase even when
a pitch crosses zero. **RISE** paces a travelling unfurl wave along the canopy:
leaves open and brighten as it passes, while the main flower keeps its own
shape. Short settings travel faster and long settings unfold slowly, but the
visual range is deliberately restrained so low RISE never becomes frantic.
Both the unfurl wave and the sap lights retain their accumulated phase, so a
RISE gesture changes velocity without teleporting anything along a branch.

The rest of the material follows the sound just as directly. **SIZE** changes
the reach and scale of the whole garden. **DIFFUSE** smoothly grows fixed
branch, root, timeline, and vapor layers rather than snapping between integer
topologies. **DECAY** adds persistent botanical after-images.
**TONE** moves the ecosystem from cool teal to warm gold. **LOW CUT** shortens
and removes the roots. **GRAIN** moves the surrounding field from many fine
spores to fewer large stars, and **DRIFT** bends, rotates, and breathes the
geometry in independently phased, restrained vertical motions. **PREDELAY**
widens the negative space around the seed and extends paired open tendrils;
**ONSET** grows a clearly visible stack of unshifted sepals; and **SHIMMER**
owns the central bud-to-bloom scale as well as leaves and satellite flowers.
At maximum SHIMMER the focal flower becomes the dominant form in the window.
FREEZE stops the organism and gives it a faint crystalline cast.

The Organic view is never perfectly static. Five close parallel histories
split from and rejoin each main bough, evoking several timelines sharing the
same origin and destination. Cloud cells deform, sap lights drift along the
veins, spores float, and the nested mandalas turn at slightly different rates.
The sap lights take roughly half a minute or longer to cross the display and
spend most of their endpoints in a long eased fade, so they never pop at a
loop boundary. This quiet motion continues between notes so the display feels
alive, while live input and tail energy make the same movements brighter.
**DRIFT** greatly expands their speed and depth; even at zero there is a very
slow baseline breath. FREEZE stops this ambient clock as well as the reverb.

Visual parameters are interpolated at display rate, so large knob movements
grow into their new state instead of snapping. The mappings deliberately use
the full travel: SIZE spans a compact seedling to an edge-filling garden,
DIFFUSE moves continuously from a sparse stem to several recursive branch and
vapor layers, PITCH spans three- to fifteen-fold lace, SHIMMER moves from a
tight central bud to an edge-filling flower, ONSET grows four distinct calyx
whorls, and GRAIN grows from fine spore dust into a denser field of modest
luminous grains.

---

## Right-click menu

- **Snap PITCH A/B to semitones** — on by default, so both pitch knobs move in
  exact one-semitone steps. Disable it for continuous tuning; enabling it again
  rounds both current values to the nearest semitone.
- **Input while frozen** — mute (default), −20 dB, or unity. Unity lets you
  keep playing into a frozen tail, which grows without bound if you are not
  careful.
- **Freeze locks out pitch shift** — on by default; see FREEZE above.
- **Randomise grain window** — on by default. Smears the pitch-shifter's splice
  rate into a band instead of a tone. Leave it on unless you want the artefact.
---

## Patch ideas

**Ambient guitar.** Audio interface → BLOOM. DECAY 0.9, SIZE 2, TONE 6 kHz,
SHIMMER 0.6, PITCH A +12, ONSET 1.5, RISE 6 s, MIX 60%. Play a chord and stop.
The reverb keeps opening for ten seconds after you do.

**Played shimmer.** Sequencer → V/OCT. The interval changes per step, so the
tail harmonises with what you are playing rather than sitting a fixed octave
above it.

**Frozen chord pads.** Play a chord, gate FRZ high, then keep playing over the
held tail with the input set to mute. Release the gate to let it decay.

**Reverse-ish swell.** DIFFUSE to 0, PREDELAY around 200 ms, RISE long. The
bypassed diffusers give a gated attack that the slow bloom then opens out of.

**Falling shimmer.** PITCH A −12, LOW CUT up around 200 Hz. Descending
shimmer, which is a much rarer sound than the ascending kind and worth
exploring.

**CV phrase opener.** Patch a slow envelope or LFO into RISE, turn its
attenuverter slightly clockwise, and keep ONSET above zero. The reverb can
open at a different rate on each phrase without changing the basic room.

**V/OCT harmoniser.** Patch a quantizer or sequencer to V/OCT and use PITCH A
as the interval centre. Keep PITCH B fixed or set it to a nearby fifth so the
tail follows the played sequence without needing a separate pitch shifter.

---

## Notes

CPU is around 0.026 µs per sample, roughly 0.13% of one core at 48 kHz.

Bypass passes the dry signal through rather than muting, so a bypassed BLOOM
is a wire.

There is no separate clean-tail/shimmer-tail output balance in the current
build. Use `SHIMMER`, `MIX`, and the two arrival controls to manage the
relationship between the dry plate character and the harmonic tail.

The module is 24 HP. BLOOM is closed-source freeware: it is free to install and
use, including in commercial music, under the bundled proprietary licence.
Product information, updates, and this manual are available from the
[public BLOOM page](https://github.com/alibros/ImpossibleKnot-Bloom). Support:
hello@impossible-knot.com.
