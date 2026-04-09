# CodePower Pitch Deck Plan for Angel Investors

## Product Understanding

**CodePower** is an AI-powered engineering intelligence platform built with Vue 3 + Rust. It solves the management gap created by AI-assisted coding (Cursor, Claude Code, Copilot) by automatically scanning GitLab/GitHub repositories, generating AI code reviews, and gamifying developer contributions.

**Core value proposition:** When AI agents multiply every engineer's output 3-10x, traditional management (reading commits, daily standups, manual code review) breaks down. CodePower replaces all of that with automated AI analysis, transparent scoring, and game mechanics.

**Current traction:** 35 users, 394 repositories scanned, 102 database migrations (mature product), 9 AI personality souls, 26 programming languages supported via AST analysis, bilingual (English + Traditional Chinese).

---

## Pitch Deck Structure (18 Slides)

### Slide 1: Cover
- **Title:** CodePower
- **Tagline:** "Engineering teams in the AI era need a new way to manage"
- **Context:** Stranity | Angel Round | 2026 Q2
- **Design:** Dark gradient background, large gradient logo text (teal-to-purple), minimal

### Slide 2: The Problem
- **Title:** "Three Pain Points of the Agent Coding Era"
- **Subtitle:** When AI makes every engineer 3-10x more productive, management can't keep up
- **Pain Point 1 -- Code Explosion:** AI agents produce thousands of lines per day. Managers can't read all commits. Traditional code review collapses.
- **Pain Point 2 -- Team Cohesion Breakdown:** Everyone works in their own AI bubble. Teams lose communication, belonging, and shared context. Code becomes a black box.
- **Pain Point 3 -- Learning Gap:** Engineers who adopt AI coding take off; those who don't get left behind entirely. Organizational transformation stalls.
- **Why this matters for investors:** This is a *new* category of problem created by the AI coding wave. It didn't exist 18 months ago.

### Slide 3: The Solution
- **Title:** CodePower = AI Output Transparency + Gamified Learning
- **Subtitle:** Auto-scan GitLab/GitHub, AI-analyze every engineer's code contributions, game mechanics drive self-directed learning
- **Six feature cards:**
  1. **AI Auto-Analysis** -- Daily scan of all commits, LLM-generated code review, 5-dimension FAIR Score
  2. **Arena Leaderboard** -- EXP system + 6-dimension skill tree + seasonal rankings
  3. **Badges & Achievements** -- 18 achievement types + rarity system + reward shop
  4. **Manager Dashboard** -- Team activity trends, member scores, subsidy amounts at a glance
  5. **AI Q&A** -- Managers ask natural-language questions ("Who performed best this week?")
  6. **AI Soul Battle** -- Choose an AI pet personality (9 types) that reviews code and generates insights in character

### Slide 4: Social Feed (Demo Slide with Screenshot)
- **Title:** "Social Activity Wall -- AI-Powered Daily Standup"
- **Key message:** Replaces daily standup meetings. Commits auto-become social cards. Cross-team visibility.
- **Features to highlight:** AI-generated daily summaries, AST code signal tags, daily XP + daily duel challenges, AI soul interactions on the feed
- **Screenshot:** Activity stream view
- **Takeaway:** Breaks the "everyone works in isolation" silo of the AI era

### Slide 5: AI Soul System (Demo Slide with Screenshot)
- **Title:** "AI Soul System -- Raise Your AI Battle Pet"
- **Key message:** 9 AI personalities (snarky cat, zen master, fox coach...) that review code in character and interact with each other
- **Unique angle:** Code review becomes entertainment. Your "Roaster" argues with a colleague's "Sage" -- the whole company watches. Social content auto-generates.
- **Screenshot:** Settings/soul selection view

### Slide 6: Personal Dashboard (Demo Slide with Screenshot)
- **Title:** "Personal Dashboard -- Your Developer Profile"
- **Key message:** FAIR Score 5-dimension quantification + AI insight radar chart + contribution heatmap
- **FAIR Score breakdown:** Activity 40%, Consistency 15%, Breadth 10%, Quality 25%, Impact 10%
- **Screenshot:** Personal overview page

### Slide 7: AI Code Review (Demo Slide with Screenshot)
- **Title:** "AI Code Review -- Beyond Just Scores"
- **Key message:** Daily LLM review of every commit across all members. 5-dimension radar chart per repo. Risk warnings + improvement suggestions. 26-language AST structural analysis.
- **Why it matters:** AI-generated code quality is inconsistent. CodePower lets managers "know quality without reading code" while giving engineers concrete improvement paths.
- **Screenshot:** Code review page

### Slide 8: Gamification Engine (Demo Slide with Screenshot)
- **Title:** "Gamification -- Make Learning Addictive"
- **Key message:** Full game engine: daily duels, skill trees, leveling (LV 1-999), 18 badge types, custom quests, reward shop, seasonal resets
- **Psychological basis:** Built on Octalysis framework. Daily challenges give you a reason to interact with colleagues. Custom quests let managers gamify management goals.
- **Screenshot:** Achievements/skill tree page

