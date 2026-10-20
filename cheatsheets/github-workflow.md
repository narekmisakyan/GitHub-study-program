# GitHub workflow — cheat sheet

Step-by-step recipes. Assumes VS Code is signed in to GitHub, or you ran
`gh auth login`.

## Put an existing local project on GitHub

🖱️ Source Control → **Publish to GitHub** → public/private. Done.

⌨️ Create an **empty** repo on github.com (no README, licence or `.gitignore`), then:

```bash
git remote add origin https://github.com/USER/project.git
git push -u origin main
```

## Start a new project from GitHub

**+ → New repository** → tick *Add a README*, pick the Python `.gitignore`
and a licence → **Git: Clone** in VS Code (or `git clone <url>`).

## Daily sync

```bash
git pull        # start of session        🖱️ ↻ Sync
# work, commit
git push        # end of session          🖱️ ↻ Sync
```

## Push rejected ("fetch first" / "non-fast-forward")

```bash
git pull        # merge their work; resolve conflicts; commit
git push
```

## The pull request recipe

```bash
git switch main && git pull                 # fresh main
git switch -c short-descriptive-name        # branch
# edit, add, commit (several commits are fine)
git push -u origin short-descriptive-name   # 🖱️ Publish Branch
```

Then **Create Pull Request** in VS Code (✨ drafts title and description)
or *Compare & pull request* on GitHub. Put `Closes #N` in the description.

| During review | After merge |
|---------------|-------------|
| commit + push on the same branch; the PR updates | GitHub: **Delete branch**. Locally: `git switch main && git pull && git branch -d name && git fetch --prune` |

## PR has conflicts

```bash
git switch my-branch
git pull origin main         # merge latest main in; resolve; commit
git push
```

## Fork workflow (no push rights)

1. **Fork** on their page.
2. `git clone https://github.com/YOU/repo.git && cd repo`
3. `git remote add upstream https://github.com/THEM/repo.git`
4. branch, commit, `git push -u origin my-fix`, open PR with base = their `main`.
5. Refresh: `git switch main && git fetch upstream && git merge upstream/main && git push`

## Issues

- New issue: title, description, labels → gets `#N`.
- In commits/PRs: `See #N`; auto-close with `Closes #N`, `Fixes #N`, `Resolves #N`.
- 🤖 Assign to **Copilot** to have the cloud agent open a PR for it.

## Copilot on GitHub

| Want | How |
|------|-----|
| Copilot review of a PR | PR → Reviewers → **Copilot** |
| automatic Copilot review on every PR | Settings → Rules → ruleset → *Request pull request review from Copilot* |
| PR title/description draft | ✨ in the PR form (web or VS Code) |
| delegate an issue | Assignees → Copilot; or github.com/copilot/agents |
| iterate on the agent's PR | comment mentioning `@copilot` |
| let its tests run | **Approve and run workflows** on the PR |

## Release a version

```bash
git tag -a v1.2.0 -m "Describe the release"
git push origin v1.2.0
```

GitHub → *Releases → Draft a new release* → pick the tag → notes → **Publish**.

## Authentication

| Method | Setup |
|--------|-------|
| VS Code | Accounts icon → Sign in with GitHub (enough for the Source Control view) |
| GitHub CLI | install `gh`, `gh auth login` (configures Git too) |
| SSH | `ssh-keygen -t ed25519 -C "email"`, paste `~/.ssh/id_ed25519.pub` into Settings → SSH keys, use `git@github.com:USER/repo.git` URLs |

Switch a clone between HTTPS and SSH: `git remote set-url origin git@github.com:USER/repo.git`

## `gh` CLI quick reference

```bash
gh repo create project --private --source=. --push    # create from current folder and push
gh repo clone OWNER/repo
gh pr create --fill                                    # PR from current branch
gh pr list
gh pr checkout 42                                      # PR #42 locally
gh pr merge 42 --squash --delete-branch
gh issue create --title "Bug" --body "Details"
gh issue list
gh issue edit 12 --add-assignee "@copilot"             # hand it to the cloud agent (verify it stuck)
gh run list                                            # Actions runs
gh run watch                                           # follow the current run
gh browse                                              # open the repo in the browser
```

## Where things are on a repo page

| Want | Where |
|------|-------|
| clone URL | green **Code** button |
| history | *commits* link |
| switch branch | branch dropdown |
| edit in browser | pencil on a file (creates a commit) |
| file history / blame | open file → *History* / *Blame* |
| CI results | *Actions* tab; ✓ / ✗ beside commits |
| rules, collaborators, secrets, delete | *Settings* tab |
| browser VS Code | press `.` anywhere in the repo |

## Markdown for READMEs, issues and PRs

````markdown
# Heading 1
## Heading 2
**bold**  *italic*  `inline code`
- bullet
1. numbered
[link text](https://url)
![image alt](path/to/image.png)
> quote
> [!TIP]
> a callout (also NOTE, IMPORTANT, WARNING, CAUTION)
- [ ] task not done
- [x] task done

```python
code block
```
````

Tables: `| a | b |` rows with a `|---|---|` line under the header.
