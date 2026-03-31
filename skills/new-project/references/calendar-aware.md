# Calendar-Aware Scheduling Reference

The calendar-aware scheduling step (Step 5 of `/new-project`) ensures that project tasks are scheduled in alignment with the user's existing calendar commitments, avoiding overload and maximizing the likelihood of completion.

---

## Core Workflow

### 1. Fetch User's Calendar

Before scheduling any tasks, retrieve the user's calendar availability:

```
Tool: gcal_find_my_free_time
Parameters:
  - calendarIds: ["primary"] (or user-specified calendars)
  - timeMin: project_start_date (ISO 8601)
  - timeMax: project_target_completion (ISO 8601)
  - minDuration: average_task_duration (in minutes)
  - timeZone: user_timezone (e.g., "America/Los_Angeles")
```

This returns a list of free time blocks where tasks can be scheduled.

### 2. Analyze Calendar Patterns

Extract patterns from the calendar:
- **Available hours per day**: How many free hours exist on average?
- **Meeting clusters**: Are there particular days/times heavily booked?
- **Focus time blocks**: Are there recurring uninterrupted blocks (e.g., mornings)?
- **Commute/travel**: Identify days/periods with heavy travel
- **Holidays/PTO**: Flag days the user is unavailable

**Example**:
```
Analysis Results:
├─ Monday–Wednesday: 6–8 free hours/day (optimal for deep work)
├─ Thursday: 2–3 hours (meeting-heavy day)
├─ Friday: 4–5 hours (afternoon free)
├─ Vacation scheduled: April 15–20 (project timeline pause)
└─ Recommendation: Schedule longer tasks Mon–Wed, short tasks Thu–Fri
```

### 3. Match Tasks to Calendar Slots

For each task in the execution plan:
1. Get task duration (from `estimated_hours`)
2. Find a calendar slot that:
   - Is available for the full duration
   - Matches the task type (deep work for writing, admin time for emails, etc.)
   - Doesn't create back-to-back overload with other tasks
   - Is within the task's target phase dates

### 4. Schedule with Buffers

Apply scheduling buffers to prevent overload:
- **Between high-effort tasks**: +30 minutes for context switching
- **After meetings**: +15 minutes for mental reset
- **Daily cap**: Don't schedule more than 60% of available hours (leave 40% buffer for context switching, breaks, and unexpected work)
- **Weekly cap**: Cap at 15 hours of project work (Jay works half-time, 15–25 hrs/week — schedule conservatively at the low end)

**Example**:
```
Day: Monday 2026-03-30
├─ 9:00–11:30 AM: Task A: Research (2.5h, deep work)
├─ 11:30 AM–12:00 PM: Buffer (context switch)
├─ 12:00–1:00 PM: Lunch
├─ 1:00–3:00 PM: Existing meeting
├─ 3:00–3:30 PM: Buffer (mental reset)
├─ 3:30–5:00 PM: Task B: Draft outline (1.5h, structured work)
├─ Available next: Tuesday 9:00 AM (daily cap: 4h of 5.5h available = 73%)
└─ Status: Optimal packing without overload
```

### 5. Surface Conflicts & Propose Resolutions

If calendar doesn't have enough free time, flag to the user:
- **Conflict**: Task needs 8 hours but only 5 hours free in target window
- **Options**:
  1. Compress task (reduce scope or increase intensity)
  2. Extend timeline (push completion date)
  3. Delegate or defer task
  4. Parallel phases (some tasks can run simultaneously if independent)

---

## Calendar Rules

### Deep Work Tasks
**Definition**: Tasks requiring focus and creative energy (writing, designing, coding, research)

**Calendar preference**:
- Schedule during morning or early afternoon (peak energy)
- Avoid after 5 PM (diminishing returns)
- Require 2+ hour uninterrupted blocks
- Bad: fragmented 30-minute slots

**Example**:
- ✓ Monday 9–11 AM: Write blog post
- ✗ Thursday 3:30–4:00 PM: Write blog post (too short, too late)

### Structured Work Tasks
**Definition**: Tasks with clear processes and checklists (email, admin, reviews, QA)

**Calendar preference**:
- Can be scheduled in 30–90 minute blocks
- Flexible on time of day (not energy-dependent)
- Batch similar tasks (e.g., all emails together)
- Can go after meetings (transitions well)

**Example**:
- ✓ Thursday 1–2 PM: Review pull requests (after meetings)
- ✓ Friday 2–3 PM: Process customer feedback

### Manual Collaboration Tasks
**Definition**: Tasks involving other people (meetings, reviews, presentations)

**Calendar rules**:
- Must be explicitly scheduled with all participants
- Account for timezone differences if remote
- Schedule at least 24 hours in advance (courtesy + preparation time)
- Avoid scheduling before deep work (context switching cost)

**Example**:
- ✓ Tuesday 10 AM: Stakeholder review meeting (scheduled 3 days prior)
- ✓ Friday 4 PM: 1:1 with team member

### Autonomous Tasks
**Definition**: Scripts, automated workflows, data collection (no human presence required)

**Calendar rules**:
- Can be scheduled for any time (night, early morning)
- Schedule during low-traffic windows if possible (reduces load on systems)
- Can run in parallel with other tasks
- Doesn't consume user's calendar time directly

**Example**:
- ✓ 11 PM: Automated data backup
- ✓ 2 AM: Scheduled report generation

---

## Time Zone Considerations

### For Single Time Zone
```
Preference: Morning > Afternoon > Evening
Example (US Pacific):
├─ 6–9 AM: Peak focus time (not calendar-blocked)
├─ 9 AM–12 PM: Good focus time (some meetings)
├─ 12–1 PM: Lunch break
├─ 1–4 PM: Post-lunch focus time
├─ 4–6 PM: Administrative/lighter work
└─ 6+ PM: Avoid scheduling (diminishing returns)
```

### For Multiple Time Zones
```
1. Map user's primary time zone
2. For each collaborative task:
   - Find overlapping hours with all participants
   - Schedule at the boundary that minimizes disruption for the user
   - Document timezone offsets in task notes

Example:
User (Pacific): 9 AM = UTC 5 PM
Collaborator (London): 5 PM = UTC