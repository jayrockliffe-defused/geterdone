---
name: update
description: >
  Use this skill when the user says "/update", "update project X", "pause X",
  "resume X", "X is blocked", "change the goal of X", "add a day to X",
  "adjust the plan for X", "mark X done", or wants to manually override
  anything in a Geterdone project. This is the manual override hatch.
version: 1.0.0
---

# /update — Manual Project Override

Manual override for any Geterdone project. Also runs two automatic syncs
on every call before applying any manual change.

Read `../new-project/references/project-schema.md` for data file path and schema.

---

## Step 0 — Automatic syncs (always run first)

Run both syncs silently before touching anything the user asked to change.

### 0a — Automation status sync

Call `list_scheduled_tasks` to get live state for all Geterdone schedules.

For each task, determine today-status:

| Condition | Status |
|-----------|--------|
| lastRunAt is today, clean completion | ran_clean |
| lastRunAt is today, partial/cutoff | partial |
| lastRunAt is today, rate limit hit | rate_limited |
| No cronExpression (manual-only) | manual |
| nextRunAt is today but hasn't run | upcoming |
| nextRunAt is future date | upcoming |

If `list_scheduled_tasks` unavailable, skip 0a and note it in confirm block.

### 0b — File-based progress sync

For each Active project in `geterdone-projects.json`:

1. Determine project output dir: `$DATA_DIR/geterdone/<project-id>/`
2. List all files present in that directory
3. For each dayPlan entry where `done == false`:
   - Parse filename(s) from the `deliverable` field
   - If all primary deliverable files exist on disk → set `done: true`
4. Recalculate `pct`:
   ```
   pct = round(100 * done_count / max(total_days, 1))
   ```
5. If pct == 100 and project was Active → set status = "Done"

File scan is non-destructive: only flips done from false → true, never reverse.

Write the synced JSON back after both 0a and 0b complete.

---

## Step 1 — Identify the project

If the user named a project or requested a change, fuzzy-match to the project.
"essay" matches "My Scholarship Essay". If ambiguous, show a short list.

If the user said "all" or only asked for a status update with no specific
project change, skip to Step 4.

---

## Step 2 — Determine what to update

| What they say | What to change |
|---------------|----------------|
| "pause X" | status = "Paused" |
| "resume X" | status = "Active" |
| "X is blocked" / "stuck on X" | status = "Blocked", write blocker to notes |
| "X is done" / "goal reached" | status = "Done", pct = 100, mark all dayPlan done |
| "change the goal to..." | Update goal field, ask if plan needs adjusting |
| "add N more days" | Append N new dayPlan entries, classify each autonomous/semi/manual |
| "change today's task to..." | Update the matching dayPlan entry task field |
| "skip day N" | Mark dayPlan entry done: true, log "Skipped by user" |
| "add a note: ..." | Append to notes |
| "make day N autonomous" | Set dayPlan[N-1].autonomous = true |
| "make day N semi-auto" | Set dayPlan[N-1].autonomous = "semi" |
| "make day N manual" | Set dayPlan[N-1].autonomous = false |
| Priority / deadline changes | Update the relevant field directly |

Multiple updates in one message: apply all in a single write.

---

## Step 3 — Write back

Apply manual changes on top of the auto-sync results. Set `lastUpdated` to today.
Write the full JSON back in a single write.

---

## Step 4 — Memory update (mandatory)

Write update event to basic-memory:
- What changed and why
- Link to project entity
- Note if user overrode decision filters or plan

Update memory graph with change relations.

---

## Step 5 — Confirm

```
Auto-synced:
  Automations: [N] statuses refreshed
  Progress: [list any days newly marked done via file scan]
             or "No new completions detected"

[Project Name] updated        (only if manual change was applied)
  [one line per change]
```

Then: `Run /status to see your full dashboard.`

---

## Notes

- Never delete projects. To remove: set status = "Archived"
- Never delete dayPlan entries or executions — they're the project's history
- If the goal changes significantly, offer to regenerate remaining dayPlan entries
- When adding new days, always classify each as autonomous/semi/manual
- Step 0 syncs are silent and automatic — never ask for confirmation
- File scan (Step 0b) is non-destructive: only flips done false → true
- For deliverables listed as "X + Y" (multiple files), all must exist to mark done
- Memory update is mandatory after every update
