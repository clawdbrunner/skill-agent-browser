# skill-agent-browser

OpenClaw skill for [agent-browser](https://github.com/vercel-labs/agent-browser) — Browser automation CLI for AI agents.

## What It Does

This skill enables browser automation via Vercel's `agent-browser` CLI:
- Navigate to URLs
- Take snapshots of page accessibility trees
- Click elements, fill forms using deterministic refs
- Take screenshots and PDFs
- Manage sessions and persistent profiles

## Installation

### Prerequisites

```bash
npm install -g agent-browser
agent-browser install  # Download Chromium
```

### Install the Skill

Clone into your OpenClaw skills directory:

```bash
cd ~/.openclaw/skills  # or ~/clawd/skills
git clone https://github.com/clawdbrunner/skill-agent-browser.git agent-browser
```

## Quick Start

```bash
# Navigate and snapshot
agent-browser open example.com
agent-browser snapshot -i --json

# Interact using refs from snapshot
agent-browser click @e2
agent-browser fill @e3 "test@example.com"

# Clean up
agent-browser close
```

## Core Workflow

1. **Open**: `agent-browser open <url>`
2. **Snapshot**: `agent-browser snapshot -i --json` (get interactive elements with refs)
3. **Interact**: Use refs like `@e1`, `@e2` to click/fill
4. **Re-snapshot**: After page changes
5. **Close**: `agent-browser close`

## Documentation

See [SKILL.md](SKILL.md) for full command reference.

## License

MIT