### Slide 9: Arena (Demo Slide with Screenshot)
- **Title:** "Arena -- High Output Catches Up"
- **Key message:** Historical repo contributions auto-convert to EXP. Sprint hard enough and newcomers can overtake veterans. Separate from FAIR Score (games are games, subsidies are subsidies).
- **Screenshot:** Arena leaderboard

### Slide 10: Team Management (Demo Slide with Screenshot)
- **Title:** "Manager View -- AI Daily Standup"
- **Key message:** 5 minutes/day to grasp everything. AI-generated text standup, quality x productivity quadrant chart, custom quests/badges, AI Q&A, scoring matrix + CSV export.
- **Time savings:** From 2-3 hours reviewing commits + 30-min standup to 5-minute dashboard scan.
- **Screenshot:** Team members view

### Slide 11: Scoring & Subsidy (Demo Slide with Screenshot)
- **Title:** "Scoring & Subsidy -- Fair and Transparent"
- **Key message:** Formula is public, weights are transparent, every employee can see how scores are calculated. Band system (B1-B4), automatic subsidy amount suggestions, anomaly detection, CSV export to HR systems.
- **Screenshot:** Scoring matrix view

### Slide 12: Technical Architecture
- **Title:** "Technology"
- **Stats bar:** 26 languages (AST), 394 repos scanned, 35 users, 4 LLM engines
- **Four cards:**
  1. Backend -- Rust (Axum + SQLx + SQLite): <200MB RAM, zero GC latency, scheduled scanning + historical backfill + CLI mode
  2. Frontend -- Vue 3 (TypeScript + Pinia + TailwindCSS): Neumorphism dark theme, bilingual i18n
  3. AI -- Multi-LLM (Gemini, Claude, OpenAI): Different models for different tasks (cheap for daily, smart for weekly)
  4. Deploy -- Docker: One-command deploy, zero-ops SQLite, GitLab OAuth + JWT + 3-tier auth
- **Tech badges:** Rust, Axum, SQLite, Vue 3, TypeScript, TailwindCSS, Tree-sitter, Docker, GitLab API, GitHub API, Gemini, Claude

### Slide 13: Market Opportunity
- **Title:** "Market Opportunity"
- **Subtitle:** Agent Coding is reshaping software development, but management tooling is blank
- **TAM:** 28 million software developers globally. ~40% of enterprises adopting AI coding in 2026, accelerating.
- **SAM:** Asia-Pacific mid-to-large tech companies (50+ engineers), ~150,000 companies. AI subsidy management is a new category -- no direct competitors.
- **Entry point:** Taiwanese enterprises are mass-adopting AI coding subsidies (Cursor, Claude Code, Copilot) and urgently need management tools.

### Slide 14: Business Model
- **Title:** "Business Model"
- **Subtitle:** SaaS subscription -- per-seat pricing, simple and transparent
- **Starter ($5/user/mo, up to 20):** GitLab/GitHub scanning, FAIR Score, personal dashboard, activity wall
- **Team ($15/user/mo, up to 200, featured):** All Starter + AI Code Review, Arena + gamification, manager dashboard, AI Q&A, subsidy management
- **Enterprise (custom pricing):** All Team + on-premise deploy, SSO/LDAP, custom LLM, API integrations, dedicated support, unlimited seats

### Slide 15: Roadmap
- **2026 Q1 (Done):** MVP complete. 35 users, 394 repos, daily auto-scan. 70+ feature iterations completed.
- **2026 Q2:** Open-source core engine (Apache-2.0). Launch SaaS hosted version. First 5 paying customers.
- **2026 Q3:** Multi-platform (Bitbucket, Azure DevOps, Jira). Slack/Teams bot daily push. CI/CD pipeline quality tracking.
- **2026 Q4:** Enterprise package + on-premise. SSO integration. Customizable scoring weights per team. AI personalized learning path recommendations.

### Slide 16: Team
- **Cookys Lin -- Founder & CTO**
- Founder of Stranity. Full-stack engineer specializing in Rust, Vue, AI system integration.
- CodePower built from zero to full product in 3 weeks (best proof-point for AI-assisted development).
- **Narrative angle:** The founder dogfoods their own thesis -- AI-assisted development works, and they've built the tool to prove it.

### Slide 17: The Ask
- **Title:** "Fundraising"
- **Subtitle:** Angel round, for SaaS-ification + first 50 customer acquisition
- **Allocation:**
  - 40% Product Development -- SaaS multi-tenancy + enterprise features
  - 35% Market Expansion -- Taiwan + Asia-Pacific tech companies
  - 25% Team Building -- Frontend + sales + customer success

