# Session Analysis & Cross-Project Patterns

Reference file for the /review skill. Provides frameworks for analyzing
work patterns across multiple projects and sessions.

## Session Productivity Metrics

### Time-to-completion
Track how long tasks actually take vs estimates:
```
accuracy = estimated_hours / actual_hours
```
- accuracy > 1.2 → consistently overestimating (good, leaves buffer)
- accuracy 0.8-1.2 → accurate estimation
- accuracy < 0.8 → underestimating (risk of missed deadlines)

### Execution density
How much real output per session:
```
density = deliverables_produced / sessions_in_period
```
- density > 1.5 → highly productive sessions
- density 0.8-1.5 → normal
- density < 0.8 → sessions are fragmented or interrupted

### Autonomous success rate
Percentage of autonomous tasks that complete without intervention:
```
success_rate = auto_completed_clean / auto_attempted
```
- > 90% → automation working well
- 70-90% → some tasks need better prompts or smaller scope
- < 70% → autonomous classification too aggressive

## Cross-Project Pattern Detection

### Reusable asset detection
Search execution outputs for:
- Research documents that cover topics relevant to multiple projects
- Code snippets or scripts used in similar ways
- Templates (email, content, report) that appear across projects
- Design assets (color palettes, layouts) shared between projects

Flag: "Asset [X] from [Project A] could save [N] hours in [Project B]"

### Duplicated effort detection
Compare dayPlan tasks across active projects:
- Similar research topics being investigated separately
- Identical outreach templates being drafted independently
- Same stakeholders being contacted by different projects

Flag: "Tasks [X] in [Project A] and [Y] in [Project B] overlap — consolidate?"

### Workload distribution
Analyze task distribution across the week:
- Are manual tasks clustered on specific days?
- Is there a pattern of "Monday planning, Friday execution"?
- Are weekends being used? (flag for burnout risk)

### Project lifecycle patterns
Track how projects typically progress:
- Average days from Active → Done
- Common phase where projects stall
- Percentage of projects that hit their original deadline
- Most common reason for deadline extension

## Proactive Suggestions Framework

Generate suggestions in priority order:

1. **Blockers** — anything preventing progress on any project
2. **Quick wins** — tasks that can be completed in < 30 minutes
3. **Leverage plays** — cross-project efficiencies
4. **Risk mitigation** — upcoming deadlines with insufficient progress
5. **Automation opportunities** — recurring manual patterns
6. **Skill creation** — patterns that should become reusable skills

### Suggestion format
```
[Priority] [Category]: [Specific action]
  Why: [One sentence justification]
  Impact: [Estimated time saved or risk reduced]
  Effort: [Time to implement]
```

## Memory Integration

After every review, write to basic-memory:
- Review date and scope
- Key findings and recommendations
- Action items generated
- Trend data (velocity, completion rates)

This creates a longitudinal record for detecting long-term patterns
that single-session analysis would miss.
