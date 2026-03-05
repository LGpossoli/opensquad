# squados-dev Skill — Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Create an internal Claude Code skill `/squados-dev` that verifies templates sync and package integrity during squados-cli development.

**Architecture:** Single SKILL.md file at `.claude/skills/squados-dev/SKILL.md`. Prompt-type skill — no code, just instructions that guide the AI through verification checks using Bash, Read, and Grep tools.

**Tech Stack:** Markdown (Claude Code skill format)

**Design doc:** `docs/plans/2026-03-01-squados-dev-skill-design.md`

---

### Task 1: Create the squados-dev skill

**Files:**
- Create: `.claude/skills/squados-dev/SKILL.md`

**Step 1: Write the skill file**

Create `.claude/skills/squados-dev/SKILL.md` with the following content:

```markdown
---
name: squados-dev
description: "SquadOS development checklist — verifies templates sync, package integrity, and distribution correctness."
---

# SquadOS Development Checklist

You are running the squados-dev verification skill inside the squados-cli repository.
Your job is to detect and report distribution issues before they reach users.

## How SquadOS Distribution Works

Understand this before checking anything:

- **`templates/`** → Copied by `src/init.js:copyCommonTemplates()` during `npx squados init`
  and by `src/update.js` during `npx squados update`. Everything in `templates/` except
  `ide-templates/` is copied recursively to the user's project. This is the PRIMARY
  distribution mechanism — if a file isn't in templates, users don't get it on init.

- **`templates/ide-templates/`** → IDE-specific files. Copied selectively based on user's
  IDE selection during init. Each subfolder (`claude-code/`, `opencode/`, `codex/`,
  `antigravity/`) maps to one IDE choice.

- **`agents/`** (project root) → Predefined agent catalog, distributed via npm
  (`package.json files[]`). NOT in templates — users install agents via
  `npx squados agents install`. Protected from overwrites in `src/update.js:PROTECTED_PATHS`.

- **`skills/`** (project root) → Bundled skills catalog, distributed via npm
  (`package.json files[]`). Users install via `npx squados skills install`.

- **`_squados/core/*`** → Framework core files. MUST have a mirror copy in
  `templates/_squados/core/*`. Any change to a core file that isn't synced to templates
  means `npx squados init` delivers STALE content to new users.

- **`_squados/skills/*`** → Installed skills in dev environment. Template copies at
  `templates/_squados/skills/*` for skills that should ship pre-installed.

- **`package.json files[]`** → Controls what enters the npm package.
  Currently: `bin/`, `src/`, `agents/`, `skills/`, `templates/`.

- **`src/update.js:PROTECTED_PATHS`** → Directories NEVER overwritten during update:
  `_squados/_memory`, `_squados/_investigations`, `agents`, `squads`.

## Verification Process

### Step 1: Detect what changed

Run these commands to identify changed files:

```bash
# Uncommitted changes (staged + unstaged)
git diff --name-only HEAD

# If no uncommitted changes, check recent commits
git log --oneline -5
git diff --name-only HEAD~5..HEAD
```

Collect all changed file paths into a list.

### Step 2: Run applicable checks

For each changed file, apply the matching verification rules below.
Only run checks that are relevant to the actual changes detected.

#### Check A: Core file sync (`_squados/core/**` changed)

For EACH changed file matching `_squados/core/**`:

1. Compute the expected template path: `templates/{same relative path}`
   Example: `_squados/core/runner.pipeline.md` → `templates/_squados/core/runner.pipeline.md`

2. Run diff:
   ```bash
   diff "_squados/core/{file}" "templates/_squados/core/{file}"
   ```

3. If diff shows differences:
   - **FAIL**: Report the file and show the diff summary
   - **Fix**: `cp _squados/core/{file} templates/_squados/core/{file}`

4. If template file doesn't exist:
   - **FAIL**: "Template missing for `_squados/core/{file}`"
   - **Fix**: `mkdir -p templates/_squados/core/{dir} && cp _squados/core/{file} templates/_squados/core/{file}`

#### Check B: Skills sync (`_squados/skills/**` changed)

Same logic as Check A but for skills:
- Source: `_squados/skills/{skill}/SKILL.md`
- Template: `templates/_squados/skills/{skill}/SKILL.md`

#### Check C: Agents directory (`agents/**` changed)

1. Read `package.json`, parse the `files` array
2. Verify `"agents/"` is present in the array
3. If missing: **FAIL** — `"agents/" not in package.json files[]`

#### Check D: Init logic (`src/init.js` changed)

1. Read `src/init.js`
2. Verify `copyCommonTemplates` function exists and references `TEMPLATES_DIR`
3. Verify `getTemplateEntries` recursively scans the templates directory
4. Flag if any new filtering logic was added that might exclude files

#### Check E: Update logic (`src/update.js` changed)

1. Read `src/update.js`
2. Extract the `PROTECTED_PATHS` array
3. Verify it includes all user-owned directories:
   - `_squados/_memory` (user preferences and company context)
   - `_squados/_investigations` (Sherlock investigation data)
   - `agents` (user-installed/customized agents)
   - `squads` (user-created squads)
4. If a new user-owned top-level directory was added to the project,
   check if it should be in PROTECTED_PATHS

#### Check F: Package manifest (`package.json` changed)

1. Read `package.json`, parse `files` array
2. Verify these directories are present: `bin/`, `src/`, `agents/`, `skills/`, `templates/`
3. If any distributable directory exists at project root but is NOT in `files[]`: **FAIL**

#### Check G: New top-level directory (any new directory at root)

1. Run `ls -d */` at project root
2. For each directory, check:
   - Is it in `package.json files[]`? (if it should be distributed)
   - Is it in `PROTECTED_PATHS`? (if it's user-owned content)
   - Is it in `templates/`? (if it should be copied during init)
3. Flag any directory that seems like it should be distributed but isn't configured

### Step 3: Report results

Present a clear summary:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔧 SquadOS Dev Checklist
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Files changed: {N}
Checks run: {N}

✅ Check A: Core file sync — {N}/{N} files in sync
❌ Check B: Skills sync — {file} out of sync
   Fix: cp _squados/skills/{x}/SKILL.md templates/_squados/skills/{x}/SKILL.md
✅ Check F: Package manifest — all directories present

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Result: {N} issues found
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

If ALL checks pass, report clean:
```
✅ All {N} checks passed — distribution is consistent.
```

If any check fails, list the fix commands at the end so the user
can approve them in batch.
```

**Step 2: Commit**

```bash
git add .claude/skills/squados-dev/SKILL.md
git commit -m "feat: add squados-dev verification skill for development workflow"
```

---

## Summary

| # | Task | Files |
|---|------|-------|
| 1 | Create squados-dev skill | New: `.claude/skills/squados-dev/SKILL.md` |

**Total**: 1 task, 1 new file
