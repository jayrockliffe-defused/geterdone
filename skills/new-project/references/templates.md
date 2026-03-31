# Project Templates — Defusely Category Domination Edition

Templates pre-structured for Defusely's four workstreams: Product Development, SEO & AI Search, Founding Partner Outreach, and Category Positioning. Each template has Defusely-specific context, tool references, and automation classifications.

---

## Template: Stripe & Billing Sprint

**Use Case**: Wire Stripe products/prices, build checkout flow, subscription management, trial-to-paid conversion
**Typical Duration**: 1-2 weeks
**Priority Stack Position**: #1 (blocks all revenue)

**Day-by-Day Skeleton**:
- **Day 1**: Stripe Setup
  - Create Stripe products and prices (Starter $249/$299, Growth $649/$799) [Semi-auto: Stripe MCP creates, Jay verifies]
  - Add subscription columns to profiles table [Semi-auto: Supabase MCP migration]
- **Day 2-3**: Checkout Flow
  - Build Stripe Checkout Session edge function [Manual: code + deploy]
  - Build webhook handler (checkout.session.completed, subscription.updated/deleted) [Manual: code + deploy]
  - Wire pricing page CTA -> Stripe Checkout -> redirect to defusely.app [Manual]
- **Day 4**: Trial Logic
  - Build trial expiry enforcement (read-only after 7 days) [Manual]
  - In-app upgrade prompt when trial < 3 days [Manual]
  - Trial banner (already built: ambient indicator days 4-7) [Verify existing]
- **Day 5**: QA & Deploy
  - End-to-end test: signup -> trial -> checkout -> paid -> access [Manual]
  - Playwright QA for checkout flow [Semi-auto]
  - Sentry baseline check post-deploy [Autonomous]

**Key integrations**: Stripe MCP, Supabase MCP, Cloudflare MCP
**QA gate**: Must test full trial-to-paid flow with real Stripe test mode
**Deploy targets**: defusely.app (Cloudflare Workers), Supabase edge functions

---

## Template: Founding Partner Outreach

**Use Case**: Identify, contact, and onboard 10 founding design partners from PR agencies
**Typical Duration**: 4-6 weeks
**Priority Stack Position**: #2 (first real users)

**Day-by-Day Skeleton**:
- **Days 1-2**: List Building
  - Apollo search: PR agencies, 10-100 employees, US/UK/Canada, crisis comms or reputation management [Semi-auto: Apollo MCP enrichment]
  - Build 50-agency target list with decision-maker contacts [Semi-auto: Apollo enriches, Jay curates]
  - Research each agency's public crisis work (LinkedIn, website, case studies) [Semi-auto]
- **Days 3-4**: Message Development
  - Draft outreach template: invitation, not pitch ("building the first Reddit crisis response platform, hand-picking 10 agencies to shape it") [Semi-auto: Geterdone drafts, Jay refines]
  - Personalize 50 messages with agency-specific hooks [Semi-auto]
  - Build founding partner application page on defusely.com [Manual]
- **Days 5-10**: Wave 1 Outreach (25 agencies)
  - Send LinkedIn connection requests + personalized messages [Manual: Jay sends]
  - Send email follow-ups to non-responders (day 3, day 7) [Semi-auto: Gmail MCP drafts]
  - Track responses in Apollo/memory [Semi-auto]
- **Days 11-20**: Wave 2 + Onboarding
  - Send Wave 2 to remaining 25 agencies [Semi-auto]
  - White-glove onboard first responders (screen share, walk through product) [Manual]
  - Collect feedback from first 5 partners [Manual]
- **Days 21-30**: Nurture & Case Study Prep
  - Weekly check-in emails with partners [Semi-auto]
  - Document usage patterns, wins, quotes for case studies [Semi-auto]
  - Ask for testimonial/logo permission [Manual]

**Key integrations**: Apollo MCP, Gmail MCP, Google Calendar MCP
**Success metric**: 5-10 founding partners actively using product by Week 6
**Revenue connection**: Partners generate case studies -> social proof -> paid conversions

---

## Template: SEO Content Sprint

**Use Case**: Research, write, and publish SEO content targeting Reddit crisis keywords
**Typical Duration**: 2-3 weeks per batch
**Priority Stack Position**: #7 (category moat, long-term compounding)

**Day-by-Day Skeleton**:
- **Day 1**: Keyword Research
  - Check GSC for existing keyword performance [Autonomous: gsc-keywords, gsc-pages]
  - Identify target keywords with buyer intent [Semi-auto: keyword research + Jay selects]
  - Check AI search citations for Defusely (ChatGPT, Perplexity) [Semi-auto: Brand Radar or manual WebSearch]
