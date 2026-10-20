# VS Code for Git — cheat sheet

Every Git task as a click path or shortcut. Shortcuts are Windows/Linux ·
Mac.

## Getting around

| Want | Shortcut / where |
|------|------------------|
| Source Control view | `Ctrl+Shift+G` · `⌃⇧G` |
| Command Palette (type any Git command) | `Ctrl+Shift+P` · `⇧⌘P`, then type `Git:` |
| Integrated terminal | ``Ctrl+` `` · ``⌃` `` |
| Explorer | `Ctrl+Shift+E` · `⇧⌘E` |
| Copilot Chat | `Ctrl+Alt+I` · `⌃⌘I` |
| Inline chat in the editor | `Ctrl+I` · `⌘I` |
| Quick open a file | `Ctrl+P` · `⌘P` |
| Markdown preview (for this course) | `Ctrl+Shift+V` · `⇧⌘V` |

## The Source Control view

```
SOURCE CONTROL                 ✓ Commit  ⌄   ↻  …
┌──────────────────────────────────────────────┐
│ ✨  Message (Ctrl+Enter to commit on "main") │
└──────────────────────────────────────────────┘
▾ Staged Changes                            −  2
    cleankit.py                      M
    README.md                        M
▾ Changes                                   ↶ + 3
    notebooks/01-demo.ipynb          M
    data/sample.csv                  U
    old.py                           D
▾ Graph
    ● Add dedupe helper            main  origin/main
    ● Merge branch 'robust-float'
    ● Skip unconvertible values     robust-float
```

| Badge | Meaning |
|-------|---------|
| `U` | untracked (new) |
| `M` | modified |
| `A` | added (staged new file) |
| `D` | deleted |
| `C` | conflict |

## Task → terminal → VS Code

| Task | ⌨️ Terminal | 🖱️ VS Code |
|------|------------|-----------|
| make a repo | `git init` | **Initialize Repository** |
| see state | `git status` | the view itself |
| stage one file / all | `git add f` / `git add .` | **+** on the file / on *Changes* |
| unstage | `git restore --staged f` | **−** |
| discard edits | `git restore f` | **↶ Discard Changes** |
| see a diff | `git diff` / `--staged` | click the file |
| commit | `git commit -m "…"` | message → **✓ Commit** or `Ctrl+Enter` |
| commit message by Copilot | — | **✨** in the message box |
| history | `git log --oneline --graph` | **Graph** section |
| file history | `git log -p f` | **Timeline** (bottom of Explorer) |
| who changed a line | `git blame f` | hover the line (built-in blame) or GitLens |
| undo last commit, keep changes | `git reset --soft HEAD~1` | **…** → Commit → **Undo Last Commit** |
| stash / pop | `git stash` / `git stash pop` | **…** → Stash → Stash / Pop Latest Stash |
| create branch | `git switch -c name` | status bar branch → **Create new branch…** |
| switch branch | `git switch name` | status bar branch → pick |
| merge | `git merge name` | Command Palette → **Git: Merge…** |
| abort merge | `git merge --abort` | **…** → Branch → Abort Merge |
| delete branch | `git branch -d name` | **Git: Delete Branch…** |
| rename branch | `git branch -m new` | **Git: Rename Branch…** |
| publish repo | `git remote add origin …` + `git push -u origin main` | **Publish to GitHub** |
| publish branch | `git push -u origin name` | **Publish Branch** (cloud icon) |
| pull + push | `git pull && git push` | **↻ Sync Changes** |
| fetch | `git fetch` | **…** → Fetch |
| clone | `git clone url` | **Git: Clone** → Clone from GitHub |
| checkout old commit | `git checkout id` | Graph → right-click → Checkout |
| tag | `git tag -a v1 -m "…"` | **…** → Tags → Create Tag |
| open PR | `gh pr create` | **Create Pull Request** (GitHub Pull Requests extension) |
| review / check out a PR | `gh pr checkout 12` | GitHub view → the PR → **Checkout** |

## Merge conflicts

When a merge or pull conflicts, files appear under **Merge Changes**. Open one:

```
  Accept Current Change | Accept Incoming Change | Accept Both Changes | Compare Changes
<<<<<<< HEAD (Current Change)        ← what is on your branch
...
=======
...                                  ← what is coming in
>>>>>>> other-branch (Incoming Change)
                                                     [ Resolve in Merge Editor ]
```

- Small conflict: click one of the **Accept** actions, or edit by hand and remove the markers.
- Larger conflict: **Resolve in Merge Editor** → tick *Accept Incoming* / *Accept Current* / both per block, or type in the *Result* pane → **Complete Merge**.
- Then **+** stage the file and **✓ Commit**.
- Lost? **…** → Branch → **Abort Merge** puts everything back.
- 🤖 Select the block and ask Copilot Chat to explain the two sides; newer versions add a *Resolve Merge Conflict with AI* action.

## Notebooks

- Click a changed `.ipynb` in Source Control for a cell-by-cell diff.
- Settings → search `notebook.diff` → tick **Ignore Outputs** and **Ignore Metadata**.

## Useful settings

| Setting | Why |
|---------|-----|
| `git.autofetch: true` | the status bar shows incoming commits without you asking |
| `git.enableSmartCommit: true` | ✓ Commit with nothing staged commits all changes (read the list first) |
| `git.confirmSync: false` | stop the confirmation dialog on Sync |
| `scm.graph.showIncomingChanges` / `showOutgoingChanges` | mark commits to pull / push in the Graph |
| `notebook.diff.ignoreOutputs: true` | notebook diffs show code only |

## Extensions

| Extension | Adds |
|-----------|------|
| **GitHub Pull Requests** | PR creation, review, checkout, issues; ✨ PR descriptions |
| **GitHub Copilot** (+ Chat) | completions, chat, agent mode, ✨ commit messages |
| **GitLens** | rich blame, history, commit search; optional |
| **Git Graph** | an alternative graph view; optional |
| **Markdown Preview Mermaid Support** | renders the diagrams in this course |
