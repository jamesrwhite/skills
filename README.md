# 🤹🏻‍♂️ Agent Skills

A collection of agent skills for use with GitHub Copilot, Claude, Codex and other AI agents.

## Installation

### `skills` CLI

```sh
brew install skills

# Install all skills
skills add jamesrwhite/skills --all -g

# Install one skill
skills add jamesrwhite/skills --skill ship -g

# Remove a skill
skills remove ship -g
```

### `gh skill`

```sh
brew install gh

# Install all skills
gh skill install jamesrwhite/skills --scope user

# Install one skill
gh skill install jamesrwhite/skills ship --scope user
```

## Skills

| Skill | Description |
|-------|-------------|
| [hello](skills/hello/) | Greets the user and demonstrates basic skill functionality |
| [goodbye](skills/goodbye/) | Says goodbye to the user and demonstrates basic skill functionality |
| [ship](skills/ship/) | Ships git changes by grouping them into small isolated commits and pushing |
