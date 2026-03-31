# Memory Update Protocol — Mandatory Step

Failure to update memory = task is not complete. This is non-negotiable.

## Step 1 — Backup JSON First

Always backup before any write:

```bash
cp "$DATA_FILE" "$DATA_FILE.backup.$(date +%s)"
```

If anything goes wrong, restore from backup.

## Step 2 — Write to basic-memory

Use the memory system to record:

```json
{
  "entities": [
    {
      "name": "[Project Name]",
      "entityType": "Project",
      "observations": [
        "Day [N] of [M] completed on [date]",
        "Deliverable: [brief description]",
        "Goal progress: [percentage or status]",
        "Output file: geterdone/[project-id]/day-[N]-[slug].md",
        "Next task: [what's planned for day N+1, if known]"
      ]
    },
    {
      "name": "[Deliverable Name]",
      "entityType": "Deliverable",
      "observations": [
        "Type: [blog post, code, research, etc.]",
        "Purpose: [one sentence on why this was created]",
        "Project: [project name]",
        "Completed: [date]",
        "Key findings: [2-3 bullet points if applicable]",
        "Reusable for: [other projects, if any]"
      ]
    }
  ]
}
```

Use the `add_observations` call if updating an existing entity.

## Step 3 — Update Memory Graph

Create relations linking the deliverable to the project:

```json
{
  "relations": [
    {
      "from": "[Project Name]",
      "to": "[Deliverable Name]",
      "relationType": "produced"
    },
    {
      "from": "[Deliverable Name]",
      "to": "[Project Name]",
      "relationType": "for"
    }
  ]
}
```

If task involved collaboration or dependencies, link to those entities too:

```json
{
  "from": "[Deliverable Name]",
  "to": "[Related Project or Person]",
  "relationType": "leverages" or "depends_on" or "unblocks"
}
```

## Step 4 — Verify Git Sync (if applicable)

If project has a Git repo:

1. Check if CLAUDE.md exists in the repo root.
2. If not, create it with a link to the Geterdone project page.
3. For code changes, verify the commit message includes project/day reference: `"[Project] Day N: [task summary]"`
4. For doc changes, verify README.md or relevant docs were updated to reflect new content.
5. Confirm no duplicate files exist (e.g., two versions of the same asset in different folders).
6. If deploying, verify the remote repo shows the new commit and CI/CD pipeline triggered.

## Step 5 — Notion Sync (if connected)

If Notion is integrated:

1. Locate the project page in Notion (create if missing).
2. Add or update an "Execution Log" section with:
   - Date: [completion date]
   - Day: [N of M]
   - Task: [what was done]
   - Output: [link to geterdone/<project-id>/day-[N]-[slug].md]
   - Goal progress: [current % or status]
   - Suggestions: [from post-exec intelligence report]

3. Verify the page is readable (test the link from a fresh session).

## Step 6 — JSON Data File Update

After memory is updated, update the main data file:

```bash
# Update the dayPlan entry for today
# Set: done = true
# Append to executionLog array:
{
  "date": "[today's date]",
  "day": [N],
  "status": "completed",
  "task": "[task description]",
  "outputFile": "geterdone/[project-id]/day-[N]-[slug].md",
  "goalProgress": "[percentage or status]",
  "adjustments": "[if plan was modified, describe it]",
  "suggestions": "[from post-exec intelligence]",
  "triggeredBy": "manual" or "schedule",
  "completedAt": "[ISO timestamp]"
}

# Recalculate project.pct:
# pct = (completed days / total planned days) * 100
# If all tasks done and goal met: status = "Done", pct = 100

# Update lastUpdated: "[ISO timestamp]"
```

## Step 7 — Verification

Verify all updates succeeded:

```bash
# Check memory was written
memory search: "[Project Name]"

# Check JSON is valid
jq . "$DATA_FILE"  # Must exit 0

# Check Notion (if connected)
# Open Notion and verify page was updated

# Check Git (if connected)
# Run: git log --oneline -5
# Verify latest commit mentions the project
```

If any step fails, do NOT proceed. Fix the error, redo the step, and verify again.

## Recovery

If JSON is corrupted during write:

1. Restore from backup: `cp "$DATA_FILE.backup.*" "$DATA_FILE"`
2. Re-run the JSON update carefully
3. Verify with `jq . "$DATA_FILE"`

If memory write fails:

1. Check network connection (if using remote memory store)
2. Verify entity name and relation types are spelled correctly
3. Retry the write

If Notion sync fails:

1. Verify Notion token is valid (check config)
2. Verify page exists or create it
3. Retry the sync

Do NOT mark the task as complete until memory is fully updated and verified.
