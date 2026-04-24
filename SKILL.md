---
name: git-semantic
description: >
  Generate and execute git commits with Conventional Commits format.
  Trigger: "/gitsemantic" slash command or when user says "commit changes", "commit now",
  "create commit", "git commit". Works with OpenCode, Claude Code, and other agents.
command: /gitsemantic
compatibility: opencode, claude-code, claude, cursor, codex, roo-code
---

# Git Semantic Commit

Execute git commits with Conventional Commits format. Single purpose: commit staged changes.

## Command Trigger

Slash command: `/gitsemantic`
Also triggers on: "commit changes", "commit now", "create commit", "git commit"

## Workflow

### 1. Check Staged Changes

```bash
git diff --cached --stat
git diff --cached --name-only
```

If nothing staged → prompt user to stage first.

### 2. Analyze Changes

Determine from diff:
- **Type**: feat | fix | docs | refactor | test | perf | ci | build | style | revert
- **Scope**: file/module affected (optional)
- **Summary**: what changed (≤72 chars, imperative mood)

### 3. Generate Commit Message

Format: `<type>(<scope>): <description>`

Examples:
```
feat(auth): add JWT token refresh
fix(api): handle null response
docs: update installation instructions
refactor(db): extract connection pooling
```

### 4. Confirm Before Commit

Show proposed message. Ask: "Commit with this message? [y/n]"

If no → offer to regenerate or accept custom message.

### 5. Execute Commit

```bash
git commit -m "<message>"
```

Show result: "Committed successfully. [hash]"

## Type Reference

| Type | Use For |
|------|---------|
| feat | New feature |
| fix | Bug fix |
| docs | Documentation |
| refactor | Code restructure |
| test | Tests |
| perf | Performance |
| ci | CI/CD |
| build | Build/dependencies |
| style | Formatting |
| revert | Revert commit |

## Notes

- No breaking changes by default (add `!` if needed)
- No body/footer unless complexity requires it
- Imperative mood: "add" not "added"
- Lowercase scope (optional)