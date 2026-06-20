# Quarks

## What They Are

Quarks are installable SuperCollider packages. A quark may provide classes, extension methods, help files, or server plugins.

## Install

### Via GUI

```supercollider
Quarks.gui;
```

### By name

```supercollider
Quarks.install("SuperDirt");
Quarks.uninstall("SuperDirt");
```

### From a git URL

```supercollider
Quarks.install("https://github.com/user/quarkname.git");
```

### From a local folder

```supercollider
Quarks.addFolder("~/myquarks");
Quarks.install("myquarkname");
```

## Recompile After Install

```supercollider
thisProcess.recompile;
```

Any quark that adds classes or extension methods requires recompilation before use.

## Update All

```supercollider
Quarks.checkoutAll;
```

## Save / Load a Project Set

```supercollider
Quarks.save("quarks.txt");
Quarks.load("quarks.txt");
```

Useful for reproducible dependencies across machines.

## Quark Layout

```text
MyQuark/
├── MyQuark.quark   ← package metadata
├── Classes/
├── Help/
└── Extras/
```

## Common Errors

| Problem | Fix |
|---|---|
| New classes not found after install | Recompile with `thisProcess.recompile` |
| Quark not found by name | Check spelling; use GUI to browse available names |
| Works on one machine, fails on another | Save quark set to file and load it on the other machine |

## Commonly Used Quarks

- **SuperDirt** — sampler/FX engine used by TidalCycles
- **Vowel** — formant filter helpers
- **MKtl** — MIDI/HID controller abstraction
- **atk-sc3** — Ambisonic Toolkit for spatial audio
- **BatLib** — buffer/pattern utilities
