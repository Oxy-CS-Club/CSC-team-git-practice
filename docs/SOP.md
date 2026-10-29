# Team Git & GitHub SOP

This is the rulebook for how our team works with Git and GitHub. Read it once fully, then use it as a reference: find your situation in the table of contents and follow the steps.

> **Before you start:** You should have finished the Meeting 1 Prerequisites (Git installed, `git config` done, `gh auth login` done) and accepted the GitHub collaborator invitation for this repo.

## Contents

1. [The Golden Rules](#1-the-golden-rules)
2. [Roles and Accounts](#2-roles-and-accounts)
3. [One-Time Setup: Clone the Repo](#3-one-time-setup-clone-the-repo)
4. [Naming Conventions](#4-naming-conventions)
5. [The Daily Workflow (Step by Step)](#5-the-daily-workflow-step-by-step)
6. [Keeping Your Branch Up to Date](#6-keeping-your-branch-up-to-date)
7. [Code Review Rules](#7-code-review-rules)
8. [Merge Conflicts](#8-merge-conflicts)
9. [CI Checks](#9-ci-checks)
10. [Troubleshooting: "Oh no, I…"](#10-troubleshooting-oh-no-i)
11. [What Never Goes Into Git](#11-what-never-goes-into-git)
12. [Cheat Sheet](#12-cheat-sheet)

---

## 1. The Golden Rules

These are not suggestions. GitHub enforces most of them automatically.

1. **Nobody changes `main` directly.** Every change goes: Issue → branch → commits → Pull Request → review → merge.
2. **One task = one Issue = one branch = one PR.** Don't mix unrelated changes in one branch.
3. **Always start a new branch from an up-to-date `main`.** Run `git switch main && git pull` first.
4. **Never work on someone else's branch** unless they ask you to.
5. **Never use `git push --force`.** If Git rejects your push, read [Section 10](#10-troubleshooting-oh-no-i) instead.
6. **Every PR needs 1 approval and green CI before merging.** You cannot approve your own PR.
7. **Never commit secrets** (passwords, API keys, `.env` files). If it happens, tell the project lead **immediately** (see [10.12](#1012-i-committed-a-password--api-key--env-file)).
8. **After your PR is merged, that branch is done.** Start a new branch for the next task. Don't keep committing to a merged branch.
9. **When unsure, stop and ask** in the team channel. A 2-minute question is cheaper than an hour of fixing.

---

## 2. Roles and Accounts

| Role | Who | What they do |
|---|---|---|
| Project lead | Barry | Creates and assigns Issues, reviews PRs, manages repo settings |
| Member | Everyone | Picks up Issues, works on branches, opens PRs, reviews teammates' PRs |

**Accounts:**

- Use **your own personal GitHub account** for everything (Issues, commits, PRs, reviews). This way every action shows who did it.
- The official `Oxy-CS-Club` account is **only** used by the project lead for admin settings. Never log in to it in your terminal.

---

## 3. One-Time Setup: Clone the Repo

Pick a folder for your projects (for example, `~/Documents/projects`), then:

```bash
cd ~/Documents/projects
gh repo clone Oxy-CS-Club/CSC-team-git-practice
cd CSC-team-git-practice
git status
```

Expected output of `git status`:

```
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

**Recommended:** set VS Code as Git's editor, so Git never drops you into Vim:

```bash
git config --global core.editor "code --wait"
```

(If `code` is not found on Mac: open VS Code → `Cmd+Shift+P` → "Shell Command: Install 'code' command in PATH".)

---

## 4. Naming Conventions

### 4.1 Branch names

Format: `<type>/<your-name>-<short-description>`, all lowercase, words joined with `-`.

| Type | Use for | Example |
|---|---|---|
| `feat` | New feature or content | `feat/amy-login-page` |
| `fix` | Bug fix | `fix/barry-navbar-mobile` |
| `docs` | Documentation only | `docs/kevin-update-readme` |
| `refactor` | Restructure code, no behavior change | `refactor/lily-split-api-utils` |
| `chore` | Config, dependencies, tooling | `chore/barry-add-gitignore` |

### 4.2 Commit messages

Format: `<type>: <what you did>`, in present tense, under ~60 characters.

```
feat: add login form
fix: correct navbar width on mobile
docs: add setup steps to README
```

Bad examples: `update`, `fix stuff`, `asdf`, `final version 2`.

**Commit often.** A commit is a save point. Small commits (every meaningful step) are easier to review and to undo.

### 4.3 PR titles

Same format as commit messages: `feat: add login form`. We use **squash merge**, so your PR title becomes the single commit message on `main`. Make it good.

---

## 5. The Daily Workflow (Step by Step)

This is the loop you will repeat for every task.

### Step 1. Get an Issue

Every piece of work starts with an Issue (a task card on GitHub, in the **Issues** tab).

- **Assigned by the lead:** you'll see your name under *Assignees*. It's yours.
- **Picking one yourself:** find an unassigned Issue, comment "I'll take this", and click *Assignees → assign yourself*. Hold **at most 2** open Issues at a time.
- **No Issue exists for what you want to do?** Create one: *Issues → New issue*, give it a clear title and description, assign yourself.

Assigning is not a permission. It just means "I'm working on this", so nobody else does the same work twice.

Note the Issue number (for example `#12`). You'll need it later.

### Step 2. Update `main`

```bash
git switch main
git pull
```

### Step 3. Create your branch

```bash
git switch -c feat/amy-login-page
```

`-c` means "create". Check where you are at any time with `git status` (first line says `On branch ...`).

### Step 4. Do the work and commit

Edit files, then:

```bash
git status                  # see what changed
git diff                    # see exact line changes (press q to quit)
git add <file1> <file2>     # stage specific files
git commit -m "feat: add login form layout"
```

Prefer `git add <file>` over `git add .`. `git add .` adds **everything**, including files you didn't mean to commit. If you do use it, run `git status` first and check the list.

### Step 5. Push your branch

First push of a new branch:

```bash
git push -u origin feat/amy-login-page
```

After that, just:

```bash
git push
```

Push at least once at the end of every work session, even if unfinished. Your laptop is not a backup.

### Step 6. Open a Pull Request

Either click the yellow **Compare & pull request** banner on the GitHub repo page, or run:

```bash
gh pr create --fill
```

Then on the PR page, check:

- **Base:** `main` ← **compare:** your branch.
- **Title** follows the [naming convention](#43-pr-titles).
- **Description** filled in (the template will guide you), including `Closes #12` so the Issue auto-closes when merged.
- **Reviewer:** request at least 1 teammate (right sidebar → *Reviewers*).

Not finished yet but want early feedback? Open it as a **Draft PR** (dropdown next to the create button). Mark *Ready for review* when done.

### Step 7. Respond to review

- Reviewer asks for changes → make them on **the same branch**, commit, `git push`. The PR updates automatically. Don't open a new PR.
- Reply to each comment ("Fixed" / or explain why not), then click **Resolve conversation**. All conversations must be resolved before merging.
- Pushing new commits removes old approvals. Ask the reviewer to look again.

### Step 8. Merge

When the PR has **1 approval**, **all checks green**, and **all conversations resolved**:

1. The **PR author** clicks **Squash and merge** → **Confirm**.
2. The branch on GitHub is deleted automatically.

### Step 9. Clean up locally

```bash
git switch main
git pull
git branch -D feat/amy-login-page   # delete your local copy of the branch
git fetch --prune                   # forget deleted remote branches
```

Why `-D` (capital)? With squash merge, Git can't tell your branch was merged, so `-d` refuses. Use `-D` **only after you've confirmed the PR shows "Merged"** on GitHub.

Then go back to Step 1 for your next task.

---

## 6. Keeping Your Branch Up to Date

### 6.1 Why this matters

When you create a branch, it's a copy of `main` **at that moment**. While you work, teammates merge their PRs and `main` moves forward. Your branch doesn't see those changes until you bring them in.

Being behind is not a problem by itself. It becomes a problem when:

- someone changed the same lines you're changing → **merge conflict**;
- someone changed something your code depends on (renamed a function, moved a file) → each PR works alone, but together they **break `main`**.

### 6.2 When to sync

Bring the latest `main` into your branch:

- at the **start of each work session** on a branch that's more than a day old;
- **right before opening a PR**;
- when your PR says **"This branch has conflicts"** or **"This branch is out-of-date"**;
- when a teammate tells you they merged something that affects your work.

### 6.3 How to sync (two ways)

**Way A: on your computer (always works, required if there are conflicts)**

```bash
git switch main
git pull                        # get latest main
git switch feat/amy-login-page  # back to your branch
git merge main                  # bring main's changes into your branch
```

- No conflicts: Git creates a merge commit (an editor may open with a message; just save and close). Then `git push`.
- Conflicts: follow [Section 8](#8-merge-conflicts).

**Way B: on GitHub (only when there are no conflicts)**

On your PR page, click **Update branch** → choose **Update with merge commit** (not rebase). Then on your computer run `git pull` so your local branch gets that commit too.

We always sync with **merge**, never rebase. Rebase rewrites history and requires force push, which is forbidden ([Golden Rule 5](#1-the-golden-rules)).

### 6.4 The "require up to date" rule

GitHub has an optional rule: **"Require branches to be up to date before merging."**

| | Rule OFF (current setting) | Rule ON (may be enabled later in the real project) |
|---|---|---|
| Your PR is behind `main`, **no conflicts** | You can merge. GitHub shows a hint that the branch is out-of-date. | **Merge button is blocked.** You must sync first. |
| Your PR **has conflicts** | Merge button is blocked. You must resolve conflicts. | Same. |

**When the rule is ON, the process for every PR is:**

1. PR is approved and green, but shows *"This branch is out-of-date with the base branch."*
2. Sync your branch ([6.3](#63-how-to-sync-two-ways), Way A or B).
3. CI runs again on the updated branch. Wait for it to turn green.
4. If the approval was removed by the new commit, ask your reviewer to approve again (a quick look is enough: the sync only brought in already-reviewed code from `main`).
5. Merge **right away**, before someone else merges and puts you behind again.

If two people are both waiting to merge, coordinate in the team channel: "I'm merging #15 now", then the other person syncs after.

---

## 7. Code Review Rules

### For the author

- Keep PRs **small** (ideally under ~300 changed lines). Big PRs get bad reviews.
- Review your own diff first (the **Files changed** tab). Remove debug prints, commented-out code, and unrelated changes.
- Fill in the PR description: what, why, how to test.

### For the reviewer

- Respond within **24 hours** (or tell the author you can't).
- Check: does it do what the Issue asks? Is it readable? Are there unrelated or accidental files? Any secrets?
- In the **Files changed** tab: click the `+` next to a line to comment. When done, click **Review changes** and choose:
  - **Approve**: good to merge.
  - **Request changes**: must be fixed before merging.
  - **Comment**: questions or suggestions, not blocking.
- Be kind and specific: "This loop runs twice; could we move it outside?" not "this is wrong".

---

## 8. Merge Conflicts

### 8.1 What a conflict is

Two branches changed **the same lines** of the same file. Git doesn't know which version to keep, so a human must decide. Conflicts are normal; they are not your fault and nothing is broken.

### 8.2 How to resolve (on your computer)

**1. Bring `main` into your branch:**

```bash
git switch main
git pull
git switch feat/amy-login-page
git merge main
```

Git says:

```
CONFLICT (content): Merge conflict in team-board.md
Automatic merge failed; fix conflicts and then commit the result.
```

**2. Open the file.** You'll see conflict markers:

```
<<<<<<< HEAD
Favorite language: Python
=======
Favorite language: JavaScript
>>>>>>> main
```

- Between `<<<<<<< HEAD` and `=======`: **your branch's** version (VS Code calls it *Current*).
- Between `=======` and `>>>>>>> main`: **main's** version, from whoever merged first (VS Code calls it *Incoming*).

**3. Decide the final content.** In VS Code, click **Accept Current**, **Accept Incoming**, or **Accept Both** above the conflict, or edit by hand. Then make sure **all three marker lines** (`<<<<<<<`, `=======`, `>>>>>>>`) are gone.

> **Rule:** never just delete a teammate's work to make the conflict go away. If you're not sure what to keep, ask the person who wrote the other version.

**4. Find any other conflicted files:**

```bash
git status
```

Files listed under *"Unmerged paths"* still need fixing.

**5. Finish the merge:**

```bash
git add team-board.md
git commit            # an editor opens with "Merge branch 'main'..."; save and close
git push
```

Your PR page will update and the conflict message disappears.

### 8.3 I messed up the merge and want to start over

As long as you haven't run `git commit` yet:

```bash
git merge --abort
```

Everything goes back to how it was before `git merge main`.

### 8.4 How to avoid conflicts

- Keep branches short-lived (a few days, not weeks).
- Sync with `main` often ([Section 6](#6-keeping-your-branch-up-to-date)).
- Tell the team when you're about to make big changes to a shared file.
- Don't reformat or reorder files you're not otherwise changing.

---

## 9. CI Checks

CI ("Continuous Integration") is a robot that runs checks on every PR automatically. A PR cannot be merged until all required checks pass.

In this practice repo, CI checks:

1. **No leftover conflict markers** (`<<<<<<<`, `=======`, `>>>>>>>`) in any file.
2. **Member profiles are named correctly**: files in `members/` must be lowercase `.md`, e.g. `members/amy.md`.

**Reading results:** at the bottom of the PR page.

- ✅ green: passed.
- ❌ red: failed. Click **Details** to see the log; the error message says which file and line.
- 🟡 yellow: still running. Wait.

**Fixing a failure:** fix the problem on your branch, commit, `git push`. CI reruns automatically. Never ask someone to bypass CI.

---

## 10. Troubleshooting: "Oh no, I…"

Find your situation. **Read the whole entry before running anything.** If your situation isn't here, stop and ask.

### 10.1 I edited files on `main` but haven't committed yet

Move your changes to a new branch. Your edits come with you:

```bash
git switch -c feat/amy-my-task
```

Then commit as usual.

### 10.2 I committed on `main` by accident (not pushed)

```bash
git switch -c feat/amy-my-task   # new branch keeps your commit
git switch main
git reset --hard origin/main     # put local main back to match GitHub
git switch feat/amy-my-task      # continue working here
```

⚠️ Run `git reset --hard` **only** on `main` and **only after** creating the new branch, or you'll lose your commit.

### 10.3 `git push` to `main` was rejected

```
remote: error: GH013: Repository rule violations found for refs/heads/main.
```

Good, the protection works. You committed on `main`; follow [10.2](#102-i-committed-on-main-by-accident-not-pushed), then push your new branch and open a PR.

### 10.4 I committed to the wrong branch (not pushed)

Example: you meant to commit to `feat/amy-b` but committed to `feat/amy-a`.

```bash
git log --oneline -3          # copy the commit ID (e.g. a1b2c3d)
git switch feat/amy-b
git cherry-pick a1b2c3d       # copy the commit here
git switch feat/amy-a
git reset --hard HEAD~1       # remove it from the wrong branch
```

If you already pushed to the wrong branch, ask the lead before doing anything.

### 10.5 `git push` rejected on my own branch: "Updates were rejected… fetch first"

The branch on GitHub has commits you don't have (for example, you clicked *Update branch* on GitHub, or pushed from another computer).

```bash
git pull
git push
```

If `git pull` reports a conflict, follow [Section 8](#8-merge-conflicts). **Do not** use `--force`.

### 10.6 `git switch` / `git pull` says "Your local changes would be overwritten"

You have uncommitted changes. Either commit them:

```bash
git add <files>
git commit -m "wip: save progress"
```

Or temporarily shelve them:

```bash
git stash          # hide changes
git switch main    # do what you needed
git switch -       # go back to your branch
git stash pop      # bring changes back
```

### 10.7 I want to throw away my uncommitted changes in a file

```bash
git restore <file>
```

⚠️ This permanently discards those edits.

### 10.8 Typo in my last commit message (not pushed)

```bash
git commit --amend -m "feat: correct message"
```

Already pushed? Leave it. With squash merge, only the PR title ends up on `main`, so just fix the PR title.

### 10.9 Undo my last commit (not pushed), but keep the changes

```bash
git reset --soft HEAD~1
```

The changes go back to "staged", ready to edit and commit again.

### 10.10 Undo a commit I already pushed to my branch

```bash
git revert <commit-id>
git push
```

This creates a new commit that undoes the old one. Safe for anything already pushed.

### 10.11 A merged PR broke `main`

Tell the team channel right away. Then on the merged PR page on GitHub, click **Revert**. This creates a new PR that undoes the change; get it reviewed and merged like any other PR. Fix the real problem in a new branch afterwards.

### 10.12 I committed a password / API key / `.env` file

1. **Tell the project lead immediately**, even if you haven't pushed.
2. **Consider the secret leaked** once it's pushed. This repo is public, and bots scan GitHub for keys within minutes. The key must be **revoked and replaced** at its source (e.g. regenerate the API key). Deleting the file in a new commit does **not** remove it from history.
3. Not pushed yet? Undo the commit ([10.9](#109-undo-my-last-commit-not-pushed-but-keep-the-changes)), remove the file, add it to `.gitignore`, then commit again.

### 10.13 My PR shows files / commits I didn't touch

You probably created your branch from another feature branch instead of `main`, or from an outdated `main`. Ask the lead. Usually the fix is: create a fresh branch from an updated `main` and copy over only your changes.

### 10.14 The merge button is grey

Read the message above the button:

| Message | What to do |
|---|---|
| "Review required" | Request a reviewer and wait for approval. |
| "Changes requested" | Address the comments, push, ask for re-review. |
| "Some checks were not successful" | Fix CI ([Section 9](#9-ci-checks)). |
| "This branch has conflicts" | Resolve conflicts ([Section 8](#8-merge-conflicts)). |
| "Unresolved conversations" | Reply to and resolve every conversation. |
| "This branch is out-of-date" (only if the rule is on) | Sync ([Section 6](#6-keeping-your-branch-up-to-date)). |

### 10.15 I deleted a branch by mistake

- Deleted on GitHub: open the PR page and click **Restore branch**.
- Deleted locally but it still exists on GitHub: `git switch <branch-name>` recreates it from GitHub.

### 10.16 A strange editor opened in my terminal (Vim)

Usually during `git commit` or `git merge`. To save and exit: press `Esc`, type `:wq`, press `Enter`. To avoid it in the future, set VS Code as the editor ([Section 3](#3-one-time-setup-clone-the-repo)).

### 10.17 "Authentication failed" / "Permission denied" when pushing

```bash
gh auth status        # are you logged in, as the right account?
gh auth login         # log in again if needed
gh auth setup-git     # make git use gh's login
```

Still denied? Make sure you accepted the collaborator invitation (check your email or github.com/notifications).

### 10.18 "You are in 'detached HEAD' state"

You checked out a commit instead of a branch. Get back to safety:

```bash
git switch main
```

If you made commits while detached that you want to keep, run `git switch -c feat/amy-rescue` **before** switching away.

---

## 11. What Never Goes Into Git

- Secrets: `.env`, passwords, API keys, tokens, credentials files.
- Dependencies and build output: `node_modules/`, `dist/`, `build/`, `__pycache__/`, `venv/`.
- OS and editor junk: `.DS_Store`, `Thumbs.db`, personal `.vscode/` or `.idea/` settings.
- Large files (anything over a few MB), unless the team agrees.

The repo's `.gitignore` already blocks the common ones. If you see a file in `git status` that shouldn't be committed, ask whether it should be added to `.gitignore`.

---

## 12. Cheat Sheet

```bash
# Where am I? What changed?
git status
git log --oneline -10
git diff

# Start a new task
git switch main
git pull
git switch -c feat/<name>-<task>

# Save and upload work
git add <files>
git commit -m "feat: describe change"
git push -u origin feat/<name>-<task>   # first time
git push                                # after that

# Open a PR
gh pr create --fill

# Bring latest main into my branch
git switch main && git pull
git switch feat/<name>-<task>
git merge main

# Abort a merge gone wrong
git merge --abort

# After my PR is merged
git switch main && git pull
git branch -D feat/<name>-<task>
git fetch --prune
```
