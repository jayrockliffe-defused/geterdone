# Performance Pattern Detection

Reference file for the /review skill. Loaded on demand when generating reviews.

## Velocity Patterns

### Healthy velocity
- Completion rate 80-120% of planned tasks
- Consistent execution across review period (no gaps > 2 days)
- Autonomous tasks completing on schedule
- Manual tasks completing within 1 day of planned date

### Declining velocity
- Completion rate dropping period-over-period
- Increasing gap between planned and actual completion
- More tasks pushed to "next day" or rescheduled
- Autonomous tasks failing or producing low-quality output

### Sprint pattern (burst then stall)
- High completion in first 2-3 days, then nothing
- Common with creative/writing projects
- Intervention: break remaining work into smaller daily chunks

### Blocked pattern
- Tasks marked Blocked for > 48 hours
- Same blocker appearing across multiple tasks
- Intervention: escalate blocker, find workaround, or pause project

## Goal Proximity Assessment

Calculate expected progress vs actual:
```
expected_pct = (days_elapsed / total_duration) * 100
actual_pct = project.pct
gap = expected_pct - actual_pct
```

| Gap | Assessment |
|-----|------------|
| < 5% | On track |
| 5-15% | Slight delay — monitor |
| 15-30% | At risk — recommend plan adjustment |
| > 30% | Critical — trigger goal recovery discussion |

## Autonomy Analysis

Track ratio of autonomous vs manual task completions:
```
autonomy_ratio = autonomous_completed / total_completed
```

| Ratio | Interpretation |
|-------|---------------|
| > 0.7 | Strong automation — system working well |
| 0.4-0.7 | Balanced — normal for mixed projects |
| < 0.4 | User bottleneck — too many manual tasks |

If autonomy ratio is low, suggest:
- Converting eligible manual tasks to semi-auto
- Breaking manual tasks into autonomous prep + manual approval
- Scheduling dedicated "task completion" blocks on calendar

## Quality Signals

Check execution logs for:
- Tasks that needed re-execution (QA failures)
- Plan adjustments mid-project (scope creep or scope reduction)
- Goal recovery runs triggered (plan extensions)
- Memory update failures (data integrity risk)

## Cross-Period Trends

Compare current review period to previous:
- Is velocity improving, stable, or declining?
- Are the same projects consistently at risk?
- Is the user taking on too many parallel projects?
- Are autonomous tasks becoming more reliable over time?

Recommended max parallel active projects: 3-5 depending on complexity.
If > 5 active, suggest pausing lowest priority.
