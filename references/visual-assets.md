# Visual Assets for Non-UI Products

## Table of Contents

1. The Input → Magic → Output Formula
2. Code Snippet Best Practices
3. Terminal Recording Guide
4. Architecture Diagram from Codebase
5. Before/After Flow Diagrams
6. Audience-Specific Asset Selection

---

## 1. The Input → Magic → Output Formula

Every non-UI product pitch needs to make the invisible visible. The universal structure:

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   INPUT      │     │   MAGIC     │     │   OUTPUT     │
│              │ ──→ │             │ ──→ │              │
│ Developer's  │     │ Your product│     │ Tangible     │
│ code/command │     │ (can be     │     │ result       │
│              │     │  invisible) │     │              │
└─────────────┘     └─────────────┘     └─────────────┘
```

### Real-World Examples

| Company | Input | Magic | Output |
|---------|-------|-------|--------|
| **Stripe** | 7-line code snippet | Stripe API | Payment confirmation JSON |
| **Twilio** | `curl` POST | Twilio | Phone rings on stage |
| **Vercel** | `git push` | Build pipeline | Live URL in 8 seconds |
| **Terraform** | HCL config file | `terraform apply` | Infrastructure graph |
| **Datadog** | Agent install command | Metrics collection | Dashboard with graphs |
| **SendGrid** | API call with template | Email delivery | Inbox screenshot |

### Applying to Your Product

1. Identify: What does the developer TYPE or SEND?
2. Identify: What do they GET BACK or what HAPPENS?
3. Show both side-by-side, with your product name in between
4. For pitch decks: animate as a 3-step sequence
5. For landing pages: interactive code block with "Run" button

---

## 2. Code Snippet Best Practices

### Tools

| Tool | Output | Best For |
|------|--------|---------|
| [Carbon.now.sh](https://carbon.now.sh) | PNG image | Pitch decks (static) |
| [Ray.so](https://ray.so) | PNG with Mac-style window | Landing pages |
| [Shiki](https://shiki.style) | HTML/SVG | Embedded in web pages |
| Hand-crafted `<pre>` | HTML | Full control |

### Rules

- **Keep it short**: 5-10 lines max. If your "hello world" is 30 lines, your DX needs work.
- **Show the happy path**: No error handling, no edge cases. Just the core value.
- **Multiple languages**: If your API has SDKs, show tabs (Python / JS / Go / curl)
- **Syntax highlighting**: Match the dark/light theme of your pitch materials
- **Make it runnable**: If possible, the exact code shown should actually work

### Example Layout (Landing Page)

```
┌─────────────────────────────────────────┐
│  Python  │  JavaScript  │  curl         │  ← language tabs
├─────────────────────────────────────────┤
│                                         │
│  import mylib                           │
│                                         │
│  result = mylib.analyze("input.csv")    │
│  print(result.summary)                  │
│                                         │
│  # Output:                              │
│  # ✓ 3,847 rows processed              │
│  # ✓ 12 anomalies detected             │
│  # ✓ Report saved to output.html       │
│                                         │
└─────────────────────────────────────────┘
```

---

## 3. Terminal Recording Guide

### VHS (Recommended for Pitch)

[VHS](https://github.com/charmbracelet/vhs) creates GIFs from scripted terminal sessions.
Version-controllable, reproducible, no manual typing.

```tape
# demo.tape
Output demo.gif
Set FontSize 16
Set Width 800
Set Height 400
Set Theme "Catppuccin Mocha"

Type "myctl deploy --env production"
Enter
Sleep 2s
# Simulate output appearing
Type@0ms "✓ Building... done (2.3s)"
Enter
Type@0ms "✓ Deploying to us-east-1... done (4.1s)"  
Enter
Type@0ms "✓ Live at https://app.example.com"
Enter
Sleep 3s
```

Run: `vhs demo.tape` → produces `demo.gif`

### asciinema (For Embeddable Player)

```bash
asciinema rec demo.cast
# Do your demo
# Ctrl+D to stop
asciinema upload demo.cast  # or embed locally
```

### Tips

- **Speed up boring parts**: Nobody wants to watch `npm install` for 30 seconds
- **Pause on results**: Let the output sit for 2-3 seconds so viewers can read
- **Clean terminal**: No shell prompt clutter, no previous command history
- **Font size**: 16-20px minimum — pitch deck screenshots are often viewed small

---

## 4. Architecture Diagram from Codebase

### Auto-Generation Tools

| Tool | Approach | Best For |
|------|----------|---------|
| **Swark** | LLM analyzes code → generates diagram | Quick overview, any language |
| **Structurizr** | C4 model DSL | Java/Spring projects |
| **D2** | Diagram-as-code | Version-controlled diagrams |
| **code2flow** | Python AST → flowchart | Function-level flows |
| **Mermaid** | Markdown-like syntax | README/docs embedding |

### D2 Example

```d2
user: Developer {shape: person}
cli: "myctl CLI" {shape: hexagon}
api: "Core API" {shape: cloud}
db: PostgreSQL {shape: cylinder}

user -> cli: "myctl deploy"
cli -> api: REST
api -> db: Query
api -> user: "✓ Deployed"
```

Renders to a clean SVG. Put the `.d2` file in your repo — it's version-controlled
documentation that doubles as pitch visual.

### What to Show in Each Context

- **Pitch deck**: High-level flow (3-5 boxes). User → Your Product → Result.
- **Landing page**: Integration ecosystem (your product in the center, arrows to/from services)
- **Technical deep-dive**: Component architecture (for Series A appendix or technical buyer)

---

## 5. Before/After Flow Diagrams

The most universally understood visual for non-technical audiences.

### Template

```
BEFORE (painful):                    AFTER (with product):

┌──────┐  manual   ┌──────┐        ┌──────┐  auto    ┌──────┐
│Step 1│ ────────→ │Step 2│        │Step 1│ ───────→ │Done! │
└──────┘  3 hours  └──────┘        └──────┘  5 min   └──────┘
    │                  │                │
    ▼    error-prone   ▼                ▼
┌──────┐  manual   ┌──────┐        (steps 2-4
│Step 3│ ────────→ │Step 4│         eliminated)
└──────┘  2 hours  └──────┘
```

### Rules

- **Left side painful, right side simple**: More boxes on the left, fewer on the right
- **Annotate with time/cost**: "3 hours" → "5 minutes" is more powerful than "slow" → "fast"
- **Use color**: Red/grey for "before", green/teal for "after"
- **Keep it honest**: Don't eliminate steps your product doesn't actually eliminate

---

## 6. Audience-Specific Asset Selection

### Quick Reference

| Audience | Primary Visual | Secondary | Avoid |
|----------|---------------|-----------|-------|
| **Seed investors** | Metrics chart, before/after flow | Customer logos | Raw code |
| **Series A investors** | Unit economics chart, growth curve | Architecture overview | Terminal recordings |
| **Technical buyer (CTO)** | Code snippet, terminal GIF | Architecture diagram | Stock photos |
| **Technical user (engineer)** | API example, live embed | Terminal recording | Process diagrams |
| **Non-technical buyer (VP)** | Before/after flow, integration map | Customer quotes | Any code |
| **Mixed audience** | Before/after flow + one code snippet | Metrics chart | Deep technical diagrams |

### The "One Visual Per Slide" Rule

Each pitch slide gets ONE visual that proves its claim. Don't mix:
- ✗ Code snippet + architecture diagram + metrics on one slide
- ✓ Code snippet alone, large, with one supporting sentence