### Slide 18: Call to Action
- **Headline:** "Make engineering teams in the AI era more transparent, more fun, more powerful"
- **Subtitle:** CodePower -- AI-Powered Engineering Intelligence
- **CTA button:** Experience CodePower (link to live demo)
- **Contact:** cookys@stranity.com | codepower.stranity.org

---

## Narrative Arc Analysis

The deck follows this persuasion flow:

1. **Urgency (Slide 2):** AI coding creates an immediate, visceral problem managers face *right now*
2. **Solution overview (Slide 3):** Quick 6-card snapshot of the answer
3. **Unique hooks (Slides 4-5):** The two most differentiated features (social feed replacing standups, AI soul battle pets) get dedicated slides to build excitement
4. **Product depth (Slides 6-11):** Six demo slides with screenshots proving this is a real, complete product -- not vaporware
5. **Credibility (Slide 12):** Technical architecture shows engineering depth (Rust, AST analysis, multi-LLM)
6. **Opportunity (Slides 13-14):** Market size and monetization path
7. **Momentum (Slide 15):** Roadmap shows they're already past MVP
8. **Trust (Slides 16-17):** Team and ask
9. **Close (Slide 18):** CTA with live demo link

---

## Key Selling Points for Angel Investors

### 1. New Category, Perfect Timing
AI coding tools (Cursor, Copilot, Claude Code) are the fastest-adopted developer tools in history. Every company buying these subscriptions now has zero tooling to measure ROI or manage the transition. CodePower is the first mover in "AI coding management."

### 2. Already Built and Running
This is not a pitch for a concept. The product has 102 database migrations, 30+ views, 9 AI personalities, 26-language AST analysis, gamification engine, subsidy management, and is actively used by 35 people scanning 394 repos daily. The existing pitch deck includes real screenshots.

### 3. Built in 3 Weeks = AI-Powered Development Proof
The founder built the entire platform in ~3 weeks using AI-assisted development. This is simultaneously the product AND the proof that AI coding works -- and therefore that the management problem it solves is real.

### 4. Open Source + SaaS Hybrid Model
Apache-2.0 open-source core with SaaS hosted version is a proven go-to-market strategy (GitLab, Sentry, PostHog model). Reduces enterprise adoption friction while capturing revenue from hosted convenience.

### 5. Gamification as Moat
The gamification layer (souls, battles, skill trees, seasonal rankings, daily duels) creates sticky engagement that pure analytics tools can't match. Engineers *want* to use it vs. being forced to.

### 6. Low Infrastructure Cost
SQLite + Rust means extremely low hosting costs per customer. No PostgreSQL cluster, no Redis, no complex infrastructure. Single Docker container deployment. This gives excellent unit economics at scale.

---

## Competitive Landscape

| Competitor | What They Do | CodePower Differentiator |
|-----------|-------------|------------------------|
| LinearB | Engineering metrics | No gamification, no AI code review, no subsidy management |
| Pluralsight Flow | Dev analytics | No AI analysis, no game mechanics, enterprise-only pricing |
| Waydev | Git analytics | Analytics only, no engagement layer |
| Sleuth | DORA metrics | Focused on deployment, not individual contribution |
| **CodePower** | **AI analysis + gamification + subsidy management** | **First to combine AI code review with game mechanics in the agent coding era** |

---

## Suggested Improvements for the Deck

1. **Add a "before/after" comparison slide** -- Show a day-in-the-life comparison of a manager with vs. without CodePower (concrete time savings)
2. **Include actual usage metrics** -- If available: daily active users, average session time, review completion rate, subsidy processed volume
3. **Add a competitive landscape slide** -- The market section says "no direct competitors" but investors will want to see adjacent players mapped
4. **Quantify the ask amount** -- Slide 17 shows percentage allocation but doesn't state the total raise amount (e.g., "$500K angel round")
5. **Add social proof** -- Even one quote from a current user/manager about time saved would significantly strengthen credibility
6. **Localize for investor audience** -- The current deck is in Traditional Chinese. For international angel investors, prepare an English version with the same structure

---

## Technical Depth Worth Mentioning in Conversations

These details won't fit on slides but are useful for investor Q&A:

- **FAIR Score v3.1** with versioned formulas for historical reproducibility
- **5 signal backends:** Commit analysis, AST structural analysis (tree-sitter), Git-native metrics (churn, survivability, bus factor), Review activity, LLM full review (two-tier: daily RAG + weekly trend)
- **Simpson's Diversity Index** for measuring breadth across programming domains
- **Identity system:** 1 user maps to N GitLab accounts maps to M emails, with admin merge queue
- **Agent Gateway:** Abstraction layer supporting gemini-cli, claude-code, codex-cli for manager Q&A via SSE
- **Repo Mirror:** Local bare repo mirroring (via git2) for survivability and bus factor analysis
- **102 database migrations** -- this is a mature, well-iterated product
- **Pipeline Health system** with automatic repair and CLI rebuild tools
