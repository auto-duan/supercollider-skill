# Debugging SuperCollider

## Workflow

1. Is the problem language-side or server-side?
2. Read the exact error text and receiver
3. Print/trace intermediate values
4. Inspect server state if audio is involved

## Language-Side Checks

```supercollider
x.postln;
postf("value: %\n", x);
obj.class.postln;
obj.isNil.postln;
```

## Server-Side Checks

```supercollider
s.meter;       // output levels
s.scope;       // oscilloscope
s.plotTree;    // node tree visualization
s.avgCPU.postln;
```

## Pattern Tracing

```supercollider
Pbind(\degree, Pseq([0, 2, 4], 1)).trace.play;
```

`.trace` prints each event as it fires.

## Reading Errors

Focus on:
- The **error text**
- The **receiver** (what object failed)
- The **first stack frame** pointing to your code

Example: `1.asdf;` → receiver is `Integer`, missing message is `asdf`.

## Common Failure Modes

| Symptom | Likely Cause |
|---|---|
| No sound | Server not booted, amp too low, wrong routing, wrong node order |
| `nil` does not understand X | Variable is nil — print it before use |
| Synth disappears immediately | Envelope with `doneAction: 2` finished, or no Out.ar |
| Sound won't stop | Didn't store synth in variable, can't call `.free` |
| Choppy/glitchy audio | CPU overload or buffer size too small |

## Tips

- Always `s.boot` before audio code
- Store running synths in variables
- Use `s.plotTree` to verify your node tree
- Use `.trace` on patterns to see events
- Wrap boot-dependent code in `s.waitForBoot({ ... })` if timing matters
