# Project Schema Reference

## JSON Structure

All projects are stored in `geterdone-projects.json` with the following schema:

```json
{
  "version": "1.0",
  "projects": [
    {
      "id": "proj_<uuid>",
      "name": "string (max 100 chars)",
      "goal": "string (1-2 sentences)",
      "description": "string (optional, detailed context)",
      "domain": "Work | Personal | Learning | Creative | Business",
      "priority": "High | Medium | Low",
      "status": "Active | Paused | Done | Blocked",
      "created_at": "ISO 8601 timestamp",
      "updated_at": "ISO 8601 timestamp",
      "target_completion": "ISO 8601 date (YYYY-MM-DD)",
      "actual_completion": "ISO 8601 date (optional, set when status = Done)",
      "color": "hex color code (see palette below)",
      "template_used": "string (template name or 'custom')",
      "owner": "user_id or 'me'",
      "stakeholders": ["user_id", "email@example.com"],
      "decision_filters": {
        "revenue_impact": "0 | 1 | 2 | 3 (weight: 3x)",
        "category_ownership": "0 | 1 | 2 | 3 (weight: 2x)",
        "leverage": "0 | 1 | 2 | 3 (weight: 1.5x)",
        "automation_roi": "0 | 1 | 2 | 3 (weight: 1x) — 0=poor, 1=fair, 2=good, 3=strong",
        "energize": "0 | 1 | 2 | 3 (weight: 0.5x — tiebreaker only)",
        "weighted_score": "number (0–24, calculated)",
        "tier": "1 | 2 | 3 | 4",
        "override": "boolean (true if user proceeded despite tier 4)",
        "override_reason": "string (optional, why override was approved)"
      },
      "phases": [
        {
          "id": "phase_<uuid>",
          "name": "string",
          "description": "string (optional)",
          "duration_days": "integer",
          "target_start": "ISO 8601 date",
          "target_end": "ISO 8601 date",
          "status": "Pending | Active | Complete | Blocked",
          "tasks": [
            {
              "id": "task_<uuid>",
              "name": "string",
              "description": "string (optional)",
              "autonomous": "true | 'semi' | false",
              "scheduled_for": "ISO 8601 timestamp or null",
              "estimated_hours": "number (decimal, e.g., 2.5)",
              "actual_hours": "number (optional, set when task complete)",
              "status": "Pending | In Progress | Complete | Blocked | Deferred",
              "assignee": "user_id or 'me'",
              "dependencies": ["task_id1", "task_id2"],
              "output_artifact": "string (path to deliverable or reference)"
            }
          ]
        }
      ],
      "pct": "integer (0-100, auto-calculated from dayPlan completion)",
      "lastUpdated": "ISO 8601 timestamp",
      "dayPlan": [
        {
          "day": "integer (1-indexed day number)",
          "date": "YYYY-MM-DD",
          "task": "string (what to do this day)",
          "deliverable": "string (expected output file or artifact)",
          "autonomous": "true | 'semi' | false",
          "done": "boolean (false until completed)",
          "phase": "string (phase name this day belongs to)"
        }
      ],
      "executionLog": [
        {
          "date": "YYYY-MM-DD",
          "day": "integer",
          "status": "completed | failed | skipped",
          "task": "string",
          "outputFile": "string (path to deliverable)",
          "goalProgress": "string (percentage or status)",
          "adjustments": "string (if plan was modified)",
          "suggestions": "string (from post-exec intelligence)",
          "triggeredBy": "manual | schedule",
          "completedAt": "ISO 8601 timestamp"
        }
      ],
      "notes": "string (freeform notes about the project)",
      "tags": ["tag1", "tag2"],
      "memory_entities": ["entity_id1", "entity_id2"],
      "scheduled_task_ids": ["scheduled_task_1", "scheduled_task_2"]
    }
  ]
}
```

## Field Definitions

### Core Fields

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `id` | string | Yes | Unique identifier: `proj_` + UUID v4 |
| `name` | string | Yes | Project title, max 100 characters |
| `goal` | string | Yes | Outcome statement, 1-2 sentences |
| `description` | string | No | Extended context, background, constraints |
| `domain` | enum | Yes | Work, Personal, Learning, Creative, Business |
| `priority` | enum | Yes | High, Medium, Low |
| `status` | enum | Yes | See status values below |
| `created_at` | ISO 8601 | Yes | Set at project creation |
| `updated_at` | ISO 8601 | Yes | Updated whenever project changes |
| `target_completion` | ISO 8601 date | Yes | Target end date (YYYY-MM-DD format) |
| `actual_completion` | ISO 8601 date | No | Set when status = Done |
| `color` | hex | Yes | UI color; see palette below |
| `template_used` | string | Yes | Template name or "custom" |
| `owner` | string | Yes | User ID or "me" |
| `stakeholders` | array | No | List of user IDs or emails |

