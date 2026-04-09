# CodePower Pitch Deck Review — Investor Readiness Assessment

**Reviewer:** Claude (automated review)
**Date:** 2026-04-08
**File:** `/home/codepower/projects/codepower/doc/pitch/pitch-deck.html`
**Context:** Demo Day presentation, angel round fundraising

---

## Overall Recommendation: CONDITIONAL GO

The deck is substantially complete and visually polished. It covers the core narrative beats investors expect. However, there are several issues that range from "should fix before Demo Day" to "nice to have." The critical ones could undermine credibility during Q&A.

---

## Slide-by-Slide Analysis

### Slide 1 — Cover
- **Status:** OK
- Company name, tagline ("AI 時代的工程團隊需要新的管理方式"), round info (angel, 2026 Q2) all present.
- Minor: No specific funding amount mentioned on cover or anywhere in the deck. This is a significant omission for an angel pitch — investors want to know the ask size.

### Slide 2 — Problem (3 Pain Points)
- **Status:** Good
- Three clear pain points: code explosion, team cohesion loss, learning gap.
- The "3-10x" productivity claim is bold. Be prepared to cite a source or qualify it as directional.

### Slide 3 — Solution (Feature Overview)
- **Status:** Needs attention
- Six feature cards is a lot for one slide. Risk of feeling like a feature dump rather than a clear value proposition.
- **Issue:** The one-liner equation "CodePower = AI 產出透明化 + 遊戲化學習" is good, but the 6 cards dilute focus. Consider which 2-3 are the "hero" features and which are supporting.

### Slide 4 — Social Feed / Daily Standup
- **Status:** Good
- Screenshot included (02-activity-stream.png). Clear value prop: replaces daily standup.

### Slide 5 — AI Soul System
- **Status:** Risky
- The "AI pet" concept is creative but may confuse or distract investors focused on enterprise value. The screenshot references `17-settings.png` — a settings page is not the most compelling visual for this feature. A screenshot showing actual AI personality interactions in the feed would be stronger.
- **Issue:** This is slide 5 of 18 — prime real estate. The soul/pet system is a differentiator but not the core value. Consider moving it later or merging it into the gamification slide.

### Slide 6 — Personal Dashboard / FAIR Score
- **Status:** Good
- FAIR Score breakdown with weights is clear and transparent. This is core value — good placement.
- Minor: "7 維度雷達圖" in the AI Insight section but FAIR Score has 5 dimensions. The distinction between the two scoring systems (FAIR vs. AI Insight radar) could confuse viewers during a quick presentation.

### Slide 7 — AI Code Review
- **Status:** Good
- Strong slide. The "why it matters" section ties back to the problem well.
- **Issue:** Mentions supporting "Gemini / Claude" but the tech slide later says "4 LLM 引擎" and lists Gemini, Claude, OpenAI. These should be consistent.

### Slide 8 — Gamification
- **Status:** Good but dense
- Too many bullet points (6 features + psychology framework reference). For a live presentation, this will fly by too fast.

### Slide 9 — Arena
- **Status:** OK
- The "FAIR Score affects subsidy, Arena is just for fun" separation is smart and well-explained.

### Slide 10 — Team Management / Manager View
- **Status:** Needs attention
- **Issue:** The title says "AI 版 Daily Standup" — same phrase used in Slide 4's title. This creates confusion. Slide 4 is about the social feed (employee-facing), Slide 10 is about manager tools. They need distinct titles.
- The "saves 2-3 hours" claim is compelling — consider making it more prominent.

### Slide 11 — Scoring & Subsidy
- **Status:** OK
- Good transparency messaging. CSV export for HR integration is a practical selling point.

### Slide 12 — Tech Architecture
- **Status:** Needs attention
- **Issue:** "35 使用者" and "394 程式庫" — these are real numbers from your current deployment. For an angel pitch, 35 users at a single company (your own) is thin traction. Investors will ask: "Is this just used internally?" Be prepared with a narrative about why these numbers prove product-market fit signals rather than just internal dogfooding.
- **Issue:** "4 LLM 引擎" — the badge section lists Gemini and Claude but the card text says "Gemini、Claude、OpenAI、Gemini CLI." Gemini CLI is not a separate LLM engine; it is a CLI interface to the same Gemini model. Listing it as a 4th engine inflates the count and looks misleading if questioned.

