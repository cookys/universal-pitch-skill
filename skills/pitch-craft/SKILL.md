---
name: pitch-craft
description: >
  Codebase-to-pitch pipeline — scans any SaaS project, identifies personas and pain
  points, then generates pitch deck and landing page materials. Use this skill whenever
  creating or updating pitch decks, landing pages, investor materials, marketing pages,
  or product announcements. Also invoke when translating features into selling points,
  retaking product screenshots, preparing for demos, or analyzing what problem a product
  solves. Triggers: "make a pitch", "update the pitch deck", "create a landing page",
  "how should we present this feature", "prepare for the investor meeting", "what's our
  value proposition", "who are our users", "what problem does this solve", "who would
  buy this", "investor deck", "fundraising materials", "product marketing", "feature
  announcement", "做簡報", "投資人簡報", "產品頁面", "幫我做 pitch".
---

# Pitch Craft

A multi-agent pipeline that transforms a codebase into investor-grade pitch decks and
high-conversion landing pages. Based on YC methodology, JTBD framework, and real
startup pitch experience.

## The Pipeline

```
Phase 1: Project Discovery    → Scanner agent reads codebase, extracts product essence
Phase 2: Persona & Pain Point → UX Architect identifies who hurts and why
Phase 3: Pitch Creation       → Marketing Pro crafts deck + landing page
Phase 4: Review Gate          → Quality checks before shipping
```

Each phase has a defined input/output contract. Phases run sequentially — each one
depends on the previous output.

---

## Phase 1: Project Discovery (Scanner)

**Agent**: Use a fast model (Haiku) via `Explore` subagent to scan broadly.

**Goal**: Extract what the product does, who it's for, and what evidence exists — purely
from the codebase. No marketing spin yet.

### What to Scan

| Source | What to Extract |
|--------|----------------|
| `README.md`, `ABOUT.md` | Official positioning, "what it does" |
| `CHANGELOG.md`, git log (3 months) | Feature velocity, what gets worked on most |
| Route handlers / API endpoints | Functional surface area |
| DB schema / migrations | Data model = feature boundaries |
| `package.json` / `Cargo.toml` / `go.mod` | Tech stack, key dependencies |
| i18n files | Supported locales, UI text = feature names |
| Test descriptions | Implied user scenarios |
| `.env.example` | Subsystems (payment, email, auth providers) |
| `doc/specs/`, `doc/proposals/` | Original problem definitions |
| Pricing config / feature flags | Tiers, experiments, unreleased capabilities |

### Frontend Detection Logic

The scanner must determine whether the product has a user-facing UI. This drives the
entire visual strategy (screenshots vs code snippets vs diagrams).

**Detection signals (check in order, stop at first confident match):**

```
Signal                                          → Type        Confidence
─────────────────────────────────────────────────────────────────────────
package.json has vue/react/next/angular/svelte   → gui         high
frontend/ or client/ or web/ directory exists     → gui         high
docker-compose has nginx/frontend service         → gui         high
.html files with <script> or bundler config       → gui         medium
Flask/Django/Rails with templates/ directory       → hybrid      medium
Only Cargo.toml/go.mod with no JS dependencies    → cli/api     high
Only Python with no web framework                  → cli/api     medium
OpenAPI spec but no frontend code                  → api         medium
main.rs/main.go with clap/cobra CLI framework      → cli         high
Nothing conclusive                                 → unknown     —
```

**Fallback chain when detection is uncertain:**

```
1. Check README for screenshots or "getting started" with localhost URL
   → If found: likely has UI, try to start and verify

2. Check docker-compose for port mappings
   → Ports 80/443/3000/3999/5173/8080 suggest web UI

3. If still unknown → ASK the user:
   "Does your product have a web interface? If yes, how do I start it?"

4. If confirmed no UI → set frontend.has_ui = false, skip screenshot pipeline,
   use non-UI visual strategy (code snippets, terminal recordings, diagrams)
```

