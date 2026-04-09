# git-scanner: Pitch Deck & Landing Page Plan

---

## Part 1: Pitch Deck (10 Slides)

---

### Slide 1 — Title

**git-scanner**
Stop secrets from leaking. Catch credentials before they ship.

- Tagline: "The CLI-native secret scanner built for CI/CD pipelines"
- Logo / terminal icon
- Presenter name, date

---

### Slide 2 — The Problem

**Secrets in code are the #1 cause of preventable breaches.**

- 2023: Over 10 million secrets detected on public GitHub repos (GitGuardian report)
- Average cost of a credential leak incident: $4.45M (IBM Cost of a Data Breach 2023)
- Developers commit AWS keys, SSH keys, and API tokens daily — often unknowingly
- Most teams discover leaked secrets *after* they've been exploited

Key stat callout: "60% of breaches involve compromised credentials" — Verizon DBIR

---

### Slide 3 — Why Existing Solutions Fall Short

| Pain Point | Current Tools |
|---|---|
| Heavy GUI overhead | Enterprise SAST tools require dashboards, onboarding, browser sessions |
| Poor CI/CD integration | Bolt-on scanning breaks pipelines, slows builds |
| High false positive rate | Generic regex patterns flood teams with noise |
| Expensive per-seat licensing | $15-50/dev/month for features most teams don't need |

**Security teams at mid-size companies (50-500 devs) need something that fits their workflow, not the other way around.**

---

### Slide 4 — The Solution: git-scanner

**A focused, CLI-first secret scanner that integrates natively into any CI/CD pipeline.**

- 15 battle-tested secret patterns (AWS keys, SSH private keys, API tokens, database URIs, JWTs, etc.)
- JSON output for easy parsing and integration
- Zero GUI — runs where your code runs: terminals, CI runners, pre-commit hooks
- Single binary, no runtime dependencies

```bash
$ git-scanner scan --format json ./repo
{
  "findings": [
    {
      "type": "aws_access_key",
      "file": "config/deploy.yml",
      "line": 42,
      "severity": "critical"
    }
  ],
  "summary": { "total": 3, "critical": 1, "high": 2 }
}
```

---

### Slide 5 — Key Features

1. **15 Secret Patterns** — AWS access/secret keys, SSH private keys, GitHub/GitLab/Slack tokens, generic API keys, database connection strings, JWTs, private keys (PEM), Stripe keys, Twilio credentials, SendGrid keys, Heroku API keys
2. **JSON-First Output** — Machine-readable results for dashboards, SIEM, ticketing integration
3. **CI/CD Native** — GitHub Actions, GitLab CI, Jenkins, CircleCI — drop-in YAML config
4. **Pre-Commit Hook** — Block secrets before they enter history
5. **Incremental Scanning** — Scan only changed files (diff mode) for fast pipeline runs
6. **Configurable Allowlists** — `.git-scanner.yml` to suppress known false positives
7. **Exit Codes** — Non-zero on findings for pipeline gate enforcement

---

### Slide 6 — How It Works (Architecture)

```
Developer commits code
        │
        ▼
┌─────────────────┐
│  Pre-commit hook │ ← Optional local gate
│  (git-scanner)   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│   CI/CD Runner   │ ← Pipeline gate (required)
│  git-scanner     │
│  --format json   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  JSON Output     │ → SIEM / Slack / Jira / Dashboard
└─────────────────┘
```

- Scan engine: pattern matching with context-aware validation (reduces false positives)
- Entropy analysis for generic high-entropy strings
- Git-aware: scans commit history, not just working tree

---

### Slide 7 — Target Market & Positioning

**Primary:** Security teams at mid-size companies (50-500 developers)

| Segment | Need | git-scanner Fit |
|---|---|---|
| Security engineers | Enforce secret hygiene across all repos | Pipeline gate + JSON reporting |
| DevOps / Platform teams | Non-disruptive CI integration | Single binary, fast scans, exit codes |
| Compliance officers | Audit trail for secret scanning | JSON logs, historical scan mode |

**Positioning:** Not competing with full SAST suites. git-scanner is the focused, fast, affordable secret-scanning layer that slots into existing toolchains.

**TAM:** ~45,000 mid-size tech companies globally. At $200-500/month team pricing = $108M-$270M addressable market.

---

### Slide 8 — Competitive Landscape

| Feature | git-scanner | GitGuardian | TruffleHog | Gitleaks |
|---|---|---|---|---|
| Secret patterns | 15 curated | 350+ | 700+ | 150+ |
| CI/CD native | Yes (first-class) | Yes | Yes | Yes |
| JSON output | Native | API | Yes | Yes |
| False positive rate | Low (context-aware) | Medium | Medium-High | Medium |
| Pricing | Affordable flat rate | $$$$ per-dev | Free/Enterprise | Free |
| Setup time | < 5 minutes | Hours | 15 min | 10 min |
| GUI required | No | Yes (dashboard) | No | No |

**Differentiation:** Curated pattern set (quality over quantity), lowest false positive rate in class, simplest CI integration, transparent flat-rate pricing.

---

### Slide 9 — Business Model & Pricing

| Tier | Price | Includes |
|---|---|---|
| **Open Core** | Free | CLI tool, 15 patterns, JSON output, pre-commit hook |
| **Team** | $299/mo | Unlimited devs, priority patterns update, Slack/Jira integration configs, email support |
| **Enterprise** | $799/mo | Custom patterns, historical full-repo scan, SSO/SAML audit log export, dedicated support |

Revenue drivers:
- Land with free tier in CI pipelines → expand to Team when org standardizes
- Enterprise upsell for compliance-driven organizations (SOC2, ISO 27001)

---

### Slide 10 — Call to Action

**Start scanning in 60 seconds.**

