# SuperCollider Agent Skill

A compact Agent Skill for AI coding tools (Claude Code, Codex, etc.) working with [SuperCollider](https://supercollider.github.io/).

## What It Does

Provides an AI agent with:
- Core SuperCollider mental model (client/server architecture)
- Quick-reference code patterns for synthesis, patterns, live coding, routing
- Debugging workflow and common mistake avoidance
- Curated reference docs distilled from a 107-file knowledge base

## Structure

```
supercollider/
  SKILL.md              # Trigger + quick reference (loaded on activation)
  references/           # Deep-dive docs (loaded when needed)
    client-server.md
    bus-group-node.md
    patterns.md
    ndef-pdef-tdef.md
    language-basics.md
    synthdef-synth.md
    debugging.md
    recipes.md
```

## Usage with Claude Code

Copy or symlink the `supercollider/` directory into your Claude Code skills path. Claude Code will auto-detect the skill when you work on SuperCollider code.

## Usage with other AI coding tools

Point your tool's skill/knowledge system at the `supercollider/` directory. The `SKILL.md` frontmatter follows the standard Agent Skill format.

## Source

Distilled from [supercollider-docs](https://github.com/bot-duan/supercollider-docs), a comprehensive AI-facing SuperCollider knowledge base.

## License

MIT
