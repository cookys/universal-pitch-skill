---
name: pitch-craft
description: >
  Universal pitch deck and landing page craft — narrative structure, selling point
  distillation, screenshot strategy, and conversion optimization. Use this skill
  whenever creating or updating pitch decks, landing pages, product marketing pages,
  or investor materials. Also invoke when translating product features into marketing
  language, retaking product screenshots, or preparing for investor/customer demos.
  If someone asks "how should we present this feature", "update the pitch", "make a
  landing page", or "prepare for the investor meeting", this is the skill to use.
---

# Pitch Craft

A framework for creating compelling pitch decks and landing pages for SaaS products.
Covers narrative structure, selling point distillation, screenshot strategy, and
pre-ship quality gates. Works for any stage (Seed through Series B) and any SaaS product.

## Narrative Architecture

### Pitch Deck Structure

Every slide answers one question: **"What's the Before/After gap?"**
If a slide has no clear gap, cut it.

#### Seed / Angel (10-12 slides)

```
1.  Cover          — One-sentence positioning (≤8 words)
2.  Problem        — Compress, don't delete. One real number, one scene, 20 sec.
3.  Solution       — How you fix it + proof screenshot
4.  Product        — Core features (2-3 screenshots max)
5.  Market         — TAM/SAM with visible calculation logic
6.  Traction       — Real numbers (users, revenue, engagement)
7.  Business Model — Pricing tiers, highlight recommended
8.  Competition    — 2x2 matrix on YOUR axes, don't name competitors
9.  Team           — Why THIS team executes
10. Ask            — Amount + allocation breakdown
```

#### Series A+ (15-18 slides)

Add after Traction: GTM strategy, unit economics (CAC/LTV/payback), financial projections, customer case studies.

#### Slide Count Wisdom

The "10 slides" rule (Kawasaki) is a Seed heuristic, not gospel. Series A investors want depth — 16-18 slides with GTM and financials is normal. The real rule: **every slide must earn its place with a clear Before/After gap.**

### Landing Page Structure

Visitors progress through a trust arc — each section earns permission for the next:

```
Hero         → "What is this?"       Establish category, single CTA
Pain Points  → "I have this problem" 3 cards, emotional hooks
Features     → "This solves it"      Alternating left/right, screenshots
Social Proof → "Others trust it"     Specific quotes (name+title, not stars)
How It Works → "It's easy"           3-4 steps
Gallery      → "It looks real"       Screenshot variety
Pricing      → "I can afford it"     3-tier, highlight middle plan
CTA          → "Let me try"          Single clear action
```

**Hero rules:**
- H1 ≤ 8 words, focused on outcome not feature
- Single primary CTA above the fold
- LCP < 2.5s (test it — slow hero = 90%+ bounce)
- Trust signal row (customer logos or key stats) below CTA

## Selling Point Distillation

Raw feature descriptions don't belong in pitch materials. Transform through three layers:

```
Layer 1: Feature → Capability
  Strip jargon. Ask: "Why does the user need this?"
  "Real-time WebSocket sync" → "Instant collaboration"

Layer 2: Capability → Emotional Outcome
  Ask: "What fear does this solve? What desire does this fulfill?"
  "Instant collaboration" → "Never worry about working on stale data"

Layer 3: Emotional Outcome → Narrative Hook
  Pick a frame:
  - Contrast:  "Before: email attachments / After: real-time sync"
  - Quantify:  "From 3-day review cycles to 5-minute feedback"
  - Social:    "Your whole team sees changes as they happen"
```

The reason this matters: technical audiences don't reject stories — they reject *empty* stories. Data wrapped in narrative is the most persuasive format. A feature list is forgettable; an emotional contrast backed by a number is not.

### Audience-Specific Framing

The same feature needs different language depending on who's reading:

| Dimension | Pitch Deck (Investors) | Landing Page (Customers) |
|-----------|----------------------|--------------------------|
| Opening | Market opportunity + team strength | Your pain, we understand |
| Evidence | TAM, CAC, NRR, competition matrix | User quotes, quantified results |
| CTA | "Invest $Xm" (strong signal) | "Free trial" (low risk) |
| Cadence | Problem → Market → Solution → Team → Ask | Pain → Solution → Proof → Action |
| Focus | Why WE can do this (team moat) | Why YOU need this (outcome) |

Technical B2B audiences (engineering managers) respond best to **data wrapped in narrative** — neither pure data nor pure story. Lead with a specific number, frame it in a concrete scenario.

## Screenshot Strategy

