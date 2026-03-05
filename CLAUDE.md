# SquadOS — Project Instructions

This project uses **SquadOS**, a multi-agent orchestration framework.

## Quick Start

Type `/squados` to open the main menu, or use any of these commands:
- `/squados create` — Create a new squad
- `/squados run <name>` — Run a squad
- `/squados help` — See all commands

## Directory Structure

- `_squados/` — SquadOS core files (do not modify manually)
- `_squados/_memory/` — Persistent memory (company context, preferences)
- `skills/` — Installed skills (integrations, scripts, prompts)
- `squads/` — User-created squads
- `squads/{name}/_investigations/` — Sherlock content investigations (profile analyses)
- `squads/{name}/output/` — Generated content and files
- `_squados/_browser_profile/` — Persistent browser sessions (login cookies, localStorage)

## How It Works

1. The `/squados` skill is the entry point for all interactions
2. The **Architect** agent creates and modifies squads
3. During squad creation, the **Sherlock** investigator can analyze reference profiles (Instagram, YouTube, Twitter/X, LinkedIn) to extract real content patterns
4. The **Pipeline Runner** executes squads automatically
5. Agents communicate via persona switching (inline) or subagents (background)
6. Checkpoints pause execution for user input/approval

## Rules

- Always use `/squados` commands to interact with the system
- Do not manually edit files in `_squados/core/` unless you know what you're doing
- Squad YAML files can be edited manually if needed, but prefer using `/squados edit`
- Company context in `_squados/_memory/company.md` is loaded for every squad run

## Browser Sessions

SquadOS uses a persistent Playwright browser profile to keep you logged into social media platforms.
- Sessions are stored in `_squados/_browser_profile/` (gitignored, private to you)
- First time accessing a platform, you'll log in manually once
- Subsequent runs will reuse your saved session
- **Important:** The native Claude Code Playwright plugin must be disabled. SquadOS uses its own `@playwright/mcp` server configured in `.mcp.json`.
