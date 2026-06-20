# SuperCollider Agent Skill

A compact Agent Skill for AI coding tools (Claude Code, Codex, Cursor, OpenCode, etc.) working with [SuperCollider](https://supercollider.github.io/).

## What It Does

Provides an AI agent with:
- Core SuperCollider mental model (client/server architecture)
- Quick-reference code patterns for synthesis, patterns, live coding, routing
- Debugging workflow and common mistake avoidance
- Curated reference docs distilled from a 107-file knowledge base

## Install

With the [`skills`](https://github.com/vercel-labs/skills) CLI (works across 70+ agents):

```bash
# Add to the current project
npx skills add auto-duan/supercollider-skill

# Or install globally
npx skills add -g auto-duan/supercollider-skill
```

The skill lives at `skills/supercollider/` and follows the standard `SKILL.md`
frontmatter spec, so it is auto-discovered.

## Structure

```
skills/
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
      buffers-samples.md
      fx-processing.md
      quarks.md
```

## Manual install

Copy or symlink `skills/supercollider/` into your agent's skills path
(e.g. `.claude/skills/` for Claude Code). The `SKILL.md` frontmatter follows
the standard Agent Skill format, so any compatible tool will pick it up.

## Source

Distilled from [supercollider-docs](https://github.com/bot-duan/supercollider-docs), a comprehensive AI-facing SuperCollider knowledge base.

## License

MIT
