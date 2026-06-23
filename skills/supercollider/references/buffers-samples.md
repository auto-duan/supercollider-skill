# Buffers & Samples

## Contents

- What buffers are
- Allocate and load
- Play back a buffer
- Scrub / random access
- Record into a buffer
- Common mistakes
- Wait for load before playing

## What Buffers Are

A `Buffer` is a server-side block of memory. Buffers hold audio data, wavetables, or arbitrary float arrays. They live on scsynth, not in sclang.

## Allocate and Load

### Empty buffer

```supercollider
~buf = Buffer.alloc(s, s.sampleRate * 2, 1);  // 2 seconds, mono
```

### Load a file from disk

```supercollider
~buf = Buffer.read(s, "/path/to/file.wav");
~buf = Buffer.readChannel(s, "/path/to/file.wav", channels: [0]);  // left channel only
```

### Check what you have

```supercollider
~buf.numFrames.postln;
~buf.numChannels.postln;
~buf.sampleRate.postln;
```

### Free when done

```supercollider
~buf.free;
```

## Play Back a Buffer

### PlayBuf — simplest playback

```supercollider
SynthDef(\player, { |buf = 0, rate = 1, amp = 0.5|
    var sig = PlayBuf.ar(1, buf, rate * BufRateScale.kr(buf), doneAction: 2);
    Out.ar(0, sig ! 2 * amp);
}).add;

Synth(\player, [\buf, ~buf]);
```

`BufRateScale.kr(buf)` corrects for sample rate differences between the file and the server — always include it.

### Loop

```supercollider
PlayBuf.ar(1, ~buf, loop: 1)
```

### Reverse

```supercollider
PlayBuf.ar(1, ~buf, -1 * BufRateScale.kr(~buf))
```

## Scrub / Random Access

### BufRd — read at a specific position

```supercollider
SynthDef(\scrub, { |buf = 0|
    var pos = LFNoise1.kr(2).range(0, BufFrames.kr(buf));
    var sig = BufRd.ar(1, buf, pos);
    Out.ar(0, sig ! 2 * 0.3);
}).add;

Synth(\scrub, [\buf, ~buf]);
```

`BufRd` lets you index into the buffer with any signal — great for granular-style and wavetable synthesis.

## Record Into a Buffer

```supercollider
~recBuf = Buffer.alloc(s, s.sampleRate * 5, 1);  // 5 seconds

SynthDef(\rec, { |buf = 0|
    RecordBuf.ar(SoundIn.ar(0), buf, loop: 0, doneAction: 2);
}).add;

~rec = Synth(\rec, [\buf, ~recBuf]);
// when done:
~recBuf.write("/tmp/recorded.wav", "wav", "int16");
```

## Common Mistakes

| Mistake | Fix |
|---|---|
| Not waiting for buffer to load | Wrap playback code in `s.sync` or a completion function |
| Missing `BufRateScale` | Always use it with `PlayBuf` to avoid pitch/speed errors |
| Buffer freed while synth still runs | Free synth first, then buffer |
| Mono buffer + 2-channel `PlayBuf.ar(2, ...)` | Match channel count to file |

## Wait for Load Before Playing

```supercollider
~buf = Buffer.read(s, "/path/to/file.wav", action: { |b|
    Synth(\player, [\buf, b]);
});
```

Or with `s.sync`:

```supercollider
s.waitForBoot({
    ~buf = Buffer.read(s, "/path/to/file.wav");
    s.sync;
    Synth(\player, [\buf, ~buf]);
});
```
