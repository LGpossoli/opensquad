# Design: Update Command (`squados-terminal update`)

**Date:** 2026-02-27
**Status:** Approved

## Problem

Users currently have no way to update SquadOS without a full reinstall, which destroys user data (`_memory/`, `_investigations/`, `squads/`). A `reinstall.sh` script exists but is a manual, destructive workaround.

## Solution

Add `npx squados-terminal@latest update` — a command that re-applies the latest system templates to the project while preserving all user data.

## File Boundary

### Updated (always overwritten)
- `_squados/core/` — agents, prompts, platforms, runner, tools engine
- `_squados/tools/registry/` — tool definitions
- `_squados/tools/installed/` — installed tool locks
- `_squados/tools/scripts/` — tool scripts
- `CLAUDE.md` — project instructions
- `.claude/skills/squados.md` — Claude Code skill

### Protected (never touched)
- `_squados/_memory/` — company context and user preferences
- `_squados/_investigations/` — Sherlock investigation cache
- `squads/` — user-created squads and their data

## Version Tracking

A file `_squados/.squados-version` stores the currently installed version (e.g., `0.1.0`).

- Created by `init` on first install
- Updated by `update` after each successful update
- If missing (legacy installs), treated as unknown version

The file contains a single line: the semver string (e.g., `0.2.0`).

## Command Flow

```
npx squados-terminal@latest update
```

1. **Check initialized** — if `_squados/` does not exist, abort with: "No installation found. Run `init` first."
2. **Read current version** — read `_squados/.squados-version`. If missing, treat as unknown.
3. **Announce** — "Updating SquadOS vOLD → vNEW..."
4. **Copy files** — recursively copy all template files to the project, skipping protected paths.
5. **Update version file** — write new version to `_squados/.squados-version`.
6. **Show summary**:
   ```
   ✓ Updated: 12 system files
   ✓ Preserved: _memory/, _investigations/, squads/
   ✓ SquadOS v0.2.0 installed successfully
   ```

## Code Changes

| File | Change |
|------|--------|
| `bin/squados.js` | Add `update` case to the command switch |
| `src/init.js` or new `src/update.js` | Add `update()` function with protected-path copy logic |
| `templates/_squados/.squados-version` | New file — version string included in npm package templates |
| `src/locales/*.json` | Add i18n strings for update messages (8 languages) |

## Conflict Resolution

Always overwrite with the new version. Users are instructed not to manually edit core system files. This keeps the update logic simple and predictable.

## Notes

- Users must use `npx squados-terminal@latest update` (with `@latest`) to guarantee the newest version. Without `@latest`, npm may serve a cached version. The CLI will print a reminder about this.
- The `update` command shares the protected-path copy logic with `init`, so this logic should be extracted into a shared utility if not already.
