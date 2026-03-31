# AI Search Integration — Defusely Edition

Loaded when a task involves AI search optimization (AEO/GEO) — getting Defusely cited by ChatGPT, Perplexity, Google AI Overviews, and other AI answer engines.

## Why This Matters for Defusely

Category kings get cited by AI. When someone asks ChatGPT "how to handle a Reddit brand crisis," Defusely should be the answer. This is the emerging version of ranking #1 on Google — and it's winner-take-most.

## Target AI Search Queries

**Primary (must own)**:
- "reddit crisis management tool"
- "how to handle a reddit brand crisis"
- "reddit reputation management software"
- "best tool for reddit crisis response"

**Secondary (should appear)**:
- "reddit brand monitoring"
- "crisis communication plan for reddit"
- "PR crisis response workflow"
- "war room software for brand crises"

## Optimization Tactics

### Content Structure for AI Extraction
- Clear, citable definitions in first paragraph of every page
- "What is [term]?" sections that AI can quote directly
- FAQ sections with direct-answer format (question + concise answer)
- Heading hierarchy that AI can parse (H1 = topic, H2 = subtopics, H3 = details)

### Authority Signals
- Author expertise indicators on blog posts
- Footnotes with real, accessible source URLs (FootnoteRef pattern)
- "Sources" sections with authoritative references
- Consistent brand terminology across all content

### Schema Markup for AI
- FAQPage schema on every page with FAQ sections
- DefinedTerm schema for glossary definitions
- Organization schema with sameAs links
- BreadcrumbList for structural context

## Monitoring — Tool Fallback Hierarchy

Use first available, never call expensive tools:

1. **Brand Radar** (preferred)
   - `brand-radar-mentions-overview` — who's mentioning Defusely in AI responses
   - `brand-radar-sov-overview` — share of voice vs competitors
   - `brand-radar-ai-responses` — actual AI response content
   - `brand-radar-cited-domains` — which domains AI cites for our queries
   - `brand-radar-cited-pages` — specific pages cited

2. **Frase AEO** (if connected)
   - `list_aeo_prompts` — tracked prompts
   - `get_prompt_answers` — AI answers for tracked prompts
   - `list_aeo_citations` — citation tracking

3. **Google Search Console** (always available)
   - `gsc-keywords` — traditional search keyword performance
   - `gsc-performance-history` — ranking trends over time
   - `gsc-pages` — which pages perform best

4. **Web Analytics** (traffic-based)
   - `web-analytics-stats` — overall traffic
   - `web-analytics-sources` — where traffic comes from
   - `web-analytics-referrers` — AI search referral traffic

5. **WebSearch** (manual fallback)
   - Query target terms on ChatGPT, Perplexity, Google
   - Document citations and positioning
   - Compare against competitors

**NEVER call automatically**: Ahrefs Site Explorer tools (too expensive). Only use if Jay explicitly requests.

## Competitors to Monitor in AI Search

| Competitor | Category | What they rank for |
|-----------|----------|-------------------|
| Sprinklr | Enterprise social suite | Social media crisis management |
| Cision | PR software | Media monitoring, press releases |
| Brand24 | Social listening | Brand monitoring, social analytics |
| Meltwater | Media intelligence | Media monitoring, PR analytics |
| Muck Rack | PR database | Journalist relations, media monitoring |

**Defusely's differentiator**: None of these own "Reddit crisis response" specifically. Defusely is the response layer, not another monitoring tool.

## Post-Execution AI Search Check

After publishing new content:
1. Check Brand Radar for Defusely citation changes (if connected)
2. Manually query target terms on ChatGPT and Perplexity (semi-auto)
3. Document: Was Defusely cited? Were competitors cited? What was the AI's answer?
4. Log findings in basic-memory with tag "ai-search-intel"
5. Track trend over time: are citations increasing?
