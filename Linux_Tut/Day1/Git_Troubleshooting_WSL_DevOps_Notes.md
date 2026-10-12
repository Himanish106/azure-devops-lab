# Git Troubleshooting in WSL Ubuntu — DevOps Learning Notes

## Scenario

Repository: `git@github.com:Himanish106/azure-devops-lab.git`  
Linux user: `himanish3211`  
Environment: WSL Ubuntu

During everyday Git operations, we encountered three distinct problems: GitHub HTTPS authentication failed, Git couldn't identify the committer, and a merge couldn't write Git objects due to root-owned files.

---

## 1. `git push` asks for username and password

**Error:**

```text
Username for 'https://github.com':
Password for 'https://...@github.com':
remote: Invalid username or token. Password authentication is not supported for Git operations.
fatal: Authentication failed
```

**Cause:** The repository's `origin` remote used HTTPS. GitHub does not accept account passwords for Git operations. Use an SSH key or a personal access token instead.

### Check the remote

```bash
git remote -v
```

HTTPS remote:

```text
origin  https://github.com/Himanish106/azure-devops-lab.git (fetch)
origin  https://github.com/Himanish106/azure-devops-lab.git (push)
```

### Set up SSH authentication in WSL

Run as your **normal Linux user**, not `root`.

```bash
whoami
ls -la ~/.ssh
```

If `~/.ssh` doesn't exist, that is normal for a user who hasn't created SSH keys yet.

Generate a key (replace the example email with your GitHub email):

```bash
ssh-keygen -t ed25519 -C "your-github-email@example.com"
```

Accept the default file location, and preferably set a passphrase. This creates:

- `~/.ssh/id_ed25519` — **private key; never share it**
- `~/.ssh/id_ed25519.pub` — public key; safe to add to GitHub

Display your public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

Add it under **GitHub → Settings → SSH and GPG keys → New SSH key**:  
https://github.com/settings/keys

Test authentication:

```bash
ssh -T git@github.com
```

On first connection, **verify the displayed host fingerprint against GitHub's published SSH fingerprints** before typing `yes`:  
https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints

Expected successful message:

```text
Hi Himanish106! You've successfully authenticated, but GitHub does not provide shell access.
```

> **Key lesson:** Successful `ssh -T` proves SSH authentication works, but **does not change an existing HTTPS Git remote**.

### Switch the remote to SSH

From anywhere inside the repository:

```bash
git remote set-url origin git@github.com:Himanish106/azure-devops-lab.git
git remote -v
```

Expected:

```text
origin  git@github.com:Himanish106/azure-devops-lab.git (fetch)
origin  git@github.com:Himanish106/azure-devops-lab.git (push)
```

Then:

```bash
git push
```

An SSH key **passphrase** prompt is normal; it is different from a GitHub account-password prompt.

---

## 2. `git merge dev`: Committer identity unknown

**Error:**

```text
Committer identity unknown
*** Please tell me who you are.
fatal: empty ident name (...) not allowed
```

**Cause:** Git did not have a configured `user.name` and/or `user.email` for the current Linux user. A merge commit needs author/committer identity.

### Fix (all repositories for this Linux user)

```bash
git config --global user.name "Himanish106"
git config --global user.email "himanish.das23@gmail.com"
```

Verify:

```bash
git config --global --get user.name
git config --global --get user.email
```

Or configure **only the current repository** by omitting `--global`:

```bash
git config user.name "Himanish106"
git config user.email "himanish.das23@gmail.com"
```

> Git's `user.name` is a commit display name, not necessarily your GitHub login. For GitHub attribution, use an email verified on your GitHub account (or GitHub's provided noreply email).

---

## 3. `git merge dev`: Insufficient permission for Git objects

**Error:**

```text
error: insufficient permission for adding an object to repository database .git/objects
Merge with strategy ort failed.
```

**Cause in our case:** Earlier Git commands were run as `root`. Some internal Git directories and files became owned by `root`, even though the top-level `.git` and `.git/objects` directories were owned by `himanish3211`.

