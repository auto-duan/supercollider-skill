# Recipes — Runnable Examples

Common tasks with copy-paste-ready code.

## Contents

- Minimal sine
- Custom SynthDef with envelope
- Simple Pbind (melody)
- Bus effect chain
- MIDI input
- OSC receiver
- Offline render (NRT)
- GUI synth control
- Live code with Ndef

## Minimal Sine

```supercollider
s.boot;
{ SinOsc.ar(440, 0, 0.1) }.play;
```

## Custom SynthDef with Envelope

```supercollider
SynthDef(\pluck, { |freq = 440, amp = 0.1|
    var env = EnvGen.kr(Env.perc(0.01, 0.3), doneAction: 2);
    Out.ar(0, Saw.ar(freq) * env * amp);
}).add;

Synth(\pluck, [\freq, rrand(200, 800)]);
```

## Simple Pbind (melody)

```supercollider
Pbind(
    \instrument, \pluck,
    \degree, Pseq([0, 2, 4, 7, 9, 7, 4, 2], inf),
    \dur, Pseq([0.25, 0.25, 0.5], inf),
    \amp, 0.1
).play;
```

## Bus Effect Chain

```supercollider
~fxBus = Bus.audio(s, 2);
~sources = Group.head(s);
~effects = Group.after(~sources);

SynthDef(\src, { Out.ar(\out.kr(0), PinkNoise.ar(0.05) ! 2) }).add;
SynthDef(\fx, { var sig = In.ar(\in.kr(0), 2); Out.ar(\out.kr(0), FreeVerb.ar(sig, 0.5, 0.8)) }).add;

Synth(\src, [\out, ~fxBus], ~sources);
Synth(\fx, [\in, ~fxBus, \out, 0], ~effects);
```

## MIDI Input

```supercollider
MIDIIn.connectAll;

MIDIdef.noteOn({ |vel, note, chan, src|
    Synth(\pluck, [\freq, note.midicps, \amp, vel / 127]);
});
```

## OSC Receiver

Listen for incoming OSC messages at an address pattern:

```supercollider
// One-off responder
OSCFunc({ |msg| msg.postln }, '/test');

// Named/replaceable responder (preferred for live coding)
OSCdef(\listen, { |msg| msg.postln }, '/test');
OSCdef(\listen).free;  // remove it
```

Send OSC to another host with `NetAddr`:

```supercollider
~target = NetAddr("127.0.0.1", 57120);
~target.sendMsg('/test', 1, 2, 3);
```

## Offline Render (NRT)

```supercollider
// Render to disk without realtime
Score.writeNRT(
    "input.txt",
    "output.aiff"
);
```

For full NRT workflow, see SuperCollider docs on `NonRealTime` and `Score`.

## GUI Synth Control

```supercollider
s.boot;
Ndef(\pad, { |freq = 220, cutoff = 1000|
    var sig = Saw.ar(freq) ! 2;
    LPF.ar(sig, cutoff) * 0.1
}).play;

Ndef(\pad).play;

// Open GUI controls
Ndef(\pad).gui;
```

## Live Code with Ndef

```supercollider
Ndef(\tone, { SinOsc.ar(220) * 0.1 }).play;

// Swap while playing — audio continues uninterrupted
Ndef(\tone, { Pulse.ar(220, 0.5) * 0.08 });

// Modulate parameters
Ndef(\tone).set(\freq, 440);

// Clean up
Ndef(\tone).stop;
Ndef(\tone).clear;
```