### The Screenshot Job Decision Tree

Before taking any screenshot, ask: **"What is this screenshot's JOB?"**

| Job | Treatment | Use for |
|-----|-----------|---------|
| Establish product category | Full-page, navigation hidden, show UI quality | Hero, first-impression slides |
| Prove a specific value claim | Crop to the UI region supporting the claim | Feature sections, demo slides |
| Create emotional anchor / wow | Spotlight (highlight key element, dim rest) | Differentiator slides, gallery |

### Navigation / Chrome Removal

For pitch and marketing screenshots, product navigation (sidebars, top bars) is typically visual noise — it takes up space without communicating value. Hide it unless the navigation structure itself is your differentiator.

Technique: inject CSS before screenshotting to hide nav elements and let content fill the viewport.

Exception: when your product's information architecture IS the selling point (e.g., a project management tool where the sidebar structure shows workflow integration).

### Dark Theme Products

Dark themes actually help with technical audiences — they match the IDE aesthetic and signal "this is a real dev tool." But they need care:

- Add subtle border or glow so screenshots don't melt into dark backgrounds
- Test on projectors at low brightness — ensure sufficient contrast
- Never place dark screenshots on white/light backgrounds
- Use dark gradient backgrounds in pitch materials to match

### Data Anonymization

If screenshots contain customer data, anonymize while preserving realism:

- **Names**: realistic fake names matching the locale
- **Numbers**: keep real magnitude (don't change 87 to 3 — audiences spot fake data)
- **Companies**: same-industry fictional names
- **Distribution**: maintain realistic variance (not all 100%, not all identical)

The goal is **"remove identifiers, preserve behavioral realism."** Perfect round numbers, identical scores, and too-clean distributions all signal "this is a demo" and erode trust.

### Screenshot-Text Pairing

Each screenshot needs companion text in three layers:

```
Top:    Verb + Result (6-12 chars)    "Ship 2x faster"
Middle: Quantified contrast (≤30 ch)  "From 3-day cycles to 5 minutes"
Bottom: CTA or emotional closure      "Start free →"
```

Visual rhythm on landing pages: alternate large screenshot → text → small screenshot → text to prevent scroll fatigue.

### Screenshot Automation (Playwright)

For products with many pages, automate screenshot capture:

```
1. Set consistent viewport (e.g., 1440x900)
2. Navigate to page
3. Wait for data load (networkidle + buffer)
4. Inject CSS to hide navigation
5. Inject anonymization script if needed
6. Take screenshot (viewport or fullPage)
```

Key pitfalls from real experience:
- `page.evaluate(stringFn)` silently fails — pass as function reference
- `cp -r` doesn't reliably overwrite existing directories — `rm -rf` first then copy
- If screenshots are baked into Docker images, rebuild with `--no-cache`
- Navigation collapse state often doesn't persist across page navigations — use CSS injection instead of UI clicks

See `references/screenshot-automation.md` for detailed workflow templates.

## Review Gate

Before shipping any pitch update, check all eight:

1. **Narrative coherence** — Every slide/section answers "Before/After gap"
2. **Data integrity** — All numbers real or clearly sourced (no invented metrics)
3. **Visual consistency** — Same viewport, same nav state, same anonymization everywhere
4. **Screenshot-text alignment** — Each screenshot proves the adjacent text's claim
5. **Audience match** — Deck speaks investor language; landing page speaks customer pain
6. **Anonymization complete** — Zero real names, emails, company names, or repo names
7. **Asset sync verified** — Source files = deployed files (check hashes if using build pipeline)
8. **Live test** — Open deployed URL, scroll entire page, all images load correctly

## Common Anti-Patterns

| Anti-Pattern | Why It Fails | Fix |
|-------------|-------------|-----|
| Feature list instead of value props | "We have X, Y, Z" doesn't answer "so what?" | Run 3-layer distillation on every feature |
| Too many screenshots | Information overload, no focus | Each screenshot has ONE job — cut the rest |
| "No competitors" on competition slide | Investors see naivety, not opportunity | Use 2x2 matrix with adjacent solutions |
| Same language for investors and customers | Different audiences, different fears | Use audience matrix to reframe |
| Showing full app UI in every screenshot | Navigation clutter, small content area | Hide nav, crop to the value-proving region |
| Anonymized data that looks fake | Round numbers, perfect scores → "it's a demo" | Keep real magnitudes, realistic variance |
| Problem slide as market education | Audience already knows the problem | Compress to one number + one scene, 20 sec |
