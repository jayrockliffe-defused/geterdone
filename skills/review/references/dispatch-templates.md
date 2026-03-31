# Dispatch Templates

Reference file for review and intel skills. Templates for dispatching summaries
to mobile via Apple Notes, Slack, Gmail, or Notion.

## Apple Notes Format

Keep it scannable on a phone screen. No HTML — plain text with emoji indicators.

### Review dispatch
```
Geterdone Review — [Date]

Projects: [N] active, [N] on track, [N] at risk

[For each project:]
[emoji] [Name] — [pct]%
  [One-line status]

Top action: [Most important thing to do]

Next review: [date]
```

### Intel dispatch
```
Geterdone Intel — [Date]

[fire] [Top finding — one sentence]

Actions:
1. [First action]
2. [Second action]

Full brief in Cowork.
```

### Execution dispatch
```
[check] [Project] — Day [N]/[M] done

Task: [what was completed]
Output: [deliverable name]
Next: [tomorrow's task]

[If suggestions]: Suggestion: [one-liner]
```

## Slack Format

Use Slack markdown. Keep messages under 300 words.

### Review dispatch
```
*Geterdone Review — [Date]*

*[N] projects active* | *[N] on track* | *[N] at risk*

[For each project:]
>[emoji] *[Name]* — [pct]% complete
>[One-line status]

*Top action:* [Most important thing]
```

### Intel dispatch
```
*Intelligence Brief*

:fire: *[Top finding]*
[One paragraph of context]

*Actions:*
1. [First]
2. [Second]
```

## Gmail Format

Subject line: "Geterdone [Review|Intel|Execution] — [Date]"
Body: Plain text, same structure as Apple Notes format.
Draft only — never send automatically.

## Notion Format

Create a page in the configured Notion database.
Title: "Geterdone [Type] — [Date]"
Properties: date, type (review/intel/execution), projects referenced.
Body: Full formatted review with headings.

## Dispatch Channel Selection

Read `geterdone-config.json` → `dispatchChannel` field.

| Value | Tool | Method |
|-------|------|--------|
| `"apple-notes"` | Read and Write Apple Notes | `add_note` |
| `"slack"` | Slack MCP | `slack_send_message` |
| `"gmail"` | Gmail MCP | `gmail_create_draft` |
| `"notion"` | Notion MCP | `notion-create-pages` |
| `"none"` | No dispatch | Display in chat only |
| Not set | Default to `"apple-notes"` if available, else `"none"` |

## Tone Rules

- Start with the answer, not context
- One emoji per project max
- No filler words
- Specific numbers, not vague assessments
- End with a clear action, not a question
- Keep mobile dispatches under 200 words
