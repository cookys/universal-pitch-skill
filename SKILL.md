---
name: pitch-craft
description: >
  Codebase-to-pitch pipeline — scans any SaaS project, identifies personas and pain
  points, then generates pitch deck and landing page materials. Use this skill whenever
  creating or updating pitch decks, landing pages, investor materials, or marketing
  pages. Also invoke when translating product features into selling points, retaking
  product screenshots, or preparing for demos. Triggers: "make a pitch", "update the
  pitch deck", "create a landing page", "how should we present this feature", "prepare
  for the investor meeting", "what's our value proposition", "who are our users".
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

### Scanner Output Contract

```json
{
  "product": {
    "name": "",
    "one_liner": "≤15 words, what it does",
    "repo_url": "",
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
  "competitive_signals": "any competitor names found in docs/code"
}
```

### Scanner Prompt Template

```
Scan this codebase and extract product intelligence. Read these files in order:
1. README.md and any doc/ files (understand positioning)
2. Route/handler files (map features)
3. DB schema/migrations (understand data model)
4. Git log --oneline -100 (see recent work)
5. i18n/test files (find user-facing language)

Output the JSON structure above. Be factual — extract what IS, don't invent
what SHOULD BE. If a field can't be determined, write "unknown".
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

After the JTBD analysis, validate by answering Michael Seibel's seven questions:

1. **What do you do?** — Company name + one sentence + one concrete example
2. **How big is the market?** — TAM/SAM with transparent math, not Gartner quotes
3. **What's your traction?** — Numbers with time frame ("$0 to $10k MRR in 3 months")
4. **What's your unique insight?** — What do you know that others don't?
5. **What's your business model?** — How you make money, validated or assumed?
6. **Who's your team?** — Specific achievements, not titles
7. **What do you want?** — Exact amount + 18-24 month milestones

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
1. What we do: ...
2. Market size: ...
3. Traction: ...
4. Unique insight: ...
5. Business model: ...
6. Team: ...
7. Ask: ...
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
Hero         → "What is this?"       Full screenshot, ≤8-word H1, single CTA
Pain Points  → "I have this problem" 3 cards from JTBD emotional layer
Features     → "This solves it"      Alternating layout, cropped screenshots
Social Proof → "Others trust it"     Quotes with name+title (not star ratings)
How It Works → "It's easy"           3-4 steps
Gallery      → "It looks real"       Screenshot variety
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

### Screenshot Strategy

Before any screenshot, ask: **"What is this screenshot's JOB?"**

| Job | Treatment | Use for |
|-----|-----------|---------|
| Establish product category | Full-page, nav hidden | Hero, first impressions |
| Prove a specific value claim | Crop to supporting region | Feature sections |
| Create emotional anchor | Spotlight effect | Differentiator slides |

**Navigation**: hide by default (inject CSS). Exception: when nav structure IS the value.

**Dark themes**: fine for technical audiences. Add border/glow to prevent melting into dark backgrounds. Never place on white backgrounds.

**Anonymization**: remove identifiers, preserve behavioral realism. Keep real number magnitudes. Realistic variance (not all round numbers).

See `references/screenshot-automation.md` for Playwright templates and gotchas.
See `references/frameworks.md` for JTBD templates and YC question lists.

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
