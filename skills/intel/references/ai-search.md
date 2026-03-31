# AI Search & SEO Intelligence

Reference file for the /intel skill. Defines the tool hierarchy for
SEO and AI search monitoring. Never call expensive tools first.

## Tool Fallback Hierarchy

Use the first available tool in this order. Check `geterdone-config.json`
for which tools are connected.

### Tier 1: Free / included tools

**Google Search Console (GSC)**
- `gsc-keywords` — what queries drive traffic
- `gsc-performance-history` — traffic trends over time
- `gsc-pages` — which pages perform best
- `gsc-keyword-history` — individual keyword tracking
- `gsc-ctr-by-position` — CTR optimization opportunities
- `gsc-performance-by-device` — mobile vs desktop
- `gsc-positions-history` — ranking movement

**Web Analytics**
- `web-analytics-stats` — overall traffic summary
- `web-analytics-sources` — where traffic comes from
- `web-analytics-top-pages` — highest traffic pages
- `web-analytics-referrers` — who links to you
- `web-analytics-devices` — device breakdown
- `web-analytics-entry-pages` — landing page performance

### Tier 2: AI search monitoring

**Brand Radar**
- `brand-radar-mentions-overview` — brand mention tracking
- `brand-radar-sov-overview` — share of voice
- `brand-radar-ai-responses` — how AI models respond about your brand
- `brand-radar-cited-domains` — which domains AI cites
- `brand-radar-cited-pages` — which pages AI cites
- `brand-radar-impressions-overview` — AI search impressions

**Frase AEO (Answer Engine Optimization)**
- `list_aeo_prompts` — tracked prompts/questions
- `get_prompt_answers` — how AI answers your tracked questions
- `list_aeo_citations` — citation tracking
- `list_aeo_domains` — domain-level AI visibility
- `create_aeo_prompt` — add new prompts to track

### Tier 3: Content intelligence

**AirOps** (if connected)
- Check workflow outputs for content performance
- Review SEO content generation results
- Monitor content pipeline status

**Frase Knowledge Base**
- `search_knowledge_base` — search internal content
- `list_pages` — audit existing content
- `get_page_details` — content quality assessment

### Tier 4: Competitive tracking

**Rank Tracker**
- `rank-tracker-overview` — keyword ranking summary
- `rank-tracker-competitors-overview` — competitor rankings
- `rank-tracker-serp-overview` — SERP feature tracking

**Keywords Explorer** (use sparingly — API credits)
- `keywords-explorer-overview` — keyword metrics
- `keywords-explorer-related-terms` — keyword expansion
- `keywords-explorer-volume-history` — search volume trends

### Tier 5: Fallback

**WebSearch**
- Manual competitive checks via web search
- Verify AI citation claims manually
- Check competitor content updates

## NEVER use these tools (too expensive)

- `site-explorer-*` (Ahrefs Site Explorer — high API cost)
- `site-explorer-all-backlinks` (extremely expensive)
- `site-explorer-organic-keywords` (use GSC instead)
- `site-explorer-referring-domains` (use GSC + web analytics instead)
- `batch-analysis` (high cost, use individual queries instead)

Exception: If user explicitly requests Ahrefs data AND acknowledges cost.

## Intelligence Patterns

### Ranking changes
Check GSC for keywords that moved > 5 positions (up or down) in the last week.
Flag significant drops as urgent. Flag gains as opportunities to double down.

### AI search visibility
Check Brand Radar for new citations or mentions.
Check Frase AEO for changes in AI responses about tracked topics.
Flag: new competitors appearing in AI responses, loss of citations.

### Content gaps
Compare GSC keywords (what you rank for) with Frase knowledge base (what you've written about).
Flag: high-impression keywords with no dedicated content page.

### Traffic anomalies
Check web analytics for:
- Sudden traffic drops (> 20% week-over-week)
- New referral sources appearing
- Entry page changes (new pages attracting traffic)
- Device shift (mobile growing, desktop declining)

## Dispatch Format

Keep SEO intel actionable and jargon-free:
```
SEO: [keyword] dropped from position [X] to [Y] — check [page URL]
AI Search: [competitor] now cited for "[query]" — you were cited last week
Content gap: "[keyword]" gets [N] impressions but you have no dedicated page
```
