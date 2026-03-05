# SquadOS (Terminal) — Design Document

> **Date:** 2026-02-23
> **Status:** Approved
> **Author:** Renato + Claude
> **License:** MIT (Open Source)

---

## 1. Vision

SquadOS is a **multi-agent orchestration framework** that runs 100% inside Claude Code. Users describe what they need in plain language, and an Architect agent creates and manages squads of specialized AI agents that work together in automated pipelines.

**Target users:** Non-developers (entrepreneurs, executives) who need AI-powered workflows for their business.

**Inspired by:** [bmad-method](https://github.com/bmad-code-org/BMAD-METHOD) and [bmad-builder](https://github.com/bmad-code-org/bmad-builder) — adopting their agent YAML format, progressive disclosure workflows, sidecar memory, and party manifest patterns. The key innovation is **automated multi-agent pipelines** with checkpoints (bmad requires manual agent switching).

---

## 2. Architecture

### Motor

- **Claude Code pure** — no Node.js runtime. Claude Code is the orchestrator.
- **npm is only for distribution** — `npx squados-terminal init` installs YAML/MD config files.
- **Hybrid execution** — pipeline steps run either as:
  - `inline` (persona switching within the main session) — for creative/interactive tasks
  - `subagent` (via Claude Code Task tool) — for background tasks like web research

### High-Level Diagram

```
┌─────────────────────────────────────────────────────────┐
│                     CLAUDE CODE                          │
│                                                          │
│  .claude/skills/squados.md ─── Entry Point (/squados)   │
│         │                                                │
│         ▼                                                │
│  ┌──────────────┐   ┌────────────────────────────────┐  │
│  │  ARCHITECT    │   │  _squados/                      │  │
│  │  (creates/    │──▶│  ├── core/ (architect, runner)  │  │
│  │   edits       │   │  ├── _memory/ (company, prefs)  │  │
│  │   squads)     │   │  └── tools/ (reusable)          │  │
│  └──────────────┘   └────────────────────────────────┘  │
│         │                                                │
│         ▼                                                │
│  ┌──────────────┐   ┌────────────────────────────────┐  │
│  │ PIPELINE      │   │  squads/{name}/                 │  │
│  │ RUNNER        │──▶│  ├── squad.yaml                 │  │
│  │ (executes     │   │  ├── squad-party.csv            │  │
│  │  squads)      │   │  ├── agents/*.agent.yaml        │  │
│  └──────┬───────┘   │  ├── pipeline/steps/*.md         │  │
│    ┌────┴────┐       │  ├── _memory/memories.md         │  │
│  inline  subagent    │  └── output/                     │  │
│ (persona  (Task      └────────────────────────────────┘  │
│  switch)   tool)                                         │
└─────────────────────────────────────────────────────────┘
```

### File Structure

```
project/
├── .claude/
│   └── skills/
│       └── squados.md              # Skill entry point
├── _squados/                       # SquadOS core
│   ├── core/
│   │   ├── architect.agent.yaml    # Architect agent
│   │   ├── runner.pipeline.md      # Pipeline execution engine
│   │   └── prompts/                # Prompt templates by agent type
│   │       ├── researcher.prompt.md
│   │       ├── writer.prompt.md
│   │       ├── reviewer.prompt.md
│   │       └── analyst.prompt.md
│   ├── tools/                      # Reusable tool definitions
│   └── _memory/                    # Global persistent memory
│       ├── company.md              # Company context (auto-populated)
│       └── preferences.md          # User preferences
├── squads/                         # User-created squads
│   └── {squad-name}/
│       ├── squad.yaml              # Squad definition
│       ├── squad-party.csv         # Agent manifest
│       ├── agents/                 # Agent YAML files
│       ├── pipeline/
│       │   ├── pipeline.yaml       # Pipeline entry point
│       │   ├── steps/              # Micro-step files (80-200 lines)
│       │   └── data/               # Reference materials
│       ├── tools/                  # Squad-specific tools
│       ├── _memory/
│       │   └── memories.md         # Squad persistent memory
│       └── output/
│           └── YYYY-MM-DD/         # Outputs organized by date
├── CLAUDE.md                       # Claude Code instructions
└── package.json
```

---

## 3. The Architect Agent

The Architect is the main agent — the user's entry point. It has 3 responsibilities:

1. **Create squads** — understands the request, asks refinement questions, generates all squad files
2. **Modify squads** — adjusts agents/workflows based on user requests
3. **Manage squads** — list, delete, duplicate existing squads

### Creation Flow

```
PHASE 1: DISCOVERY (3-5 questions)
  → What niche/topic?
  → What's the target audience?
  → What's the desired frequency?
  → Any references or examples?

PHASE 2: DESIGN (presents to user)
  → "I'll create a squad with N agents:"
  → Shows each agent's role and the pipeline
  → User approves

PHASE 3: BUILD (generates files)
  → squad.yaml, agent YAMLs, pipeline steps, party CSV
  → "Squad created! Use /squados run {name}"
```

### Menu Commands

| Trigger | Action | Description |
|---------|--------|-------------|
| CS | Create Squad | Create a new squad from natural language |
| RS | Run Squad | Run a squad's pipeline |
| ES | Edit Squad | Modify an existing squad |
| LS | List Squads | List all squads with status |
| DS | Delete Squad | Delete a squad |
| EC | Edit Company | Edit company profile |
| SC | Show Company | Show company profile |
| ST | Settings | Change language, preferences |
| HP | Help | Show help and examples |

---

## 4. Squad Structure

### squad.yaml

```yaml
code: instagram-content
name: "Instagram Carousel Content Creator"
description: "Produces viral Instagram carousel content"
icon: 🎠

agents:
  - researcher.agent.yaml
  - ideator.agent.yaml
  - copywriter.agent.yaml
  - reviewer.agent.yaml

party: ./squad-party.csv

pipeline:
  mode: hybrid
  autonomy: high
  max_review_cycles: 3

  steps:
    - id: research
      agent: researcher
      execution: subagent
      output: "{output_folder}/{code}/drafts/research.md"

    - id: ideation
      agent: ideator
      execution: inline
      input: "{output_folder}/{code}/drafts/research.md"
      output: "{output_folder}/{code}/drafts/ideas.md"

    - id: user-choice
      type: checkpoint
      message: "Here are the content ideas. Which one should we develop?"

    - id: writing
      agent: copywriter
      execution: inline
      output: "{output_folder}/{code}/drafts/draft.md"

    - id: review
      agent: reviewer
      execution: inline
      output: "{output_folder}/{code}/drafts/review.md"
      on_reject: writing
      on_approve: final

    - id: final
      type: checkpoint
      message: "Content is ready! Review and approve."
      output: "{output_folder}/{code}/YYYY-MM-DD-content.md"

tools:
  - web_search: native
  - web_fetch: native
```

### agent.yaml (bmad-compatible format)

```yaml
agent:
  webskip: false
  metadata:
    id: "_squados/squads/instagram-content/agents/researcher"
    name: Scout
    title: Content Research Specialist
    icon: 🔍
    squad: instagram-content
    hasSidecar: false

  persona:
    role: >
      Content Research Specialist who identifies trending topics,
      viral patterns, and high-engagement content opportunities.
    identity: >
      Obsessively curious digital native with a nose for what's about
      to blow up. Thinks in data patterns but communicates in insights.
    communication_style: >
      Data-driven and concise. Presents findings as structured briefs
      with relevance scores. Uses bullet points religiously.
    principles:
      - Freshness over evergreen — prioritize last 7 days
      - Engagement metrics over vanity metrics
      - Cross-reference multiple sources before recommending
      - Always include source URLs
      - When in doubt, search more rather than guess

  tools:
    - web_search: native
    - web_fetch: native
```

### squad-party.csv

```csv
name,displayName,title,icon,role,identity,communicationStyle,squad,path
Scout,Scout,Content Research Specialist,🔍,Content Research...,Obsessively curious...,Data-driven...,instagram-content,./agents/researcher.agent.yaml
Spark,Spark,Viral Content Ideator,💡,Creative Ideation...,Sees connections...,Energetic...,instagram-content,./agents/ideator.agent.yaml
Quill,Quill,Carousel Copywriter,✍️,Copywriting...,Lives for the perfect...,Conversational...,instagram-content,./agents/copywriter.agent.yaml
Eagle,Eagle,Content Quality Reviewer,✅,Quality Assurance...,Perfectionist...,Constructive...,instagram-content,./agents/reviewer.agent.yaml
```

### Pipeline Steps (progressive disclosure)

Each step is a separate file (80-200 lines), loaded one at a time:

```
pipeline/steps/
├── step-01-research.md      # Load Scout, execute search
├── step-02-ideation.md      # Load Spark, generate ideas
├── step-03-user-choice.md   # Checkpoint: user picks idea
├── step-04-writing.md       # Load Quill, write content
├── step-05-review.md        # Load Eagle, review quality
└── step-06-final.md         # Checkpoint: final approval
```

Each step has frontmatter:
```yaml
---
name: step-01-research
agent: researcher
execution: subagent
nextStepFile: ./step-02-ideation.md
outputFile: "{output_folder}/drafts/research.md"
---
```

---

## 5. Memory System (3 layers)

### Layer 1: Company Context (~500-1000 tokens, always loaded)

`_squados/_memory/company.md` — auto-populated during onboarding via web research on user's URL.

Contains: company name, sector, audience, tone of voice, products/services, social profiles.

### Layer 2: Squad Memory (~500 tokens, loaded per squad)

`squads/{name}/_memory/memories.md` — updated automatically after each pipeline run.

Contains: learned preferences, patterns, what works/doesn't, review cycle insights.

### Layer 3: Outputs (files, not loaded in memory)

`squads/{name}/output/YYYY-MM-DD/` — content produced, consulted on-demand via Read tool.

---

## 6. Tools System

### Native tools (always available)

WebSearch, WebFetch, Read, Write, Edit, Bash, Task (subagents)

### Custom tools (per squad)

```yaml
tools:
  - web_search: native
  - instagram_publish:
      type: mcp
      server: "@modelcontextprotocol/server-instagram"
  - blotato_publish:
      type: api
      endpoint: "https://api.blotato.com/v1/publish"
      auth: env:BLOTATO_API_KEY
  - generate_image:
      type: script
      command: "node ./tools/generate-image.js"
```

The Architect guides non-dev users through tool setup with step-by-step instructions.

---

## 7. Installation & Onboarding

### Prerequisites
- Node.js 20+
- Claude Code installed and configured

### Installation
```bash
npx squados-terminal init
```

### Onboarding Flow
1. Ask user's name and preferred language
2. Ask company name/description + website URL
3. **Auto-research** company via WebFetch + WebSearch
4. Present findings for user review/correction
5. Save to `_squados/_memory/company.md`
6. Show main menu

### Commands

| Command | Description |
|---------|-------------|
| `/squados` | Main menu (interactive selector) |
| `/squados help` | Help with commands and examples |
| `/squados create` | Create a new squad |
| `/squados list` | List all squads |
| `/squados run <name>` | Run a squad |
| `/squados edit <name>` | Edit a squad |
| `/squados delete <name>` | Delete a squad |
| `/squados edit-company` | Edit company profile |
| `/squados show-company` | Show company profile |
| `/squados settings` | Preferences and configuration |
| `/squados reset` | Reset configuration |

### Main Menu (interactive)

`/squados` shows a selector with options:
- Create a new squad
- Run an existing squad
- My squads (view/edit/delete)
- Company profile
- Settings
- Help

---

## 8. MVP Scope

### Included

- `npx squados-terminal init` (installation + onboarding with web research)
- Architect agent (create squads via natural language)
- Pipeline Runner (hybrid: inline + subagent)
- Memory system (3 layers)
- Interactive menu + help
- Squad example: Instagram Content Creator (4 agents)
- Native tools + custom tool support (MCP/API/script)
- Progressive disclosure (micro-steps)
- Prompt templates by agent type
- English codebase with auto language support
- MIT license

### Not Included (Phase 2)

- Visual 2D interface (Gather.town-like)
- Squad marketplace
- Federated modules (separate npm packages)
- Desktop app (Electron/Tauri)

---

## 9. Key Design Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Runtime | Claude Code pure | No infrastructure complexity, follows bmad pattern |
| Squad creation | Natural language | Non-dev users can't edit YAML manually |
| Agent execution | Hybrid (inline + subagent) | Best of both: fluidity + parallelism |
| Agent flow | Automatic pipeline with checkpoints | User only intervenes at decision points |
| Memory | 3-layer (company, squad, outputs) | Token-efficient, persistent where it matters |
| File format | YAML agents + MD workflows | bmad-compatible, human-readable |
| Distribution | npm package | Standard, easy to install |
| Onboarding | Web research on company URL | Zero-effort setup, rich initial context |
| Language | English code, auto-detect user language | Global reach, localized experience |
| License | MIT | Maximum community adoption |
