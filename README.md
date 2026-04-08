# universal-pitch-skill

A Claude Code skill that transforms any SaaS codebase into investor-grade pitch decks and high-conversion landing pages.

## What It Does

A 4-phase multi-agent pipeline:

| Phase | Agent | Input | Output |
|-------|-------|-------|--------|
| 1. **Discovery** | Project Scanner | Your codebase | Product essence (JSON) |
| 2. **Analysis** | UX Architect | Scanner output | Personas, pain points, JTBD |
| 3. **Creation** | Marketing Pro | UX analysis | Pitch deck + landing page |
| 4. **Review** | Quality Gate | Draft materials | Go/no-go assessment |

## Why This Exists

Every pitch tool (Gamma, Beautiful.ai, Slidebean) requires you to manually describe your product. None can read your codebase and figure out what you built, who it's for, and why it matters.

This skill bridges that gap using frameworks from YC (Seibel's 7 Questions), JTBD (Jobs to Be Done), and Value Proposition Canvas.

## Supports Both GUI and Non-UI Products

- **Web apps / dashboards**: Automated screenshot capture with Playwright, anonymization, navigation hiding
- **CLI / API / infrastructure**: Input-Magic-Output visual formula (code snippets, terminal recordings, architecture diagrams)

## Installation

### Claude Code Marketplace (Recommended)

```bash
claude install github:cookys/universal-pitch-skill
```

### Manual Install

```bash
# Option 1: Clone as plugin
git clone https://github.com/cookys/universal-pitch-skill.git \
  ~/.claude/plugins/pitch-craft

# Option 2: Copy skill only into a project
cp -r universal-pitch-skill/skills/pitch-craft/ \
  your-project/.claude/skills/pitch-craft/
```

## Usage

Once installed, the skill triggers automatically when you say things like:

- "Create a pitch for this project"
- "Add this feature to the pitch deck"
- "How should we present this to investors?"
- "Update the landing page"
- "Retake the product screenshots"
- "Who are our target users?"

You can also run individual phases:

- **Discovery only**: "What does this product actually do?"
- **Update existing**: "Add the new search feature to the pitch"
- **Review only**: "Check if the pitch is ready for the investor meeting"

## File Structure

```
.claude-plugin/
├── plugin.json                   # Plugin metadata (name, version, author)
└── marketplace.json              # Claude Code marketplace registration
skills/
└── pitch-craft/
    ├── SKILL.md                  # Main skill — 4-phase pipeline
    └── references/
        ├── frameworks.md         # YC 7 Questions, JTBD, VPC, Mom Test, 2x2 matrix
        ├── screenshot-automation.md  # Playwright templates, anonymization, sync
        └── visual-assets.md      # Non-UI: code snippets, terminal GIF, architecture
```

## Frameworks Included

| Framework | Source | Used In |
|-----------|--------|---------|
| **Seibel's 7 Questions** | Y Combinator | Phase 2 — pitch validation |
| **JTBD (3-layer)** | Clayton Christensen | Phase 2 — pain point analysis |
| **Value Proposition Canvas** | Strategyzer | Phase 2 — product-market fit |
| **Mom Test** | Rob Fitzpatrick | Phase 2 — interview-to-pitch conversion |
| **3-Layer Distillation** | Original | Phase 3 — feature → capability → emotion |
| **Input-Magic-Output** | Stripe/Twilio pattern | Phase 3 — non-UI visual strategy |

## Requirements

- [Claude Code](https://claude.ai/code) (CLI, desktop app, or IDE extension)
- No other dependencies — the skill is pure Markdown instructions

Optional (for screenshot automation):
- Playwright MCP plugin (for automated screenshot capture)
- Running instance of your web app (for GUI products)

## License

MIT

## Credits

Research sources: Y Combinator (Michael Seibel, Dalton Caldwell), The Mom Test (Rob Fitzpatrick), Jobs to Be Done (Clayton Christensen), Value Proposition Canvas (Strategyzer), CrewAI multi-agent patterns.

Built with [Claude Code](https://claude.ai/code).
