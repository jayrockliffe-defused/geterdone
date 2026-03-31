# Cross-Project Intelligence Patterns — Defusely Edition

Reference file for the /intel skill. Loaded when analyzing patterns across Defusely workstreams.

## Defusely Workstream Map

All projects feed a single goal: category king of Reddit Crisis Response, $5K MRR.

```
Revenue Pipeline
├── Stripe & Billing (blocks everything)
├── Founding Partner Outreach (first users)
│   └── Case Studies (social proof)
│       └── Cold Outbound (paid conversions)
└── Trial-to-Paid Flow (conversion path)

Category Moat
├── SEO Content (organic discovery)
├── AI Search Optimization (emerging channel)
├── Competitive Comparisons (positioning)
└── Glossary & Definitions (own the language)

Product
├── Feature Development (from partner feedback)
├── Bug Fixes (from partner usage)
├── Deploy Pipeline (reliability)
└── QA Automation (regression prevention)

Operations
├── Weekly SEO/AI Search monitoring
├── Trial signup monitoring
├── Competitive pulse
└── Memory & documentation updates
```

## Leverage Detection — Defusely Specific

### Content reuse across workstreams
- Blog post research -> outreach message personalization hooks
- Competitive analysis -> comparison page content
- Founding partner conversations -> case study material + feature requests
- Crisis management knowledge -> product AI prompts + blog content
- SEO keyword research -> AI search optimization targets

### Code reuse
- Edge function patterns (CORS inline, Deno imports) -> all future functions
- BlogArticleLayout pattern -> all new content pages
- Schema markup patterns (JSON-LD) -> all new pages
- Mobile responsive patterns (March 16 audit) -> all new components

### Knowledge reuse
- PR agency workflows (from pr-comms-personas skill) -> outreach messaging + product design
- Crisis management frameworks (from crisis-reputation skill) -> product features + content
- Competitor weaknesses (from competitive intel) -> positioning + comparison pages

## Conflict Detection — Defusely Specific

### Priority conflicts (most common)
- **Feature building vs Stripe wiring**: Any feature request before Stripe is done = wrong priority
- **SEO content vs outreach**: Both compete for Jay's time. Rule: outreach wins until 10 partners onboarded
- **Polish vs ship**: UI improvement competes with revenue-generating work. Rule: ship wins

### Timeline conflicts
```
For Defusely specifically:
  Phase 1 (Stripe) MUST complete before Phase 2 (outreach starts)
  Phase 2 (outreach) can overlap with SEO work (different workstreams)
  Product features wait until founding partners provide feedback
```

### Context-switching cost
Jay works half-time (15-25 hrs/week). With 4 workstreams:
- Max 2 active workstreams at once
- Switch workstreams daily, not hourly
- Deep work blocks: morning for code, afternoon for outreach/content

## Dependency Mapping — Defusely Specific

### Critical dependency chain
```
Stripe -> Trial-to-Paid Flow -> Revenue
Founding Partners -> Real Usage Data -> Case Studies -> Social Proof -> Cold Outbound -> Paid Customers
SEO Content -> Google Rankings -> Organic Traffic -> Trial Signups -> Revenue
AI Search Citations -> Brand Authority -> Organic Discovery -> Trial Signups
```

### Blocking dependencies (check these every /intel run)
1. Is Stripe fully wired? If no, everything downstream is blocked.
2. Are founding partners onboarded? If no, case studies are blocked.
3. Is trial-drip-emails cron configured? If no, onboarding emails don't send.
4. Is GSC indexing requested for all published pages? If no, SEO content isn't discoverable.

### External dependencies
- Stripe: account active, products/prices created
- Supabase: project healthy, edge functions responding
- Cloudflare: Pages builds succeeding, Workers healthy
- Apollo: API access active, credits available
- Google Search Console: property verified, indexing working

## Automation Candidates — Defusely Specific

### High-value automation targets

| Pattern | Frequency | Automation | Tool |
|---------|-----------|------------|------|
| Check GSC keyword rankings | Weekly | Autonomous | gsc-keywords MCP |
| Check Brand Radar AI citations | Weekly | Autonomous | brand-radar MCP |
| Monitor trial signups | Daily | Autonomous | Supabase execute_sql |
| Check Sentry for new errors | Daily | Autonomous | Sentry MCP |
| Enrich new prospect leads | Weekly | Semi-auto | Apollo MCP |
| Draft blog post outlines | Biweekly | Semi-auto | Geterdone drafts |
| Deploy QA check | Per push | Autonomous | Playwright + Sentry |
| Competitive news sweep | Biweekly | Semi-auto | WebSearch |

### Skill creation threshold
| Occurrences | Action |
|-------------|--------|
| 2 | Note the pattern in memory |
| 3 | Suggest automation to Jay |
| 5+ | Strongly recommend new skill creation |

### Existing relevant skills (already available)
- `seo-audit` — Full SEO audit for defusely.com and defusely.app
- `defusely-build-deploy` — Pre-push checklists, deploy pipelines
- `drift-prevention` — Push summary protocol
- `crisis-reputation` — Crisis management knowledge for product
- `pr-comms-personas` — PR agency persona knowledge
- `competitive-intel` — Competitive intelligence operations
- `category-design` — Category king framework (Play Bigger)
- `copywriting-persuasion` — Conversion copy for marketing site
- `customer-success` — Retention for irregular-usage SaaS

## Intelligence Scoring — Revenue-Weighted

Rate each finding on four dimensions:

| Dimension | Score | Defusely Weight |
|-----------|-------|-----------------|
| Revenue impact | 1-5 | 3x |
| Category impact | 1-5 | 2x |
| Urgency | 1-5 | 1.5x |
| Confidence | 1-5 | 1x |

Priority = (Revenue × 3 + Category × 2 + Urgency × 1.5 + Confidence) / 7.5

Only surface findings with priority > 3.0 to avoid noise.

## Category Domination Signals (track these)

**Winning signals**:
- Defusely ranking #1 for target keywords
- Defusely cited by AI search engines
- Founding partners referring other agencies unprompted
- Competitors mentioning "Reddit crisis" in their marketing (category validation)
- PR industry publications writing about Reddit crisis response (category awareness)

**Losing signals**:
- Competitor launches Reddit-specific crisis feature
- Defusely keyword rankings declining
- AI search citations going to competitors
- Founding partners churning or going inactive
- No new trial signups for 2+ weeks
