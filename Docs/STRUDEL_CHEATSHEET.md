# Strudel Cheatsheet

A compact reference for building slow, evolving, generative music in Strudel.

---

# 1. Design philosophy

When building a piece, think in **layers rather than notes**.

Each layer should have one clear job.

| Layer | Purpose |
|--------|---------|
| Pad | Atmosphere and harmony |
| Bass | Foundation and weight |
| Pulse | Rhythm and momentum |
| Texture | Interest and detail |
| Motion | Slow evolution |

If every layer tries to do everything, the mix quickly becomes muddy.

**Rule:** add one idea at a time.

---

# 2. Basic pattern

```javascript
$: note("<c4 e4 g4 d4>")
  .sound("gm_choir_aahs")
  .gain(0.4)
```

- `note(...)` defines pitches.
- Angle brackets (`<...>`) step through values over successive cycles.
- `.sound(...)` selects an instrument or sample.
- `.gain(...)` controls output level.

---

# 3. Current choir sketch

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

- `.attack("<2 6>".slow(16))` — alternates between fast and slow fade-ins.
- Long attacks can soften a sound because notes never fully reach maximum volume.
- `.release(...)` — controls fade-out and overlap.
- Longer releases build continuous clouds.
- `.lpf("<800 2000>".slow(32))` — slowly changes brightness.
- Lower values feel darker and further away.
- `.detune("<-5 5>".slow(48))` — gentle pitch drift and beating.
- `.room(0.9)` — reverb amount.
- `.size(0.98)` — virtual room size.
- `.gain(0.4)` — leaves headroom for more layers.

---

# 4. Envelope controls

```javascript
.attack(2)
.release(8)
```

Interactive versions:

```javascript
.attack(slider(2, 0.01, 8, 0.01))
.release(slider(8, 0.1, 20, 0.1))
```

### What they do

**Attack**

- Short → immediate
- Long → softer
- Very long → may never fully peak

**Release**

- Short → separate notes
- Long → continuous pad
- Very long → dense cloud

---

# 5. Parameters are patterns

One of Strudel's biggest ideas.

Nearly every parameter can become musical.

```javascript
.attack("<2 6>".slow(16))
.release("<8 12>".slow(24))
.lpf("<600 1800>".slow(32))
.gain("<0.25 0.4 0.3>".slow(48))
```

Instead of automating controls...

**the controls become part of the composition.**

---

# 6. Slow parameter movement

Any parameter can receive patterns.

```javascript
.lpf("<500 900 1800 1200>".slow(64))
```

Use different cycle lengths.

```javascript
.attack("<1 4>".slow(24))
.release("<5 9 12 7>".slow(40))
.lpf("<500 1800>".slow(64))
.detune("<-7 0 5 -2>".slow(96))
```

When every parameter evolves independently, the music keeps changing without obvious repetition.

---

# 7. Phasing

One of the most powerful minimalist techniques.

Instead of changing notes, duplicate a layer and let timing drift slightly.

Inspired by Steve Reich and heard throughout artists like GAS and The Field.

```javascript
stack(
  note("<c4 e4 g4 d4>")
    .sound("gm_choir_aahs"),

  note("<c4 e4 g4 d4>")
    .sound("gm_choir_aahs")
    .fast(1.003)
    .gain(0.25)
)
```

For percussion:

```javascript
sound("hh*16")
  .fast(1.002)
```

Tiny timing differences create evolving patterns that may take minutes to repeat.

### Guidelines

- 1.001–1.01 usually feels natural.
- Small values = gentle movement.
- Large values become separate rhythms rather than phasing.

---

# 8. Long cycles

Avoid everything repeating together.

Instead of:

```javascript
.attack("<2 6>".slow(8))
.release("<8 12>".slow(8))
.lpf("<500 2000>".slow(8))
```

Try:

```javascript
.attack("<2 6>".slow(16))
.release("<8 12>".slow(24))
.lpf("<500 2000>".slow(40))
.detune("<-5 5>".slow(56))
```

Nothing lines up.

Everything slowly evolves.

---

# 9. Useful ambient parameters

```javascript
.attack(...)
.release(...)
.lpf(...)
.detune(...)
.room(...)
.size(...)
.gain(...)
```

Useful additions:

```javascript
.pan(...)
.delay(...)
.delaytime(...)
.delayfeedback(...)
.phaser(...)
.vib(...)
```

Only add one new effect at a time so you can hear what changed.

---

# 10. Sound selection

General MIDI

```javascript
.sound("gm_choir_aahs")
.sound("gm_electric_piano_1")
.sound("gm_voice_oohs")
```

Oscillators

```javascript
.sound("sine")
.sound("sawtooth")
.sound("square")
```

Interesting sound sources

- Choirs
- Electric pianos
- Strings
- Pads
- Bells
- Filtered saws
- Field recordings
- Custom samples

---

# 11. Pattern timing

Slow an entire pattern

```javascript
note("<c4 e4 g4 d4>".slow(4))
```

Repeat

```javascript
note("<c4 e4 g4 d4>*2")
```

Rests

```javascript
note("<c4 ~ e4 ~ g4 ~ d4 ~>")
```

Global tempo

```javascript
setcps(0.25)
```

Slower CPS often changes the feel more than adding more notes.

---

# 12. Layering

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

### Layering rules

Every layer should have a distinct role.

Example:

**Pad**

- high
- wide
- slow

**Bass**

- low
- centred
- simple

**Pulse**

- short
- rhythmic

**Texture**

- quiet
- irregular

If two layers occupy the same space, ask whether both are needed.

---

# 13. Reusable helpers

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

Prefer naming layers by function:

```
pad1
pad2
bass
pulse
texture
```

rather than by instrument.

---

# 14. Visual feedback

### Piano roll

```javascript
._pianoroll({ labels: 1 })
```

### Background piano roll

```javascript
.pianoroll({ labels: 1 })
```

### Punchcard

```javascript
._punchcard()
```

### Oscilloscope

```javascript
._scope()
```

### Spectrum

```javascript
._spectrum()
```

### Spiral

```javascript
._spiral({ steady: 0.96 })
```

An underscore draws inside the code area.

Without it, visuals appear in the page background.

---

# 15. Suggested project structure

```text
BlueCalx/
├── sketches/
├── snippets/
├── pieces/
├── samples/
├── field-recordings/
├── renders/
├── STRUDEL_CHEATSHEET.md
└── IDEAS_AND_ROADMAP.md
```

---

# 16. Saving useful snippets

Instead of describing the code...

Describe **what you heard.**

Good notes:

```javascript
// Long attack softened the layer because notes never reached full volume.

// Slower CPS made the pulse feel embedded inside the pad.

// Lower LPF pushed the choir into the background.

// ±5 detune produced warmth without obvious pitch wobble.
```

Avoid comments that simply repeat the code.

---

# 17. Things I've learned

- Long attack can make sounds feel quieter because notes never fully peak.
- Release creates overlap more than sustain.
- Slow CPS often changes the atmosphere more than adding notes.
- Bass usually works best as a simple anchor.
- Tiny detuning creates warmth through beating.
- Most movement comes from changing parameters rather than adding notes.
- Different modulation lengths create continuous evolution.
- Phasing creates movement through tiny timing differences instead of new material.
- Every layer should have one clear purpose.
- Less is usually more.