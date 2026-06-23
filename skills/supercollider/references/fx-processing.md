# FX & Signal Processing

Common effect UGens and how to use them, organized by type.

## Contents

- Filters
- Reverb
- Delay
- Distortion / saturation
- Pitch / frequency shifting
- Tremolo / ring modulation
- Combining effects
- Dry/wet mix pattern

## Filters

### Low-pass / High-pass

```supercollider
LPF.ar(sig, 800)       // low-pass, cutoff 800 Hz
HPF.ar(sig, 200)       // high-pass, cutoff 200 Hz
BPF.ar(sig, 1000, 0.3) // band-pass, center 1000 Hz, rq 0.3
```

`rq` is the reciprocal of Q — lower values = narrower band.

### Resonant filter with modulation

```supercollider
SynthDef(\filtSweep, { |freq = 110|
    var src = Saw.ar(freq) * 0.3;
    var cutoff = LFNoise1.kr(0.5).exprange(200, 4000);
    Out.ar(0, RLPF.ar(src, cutoff, 0.2) ! 2);
}).add;
```

## Reverb

### FreeVerb — simple built-in

```supercollider
FreeVerb.ar(sig, mix: 0.4, room: 0.8, damp: 0.5)
```

- `mix` — dry/wet (0 = dry, 1 = wet)
- `room` — room size
- `damp` — high-frequency damping

### GVerb — richer but heavier

```supercollider
GVerb.ar(sig[0], roomsize: 20, revtime: 3, damping: 0.5, drylevel: 0.7)
```

`GVerb` takes mono input — pass `sig[0]` if your source is stereo.

## Delay

### Simple delay

```supercollider
DelayN.ar(sig, 0.5, 0.3)   // max delay 0.5s, actual delay 0.3s
DelayL.ar(sig, 0.5, 0.3)   // linear interpolation (smoother pitch)
DelayC.ar(sig, 0.5, 0.3)   // cubic interpolation (best quality)
```

### Echo / feedback delay

```supercollider
SynthDef(\echo, { |in = 0, out = 0, dtime = 0.3, decay = 0.5|
    var sig = In.ar(in, 2);
    var echo = CombL.ar(sig, 1.0, dtime, decay);
    Out.ar(out, sig + echo);
}).add;
```

`CombL` is a comb filter — delay with feedback — useful for echo and pitch effects.

## Distortion / Saturation

```supercollider
sig.distort          // asymmetric soft clip
sig.softclip         // symmetric soft clip
sig.tanh             // tanh saturation
(sig * 8).clip2(1)   // hard clip with pre-gain
```

## Pitch / Frequency Shifting

```supercollider
PitchShift.ar(sig, 0.1, 2.0)   // octave up, window 0.1s
FreqShift.ar(sig, 200)          // shift all frequencies up 200 Hz (not musical)
```

`PitchShift` is musical (scales harmonics). `FreqShift` shifts absolutely (changes timbre).

## Tremolo / Ring Modulation

```supercollider
// Tremolo — amplitude modulation
sig * SinOsc.kr(6).range(0.5, 1.0)

// Ring modulation — multiply by audio-rate carrier
sig * SinOsc.ar(440)
```

## Combining Effects

Chain effects in a single SynthDef:

```supercollider
SynthDef(\chain, { |freq = 220, amp = 0.15|
    var env = EnvGen.kr(Env.perc(0.01, 1.5), doneAction: 2);
    var src = Saw.ar(freq) * env * amp;
    var filtered = LPF.ar(src, 1200);
    var verb = FreeVerb.ar(filtered, 0.4, 0.7);
    Out.ar(0, verb ! 2);
}).add;
```

Or use separate synths on buses for more flexibility — see `bus-group-node.md`.

## Dry/Wet Mix Pattern

```supercollider
var dry = sig;
var wet = FreeVerb.ar(sig, 1.0, 0.8);
var mix = 0.4;
XFade2.ar(dry, wet, mix * 2 - 1)  // XFade2 uses -1..1 range
// or simply:
dry + (wet * mix)
```
