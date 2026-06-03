# Tool Setup Guide — Fireflies MCP

> **Required for:** CTO, Project Manager, Team Leads
> **Recommended for:** All team members who attend project meetings
> **Role:** Automatic meeting transcription and AI-powered summaries. MCP integration lets Claude search transcripts, extract action items, and surface decisions across all recorded meetings.

---

## 1. What It Is

Fireflies.ai automatically records, transcribes, and summarises every meeting — standups, sprint planning, design reviews, stakeholder calls. Once a meeting is recorded, Claude can search across all transcripts to find decisions, action items, blockers, and commitments made in any meeting.

This is especially valuable for the CTO and PM: instead of sitting through recordings, you ask Claude and get the answer in seconds.

---

## 2. Who Needs This

| Role | Priority | Why |
|---|---|---|
| CTO / IT Head | **Required** | Monitor all project decisions without attending every meeting |
| Project Manager | **Required** | Extract action items and blockers from standups automatically |
| Team Leads | **Required** | Track what was discussed and committed in their team's meetings |
| All Engineers | **Recommended** | Catch up on meetings you missed; no more "can you summarise the call?" |
| UIX Designer | **Recommended** | Reference design review discussions and feedback |

---

## 3. Account Setup

1. Go to [fireflies.ai](https://fireflies.ai) and sign up with your `@qtrust.id` Google account
2. Ask the IT Head to add you to the QTrust Fireflies workspace
3. Accept the workspace invitation
4. Install the Fireflies browser extension (optional but recommended for Google Meet)

---

## 4. Adding Fireflies to Your Meetings

**Google Meet (recommended method):**

- Invite `fred@fireflies.ai` to any Google Meet call as a participant
- Fireflies will join automatically, record, and transcribe the meeting
- The transcript is available in your Fireflies dashboard within 5–10 minutes after the meeting ends

**Zoom / Microsoft Teams:**

- Connect your Zoom or Teams calendar in Fireflies settings
- Fireflies will automatically join scheduled meetings

**Auto-join (recommended):**

In Fireflies settings → **Auto-join** → enable for all meetings from your `@qtrust.id` calendar. This ensures no meeting is ever missed.

---

## 5. Connecting Fireflies MCP in Claude Desktop

The Fireflies MCP is already installed in the QTrust Claude Desktop setup. To verify it is active:

1. Open a Claude conversation in your project
2. Ask: `"What tools do you have access to?"`
3. Fireflies should appear in the list

If it is not active:

1. Go to Claude Desktop → **Settings** → **Integrations**
2. Find **Fireflies** and click **Connect**
3. Authorise with your `@qtrust.id` account

---

## 6. Verification Test

Run these prompts in Claude Desktop to confirm the integration is working:

```
"Fetch my recent Fireflies meetings."
"Search Fireflies for meetings about [topic] in the last 2 weeks."
"Get the summary of my most recent meeting."
```

---

## 7. Usage Examples by Role

**CTO — Weekly team pulse:**

```
"Fetch summaries of all project meetings recorded this week in Fireflies.
For each meeting, extract: key decisions made, open questions, and action items with owners."
```

**Project Manager — Action item extraction:**

```
"Get the transcript of the sprint planning meeting from [date].
Extract all action items with the person responsible and any mentioned deadlines."
```

**Project Manager — Blocker tracking:**

```
"Search Fireflies transcripts from the last 2 weeks for mentions of 'blocked', 'blocker', or 'waiting on'.
Summarise what each blocker is and who mentioned it."
```

**Engineer — Meeting catch-up:**

```
"I missed the design review meeting on [date]. Get the Fireflies summary and tell me:
what was decided about [feature], and are there any action items assigned to the Backend team?"
```

**CTO — Cross-project visibility:**

```
"Search all Fireflies meetings from this month.
Which technical decisions came up across multiple meetings?
Are there any recurring blockers or risks mentioned more than once?"
```

---

## 8. Meeting Hygiene Rules

To get the most from Fireflies across all projects:

- Always invite `fred@fireflies.ai` to project meetings
- Give meetings descriptive titles: `[PROJECT_CODE] Sprint 1 Planning` not just `Meeting`
- Start meetings with a brief agenda recap — Fireflies will include it in the summary
- End meetings with explicit action items: *"So [Name] will do X by [date]"* — Fireflies captures this well

---

## 9. First-Day Checklist

- [ ] Fireflies account created with `@qtrust.id` Google SSO
- [ ] Added to QTrust Fireflies workspace
- [ ] Auto-join enabled for `@qtrust.id` calendar meetings
- [ ] `fred@fireflies.ai` added to recurring project meeting invites
- [ ] Fireflies MCP active in Claude Desktop (run verification test)
- [ ] First transcript fetched via Claude
