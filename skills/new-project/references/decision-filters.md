# Decision Filters Reference — Defusely Category Domination Edition

Decision filters evaluate every project against Defusely's north star: **become the category king of Reddit Crisis Response and reach $5K MRR.** Filters are revenue-weighted because Defusely is pre-revenue, bootstrapped, and half-time (~15-25 hrs/week). Every hour must count.

---

## Filter Categories & Scoring

### 1. Revenue Impact (Weight: 3x)

**Question**: Does this directly move Defusely toward collecting money?

**Scoring**:
- **0**: No revenue connection — internal tooling, nice-to-have polish, hypothetical users
- **1**: Indirect — SEO content that ranks in 6+ months, brand awareness with no conversion path
- **2**: Moderate — Improves trial conversion, reduces churn, or enables upsell
- **3**: Direct — Unblocks payment (Stripe), closes a deal, converts a founding partner to paid, or fixes trial-to-paid flow

**Defusely context**:
- Stripe integration (products, checkout, webhooks) = 3
- Founding partner outreach to 50 agencies = 3
- Onboarding drip emails to prevent trial abandonment = 2
- New SEO blog post = 1 (long game, not immediate revenue)
- UI polish on existing feature = 0-1

**Red flags**:
- Score 0 and estimated time > 4 hours — this is a distraction
- Building features before Stripe is fully wired = always wrong priority

---

### 2. Category Ownership (Weight: 2x)

**Question**: Does this reinforce Defusely as THE Reddit Crisis Response platform?

**Scoring**:
- **0**: Generic — could be any SaaS, doesn't strengthen category position
- **1**: Tangential — touches crisis/reputation space but doesn't differentiate from Sprinklr/Cision/Brand24
- **2**: Reinforcing — deepens the "Reddit crisis response" narrative (case studies, specific workflows, community recognition)
- **3**: Defining — creates the category language, owns the search terms, makes competitors explain why they're NOT Defusely

**Defusely context**:
- Ranking #1 for "reddit crisis management tool" = 3
- Getting cited by AI search (ChatGPT, Perplexity) for reddit crisis = 3
- Publishing real case study from founding partner = 3
- Comparison page (Slack vs Defusely, Sprinklr vs Defusely) = 2
- Glossary of crisis terms = 2
- Generic SaaS feature (dark mode, notifications) = 0

**Red flags**:
- Score 0 on multiple consecutive projects = drifting from category king path
- Building what competitors already do instead of what only Defusely does

---

### 3. Leverage & Compounding (Weight: 1.5x)

**Question**: Does the output compound? Will it keep working after you stop touching it?

**Scoring**:
- **0**: One-off — consumed once, no residual value
- **1**: Light — reusable template or minor efficiency gain
- **2**: Moderate — creates a system, template, or asset used 10+ times
- **3**: High — compounds indefinitely (SEO content that ranks, automation that runs daily, social proof that converts for months)

**Defusely context**:
- Automated trial-to-paid email sequence = 3 (runs forever, converts while you sleep)
- SEO content ranking for buyer-intent keywords = 3 (compounds for years)
- Playwright QA suite = 3 (catches regressions on every deploy)
- Founding partner case study = 3 (social proof that closes future deals)
- One-off bug fix = 0-1
- Manual outreach email = 1 (each email is unique effort)

---

### 4. Automation ROI (Weight: 1x)

**Question**: Can Geterdone execute this autonomously or semi-autonomously?

**Scoring**:
- **Poor**: Requires 100% manual effort, no automation possible
- **Fair**: Can be partially automated (data collection auto, human review)
- **Good**: Semi-auto (Geterdone drafts, Jay reviews and publishes)
- **Strong**: Fully autonomous (Geterdone researches, writes, deploys, verifies)

**Defusely-specific automation targets**:

