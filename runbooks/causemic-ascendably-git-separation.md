# Runbook: Keeping CauseMic and Ascendably Git separate on one Mac

**Owner:** Jen Day · **Set up:** 2026-10-23 · **Machine:** Jen's Mac

## Why this exists

I work for two consulting firms from one laptop. This setup makes sure CauseMic repos only ever commit and push as CauseMic, and Ascendably repos only ever as Ascendably. Nothing in the Ascendably setup (configured by Matt) was changed. Everything CauseMic was **added alongside** it.

**No secrets are stored in this runbook.** The private key lives only in 1Password and in `~/.ssh` on the Mac.

## What's separated

| | CauseMic | Ascendably (unchanged) |
|---|---|---|
| GitHub account | `jen-day-causemic` | `jen-ascend` |
| Push/pull credentials | SSH key `id_ed25519_causemic`, served by 1Password (Touch ID on every connection) | HTTPS, saved in the Mac Keychain |
| Repo address style | `git@github-causemic:CauseMic-Hub/<repo>.git` | `https://github.com/...` |
| Commit identity | Jen Day / jennifer@causemic.com (CauseMic folders only) | Jen Day / jen@ascendably.com (machine default) |
| Claude account | Claude Desktop (CauseMic login) | `claude` in a terminal, including VS Code's |

**Built-in safety net:** the Ascendably account has no access to CauseMic's private repos, so a misconfigured push fails rather than landing under the wrong name.

## Where things live

| File / place | What it does |
|---|---|
| 1Password item "GitHub – CauseMic" (private vault) | Holds the CauseMic SSH private key |
| 1Password → Settings → Developer → SSH agent | On. Serves the key with Touch ID approval. Key names enabled. |
| `~/.ssh/id_ed25519_causemic` / `.pub` | Original key files on the Mac (the `.pub` is the public half) |
| `~/.ssh/config` | Defines the `github-causemic` nickname (see below) |
| `~/.gitconfig` | Machine default identity (Ascendably), plus two `includeIf` lines for CauseMic folders |
| `~/.gitconfig-causemic` | CauseMic name and email |
| `~/VSCODE/Clients/CauseMic/` | Where CauseMic client repos are cloned |

### `~/.ssh/config`

```
# CauseMic GitHub (jen-day-causemic) - key served by 1Password
Host github-causemic
  HostName github.com
  User git
  IdentityAgent "~/Library/Group Containers/2BUA8C4S2C.com.1password/t/agent.sock"
  IdentityFile ~/.ssh/id_ed25519_causemic.pub
  IdentitiesOnly yes
```

### Lines added to `~/.gitconfig`

```
[includeIf "gitdir:~/VSCODE/Clients/CauseMic/"]
	path = ~/.gitconfig-causemic
[includeIf "gitdir:~/MyClaude/CauseMic/"]
	path = ~/.gitconfig-causemic
```

## Rebuild steps (new Mac, or if something breaks)

Each step lists what you should see. A command that prints nothing has succeeded. Failures start with `error:`, `fatal:`, or `Permission denied`.

### 1. Create the SSH key

```bash
ssh-keygen -t ed25519 -C "jennifer@causemic.com" -f ~/.ssh/id_ed25519_causemic
```
**Expect:** it asks for a passphrase twice, then prints "Your identification has been saved…" and a box of random characters (the visual fingerprint). If it asks "Overwrite (y/n)?", answer `n` and stop.

Then:
- Import the private key (`~/.ssh/id_ed25519_causemic`, no `.pub`) into 1Password as an **SSH Key** item in the **private vault**. In the file picker, press ⌘⇧G and type `~/.ssh`.
- Add the public key to GitHub (`jen-day-causemic`) → Settings → SSH and GPG keys → New SSH key → Authentication Key.
- **Check:** the fingerprints match in 1Password, on GitHub, and from:
  ```bash
  ssh-keygen -lf ~/.ssh/id_ed25519_causemic.pub
  ```
  **Expect:** one line starting `256 SHA256:…`.

### 2. Turn on the 1Password agent and add the nickname

- 1Password → Settings → Developer → **Use the SSH agent**. Choose **Use Key Names**. **Decline** "edit automatically" (it applies to every SSH connection, not just CauseMic).
- Add the `~/.ssh/config` block shown above. Append it **once** with `cat >> ~/.ssh/config`, then `chmod 600 ~/.ssh/config`. **Expect:** nothing prints. Running it twice duplicates the block, so check with `cat ~/.ssh/config`.
- **Check:**
  ```bash
  ssh -T git@github-causemic
  ```
  **Expect:** the first time, a prompt to confirm GitHub's fingerprint `SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU` (type `yes`), then a 1Password Touch ID prompt, then "Hi jen-day-causemic! You've successfully authenticated, but GitHub does not provide shell access."

### 3. CauseMic identity in CauseMic folders

```bash
cp ~/.gitconfig ~/.gitconfig.bak-$(date +%Y%m%d)
git config --file ~/.gitconfig-causemic user.name "Jen Day"
git config --file ~/.gitconfig-causemic user.email "jennifer@causemic.com"
git config --global includeIf."gitdir:~/VSCODE/Clients/CauseMic/".path ~/.gitconfig-causemic
git config --global includeIf."gitdir:~/MyClaude/CauseMic/".path ~/.gitconfig-causemic
```
**Expect:** nothing prints for any of these.

### 4. Clone CauseMic repos using the nickname

```bash
cd ~/VSCODE/Clients/CauseMic
git clone git@github-causemic:CauseMic-Hub/<repo>.git
```
**Expect:** "Cloning into '<repo>'...", a Touch ID prompt, then "Receiving objects: 100%… done."

### 5. Checks that prove the separation

| Run inside | Command | Expect |
|---|---|---|
| a CauseMic repo | `git config user.email` | `jennifer@causemic.com` |
| a CauseMic repo | `git remote -v` | two lines with `git@github-causemic:…` |
| an Ascendably repo | `git config user.email` | `jen@ascendably.com` |

## Day-to-day rules

- **Clone CauseMic repos only with the `git@github-causemic:` address**, and only under `~/VSCODE/Clients/CauseMic/` or `~/MyClaude/CauseMic/`. Anywhere else, commits fall back to the Ascendably email.
- **Do CauseMic Claude work in Claude Desktop** (CauseMic login), with the session opened in the repo folder. Plain `claude` in a terminal is signed in as Ascendably.
- **Approve a Touch ID prompt only when you expect a CauseMic push, pull, or clone.** An unexpected prompt means something is reaching GitHub that you didn't start.

## Not set up yet

- **GitHub command-line sign-in for CauseMic** (for opening pull requests from the terminal). Plan: a separate sign-in folder via `GH_CONFIG_DIR`, referenced from each repo's `.claude/settings.local.json` (personal, not committed). Until then, open pull requests in the browser.
- **VS Code's GitHub sign-in** for its pull request panel. Optional.
- **A separate `claude-causemic` terminal command.** Optional, using `CLAUDE_CONFIG_DIR="$HOME/.claude-causemic"`. Not needed while CauseMic work happens in Claude Desktop.

## How to undo

- **Git identity:** remove the two `includeIf` sections from `~/.gitconfig` (or restore `~/.gitconfig.bak-20261023`), and delete `~/.gitconfig-causemic`.
- **SSH:** delete the `github-causemic` block from `~/.ssh/config` (the original three-copy version is saved as `~/.ssh/config.bak-20261023`).
- **Key:** remove it from GitHub (Settings → SSH and GPG keys) and from 1Password.

None of these steps touch the Ascendably setup.
