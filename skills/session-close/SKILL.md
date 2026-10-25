---
name: session-close
description: End-of-session checklist for CauseMic work. Recaps the session, checks for unfinished Git or Salesforce work, routes lessons to the right place, writes a session log, and hands off cleanly so another collaborator or a later session can pick up. Run only when the user types /session-close.
disable-model-invocation: true
---

# /session-close

Make sure everything this session learned or changed ends up where the next person, or the next Claude session, will find it. That next person may be a different collaborator. Write for someone who wasn't in this conversation.

**Ground rules for the whole checklist:**

- **Propose, don't apply.** List every change you want to make (file, location, exact wording) and wait for the user's yes before writing anything.
- **"Nothing worth saving" is a valid outcome.** Don't invent lessons or logs to fill the checklist.
- **Client isolation is absolute.** A client's details only ever go into that client's repo.
- **No secrets, no client data.** Before writing anything, check it for passwords, keys, tokens, and personal data about donors, constituents or staff. Link to Google Drive instead.
- **Name people by role or name, never "me" or "I".** Logs are read by other collaborators. Write "decided by: <name>", not "I decided".

## Step 0: Is there anything to save?

Look back over the conversation. If the session was only questions, exploration or chat, and nothing changed, was decided or was learned that someone else would need, say so plainly:

> Nothing in this session needs saving: no changes, decisions or lessons another collaborator would need.

Then still run Step 2 (unfinished work). If that's also clean, give the short receipt (see the end) and stop. If the user wants a log anyway, continue.

## Step 1: Recap

Draft:

- **Who:** the user who ran this session (name from `git config user.name` in the repo, or ask).
- **Goal:** what this session set out to do.
- **What changed:**
  - in repos: files, commits, pushes;
  - outside repos: the Salesforce org (alias), Google Drive, Asana, Slack, email, settings on the user's computer.
- **Decisions:** what was decided and **who decided it** (the user, another collaborator, the client, CauseMic leadership, or Claude with the user's approval).
- **Verified vs. assumed:** what was actually checked, and what wasn't.
- **Open questions:** each with an owner.

## Step 2: Unfinished work

In every repo touched this session, check for local work and for collaborators' work:

```bash
git fetch
git status -sb
```

- **Behind** (the status line shows `behind`): someone else has pushed. Tell the user before committing anything; pull first.
- **Uncommitted changes, or commits not yet pushed:** list them and ask whether to commit or push. Don't leave them silently.
- **Salesforce metadata** changed this session that hasn't been deployed: **flag it with the target org.** Never deploy as part of this checklist.

## Step 3: Where each lesson goes

For each thing worth keeping, choose one destination. The default is nowhere.

| If the lesson is… | It goes to… |
|---|---|
| About one client (names that client, its org, people, rules) | That client's repo, `Knowledge/` |
| True across CauseMic work, with no client details | The CauseMic CoE repo (currently `jen_center_of_excellence`): runbooks, standards, skills |
| A change to a convention for one repo | That repo's `CLAUDE.md` |
| True of one computer only (paths, local setup) | An uncommitted local note on that computer, never a shared file |
| One user's personal preference or feedback on how Claude works | Claude's memory for that user, not a shared file |

Also check whether this session made any existing doc out of date (a runbook, a decision record, a pending-work list). If so, propose the specific edit.

## Step 4: Session log

**First, confirm where it goes.** It belongs either in the client's repo (`Sessions/`) or in the CauseMic CoE repo (`sessions/`).

- Clearly about one client: propose that client's repo.
- General setup or CauseMic-wide: propose the CoE repo.
- **Not obvious, or touched more than one client: ask the user before writing anything.** Never put one client's details in another client's log.

File name: `YYYY-MM-DD-<user>-short-topic.md`, where `<user>` is a short name for whoever ran the session, so two collaborators' logs from the same day never collide. Contents:

```markdown
# <Date>: <short topic>

**Session by:** <name>
**Repo / org touched:** …
**Goal:** …

## What changed
- …

## Decisions
- <decision> (decided by: <name or role>)

## Verified / not verified
- Verified: …
- Not verified: …

## Open questions
- <question> (owner: <name or role>)

## Next
- Next action: …
- Pick up from: <exact file path or link>
- Deliberately left undone: … (why)
- Waiting on a person: … (who)
```

## Step 5: Handoff check (only if the work continues)

Don't rely on the recap. Walk the handoff the way a fresh session, or a different collaborator, would:

- Open the "pick up from" path and confirm it exists and says what the log claims.
- Confirm the next action can be started from what's written, without this conversation.

If something is missing, fix the log before going further.

## Step 6: Project tracker

Follow the **asana** skill's standards. Asana is the record of what happened, so don't copy task details into the session log or anywhere else.

1. **Find the tracker** in the client repo's `CLAUDE.md` ("Key links"). If the client uses its own board, draft the update as a table for the user to enter, and stop here.
2. **Changes already made this session:** re-read each task and confirm the change landed. Don't re-propose them. List them in the receipt as verified, or as missing with a proposed fix.
3. **Work not yet reflected in Asana:** find the tasks that cover this session's work and propose updates in the asana skill's approval table (dated description update, status, blockers, completion if clearly met). If no task covers the work, say where you looked, and propose a new task or milestone only if it's real follow-up work.
4. Apply only the approved rows, then re-read and confirm.

## Step 7: Review, commit, push

1. **🛑 Show the user everything at once:** the session log, and every proposed edit from steps 2–3, each with its destination. Wait for approval, corrections or cuts.
2. Write only what was approved.
3. Check the commit identity with `git config user.email`. It must be the user's CauseMic address. If it isn't, stop and tell them.
4. Show the commit (`git log --stat -1`) for each repo, then ask to push.
5. **🛑 Push only after the user says yes.** They approve the push in whatever credential prompt their setup uses.
6. Confirm with `git status -sb` that each repo shows no unpushed commits.

## Closing receipt

End with a short receipt, one line per item.

**Full version:**

```
Session by:    <name>
Session log:   <path>  (pushed / not pushed)
Lessons:       <destination> ← <one-line summary>   |  or "none this session"
Docs updated:  <file>  |  or "none"
Unfinished:    <item + why>  |  or "none"
Salesforce:    <metadata needing deploy + org>  |  or "nothing pending"
Asana:         <task> ✓ verified / updated / missing  <link>  (one line per task)  |  or "no task covers this work; looked in <project>"
Next session:  <pick-up path + next action>  |  or "work complete"
```

**Short version (nothing worth saving):**

```
Session by:    <name>
Session log:   not saved (nothing significant this session)
Unfinished:    none
```
