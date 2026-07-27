# Strudel Ambient Cheatsheet

A compact reference for slow, evolving, ambient-oriented Strudel sketches.

## 1. Basic pattern

```javascript
$: note("<c4 e4 g4 d4>")
  .sound("gm_choir_aahs")
  .gain(0.4)
```

- `note(...)` defines pitches.
- Angle brackets such as `<c4 e4 g4 d4>` play one item per cycle in sequence.
- `.sound(...)` selects the synth or sample instrument.
- `.gain(...)` controls output level.

## 2. Current choir sketch

```javascript
$: note("<c4 e4 g4 d4>")
  .sound("gm_choir_aahs")
  .attack("<2 6>".slow(16))
  .release(slider(10.3, 1, 16, 0.1))
  .lpf("<800 2000>".slow(32))
  .detune("<-5 5>".slow(48))
  .room(0.9)
  .size(0.98)
  .gain(0.4)
```

### What each line does

- `.attack("<2 6>".slow(16))`: alternates between a 2-unit and 6-unit fade-in over a long cycle. At the longer setting, notes may not reach full volume before the next event, making the result softer and less obviously modulated.
- `.release(slider(10.3, 1, 16, 0.1))`: sets fade-out and overlap. Longer values build a denser cloud.
- `.lpf("<800 2000>".slow(32))`: slowly changes brightness. Lower values sound darker and more distant.
- `.detune("<-5 5>".slow(48))`: introduces slight pitch movement and gentle beating.
- `.room(0.9)`: controls reverb amount.
- `.size(0.98)`: makes the simulated room very large.
- `.gain(0.4)`: reduces level and leaves space for future layers.

## 3. Envelope controls

```javascript
.attack(2)
.release(8)
```

Useful experiments:

```javascript
.attack(slider(2, 0.01, 8, 0.01))
.release(slider(8, 0.1, 20, 0.1))
```

- Short attack: immediate and defined.
- Long attack: softer, slower, and possibly quieter.
- Short release: separated events.
- Long release: overlapping pad or cloud.

## 4. Slow parameter movement

Any parameter can usually receive a pattern:

```javascript
.lpf("<500 900 1800 1200>".slow(64))
```

Long unrelated cycle lengths create gradual evolution:

```javascript
.attack("<1 4>".slow(24))
.release("<5 9 12 7>".slow(40))
.lpf("<500 1800>".slow(64))
.detune("<-7 0 5 -2>".slow(96))
```

## 5. Useful ambient parameters

```javascript
.attack(...)
.release(...)
.lpf(...)
.detune(...)
.room(...)
.size(...)
.gain(...)
```

Good later additions:

```javascript
.pan(...)
.delay(...)
.delaytime(...)
.delayfeedback(...)
.phaser(...)
.vib(...)
```

Add one new control at a time so its effect remains audible.

## 6. Sound selection

General MIDI sounds use full names:

```javascript
.sound("gm_choir_aahs")
.sound("gm_electric_piano_1")
```

Built-in oscillator examples:

```javascript
.sound("sine")
.sound("sawtooth")
.sound("square")
```

Promising sound families:

- choir
- electric piano
- strings
- pads
- bells and glassy FM-like sounds
- filtered saw waves
- field recordings and custom samples

## 7. Pattern timing

Slow the whole note sequence:

```javascript
note("<c4 e4 g4 d4>".slow(4))
```

Repeat within a cycle:

```javascript
note("<c4 e4 g4 d4>*2")
```

Add rests:

```javascript
note("<c4 ~ e4 ~ g4 ~ d4 ~>")
```

Set the overall cycle rate:

```javascript
setcps(0.25)
```

## 8. Layering

Use `stack(...)` to play patterns together:

```javascript
$: stack(
  note("<c4 e4 g4 d4>")
    .sound("gm_choir_aahs")
    .attack(3)
    .release(10)
    .lpf(1600)
    .room(0.9)
    .gain(0.3),

  note("<c2 ~ g1 ~>")
    .sound("sine")
    .attack(6)
    .release(14)
    .lpf(500)
    .room(0.7)
    .gain(0.12)
)
```

Layering rule:

- Give each layer a distinct role.
- Separate layers by pitch, brightness, movement, or stereo position.
- Start every new layer quietly.
- Remove anything that does not clearly improve the whole.

## 9. Reusable helpers inside one sketch

```javascript
const spacious = p => p
  .room(0.9)
  .size(0.98)

const breathing = p => p
  .attack("<2 6>".slow(16))
  .release(10)
  .lpf("<800 2000>".slow(32))

const choir = breathing(
  spacious(
    note("<c4 e4 g4 d4>")
      .sound("gm_choir_aahs")
  )
)

$: choir.gain(0.4)
```

## 10. Visual feedback

### Inline piano roll

```javascript
$: note("<c4 e4 g4 d4>")
  .sound("gm_choir_aahs")
  ._pianoroll({ labels: 1 })
```

### Background piano roll

```javascript
$: note("<c4 e4 g4 d4>")
  .sound("gm_choir_aahs")
  .pianoroll({ labels: 1 })
```

### Punchcard

```javascript
$: note("<c4 e4 g4 d4>")
  .sound("gm_choir_aahs")
  ._punchcard()
```

### Oscilloscope

```javascript
$: note("<c4 e4 g4 d4>")
  .sound("gm_choir_aahs")
  ._scope()
```

### Spectrum analyser

```javascript
$: note("<c4 e4 g4 d4>")
  .sound("gm_choir_aahs")
  ._spectrum()
```

### Spiral

```javascript
$: note("<c4 e4 g4 d4>")
  .sound("gm_choir_aahs")
  ._spiral({ steady: 0.96 })
```

An underscore places the visual inside the code area. Without the underscore, it is drawn in the page background.

## 11. Suggested file organisation

```text
strudel/
├── snippets/
│   └── choir-drift.js
├── pieces/
├── experiments/
├── renders/
├── STRUDEL_CHEATSHEET.md
└── IDEAS_AND_ROADMAP.md
```

## 12. Saving a useful snippet

```javascript
// Choir Drift v1
// Long attack makes the layer softer because events do not fully peak.
// Release around 8–12 creates a continuous cloud.
// ±5 detune gives gentle beating without obvious pitch instability.
```

Comments should record what you heard, not merely restate the code.