| Task | Frequency | Time/occurrence | Automation | Break-even |
|------|-----------|-----------------|------------|------------|
| SEO keyword monitoring | Weekly | 2 hrs | GSC + Brand Radar auto-check | 1 week |
| Competitive intel sweep | Biweekly | 3 hrs | WebSearch + memory write | 2 weeks |
| Trial signup monitoring | Daily | 30 min | Supabase query + Apple Notes dispatch | 1 day |
| Blog post drafting | Weekly | 4 hrs | Semi-auto (draft, Jay reviews) | 2 weeks |
| Deploy QA | Per push | 1 hr | Playwright + Sentry auto-check | 1 push |
| Apollo prospect enrichment | Weekly | 2 hrs | Apollo MCP auto-enrich | 1 week |

---

### 5. Energize Jay (Weight: 0.5x — tiebreaker only)

**Question**: Does this energize Jay? Prevents burnout on a bootstrapped half-time schedule.

**Scoring**: 0-3 (same as generic). Used only to break ties between equal-priority tasks. Jay grinds regardless, but sustained energy prevents abandonment.

---

## Passing Criteria (Revenue-Weighted)

Calculate weighted score:

```
Score = (Revenue Impact × 3) + (Category Ownership × 2) + (Leverage × 1.5) + (Automation ROI × 1) + (Energize × 0.5)

Max possible: 9 + 6 + 4.5 + 3 + 1.5 = 24
```

**Automation ROI scoring for formula**: Poor = 0, Fair = 1, Good = 2, Strong = 3

### Tier 1: Execute immediately (Score >= 16)
High-revenue, high-category tasks. These are the priority stack.

### Tier 2: Schedule this week (Score 10-15)
Important but not urgent. Good leverage or category work that compounds.

### Tier 3: Backlog (Score 5-9)
Nice-to-have. Only do if Tier 1 and 2 are empty.

### Tier 4: Don't do (Score < 5)
This is a distraction. Actively resist. If Jay insists, document the override reason and set a 1-week review checkpoint.

---

## Defusely Priority Stack (Current State: March 2026)

These are the canonical priorities, derived from the Profitability Plan. Any new project gets scored against this stack:

### Must-do (blocks all revenue)
1. **Wire Stripe** — products, checkout, webhooks, subscription status
2. **Fix dual signup flow** — one path from marketing site -> app trial
3. **Trial expiry logic** — read-only after 7 days, upgrade prompt

### Revenue accelerators
4. **Founding partner outreach** — 50 PR agencies via Apollo + LinkedIn
5. **Onboarding drip emails** — Day 1/3/5/7 sequence (Day 3/5/7 built, Day 1 welcome exists)
6. **Founding partner application page** — separate from trial, captures agency details

### Category moat
7. **SEO content** — rank #1 for Reddit crisis keywords (6 blog posts live, need more)
8. **AI search optimization** — get cited by ChatGPT/Perplexity for reddit crisis queries
9. **Real case studies** — from founding partner usage (requires partners first)
10. **Competitive comparison pages** — Slack vs Defusely live, need Sprinklr, Cision, Brand24

### Sustainability
11. **Playwright QA automation** — catch regressions before users see them
12. **Deploy pipeline hardening** — Cloudflare + Supabase deploy reliability
13. **Error monitoring** — Sentry integration for defusely.app

---

## Override Protocol

If Jay wants to override a low-scoring project:

**Valid reasons for Defusely specifically**:
- "Founding partner asked for this" — customer-driven always wins
- "This blocks a higher-priority item" — dependency chain justification
- "Competitive response" — a competitor just shipped something that threatens positioning

**Invalid reasons**:
- "It would be nice to have" — nice doesn't pay bills
- "Other SaaS products have this" — we're not other SaaS products
- "I want to improve the design" — design polish comes after $1K MRR

Document override in memory with a 1-week review: "Still worth it?"

---

## Continuous Calibration

After each project completes, Geterdone checks:
- Did this project generate or contribute to revenue? Update Revenue Impact model.
- Did this project get Defusely mentioned/cited/ranked? Update Category Ownership model.
- Did the output keep producing value? Update Leverage model.
- Was the automation ROI estimate accurate? Calibrate future estimates.

These calibrations are logged in basic-memory for pattern analysis during /review.
