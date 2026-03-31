# Outreach Integration — Defusely Edition

Loaded when a task involves founding partner outreach, lead enrichment, or email campaigns.

## Defusely Outreach Context

**Goal**: Get 10 founding design partners from PR agencies (12 months free, feedback + case study in exchange)
**Target**: PR agencies, 10-100 employees, US/UK/Canada, crisis comms or reputation management
**Approach**: Invitation, not pitch — "building the first Reddit crisis response platform, hand-picking 10 agencies to shape it"
**Tools**: Apollo MCP + Gmail MCP + LinkedIn (manual)

## Apollo MCP — Lead Enrichment

### Finding target agencies
```
find-and-enrich-company:
  - Industry: Public Relations
  - Employee count: 10-100
  - Location: US, UK, Canada
  - Keywords: crisis communications, reputation management, PR crisis
```

### Finding decision-makers at target agencies
```
find-and-enrich-contacts-at-company:
  - Titles: Account Director, VP Communications, Crisis Practice Lead, Managing Director
  - Seniority: Director+
```

### Bulk prospecting
```
find-and-enrich-list-of-contacts:
  - Use ICP criteria above
  - Enrich with email + phone + LinkedIn URL
  - Deduplicate against existing contacts
```

## Gmail MCP — Outreach Sequences

### Initial outreach (Semi-auto: Geterdone drafts, Jay reviews)
- Subject: "Building something new for PR crisis teams — need your input"
- Body: Invitation framing, not sales pitch
- Include: What Defusely does, what founding partners get, why limited to 10
- CTA: Book a 15-min call or reply with interest

### Follow-up sequence
- **Day 3**: "Quick follow-up — still looking for 3 more agencies"
- **Day 7**: "Last note — closing founding partner program [date]"
- **If reply**: Personalized response, book call via Google Calendar

### Using Gmail MCP
- `gmail_create_draft` — create drafts for Jay to review before sending
- `gmail_search_messages` — track replies and engagement
- Never auto-send — always create drafts for Jay's review

## Google Calendar — Meeting Coordination

- `gcal_find_my_free_time` — find available slots for partner calls
- `gcal_create_event` — schedule confirmed calls
- Include video meeting link in calendar invite

## Tracking

All outreach activity logged in basic-memory:
- Contact name, agency, email, LinkedIn
- Outreach date and channel
- Response status (no reply, interested, declined, onboarded)
- Follow-up dates
- Notes from conversations

Update memory graph: link each contact to "Founding Partner Outreach" project entity.

## Founding Partner Application Page

When built, the application page on defusely.com should capture:
- Agency name
- Number of clients
- Current Reddit crisis process ("How do you handle Reddit brand crises today?")
- Willingness to do monthly feedback call
- Primary contact name + email

This is separate from the trial signup flow. Different path, different form, different tracking.

## Success Metrics

| Metric | Target |
|--------|--------|
| Agencies contacted | 50 |
| Response rate | 15-20% (8-10 responses) |
| Founding partners onboarded | 5-10 |
| Case studies generated | 2-3 |
| Referrals from partners | 3-5 warm intros |
