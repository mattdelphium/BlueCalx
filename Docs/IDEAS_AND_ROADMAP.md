# Strudel Ideas and Roadmap

A living list based on the current ambient/drone experiments and longer-term musical interests.

## Current direction

The strongest signal so far is that sound design is more satisfying than conventional drum sequencing.

Current interests:

- long, evolving textures
- slow modulation
- drones and overlapping releases
- subtle beating and detuning
- filters that change over minutes
- ambient territory around GAS, Deepchord, Burial and *Selected Ambient Works Volume II*
- generative systems that can be left playing in the background

The immediate goal does not need to be finishing songs. A better goal is to build a small personal library of textures that are enjoyable on their own.

## Next: layering

### Goal

Combine two simple layers without turning the sound into mush.

### First exercise

Keep the choir as the main layer and add a quiet low sine drone:

```javascript
$: stack(
  note("<c4 e4 g4 d4>")
    .sound("gm_choir_aahs")
    .attack("<2 6>".slow(16))
    .release(10.3)
    .lpf("<800 2000>".slow(32))
    .detune("<-5 5>".slow(48))
    .room(0.9)
    .size(0.98)
    .gain(0.35),

  note("<c2 ~ g1 ~>")
    .sound("sine")
    .attack(6)
    .release(14)
    .lpf(450)
    .room(0.7)
    .gain(0.1)
)
```

Listen for:

- Does the drone add weight or simply make the mix muddy?
- Can the choir still be heard clearly?
- Does the low layer need fewer notes?
- Would heavier filtering help?
- Does the second layer need different movement rather than more notes?

### Later layering exercises

1. Bright choir plus dark choir.
2. Choir plus low drone.
3. Sustained texture plus sparse bell.
4. Stable layer plus slowly detuned layer.
5. Wide reverb layer plus dry central layer.
6. Continuous pad plus a very occasional noisy event.

## Build a personal sound catalogue

Create one short file per promising sound:

```text
sounds/
├── choirs.md
├── electric-pianos.md
├── strings.md
├── pads.md
├── drones.md
├── noisy-textures.md
└── custom-samples.md
```

Record:

- exact `.sound(...)` name
- useful pitch range
- good attack and release ranges
- useful low-pass range
- whether detune helps
- whether the sample loops smoothly
- one-line emotional description

Example:

```markdown
## gm_choir_aahs

- Best range: roughly C3–G4
- Attack: 2–5
- Release: 8–12
- LPF: 700–2000
- Detune: subtle movement at ±5
- Character: distant human pad; becomes less vocal with long release
```

## Learn parameters by isolation

Create tiny studies:

- Attack study: keep everything fixed and move only attack.
- Release study: keep everything fixed and move only release.
- Filter study: use a constant note and slowly sweep the low-pass filter.
- Detune study: compare static and slowly patterned values.
- Reverb study: compare room amount and room size independently.

These can be saved even when they are not complete musical pieces.

## Long, independent modulation cycles

```javascript
.attack("<1 4>".slow(24))
.release("<6 11 8>".slow(35))
.lpf("<500 1800 900 2400>".slow(64))
.detune("<-6 2 5 -3>".slow(91))
```

Questions:

- Does every parameter need movement?
- Which modulation remains audible after several minutes?
- At what point does movement become distracting?
- Is the texture stronger with one stable anchor?

## Harmony without conventional composition pressure

Explore slow chord colour rather than melodies:

- one chord for several minutes
- two-chord oscillation
- suspended chords
- fifths with no third
- minor ninth colours
- bass note changing beneath a fixed upper texture
- very slow inversions

Later, explore Strudel chord and voicing tools after simple layers feel controlled.

## Rhythm without “making beats”

Rhythm can enter through texture:

- filter pulses
- gain breathing
- delayed echoes
- intermittent note omissions
- irregular noise events
- slow tremolo
- one very quiet kick every few cycles
- environmental samples at sparse intervals

## Visual feedback

Useful visual experiments:

1. `_pianoroll({ labels: 1 })` to understand note order.
2. `_punchcard()` to see transformed events.
3. `_scope()` to observe waveform shape and beating.
4. `_spectrum()` to watch filter movement.
5. `_spiral({ steady: 0.96 })` for a playful view of cycles.
6. Hydra later, once the audio workflow feels settled.

Use visuals to deepen listening rather than turning them into a separate project immediately.

## Custom samples

Longer-term material:

- room tone
- household hum
- distant traffic
- wind and rain
- metal resonance
- short voice fragments
- toy instruments
- scraped surfaces
- bass guitar harmonics
- recordings played back at different rates

Workflow:

1. Record a short sound.
2. Import it into Strudel.
3. Slow or transpose it.
4. Filter it.
5. Apply long release and reverb.
6. Layer it below a cleaner tonal sound.

## Arrangement and change over time

### Fade-in form

- drone begins alone
- choir enters
- brighter layer appears
- everything gradually darkens
- one layer remains

### Density form

- sparse events
- increasing overlap
- maximum cloud
- sudden reduction
- slow decay

### Register form

- low drone
- mid choir
- high glassy tone
- high layer disappears
- drone resolves or fades

## Performance and interaction

Keep sliders for parameters that are satisfying to move by hand:

```javascript
.release(slider(10, 1, 16, 0.1))
.lpf(slider(1200, 200, 4000, 10))
.gain(slider(0.35, 0, 0.8, 0.01))
```

Later questions:

- Which parameters should be automated?
- Which are more enjoyable as live controls?
- Can one slider create a meaningful transition?
- Would a MIDI controller make the process more tactile?

## Recording and preserving results

For anything worth returning to, save:

1. The code.
2. A short audio render.
3. A few lines describing what worked.
4. The date or version.

Example:

```text
choir-drift-v1.js
choir-drift-v1.wav
choir-drift-v1.md
```

## Possible longer-term projects

### Personal ambient instrument

A reusable collection of layers and sliders that can be left running and manually shaped.

### Generative background radio

Several related textures drifting between states over long periods.

### GAS-inspired forest loop

Filtered orchestral or choir material, distant pulse, slow brightness movement and environmental noise.

### Deepchord-inspired space

Muted chord stabs, long delay tails, tape-like noise and slow stereo movement.

### SAW II-style isolated studies

One unusual sound source per piece, minimal harmony and strong atmosphere.

### Strudel plus bass guitar

Record harmonics, muted notes or short phrases and transform them into drones and textures.

### Audio-reactive visuals

Use Hydra after the music system is enjoyable without visuals.

## Suggested learning order

### Now

- annotate and save the choir snippet
- test visualisers
- add one low drone layer
- practise balancing layer gain

### Soon

- catalogue useful GM sounds
- isolate envelope, filter and detune behaviours
- try three-layer textures
- add sparse events
- record short renders

### Later

- custom samples
- chord voicings
- delays and phasers
- MIDI control
- Hydra visuals
- a custom Strudel setup only when the browser REPL becomes a genuine limitation

## Guiding principle

Prefer a small number of sounds that evolve beautifully over a large number of events.

A sketch is successful when it remains pleasant or intriguing after being left running for several minutes.
