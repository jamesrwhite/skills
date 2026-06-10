# AGENTS.md

This file provides guidance to AI agents when working with code in this repository.

## What This Repo Is

A collection of agent skills for use with AI coding agents. Skills are installed via the `skills` CLI (`brew install skills`) and distributed from this repo with `skills install -g jamesrwhite/skills`.

## Skill Structure

Each skill lives under `skills/<name>/SKILL.md`. That single file is the entire skill — there is no build step, no tests, and no code outside the markdown.

A `SKILL.md` must start with YAML frontmatter:

```yaml
---
name: <skill-name>
description: <one-line description used as the trigger hint>
license: MIT          # optional
allowed-tools: Bash   # optional, comma-separated list of tools the skill may use
---
```

The body is the instruction set the agent follows when the skill is invoked. See `skills/hello/SKILL.md` and `skills/goodbye/SKILL.md` for minimal examples, and `skills/ship/SKILL.md` for a more complex one with explicit workflow stages and safety rules.

## Adding a Skill

1. Create `skills/<name>/SKILL.md` with valid frontmatter and a clear `## When to Use This Skill` section.
2. Add a row to the skills table in `README.md` — the table lists every skill with a link to its directory and a short description.

## Key Conventions

- The `description` frontmatter field doubles as the trigger hint shown to the user — keep it concise and action-oriented.
- Skills are pure instructions; they do not contain executable code.
- `allowed-tools` restricts which agent tools the skill can invoke — omit it if the skill only produces text output.
