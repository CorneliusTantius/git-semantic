# git-semantic

Slash-command skill for generating and executing conventional git commits.

## Install

```bash
npx skills add CorneliusTantius/git-semantic
```

## Usage

Type `/gitsemantic` in any supported agent:
- **OpenCode**: `/gitsemantic`
- **Claude Code**: `/gitsemantic`
- **Cursor**: `/gitsemantic`
- **Other agents**: Uses description-based matching

## What It Does

1. Checks for staged changes (`git diff --cached`)
2. Analyzes diff to determine type + scope
3. Generates Conventional Commits message
4. Confirms with user before committing
5. Executes `git commit -m "<message>"`

## Supported Types

`feat`, `fix`, `docs`, `refactor`, `test`, `perf`, `ci`, `build`, `style`, `revert`

## Example

```
$ git add .
$ /commit
→ Analyzes staged changes
→ Generates: "feat(auth): add JWT token refresh"
→ Confirms with you
→ Executes: git commit -m "feat(auth): add JWT token refresh"
```

## Compatibility

- OpenCode ✅
- Claude Code ✅
- Claude (claude.ai) ✅
- Cursor ✅
- Codex ✅
- All agents supporting Anthropic Agent Skills spec

## Structure

```
git-commit/
├── SKILL.md       # Skill instructions
├── package.json   # npm metadata
└── README.md      # This file
```