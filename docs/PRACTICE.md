# Meeting 1 Practice: Your First Team Workflow

In this practice you'll go through the full team workflow twice:

- **Round 1:** add your own profile. Nobody touches the same file, so there are no conflicts. Focus on the process: Issue → branch → commit → push → PR → review → merge.
- **Round 2:** you and your partner edit **the same line** of the same file on purpose. One of you will get a **merge conflict** and fix it.

Keep [SOP.md](SOP.md) open in another tab. Every step here follows it.

**Throughout this doc:** replace `amy` with **your first name in lowercase** (if two people share a first name, add your last initial: `amy-c`), and `#N` with your Issue number.

**🛑 Checkpoint** means: stop, check your result matches, and raise your hand if it doesn't. Don't continue on a broken step.

---

## Part 0. Pair Up (1 min)

Count off around the room: 1, 2, 3, 4, …

| Your number | Your pair | Role in Round 2 |
|---|---|---|
| 1, 2 | Pair 1 | odd number = **first**, even number = **second** |
| 3, 4 | Pair 2 | |
| 5, 6 | Pair 3 | |
| 7, 8 | Pair 4 | |
| 9, 10 | Pair 5 | |

Odd total? The last person joins the previous pair as a trio (for example, number 9 joins Pair 4). In a trio, the third person goes **third** in Round 2.

Write down: your pair number, your partner's GitHub username, and whether you are first or second.

---

## Part 1. Clone the Repo (3 min)

```bash
cd ~/Documents            # or any folder you keep projects in
gh repo clone Oxy-CS-Club/CSC-team-git-practice
cd CSC-team-git-practice
git status
```

Open the folder in VS Code:

```bash
code .
```

🛑 **Checkpoint:** `git status` shows:

```
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

Problems? `Permission denied` or `Repository not found` → check that you accepted the collaborator invitation (github.com/notifications), then see SOP [10.17](SOP.md#1017-authentication-failed--permission-denied-when-pushing).

---

# Round 1: Add Your Profile

## Part 2. Create Your Issue (2 min)

On GitHub, open the repo → **Issues** tab → **New issue**.

- **Title:** `Add member profile for Amy`
- **Description:** `Create members/amy.md with my intro.`
- Right sidebar → **Assignees** → click **assign yourself**.
- Click **Create**.

Note the Issue number at the top (e.g. `#7`).

🛑 **Checkpoint:** your Issue shows your avatar under *Assignees*.

---

## Part 3. Branch, Edit, Commit, Push (5 min)

### 3.1 Start from an up-to-date main and create your branch

```bash
git switch main
git pull
git switch -c feat/amy-profile
```

Expected:

```
Switched to a new branch 'feat/amy-profile'
```

### 3.2 Create your profile file

Create the file `members/amy.md` (in VS Code: right-click the `members` folder → New File). Paste this and fill it in:

```markdown
# Amy Chen

- **GitHub:** @your-username
- **Year / Major:**
- **One thing I want to learn in this project:**
- **Fun fact:**
```

Save the file.

### 3.3 Check what changed, then commit

```bash
git status
```

Expected: `members/amy.md` listed under *Untracked files*. Only that file. If you see anything else, ask before continuing.

```bash
git add members/amy.md
git commit -m "feat: add member profile for amy"
```

### 3.4 Push your branch

```bash
git push -u origin feat/amy-profile
```

🛑 **Checkpoint:** on GitHub, the branch dropdown (top left of the code view, says `main`) lists `feat/amy-profile`.

---

## Part 4. Open a Pull Request (3 min)

On the repo page, click the yellow **Compare & pull request** banner. (Or run `gh pr create --web`.)

Check and fill in:

- **base:** `main` ← **compare:** `feat/amy-profile`
- **Title:** `feat: add member profile for amy`
- **Description:** fill in the template. Make sure it contains `Closes #N` with **your** Issue number.
- Right sidebar → **Reviewers** → select **your partner**. (Trio: the person after you; the third person picks the first.)

Click **Create pull request**.

Wait for the checks at the bottom of the PR to finish.

🛑 **Checkpoint:** your PR shows ✅ for all checks, and *"Review required"* above the merge button. The merge button is grey. That's the protection working.

---

## Part 5. Review Your Partner's PR (6 min)

Now **switch roles**: open the PR your partner requested you to review (repo → **Pull requests** tab, or the notification).

### 5.1 Reviewer: leave a comment (don't approve yet)

1. Go to the **Files changed** tab.
2. Hover over any line of their profile, click the blue **+**, and write a friendly suggestion, e.g. *"Could you add one more fun fact?"*
3. Click **Start a review** (not *Add single comment*).
4. Top right: **Review changes** → choose **Comment** → **Submit review**.

### 5.2 Author: respond to your review

When you see your partner's comment on **your** PR:

1. Edit `members/amy.md` to address it. Save.
2. Commit and push to the **same branch**:

   ```bash
   git add members/amy.md
   git commit -m "feat: add another fun fact"
   git push
   ```

3. On the PR, reply to the comment (e.g. *"Done!"*) and click **Resolve conversation**.

Your PR updates by itself. You do **not** open a new PR.