- **Day 2**: Content Planning
  - Outline 3-5 articles targeting selected keywords [Semi-auto: Geterdone drafts outlines]
  - Map internal linking strategy (cross-link to existing 6 blog posts, glossary, comparison pages) [Semi-auto]
  - Define schema markup for each (BlogPosting + FAQPage + BreadcrumbList) [Autonomous]
- **Days 3-7**: Writing
  - Draft articles using BlogArticleLayout pattern [Semi-auto: Geterdone writes, Jay edits]
  - Include FootnoteRef citations with real sources [Semi-auto]
  - Write meta titles (<60 chars), meta descriptions (<155 chars) [Semi-auto]
  - Create FAQ sections optimized for featured snippets and AI search [Semi-auto]
- **Days 8-9**: Technical SEO
  - Add JSON-LD schema markup [Semi-auto]
  - Add to sitemap (generate-sitemap.mjs handles this) [Autonomous]
  - Verify canonical URLs and internal links [Autonomous]
  - Run seo-audit skill [Autonomous]
- **Day 10**: Publish & Index
  - Deploy to defusely.com (bun install, npm run build, push) [Semi-auto: defusely-build-deploy skill]
  - Request indexing in GSC for new URLs [Semi-auto: Chrome MCP]
  - Post to relevant channels (LinkedIn, Reddit if appropriate) [Manual]

**Key integrations**: GSC (gsc-keywords, gsc-pages), Brand Radar, Web Analytics, Cloudflare Pages
**Existing content**: 6 blog posts, 1 glossary (24 terms), 1 comparison page (Slack vs Defusely)
**Build pattern**: Uses BlogArticleLayout with meta, footnotes, crossLinks, secondaryCta props
**Deploy**: Cloudflare Pages auto-deploy on push to main. Always bun install first.

---

## Template: AI Search Optimization Sprint

**Use Case**: Optimize Defusely to be cited by AI search engines (ChatGPT, Perplexity, Google AI Overviews) for Reddit crisis queries
**Typical Duration**: 1-2 weeks
**Priority Stack Position**: #8 (category moat, emerging channel)

**Day-by-Day Skeleton**:
- **Day 1**: Baseline Audit
  - Check Brand Radar for current AI mentions and citations [Autonomous]
  - Query ChatGPT, Perplexity, Google for "reddit crisis management" variants [Semi-auto]
  - Document current citation status and competitors cited [Semi-auto]
- **Days 2-3**: Content Optimization
  - Add FAQ schema with direct-answer questions to key pages [Semi-auto]
  - Ensure all blog posts have clear, citable definitions in first paragraph [Semi-auto]
  - Add "What is [term]?" sections optimized for AI extraction [Semi-auto]
  - Structure content with clear H2/H3 hierarchy for AI parsing [Semi-auto]
- **Days 4-5**: Authority Signals
  - Add author bio and expertise signals to blog posts [Semi-auto]
  - Ensure all footnotes have real, accessible source URLs [Autonomous: link check]
  - Add "Sources" sections with authoritative references [Semi-auto]
- **Day 6**: Verification
  - Re-query AI search engines for target terms [Semi-auto]
  - Check Brand Radar for citation changes [Autonomous]
  - Document improvements in memory for trend tracking [Autonomous]

**Key integrations**: Brand Radar (mentions, citations, SOV), Frase AEO, GSC
**Target queries**: "reddit crisis management tool", "how to handle reddit brand crisis", "reddit reputation management software"
**Competitors to monitor**: Sprinklr, Cision, Brand24, Meltwater, Muck Rack

---

## Template: Competitive Comparison Page

**Use Case**: Build a detailed comparison page (Defusely vs [Competitor]) for SEO and sales
**Typical Duration**: 3-5 days
**Priority Stack Position**: #10 (category moat)

**Day-by-Day Skeleton**:
- **Day 1**: Competitor Research
  - Deep dive on competitor's Reddit/crisis capabilities [Semi-auto: WebSearch + manual review]
  - Document pricing, features, limitations [Semi-auto]
  - Check G2/Capterra reviews for competitor pain points [Semi-auto]
- **Day 2**: Draft Comparison
  - Write comparison page following Slack vs Defusely pattern [Semi-auto: Geterdone drafts]
  - Include FAQPage schema for "Is [competitor] good for Reddit crisis?" queries [Semi-auto]
  - Add honest pros/cons (credibility > cheerleading) [Semi-auto]
- **Day 3**: Build & Deploy
  - Create React page component with JSON-LD, meta tags, BreadcrumbList [Semi-auto]
  - Add route to App.tsx, update sitemap [Semi-auto]
  - Deploy via defusely-build-deploy skill [Semi-auto]
  - Request GSC indexing [Semi-auto: Chrome MCP]

