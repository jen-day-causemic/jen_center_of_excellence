# session-close: maintainer notes

This file is for people maintaining the skill. **Claude doesn't load it when the skill runs.** Only `SKILL.md` is loaded, and it doesn't reference this file. That keeps everything below inactive.

## Status

- **Draft, testing with one user (Jen Day) on one Mac.**
- Written to work for multiple collaborators and operating systems. The items below are **not built out yet**.

## How to install (for testing)

Copy the whole `session-close/` folder into a CauseMic repo at `.claude/skills/session-close/`. Then, in a Claude session started in that repo, type `/session-close`.

**Don't** install it in the user-level skills folder (`~/.claude/skills/` on Mac). That folder applies to every Claude session on the computer, including other firms' work.

## Future changes: multi-user (not built yet)

| Where in SKILL.md | Current wording | Change when |
|---|---|---|
| Step 3 table, Step 4 | "The CauseMic CoE repo (currently `jen_center_of_excellence`)" | A shared CauseMic CoE repo exists in CauseMic-Hub. Replace with its name. `jen_center_of_excellence` stays Jen's personal space. |
| Step 4 | CoE session logs go in one `sessions/` folder | Several people log there. Consider `sessions/<user>/` subfolders. |
| Step 7 | Push is approved by the user directly to `main` | The shared repos turn on branch protection (GitHub Team plan). Change to: create a branch, push, open a pull request for review. |
| Step 2 | `git fetch` + warn when behind | Several people work in the same client repo at once. Add guidance for merge conflicts. |
| Step 6 | Uses the `asana` skill. Tracker link comes from each client repo's `CLAUDE.md` "Key links" | Client repos get their "Key links" section. Until then, ask the user for the project. |
| Step 1 | User name comes from `git config user.name` | A shared roster exists. Consider a short-name convention for file names. |

## Future changes: multiple operating systems (not built yet)

| Where | Current assumption | Change when |
|---|---|---|
| Install notes above | Mac paths (`~/.claude/skills/`) | A Windows user joins. Add the Windows path (`%USERPROFILE%\.claude\skills\`). |
| Step 2, Step 7 commands | `git fetch`, `git status -sb`, `git log`, `git config` | These already work the same on Mac, Windows and Linux. Recheck if non-Git commands are added. |
| Step 7, push approval | "Whatever credential prompt their setup uses" | Each user's setup is documented. The Mac setup uses 1Password + Touch ID (see `runbooks/causemic-ascendably-git-separation.md`). Windows would use 1Password + Windows Hello, or another approach. |
| Firm-separation runbook | Mac only | A Windows or Linux version of `runbooks/causemic-ascendably-git-separation.md` is needed. |

## Change log

- 2026-10-25: Step 6 now uses the `asana` skill: it verifies changes already made in the session and proposes only what's missing. The receipt lists one line per task.
- 2026-10-24: Made user-neutral (no personal names in the skill), added Step 0 ("nothing worth saving" exit), collaborator check (`git fetch`, behind warning), "Session by" in logs and receipt, user name in log file names. Moved install notes and future items here.
- 2026-10-23: First draft.
