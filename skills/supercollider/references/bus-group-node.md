# Bus, Group, and Node

Server-side routing primitives — understand these for any non-trivial audio routing.

## Node
A running server-side entity in the node tree. Most important types: `Synth` and `Group`. Controls execution order and lifecycle.

## Group
A container node that organizes other nodes. Use to:
- Separate source synths from effect synths
- Control execution order
- Free/inspect a whole section of your graph

## Bus
A signal path on the server. Use to:
- Send audio from source to effect
- Map a control bus to a synth parameter
- Avoid writing everything directly to hardware output

Two kinds: **audio buses** and **control buses**.

## Minimal Example

```supercollider
s.boot;

~fxBus = Bus.audio(s, 2);
~sources = Group.head(s);
~effects = Group.after(~sources);

SynthDef(\src, {
    Out.ar(\out.kr(0), PinkNoise.ar(0.05) ! 2);
}).add;

SynthDef(\fx, {
    var sig = In.ar(\in.kr(0), 2);
    Out.ar(\out.kr(0), LPF.ar(sig, 1200));
}).add;

Synth(\src, [\out, ~fxBus], ~sources);
Synth(\fx, [\in, ~fxBus, \out, 0], ~effects);
```

## Common Confusions

- A bus **carries** signals — it does not organize execution order
- A group **organizes** nodes — it does not carry audio
- A node **runs** — it does not carry signals between nodes
