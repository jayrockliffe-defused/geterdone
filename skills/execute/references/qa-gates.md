# QA Gates — Defusely Verification Protocol

Before marking a task `done: true`, execute verification matching the deliverable type. These gates are non-negotiable — QA failures block completion.

## File Existence & Integrity

Always verify first:
- File exists at specified path
- File is non-empty (not a placeholder or stub)
- File permissions allow reading
- No truncation or encoding errors

```bash
if [ ! -f "$OUTPUT_FILE" ] || [ ! -s "$OUTPUT_FILE" ]; then
  echo "FAIL: Output file missing or empty"
  exit 1
fi
```

## Content Deliverables (blog posts, SEO pages, case studies, comparison pages)

- **Word count**: Verify meets spec (e.g., "1500+ words"). Count using `wc -w`.
- **No placeholder text**: Search for "TODO", "FIXME", "placeholder", "TBD", "[NEED...]" — if found, task fails QA.
- **Formatting**: Headers, links, lists properly formatted. Run through markdown linter if available.
- **Source citations**: Spot-check 2-3 citations for validity. Defusely blog posts use FootnoteRef pattern.
- **SEO metadata**: Title (<60 chars), meta description (<155 chars), slug, canonical URL all present.
- **JSON-LD schema**: BlogPosting + FAQPage + BreadcrumbList for blog posts. DefinedTerm for glossary. FAQPage + WebPage for comparison pages.
- **Internal links**: Cross-link to existing content (6 blog posts, glossary, comparison pages).
- **BlogArticleLayout pattern**: Uses `meta`, `footnotes`, `crossLinks`, `secondaryCta` props.

Fail QA if word count below spec, placeholder text remains, schema markup missing, or internal links absent.

## Code Deliverables (defusely.app features, edge functions)

### defusely.app (Cloudflare Workers)
- **TypeScript check**: `npx tsc --noEmit` — zero errors required
- **ESLint**: `npx eslint src/ --ext .ts,.tsx` — clean (Lovable enforces strictly)
- **Vite build**: `npx vite build` — must pass
- **Git sync**: `git fetch origin main` first — Lovable pushes its own commits
- **No TODOs in committed code**: Search for `// TODO`, `// FIXME` — if found, QA fails

### defusely.com (Cloudflare Pages)
- **Bun lockfile**: `bun install` — CRITICAL before any push (Cloudflare's `--frozen-lockfile` fails otherwise)
- **Build**: `npm run build` (runs generate-sitemap.mjs -> vite build -> prerender.mjs) — must pass
- **Sitemap**: `git checkout -- public/sitemap.xml` (restore committed version after build)

### Supabase Edge Functions
- **Import pattern**: MUST use `import { serve } from "https://deno.land/std@0.168.0/http/server.ts"` — NEVER `Deno.serve()` (broke all edge functions March 7 2026)
- **CORS**: Must inline CORS code — MCP can't resolve `../_shared/cors.ts` imports
- **CORS allowed origins**: defusely.com, defusely.app, workers.dev staging, localhost:5173/8080
- **Deploy**: Supabase MCP `deploy_edge_function` or push to main triggers GitHub Actions

Fail QA if TypeScript errors, ESLint violations, build fails, or Deno.serve() used.

## Deploy Verification (post-push)

### Cloudflare Pages (defusely.com)
- Auto-deploys in 30-60s after push to main
- Verify: hard refresh the target URL, check for changes
- Check Cloudflare dashboard for build success

### Cloudflare Workers (defusely.app)
- Auto-deploys on push to main
- Verify: check target page loads, no 500 errors
- Check Sentry for new errors (if connected)

### Supabase Edge Functions
- Deploy via MCP or GitHub Actions
- Verify: call function endpoint, check response
- Check Supabase dashboard for function health

### Browser Verification (Playwright)
- If `playwright-qa` skill available, invoke it for web deliverables
- Check: page loads, interactive elements work, no console errors
- Mobile: verify 375px viewport doesn't break (March 16 audit patterns)

### Error Monitoring
- **Sentry** (if connected): No new errors introduced. Check release health.
- **Supabase**: Check edge function logs for errors
- **Cloudflare**: No 5xx errors in last 1h

Fail QA if deploy fails, Sentry detects new errors, page doesn't load, or mobile breaks.

## Integration Check

- **Notion**: If updating a Notion page, verify page modified and content visible.
- **Apple Notes**: If dispatching summary, verify note exists.
- **Gmail**: If sending outreach, verify draft created or message sent.
- **Apollo**: If enriching leads, verify enrichment returned valid data.
- **Stripe**: If creating products/prices, verify via Stripe MCP list call.
- **Git**: If code pushed, verify commit in remote and CI/CD triggered.
- **Memory**: If writing to basic-memory or memory graph, verify searchable.

Fail QA if integration reports failure.

## Goal Recovery Protocol

If past last planned day and goal not yet met:

1. **Read all prior execution logs** from project directory
2. **Assess the gap**: Progress vs goal? What's remaining?
3. **Append new dayPlan entries** with `autonomous: true` and realistic deadlines
4. **Execute today's new task immediately**
5. **Log recovery run**: "Goal recovery run — extended plan by N days. Reason: [what was blocking]"

Recovery runs should be rare. If triggered repeatedly, score the project against decision filters again — it may be Tier 3/4 work disguised as Tier 1.

## Retry Logic

If QA fails:
- Log the failure with reason
- Do NOT mark done: true
- Do NOT update the JSON data file
- Return to execute step and remediate
- Re-run QA
- Only mark done after QA passes

## Defusely-Specific QA Checklist (run after every push)

- [ ] TypeScript compiles clean (zero errors)
- [ ] ESLint clean (zero violations)
- [ ] Build passes (npm run build or npx vite build)
- [ ] Bun lockfile updated (defusely.com only)
- [ ] No Deno.serve() in edge functions
- [ ] CORS inlined in edge functions (not imported from _shared)
- [ ] Mobile responsive (375px viewport check)
- [ ] Schema markup valid (JSON-LD)
- [ ] Internal links present (cross-reference existing content)
- [ ] Memory updated (basic-memory + memory graph)
- [ ] CLAUDE.md updated (if new pattern or significant change)
- [ ] Drift prevention push summary written
