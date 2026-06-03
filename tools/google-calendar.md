# Tool Setup Guide — Google Calendar MCP

> **Required for:** CTO / IT Head, Project Manager  
> **Recommended for:** All team members  
> **Role:** Schedule and manage meetings, sprint ceremonies, and cross-team coordination. MCP integration lets Claude create events, find free time slots, and manage the full meeting lifecycle — completing the Calendar → Fireflies → Claude intelligence loop.

---

## 1. What It Is

Google Calendar is QTrust's scheduling platform for all project meetings and sprint ceremonies. The Calendar MCP integration allows Claude to create, read, update, and query calendar events directly from conversations — without opening a browser.

The most powerful use of Calendar MCP is the **full meeting loop**:

```
Claude creates event in Google Calendar
        ↓
Google Calendar sends invite to all participants
        ↓
Fireflies auto-joins the meeting (if auto-join is enabled)
        ↓
Fireflies transcribes and summarises
        ↓
Claude extracts action items and decisions from the transcript
```

This means you can ask Claude to schedule a sprint planning session, and the entire flow from scheduling to post-meeting summary is handled automatically.

---

## 2. Who Needs This

| Role | Priority | Why |
|---|---|---|
| CTO / IT Head | **Required** | Cross-project schedule visibility, stakeholder meeting coordination |
| Project Manager | **Required** | Schedule and manage all sprint ceremonies, standups, reviews |
| All Engineers | **Recommended** | Check meeting schedules, respond to invites |
| All Roles | Recommended | Find free time for ad-hoc meetings |

---

## 3. Account Setup

Your `@qtrust.id` Google Workspace account already includes Google Calendar — no separate sign-up needed.

1. Go to [calendar.google.com](https://calendar.google.com) and sign in with your `@qtrust.id` account
2. Confirm your calendar is visible and existing meetings are showing
3. Ask the IT Head to create a **shared project calendar** for each active project:
   - Calendar name: `[PROJECT_CODE] — Team` (e.g., `QT-PORTAL — Team`)
   - Share with all project team members with **Make changes to events** permission
   - This shared calendar is where all sprint ceremonies are scheduled

---

## 4. Connecting Google Calendar MCP in Claude Desktop

1. Open Claude Desktop → **Settings** → **Integrations**
2. Find **Google Calendar** and click **Connect**
3. You will be redirected to Google to authorise the connection using your `@qtrust.id` account
4. Grant the requested permissions (read and write calendar events)
5. Return to Claude Desktop — the connection should show **Connected**

> **Note:** Claude will have access to all calendars visible in your Google account, including shared project calendars. This is intentional — it allows Claude to schedule meetings on behalf of the whole team.

---

## 5. Verification Test

Once connected, run these in a Claude conversation:

```
"List my calendars."
"What meetings do I have scheduled this week?"
"Find a 1-hour slot this week where [Name] and I are both free."
```

---

## 6. Sprint Ceremony Scheduling

The Project Manager should set up all recurring sprint ceremonies at the start of each project. Use Claude to do this efficiently.

### Recommended Sprint Ceremony Schedule

| Ceremony | Frequency | Duration | Suggested Time |
|---|---|---|---|
| Daily Standup | Mon–Fri | 15 min | 09:30 |
| Sprint Planning | First Monday of sprint | 2 hours | 10:00 |
| Backlog Grooming | Mid-sprint Wednesday | 1 hour | 14:00 |
| Sprint Review | Last Friday of sprint | 1 hour | 14:00 |
| Sprint Retrospective | Last Friday of sprint | 45 min | 15:15 |

### Claude Prompt — Schedule All Sprint Ceremonies at Once

```
"Schedule all Sprint 1 ceremonies on the [PROJECT_CODE] — Team calendar.
Sprint duration: [start date] to [end date].

- Daily Standup: Monday to Friday, 09:30–09:45, recurring for the sprint duration.
  Invite: [list all team member emails]

- Sprint Planning: [date], 10:00–12:00.
  Invite: [all team members]
  Description: 'Review sprint backlog, commit to sprint goal, assign issues.'

- Backlog Grooming: [mid-sprint date], 14:00–15:00.
  Invite: [PM + Tech Leads + Product]
  Description: 'Refine and estimate next sprint backlog items.'

- Sprint Review: [last Friday], 14:00–15:00.
  Invite: [all team members + stakeholders]
  Description: 'Demo completed work from Sprint 1.'

- Sprint Retrospective: [last Friday], 15:15–16:00.
  Invite: [all team members]
  Description: 'What went well, what to improve, action items for Sprint 2.'"
```

---

## 7. Usage Examples by Role

### CTO — Cross-Project Schedule Overview

```
"List all meetings on my calendar this week that are tagged with a project code.
Group them by project and show me the total meeting hours per project."
```

```
"I need a 90-minute window this week for a cross-project leads review.
Find a time when [PM Name], [Lead Engineer], and I are all free between 09:00 and 17:00."
```

```
"Show me all sprint planning meetings scheduled in the next two weeks across all project calendars."
```

### Project Manager — Sprint Management

```
"Create a Sprint 2 Planning event on the [PROJECT_CODE] — Team calendar.
Date: [date], 10:00–12:00.
Invite all team members: [emails].
Add a description that includes the sprint goal: [goal] and a link to the Linear sprint: [URL]."
```

```
"The sprint review on [date] needs to be moved to [new date and time].
Update the event and notify all attendees."
```

```
"Find a time next week when the QA team and Backend team are both free for a 30-minute sync.
Relevant people: [emails]."
```

### All Team Members

```
"What meetings do I have tomorrow? Give me a summary of each one."
```

```
"I need to block off 3 hours for deep work tomorrow morning. Create a focus time event called 'Deep Work — [task]' on my calendar from 09:00 to 12:00."
```

---

## 8. Integration with Fireflies

Google Calendar and Fireflies work together automatically when Fireflies auto-join is enabled (see [`tools/fireflies.md`](fireflies.md)):

1. **PM schedules a meeting via Claude** → event appears in Google Calendar
2. **Fireflies detects the new Calendar event** and schedules the bot to join
3. **Meeting happens** → Fireflies records and transcribes
4. **After meeting**, ask Claude: *"Get the Fireflies summary of today's sprint planning. Extract the sprint goal, committed issues, and any open questions."*

> **Best practice:** Always add a meeting description and agenda when creating events via Claude. Fireflies includes the description in its summary context, which improves the quality of action item extraction.

---

## 9. Calendar Hygiene Rules

To keep calendars useful for everyone:

- **Use the shared project calendar** for all sprint ceremonies — not personal calendars
- **Always include a description** with agenda points when creating project meetings
- **Decline promptly** if you cannot attend — this keeps free-time search accurate
- **Name meetings clearly:** `[PROJECT_CODE] Sprint 2 Review` not just `Review`
- **Add the video call link** (Google Meet) when creating events — team members should not need to ask for it

---

## 10. First-Day Checklist

- [ ] Google Calendar accessible at [calendar.google.com](https://calendar.google.com) with `@qtrust.id` account
- [ ] Added to the shared project calendar(s) for your project(s)
- [ ] Google Calendar MCP connected in Claude Desktop
- [ ] Verification test passed — can list calendars and events via Claude
- [ ] **Project Managers:** all Sprint 1 ceremonies scheduled on the shared calendar
- [ ] **Project Managers:** confirmed `fred@fireflies.ai` is invited to all recurring ceremonies

---

*See [`tools/README.md`](README.md) for the full tool matrix and setup order.*