**Fallback for partial detection (has UI but can't fully detect):**

```
frontend.has_ui = true but framework/router/port unknown:
  → Ask user: "I see a frontend but can't detect how to start it.
    What command starts the dev server? What port does it run on?"
  → Fill in frontend.dev_command and frontend.dev_port from answer

frontend running but router not found:
  → Navigate to root URL, take a test screenshot
  → Use browser snapshot to discover navigation links
  → Build route list from visible nav items instead of code analysis

frontend running but nav element not detected:
  → Take screenshot of any page
  → Visually identify navigation region (left sidebar? top bar? none?)
  → Build CSS hide selector from observed DOM structure
  → If no nav visible, skip nav hiding (some SPAs have no persistent nav)

frontend running but auth blocks access:
  → Check for /login route or OAuth redirect
  → Ask user: "The app redirects to login. Can you provide test credentials
    or a session cookie?"
  → If no auth available, screenshot only public pages (login, landing, docs)
```

### Scanner Output Contract

```json
{
  "product": {
    "name": "",
    "one_liner": "≤15 words, what it does",
    "repo_url": "",
    "type": "gui | cli | api | infrastructure | hybrid",
    "tech_stack": { "backend": [], "frontend": [], "integrations": [] }
  },
  "features": [
    {
      "name": "",
      "description": "",
      "complexity": "core | secondary | experimental",
      "commit_frequency": "high | medium | low",
      "implied_pain": "what problem this feature seems to solve"
    }
  ],
  "audience_hints": {
    "personas": ["role names found in code/docs"],
    "industries": [],
    "company_size_hints": ""
  },
  "traction_signals": {
    "users_or_customers": "",
    "repos_or_data_points": "",
    "feature_iterations": "",
    "age_of_project": ""
  },
  "pricing_hints": "",
  "competitive_signals": "any competitor names found in docs/code",
  "frontend": {
    "has_ui": true,
    "framework": "vue | react | next | angular | svelte | none",
    "dev_command": "npm run dev",
    "dev_port": 3000,
    "router_file": "src/router/index.ts",
    "routes": [
      { "path": "/", "name": "Dashboard", "has_data": true },
      { "path": "/settings", "name": "Settings", "has_data": false }
    ],
    "nav_element": "nav | aside | .sidebar (CSS selector for navigation)",
    "auth_method": "cookie | token | oauth | none",
    "auth_login_url": "/login"
  }
}
```

### Scanner Prompt Template

```
Scan this codebase. Use the "What to Scan" table and "Frontend Detection Logic"
above as your checklist. Output the JSON contract above.

Be factual — extract what IS, don't invent what SHOULD BE.
If a field can't be determined, write "unknown" and note why.
For frontend fields, follow the fallback chain if detection is uncertain.
```

---

## Phase 2: Persona & Pain Point Analysis (UX Architect)

**Agent**: Use `voltagent-biz:ux-researcher` or similar UX-focused agent.

**Goal**: Transform scanner output into personas, pain points, and emotional journeys
using YC + JTBD methodology.

### Framework: JTBD × Value Proposition Canvas

For each persona identified by the scanner:

**Step 1 — Define the Job (JTBD format)**

```
When I [situation/trigger],
I want to [motivation/action],
So I can [expected outcome].
```

Three layers per job:
- **Functional**: The actual task ("Review my team's code contributions in 5 minutes")
- **Social**: How they want to be perceived ("Be seen as a data-driven manager")
- **Emotional**: How they want to feel ("Confident my assessments are fair")

**Step 2 — Map the Before/After Canvas**

| Dimension | Before (Pain) | After (Gain) |
|-----------|--------------|--------------|
| Time | Hours doing X | Minutes with product |
| Quality | Subjective, error-prone | Objective, data-backed |
| Confidence | "Am I missing something?" | "I see the full picture" |
| Cost | $X in manual labor | $Y subscription |

**Step 3 — Extract Emotional Beats**

Map the user's emotional journey through the product:
1. **First impression** (0-10 sec): What creates trust or skepticism?
2. **Aha moment** (10-60 sec): When do they realize "this is different"?
3. **Value delivery** (1-5 min): When do they get the actual outcome?
4. **Retention hook**: Why do they come back tomorrow?

### Seibel's Seven Questions (Validation)

After the JTBD analysis, validate by answering Michael Seibel's seven questions.
Items marked 🤖 can be derived from the codebase; items marked 👤 need founder input.

1. 🤖 **What do you do?** — Company name + one sentence + one concrete example
2. 👤 **How big is the market?** — TAM/SAM with transparent math *(founder provides numbers)*
3. 🤖 **What's your traction?** — Extract from git history, user count, deployment age
4. 🤖 **What's your unique insight?** — Infer from architecture choices and differentiators
5. 👤 **What's your business model?** — How you charge *(founder confirms pricing)*
6. 👤 **Who's your team?** — Specific achievements *(founder provides bios)*
7. 👤 **What do you want?** — Amount + milestones *(founder defines ask)*

For 👤 items, produce a clear prompt asking the founder to fill in specifics.
Don't invent market sizes or team bios — flag them as "NEEDS FOUNDER INPUT".

**Clarity test**: Send the one-liner to a smart friend. If they ask a clarifying question, rewrite. (Seibel's "Email Test")

### UX Architect Output Contract

```markdown
# Persona: [Role Name]

## Job to Be Done
"When I [situation], I want to [motivation], so I can [outcome]."

- Functional: [task-level job]
- Social: [perception job]  
- Emotional: [feeling job]

## Before → After Canvas
| Dimension | Before | After |
|-----------|--------|-------|
| ... | ... | ... |

## Emotional Beats
1. First impression: "..."
2. Aha moment: "..."
3. Value delivery: "..."
4. Retention hook: "..."

## Selling Points (per channel)
- Investor: "..." (differentiation, moat, market size)
- Customer: "..." (pain resolution, outcome)

## Seibel Seven Answers
1. 🤖 What we do: ...
2. 👤 Market size: [NEEDS FOUNDER INPUT — provide TAM/SAM with math]
3. 🤖 Traction: ...
4. 🤖 Unique insight: ...
5. 👤 Business model: [NEEDS FOUNDER INPUT — confirm pricing]
6. 👤 Team: [NEEDS FOUNDER INPUT — provide bios]
7. 👤 Ask: [NEEDS FOUNDER INPUT — amount + milestones]
```

Produce one block per persona (typically 2-3 personas: buyer, user, champion).

---

## Phase 3: Pitch Material Creation (Marketing Pro)

**Agent**: Use `voltagent-biz:content-marketer` or marketing-focused agent.

**Goal**: Transform UX analysis into pitch deck structure + landing page structure +
screenshot strategy.

### Selling Point Distillation (3-Layer Framework)

Every feature must pass through three layers before appearing in pitch materials:

```
Layer 1: Feature → Capability
  Strip jargon. Ask: "Why does the user need this?"
  "Real-time WebSocket sync" → "Instant collaboration"

Layer 2: Capability → Emotional Outcome
  Ask: "What fear does this solve? What desire does this fulfill?"
  "Instant collaboration" → "Never worry about working on stale data"

Layer 3: Emotional Outcome → Narrative Hook
  Pick a frame:
  - Contrast:  "Before: email attachments → After: real-time sync"
  - Quantify:  "From 3-day review cycles to 5-minute feedback"
  - Social:    "Your whole team sees changes as they happen"
```

Why this matters: technical audiences don't reject stories — they reject *empty* stories.
Data wrapped in narrative is the most persuasive format.

### Pitch Deck Structure (YC-influenced)

Every slide answers: **"What's the Before/After gap?"** No gap → cut the slide.

```
Seed/Angel (10-12 slides):
1.  Cover          — One-sentence positioning (≤8 words)
2.  Problem        — One real number, one scene, 20 seconds max
3.  Solution       — How you fix it + proof screenshot
4.  Product        — Core features (2-3 screenshots)
5.  Market         — TAM/SAM with visible calculation logic
6.  Traction       — Numbers WITH time frame
7.  Business Model — Pricing tiers, highlight recommended
8.  Competition    — 2x2 matrix on YOUR axes
9.  Team           — Achievements, not titles
10. Ask            — Exact amount + allocation + milestones

Series A+ (15-18 slides): add GTM, unit economics, case studies, financials.
```

### Landing Page Structure (Trust-Building Curve)

```
Hero         → "What is this?"       Visual proof + ≤8-word H1 + single CTA
                                     GUI: full screenshot | CLI/API: code snippet + output
Pain Points  → "I have this problem" 3 cards from JTBD emotional layer
Features     → "This solves it"      Alternating layout, visual per feature
                                     GUI: cropped screenshots | CLI/API: input→output demos
Social Proof → "Others trust it"     Quotes with name+title (not star ratings)
How It Works → "It's easy"           3-4 steps (GUI: screenshots | CLI: terminal steps)
Gallery      → "It looks real"       GUI: screenshot variety | CLI/API: integration map
Pricing      → "I can afford it"     3-tier, highlight middle
CTA          → "Let me try"          Single clear action
```

Hero rules: outcome-focused H1, single CTA above fold, LCP < 2.5s.

### Audience-Specific Messaging

| Dimension | Pitch Deck (Investors) | Landing Page (Customers) |
|-----------|----------------------|--------------------------|
| Opening | Market opportunity + team | Your pain, we understand |
| Evidence | TAM, CAC, NRR, moat | User quotes, time saved |
| CTA | "Invest $Xm" | "Free trial" |
| Focus | Why WE can do this | Why YOU need this |

### Visual Strategy

First, determine your product type — the visual approach is fundamentally different:

```
Has GUI (web app, dashboard, mobile)?
  → Screenshot Strategy (below)

No GUI (CLI, API, SDK, infrastructure, data pipeline)?
  → Non-UI Visual Strategy (below)

Hybrid (API with dashboard, CLI with web console)?
  → Mix both — dashboard screenshots for overview, code/terminal for technical depth
```

#### Screenshot Strategy (GUI Products)

Before any screenshot, ask: **"What is this screenshot's JOB?"**

| Job | Treatment | Use for |
|-----|-----------|---------|
| Establish product category | Full-page, nav hidden | Hero, first impressions |
| Prove a specific value claim | Crop to supporting region | Feature sections |
| Create emotional anchor | Spotlight effect | Differentiator slides |

**Navigation**: hide by default (inject CSS). Exception: when nav structure IS the value.

**Dark themes**: fine for technical audiences. Add border/glow to prevent melting into backgrounds. Never place on white backgrounds.

**Anonymization**: remove identifiers, preserve behavioral realism. Keep real number magnitudes and realistic variance (not all round numbers).

See `references/screenshot-automation.md` for Playwright templates and gotchas.

#### Non-UI Visual Strategy (CLI / API / Infrastructure Products)

The universal formula for non-UI products is **Input → Magic → Output**:

```
Show the developer's code/command (Input)
  → Your product processes it (Magic — can be invisible)
  → Show the tangible result (Output)

Examples:
  Stripe:    code snippet → Stripe API → payment confirmation
  Twilio:    API call → Twilio → phone rings on stage
  Terraform: HCL config → terraform apply → infrastructure graph
  Vercel:    git push → Vercel → deployed URL in 8 seconds
```

Visual asset types by effectiveness:

| Asset Type | Best For | Tools |
|-----------|----------|-------|
| **Code snippet** (syntax-highlighted) | Technical buyers, hero sections | Carbon.now.sh, Ray.so, Shiki |
| **Terminal recording** (GIF/video) | Technical buyers, "see it work" | VHS (charmbracelet), asciinema |
| **API request → response** | Technical buyers, feature proof | Hand-crafted HTML, Readme.io |
| **Architecture diagram** | All audiences, system overview | D2, Excalidraw, Mermaid, Structurizr |
| **Before/After flow diagram** | Investors, non-technical buyers | Figma, Whimsical |
| **Metrics growth chart** | Investors, traction slide | Recharts, Flourish, simple SVG |
| **Integration ecosystem map** | Non-technical buyers, partnerships | Figma, logo grid |
| **Live embed** | Technical buyers, "try it now" | CodeSandbox, StackBlitz |

**Choosing by audience:**

- **Investors (non-technical)**: metrics chart > before/after flow > customer logos > integration map. No raw code. They spend ~2 min on a deck — visuals must be instantly readable.
- **Technical buyers (engineers, CTO)**: code snippet > terminal GIF > API example > architecture diagram. They want to see it's real and evaluate adoption effort.
- **Non-technical buyers (PM, VP)**: before/after process flow > integration map > customer logos > metrics dashboard. Avoid terminal output.

**Auto-generating visuals from codebase:**

| What | Tool |
|------|------|
| Architecture diagram from code | Swark (LLM-based), Structurizr (C4 model) |
| API docs → interactive page | Readme.io, Bump.sh (from OpenAPI spec) |
| Terminal demo | VHS tape scripts (version-controllable) |
| Flowcharts from code | code2flow (Python) |
| Code snippets | Extract from README/examples/, syntax-highlight with Ray.so |

See `references/visual-assets.md` for templates and detailed tool guides.
See `references/frameworks.md` for JTBD templates and YC question lists.

### Screenshot Capture Pipeline (GUI Products)

When the scanner detects `frontend.has_ui = true`, the screenshot pipeline runs
automatically. It uses scanner output (framework, routes, nav element, auth) to
adapt to any project without hardcoded configuration.

The pipeline: detect/start frontend → build screenshot plan from routes → hide
navigation via CSS injection → anonymize data via DOM text replacement → capture
with Playwright → verify each screenshot (no nav, no real names, data loaded).

The screenshot plan is saved as `screenshot_plan.json` for future retakes.

Read `scripts/capture-screenshots.md` for the full agent prompt with step-by-step
instructions, input/output contracts, and Playwright code templates.
Also see `references/screenshot-automation.md` for gotchas and sync/deploy checklist.

---

## Phase 4: Review Gate

Before shipping, check all eight:

1. **Narrative coherence** — Every slide/section answers "Before/After gap"
2. **Data integrity** — All numbers real or sourced (no invented metrics)
3. **Seibel Seven** — Can you answer all 7 questions clearly? Email Test passed?
4. **Screenshot-text alignment** — Each screenshot proves the adjacent claim
5. **Audience match** — Deck = investor language; Landing = customer pain
6. **Anonymization** — Zero real names, emails, companies, repos
7. **Asset sync** — Source files = deployed files (check hashes)
8. **Live test** — Open deployed URL, scroll everything, all images load

---

## Invoking the Pipeline

When invoking this skill, you can run the full pipeline or jump to a specific phase:

```
Full pipeline:   "Create a pitch for this project"
                 → Run Phase 1 → 2 → 3 → 4

Update existing: "Add this new feature to the pitch"
                 → Skip to Phase 3 (distill the feature, update materials)

Review only:     "Check if the pitch is ready for investors"
                 → Skip to Phase 4 (review gate)

Discovery only:  "What does this product actually do?"
                 → Run Phase 1 only
```

## Anti-Patterns

| Anti-Pattern | Why It Fails | Fix |
|-------------|-------------|-----|
| Start with features, not pain | "We have X,Y,Z" ≠ "so what?" | Run Phase 2 JTBD first |
| Skip the scanner, assume you know | Misses features buried in code | Let the scanner find surprises |
| Same language for investors and customers | Different audiences, different fears | Use audience matrix |
| AI-generated pitch without user research | Stays at feature level, fails Mom Test | Phase 2 forces persona + emotional layer |
| Too many screenshots | Information overload | Each screenshot has ONE job |
| "No competitors" slide | Investors see naivety | 2x2 matrix with adjacent solutions |
| Fake-looking anonymized data | Round numbers, perfect scores | Keep real magnitudes + variance |
