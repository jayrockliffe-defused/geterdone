# SEO Integration — Defusely Edition

Loaded when a task involves SEO content, keyword optimization, schema markup, or search performance for defusely.com.

## Defusely SEO Context

**Target category**: Reddit Crisis Response (own the search terms)
**Existing content** (as of March 2026):
- 6 blog posts (including crisis response guides, communication plan template)
- 1 glossary page (24 DefinedTerm definitions)
- 1 comparison page (Slack vs Defusely)
- Homepage, pricing, features pages with schema markup

**Target keywords** (buyer intent):
- "reddit crisis management tool"
- "reddit brand crisis response"
- "reddit reputation management software"
- "how to handle reddit brand crisis"
- "reddit crisis communication plan"

**Content architecture**:
- Blog posts use `BlogArticleLayout` with `meta`, `footnotes`, `crossLinks`, `secondaryCta` props
- `FootnoteRef` component for inline citations
- All pages need JSON-LD schema via `<script type="application/ld+json">`
- Routes lazy-loaded with `React.lazy()` + `Suspense` in App.tsx
- New pages need: route, sitemap entry, internal cross-links

## Pre-execution SEO check
1. `gsc-keywords` — existing keywords for target page
2. `gsc-pages` — current page performance
3. `web-analytics-entry-pages` — which pages attract organic traffic
4. `brand-radar-mentions-overview` — AI search citations (if connected)

## During execution — content requirements
- Target keywords naturally placed (no stuffing)
- Meta title < 60 chars, meta description < 155 chars
- H1 > H2 > H3 heading hierarchy
- Internal links to related existing content (all 6 blog posts, glossary, comparison)
- FAQ section optimized for featured snippets and AI search extraction
- FootnoteRef citations with real, accessible source URLs
- CrossLinks to related content pages

## Schema markup requirements (by page type)

| Page Type | Required Schema |
|-----------|----------------|
| Blog post | BlogPosting + FAQPage + BreadcrumbList |
| Glossary | DefinedTerm + DefinedTermSet + WebPage + BreadcrumbList |
| Comparison | FAQPage + WebPage + BreadcrumbList |
| Homepage | Organization + WebSite |
| Product pages | SoftwareApplication (if applicable) |

## Post-execution SEO verification
1. Meta tags present and within length limits
2. JSON-LD schema valid (check structure)
3. Canonical URL set correctly
4. Internal links working (no broken links)
5. Sitemap entry added (generate-sitemap.mjs handles this on build)
6. Run seo-audit skill for comprehensive check

## Deploy & index
1. Deploy via defusely-build-deploy skill (bun install, npm run build, push)
2. Request indexing in GSC for new URLs (Chrome MCP -> URL Inspection tool)
3. Hard refresh to verify live

## Tool reference
SEO tools follow the fallback hierarchy in intel/references/ai-search.md.
For execution, primarily use GSC and Web Analytics (Tier 1 tools only).
Never call expensive Ahrefs Site Explorer tools automatically.
