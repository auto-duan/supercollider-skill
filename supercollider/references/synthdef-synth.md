# SynthDef and Synth

## SynthDef — the template

`SynthDef` defines a reusable synth graph. It is NOT the running sound. It's the blueprint.

```supercollider
SynthDef(\tone, { |freq = 440, amp = 0.1|
    Out.ar(0, SinOsc.ar(freq) * amp);
}).add;
```

- `.add` sends the definition to the default server
- Named arguments (`freq`, `amp`) become controllable parameters
- Build the graph from UGens (SinOsc, Saw, PinkNoise, etc.)

## Synth — the running instance

`Synth` creates an actual node on the server from a SynthDef.

```supercollider
x = Synth(\tone, [\freq, 660]);
x.set(\freq, 880);   // change parameter while running
x.free;               // stop it
```

## Lifecycle

1. Boot server: `s.boot;`
2. Define SynthDef: `SynthDef(\name, { ... }).add;`
3. Create instance: `x = Synth(\name, [\param, value]);`
4. Control: `x.set(\param, newValue);`
5. Stop: `x.free;`

## Common UGens

- **Oscillators**: `SinOsc`, `Saw`, `Pulse`, `PinkNoise`, `WhiteNoise`
- **Filters**: `LPF`, `HPF`, `BPF`, `RLPF`
- **Envelopes**: `EnvGen.kr(Env.adsr, gate)`
- **FX**: `FreeVerb`, `DelayC`, `CombC`

## Envelope Example

```supercollider
SynthDef(\pluck, { |freq = 440, amp = 0.1|
    var env = EnvGen.kr(Env.perc(0.01, 0.3), doneAction: 2);
    Out.ar(0, Saw.ar(freq) * env * amp);
}).add;

Synth(\pluck, [\freq, rrand(200, 800)]);
```

`doneAction: 2` frees the synth automatically when the envelope finishes.

## Common Errors

- Forgetting `.add` — server never receives the definition
- Treating SynthDef as running sound — need `Synth(\name)` to hear anything
- Not storing the Synth in a variable — can't free it later
- Forgetting `Out.ar` — the signal goes nowhere
