---
name: intel
description: >
  Use this skill when the user says "/intel", "what should I know", "any suggestions",
  "proactive intelligence", "blind spots", "what am I missing", "competitive intel",
  "SEO check", "AI search check", or when the execute skill triggers post-execution
  intelligence. This skill surfaces insights, opportunities, and risks across all
  projects using connected tools.
version: 1.0.0
---

# /intel — Defusely Category Domination Intelligence Engine

Surfaces insights Jay hasn't asked for but needs to know. Optimized for Defusely's four workstreams: Product, SEO & AI Search, Founding Partner Outreach, and Category Positioning.

**North star**: Is Defusely getting closer to category king of Reddit Crisis Response and $5K MRR?

**Competitors to monitor**: Sprinklr, Cision, Brand24, Meltwater, Muck Rack

**Category domination signals to track**:
- Keyword ranking changes for Reddit crisis terms
- AI search citations (ChatGPT, Perplexity mentioning Defusely)
- Founding partner engagement and referrals
- Competitor moves into Reddit crisis space
- Trial signup velocity

See `references/cross-project.md` for the full Defusely workstream map, dependency chains, and automation targets.
See `references/ai-search.md` for AI search monitoring tool hierarchy.

---

## Step 0 — Load context

```bash
SESSION=$(ls /sessions/ | grep -v lost 2>/dev/null | head -1)
DATA_DIR="/sessions/$SESSION/mnt/claude-cowork/Scheduled"
CONFIG="$DATA_DIR/geterdone-config.json"
DATA_FILE="$DATA_DIR/geterdone-projects.json"
```

Read config for connected integrations. Read all active projects.

---

## Step 1 — Determine intel scope

| Trigger | Scope |
|---------|-------|
| Post-execution (auto) | Single project just executed |
| `/intel` (manual) | All active projects |
| `/intel [project]` | Named project only |
| Scheduled (daily brief) | All active projects + general environment |

---

## Step 2 — Cross-project analysis

Read `references/cross-project.md` for pattern library.

Scan all active projects for:

**Leverage opportunities**
- Content from project A reusable in project B
- Code/templates that could be extracted into shared assets
- Research findings applicable across projects

**Conflict detection**
- Scheduling conflicts between projects
- Resource contention (same person assigned to parallel tasks)
- Deadline clustering (multiple projects ending same week)

**Dependency mapping**
- Project B waiting on output from project A
- External dependencies (API availability, third-party approvals)
- Implicit dependencies not captured in plans

**Automation candidates**
- Recurring manual tasks across projects
- Tasks that follow a pattern suitable for skill creation
- Processes that could be scheduled instead of manual

---

## Step 3 — Connected tool intelligence

Only query tools that are connected (check config). Skip gracefully if unavailable.

### SEO & AI Search (read references/ai-search.md for tool hierarchy)

**Fallback order** (use first available, never call expensive Ahrefs tools):
1. **GSC** (Google Search Console) — `gsc-keywords`, `gsc-performance-history`
2. **Brand Radar** — `brand-radar-mentions-overview`, `brand-radar-sov-overview`
3. **Frase AEO** — `list_aeo_prompts`, `get_prompt_answers`
4. **AirOps** — if connected, check workflow outputs
5. **Web Analytics** — `web-analytics-stats`, `web-analytics-sources`
6. **Rank Tracker** — `rank-tracker-overview`
7. **WebSearch** — fallback for manual competitive checks

Surface: ranking changes, mention spikes, AI citation appearances, traffic anomalies.

### Error Monitoring
- **Sentry** — `search_issues` for new/unresolved errors in active code projects
- Surface: error rate changes, new issue types, regression indicators

### Outreach & Sales
- **Apollo** — check recent enrichment results, sequence performance
- Surface: response rates, new leads matching ICP, sequence health

### Design
- **Figma** — check for new comments on active design files
- **Canva** — check for shared design updates
- Surface: unresolved design feedback, design-dev sync issues

### Calendar
- **Google Calendar** — `gcal_list_events` for upcoming week
- Surface: meeting load vs deep work time, scheduling conflicts with project deadlines

---

## Step 4 — Pattern recognition

Search basic-memory for:
- Previous intel reports (compare trends)
- User decisions and their outcomes (what worked last time)
- Stale projects that haven't been touched in 2+ weeks

Search memory graph for:
- Disconnected entities (projects with no recent relations)
- Clusters of related work that could be consolidated

---

## Step 5 — Prioritize findings

Rank all findings by:
1. **Impact**: How much does this affect active project goals?
2. **Urgency**: Is there a time window for action?
3. **Effort**: How much work to act on this?
4. **Novelty**: Has the user seen this before?

Keep the top 3-5 findings. Don't overwhelm with noise.

---

## Step 6 — Format and dispatch

```
Intelligence Brief — [Date]
═══════════════════════════

[fire] High Priority
  [Finding with specific action]

[lightning] Opportunities
  [Finding with specific action]

[eye] Blindspots
  [Finding with specific action]

[calendar] Upcoming
  [Time-sensitive items]

Actions:
  1. [Most impactful thing to do right now]
  2. [Second action]
  3. [Third action]
```

Dispatch to configured channel (Apple Notes, Slack, Gmail, Notion).

---

## Step 7 — Update memory

Write intel findings to basic-memory:
- Tag: "intel-brief"
- Include date, findings, recommended actions
- Link to relevant project entities

This creates a searchable history of intelligence for trend analysis.

---

## Step 8 — Skill creation trigger

If intel detects a pattern that repeats 3+ times across projects:
- Suggest creating a new skill using the skill-creator
- Draft the skill spec based on the pattern
- Ask user for approval before creating

Example: "I've noticed you research competitors the same way in 3 projects.
Want me to create a /competitor-research skill that automates this?"

---

## Notes

- Intel runs automatically after every /execute — never skip it
- SEO/AI search tools follow the fallback hierarchy — never call expensive tools first
- Cross-project intelligence is the highest-value output
- Keep findings actionable — "X is happening" is useless; "Do Y because X is happening" is useful
- Memory update is mandatory
- Skill creation suggestions should be rare and high-confidence
