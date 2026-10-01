# Kilo Code Integration

Installs the full Agency roster as [Kilo Code](https://kilocode.ai) agents. Each
agent becomes one `.md` file whose name (the filename) is the agent name — Kilo
derives it from the file, so no `name` key is emitted.

## Install

```bash
./scripts/install.sh --tool kilo-code
```

By default this writes to `~/.config/kilo/agent/` (user-wide). Set
`KILO_AGENTS_DIR` to install somewhere else:

```bash
# project-scoped, run from your project root
KILO_AGENTS_DIR=.kilo/agents ./scripts/install.sh --tool kilo-code
```

Project-scoped agents land in `.kilo/agents/`, which Kilo Code reads
automatically from the project root.

> Kilo Code discovers agents from disk on each session — no restart or config
> file edit needed after an install.

## Activate an Agent

Every generated agent is written with `mode: all`, which means it does both:

- shows up in the **agent picker** (`@agent-name`, or the picker in the TUI), and
- can be **delegated to** by another agent through the `task` tool.

```
@frontend-developer review this React component
```

or, from inside another agent:

```
Delegate the API contract review to the backend-architect subagent.
```

## Regenerate

After modifying agents, regenerate the Kilo Code agent files:

```bash
./scripts/convert.sh --tool kilo-code
```

## File Format

Each agent is a Markdown file with Kilo Code frontmatter and the agent persona
as the body. The filename carries the agent name, so the frontmatter holds only
`description` and `mode`:

```markdown
---
description: 'Expert frontend developer specializing in modern web technologies, React/Vue/Angular frameworks, UI implementation, and performance optimization'
mode: all
---
...agent body...
```

Kilo Code also accepts `color`, `model`, and `permission` keys; the Agency
conversion leaves them out so the rendered file stays minimal. There is no
`tools` key in Kilo Code agent frontmatter, so nothing is emitted for it.

Because there is no `name` key, this shape is **not** byte-identical to the
`gemini-md` / `qwen-md` renderings — it has its own `kilo-agent-md` format.

## Directories Kilo Code Scans

| Scope | Path |
|---|---|
| project | `<project>/.kilo/agents/` |
| project (legacy) | `<project>/.kilo/agent/` |
| project (legacy) | `<project>/.kilocode/agents/` |
| user | `~/.config/kilo/agent/` |

Project agents take precedence over user agents with the same name.
