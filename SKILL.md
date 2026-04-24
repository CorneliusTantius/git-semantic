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
- **Type**: feat | fix | refactor | perf | test | docs | chore | ci | build | revert
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

| Type | When to Use |
|------|-------------|
| `feat` | A new feature visible to users or downstream consumers |
| `fix` | A bug fix |
| `refactor` | Code change with no feature or bug change |
| `perf` | Performance improvement |
| `test` | Adding or updating tests |
| `docs` | Documentation only |
| `chore` | Tooling, dependencies, config (no production code) |
| `ci` | Changes to CI/CD configuration |
| `build` | Changes to build system or external dependencies |
| `revert` | Reverts a previous commit |

## Atomic Commit Rules

An atomic commit does exactly one thing. Not one feature — one logical change. Smallest unit that can stand alone, be reviewed, reverted, or cherry-picked without side effects.

### Golden Rules

1. **One logical change per commit** — If message needs "and", split it.
   ```
   # Not atomic — two concerns
   feat(profile): add avatar upload and fix broken validation

   # Atomic — two commits
   feat(profile): add avatar upload endpoint
   fix(profile): correct validation error for missing fields
   ```

2. **Must compile, tests must pass** — Every commit must pass `npm run build` and `npm test`. Makes `git bisect` reliable.

3. **Staged changes must be clean** — Use lint-staged. Commits with lint errors or formatting violations will be aborted.

4. **Use interactive rebase before PR** — Clean WIP commits before opening PR:
   ```bash
   git rebase -i origin/main
   ```
   Use `squash`/`fixup` to collapse into clean atomic commits.

5. **Stage selectively — not `git add .`** — Use `git add -p` to stage only relevant lines. Forces atomic thinking at line level.

### Size Guard

| Too Small | Too Big | Just Right |
|-----------|---------|------------|
| Single typo fix | 5+ files, multiple concerns | 1 file or 1 logical unit |
| "oops" commit | Mixed feat + fix + refactor | Single purpose, reviewable |

If changes >5 files or multiple concerns → prompt to split.

## Notes

- No breaking changes by default (add `!` if needed)
- No body/footer unless complexity requires it
- Imperative mood: "add" not "added"
- Lowercase scope (optional)

## Anti-Patterns to Avoid

| Anti-pattern | Why It's Wrong |
|---|---|
| `"fix"` with no description | Zero context; useless in `git log` |
| `"WIP"` commits merged to trunk | Pollutes history; use Draft PRs instead |
| Multi-sentence subject lines | Breaks changelog tooling and git log readability |
| `feat: add feature, fix bug, update docs` | Multiple concerns in one commit — violates atomic commit principle |
| Skipping footer for breaking changes | Automated tooling will miss the SemVer major bump |
| Past tense (`"added"`, `"fixed"`) | Inconsistent with imperative convention |
| `Jira: ca-1042` (lowercase) | Regex won't match; Slack won't auto-link |
| `Jira: CA1042` (missing hyphen) | Regex won't match |
| Putting ticket in subject: `CA-1042 feat: ...` | Breaks type-first format; commitlint will reject |
| `Jira: https://...CA-1042` | Use key only, not full URL |