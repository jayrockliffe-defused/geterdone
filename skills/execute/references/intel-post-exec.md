# Post-Execution Intelligence Protocol

After QA passes and before updating the data file, analyze what was produced and generate actionable proactive intelligence.

## Step 1 — Analyze the Deliverable

Read the output file and extract:
- **Key takeaways**: What was learned, built, or created?
- **Reusable assets**: Any code, templates, research, or content that could be leveraged elsewhere?
- **Duplicated effort**: Did this task overlap with prior work? If so, note it.
- **Gaps identified**: What's needed next? What's still missing?
- **Time investment vs. output**: Was the time-to-value ratio good? Note if task took longer than expected.

## Step 2 — Cross-Reference with Memory

Query basic-memory for related context:

```bash
memory search: "project-name" + "deliverable-type"
```

Look for:
- **Prior work on this project**: Check execution log for cumulative progress toward the goal.
- **Related projects**: Has another project tackled a similar problem? Suggest reuse.
- **Dependencies**: Did this task unblock anything? Note downstream implications.
- **Person mentions**: If the deliverable mentions a collaborator, cross-reference their involvement.

## Step 3 — Check Memory Graph for Leverage

Use memory graph queries to find:
- **Cross-project duplicates**: Did two projects produce similar content? Flag for consolidation.
- **Asset chains**: If Project A outputs something that Project B inputs, note the dependency.
- **Expertise centers**: If multiple projects needed the same skill, flag for training/documentation.

## Step 4 — Generate Proactive Suggestions

Based on analysis, suggest:

- **Next logical step**: What should happen after today's task? (e.g., "Consider publishing this blog post to LinkedIn.")
- **Quick wins**: Can related tasks be batched? (e.g., "Two other projects need code review — batch them.")
- **Risk flags**: Did task surface any blockers? (e.g., "API limit hit — may need pagination next sprint.")
- **Improvement opportunities**: Is the process inefficient? Suggest optimization. (e.g., "This research takes 3h per project — consider building a template.")
- **Leverage plays**: Did task produce an asset that could help another project? (e.g., "The authentication logic here could be reused in Project B.")

## Step 5 — Integration-Specific Intelligence

### If Notion Connected
- Check if project page exists in Notion. If not, suggest creating one.
- Verify execution log is synced to Notion.
- If task required Notion research, flag if any documents are outdated.

### If Slack Connected
- Suggest who to notify about this completion (based on project stakeholders in memory).
- If task involved team communication, suggest follow-up messages.

### If Git Connected
- Verify that CLAUDE.md or project README was updated with today's changes.
- If code was merged, suggest updating related documentation.
- Check for any orphaned branches or stale PRs.

### If Sentry Connected
- Any errors logged during task execution? Investigate and flag for remediation.
- Did task close any open error patterns? Note the fix in the execution log.

### If Apple Notes Connected
- Verify execution summary was added to Notes.
- Suggest related notes that might be helpful for next task.

## Step 6 — Compile Intelligence Report

Assemble findings into a brief report:

```
[Project Name] — Post-Execution Intelligence

Deliverable: [what was completed]
Reusable assets: [any for other projects?]
Duplicated effort: [if any]
Recommended next steps: [2-3 bullets]
Leverage opportunities: [cross-project, if any]
Risk flags: [blockers or issues, if any]
Integration updates: [Notion, Slack, Git, etc. status]
```

This report is included in the execution log and sent to mobile via dispatch channel.
