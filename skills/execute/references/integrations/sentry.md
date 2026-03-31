# Sentry Integration — Execute Skill

Loaded when a task involves code deployment, error monitoring, or debugging.

## When to load this file
- Task involves deploying code or checking deployment health
- Project is a software/code project
- Deliverable is a bug fix, feature, or infrastructure change

## Pre-execution check
1. `search_issues` — check for unresolved errors in the project
2. `find_projects` — verify Sentry project exists and is active
3. `find_releases` — check latest release status

## During execution
- Reference Sentry issue IDs in commit messages when fixing bugs
- Check error rates before and after changes
- Monitor for new error patterns introduced by the change

## Post-execution check
1. `search_issues` — any new errors since deployment?
2. `search_events` — check event volume (spike = problem)
3. `analyze_issue_with_seer` — use AI analysis for complex errors
4. Compare error rate: if higher than pre-deploy, flag immediately

## Error response protocol
If new errors detected post-deploy:
1. Log the errors in execution record
2. Assess severity: crash-level vs warning-level
3. If crash-level: flag as Blocked, notify user via dispatch
4. If warning-level: note in execution log, monitor next execution
5. Never mark task as "done" if new crash-level errors exist

## Tool reference
Sentry tools: `search_issues`, `search_events`, `find_projects`,
`find_releases`, `analyze_issue_with_seer`, `get_issue_tag_values`
