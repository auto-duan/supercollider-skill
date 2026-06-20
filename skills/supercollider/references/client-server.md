# Client and Server

## The Fundamental Split

SuperCollider has two sides:
- **sclang** — the language client (variables, functions, patterns, control logic, OSC messaging)
- **scsynth** — the audio server (UGens, running synth nodes, buses, audio rendering)

You write code on the client side. The client sends messages to the server. The server runs synth graphs and produces audio.

## Why This Matters

Most mistakes come from mixing these responsibilities:
- Writing language logic as if it runs on the audio server
- Assuming `SynthDef` itself makes sound
- Forgetting that many operations require a booted server

## Minimal Example

```supercollider
s.boot;

SynthDef(\beep, { |freq = 440, amp = 0.1|
    Out.ar(0, SinOsc.ar(freq) * amp);
}).add;

x = Synth(\beep, [\freq, 660]);
x.free;
```

## Key Distinctions

- `SynthDef` defines a template; `Synth` creates a running node from that template on the server
- A `Bus` object on the language side references a server-side signal path — the signal itself exists on the server
- Patterns generate events in sclang; those events result in synth creation on the server
- Variables (`var`, `~envVar`) are language-side — they are not audio signals
