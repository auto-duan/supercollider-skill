# SuperCollider Agent Skill

A ready-to-install **Agent Skill** that teaches AI coding assistants how to write,
debug, and reason about [SuperCollider](https://supercollider.github.io/) — the
real-time audio synthesis and algorithmic composition language.

Works with Claude Code, Cursor, OpenCode, Codex, and any other tool that supports
the open [Agent Skills](https://github.com/vercel-labs/skills) format.

---

## What is this?

An **Agent Skill** is a small, self-contained bundle of expert knowledge you drop
into your AI coding tool. Once installed, the agent automatically pulls it in
whenever you start working on a relevant task — no prompting required.

This particular skill gives your agent a solid grounding in SuperCollider so it
stops making the classic mistakes (confusing the language with the audio server,
forgetting to boot the server, treating a `SynthDef` as if it makes sound, etc.)
and instead writes idiomatic, runnable code.

## Why use it?

SuperCollider has a steep learning curve and a split architecture (a scripting
language **and** a separate audio server) that trips up both humans and LLMs. Out
of the box, an AI assistant tends to produce code that looks plausible but won't
run. This skill packs in:

- **The core mental model** — the sclang (client) vs scsynth (server) split that
  underlies almost every beginner error
- **Quick-reference patterns** — copy-pasteable snippets for synthesis,
  sequencing, live coding, and signal routing
- **A debugging workflow** — language-side and server-side tools, plus how to
  read error messages
- **A "common mistakes" checklist** the agent actively avoids
- **Eleven deep-dive reference docs** loaded only when the task needs them

All distilled from a comprehensive, AI-facing SuperCollider knowledge base.

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

If you'd rather not use the CLI, copy or symlink `skills/supercollider/` into your
agent's skills directory:

| Tool         | Skills path                  |
| ------------ | ---------------------------- |
| Claude Code  | `.claude/skills/`            |
| Cursor       | `.cursor/skills/`            |
| OpenCode     | `.opencode/skills/`          |

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
   common-mistakes list, and conventions.
2. **`references/`** docs are loaded on demand — only when the agent needs the
   detail for a specific subtopic (e.g. it pulls in `patterns.md` when you ask
   about `Pbind`).

This means the agent stays fast for simple tasks but has depth available the
moment it's needed.

## What's inside

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
```

## Try it

After installing, just ask your agent something like:

> "Write a SuperCollider SynthDef for a plucked string and sequence it with a pattern."

It should boot the server, define the synth correctly, and drive it with a
`Pbind` — instead of the not-quite-runnable code you'd get without the skill.

## Source

Distilled from
[supercollider-docs](https://github.com/bot-duan/supercollider-docs), a
comprehensive AI-facing SuperCollider knowledge base.

## Contributing

Issues and pull requests are welcome — especially additional reference topics,
corrections, and more recipes. Keep examples minimal and runnable, and state the
expected output.

## License

[MIT](./LICENSE)