### Decision Filters (Revenue-Weighted)

| Field | Type | Weight | Notes |
|-------|------|--------|-------|
| `revenue_impact` | integer 0–3 | 3x | Does this directly move toward collecting money? |
| `category_ownership` | integer 0–3 | 2x | Does this reinforce Defusely as THE Reddit Crisis Response platform? |
| `leverage` | integer 0–3 | 1.5x | Does the output compound? Will it keep working after you stop? |
| `automation_roi` | integer 0–3 | 1x | Can Geterdone execute this autonomously? (0=poor, 1=fair, 2=good, 3=strong) |
| `energize` | integer 0–3 | 0.5x | Does this energize Jay? (tiebreaker only) |
| `weighted_score` | number | — | Calculated: `(revenue×3) + (category×2) + (leverage×1.5) + (automation×1) + (energize×0.5)`. Max = 24 |
| `tier` | integer 1–4 | — | 1 (>=16): Execute immediately. 2 (10-15): Schedule this week. 3 (5-9): Backlog. 4 (<5): Don't do. |
| `override` | boolean | — | True if user proceeded despite tier 4 |
| `override_reason` | string | — | Why override was approved (if applicable) |

**Tier-Based Passing Criteria**:
- **Tier 1 (Score >= 16)**: Execute immediately — high revenue, high category
- **Tier 2 (Score 10-15)**: Schedule this week — important but not urgent
- **Tier 3 (Score 5-9)**: Backlog — only if Tier 1 and 2 empty
- **Tier 4 (Score < 5)**: Don't do — actively resist. Override requires documented reason + 1-week review checkpoint

### Phase Fields

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `id` | string | Yes | Unique identifier: `phase_` + UUID v4 |
| `name` | string | Yes | Phase title (e.g., "Research", "Execution") |
| `description` | string | No | What happens in this phase |
| `duration_days` | integer | Yes | Estimated duration in days |
| `target_start` | ISO 8601 date | Yes | When phase should begin |
| `target_end` | ISO 8601 date | Yes | When phase should conclude |
| `status` | enum | Yes | Pending, Active, Complete, Blocked |
| `tasks` | array | Yes | List of task objects (see below) |

### Task Fields

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `id` | string | Yes | Unique identifier: `task_` + UUID v4 |
| `name` | string | Yes | Task title |
| `description` | string | No | Details, context, acceptance criteria |
| `autonomous` | boolean or string | Yes | true (fully auto), "semi" (needs validation), false (manual) |
| `scheduled_for` | ISO 8601 timestamp | No | When task is/was scheduled to execute |
| `estimated_hours` | number | Yes | Estimated effort in hours (can be decimal) |
| `actual_hours` | number | No | Actual time spent (set when task complete) |
| `status` | enum | Yes | Pending, In Progress, Complete, Blocked, Deferred |
| `assignee` | string | No | User ID or "me" |
| `dependencies` | array | No | List of task IDs that must complete first |
| `output_artifact` | string | No | Path, URL, or reference to deliverable |

### Execution Fields (used by /execute, /done, /update, /review)

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `pct` | integer | Yes | Completion percentage (0-100). Auto-calculated: `round(100 * done_count / total_days)` |
| `lastUpdated` | ISO 8601 | Yes | Updated whenever project changes |
| `dayPlan` | array | Yes | Flattened day-by-day execution plan generated from phases. This is the primary structure used by all execution skills. |
| `executionLog` | array | No | Append-only log of completed work. Each entry records what was done, output files, and goal progress. |

**dayPlan entry fields:**

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `day` | integer | Yes | 1-indexed day number |
| `date` | string | Yes | YYYY-MM-DD scheduled date |
| `task` | string | Yes | What to do this day |
| `deliverable` | string | Yes | Expected output file or artifact name |
| `autonomous` | boolean/string | Yes | `true` (fully auto), `"semi"` (needs validation), `false` (manual) |
| `done` | boolean | Yes | `false` until completed. Set to `true` by /execute or /done. |
| `phase` | string | No | Phase name this day belongs to (for grouping) |

**Important**: The `dayPlan` array is the execution-facing structure. It is generated from `phases[].tasks[]` during project creation (Step 4 of /new-project). All execution skills (/execute, /done, /update, /review) read and write `dayPlan`, not `phases` directly.

### Metadata Fields

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `notes` | string | No | Freeform notes about the project |
| `tags` | array | No | Array of string tags for filtering |
| `memory_entities` | array | No | Entity IDs in basic-memory or memory graph |
| `scheduled_task_ids` | array | No | IDs of scheduled tasks created for this project |

---

## Status Values

### Project Status
- **Active**: Project is in progress
- **Paused**: Project is on hold (temporary break)
- **Done**: Project is completed
- **Blocked**: Project is blocked by external dependency

