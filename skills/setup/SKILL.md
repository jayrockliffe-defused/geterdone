---
name: setup
description: >
  Use this skill when the user says "/setup", "configure geterdone", "set up
  my projects", "what tools do I have", or when first installing Geterdone.
  Self-configuring onboarding that scans connected tools, searches the MCP
  registry, recommends connectors, and generates a personalized config file.
version: 1.0.0
---

# /setup — Geterdone Command Center Setup

Self-configuring onboarding that adapts to any workflow. Scans your connected tools, searches the MCP registry for useful additions, and generates a personalized config.

See `references/connector-recommendations.md` for connector categories and recommendations by workflow type.

---

## Step 1 — Ask workflow type

Ask the user one question:
"What kind of work do you primarily do?"
Options: coding, content creation, sales/outreach, analytics, design, team management, or a mix.

This determines which connector categories to prioritize.

---

## Step 2 — Scan connected tools

Silently inventory what the user already has:

1. Call `list_scheduled_tasks` — check for existing automations
2. Check for MCP tools by probing known tool prefixes:
   - Gmail: `gmail_search_messages`
   - Google Calendar: `gcal_list_calendars`
   - Notion: `notion-search`
   - Slack: check for slack tools
   - Sentry: `search_issues`
   - Figma: `get_metadata`
   - Canva: `search-designs`
   - Apollo: `find-and-enrich-company`
   - Cloudflare: `accounts_list`
   - Supabase: `list_projects`
   - Frase: `list_pages`
   - Apple Notes: `list_notes`
   - Basic Memory: `search`
   - Memory Graph: `search_nodes`
   - Ahrefs/SEO tools: `gsc-keywords`
   - Brand Radar: `brand-radar-mentions-overview`

Build a list: `connectedTools[]` and `missingTools[]`.

---

## Step 3 — Search MCP registry globally

Based on workflow type, search for connectors the user doesn't have:

```
search_mcp_registry(query: "[workflow-relevant terms]")
```

Search terms by workflow:
- **Coding**: "github", "sentry", "supabase", "cloudflare", "linear"
- **Content**: "notion", "frase", "wordpress", "substack", "google search console"
- **Sales**: "apollo", "hubspot", "salesforce", "outreach", "gmail"
- **Analytics**: "posthog", "google analytics", "mixpanel", "amplitude"
- **Design**: "figma", "canva", "adobe"
- **All workflows**: "slack", "calendar", "notion", "memory"

Read `references/connector-recommendations.md` for the full recommendation matrix.

---

## Step 4 — Present recommendations

Show the user what they have and what they're missing:

```
Connected tools: [N]
  [check] [Tool name] — [what it does for your workflow]
  ...

Recommended additions: [top 3-5 based on workflow]
  [arrow] [Tool name] — [why it matters for your work]
       Install: [suggest_connectors call or install link]
  ...

Optional: [tools that are useful but not critical]
  [dot] [Tool name] — [brief description]
```

Use `suggest_connectors` to offer one-click install for recommended tools.

---

## Step 5 — Configure preferences

Ask the user:
1. **Dispatch channel**: Where should summaries be sent? (Apple Notes / Slack / Gmail / Notion / None)
2. **Review cadence**: How often do you want progress reviews? (Daily / Weekly / Biweekly / Monthly)
3. **Auto-execute time**: When should autonomous tasks run? (Default: 9:00 AM)
4. **Morning brief**: Want a daily summary of overnight activity? (Yes / No)

---

## Step 6 — Generate config file

```bash
SESSION=$(ls /sessions/ | grep -v lost 2>/dev/null | head -1)
DATA_DIR="/sessions/$SESSION/mnt/claude-cowork/Scheduled"
CONFIG="$DATA_DIR/geterdone-config.json"
```

Read `references/config-schema.md` for the full schema.

Write `geterdone-config.json` with:
- User profile (workflow type, preferences)
- Connected tools inventory
- Dispatch channel setting
- Review cadence
- Auto-execute schedule
- Morning brief toggle
- Version and timestamp

If config already exists, merge new settings (don't overwrite existing project data).

---

## Step 7 — Set up scheduled tasks

If morning brief enabled:
- Create scheduled task: "Geterdone Morning Brief" at configured time
- Prompt: "Run /review for all active projects, dispatch summary"

If auto-execute enabled:
- Create scheduled task: "Geterdone Auto-Execute" at configured time
- Prompt: "Run /execute for all active projects with autonomous tasks due today"

---

## Step 8 — Welcome summary

```
Geterdone configured!

Workflow: [type]
Connected: [N] tools
Dispatch: [channel]
Review cadence: [setting]
Auto-execute: [time or disabled]
Morning brief: [enabled/disabled]

Config saved to: geterdone-config.json

Next steps:
1. Run /new-project to create your first project
2. [If missing tools]: Install [recommended tool] for [specific benefit]
3. Run /status anytime to see your dashboard
```

---

## Notes

- Setup can be re-run anytime — it preserves existing data
- Tool scanning is silent — don't ask permission for each probe
- MCP registry search discovers tools globally, not just what's installed
- Always use suggest_connectors for install recommendations
- Config file location uses dynamic session paths, never hardcoded
