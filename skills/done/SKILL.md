---
name: done
description: >
  Use this skill when a user manually completes a semi-auto or manual task and wants
  to mark it done. This is the manual "I finished the work" endpoint. Claude reads
  the project, finds the task, verifies completion, updates memory, and marks it done.
version: 1.0.0
---

## Step 1 — Locate the Project and Task

```bash
SESSION=$(ls /sessions/ | grep -v lost 2>/dev/null | head -1)
DATA_DIR="/sessions/$SESSION/mnt/claude-cowork/Scheduled"
CONFIG="$DATA_DIR/geterdone-config.json"
DATA_FILE="$DATA_DIR/geterdone-projects.json"
TODAY=$(date +%Y-%m-%d)
```

Read `$DATA_FILE`. Find the project (user names it or it's inferred from context).
Within that project, find today's dayPlan entry where `done == false`.
Verify it exists. If multiple entries for today, ask user which one.

## Step 2 — Verify Task is Complete

Ask user: "I found [Task Name] for today. Have you completed the deliverable?"

If user confirms:
- Ask for the **output file path** or **deliverable location** (link, file, Notion page, etc.).
- Verify the deliverable exists and is accessible.
- If it's a file, confirm it's non-empty.
- If it's a web link (Notion, Google Doc, blog post), verify it's published and readable.

If user is unsure or hasn't finished, do NOT mark done. Offer to help complete the task instead.

## Step 3 — Quick QA

Run a lightweight QA check (not the full gate):

- **Content deliverables**: Check word count is reasonable (e.g., >500 words for blog), no obvious placeholders.
- **Code deliverables**: Ask user if tests pass and code runs. If they say yes, trust them.
- **Web deliverables**: Ask user if the page loads and looks right. Ask them to paste a screenshot if unsure.
- **Integration deliverables** (Notion, Slack, etc.): Ask user if the item was created/published in the target system.

If QA check fails, offer to help remediate before marking done.

## Step 4 — Update Memory

Same as the execute skill:

1. **Backup JSON** first.
2. **Write to basic-memory**: Project name, day completed, deliverable summary, goal progress.
3. **Update memory graph**: Link deliverable to project.
4. **Verify Git/Notion/etc** were updated (if applicable).

See `execute/references/memory-protocol.md` for detailed steps.

## Step 5 — Update Data File

Mark the dayPlan entry `done: true`.
Append execution log entry:

```json
{
  "date": "[today's date]",
  "day": [N],
  "status": "completed",
  "task": "[task description]",
  "outputFile": "[deliverable location]",
  "goalProgress": "[percentage or status]",
  "adjustments": "[if any]",
  "triggeredBy": "manual",
  "completedAt": "[ISO timestamp]"
}
```

Recalculate project `pct`. Set `lastUpdated`.
Write JSON back. Verify with `jq`.

## Step 6 — Dispatch Success

If config specifies a dispatch channel:

- **Apple Notes**: Add note with completion summary + next task suggestion.
- **Slack**: Send message to configured channel: "[Project] Day [N] complete. Deliverable: [link/file]."

## Step 7 — Summary Card

```
[check] [Project Name] — Day N of M complete [arrow manual]
Task: [what was done]
Output: [link or file path]
Goal progress: [one sentence]
Next: [what's planned for day N+1, if known]
```

---

**Notes:**
- Use this skill for manual completions only. For autonomous tasks, the execute skill handles the full flow.
- If the user says they haven't finished, offer to help. Don't mark tasks done prematurely.
- Memory update is mandatory. See memory-protocol.md for details.
- If task extended past the original plan, note it and trigger goal recovery run if needed (see execute/references/qa-gates.md).