### Phase Status
- **Pending**: Not yet started
- **Active**: Currently in progress
- **Complete**: All tasks finished
- **Blocked**: Blocked by dependency or external event

### Task Status
- **Pending**: Not yet started
- **In Progress**: Currently being worked on
- **Complete**: Finished (autonomous or manual completion)
- **Blocked**: Blocked by dependency
- **Deferred**: Postponed to future date

---

## Color Palette

| Hex Code | Name | Use Case |
|----------|------|----------|
| `#1251C0` | Blue | Default, professional/work |
| `#E8533C` | Red | High priority, urgent |
| `#10B981` | Green | Creative, growth, learning |
| `#F5A623` | Orange | Personal projects, exploration |
| `#7C3AED` | Purple | Strategic, long-term |
| `#0891B2` | Cyan | Innovation, technical |
| `#D97706` | Amber | Content, writing, communication |
| `#BE185D` | Pink | Relationship, personal development |

---

## Examples

### Minimal Project
```json
{
  "id": "proj_a1b2c3d4",
  "name": "Read Design of Everyday Things",
  "goal": "Understand UX design principles by reading and reflecting on Don Norman's insights",
  "domain": "Learning",
  "priority": "Medium",
  "status": "Active",
  "created_at": "2026-03-29T10:00:00Z",
  "updated_at": "2026-03-29T10:00:00Z",
  "target_completion": "2026-04-30",
  "color": "#10B981",
  "template_used": "custom",
  "owner": "me",
  "decision_filters": {
    "revenue_impact": 0,
    "category_ownership": 1,
    "leverage": 2,
    "automation_roi": 0,
    "energize": 3,
    "weighted_score": 6.5,
    "tier": 3
  },
  "phases": [
    {
      "id": "phase_xyz123",
      "name": "Reading",
      "duration_days": 30,
      "target_start": "2026-03-29",
      "target_end": "2026-04-29",
      "status": "Active",
      "tasks": [
        {
          "id": "task_read_ch1",
          "name": "Read Chapter 1",
          "autonomous": false,
          "estimated_hours": 1.5,
          "status": "Pending"
        }
      ]
    }
  ]
}
```

### Complex Multi-Phase Project
```json
{
  "id": "proj_b2c3d4e5",
  "name": "Q2 Content Calendar",
  "goal": "Publish 12 high-quality blog posts and 24 social media updates across Q2 2026",
  "domain": "Work",
  "priority": "High",
  "status": "Active",
  "created_at": "2026-03-29T14:30:00Z",
  "updated_at": "2026-03-29T14:30:00Z",
  "target_completion": "2026-06-30",
  "color": "#1251C0",
  "template_used": "Content Calendar",
  "owner": "me",
  "stakeholders": ["editor@company.com", "designer@company.com"],
  "decision_filters": {
    "revenue_impact": 2,
    "category_ownership": 3,
    "leverage": 3,
    "automation_roi": 3,
    "energize": 2,
    "weighted_score": 18.5,
    "tier": 1
  },
  "phases": [
    {
      "id": "phase_planning",
      "name": "Planning",
      "duration_days": 5,
      "target_start": "2026-03-29",
      "target_end": "2026-04-02",
      "status": "Active",
      "tasks": [
        {
          "id": "task_research",
          "name": "Research audience topics",
          "autonomous": "semi",
          "estimated_hours": 4,
          "status": "Pending"
        },
        {
          "id": "task_editorial",
          "name": "Create editorial calendar",
          "autonomous": false,
          "estimated_hours": 3,
          "dependencies": ["task_research"],
          "status": "Pending"
        }
      ]
    },
    {
      "id": "phase_week1",
      "name": "Week 1 Publishing",
      "duration_days": 7,
      "target_start": "2026-04-06",
      "target_end": "2026-04-12",
      "status": "Pending",
      "tasks": [
        {
          "id": "task_write_post_1",
          "name": "Write blog post 1",
          "autonomous": false,
          "estimated_hours": 3,
          "status": "Pending"
        }
      ]
    }
  ],
  "tags": ["content", "marketing", "q2-2026"],
  "memory_entities": ["entity_content_calendar_q2"],
  "scheduled_task_ids": ["scheduled_research_topics", "scheduled_write_post_1"]
}
```

---

## Usage Notes

- **UUIDs**: Generate with `uuid.v4()` or equivalent
- **ISO 8601**: Use format `YYYY-MM-DDTHH:MM:SSZ` for timestamps, `YYYY-MM-DD` for dates
- **Validation**: Check that `target_end >= target_start` for all phases and tasks
- **Versioning**: Increment `updated_at` whenever the project is modified
- **Archiving**: When projects reach "Done" status, consider archiving to a separate `completed_projects.json` file for performance
