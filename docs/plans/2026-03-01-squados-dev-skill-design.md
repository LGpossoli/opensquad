# squados-dev Skill — Design Document

> Internal development skill for the squados-cli repository.
> Ensures templates sync, package integrity, and distribution correctness.

## Problem

When modifying core SquadOS files (`_squados/core/*`, `agents/`, `src/init.js`, etc.),
we frequently forget to:
- Sync changes to `templates/` (which is what `npx squados init` copies)
- Verify `package.json files[]` includes new directories
- Update `PROTECTED_PATHS` in `src/update.js` for new user-owned directories

This causes new installations to receive stale files.

## Solution

A Claude Code skill (`/squados-dev`) invoked manually in every development session
on the squados-cli repo. It provides:

1. **Architecture context** — how the distribution system works (15 lines)
2. **Dynamic checklist** — based on `git diff`, generates specific verification tasks
3. **Discrepancy report** — diffs core vs template files, reports mismatches

## Architecture Context (embedded in skill)

The skill includes this reference for the AI to understand the system:

- `templates/` → copied by `src/init.js:copyCommonTemplates()` during `npx squados init`
  and by `src/update.js` during `npx squados update`. Everything in `templates/` except
  `ide-templates/` is copied recursively to the user's project.
- `agents/` (root) → predefined agent catalog, distributed via npm (`package.json files[]`).
  NOT in templates — users install agents via `npx squados agents install`.
  Protected from overwrites in `src/update.js:PROTECTED_PATHS`.
- `_squados/core/*` → MUST have a mirror copy in `templates/_squados/core/*`.
  Any change to a core file that isn't synced means `npx squados init` delivers stale content.
- `_squados/skills/*` → installed skills. Template copies in `templates/_squados/skills/*`.
- `package.json files[]` → controls what enters the npm package. Currently:
  `bin/`, `src/`, `agents/`, `skills/`, `templates/`.
- `src/update.js:PROTECTED_PATHS` → directories never overwritten during update:
  `_squados/_memory`, `_squados/_investigations`, `agents`, `squads`.

## Dynamic Checklist Logic

On invocation, the skill instructs the AI to:

1. Run `git diff --name-only HEAD` (unstaged + staged changes)
2. Also run `git diff --name-only HEAD~{N}..HEAD` for recent commits if no uncommitted changes
3. Based on changed file paths, generate applicable checks:

| Changed path pattern | Verification |
|---------------------|-------------|
| `_squados/core/**` | Diff each changed file against `templates/_squados/core/**` counterpart |
| `_squados/skills/**` | Diff against `templates/_squados/skills/**` counterpart |
| `agents/**` | Verify `"agents/"` in `package.json files[]` |
| `src/init.js` | Review that `copyCommonTemplates` logic matches templates structure |
| `src/update.js` | Review `PROTECTED_PATHS` array for completeness |
| `package.json` | Verify `files[]` includes all distributable directories |
| New top-level directory | Check if it needs to be in `files[]` and/or `PROTECTED_PATHS` |

4. For each applicable check, run the actual verification (read files, diff content)
5. Report results: pass/fail per check, with suggested fix commands for failures

## Skill Location

`.claude/skills/squados-dev/SKILL.md` — Claude Code skill (not a SquadOS skill).
Invoked with `/squados-dev` in Claude Code.

## Out of Scope

- Automatic npm publish workflow (separate concern)
- Migration of `_squados/skills/` to `/skills` root (separate design doc)
- CI/CD integration (future enhancement)
