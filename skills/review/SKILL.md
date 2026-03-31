---
name: review
description: >
  Use this skill when the user says "/review", "how are my projects doing",
  "weekly review", "progress report", "what happened this week", "review all projects",
  "performance summary", or wants a comprehensive progress assessment across all
  Geterdone projects. This skill generates variable-cadence reviews, dispatches
  summaries to mobile, and surfaces proactive intelligence.
version: 1.0.0
---

# /review — Defusely Progress Review & Category Domination Check

Variable-cadence review across all Defusely workstreams. Generates performance analysis, dispatches summaries, and surfaces blindspots.

**Every review must answer these questions**:
1. Is Defusely closer to $1K MRR than last review? What moved?
2. Are we winning or losing on Reddit crisis search terms? (GSC + Brand Radar)
3. Are founding partners engaged or going dark? (Supabase usage data + outreach tracking)
4. Is any competitor moving into Reddit crisis space? (competitive intel)
5. What's the #1 thing Jay should do this week to accelerate revenue?

**Revenue scoreboard** (include in every review):
- Current MRR: $[X]
- Trial signups this period: [N]
- Active founding partners: [N]
- Keyword rankings: [trending up/down/flat]
- AI search citations: [Y/N, which engines]

Read `../new-project/references/project-schema.md` for data file path and schema.

---

## Step 0 — Load data

```bash
SESSION=$(ls /sessions/ | grep -v lost 2>/dev/null | head -1)
DATA_DIR="/sessions/$SESSION/mnt/claude-cowork/Scheduled"
CONFIG="$DATA_DIR/geterdone-config.json"
DATA_FILE="$DATA_DIR/geterdone-projects.json"
TODAY=$(date +%Y-%m-%d)
```

Read config for review cadence (default: weekly). Read all projects.

---

## Step 1 — Determine review scope

Check config `reviewCadence` field:
- `"daily"` — review since yesterday
- `"weekly"` — review last 7 days (default)
- `"biweekly"` — review last 14 days
- `"monthly"` — review last 30 days
- `"on-demand"` — only when user triggers

If user specifies a project name, scope to that project only.
If user says "all", review everything.

---

## Step 2 — Gather execution data

For each Active project in scope:
1. Count dayPlan entries completed in the review period
2. Count days missed (done == false, date in past)
3. Calculate velocity: tasks completed / calendar days elapsed
4. Read execution logs for the period
5. Collect all outputFiles produced
6. Note any status changes (Active → Blocked, etc.)
7. Note any plan adjustments logged

---

## Step 3 — Performance analysis

Read `references/performance-patterns.md` for pattern detection.

For each project, assess:

| Metric | Calculation | Signal |
|--------|-------------|--------|
| Completion rate | done_tasks / planned_tasks | <70% = falling behind |
| Velocity trend | this_period vs last_period | declining = risk |
| Goal proximity | pct vs expected_pct_by_now | >10% gap = concern |
| Blocked days | count of Blocked status periods | >2 days = intervention needed |
| Autonomy ratio | autonomous_done / total_done | low = user bottleneck |

Flag projects that need attention:
- **At risk**: completion rate < 70% or goal proximity gap > 15%
- **Blocked**: status == Blocked for > 48 hours
- **Ahead**: completion rate > 100% (bonus tasks completed)
- **Stale**: no execution in review period despite Active status

---

## Step 4 — Cross-project intelligence

Read `references/session-analysis.md` for cross-project patterns.

Search basic-memory and memory graph for:
- **Reusable assets**: content, templates, or code from one project useful in another
- **Duplicated effort**: similar tasks running in parallel across projects
- **Dependency chains**: project B blocked until project A delivers X
- **Skill patterns**: repeated task types that should become automated workflows

Surface findings as actionable suggestions.

---

## Step 5 — Generate review report

Format depends on dispatch target. Core structure:

```
Review: [Date Range]
════════════════════

[For each project:]
[status-emoji] [Project Name] — [pct]% complete
  Completed: [N] of [M] tasks this period
  Velocity: [trend arrow] [tasks/day]
  Goal: [one-sentence proximity assessment]
  [If at-risk]: ⚠ [specific concern]
  [If blocked]: 🔒 [blocker description]

Cross-Project Intelligence:
  • [Finding 1]
  • [Finding 2]

Recommendations:
  1. [Most impactful action]
  2. [Second action]
  3. [Third action]

Next review: [date based on cadence]
```

---

## Step 6 — Dispatch summary

Read config `dispatchChannel`. Push the review to the configured channel.

| Channel | Method |
|---------|--------|
| Apple Notes | `add_note` with formatted summary |
| Slack | `slack_send_message` to configured channel |
| Gmail | `gmail_create_draft` with summary as email body |
| Notion | Create page in configured Notion database |

If no dispatch configured, display in chat only.

---

## Step 7 — Update memory

Write review summary to basic-memory:
- Project: "Geterdone Review [date]"
- Key findings, velocity trends, recommendations
- Link to each reviewed project entity

Update memory graph with review relations.

---

## Step 8 — Schedule next review

If reviewCadence is set and no existing schedule:
- Create scheduled task for next review date
- Task prompt: "Run /review for all active projects"

If schedule exists, verify next run time is correct.

---

## Notes

- Review cadence is configurable per user, not hardcoded weekly
- Always dispatch to mobile if a channel is configured — reviews are most useful on the go
- Cross-project intelligence is the highest-value output; prioritize it
- If user asks "what should I focus on today?", run a mini daily review
- Memory update is mandatory after every review
