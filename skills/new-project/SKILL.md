---
name: new-project
description: >
  This skill should be used when the user says "/new-project", "start a new project",
  "plan a project", "create a project", "I want to build X", or describes a multi-day
  goal that needs planning and scheduling. Handles smart triage, decision filters,
  calendar-aware scheduling, and day-by-day execution plan generation.
version: 1.0.0
---

## Defusely Context

This plugin is customized for **Defusely** — a Reddit Crisis Response SaaS platform (defusely.com / defusely.app). All projects are evaluated against Defusely's north star: **become the category king of Reddit Crisis Response and reach $5K MRR.**

Four workstreams: Product Development, SEO & AI Search Domination, Founding Partner Outreach, Marketing & Category Positioning. Decision filters are revenue-weighted. Templates include Defusely-specific deploy pipelines, outreach sequences, and SEO workflows.

See `references/decision-filters.md` for the revenue-weighted scoring system and priority stack.
See `references/templates.md` for Defusely-specific project templates.

---

## Overview

The `/new-project` skill handles intelligent project intake and triage. It classifies work as either quick tasks (handled inline) or multi-day projects (proceed through full workflow), applies decision filters to validate project alignment, generates execution plans with automation levels, schedules calendar-aware milestones, and stores everything in persistent JSON + memory graph.

---

## 9-Step Workflow

### Step 0: Load Configuration
Load user configuration from:
- `geterdone-config.json` — user preferences, calendar URL, memory graph access
- `geterdone-projects.json` — existing projects, taxonomy, decision filter thresholds
- `geterdone-templates.json` — available project templates and defaults

If files don't exist, initialize with sensible defaults.

### Step 1: Smart Triage
Classify the incoming request:
- **Quick Task** (< 4 hours estimated): Handle inline — create a single dayPlan entry with today's date and execute directly. No full project scaffolding needed.
- **Multi-Day Project** (≥ 4 hours, spans 2+ calendar days): Continue to Step 2
- **Standalone Task**: If single action item, handle inline but allow user to upgrade to full project if desired.

Decision logic:
- Parse user input for time estimates, deadline language, and scope signals
- If duration unclear, ask: "How long will this take?" and "When's your target completion date?"
- If multi-day, proceed; otherwise handle inline

### Step 2: Capture Project Essentials
Gather structured data:
- **Name**: Clear, memorable project title
- **Goal**: 1-2 sentence outcome definition
- **Timeline**: Start date, target completion, key milestones
- **Domain**: Category (Work, Personal, Learning, Creative, Business)
- **Priority**: High / Medium / Low
- **Template**: Choose from available templates or custom (see references/templates.md)

Use conversational flow; don't force all fields at once. Fill in from context where possible.

### Step 3: Run Decision Filters (Revenue-Weighted)
Apply Defusely's revenue-weighted decision filters (see references/decision-filters.md for full scoring):
1. **Revenue Impact** (Weight: 3x): Does this directly move toward collecting money? Score 0–3
2. **Category Ownership** (Weight: 2x): Does this reinforce Defusely as THE Reddit Crisis Response platform? Score 0–3
3. **Leverage & Compounding** (Weight: 1.5x): Does the output compound? Will it keep working after you stop? Score 0–3
4. **Automation ROI** (Weight: 1x): Can Geterdone execute this autonomously? Poor/Fair/Good/Strong (0-3)
5. **Energize Jay** (Weight: 0.5x — tiebreaker only): Does this energize? Score 0–3

**Weighted score formula**:
```
Score = (Revenue × 3) + (Category × 2) + (Leverage × 1.5) + (Automation × 1) + (Energize × 0.5)
Max = 24
```

**Tier-based passing criteria**:
- **Tier 1 (Score >= 16)**: Execute immediately — high revenue, high category
- **Tier 2 (Score 10-15)**: Schedule this week — important but not urgent
- **Tier 3 (Score 5-9)**: Backlog — only if Tier 1 and 2 empty
- **Tier 4 (Score < 5)**: Don't do — actively resist. If Jay insists, document override and set 1-week review checkpoint

### Step 4: Draft Execution Plan
Break project into phases and tasks. Classify each task's automation level:
- **Autonomous** (`true`): Geterdone executes without human input (scheduled scripts, data pulls, template fills)
- **Semi-Auto** (`"semi"`): Geterdone prepares, human validates/submits (email drafts, meeting agendas, reports)
- **Manual** (`false`): Human owns the work; Geterdone tracks progress

Example structure:
```
Phase 1: Research (3 days)
  - Task 1a: Gather competitor data [Semi-Auto]
  - Task 1b: Document findings [Manual]
Phase 2: Strategy (2 days)
  - Task 2a: Draft recommendations [Semi-Auto]
  - Task 2b: Present to stakeholder [Manual]
```

**After defining phases, flatten into a `dayPlan` array for the execution engine.** Each dayPlan entry maps to a single calendar day. This is what /execute, /done, /update, and /review all read:
```json
"dayPlan": [
  { "day": 1, "date": "2026-04-01", "task": "Gather competitor data", "deliverable": "competitor-analysis.md", "autonomous": "semi", "done": false, "phase": "Research" },
  { "day": 2, "date": "2026-04-02", "task": "Document findings", "deliverable": "research-summary.md", "autonomous": false, "done": false, "phase": "Research" },
  { "day": 3, "date": "2026-04-03", "task": "Draft recommendations", "deliverable": "recommendations.md", "autonomous": "semi", "done": false, "phase": "Strategy" }
]
```

### Step 5: Calendar-Aware Scheduling
Check user's calendar (via Google Calendar integration) to:
- Identify free time blocks matching task durations
- Detect meeting clusters and avoid overload
- Schedule milestones between existing commitments
- Flag calendar conflicts and offer alternatives