### Slide 13 — Market
- **Status:** Needs significant improvement
- **Critical Issue:** TAM/SAM numbers lack sources. "2,800 萬軟體開發者" — from where? (GitHub's 2024 report says 100M+ accounts, but active developers are ~30M). "40% adopting AI coding by 2026" — source?
- **Critical Issue:** No SOM (Serviceable Obtainable Market). You jump from SAM (150K companies in APAC) to "no direct competitor" without saying how many you can realistically reach in 12-18 months.
- **Issue:** "沒有直接競品" is a red flag statement for investors. Either (a) the market does not exist, or (b) you have not researched competitors well enough. Competitors to acknowledge: LinearB, Jellyfish, Pluralsight Flow (engineering analytics), Waydev, CodeClimate. Position CodePower as differentiated (gamification + AI coding era focus) rather than claiming no competition.

### Slide 14 — Business Model
- **Status:** Good structure, questionable pricing
- $5/user/month Starter and $15/user/month Team are reasonable for SMB. Enterprise is "contact us" — standard.
- **Issue:** No revenue projection or unit economics. At $15/user for a 50-person team, that is $750/month ($9K/year). How many customers to break even? What is the CAC assumption? Investors will ask.

### Slide 15 — Roadmap
- **Status:** Needs attention
- Q1 "MVP done" is good — shows traction.
- **Issue:** "Open Source + SaaS" in Q2 is a major strategic decision that gets one bullet point. Open-sourcing the core has enormous implications (community vs. monetization tension). Investors will have strong opinions. Be prepared to defend this strategy and explain the moat if the core is open source.
- **Issue:** "取得首批 5 家付費客戶" in Q2 — is this aspirational or committed? If aspirational, the deck should be honest about current pipeline status.

### Slide 16 — Team
- **Status:** Critical weakness
- **Critical Issue:** Solo founder. One person listed. For an angel round, this is the #1 concern investors will raise. The "built in 3 weeks" narrative is impressive but also signals: what happens if Cookys gets sick? What about sales, marketing, customer success?
- **Suggestion:** If there are advisors, co-founders in discussion, or committed early hires, they should be on this slide. If truly solo, address the "key person risk" proactively.

### Slide 17 — The Ask
- **Status:** Incomplete
- **Critical Issue:** No funding amount specified. "天使輪" and allocation percentages (40/35/25) are there, but 40% of WHAT? Investors need a specific number. Even a range (e.g., "$150K-$300K USD") is better than nothing.
- **Issue:** No stated valuation or terms structure (SAFE, convertible note, priced round).

### Slide 18 — CTA / Closing
- **Status:** OK
- Contact info present. Live demo link included. Clean closing.

---

## Structural Issues

| Issue | Severity | Recommendation |
|-------|----------|----------------|
| No funding amount on Ask slide | **CRITICAL** | Add specific dollar amount or range |
| Solo founder with no team plan | **CRITICAL** | Add advisors/planned hires, or address key-person risk |
| "No competitors" claim | **HIGH** | Reframe as "no direct competitor in AI-era engineering gamification" and acknowledge adjacent players |
| TAM/SAM without sources or SOM | **HIGH** | Add citation sources and include SOM with 18-month realistic target |
| Duplicate title "AI 版 Daily Standup" on slides 4 and 10 | **MEDIUM** | Rename slide 10 to differentiate manager view |
| 18 slides is long for Demo Day | **MEDIUM** | Most Demo Days allow 3-5 minutes. 18 slides at that pace = 10-17 seconds per slide. Consider cutting to 12-14 or identifying which slides to skip in a short format |
| No revenue/unit economics | **MEDIUM** | Add a simple model: target customers x ARPU = projected ARR |
| LLM engine count inconsistency | **LOW** | Standardize to 3 (Gemini, Claude, OpenAI) or clarify what counts |
| FAIR Score vs AI Insight radar confusion | **LOW** | Clarify distinction or simplify for pitch context |

---

## What Works Well

1. **Visual quality** — The dark theme with teal/purple accents is professional and distinctive. CSS-only deck (no PowerPoint dependency) is technically impressive.
2. **Problem framing** — The three pain points are timely and resonate with anyone managing engineering teams adopting AI tools.
3. **Product screenshots** — Real product screenshots (23 files in screenshots/) demonstrate this is a working product, not a mockup.
4. **FAIR Score transparency** — The public formula approach addresses the "black box AI scoring" concern preemptively.
5. **Dual-track design** — Separating FAIR Score (subsidy/compensation) from Arena (gamification/fun) is smart product design and easy to explain.
6. **Localization** — Full zh-TW deck appropriate for Taiwan investor audience.

---

## Recommended Action Items Before Demo Day

### Must Fix (before presenting)
1. Add a specific funding amount to Slide 17 (The Ask).
2. Remove or rephrase the "no direct competitor" claim on Slide 13. Replace with competitive positioning.
3. Add TAM/SAM source citations (even as small footnote text).

### Should Fix (high impact, moderate effort)
4. Add at least advisors or planned team composition to Slide 16.
5. Rename Slide 10 title to avoid duplication with Slide 4.
6. Prepare a 12-slide "short version" order for 5-minute Demo Day slots (suggested cut: merge slides 5+8, drop slide 11, condense slide 12).

### Nice to Have
7. Add a simple revenue projection slide or append numbers to the business model slide.
8. Fix the "4 LLM engines" count to be accurate.
9. Add SOM to the market slide.

---

## Verdict

**CONDITIONAL GO** — The deck is presentable and the product is real, which puts you ahead of most angel-stage pitches. However, the three "Must Fix" items (funding amount, competitor framing, market sources) are the kind of gaps that experienced investors will probe immediately. Fix those three and this deck is ready for Demo Day.
