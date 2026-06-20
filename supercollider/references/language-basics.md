# Language Basics (sclang)

Minimum language-level facts for reading and writing SuperCollider code.

## Variables

```supercollider
var x = 1;
var a = 1, b = 2;
~envVar = 100;  // environment variable
```

## Basic Values

- Numbers: `1`, `2.5`
- Strings: `"hello"`
- Symbols: `\freq`, `SinOsc`
- Arrays: `[1, 2, 3]`
- Ranges: `(0..5)`, `(0,2..10)`

## Message Sending

```supercollider
100.sqrt;
"hello".postln;
[3, 1, 2].sort;
```

Almost everything in SC is message sending to objects.

## Functions

```supercollider
f = { |x, y| x + y };
f.value(1, 2);  // => 3
```

Functions are the main unit of reusable logic. Call with `.value()`.

## Collections

```supercollider
[1, 2, 3].collect { |x| x * 2 };  // => [ 2, 4, 6 ]
(0..5).do { |i| i.postln };
[1, 2, 3].choose;  // random element
```

## Operators

Most operators are syntactic sugar for message sends:
- `1 + 2` is `1.perform('+', 2)`
- `4 * 5`, `10 / 2`, `2 ** 8`

## Control Structures

```supercollider
if (x > 0) { "positive".postln } { "negative".postln };

3.do { |i| i.postln };

while { x < 10 } { x = x + 1 };

case
  { x == 0 } { "zero" }
  { x == 1 } { "one" }
  { "other" };
```

## Key Gotchas

- `~name` creates/reads an environment variable — useful for live coding
- Symbols (`\freq`) are not strings (`"freq"`)
- Language-side variables are NOT audio signals
- `.value` is how you call functions — not `f()`