Use `gcal_find_my_free_time` (see references/calendar-aware.md) to find optimal scheduling windows.

### Step 6: Write to JSON Data File
Persist the project to `geterdone-projects.json`. Must include both `phases` (planning structure) AND `dayPlan` (execution structure):
```json
{
  "id": "proj_<uuid>",
  "name": "Project Name",
  "goal": "Outcome description",
  "domain": "Work|Personal|Learning|Creative|Business",
  "priority": "High|Medium|Low",
  "status": "Active",
  "created_at": "2026-03-29T14:30:00Z",
  "target_completion": "2026-04-15",
  "color": "#1251C0",
  "template_used": "Content Calendar",
  "pct": 0,
  "lastUpdated": "2026-03-29T14:30:00Z",
  "decision_filters": {
    "revenue_impact": 2,
    "category_ownership": 3,
    "leverage": 2,
    "automation_roi": "strong",
    "energize": 2,
    "weighted_score": 17.5,
    "tier": 1,
    "override": false
  },
  "phases": [ ... ],
  "dayPlan": [
    { "day": 1, "date": "2026-03-30", "task": "Task name", "deliverable": "output.md", "autonomous": "semi", "done": false, "phase": "Phase 1" }
  ],
  "executionLog": []
}
```

See references/project-schema.md for full schema details.

### Step 7: Create Scheduled Task
For each task with automation level `true` or `"semi"`, create a scheduled task:
- Use the Cowork `create_scheduled_task` tool to set up recurring or one-time execution
- Configure task prompt to reference the project ID and day number
- Set notifications via Apple Notes or Gmail if applicable
- Link back to project JSON for context retrieval

### Step 8: Write to Memory Graph
Create memory entries to track the project long-term:
1. **Project Entity**: `entityName: "Project: <name>"`, `entityType: "project"`, observations capturing goal, domain, priority, and decision filter scores
2. **Milestone Relations**: Links from project to key milestones and deliverables
3. **Stakeholder Relations**: Links to people involved (team members, stakeholders)
4. **Template Relation**: Link to the template used, for pattern matching on future projects

Example:
```
Entity: "Project: Q2 Content Calendar"
Type: project
Observations:
  - "Goal: Publish 12 blog posts and 24 social updates"
  - "Domain: Work"
  - "Priority: High"
  - "Decision scores: Revenue Impact (2×3=6), Category Ownership (3×2=6), Leverage (3×1.5=4.5), Automation (Strong×1=3), Energize (2×0.5=1) = 20.5 Tier 1"

Relation: "Project: Q2 Content Calendar" → "Content Calendar" (uses_template)
Relation: "Project: Q2 Content Calendar" → "User" (owned_by)
Relation: "Project: Q2 Content Calendar" → "Milestone: Publish Week 1 Posts" (has_milestone)
```

### Step 9: Present the Plan
Output a clear, human-readable summary:
- Project name, goal, and timeline
- Decision filter assessment (pass/fail with scores)
- Phase breakdown with task classifications
- Calendar conflicts (if any) and proposed solutions
- First three actions the user should take
- Link to JSON data file for future reference
- Scheduled tasks created (list with next-run times)

Example output:
```
✓ Project Created: Q2 Content Calendar
├─ Goal: Publish 12 blog posts and 24 social updates
├─ Timeline: 2026-03-30 → 2026-06-30 (13 weeks)
├─ Priority: High
├─ Status: Active
├─ Template: Content Calendar
└─ Color: #1251C0

Decision Filters (Weighted Score: 18.5 — Tier 1: Execute immediately):
├─ Revenue Impact: 2/3 (×3 = 6)
├─ Category Ownership: 3/3 (×2 = 6)
├─ Leverage: 3/3 (×1.5 = 4.5)
├─ Automation ROI: Strong (×1 = 3)
└─ Energize: 2/3 (×0.5 = 1) [tiebreaker]

Phases:
├─ Phase 1: Content Planning (3 days)
│  ├─ Research audience topics [Semi-Auto, 2h]
│  └─ Draft editorial calendar [Manual, 3h]
└─ Phase 2: Week 1 Publishing (7 days)
   ├─ Write post 1 [Manual, 4h, scheduled 2026-03-30]
   └─ Schedule social posts [Semi-Auto, 1h]

Calendar: No conflicts detected. Free time allocated for all tasks.

Next Steps:
1. Review and approve editorial calendar (draft ready at geterdone-projects.json#phases.0)
2. Begin Week 1 research (scheduled for 2026-03-30 09:00 AM)
3. Share calendar link with team members

Stored:
├─ Project JSON: geterdone-projects.json
├─ Scheduled Tasks: 3 tasks created (next run: 2026-03-30)
└─ Memory Graph: Project entity + 5 relations created
```

---

## Troubleshooting

- **Triage decision unclear**: Ask for explicit duration and deadline
- **Decision filters fail**: Offer user choice to override; document override reason in memory
- **Calendar conflicts**: Suggest alternative dates; ask if compression or delegation is viable
- **JSON write error**: Log failure, retry once, then ask user to save manually
- **Memory write error**: Log failure, project still created in JSON, but memory link is missing (flag for manual sync later)

---

## Related Skills

- `/execute` — Autonomous execution engine: picks up today's dayPlan task, does the work, runs QA, writes output
- `/done` — Manual completion: mark a semi-auto or manual task done after Jay finishes the work
- `/update` — Manual override: pause, resume, block, adjust plan, add days, change goals
- `/review` — Progress review: velocity, completion rates, cross-project patterns, proactive intelligence
- `/status` — Visual dashboard: kanban cards, automation status, revenue scoreboard
- `/intel` — Proactive intelligence: SEO checks, AI search monitoring, competitive signals, blind spots
