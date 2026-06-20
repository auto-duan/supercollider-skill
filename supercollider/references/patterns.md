# Pattern Vocabulary

Most useful pattern families for everyday SuperCollider sequencing.

## Sequence Patterns

- `Pseq(list, repeats)` — play list in order: `Pseq([0, 2, 4, 7], inf)`
- `Pser(list, length)` — like Pseq but stops after N outputs
- `Pslide(list, repeats, len, step)` — overlapping windows across a list

## Random-Choice Patterns

- `Prand(list, repeats)` — choose randomly: `Prand([0, 2, 4, 7], inf)`
- `Pxrand(list, repeats)` — random without immediate repeat
- `Pshuf(list, repeats)` — shuffle whole list, output shuffled order
- `Pwrand(items, weights, repeats)` — weighted random: `Pwrand([\a, \b, \c], [0.6, 0.3, 0.1], inf)`

## Numeric-Generation Patterns

- `Pseries(start, step, length)` — arithmetic sequence
- `Pgeom(start, grow, length)` — geometric sequence

## Random-Number Patterns

- `Pwhite(lo, hi, repeats)` — uniform random range
- `Pexprand(lo, hi, repeats)` — exponential random (better for frequencies)
- `Pbrown(lo, hi, step, repeats)` — Brownian random walk

## Function-Driven Patterns

- `Pfunc({ ... })` — generate each value from a function
- `Prout({ ... })` — routine-like custom yield logic

## Minimal Example

```supercollider
Pbind(
    \degree, Prand([0, 2, 4, 7], inf),
    \dur, Pseq([0.25, 0.25, 0.5], inf),
    \amp, 0.1
).play;
```

## Tips

- Start from the simplest pattern class that expresses the behavior
- `Pexprand` and `Pbrown` often fit musical parameters better than `Pwhite`
- Patterns produce streams — use `.asStream` to inspect emitted values