```bash
brew install git-scanner    # or cargo install, or download binary
git-scanner scan .
```

- Website: gitsscanner.dev
- GitHub: github.com/git-scanner/git-scanner
- Contact: hello@gitscanner.dev

"Your secrets aren't safe in git. Make sure your code is."

---

## Part 2: Landing Page Plan

---

### Page Structure (Single-Page, Top to Bottom)

#### Section 1: Hero

- **Headline:** "Catch secrets before they leak. In your terminal. In your CI."
- **Subheadline:** "git-scanner is a CLI-first secret scanner that finds AWS keys, API tokens, and credentials in your code — with JSON output built for automation."
- **Primary CTA:** "Get Started Free" (links to install instructions)
- **Secondary CTA:** "View on GitHub"
- **Hero visual:** Animated terminal showing a scan running with findings appearing in real-time (use asciinema or CSS animation)

#### Section 2: Problem Statement

- **Headline:** "Secrets in code cost companies millions"
- Three stat cards:
  - "10M+ secrets leaked on GitHub in 2023"
  - "$4.45M average breach cost"
  - "60% of breaches involve credentials"
- Brief paragraph: Most secret scanning tools are enterprise-heavy, expensive, and designed around GUIs that security teams don't need.

#### Section 3: How It Works (3-Step)

1. **Install** — `brew install git-scanner` (one command, no dependencies)
2. **Scan** — `git-scanner scan --format json .` (run locally or in CI)
3. **Act** — Parse JSON output, block PRs, alert on Slack, log to SIEM

Each step gets an icon and a code snippet.

#### Section 4: Features Grid

Six feature cards in a 2x3 or 3x2 grid:

| Card | Title | Description |
|---|---|---|
| 1 | 15 Secret Patterns | AWS keys, SSH keys, API tokens, JWTs, database URIs, and more |
| 2 | JSON Output | Machine-readable results for any integration |
| 3 | CI/CD Ready | Drop-in configs for GitHub Actions, GitLab CI, Jenkins |
| 4 | Pre-Commit Hooks | Block secrets before they enter git history |
| 5 | Fast & Incremental | Diff-mode scanning — only check changed files |
| 6 | Configurable | Allowlists, custom severity, pattern toggling via `.git-scanner.yml` |

#### Section 5: CI/CD Integration Examples

Tabbed code blocks showing integration snippets:

- **GitHub Actions** tab:
```yaml
- name: Scan for secrets
  run: git-scanner scan --format json --fail-on critical .
```

- **GitLab CI** tab:
```yaml
secret_scan:
  script:
    - git-scanner scan --format json --fail-on critical .
  allow_failure: false
```

- **Pre-commit** tab:
```yaml
repos:
  - repo: https://github.com/git-scanner/git-scanner
    hooks:
      - id: git-scanner
```

#### Section 6: Comparison Table

Simplified version of pitch deck slide 8. Columns: git-scanner vs GitGuardian vs Gitleaks vs TruffleHog. Rows: Price, Setup Time, False Positive Rate, GUI Required, CI Native.

Highlight git-scanner advantages with checkmarks / color.

#### Section 7: Pricing

Three-column pricing cards (Free / Team $299 / Enterprise $799) matching pitch deck slide 9. Each card has feature bullets and a CTA button.

- Free: "Install Now"
- Team: "Start Trial"
- Enterprise: "Contact Sales"

#### Section 8: Testimonials / Social Proof

Placeholder for:
- "Used by X security teams"
- Quote from beta user (security engineer persona)
- Logos of CI/CD platforms supported (GitHub, GitLab, Jenkins, CircleCI)

#### Section 9: FAQ

- "How is this different from Gitleaks?" — Curated patterns, lower false positives, flat pricing.
- "Can I add custom patterns?" — Enterprise tier supports custom regex patterns.
- "Does it scan git history?" — Yes, full history scan available in Team and Enterprise tiers.
- "What languages/file types does it support?" — Language-agnostic. Scans all text files.
- "Is it open source?" — Core CLI is open source. Team/Enterprise features are proprietary.

#### Section 10: Footer CTA

- **Headline:** "Your CI pipeline is incomplete without secret scanning."
- **CTA:** "Get Started Free" button
- Links: Documentation, GitHub, Blog, Contact, Privacy Policy

---

### Design & Technical Notes

**Visual style:**
- Dark theme (terminal aesthetic) with accent color (green or cyan, like terminal output)
- Monospace font for code blocks, clean sans-serif for body text
- Minimal illustration — let the terminal output be the visual

**Tech stack for landing page:**
- Static site (Astro, Next.js static export, or plain HTML/TailwindCSS)
- Hosted on Vercel/Netlify/GitHub Pages
- Analytics: Plausible (privacy-friendly)
- No cookies, no tracking scripts — aligns with security-conscious audience values

**SEO targets:**
- "git secret scanner"
- "scan git for secrets"
- "CI/CD secret scanning tool"
- "find AWS keys in code"
- "pre-commit secret scanner"

**Conversion funnel:**
1. Organic search / GitHub discovery → Landing page
2. Hero CTA → Install instructions (README or /docs/install)
3. Free usage in CI → Team upgrade when org adopts
4. Enterprise inquiry via "Contact Sales" form

---

### Content Deliverables Checklist

- [ ] Hero section copy (headline, subheadline, CTAs)
- [ ] Problem stats with source citations
- [ ] 3-step how-it-works copy and code snippets
- [ ] 6 feature card descriptions
- [ ] CI/CD integration code examples (3 platforms)
- [ ] Comparison table data
- [ ] Pricing tier descriptions
- [ ] FAQ answers (5 questions)
- [ ] Footer CTA copy
- [ ] Meta description and OG tags for social sharing
- [ ] Terminal animation / asciinema recording for hero
