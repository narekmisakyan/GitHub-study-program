# Glossary

Alphabetical. Words in *italics* have their own entry.

**add** — Put a file's changes into the *staging area*. `git add file` · 🖱️ **+**.

**agent (AI)** — Software that plans and performs multi-step work: in VS Code, Copilot's *Agent mode* edits files and runs commands; the *Copilot cloud agent* works on GitHub's servers and opens pull requests.

**Agent HQ / mission control** — GitHub's place to run and monitor several coding agents (Copilot, Claude, Codex…) from GitHub, VS Code and the CLI.

**AGENTS.md** — A plain-text instructions file at the repo root read by many AI tools. Copilot also reads `.github/copilot-instructions.md`.

**amend** — Replace the most recent commit with a corrected one. Unpushed commits only.

**artifact (Actions)** — Files saved from a workflow run, downloadable from the run page.

**base branch** — In a *pull request*, the branch you want to merge into, usually `main`.

**blame** — Show who last changed each line of a file.

**branch** — A named pointer to a commit; an independent line of development. Instant to create.

**checkout** — Older command for switching branches or restoring files; now `switch` and `restore`. Still used to view an old commit.

**CI (continuous integration)** — Automated checks, usually tests, run on every push or PR. On GitHub: *GitHub Actions*.

**clone** — Copy a remote repository, with its history, to your machine.

**CODEOWNERS** — A file naming who must review changes to which paths.

**commit** — (noun) A snapshot of all tracked files plus author, date and message, identified by a *hash*. (verb) To create one.

**conflict** — A merge where the same lines changed differently on both sides. Marked with `<<<<<<<`, `=======`, `>>>>>>>`.

**Copilot cloud agent** — GitHub's hosted agent (called the *coding agent* until 2026). Assign it an issue; it works on a `copilot/…` branch and opens a draft PR.

**copilot-instructions.md** — `.github/copilot-instructions.md`: project rules that Copilot applies to every request in that repo.

**copilot-setup-steps.yml** — A workflow whose job named `copilot-setup-steps` preinstalls dependencies for the cloud agent.

**cron** — Five-field schedule syntax (`minute hour day month weekday`) used by `on: schedule` in Actions. UTC.

**default branch** — The branch GitHub shows first and PRs target by default; usually `main`.

**detached HEAD** — *HEAD* points at a commit instead of a branch, usually after checking out an old commit. Make a branch before committing there.

**diff** — Line-by-line difference between two versions; `-` removed, `+` added.

**draft pull request** — A PR marked not ready for review. The cloud agent opens its PRs as drafts.

**fast-forward** — A merge where the target branch simply moves forward; no merge commit.

**fetch** — Download new commits from a *remote* without merging.

**fork** — Your own copy of someone else's repository on GitHub, so you can push to it and open PRs against the original.

**GITHUB_TOKEN** — The automatic credential a workflow gets; repo-scoped, limited (cannot assign Copilot).

**GitHub Actions** — GitHub's automation system: *workflows* triggered by events or schedules.

**GitHub Agentic Workflows (gh-aw)** — Experimental tool that compiles Markdown task descriptions into Actions workflows that run an AI agent.

**`.gitignore`** — Patterns of files Git must not track.

**hash / SHA** — The 40-character ID of a commit; usually abbreviated to 7.

**HEAD** — The commit you are on; `HEAD~1` is its parent.

**index** — Another name for the *staging area*.

**instructions file** — See *copilot-instructions.md*, *AGENTS.md*; also `.github/instructions/*.instructions.md` scoped with `applyTo`.

**issue** — A GitHub thread for a bug, task or question, numbered `#N`. Can be assigned to a person or to Copilot.

**main** — Conventional name of the primary branch (formerly `master`).

**MCP (Model Context Protocol)** — A standard for giving agents tools such as databases or APIs.

**merge** — Combine one branch's commits into another; may create a *merge commit*.

**merge editor** — VS Code's three-pane view (incoming · current · result) for resolving conflicts.

**origin** — Conventional name of your main *remote*.

**premium requests / AI credits** — The metered units Copilot plans use for chat, agent and cloud-agent usage.

**prompt file** — `.github/prompts/name.prompt.md`, a reusable task run with `/name` in Copilot Chat.

**pull** — `fetch` + `merge`: get remote commits into your branch. 🖱️ part of **Sync**.

**pull request (PR)** — A GitHub request to merge a branch, with diff, review and discussion.

**push** — Send your commits to a remote. 🖱️ part of **Sync**.

**rebase** — Replay a branch's commits on top of another branch. Rewrites commits; avoid on shared branches.

**reflog** — Git's log of everywhere HEAD has been; the recovery tool.

**remote** — A named link to another copy of the repo, e.g. `origin`, `upstream`.

**repository / repo** — A folder tracked by Git, containing `.git`.

**reset** — Move the branch pointer to another commit; `--soft` keeps changes staged, `--hard` discards them.

**restore** — Put a file back to a previous state: discard edits, unstage (`--staged`), or an old version (`--source`).

**revert** — A new commit that undoes an earlier one. Safe for shared history.

**ruleset / branch protection** — Repository rules such as *require a pull request*, *require approvals*, *require status checks*, *block force pushes*.

**runner** — The virtual machine a workflow job runs on, e.g. `ubuntu-latest`.

**squash** — Combine several commits into one, typically when merging a PR.

**staging area** — Changes queued for the next commit. Filled by `add`, emptied by `commit`.

**stash** — Temporarily shelve uncommitted changes.

**status check** — A workflow result shown on a PR; can be made required before merging.

**switch** — Change the current branch; `-c` creates one.

**Sync** — VS Code's button that pulls then pushes.

**tag** — A permanent label on a commit, used for versions like `v1.0`.

**Timeline** — VS Code view showing the history of the open file.

**tracked / untracked** — Tracked: Git has the file in a commit or the staging area. Untracked: exists on disk, unknown to Git.

**upstream** — (1) The remote branch a local branch is linked to. (2) The original repo when you work on a fork.

**workflow** — A YAML file in `.github/workflows/` describing jobs to run on given triggers.

**workflow_dispatch** — A trigger that adds a manual *Run workflow* button.

**working directory / working tree** — The actual files on disk that you edit.