### Diagnose

```bash
whoami
ls -ld ~/azure-devops-lab/.git
ls -ld ~/azure-devops-lab/.git/objects
find ~/azure-devops-lab/.git -user root -ls
```

We found root-owned paths including:

```text
.git/logs/refs/heads/dev
.git/objects/50
.git/objects/8d
.git/objects/b2
.git/objects/af
.git/objects/bf
.git/objects/37
.git/COMMIT_EDITMSG
.git/refs/heads/dev
```

**Important:** Checking only `.git` and `.git/objects` is insufficient. Their child directories and files may have different owners.

### Fix ownership

Use this command **only for your own repository after verifying its path**:

```bash
sudo chown -R "$(id -un):$(id -gn)" /home/himanish3211/azure-devops-lab
```

Verify that no Git metadata remains root-owned:

```bash
find ~/azure-devops-lab/.git -user root -ls
```

Expected: no output.

Check repository state before attempting another merge:

```bash
git status
```

If no merge is in progress:

```bash
git merge dev
```

If a merge is already in progress, follow the instructions from `git status`; resolve conflicts and complete the merge, or abort intentionally with `git merge --abort` if appropriate. **Do not blindly restart or abort a merge.**

After a successful merge:

```bash
git push
```

> Avoid `sudo git add`, `sudo git commit`, `sudo git merge`, and `sudo git push`. Running normal Git operations as root can create ownership problems.

---

## 4. WSL user vs root: why it matters

Your terminal prompts identify which Linux user is running commands:

```text
root@DESKTOP-IET4SNM:...#
himanish3211@DESKTOP-IET4SNM:...$
```

- SSH keys under `/root/.ssh` belong to root's environment.
- SSH keys under `/home/himanish3211/.ssh` belong to the regular user's environment.
- `git config --global` is also user-specific.
- The same repository may have mixed file ownership if different users modify it.

Check identity and home directory:

```bash
whoami
echo "$HOME"
```

Switch from root to the regular user:

```bash
su - himanish3211
```

---

## 5. Quick troubleshooting cheat sheet

| Symptom | Likely cause | First command / fix |
|---|---|---|
| Git asks for GitHub username/password | HTTPS remote | `git remote -v` |
| SSH works but push asks for password | Remote still uses HTTPS | `git remote set-url origin git@github.com:Himanish106/azure-devops-lab.git` |
| SSH authentication fails | Key not registered, loaded, or accessible | `ssh -T git@github.com` |
| `Committer identity unknown` | Missing Git name/email | `git config --global --list` |
| Cannot write `.git/objects` | Permissions/ownership | `find .git -user root -ls` (run at repo root) |
| Merge fails or reports conflicts | Incomplete merge or conflicting changes | `git status` |

### Daily Git workflow

```bash
cd ~/azure-devops-lab
git status
git branch --show-current
git remote -v
git add .
git commit -m "Update Linux learning notes"
git push
```

For merging `dev` **into your current branch**:

```bash
git status
git branch --show-current
git merge dev
git status
```

> `git merge dev` merges the local `dev` branch **into whichever branch is currently checked out**. It does not automatically switch to `dev` or update `dev` from GitHub.

---

## 6. Self-check questions

1. Why does GitHub reject your account password for HTTPS Git operations?
2. What does `ssh -T git@github.com` verify?
3. Why might `git push` still request HTTPS credentials after SSH setup?
4. What is the difference between `git config --global` and `git config`?
5. Why can `.git/objects` appear correctly owned while a merge still fails?
6. Why should routine Git commands not be run with `sudo`?
7. What does `git merge dev` do if you're currently on `main`?

**Answers:** (1) Password authentication for Git operations is unsupported. (2) SSH authentication. (3) The remote URL still uses HTTPS. (4) Per-user vs per-repository settings. (5) Child paths may be root-owned. (6) Root-owned files can block the normal user. (7) It merges local `dev` into `main`.