### 5.3 Reviewer: approve

Back on your partner's PR → **Files changed** → check the new change → **Review changes** → **Approve** → **Submit review**.

🛑 **Checkpoint:** your own PR now shows ✅ *"Changes approved"*, ✅ all checks passed, and the green **Squash and merge** button is clickable.

---

## Part 6. Merge and Clean Up (3 min)

### 6.1 Merge (author does this on their own PR)

Click **Squash and merge** → **Confirm squash and merge**.

Then check:

- The PR says **Merged** (purple).
- Your Issue `#N` is now **Closed** (because of `Closes #N`).
- Your branch was deleted on GitHub automatically.

### 6.2 Clean up locally

```bash
git switch main
git pull
git branch -D feat/amy-profile
git fetch --prune
```

🛑 **Checkpoint:** `ls members/` shows your file **and** the files of everyone who has merged so far. Run `git log --oneline -5` and find your commit message on `main`.

---

# Round 2: Merge Conflict

## Part 7. Both Partners Edit the Same Line (5 min)

Open `team-board.md`. Find your pair's section:

```markdown
## Pair 1

Members: (add your name here)
```

Both of you are going to change **that same `Members:` line**, each writing only your own name. Git won't be able to combine them automatically. That's the point.

### 7.1 Create an Issue and a branch (both partners)

Create an Issue: title `Add Amy to team board, Pair 1`, assign yourself, note the number.

```bash
git switch main
git pull
git switch -c feat/amy-team-board
```

### 7.2 Edit the line (both partners)

Change your pair's line to contain **only your own name**:

```markdown
Members: Amy
```

**Only edit your own pair's section.** Save, then:

```bash
git add team-board.md
git commit -m "feat: add amy to team board"
git push -u origin feat/amy-team-board
```

### 7.3 Open a PR and request review (both partners)

Same as Part 4: title `feat: add amy to team board`, `Closes #N`, reviewer = your partner.

🛑 **Checkpoint:** both partners have an open PR with green checks. Neither shows a conflict yet, because each one by itself is fine.

---

## Part 8. First Partner Merges (2 min)

1. **Second** partner: review and **Approve** the **first** partner's PR (no comment round needed this time).
2. **First** partner: **Squash and merge**.

Now **second** partner, refresh your PR page.

🛑 **Checkpoint (second partner):** your PR now says **"This branch has conflicts that must be resolved"**. The merge button is blocked. 🎉 This is exactly what we wanted.

First partner: clean up as in [6.2](#62-clean-up-locally), then help your partner through Part 9.

---

## Part 9. Second Partner Resolves the Conflict (7 min)

Follow SOP [Section 8](SOP.md#8-merge-conflicts). Here it is for this case:

### 9.1 Bring the latest main into your branch

```bash
git switch main
git pull
git switch feat/amy-team-board
git merge main
```

Expected:

```
Auto-merging team-board.md
CONFLICT (content): Merge conflict in team-board.md
Automatic merge failed; fix conflicts and then commit the result.
```

### 9.2 Look at the conflict

Open `team-board.md` in VS Code. In your pair's section you'll see:

```
<<<<<<< HEAD
Members: Amy
=======
Members: Kevin
>>>>>>> main
```

- **Top (HEAD / Current):** your version.
- **Bottom (main / Incoming):** your partner's version, already merged.

### 9.3 Fix it: keep **both** names

Edit the block so it becomes exactly one line, with **no marker lines left**:

```markdown
Members: Kevin, Amy
```

(VS Code's **Accept Both** puts both lines in; then combine them into one line by hand.)

### 9.4 Finish the merge

```bash
git status            # team-board.md listed under "Unmerged paths"
git add team-board.md
git commit            # an editor opens with "Merge branch 'main'..."; save and close it
git push
```

🛑 **Checkpoint:** refresh your PR. The conflict message is gone and checks are running.

> **CI catches mistakes:** if you accidentally left a `<<<<<<<`, `=======`, or `>>>>>>>` line in the file, the check turns ❌ red and tells you the file and line. Remove it, commit, push again.

### 9.5 Get approval and merge

1. **First** partner: review the PR. In **Files changed**, confirm the line has **both** names and no markers. **Approve**.
2. **Second** partner: **Squash and merge**, then clean up as in [6.2](#62-clean-up-locally).

**Trio:** the third person now has a conflict too, because `main` changed again. Repeat Part 9; the final line has all three names.

🛑 **Final checkpoint (everyone):**

```bash
git switch main
git pull
```

Open `team-board.md`: your pair's line lists **all** of you, and there are no conflict markers anywhere in the file.

---

## Part 10. Wrap-Up (3 min)

You just did the whole workflow:

1. Issue → assign yourself
2. `git switch main && git pull` → `git switch -c <branch>`
3. Edit → `git add` → `git commit` → `git push`
4. Open PR → request review → respond to comments → get approval
5. Squash and merge → clean up
6. When there's a conflict: `git merge main` → fix → `git add` → `git commit` → `git push`

From now on, every change to our real project follows these same steps. When something goes wrong, check SOP [Section 10](SOP.md#10-troubleshooting-oh-no-i) first, then ask.
