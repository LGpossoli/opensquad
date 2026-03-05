# OpenSquad

Create AI squads that work together — right from your terminal.

## Installation

**Prerequisites:** Node.js 20+ and [Claude Code](https://claude.com/claude-code).

```bash
npx opensquad init
```

## Update

```bash
npx opensquad update
```

## How to Use

Open this folder in Claude Code and type:

```
/opensquad
```

This opens the main menu. From there you can create squads, run them, open the Virtual Office, and more.

You can also be direct — describe what you want in plain language:

```
/opensquad create a squad for writing LinkedIn posts about AI
/opensquad run my-squad
```

## Create a Squad

Type `/opensquad` and choose "Create squad" from the menu, or be direct:

```
/opensquad create a squad for [what you need]
```

The Architect will ask a few questions, design the squad, and set everything up automatically.

## Run a Squad

Type `/opensquad` and choose "Run squad" from the menu, or be direct:

```
/opensquad run the <squad-name> squad
```

The squad runs automatically, pausing only at decision checkpoints where your input is needed.

## Virtual Office

The Virtual Office is a 2D visual interface that shows your agents working in real time.

**Step 1 — Generate the dashboard** (in Claude Code):

```
/opensquad dashboard
```

**Step 2 — Serve it locally** (in terminal):

```bash
npx serve squads/<squad-name>/dashboard
```

**Step 3 —** Open `http://localhost:3000` in your browser.