**Existing example**: /slack-vs-defusely (live, use as pattern)
**Target competitors**: Sprinklr, Cision, Brand24, Meltwater, Muck Rack
**Schema**: FAQPage + WebPage + BreadcrumbList (same as existing comparison)

---

## Template: Product Feature Sprint

**Use Case**: Build, test, and deploy a new feature or fix in defusely.app
**Typical Duration**: 1-2 weeks
**Priority Stack Position**: Varies (score against decision filters)

**Day-by-Day Skeleton**:
- **Day 1**: Planning
  - Define requirements (from founding partner feedback or priority stack) [Manual]
  - Check existing codebase for related components [Semi-auto: Explore agent]
  - Estimate effort and break into tasks [Manual]
- **Days 2-4**: Development
  - Implement feature in defusely.app repo [Manual]
  - Write/update TypeScript types [Manual]
  - Ensure mobile responsiveness (follow March 16 audit patterns) [Manual]
- **Day 5**: QA & Deploy
  - TypeScript check: `npx tsc --noEmit` [Autonomous]
  - ESLint: `npx eslint src/ --ext .ts,.tsx` [Autonomous]
  - Vite build: `npx vite build` [Autonomous]
  - Playwright QA for new feature [Semi-auto]
  - Push to main, verify deploy [Semi-auto]
  - Sentry check for new errors [Autonomous]
  - Update CLAUDE.md and docs [Semi-auto]

**Deploy target**: defusely.app (Cloudflare Workers)
**Key rule**: Always `git fetch origin main` first — Lovable pushes its own commits
**Edge function deploy gotcha**: MCP can't resolve `../_shared/cors.ts` — must inline CORS

---

## Template: Case Study

**Use Case**: Write and publish a real case study from a founding partner's usage
**Typical Duration**: 1-2 weeks
**Priority Stack Position**: #9 (social proof, requires partners first)

**Day-by-Day Skeleton**:
- **Day 1**: Data Collection
  - Interview founding partner (record key quotes, metrics, timeline) [Manual]
  - Pull usage data from Supabase (War Rooms created, response times, incidents handled) [Semi-auto: Supabase MCP]
- **Day 2-3**: Write
  - Draft case study: "How [Agency] reduced Reddit crisis response time by X%" [Semi-auto]
  - Include real metrics, direct quotes, before/after comparison [Semi-auto]
  - Get partner approval on draft [Manual]
- **Day 4**: Publish
  - Add to defusely.com as blog post with BlogArticleLayout [Semi-auto]
  - Add testimonial quotes to pricing and homepage [Semi-auto]
  - Add agency logo to social proof section [Semi-auto]
  - Deploy and request GSC indexing [Semi-auto]

**Dependency**: Requires active founding partners (Template: Founding Partner Outreach must complete first)
**Revenue connection**: Case studies are the #1 conversion driver for B2B SaaS

---

## Template: Weekly Operations (Recurring)

**Use Case**: Ongoing operational tasks that run every week
**Typical Duration**: Recurring (2-4 hrs/week)
**Automation Level**: Mostly autonomous

**Weekly Tasks**:
- **Monday**: SEO check [Autonomous]
  - GSC keyword rankings check (gsc-keywords)
  - Brand Radar AI search mentions (brand-radar-mentions-overview)
  - Web analytics traffic check (web-analytics-stats)
  - Dispatch summary to Apple Notes
- **Wednesday**: Competitive pulse [Autonomous]
  - WebSearch for competitor news/launches
  - Check G2/Capterra for new competitor reviews
  - Update competitive intel in memory
- **Friday**: Trial & outreach metrics [Semi-auto]
  - Supabase query: new trial signups this week
  - Apollo sequence: response rates, new leads
  - Revenue pipeline: any upgrade signals
  - Dispatch weekly summary to Apple Notes

**Key integrations**: GSC, Brand Radar, Web Analytics, Supabase MCP, Apollo MCP, Apple Notes
**Output**: Weekly intel brief dispatched to mobile every Friday

---

## How to Choose a Template

For Defusely projects, match against the priority stack first:
1. Does this block revenue? -> Stripe & Billing Sprint
2. Does this get founding partners? -> Founding Partner Outreach
3. Does this own the search terms? -> SEO Content Sprint or AI Search Optimization
4. Does this build the moat? -> Competitive Comparison Page or Case Study
5. Does this ship product? -> Product Feature Sprint
6. Does this keep operations running? -> Weekly Operations

If no template fits, use Custom but still score against decision filters.
