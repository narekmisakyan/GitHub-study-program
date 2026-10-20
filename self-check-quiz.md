# Self-check quiz

Twenty questions on Days 1–2, eight on Day 3. Answer in your head, then
expand. Aim for 16+ on the first twenty before calling the course done.

## Days 1–2

### 1. Git vs. GitHub?
<details><summary>Answer</summary>
Git is the version control program on your computer. GitHub is a website that hosts Git repositories and adds pull requests, issues, reviews and Actions. Git works without GitHub.
</details>

### 2. What does `git status` change?
<details><summary>Answer</summary>
Nothing. It only reports. The Source Control view is a live version of it.
</details>

### 3. Why are `add` and `commit` separate steps?
<details><summary>Answer</summary>
So you choose which changes go into a commit. The staging area lets you make several focused commits from one editing session.
</details>

### 4. You edited `a.py` and `b.py`. How do you commit only `a.py`?
<details><summary>Answer</summary>
Stage only `a.py` (`git add a.py` or the **+** on that file), then commit. Not `git add .` or Stage All.
</details>

### 5. What does `git commit -am "msg"` leave out?
<details><summary>Answer</summary>
New, untracked files. `-a` stages only modifications and deletions of tracked files.
</details>

### 6. What is `HEAD`?
<details><summary>Answer</summary>
The commit you are currently on, normally the tip of the current branch.
</details>

### 7. Discard uncommitted edits to `notes.txt`.
<details><summary>Answer</summary>
`git restore notes.txt` or **↶ Discard Changes**. Irreversible.
</details>

### 8. Committed too early; want to add more before pushing.
<details><summary>Answer</summary>
Stage the extra changes, then `git commit --amend --no-edit`. Or **Undo Last Commit** and commit again. Unpushed commits only.
</details>

### 9. `git reset --soft HEAD~1` vs. `--hard`?
<details><summary>Answer</summary>
Both remove the last commit from the branch. `--soft` keeps its changes staged; `--hard` deletes them from disk too.
</details>

### 10. A bad commit was pushed to `main` two days ago.
<details><summary>Answer</summary>
`git revert <id>`: a new commit that undoes it. Never rewrite shared history.
</details>

### 11. What is a branch, physically?
<details><summary>Answer</summary>
A name pointing to a commit. Committing on it moves the pointer. Creating one copies nothing.
</details>

### 12. What is a fast-forward merge?
<details><summary>Answer</summary>
The target branch had no new commits, so Git moves its pointer to the merged branch's tip. No merge commit.
</details>

### 13. You see `<<<<<<< HEAD` in a file.
<details><summary>Answer</summary>
A merge conflict. Edit to the version you want (or Accept Current / Incoming / merge editor), remove the markers, stage, commit. `git merge --abort` cancels.
</details>

### 14. `git push` rejected with "fetch first".
<details><summary>Answer</summary>
The remote has commits you lack. Pull (resolve any conflict), then push. In VS Code, Sync does both.
</details>

### 15. What does `git pull` actually do?
<details><summary>Answer</summary>
`git fetch` then `git merge` of the remote branch into your current branch.
</details>

### 16. What is `origin`?
<details><summary>Answer</summary>
The conventional name of your default remote, usually the GitHub copy.
</details>

### 17. When must you never force-push?
<details><summary>Answer</summary>
On a shared branch such as `main`, or any branch others may have pulled. `--force-with-lease` on your own unshared feature branch after an amend is acceptable.
</details>

### 18. What is a pull request?
<details><summary>Answer</summary>
A GitHub request to merge one branch into another, with description, diff, review and a merge button. Not a Git command.
</details>

### 19. Why do notebooks cause merge conflicts, and the fix?
<details><summary>Answer</summary>
`.ipynb` files store outputs and execution counts as JSON that changes on every run. Strip outputs on commit with `nbstripout --install`, and move reusable code into `.py` files.
</details>

### 20. Four things that belong in a Python project's `.gitignore`.
<details><summary>Answer</summary>
Any four of `.ipynb_checkpoints/`, `__pycache__/`, `*.pyc`, `.venv/`, `.env`, `data/`, `*.csv`, `.DS_Store`, `.vscode/`.
</details>

### Bonus. You ran `git reset --hard` and a commit vanished. Gone?
<details><summary>Answer</summary>
No. `git reflog` lists where HEAD has been; `git reset --hard <id>` or `git switch -c rescue <id>`. Only never-committed work is truly lost.
</details>

## Day 3

### 21. What does Copilot's ✨ in the commit box do, and what should you add?
<details><summary>Answer</summary>
It drafts a message from the staged diff (the *what*). You add the *why* and check the subject is imperative and short.
</details>

### 22. Ask, Edit, Agent: which one runs terminal commands?
<details><summary>Answer</summary>
Agent mode, and it asks for confirmation before each command. Ask changes nothing; Edit changes the files you point it at.
</details>

### 23. How do you review what Agent mode did before it becomes permanent?
<details><summary>Answer</summary>
Its edits are ordinary uncommitted changes: read each diff in the Source Control view, discard what you dislike, stage and commit the rest. Do it on a branch.
</details>

### 24. Where do repo-wide rules for Copilot live?
<details><summary>Answer</summary>
`.github/copilot-instructions.md` (Copilot-specific) and/or `AGENTS.md` at the root (shared by many tools). `/init` scaffolds the former.
</details>

### 25. What does the Copilot cloud agent produce when you assign it an issue?
<details><summary>Answer</summary>
A branch named `copilot/…`, commits, and a draft pull request linked to the issue, with a session log. It then requests your review.
</details>

### 26. Why do test workflows not run on the agent's PR at first?
<details><summary>Answer</summary>
Its PRs are treated like an outside contributor's: a person with write access must press **Approve and run workflows**. Admins can disable this.
</details>

### 27. Three ruleset settings that stop anything merging without you.
<details><summary>Answer</summary>
Require a pull request before merging; require at least one approval (the requester cannot approve the agent's PR); require status checks to pass. Also block force pushes.
</details>

### 28. `cron: "15 6 * * 1-5"` means?
<details><summary>Answer</summary>
06:15 UTC, Monday to Friday. Add `workflow_dispatch` so you can also run it by hand; remember runs can start late and only fire from the default branch.
</details>
