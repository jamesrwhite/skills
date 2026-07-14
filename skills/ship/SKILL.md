---
name: ship
description: 'Ship git changes by grouping them into small isolated commits and pushing. Use when user asks to ship, push, commit and push, or "/ship". Analyzes all unstaged/staged changes, groups them into logical isolated commits by concern, commits each group with a clear message, then pushes to the remote.'
license: MIT
allowed-tools: Bash
---

# Ship — Group, Commit, and Push

## Overview

Analyse all current git changes (staged and unstaged), split them into the smallest reasonable logical groupings, commit each group with a clear and succinct message describing what changed and why, then push the branch.

## Workflow

### 1. Understand the current state

```bash
git status --porcelain
git diff --stat
git diff --staged --stat
```

Also check the current branch and remote:

```bash
git branch --show-current
git remote -v
git log --oneline -5
```

### 2. Read the full diff

```bash
# All changes (staged + unstaged combined view)
git diff HEAD
```

Look at every changed file and understand *what* changed and *why*. Group by logical concern — not by file type or directory, but by the reason the change exists.

**Good groupings:**
- All changes that implement a single feature or fix a single bug
- A dependency bump and any code changes it required
- A config change and the code that uses it
- Tests for a specific area
- A pure refactor with no behaviour change

**Bad groupings:**
- Everything in one commit
- One file per commit when files are tightly coupled
- Mixing unrelated changes

### 3. Stage and commit each group

For each logical group, stage only the relevant files (or hunks) and commit:

```bash
# Stage specific files
git add path/to/file1 path/to/file2

# Stage specific hunks when a file has mixed concerns
git add -p path/to/file
```

Commit immediately after staging each group — don't batch them up.

Before the first commit, determine the Co-Authored-By trailer based on your own identity:

| Agent | Trailer |
|-------|---------|
| Claude (Anthropic) | `Co-Authored-By: Claude [Model Name] <noreply@anthropic.com>` |
| GitHub Copilot | `Co-Authored-By: GitHub Copilot <noreply@github.com>` |
| OpenAI Codex / ChatGPT | `Co-Authored-By: OpenAI Codex <noreply@openai.com>` |
| Gemini | `Co-Authored-By: Google Gemini <noreply@google.com>` |
| Amazon Q | `Co-Authored-By: Amazon Q <noreply@amazon.com>` |
| Windsurf | `Co-Authored-By: Windsurf <noreply@codeium.com>` |
| Cursor | `Co-Authored-By: Cursor <noreply@cursor.com>` |

Use the row that matches your identity. If you are Claude, substitute your specific model name (e.g. `Claude Sonnet 4.6`). If your agent is not listed, use `Co-Authored-By: [Your Agent Name] <noreply@ai>`.

Set this as `_CO_AUTHOR` and then commit, including the trailer:

```bash
git commit -m "$(cat <<EOF
Short, direct description of what this change does and why

${_CO_AUTHOR:+$_CO_AUTHOR}
EOF
)"
```

Good commit message style:
- Say *what* changed and *why*, not *how*
- One sentence is usually enough; add a blank line + body only when context is genuinely non-obvious
- Present tense, imperative: "add retry logic" not "added retry logic"
- Under 72 characters on the first line

Repeat until `git status` shows nothing left to commit.

### 4. Push

```bash
git push
```

If the branch has no upstream yet:

```bash
git push -u origin $(git branch --show-current)
```

### 5. Offer to create a PR (if applicable)

The `git push` output from step 4 includes a "Create a pull request" URL when no PR exists for the branch — use this to infer whether a PR is needed without an extra network call.

If the push output **does not** contain a "create a pull request" prompt, a PR likely already exists — skip this step silently.

If the push output **does** contain such a prompt, check whether `gh` is available:

```bash
which gh 2>/dev/null
```

If `gh` is not available, skip silently.

If `gh` is available, use `AskUserQuestion` to prompt the user with a single question:

- **Question:** "No PR exists for this branch — would you like me to create one?"
- **Options:**
  - "Draft PR" — create the PR in draft state (`gh pr create --draft`)
  - "Ready for review" — create the PR ready for review (no `--draft` flag)
  - "No PR" — skip PR creation

If the user chooses "No PR", skip silently.

Otherwise, draft a title and body from the commits just pushed. Use the same agent identity you determined for the Co-Authored-By trailer to populate the generated-by footer — e.g. `🤖 Generated with Claude Sonnet 4.6` or `🤖 Generated with GitHub Copilot`. Then run with the appropriate flag:

```bash
# Draft
gh pr create --draft --title "<title>" --body "$(cat <<'EOF'
## Summary
<bullet points derived from the commits>

## Test plan
- [ ] <relevant test steps>

🤖 Generated with <agent name and version>
EOF
)"

# Ready for review
gh pr create --title "<title>" --body "$(cat <<'EOF'
## Summary
<bullet points derived from the commits>

## Test plan
- [ ] <relevant test steps>

🤖 Generated with <agent name and version>
EOF
)"
```

Show the PR URL after creation.

### 6. Confirm

Show a summary of what was shipped:

```bash
git log --oneline origin/HEAD..HEAD 2>/dev/null || git log --oneline -10
```

Then respond with a selection of ship/rocket emojis to celebrate the launch. 🚀🛸⛵🚢🛳️🌊💫🔥

## Safety Rules

- **Never commit secrets** — skip `.env`, credentials, private keys. Warn the user.
- **Never skip hooks** (`--no-verify`) unless the user explicitly asks.
- **Never force-push `main` or `master`.**
- **If a commit hook fails** — fix the issue, re-stage, and create a NEW commit. Do not `--amend`.
- **Never `git add .` or `git add -A`** without first reviewing `git status` — it can pull in unintended files.
- **If already on `main`/`master`** — pause and confirm with the user before pushing.

## Tips

- When a single file mixes two concerns, use `git add -p` to stage individual hunks.
- If there are untracked files you're unsure about, ask the user before staging them.
- Aim for commits that make sense independently — each one should build and be reviewable alone.
