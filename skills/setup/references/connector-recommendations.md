# Connector Recommendations — Defusely Edition

Defusely requires a specific stack of connectors across four workstreams: Product Development, SEO & AI Search, Founding Partner Outreach, and Category Positioning.

---

## Jay's Connected Tools (as of March 2026)

These are already connected — verify during /setup:

| Tool | MCP Probe | Workstream |
|------|-----------|------------|
| Supabase | `list_projects` | Product (DB, edge functions, auth) |
| Cloudflare | `accounts_list` | Product (Pages, Workers, D1, KV) |
| Google Calendar | `gcal_list_calendars` | Operations (scheduling) |
| Gmail | `gmail_search_messages` | Outreach (email sequences) |
| Apollo | `find-and-enrich-company` | Outreach (lead enrichment) |
| Notion | `notion-search` | Memory (knowledge base, archives) |
| Apple Notes | `list_notes` | Dispatch (mobile summaries) |
| Basic Memory | `search` | Memory (persistent context) |
| Memory Graph | `search_nodes` | Memory (entity relations) |
| Figma | `get_metadata` | Design (design files) |
| Canva | `search-designs` | Design (marketing assets) |
| Sentry | `search_issues` | Product (error monitoring) |
| Ahrefs/SEO tools | `gsc-keywords` | SEO (GSC, Brand Radar, Frase, Rank Tracker, Web Analytics) |
| Chrome MCP | `navigate` | Operations (browser automation) |

## Recommended Additions (Priority Order)

### Tier 1: Critical for Revenue (connect immediately)

**Stripe**
- UUID: `de127013-63f1-43d0-8dd2-b6cb5b4e5d1b`
- What: Payment processing, subscriptions, billing
- Why: Defusely literally cannot collect money without it. #1 blocker.
- Workstream: Product (billing), Operations (revenue tracking)
- Probe: Check for Stripe MCP tools (create products, prices, subscriptions)

### Tier 2: High Value for Category Domination

**Slack** (if Jay uses it for dispatch)
- UUID: `597f662f-36de-437e-836e-5a81013cbfbe`
- What: Team communication and notifications
- Why: Alternative dispatch channel to Apple Notes. Better for real-time alerts.
- Workstream: Operations (dispatch)

**G2**
- UUID: `df731661-86d2-48e9-ac51-bdeceb265948`
- What: B2B reviews and competitive intelligence
- Why: Monitor competitor reviews, understand customer perception, find positioning angles
- Workstream: Category Positioning (competitive intel)

### Tier 3: Nice-to-Have

**PostHog**
- UUID: `50688846-553c-4a12-bc21-df94d2173734`
- What: Product analytics and user behavior
- Why: Track how founding partners actually use the product. Essential for product-market fit.
- Workstream: Product (analytics)

**n8n**
- UUID: `d86fa999-100c-4212-ad7f-2fefea661ef1`
- What: Workflow automation
- Why: Automate trial signup notifications, drip email triggers, competitive monitoring
- Workstream: Operations (automation)

---

## Defusely-Specific Setup Configuration

When running /setup for Defusely, auto-configure these preferences:

```json
{
  "workflow_type": "mix",
  "workflow_detail": "SaaS founder — product dev, SEO, outreach, category positioning",
  "product": {
    "name": "Defusely",
    "category": "Reddit Crisis Response SaaS",
    "urls": {
      "marketing": "defusely.com",
      "app": "defusely.app"
    },
    "repos": {
      "marketing": "jayrockliffe-defused/defusely",
      "app": "jayrockliffe-defused/defusely-confilict-os-reddit-platform"
    },
    "supabase_ref": "poqqwtyqadfzyxcrfcus",
    "stripe_account": "acct_1SvA4E9KYh6gN3yU",
    "cloudflare_account": "5649b38cc693eaaebbb38adaaf291597"
  },
  "dispatch_channel": "apple_notes",
  "review_cadence": "weekly",
  "auto_execute_time": "09:00",
  "morning_brief": true,
  "revenue_targets": {
    "milestone_1": "$1K MRR (4 Starter customers)",
    "milestone_2": "$5K MRR (sustainable)",
    "break_even": "2 Starter customers (~$500/mo)"
  },
  "competitors": ["Sprinklr", "Cision", "Brand24", "Meltwater", "Muck Rack"],
  "target_keywords": [
    "reddit crisis management tool",
    "reddit brand crisis response",
    "reddit reputation management software"
  ],
  "seo_tool_hierarchy": [
    "GSC (free, always use first)",
    "Brand Radar (AI search monitoring)",
    "Frase AEO (answer engine optimization)",
    "Web Analytics (traffic data)",
    "Rank Tracker (keyword rankings)",
    "WebSearch (manual fallback)",
    "Ahrefs Site Explorer (NEVER auto-call — too expensive)"
  ]
}
```

## Tool Scanning Order

During /setup, probe tools in this order (skip gracefully if unavailable):

1. **Revenue tools**: Stripe, Supabase, Cloudflare
2. **Outreach tools**: Apollo, Gmail, Google Calendar
3. **SEO/AI tools**: GSC, Brand Radar, Frase, Web Analytics, Rank Tracker
4. **Memory tools**: Basic Memory, Memory Graph, Notion, Apple Notes
5. **Monitoring tools**: Sentry, Chrome MCP
6. **Design tools**: Figma, Canva

## Quick Reference: All Defusely Connectors

| Connector | UUID | Status | Workstream |
|-----------|------|--------|------------|
| Stripe | de127013-63f1-43d0-8dd2-b6cb5b4e5d1b | Needed | Product/Revenue |
| Supabase | (connected) | Connected | Product |
| Cloudflare | (connected) | Connected | Product |
| Apollo | (connected) | Connected | Outreach |
| Gmail | (connected) | Connected | Outreach |
| Google Calendar | (connected) | Connected | Operations |
| Notion | (connected) | Connected | Memory |
| Apple Notes | (connected) | Connected | Dispatch |
| Basic Memory | (connected) | Connected | Memory |
| Memory Graph | (connected) | Connected | Memory |
| Sentry | (connected) | Connected | Monitoring |
| GSC/Ahrefs tools | (connected) | Connected | SEO |
| Figma | (connected) | Connected | Design |
| Canva | (connected) | Connected | Design |
| Chrome MCP | (connected) | Connected | Operations |
| Slack | 597f662f-36de-437e-836e-5a81013cbfbe | Optional | Dispatch |
| G2 | df731661-86d2-48e9-ac51-bdeceb265948 | Optional | Intel |
| PostHog | 50688846-553c-4a12-bc21-df94d2173734 | Optional | Analytics |
| n8n | d86fa999-100c-4212-ad7f-2fefea661ef1 | Optional | Automation |
