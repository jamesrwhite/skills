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

### `claude plugin`

```sh
# Add this repo as a marketplace
claude plugin marketplace add https://github.com/jamesrwhite/skills

# Install all skills as a single plugin
claude plugin install jamesrwhite-skills
```

### Claude (manual)

Skills are installed by placing a directory containing a `SKILL.md` file into `~/.claude/skills/`.

```sh
# Clone the repo
git clone https://github.com/jamesrwhite/skills.git

# Install a specific skill
ln -s "$(pwd)/skills/ship" ~/.claude/skills/ship

# Or copy it
cp -r skills/ship ~/.claude/skills/ship
```

Installed skills are available as slash commands in Claude Code (e.g. `/ship`).

## Skills

### Examples

| Skill | Description |
|-------|-------------|
| [hello](skills/hello/) | Greets the user and demonstrates basic skill functionality |
| [goodbye](skills/goodbye/) | Says goodbye to the user and demonstrates basic skill functionality |

### Git

| Skill | Description |
|-------|-------------|
| [ship](skills/ship/) | Ships git changes by grouping them into small isolated commits and pushing |
