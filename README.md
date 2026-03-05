# opensquad (Terminal)

> Create AI squads that work together — right from your terminal.

opensquad is a multi-agent orchestration framework for [Claude Code](https://claude.com/claude-code). Describe what you need in plain language, and opensquad creates a squad of specialized AI agents that work together automatically.

## What is a Squad?

A squad is a team of AI agents that collaborate on a task. For example:

**Instagram Content Squad** (included as example):
- 🔍 **Scout** researches trending content
- 💡 **Spark** generates viral content ideas
- ✍️ **Quill** writes the carousel copy
- ✅ **Eagle** reviews quality before delivery

The agents work in an automated pipeline — you only step in at key decision points.

## Quick Start

### Prerequisites

1. **Node.js 20+** — [Download here](https://nodejs.org)
2. **Claude Code** — [Install here](https://claude.com/claude-code)

### Installation

Open your terminal in any project folder and run:

```bash
npx opensquad init
```

> This will download opensquad and set up the framework in the current folder. No global install needed.

To update an existing installation:

```bash
npx opensquad update
```

Then open the folder in Claude Code and type:

```
/opensquad
```

opensquad will guide you through setup and show you the main menu.

#### Global install (optional)

If you prefer to have the `opensquad` command always available:

```bash
npm install -g opensquad
```

Then use `opensquad init` in any project folder.

### Local development (before npm publish)

If you're running opensquad from source (e.g. cloned the repo), there are two ways to install it in another project:

**Option 1 — `npm link` (recommended)**

```bash
# 1. In the opensquad project folder, create a global symlink
cd "d:/Coding Projects/squadkit"
npm link

# 2. In any other project, use it normally
cd /path/to/your-project
opensquad init
```

> Changes to the opensquad source are reflected immediately — no reinstall needed.

To remove the link later:

```bash
npm unlink -g opensquad
```

**Option 2 — Direct path**

Run the CLI directly without linking:

```bash
node "d:/Coding Projects/squadkit/bin/opensquad.js" init
```

You can also create a shell alias for convenience:

```bash
# Add to your ~/.bashrc or ~/.zshrc
alias opensquad='node "d:/Coding Projects/squadkit/bin/opensquad.js"'
```

Then use `opensquad init`, `opensquad update`, etc. from any folder.

**Installing skills locally**

The `opensquad install <name>` command fetches skills from GitHub. In local development, the skills aren't published yet, so you need to copy them manually:

```bash
# Copy a single skill (e.g. "canva") to your project
cp -r "d:/Coding Projects/squadkit/skills/canva" /path/to/your-project/skills/

# Or copy all available skills at once
cp -r "d:/Coding Projects/squadkit/skills/"* /path/to/your-project/skills/
```

Available skills in the repo: `apify`, `blotato`, `canva`, `image-creator`, `image-fetcher`, `instagram-publisher`, `opensquad-skill-creator`.

Each skill is a folder with a `SKILL.md` file. The destination must be `skills/<skill-name>/` inside the initialized project.

## Commands

| Command | What it does |
|---------|-------------|
| `/opensquad` | Open the main menu |
| `/opensquad help` | Show all commands |
| `/opensquad create` | Create a new squad |
| `/opensquad run <name>` | Run a squad |
| `/opensquad list` | See all your squads |
| `/opensquad edit <name>` | Modify a squad |
| `/opensquad skills` | Browse installed skills |
| `/opensquad install <name>` | Install a skill from catalog |
| `/opensquad uninstall <name>` | Remove an installed skill |

## Creating Your Own Squad

Just describe what you need:

```
/opensquad create "A squad that writes LinkedIn posts about AI trends"
```

The Architect will ask you a few questions, design the squad, and set it up automatically.

## Examples

```
/opensquad create "Weekly email newsletter squad for my SaaS product"
/opensquad create "Social media content calendar squad"
/opensquad create "Customer support email response squad"
/opensquad create "Data analysis squad for sales spreadsheets"
/opensquad create "Blog post production squad"
```

## How It Works

1. You describe what you need
2. The **Architect** agent designs a squad with the right agents
3. You approve the design
4. The squad runs automatically, pausing only for your input at checkpoints
5. Final output is saved to your project

## License

MIT — use it however you want.
