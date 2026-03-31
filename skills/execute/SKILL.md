---
name: execute
description: >
  Use this skill when the user says "/execute", "run today's task", "do the work",
  "execute the plan", "what's Geterdone doing today", "run geterdone", "continue the project",
  or when a scheduled Geterdone execution fires automatically. This is the autonomous engine —
  Claude reads the plan, does the actual work, runs QA, generates proactive intelligence,
  updates memory, and dispatches suggestions to mobile.
version: 1.0.0
---

## Defusely Context

Customized for **Defusely** — Reddit Crisis Response SaaS (defusely.com / defusely.app). Execution follows Defusely deploy pipelines, QA gates, and integrations.

Key references (loaded on demand):
- `references/qa-gates.md` — Defusely QA checklist (TypeScript, ESLint, bun lockfile, CORS, mobile, schema)
- `references/integrations/deploy.md` — Cloudflare Pages + Workers + Supabase deploy pipeline
- `references/integrations/seo.md` — SEO patterns, schema markup, BlogArticleLayout
- `references/integrations/outreach.md` — Apollo + Gmail founding partner outreach
- `references/integrations/ai-search.md` — AI search optimization, Brand Radar, tool hierarchy

Always run `defusely-build-deploy` skill before any push. Always run `drift-prevention` after.

---

## Step 1 — Load projects and find today's work

```bash
SESSION=$(ls /sessions/ | grep -v lost 2>/dev/null | head -1)
DATA_DIR="/sessions/$SESSION/mnt/claude-cowork/Scheduled"
CONFIG="$DATA_DIR/geterdone-config.json"
DATA_FILE="$DATA_DIR/geterdone-projects.json"
TODAY=$(date +%Y-%m-%d)
```

Read config and data file. For each Active project, find today's dayPlan entry where done == false.
If user named a specific project, run only that one. Otherwise run all with work due.
If all entries done → set status = "Done", pct = 100.
If past last day and goal not met → trigger goal recovery run (read references/qa-gates.md for recovery protocol).

## Step 2 — Autonomous vs semi-auto vs manual

| autonomous | Executed by | When |
|---|---|---|
| true | Daily schedule | Automatically at configured time |
| "semi" | Claude prepares, user approves | Claude drafts via Chrome, user clicks publish |
| false | User runs /execute | Manual trigger |

When manually triggered: execute today's task (any type). Also catch up missed autonomous tasks.
When scheduled: only execute autonomous: true. Skip semi/manual and log note.
Log triggeredBy: "manual" or "schedule" in execution record.

## Step 3 — Execute the day's task

Do the actual work. This is execution, not planning.
- Read task and deliverable fields
- Check output folder for prior day outputs, build on them
- Execute fully: real text, real research, real code, real drafts — no placeholders
- Save output to `$DATA_DIR/geterdone/<project-id>/day-<N>-<slug>.md`

For semi-auto tasks: Claude creates the full deliverable AND prepares it for publishing via Chrome MCP (opens editor, pastes content). User just reviews and clicks publish.

The goal is the north star. If today's task doesn't serve the goal, adapt it.

## Step 4 — QA gate

Read `references/qa-gates.md` for verification protocol.
- Verify file exists and is non-empty
- For web tasks: invoke playwright-qa skill for browser verification (if available)
- For content: check word count meets spec, verify no placeholder text remains
- For code: verify it runs without errors
- For deploys: check Cloudflare status (if connected), run Sentry scan (if connected)
Do NOT mark done until QA passes.

## Step 5 — Post-execution intelligence

Read `references/intel-post-exec.md`.
- Analyze what was produced
- Cross-reference with basic-memory for related context, reusable assets, duplicated effort
- Check memory graph for cross-project leverage
- Generate proactive suggestions

## Step 6 — Update data file

- Backup JSON first (always)
- Mark dayPlan entry done: true
- Append execution log entry with summary, output files, goal progress, adjustments, triggeredBy
- Recalculate pct
- Set lastUpdated
- Write full JSON back

## Step 7 — Memory update (mandatory — never skip)

- Write to basic-memory: project name, day completed, deliverable summary, goal progress
- Update memory graph: link deliverable to project entity
- If code project with git push: verify CLAUDE.md and relevant docs are updated
- If Notion connected: update project page with execution log
- FAILURE TO UPDATE MEMORY = TASK NOT COMPLETE

## Step 8 — Dispatch to mobile

Read config for dispatch channel. Push proactive suggestions to configured channel.
If Apple Notes: add_note with execution summary + suggestions.
If Slack: slack_send_message to configured channel.

## Step 9 — Summary card

```
[check] [Project Name] — Day N of M complete  [lightning auto | arrow manual | semi semi-auto]
Task: [what was done]
Output: [computer:// link to file]
Goal progress: [one sentence]
Suggestions: [proactive intelligence findings]
[If adjustments]: Plan adjusted: [what changed]
[If blocked]: Blocked: [what's needed from user]
```

## Goal Recovery Run

If past last planned day and goal not met:
1. Read all prior execution summaries
2. Assess gap
3. Append new dayPlan entries with autonomous classification
4. Execute today's new task immediately
5. Log "Goal recovery run — extended plan by N days"
