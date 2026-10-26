---
name: asana
description: CauseMic standards for reading, creating and updating Asana tasks and milestones on client projects. Use whenever a session creates, updates, completes or reviews Asana work, and from /session-close. Covers duplicate checks, required fields, description update format, client visibility, and the approval table.
---

# Asana at CauseMic

Asana is the record of **what's being done, by whom, and what's next** on a client project. Keep it accurate, readable at a glance, and safe for clients to see.

## Before touching Asana

1. **Find the project.** The client repo's `CLAUDE.md` has a "Key links" section with the project tracker. If it says the client uses **their own board** (not Asana), don't use Asana. Draft the update as a table for the user to enter on the client's board.
2. **Confirm the account.** The Asana connection must be the user's CauseMic account. If the identity check shows a different workspace or email, stop.
3. **Read the actual fields.** Before the first change in a project during a session, read the custom fields and their allowed values **from a task in that project**. A project's own settings can leave out fields that its tasks do have. Never guess a value.

## Writing for a shared audience

- **Assume the client can read it.** Client staff may be guests on the project. Titles, descriptions and comments must be fine for them to see. Internal-only notes go in the internal project notes doc in Google Drive, not in Asana.
- **No notifications without approval.** Don't @mention anyone, add followers, or assign someone (all of which email people, possibly at the client) unless the user approved that specific change.

## Creating tasks

- **Check for duplicates first.** Search the project for tasks with a similar subject before proposing a new one. If a likely match exists, propose updating it instead.
- **Only for real follow-up work.** Don't create a task for every small thought from a session.
- **Milestone or task?** If it marks a meaningful checkpoint (a sign-off, a go-live, a phase finished), propose a **milestone** instead of a task.
- **Titles read like headlines:** short and specific. Details go in the description.
  - Good: "Configure recurring donation close dates"
  - Too long: "Configure recurring donation close dates for monthly donors per the client's 10/20 email and validate with the integration vendor"

## Fields

| Field | Rule |
|---|---|
| **Phase Domain** | **Always set** when the field exists. Values seen (10/25/26): Project Mgmt · Governance + Design · Platform + Licenses · Advisory + Training · Development Substitution · Optimization Ideas · Troubleshooting. The list changes, so read the current values from a task in the project. **Optimization Ideas:** start the description with "Suggested by <name>, <date>, <source>". |
| **Client (SF)** | **Always set** when the field exists. |
| *(missing field)* | If a required field isn't on the task, say so in the approval table. Don't skip it silently. |

Dropdown fields are set by their option ID, not their label. Look up the IDs from a task in the project (step 3 above) and show the labels in the approval table. *(Tested 10/25/26: dropdown, text field and description update in one change all saved correctly.)*
| **Status (CRM Project)** | Set when it's clear: New · Scoping · In Progress · With Client · Complete · Blocked · Deferred · Rejected. **Ask if unclear.** |
| **Due date** | Leave blank unless the user gave a date or asked for an estimate. If unsure, ask. |
| **Assignee** | Same as due date: leave blank unless given or asked for. If unsure, ask. |
| **Responsible Party** | Free text: the client person or people responsible. |
| **Agenda** | Only when clearly called out in the session. It's subjective, so don't set it routinely. |
| **Dependencies / Blockers** | Suggestions are welcome. Propose them in the approval table. |

## Description format

The top of the description is the **core description**: what the task is for. It stays stable. Below it is a horizontal divider, then **dated updates, newest first**:

```
<Core description: what this is for and what "done" means>
────────────────────────
10/25/26 Pat Lee – <newest update>
10/20/26 Pat Lee – <earlier update>
```

- Each update starts with **date (m/d/yy, no leading zeros), the name of the person it's on behalf of, then a dash.** Use the name of the user running the session.
- Write the description with Asana's rich-text field: the divider is `<hr/>`, and each update is on its own line. *(Tested 10/25/26: the divider, line breaks and newest-first order all hold.)*
- **Add, don't rewrite.** Insert the new update at the top of the updates section. Never change the core description or earlier updates unless the user asks.
- **Re-read the task immediately before writing.** A teammate may have edited it since the session began. Rebuild the description from that fresh copy so nothing they added is lost.
- If the task has no divider yet, add one under the existing text, and treat that text as the core description.
- Use comments only when someone needs to be notified, and only with approval (see above).

## Completing tasks

- Mark **Complete** only when the user says so in the session, or when everything in the description's definition of done is clearly met.
- Otherwise, suggest it in the approval table.

## The approval table

Propose every create, update or completion as one table: **rows = tasks, columns = fields that change.** Show only the changes.

| Task | Action | Status | Phase Domain | Due | Assignee | Description update | Other |
|---|---|---|---|---|---|---|---|
| Configure recurring donation close dates | Update | In Progress → With Client | (no change) | (blank) | (blank) | 10/25/26 Pat Lee – Sent config summary to client lead for review | Blocker: awaiting client lead |
| UAT sign-off: Gift entry | **New milestone** | New | Governance + Design | (ask) | (blank) | Core: Client confirms gift entry works end to end | Duplicate check: none found |

Apply only the rows the user approves. Approval covers those rows only.

## After applying

Re-read every task you changed and confirm the change landed. Report one line per task, with its link:

```
✓ Configure recurring donation close dates: Status → With Client, update added  <link>
```

If something didn't apply, say so and propose a fix. Don't report success from memory.

## Not built yet

- Projects outside Asana (for example, a client's own board) can only be handled as drafted tables for manual entry.
