---
name: supercollider
description: "Write, debug, and reason about SuperCollider (sclang/scsynth) code. Use when the user works with SuperCollider synthesis, patterns, live coding, routing, MIDI/OSC, or audio rendering."
---

# SuperCollider

Use this skill when writing or debugging SuperCollider code. Covers synthesis (SynthDef, UGens), sequencing (Patterns, Pbind), live coding (Ndef/Pdef/Tdef), routing (buses, groups), and I/O (MIDI, OSC, offline render).

## Mental Model (critical)

SuperCollider has two sides — **always know which side you're on**:

- **sclang** (client): variables, functions, patterns, control logic, OSC messaging
- **scsynth** (server): UGens, synth nodes, buses, audio rendering

Most beginner/AI mistakes come from mixing these. See `references/client-server.md`.

## Quick Start Patterns

### First sound

```supercollider
s.boot;
{ SinOsc.ar(440, 0, 0.1) }.play;
```

### Define an instrument

```supercollider
SynthDef(\tone, { |freq = 440, amp = 0.1|
    Out.ar(0, SinOsc.ar(freq) * amp);
}).add;

x = Synth(\tone, [\freq, 660]);
x.free;
```

### Play a pattern

```supercollider
Pbind(
    \degree, Prand([0, 2, 4, 7], inf),
    \dur, Pseq([0.25, 0.25, 0.5], inf),
    \amp, 0.1
).play;
```

### Live code with Ndef

```supercollider
Ndef(\tone, { |freq = 220| SinOsc.ar(freq) * 0.1 }).play;
Ndef(\tone).set(\freq, 330);
Ndef(\tone, { |freq = 220| Pulse.ar(freq, 0.4) * 0.08 });  // hot-swap
Ndef(\tone).stop;
```

### Effect chain with bus

```supercollider
~fxBus = Bus.audio(s, 2);
~sources = Group.head(s);
~effects = Group.after(~sources);

SynthDef(\src, { Out.ar(\out.kr(0), PinkNoise.ar(0.05) ! 2) }).add;
SynthDef(\fx, { var sig = In.ar(\in.kr(0), 2); Out.ar(\out.kr(0), LPF.ar(sig, 1200)) }).add;

Synth(\src, [\out, ~fxBus], ~sources);
Synth(\fx, [\in, ~fxBus, \out, 0], ~effects);
```

## Common Mistakes (avoid these)

- **Treating SynthDef as a running synth** — it's only a template; use `Synth(\name)` to hear sound
- **Forgetting `s.boot`** — server must be running before any audio
- **Forgetting `.add`** on SynthDef — without it the server never receives the definition
- **Confusing bus vs group** — buses carry signals, groups organize node order
- **Writing language logic as server code** — variables/functions live in sclang, not on the audio server
- **Not freeing nodes** — store synths in variables and call `.free`

## Code Conventions

- Use minimal, runnable examples — one concept at a time
- Use simple names: `freq`, `amp`, `src`, `fx`, `bus`
- Always state expected output
- Boot server (`s.boot`) before audio examples
- Prefer `Pseq`/`Prand` over complex patterns when simple logic suffices
- Use `Pexprand`/`Pbrown` for musical parameters over flat `Pwhite`

## Debugging

- Language-side: `x.postln;`, `obj.class.postln;`, `obj.isNil.postln;`
- Server-side: `s.meter;`, `s.scope;`, `s.plotTree;`, `s.avgCPU.postln;`
- Pattern tracing: `Pbind(...).trace.play;`
- Read error text → identify receiver → find first frame pointing to your code

## References

Load a reference file when the task calls for it. Each is self-contained — read the whole file, don't skim.

- **Client/server split** → read [references/client-server.md](references/client-server.md) when reasoning about what runs in sclang vs scsynth, or debugging a "wrong side" error.
- **sclang syntax** → read [references/language-basics.md](references/language-basics.md) for variables, functions, messages, collections.
- **Instruments** → read [references/synthdef-synth.md](references/synthdef-synth.md) when defining SynthDefs, instantiating/controlling/freeing Synths, or choosing UGens.
- **Routing** → read [references/bus-group-node.md](references/bus-group-node.md) when wiring buses, groups, or node execution order.
- **Sequencing** → read [references/patterns.md](references/patterns.md) for pattern families (Pseq, Prand, Pwhite, Pexprand, etc.).
- **Live coding** → read [references/ndef-pdef-tdef.md](references/ndef-pdef-tdef.md) when hot-swapping sound (Ndef), patterns (Pdef), or tasks (Tdef).
- **Samples** → read [references/buffers-samples.md](references/buffers-samples.md) for Buffer alloc/load, PlayBuf, BufRd, recording.
- **Effects** → read [references/fx-processing.md](references/fx-processing.md) for filters, reverb, delay, distortion, pitch shift.
- **Packages** → read [references/quarks.md](references/quarks.md) when installing or managing Quarks.
- **Ready-made code** → read [references/recipes.md](references/recipes.md) for runnable examples (incl. MIDI input, OSC, offline render).
- **Debugging** → read [references/debugging.md](references/debugging.md) for the language- vs server-side workflow and tools.
