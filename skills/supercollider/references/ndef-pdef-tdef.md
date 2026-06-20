# Ndef, Pdef, Tdef — Live Coding Proxies

All three are named proxies that keep a stable identity while you swap the definition behind them.

## Ndef — audio/control signal proxy

```supercollider
Ndef(\tone, { |freq = 220| SinOsc.ar(freq) * 0.1 }).play;
Ndef(\tone).set(\freq, 330);
Ndef(\tone, { |freq = 220| Pulse.ar(freq, 0.4) * 0.08 });  // hot-swap while running
Ndef(\tone).stop;
```

Use for: live sound replacement, effect chains, signal routing.

## Pdef — pattern proxy

```supercollider
Pdef(\melody, Pbind(\degree, Pseq([0, 2, 4, 7], inf), \dur, 0.25));
Pdef(\melody).play;
Pdef(\melody, Pbind(\degree, Prand([0, 3, 5, 9], inf), \dur, 0.125));  // swap pattern
Pdef(\melody).stop;
```

Use for: live pattern replacement, sequencing.

## Tdef — task proxy

```supercollider
Tdef(\loop, {
    loop {
        "tick".postln;
        0.5.wait;
    }
});
Tdef(\loop).play;
Tdef(\loop, { loop { "tock".postln; 0.25.wait } });  // swap task
Tdef(\loop).stop;
```

Use for: timed routines, scheduled loops.

## Key Behaviors

- All three support **hot-swap**: redefine while playing without stopping
- All three keep a **stable name** — other code can depend on the name
- All three accumulate state quickly in live sessions — stop/clear when done
- Ndef is for **signals**, Pdef is for **patterns**, Tdef is for **tasks/timing**
