# SuperCollider Agent Skill

A ready-to-install **Agent Skill** that teaches AI coding assistants how to write,
debug, and reason about [SuperCollider](https://supercollider.github.io/) — the
real-time audio synthesis and algorithmic-composition language.

Works with Claude Code, Cursor, OpenCode, Codex, and any other tool that supports
the open [Agent Skills](https://github.com/vercel-labs/skills) format.

---

## Table of contents

- [What is this?](#what-is-this)
- [Why use it?](#why-use-it)
- [Prerequisites](#prerequisites)
- [Install](#install)
- [How it works](#how-it-works)
- [What the skill covers](#what-the-skill-covers)
- [Quick taste](#quick-taste)
- [Repository layout](#repository-layout)
- [Try it](#try-it)
- [Source](#source)
- [Contributing](#contributing)
- [License](#license)

## What is this?

An **Agent Skill** is a small, self-contained bundle of expert knowledge you drop
into your AI coding tool. Once installed, the agent automatically pulls it in
whenever you start a relevant task — you don't have to paste docs or re-explain
anything.

This skill gives your agent a solid grounding in SuperCollider so it stops making
the classic mistakes (confusing the language with the audio server, forgetting to
boot the server, treating a `SynthDef` as if it already makes sound) and instead
writes idiomatic, runnable code.

## Why use it?

SuperCollider has a steep learning curve and a **split architecture** — a
scripting language (`sclang`) *and* a separate audio server (`scsynth`) — that
trips up both humans and LLMs. Out of the box, an AI assistant tends to produce
code that looks plausible but won't actually run.

This skill fixes that by packing in:

- **The core mental model** — the client/server split that underlies almost every
  beginner error
- **Quick-reference patterns** — copy-pasteable snippets for synthesis,
  sequencing, live coding, and signal routing
- **A debugging workflow** — language-side and server-side tools, plus how to read
  SuperCollider's error messages
- **A "common mistakes" checklist** the agent actively avoids
- **Eleven deep-dive reference docs** loaded only when a task actually needs them

All distilled from a comprehensive, AI-facing SuperCollider knowledge base.

## Prerequisites

This skill teaches an *agent* about SuperCollider — it does not bundle
SuperCollider itself. To actually run the code your agent produces, install the
SuperCollider environment from
[supercollider.github.io/downloads](https://supercollider.github.io/downloads)
(macOS, Windows, Linux).

You'll also need a tool that supports Agent Skills (e.g.
[Claude Code](https://www.claude.com/product/claude-code)) and, for the one-line
install below, [Node.js](https://nodejs.org/) (for `npx`).

## Install

The easiest way is the [`skills`](https://github.com/vercel-labs/skills) CLI,
which supports 70+ agents:

```bash
# Add to the current project
npx skills add auto-duan/supercollider-skill

# Or install globally (available in every project)
npx skills add -g auto-duan/supercollider-skill
```

The skill lives at `skills/supercollider/` and follows the standard `SKILL.md`
frontmatter spec, so compatible tools auto-discover it.

### Manual install

Prefer not to use the CLI? Copy or symlink `skills/supercollider/` into your
agent's skills directory:

| Tool        | Skills path         |
| ----------- | ------------------- |
| Claude Code | `.claude/skills/`   |
| Cursor      | `.cursor/skills/`   |
| OpenCode    | `.opencode/skills/` |

```bash
git clone https://github.com/auto-duan/supercollider-skill.git
cp -r supercollider-skill/skills/supercollider ~/.claude/skills/
```

Because the `SKILL.md` frontmatter is standard, any Agent Skills–compatible tool
will pick it up.

## How it works

Agent Skills use **progressive disclosure** to keep your context window lean:

1. **`SKILL.md`** is lightweight and loads as soon as the agent detects a
   SuperCollider task. It carries the mental model, quick-start patterns, the
   common-mistakes list, and code conventions.
2. **`references/`** docs are loaded on demand — only when the agent needs the
   detail for a specific subtopic (for example, it pulls in `patterns.md` when you
   ask about `Pbind`, or `buffers-samples.md` when you load a sample).

The result: the agent stays fast and cheap on simple tasks, but has real depth
available the moment it's needed.

## What the skill covers

| Topic                | Reference doc        | What the agent can do                                                       |
| -------------------- | -------------------- | -------------------------------------------------------------------------- |
| Client/server model  | `client-server.md`   | Reason about the `sclang` vs `scsynth` split; avoid mixing the two sides     |
| Language essentials  | `language-basics.md` | Variables, functions, messages, collections, symbols — read & write sclang  |
| Instruments          | `synthdef-synth.md`  | Define `SynthDef`s, instantiate `Synth`s, control & free them, pick UGens    |
| Routing              | `bus-group-node.md`  | Wire audio/control buses, organize nodes with groups, set execution order    |
| Sequencing           | `patterns.md`        | Build melodies/rhythms with `Pseq`, `Prand`, `Pwhite`, `Pexprand`, etc.      |
| Live coding          | `ndef-pdef-tdef.md`  | Hot-swap sound (`Ndef`), patterns (`Pdef`), and tasks (`Tdef`) while running |
| Samples              | `buffers-samples.md` | Allocate/load buffers, play with `PlayBuf`/`BufRd`, record audio             |
| Effects              | `fx-processing.md`   | Apply filters, reverb, delay, distortion, pitch shift                        |
| Packages             | `quarks.md`          | Install and manage Quarks (e.g. SuperDirt)                                   |
| Recipes              | `recipes.md`         | Reach for ready-made, runnable solutions to common tasks                     |
| Debugging            | `debugging.md`       | Isolate language- vs server-side bugs, trace patterns, read error text       |

## Quick taste

A flavour of the idiomatic code the skill steers the agent toward — define an
instrument, then drive it with a pattern:

```supercollider
s.boot;

// An instrument: a plucked tone that frees itself when the envelope ends
SynthDef(\pluck, { |freq = 440, amp = 0.1|
    var env = EnvGen.kr(Env.perc(0.01, 0.3), doneAction: 2);
    Out.ar(0, Saw.ar(freq) * env * amp ! 2);
}).add;

// Sequence it
Pbind(
    \instrument, \pluck,
    \degree, Pseq([0, 2, 4, 7, 9, 7, 4, 2], inf),
    \dur, Pseq([0.25, 0.25, 0.5], inf),
    \amp, 0.1
).play;
```

## Repository layout

```
skills/
  supercollider/
    SKILL.md                    # Trigger + quick reference (loaded on activation)
    references/                 # Deep-dive docs (loaded on demand)
      client-server.md          # The fundamental sclang/scsynth split
      language-basics.md        # sclang syntax essentials
      synthdef-synth.md         # SynthDef/Synth lifecycle and usage
      bus-group-node.md         # Server-side routing primitives
      patterns.md               # Pattern families (Pseq, Prand, Pwhite, …)
      ndef-pdef-tdef.md         # Live-coding proxies compared
      buffers-samples.md        # Buffer alloc/load, PlayBuf, BufRd, recording
      fx-processing.md          # Filters, reverb, delay, distortion, pitch shift
      quarks.md                 # Installing and managing quark packages
      recipes.md                # Runnable examples for common tasks
      debugging.md              # Debugging workflow and tools
README.md
LICENSE
```

## Try it

After installing, just ask your agent something like:

> "Write a SuperCollider SynthDef for a plucked string and sequence it with a pattern."

It should boot the server, define the synth correctly (with a self-freeing
envelope), and drive it with a `Pbind` — instead of the not-quite-runnable code
you'd get without the skill.

## Source

Distilled from
[supercollider-docs](https://github.com/bot-duan/supercollider-docs), a
comprehensive AI-facing SuperCollider knowledge base.

## Contributing

Issues and pull requests are welcome — especially additional reference topics,
corrections, and more recipes. Keep examples minimal and runnable, and always
state the expected output.

## License

[MIT](./LICENSE)
