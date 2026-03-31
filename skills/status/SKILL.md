---
name: status
description: >
  Use this skill when the user says "/status", "show me my projects",
  "dashboard", "what's happening", "project overview", or wants to see
  a visual summary of all Geterdone projects. Renders an offline HTML
  dashboard with kanban layout, pixel characters, and automation status.
version: 1.0.0
---

# /status — Defusely Command Center

Renders an interactive HTML dashboard showing all Defusely workstreams, progress toward $5K MRR, automation status, and category domination signals. No network calls — system fonts only, all assets inline.

**Dashboard must include a Defusely Revenue Scoreboard section**:
- MRR target vs actual ($0 -> $1K -> $5K)
- Trial signups (total and this week)
- Founding partners (contacted / responded / onboarded)
- SEO keyword rankings (top 5 target keywords with position)
- AI search citation status (cited Y/N by ChatGPT, Perplexity)
- Priority stack progress (which of the 13 items are done/in-progress/blocked)

Read `../new-project/references/project-schema.md` for data file path and schema.

---

## Step 1 — Load data

```bash
SESSION=$(ls /sessions/ | grep -v lost 2>/dev/null | head -1)
DATA_DIR="/sessions/$SESSION/mnt/claude-cowork/Scheduled"
CONFIG="$DATA_DIR/geterdone-config.json"
DATA_FILE="$DATA_DIR/geterdone-projects.json"
STATUS_FILE="$DATA_DIR/geterdone-status.html"
TODAY=$(date +%Y-%m-%d)
```

Read config and projects JSON. If no projects exist, show empty state with
link to /new-project.

---

## Step 2 — Sync automation status

Call `list_scheduled_tasks` to get live state for all Geterdone schedules.

For each scheduled task, determine today-status:

| Condition | Status | Dot color |
|-----------|--------|-----------|
| lastRunAt is today, clean completion | ran_clean | green |
| lastRunAt is today, partial/cutoff | partial | amber |
| lastRunAt is today, rate limited | rate_limited | red |
| No cronExpression (manual-only) | manual | grey |
| nextRunAt today but hasn't run | upcoming | blue |
| nextRunAt is future date | upcoming | grey |

If `list_scheduled_tasks` unavailable, skip and note in dashboard footer.

---

## Step 3 — Build HTML dashboard

Write a single self-contained HTML file to `$STATUS_FILE`. Rules:

**No network calls:**
- System fonts only: `-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif`
- Monospace: `"SF Mono", "Monaco", "Courier New", monospace`
- All SVG/CSS inline — no CDN imports, no Google Fonts, no jsDelivr

**Layout — three tabs:**

### Tab 1: Projects (default)
Kanban-style cards for each project. **Cards are clickable** — clicking a card expands it to show the task drill-down panel.

**Collapsed card (default view)** shows:
- Project name + color dot
- Status badge (Active/Paused/Done/Blocked) with schema colors
- Priority badge (High/Medium/Low)
- Progress bar with pct label
- Task count badge: "[N] tasks remaining" (count of dayPlan entries where `done == false`)
- Current day task (today's dayPlan entry, if any)
- Pixel character (read references/characters.md for SVG data)

**Expanded card (after click)** shows all remaining tasks from dayPlan:
- Each undone dayPlan entry as a row: `Day [N] — [task] — [deliverable] — [autonomous badge]`
- Autonomous badge colors: green for `true`, amber for `"semi"`, grey for `false`
- Each task row has a "Help me with this" button
- Clicking "Help me with this" copies the task context to clipboard in this format:
  ```
  /execute project:[project_name] day:[day_number]
  ```
  And displays a message: "Copied! Paste this in a new Cowork session to start working on this task."
- Also show completed tasks (greyed out, strikethrough) below remaining tasks for context
- A "Collapse" link at the bottom to return to card view

Card arrangement:
- Active projects first, sorted by priority (High → Medium → Low)
- Then Paused, Blocked, Done

### Tab 2: Automations
Table of all scheduled tasks with:
- Task name
- Status dot (color from Step 2)
- Last run time
- Next run time
- Schedule expression (human-readable)

Summary strip at top: Total | Ran Clean | Partial/Rate-Limited | Upcoming

### Tab 3: Intelligence
Latest intel findings (if available). Read from basic-memory for
recent intel-brief entries. Show:
- Date of last intel run
- Top 3 findings with priority indicators
- Recommended actions
- Link to run /intel for fresh data

**Design tokens:**
```css
:root {
  --bg-primary: #ffffff;
  --bg-secondary: #f8fafc;
  --bg-card: #ffffff;
  --text-primary: #0f172a;
  --text-secondary: #64748b;
  --border: #e2e8f0;
  --radius: 12px;
  --shadow: 0 1px 3px rgba(0,0,0,0.08);
  --font-sans: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
  --font-mono: "SF Mono", "Monaco", "Courier New", monospace;
}
```

**Status badge colors** (from project schema):

| Status | Text | Background |
|--------|------|------------|
| Active | #1251C0 | #EEF4FF |
| Paused | #64748B | #F1F5F9 |
| Done | #10B981 | #ECFDF5 |
| Blocked | #7C3AED | #F5F3FF |

**Priority badge colors:**

| Priority | Text | Background |
|----------|------|------------|
| High | #D97706 | #FFFBEB |
| Medium | #64748B | #F1F5F9 |
| Low | #94A3B8 | #F8FAFC |

---

## Step 4 — Pixel characters

Read `references/characters.md` for SVG pixel art data.

Assign one character per project (cycle through available characters).
Characters appear on project cards with idle bob animation:
```css
@keyframes bob {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-3px); }
}
.character { animation: bob 2s ease-in-out infinite; }
```

---

## Step 5 — Write and present

Write the complete HTML to `$STATUS_FILE`.
Present to user with: `[View your dashboard](computer:///path/to/geterdone-status.html)`

Also display a text summary in chat:
```
Dashboard updated — [N] projects

[For each active project:]
[color-dot] [Name] — [pct]% [status]
  Today: [current task or "No task scheduled"]

Automations: [N] total, [N] ran clean, [N] upcoming
```

---

## Step 6 — Memory update

Write dashboard generation event to basic-memory:
- Date, project count, overall health summary
- Any projects flagged as at-risk

---

## Notes

- Dashboard must be fully offline — zero network requests
- System fonts only, all SVGs inline, all CSS inline
- Use dynamic session paths, never hardcoded user paths
- Characters are decorative fun — don't let them bloat the HTML
- Tab switching is pure CSS (no JavaScript framework needed, vanilla JS only)
- Dashboard should render correctly in any modern browser
- Mobile responsive: cards stack vertically on small screens
