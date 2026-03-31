# Deploy Integration — Defusely Edition

Loaded when a task involves deploying code to defusely.com or defusely.app.

## Defusely Architecture

| Property | URL | Repo | Deploy | Method |
|----------|-----|------|--------|--------|
| Marketing site | defusely.com | `jayrockliffe-defused/defusely` | Cloudflare Pages | Auto on push to `main` |
| Product app | defusely.app | `jayrockliffe-defused/defusely-confilict-os-reddit-platform` | Cloudflare Workers | Auto on push to `main` |
| Edge functions | — | In app repo `supabase/functions/` | Supabase MCP or GitHub Actions | Manual or CI |

**Supabase project ref**: `poqqwtyqadfzyxcrfcus`
**Stripe account**: `acct_1SvA4E9KYh6gN3yU`
**Cloudflare account ID**: `5649b38cc693eaaebbb38adaaf291597`

## Pre-deploy: defusely.com (Cloudflare Pages)
1. `bun install` — update lockfile (CRITICAL — Cloudflare uses `--frozen-lockfile`)
2. `npm run build` — runs generate-sitemap.mjs -> vite build -> prerender.mjs
3. `git checkout -- public/sitemap.xml` — restore committed version
4. Stage specific files (NOT `git add -A`)
5. Push to main — auto-deploys in 30-60s

## Pre-deploy: defusely.app (Cloudflare Workers)
1. `npx tsc --noEmit` — zero TypeScript errors
2. `npx eslint src/ --ext .ts,.tsx` — clean (Lovable enforces)
3. `npx vite build` — must pass
4. `git fetch origin main` — ALWAYS (Lovable pushes its own commits)
5. Push to main — auto-deploys

## Pre-deploy: Supabase Edge Functions
- **MUST** use `import { serve } from "https://deno.land/std@0.168.0/http/server.ts"` — NEVER `Deno.serve()`
- **MUST** inline CORS code — MCP can't resolve `../_shared/cors.ts`
- CORS origins: defusely.com, defusely.app, workers.dev staging, localhost:5173/8080

## Key Edge Functions
| Function | Version | Purpose |
|----------|---------|---------|
| brand-autofill-fast | v44 | Logo, colors, guidelines |
| brand-autofill-slow | v30 | Reddit threats, patterns, strategic intel |
| send-auth-email | v10 | Auth hook — branded emails via Resend, has keep-warm |
| trial-drip-emails | NEW | Day 3/5/7 onboarding drip. Needs daily cron. |

## Post-deploy verification
1. Hard refresh target URL
2. Sentry scan — no new errors (if connected)
3. Playwright QA — page loads, no console errors (if available)
4. Mobile 375px viewport check
5. Cloudflare/Supabase dashboard — build success, no 5xx

## Rollback
- Critical: `git revert HEAD` + push, or redeploy previous edge function version
- Non-critical: log, create follow-up task
- Always dispatch notification to Jay via Apple Notes

## Mandatory post-deploy
- Update CLAUDE.md if new pattern
- Drift-prevention push summary
- Update basic-memory session state
- Run seo-audit skill if schema/meta/structure changed
