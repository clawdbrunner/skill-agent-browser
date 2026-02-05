---
name: agent-browser
description: Browser automation CLI for AI agents using Vercel's agent-browser. Use for web automation tasks requiring headless browser control — navigating pages, clicking elements, filling forms, taking screenshots, extracting content. Core workflow: snapshot (get accessibility tree with refs), interact via refs (@e1, @e2), re-snapshot after changes.
---

# agent-browser Skill

Headless browser automation via Vercel's `agent-browser` CLI.

## Prerequisites

- `agent-browser` installed globally: `npm install -g agent-browser`
- Chromium installed: `agent-browser install`

## Core Workflow

1. **Open** a page: `agent-browser open <url>`
2. **Snapshot** for interactive elements: `agent-browser snapshot -i --json`
3. **Interact** using refs: `agent-browser click @e1` / `agent-browser fill @e2 "text"`
4. **Re-snapshot** after page changes

## Common Commands

### Navigation
```bash
agent-browser open example.com
agent-browser open example.com --headed          # Visible browser
agent-browser open example.com --json            # JSON output
```

### Snapshot (Essential for AI)
```bash
agent-browser snapshot                           # Full accessibility tree
agent-browser snapshot -i                        # Interactive elements only
agent-browser snapshot -i -c                     # Interactive + compact
agent-browser snapshot -i --json                 # JSON for parsing
agent-browser snapshot -s "#main"                # Scope to selector
```

### Interaction
```bash
agent-browser click @e2                          # Click by ref
agent-browser fill @e3 "text"                    # Fill input by ref
agent-browser type @e3 "text"                    # Type without clearing
agent-browser press Enter                        # Press key
agent-browser hover @e4                          # Hover element
```

### Information
```bash
agent-browser get text @e1                       # Get element text
agent-browser get url                            # Current URL
agent-browser get title                          # Page title
agent-browser screenshot page.png                # Screenshot
agent-browser screenshot --full page.png         # Full page
```

### Session Management
```bash
agent-browser --session <name> open <url>        # Named session
agent-browser --profile <path> open <url>        # Persistent profile
agent-browser session list                       # List sessions
agent-browser close                              # Close browser
```

## Selectors

- **Refs (preferred)**: `@e1`, `@e2` from snapshot output
- **CSS**: `#id`, `.class`, `div > button`
- **Text**: `text=Submit`
- **XPath**: `xpath=//button`

## Options

| Flag | Description |
|------|-------------|
| `--json` | Machine-readable JSON output |
| `-i` | Interactive elements only |
| `-c` | Compact (remove empty structural elements) |
| `-d <n>` | Limit tree depth |
| `-s <sel>` | Scope to CSS selector |
| `--headed` | Show browser window |
| `--session <name>` | Use isolated session |
| `--profile <path>` | Persistent profile directory |
| `--cdp <port>` | Connect via Chrome DevTools Protocol |

## Example Session

```bash
# Open and snapshot
agent-browser open https://example.com
SNAPSHOT=$(agent-browser snapshot -i --json)

# Parse refs from JSON, then interact
agent-browser click @e1
agent-browser fill @e2 "test@example.com"

# Re-snapshot after interaction
agent-browser snapshot -i --json

# Clean up
agent-browser close
```

## Tips

- Always use `--json` for programmatic parsing
- Use `-i` flag to reduce snapshot size (only interactive elements)
- Re-snapshot after any action that changes the page
- Sessions persist between commands until `close`
- Profiles persist cookies/logins across browser restarts
