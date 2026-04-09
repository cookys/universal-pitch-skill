# Pitch Deck Review Gate — CodePower

**Deck**: `doc/pitch/pitch-deck.html` (18 slides)
**Date**: 2026-04-08
**Context**: Demo Day preparation
**Verdict**: **CONDITIONAL GO** — 3 blockers must be fixed before presenting

---

## 8-Point Review Gate

### 1. Narrative Coherence — Every slide answers "Before/After gap"

**Rating: PASS with notes**

The deck follows a clear arc: Problem (slide 2) → Solution (slide 3) → Feature deep-dives (slides 4-11) → Tech (12) → Market (13) → Business model (14) → Roadmap (15) → Team (16) → Ask (17) → CTA (18).

The Before/After gap is strong on slides 2-3 (pain points → solution) and slide 10 ("2-3 hours reviewing commits → 5 minutes scanning dashboard").

**Issues:**
- **Slides 4-9 (6 consecutive feature deep-dives) break narrative momentum.** This is too many feature slides for a 10-minute pitch. The audience will lose the thread. Recommend consolidating to 3 feature slides max: (a) AI Analysis + Dashboard, (b) Gamification + Arena, (c) Social Feed + Soul System.
- Slide 4 title "AI 版 Daily Standup" and Slide 10 title "AI 版 Daily Standup" use the **exact same phrase** on two different slides. This is confusing — slide 4 is about the social feed, slide 10 is about manager tools. One of them needs a different title.
- The Cover (slide 1) tagline "AI 時代的工程團隊需要新的管理方式" is a statement, not a positioning hook. Compare to the CTA slide's "更透明、更有趣、更強大" which is much stronger. Consider swapping.

### 2. Data Integrity — All numbers real or sourced

**Rating: FAIL (BLOCKER)**

- Slide 12 claims **394 已掃描程式庫** and **35 使用者** and **4 LLM 引擎**. These appear to be real internal metrics — good.
- Slide 13 (Market): "全球 2,800 萬軟體開發者" — this number is plausible (Statista/GitHub surveys cite 27-30M), but **no source is cited**. "導入 AI Coding 的企業 2026 年預估佔 40%" — **no source**. "亞太區中大型科技公司（50+ 工程師），約 15 萬家" — **no source or calculation logic visible**.
- The skill spec says market slide needs "TAM/SAM with visible calculation logic." Currently the math is opaque.

**Fix required**: Add source citations for TAM/SAM claims, or show the math (e.g., "28M devs x 40% AI adoption x $15/mo = $X TAM"). Investors will challenge unsourced market numbers.

### 3. Seibel Seven — Can you answer all 7 clearly? Email Test passed?

**Rating: PARTIAL PASS**

