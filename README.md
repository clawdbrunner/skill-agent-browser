# skill-agent-browser

OpenClaw skill for [agent-browser](https://github.com/vercel-labs/agent-browser) — The best browser automation CLI for AI agents.

## Why This Is Better

### Deterministic Refs (Game-Changer for AI)

Traditional browser automation uses **CSS selectors** or **coordinates** that break constantly. agent-browser uses **deterministic refs** from accessibility trees:

```bash
# Get snapshot — each element gets a stable ref
agent-browser snapshot -i --json
# Output: - button "Submit" [ref=e2]

# Use that ref — it NEVER changes for that element
agent-browser click @e2
```

**Why this matters:**
- CSS selectors break when websites update
- Coordinates fail on responsive layouts  
- Vision-based automation is slow and expensive
- **Refs just work** — point to the exact element from your snapshot

### Built for AI Agents (Not Developers)

| | Traditional Tools | agent-browser |
|---|---|---|
| **Element targeting** | Fragile selectors | Deterministic refs |
| **Page understanding** | Raw HTML soup | Semantic accessibility tree |
| **Output** | Text logs | Structured JSON |
| **Speed** | Slow per command | Fast daemon architecture |
| **Designed for** | Human developers | AI agents |

### Fast Architecture

- **Rust CLI** — Native binary for instant command parsing
- **Node.js Daemon** — Browser stays warm between commands
- **First launch:** ~2 seconds
- **Subsequent commands:** ~100 milliseconds

## Installation

### Prerequisites

```bash
npm install -g agent-browser
agent-browser install  # Downloads Chromium (~30 seconds)
```

### Install the Skill

```bash
cd ~/.openclaw/skills  # or ~/clawd/skills
git clone https://github.com/clawdbrunner/skill-agent-browser.git agent-browser
```

## The AI Workflow

The pattern that makes agent-browser shine:

```bash
# 1. Navigate
agent-browser open example.com

# 2. Get structured snapshot (AI "sees" the page)
agent-browser snapshot -i --json

# 3. AI picks refs, executes actions
agent-browser click @e1
agent-browser fill @e2 "user@example.com"

# 4. Verify state changed
agent-browser snapshot -i --json

# 5. Done
agent-browser close
```

## Quick Reference

### Essential Commands

```bash
# Navigation
agent-browser open <url> [--json] [--headed]

# Snapshot (the killer feature)
agent-browser snapshot [-i] [-c] [--json]              # Interactive, compact, JSON

# Interaction with refs
agent-browser click @e1
agent-browser fill @e2 "text"
agent-browser type @e2 "text"                          # Without clearing
agent-browser press Enter

# State checks
agent-browser get text @e1
agent-browser get url
agent-browser is visible @e2

# Session management
agent-browser --session <name> open <url>            # Isolated session
agent-browser --profile <path> open <url>            # Persistent cookies
agent-browser close
```

### Snapshot Options (Control What AI Sees)

| Flag | Purpose |
|------|---------|
| `-i` | Interactive elements only — **recommended** |
| `-c` | Compact (remove empty elements) |
| `-d <n>` | Limit tree depth |
| `-s <sel>` | Scope to CSS selector |
| `--json` | Machine-readable JSON — **essential for AI** |

**Best for AI:** `agent-browser snapshot -i -c --json`

### Selector Strategies (Ranked)

1. **Refs** (Best): `agent-browser click @e2`
2. **Semantic**: `agent-browser find role button click --name "Submit"`
3. **CSS**: `agent-browser click "#submit"`
4. **Text/XPath** (Last resort): `agent-browser click "text=Submit"`

## Example: Complex Form Automation

```bash
# Open billing portal
agent-browser open https://portal.aeronetpr.com

# Login flow using refs from snapshot
agent-browser fill @e1 "username"
agent-browser fill @e2 "password"
agent-browser click @e3

# Wait for navigation, get new state
sleep 2
agent-browser snapshot -i --json

# Add credit card
agent-browser click @e24                                    # "Add new credit card"
agent-browser fill @e10 "John Smith"                       # Name on card
agent-browser fill @e11 "4111111111111111"                 # Card number (test)
agent-browser fill @e12 "12/2030"                          # Expiration
agent-browser fill @e13 "123"                              # CVV
agent-browser fill @e14 "123 Main Street"                  # Address
agent-browser fill @e260 "San Juan"                        # City
agent-browser fill @e322 "00907"                           # ZIP
agent-browser click @e324                                  # Submit

# Verify
agent-browser snapshot -i --json

# Cleanup
agent-browser close
```

## Documentation

See [SKILL.md](SKILL.md) for complete documentation including:
- Detailed comparison to other tools
- All commands and options
- Tips for AI agents
- When to use this vs built-in browser tools

## Comparison to Alternatives

| Tool | agent-browser Advantage |
|------|------------------------|
| **Puppeteer/Playwright** | Purpose-built for AI, deterministic refs vs brittle selectors |
| **Selenium** | 10x faster, modern architecture |
| **browser-use** | Better refs system, faster CLI |
| **Vision-based** | 100x cheaper, 10x faster, no API calls |
| **OpenClaw browser** | Better for complex multi-step flows |

## When to Use This

**Use agent-browser for:**
- Multi-step web workflows
- Complex form filling
- Reliable, repeatable automation
- Dynamic/modern web apps
- Cost-sensitive operations (no vision APIs)

**Use OpenClaw's built-in browser tool for:**
- Simple single-page checks
- Quick screenshots
- Already-authenticated Chrome sessions

## Links

- **Upstream project:** https://github.com/vercel-labs/agent-browser
- **This skill:** https://github.com/clawdbrunner/skill-agent-browser
- **Releases:** https://github.com/clawdbrunner/skill-agent-browser/releases

## License

MIT
