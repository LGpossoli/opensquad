# Fix Init Distribution — Design Document

> Fix SquadOS distribution: sync stale templates, make init install agents + skills automatically.

## Problem

When users run `npx squados init`, they receive:
- **Missing:** `_squados/core/formats/` (14 format files) — never synced to templates
- **Stale:** `_squados/core/platforms/` (deprecated, deleted from source but still in templates)
- **Outdated:** `architect.agent.yaml`, `runner.pipeline.md`, `sherlock.prompt.md` — template copies behind source
- **Missing:** No agents installed (agents are only available via separate `npx squados agents install`)
- **Missing:** No bundled skills installed (only MCP skills are offered during init)

## Root Cause

1. The formats system was added to `_squados/core/` but the templates directory was never synced
2. The deprecated `platforms/` was deleted from source but not from templates
3. `src/init.js` only copies from `templates/` — it never calls `installAgent()` or `installSkill()`

## Solution

### Part 1: Template Sync (immediate fix)

Sync all stale/missing files between `_squados/core/` and `templates/_squados/core/`:

| Source | Template | Action |
|--------|----------|--------|
| `_squados/core/formats/*.md` (14 files) | `templates/_squados/core/formats/` | **Create** directory + copy all |
| `_squados/core/architect.agent.yaml` | `templates/_squados/core/architect.agent.yaml` | **Overwrite** |
| `_squados/core/runner.pipeline.md` | `templates/_squados/core/runner.pipeline.md` | **Overwrite** |
| `_squados/core/prompts/sherlock.prompt.md` | `templates/_squados/core/prompts/sherlock.prompt.md` | **Overwrite** |
| — | `templates/_squados/core/platforms/` | **Delete** (deprecated) |

### Part 2: Init installs agents + skills automatically

Modify `src/init.js` to install all bundled agents and non-MCP skills after copying templates:

```
init() flow:
  1. copyCommonTemplates(targetDir)     ← existing
  2. copyIdeTemplates(ides, targetDir)  ← existing
  3. installAllAgents(targetDir)        ← NEW
  4. installAllBundledSkills(targetDir) ← NEW (non-MCP skills only)
  5. writeProjectReadme(targetDir)      ← existing
  6. [skills MCP selection prompt]      ← existing (unchanged)
```

**`installAllAgents(targetDir)`:**
- Reads `agents/` directory from npm package (BUNDLED_AGENTS_DIR)
- For each agent directory, calls existing `installAgent(id, targetDir)`
- Copies `agents/{id}/AGENT.md` → `{targetDir}/agents/{id}.agent.md`
- Logs each installed agent

**`installAllBundledSkills(targetDir)`:**
- Reads `skills/` directory from npm package (BUNDLED_SKILLS_DIR)
- Filters out `squados-skill-creator` (internal, already filtered elsewhere)
- For each skill, calls existing `installSkill(id, targetDir)`
- Copies `skills/{id}/SKILL.md` → `{targetDir}/_squados/skills/{id}/SKILL.md`
- These are non-MCP skills (no env vars, no configuration needed)
- MCP skills continue to be offered via the existing interactive prompt

### Part 3: Update also installs new agents/skills

Modify `src/update.js`:
- After updating templates, check for new agents/skills in the bundle
- Install any agent/skill that exists in the bundle but NOT in the user's project
- Respect `PROTECTED_PATHS` — never overwrite existing agents
- Log new additions separately from updates

### Part 4: Update squados-dev skill

Add Check H to `.claude/skills/squados-dev/SKILL.md`:
- Verify that `src/init.js` calls `installAllAgents()` and `installAllBundledSkills()`
- Verify the functions import from `src/agents.js` and `src/skills.js`

## Files Modified

- `src/init.js` — Add agent/skill auto-installation
- `src/update.js` — Add new agent/skill detection during update
- `templates/_squados/core/` — Full sync with source
- `.claude/skills/squados-dev/SKILL.md` — Add Check H

## Out of Scope

- Moving `_squados/skills/` to project root `/skills/` (separate design doc)
- Agent selection UI during init (decided: install all silently)
- MCP skill flow changes (unchanged)