| # | Question | Status |
|---|----------|--------|
| 1 | What do you do? | PASS — "AI 自動掃描 GitLab/GitHub，量化工程師貢獻，遊戲化學習" (clear from slides 1-3) |
| 2 | How big is the market? | FAIL — Numbers given but unsourced (see #2 above) |
| 3 | What's your traction? | PASS — 35 users, 394 repos, 70+ iterations, MVP live |
| 4 | What's your unique insight? | PASS — "AI makes code volume explode; managers can't keep up" is a strong insight |
| 5 | What's your business model? | PASS — 3-tier SaaS, per-user pricing, clearly shown |
| 6 | Who's your team? | WEAK — Single founder card with minimal bio. No mention of domain expertise in engineering management, no prior startup outcomes, no advisory board |
| 7 | What do you want? | FAIL — **No fundraising amount is stated.** Slide 17 shows allocation percentages (40/35/25) but never says how much money is being raised. This is a critical omission for an investor pitch. |

**Email test**: The one-liner "AI 時代的工程團隊需要新的管理方式" is too vague. A better email-test version: "CodePower 用 AI 掃描 Git，自動 review code、量化貢獻、遊戲化學習 — 讓主管 5 分鐘掌握全貌" (more specific, passes the "would a smart friend understand?" test).

### 4. Screenshot-Text Alignment — Each screenshot proves the adjacent claim

**Rating: PASS**

All 8 screenshots referenced in the HTML exist on disk. The screenshot-to-claim mapping is logical:
- Slide 4: Activity stream screenshot ↔ social feed description ✓
- Slide 5: Settings/Soul screenshot ↔ AI personality system ✓
- Slide 6: Personal dashboard screenshot ↔ FAIR Score description ✓
- Slide 7: Code review screenshot ↔ AI review description ✓
- Slide 8: Achievements screenshot ↔ gamification description ✓
- Slide 9: Arena screenshot ↔ leaderboard description ✓
- Slide 10: Team members screenshot ↔ manager dashboard ✓
- Slide 11: Scoring matrix screenshot ↔ subsidy mechanism ✓

**Minor note**: Slide 5 uses `17-settings.png` for the Soul system. A dedicated Soul/personality selection screenshot would be more compelling than a generic settings page.

### 5. Audience Match — Deck = investor language; Landing = customer pain

**Rating: PASS with notes**

The deck is appropriately investor-focused: market sizing, business model, traction, roadmap, ask. The language leans toward showing opportunity rather than just pain resolution.

**Issues:**
- Some slides use customer-facing language where investor language would be better. Slide 8 says "讓學習上癮" (make learning addictive) — investors want to hear "high engagement → low churn → strong NRR."
- No unit economics mentioned anywhere. Even at angel stage, showing "35 users x $15 = potential $525 MRR if converted" demonstrates commercial thinking.
- No competition slide. The skill spec explicitly calls for a "2x2 matrix on YOUR axes." Saying "沒有直接競品" (no direct competitors) in passing on the SAM card is not enough — investors see this as naive. Adjacent competitors exist: LinearB, Jellyfish, Waydev, Pluralsight Flow. A 2x2 with axes like "AI-native analysis" vs "Gamification depth" would position CodePower uniquely.

### 6. Anonymization — Zero real names, emails, companies, repos

**Rating: CANNOT FULLY VERIFY (BLOCKER)**

The HTML source itself contains one real name: **"Cookys Lin"** on the Team slide (slide 16) — this is intentional and appropriate for a founder bio.

Contact info on slide 18: **cookys@stranity.com** — intentional for CTA.

**However, the screenshots cannot be verified from HTML alone.** The filenames suggest they are actual product screenshots. If this is a production system with real user data:
- Activity stream (02) may contain real employee names, commit messages, repo names
- Team members (09) may show real names and scores
- Scoring matrix (10) may show real names linked to subsidy amounts
- Arena (07) may show real user rankings

**Fix required**: Visually inspect every screenshot for real employee names, email addresses, internal repo names, or company-specific data. The skill spec warns: "remove identifiers, preserve behavioral realism."

### 7. Asset Sync — Source files = deployed files

**Rating: CANNOT VERIFY**

This check requires comparing the local `doc/pitch/pitch-deck.html` against whatever is deployed (if anything). The `git status` shows the deck is tracked in the repo. No deployed URL was provided for comparison.

**Action needed**: If presenting from a deployed URL (e.g., on stranity.org), verify the deployed version matches the repo version before Demo Day. If presenting from local file, this is N/A.

### 8. Live Test — Open deployed URL, scroll everything, all images load

**Rating: CANNOT VERIFY FROM CODE REVIEW**

This requires opening the HTML in a browser. From the source analysis:
- All 8 image `src` paths use relative paths (`screenshots/xxx.png`) — correct if the HTML is served from `doc/pitch/`.
- All referenced screenshots exist on disk.
- The CSS uses Google Fonts import (`fonts.googleapis.com`) — requires internet connection during presentation.
- `scroll-snap-type: y mandatory` is set — this should work for slide-by-slide scrolling in modern browsers.

**Action needed**: Before Demo Day, open `pitch-deck.html` in the presentation browser (Chrome recommended), scroll through all 18 slides, verify all images render and scroll-snap works. Test with and without internet (Google Fonts fallback is system-ui, which is acceptable).

---

## Summary

| Check | Status |
|-------|--------|
| 1. Narrative coherence | PASS (with structural notes) |
| 2. Data integrity | **FAIL** — unsourced market numbers |
| 3. Seibel Seven | **FAIL** — no fundraising amount, weak team slide |
| 4. Screenshot-text alignment | PASS |
| 5. Audience match | PASS (with notes) |
| 6. Anonymization | **CANNOT VERIFY** — screenshots need visual inspection |
| 7. Asset sync | N/A (local presentation assumed) |
| 8. Live test | NEEDS MANUAL CHECK |

## Verdict: CONDITIONAL GO

### Must fix before Demo Day (blockers):

1. **Add the fundraising amount** to slide 17. "天使輪 $X 萬" — investors need to know the number.
2. **Add market number sources** or show calculation logic on slide 13. Unsourced TAM/SAM kills credibility.
3. **Visually inspect all 8 screenshots** for real employee data. Any PII leak in a public pitch is a trust destroyer.

### Should fix (high-impact improvements):

4. **Add a Competition slide** (2x2 matrix). Position against LinearB, Jellyfish, Waydev, Pluralsight Flow on axes like "AI-native" vs "Gamification/Engagement."
5. **Consolidate feature slides** from 6 to 3. At 18 slides, the deck is long for a 10-minute Demo Day slot. Cut to 12-14 slides.
6. **Fix duplicate title** — slides 4 and 10 both say "AI 版 Daily Standup."
7. **Strengthen team slide** — add domain expertise, advisory board, or "why you?" differentiator.
8. **Sharpen the cover tagline** — make it specific and memorable, not generic.

### Nice to have:

9. Add one unit economics data point (even projected).
10. Replace `17-settings.png` on the Soul slide with a more compelling Soul-specific screenshot.
11. Do a live scroll-through test with the actual presentation device/browser.
