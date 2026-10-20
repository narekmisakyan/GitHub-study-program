# Copilot & agents — cheat sheet

## Copilot in VS Code

| Action | How |
|--------|-----|
| accept a completion / next word | `Tab` / `Ctrl+→` · `⌘→` |
| dismiss a completion | `Esc` |
| open Chat | `Ctrl+Alt+I` · `⌃⌘I` |
| inline chat on selected code | `Ctrl+I` · `⌘I` |
| switch mode | dropdown at the bottom of Chat: **Ask · Edit · Agent** |
| pick a model | model dropdown next to the mode |
| generate commit message | **✨** in the Source Control message box |
| generate PR title + description | **✨** in the Create Pull Request view |
| explain a merge conflict | select the block → Chat → *"explain both sides"* |

### Modes

| Mode | Changes files? | Runs commands? | Use for |
|------|----------------|----------------|---------|
| Ask | no | no | questions, explanations, drafts you paste yourself |
| Edit | yes, where you point it | no | targeted edits across a few files |
| Agent | yes, anywhere | yes, asks first | multi-step tasks with tests |

### Context variables and commands

| Syntax | Means |
|--------|-------|
| `#file` | a specific file (type `#` and pick) |
| `#selection` | the selected text |
| `#changes` | your uncommitted changes |
| `#codebase` | search the whole repo for relevant code |
| `@workspace` | questions about the whole project |
| `/explain` `/fix` `/tests` `/doc` | slash commands on the selection |
| `/init` | scaffold `.github/copilot-instructions.md` |
| `/name` | run `.github/prompts/name.prompt.md` |
| `#copilotCodingAgent` | hand the task to the cloud agent (opens a PR) |

### Recipes

```
Review #changes for bugs, edge cases and missing tests. Cite line numbers.
Write a commit message for #changes: imperative subject, body explaining why.
Explain what #file changes and the risk of this change.
Summarise this week's work from the pasted git log --oneline output.
Add pytest tests for every function in #file and run them.
```

## Customisation files

| File | Scope | Notes |
|------|-------|-------|
| `.github/copilot-instructions.md` | whole repo, always applied | run `/init` to scaffold |
| `AGENTS.md` (root) | whole repo, read by many tools | Copilot CLI, Codex, Claude Code… |
| `.github/instructions/*.instructions.md` | files matching `applyTo` front matter | e.g. `applyTo: "notebooks/**"` |
| `.github/prompts/*.prompt.md` | on demand via `/name` | reusable tasks |
| `.github/agents/*.agent.md` | a custom agent in the Chat dropdown | front matter: `description`, `tools`, `model` |

Minimal instructions file:

```markdown
# Copilot instructions
- Python 3.12; install with `pip install -r requirements.txt`; test with `pytest`.
- Pure functions with type hints and docstrings. Reusable code lives in src/, not notebooks.
- Never commit data, notebook outputs, or .env.
- Keep PRs small; ask in the PR when unsure.
```

## Copilot CLI (terminal agent)

```bash
copilot                    # start inside a repo
/model                     # switch model
& <task>                   # delegate to the cloud agent and keep working
```

## Copilot cloud agent (formerly coding agent)

| Start from | How |
|------------|-----|
| issue | Assignees → **Copilot** |
| Agents page | github.com/copilot/agents |
| VS Code Chat | task + `#copilotCodingAgent` or *Delegate to coding agent* |
| Copilot CLI | `& task` |
| GitHub CLI | `gh issue edit N --add-assignee "@copilot"` (verify it stuck) |

What it does: 👀 on the issue → branch `copilot/…` → draft PR → requests
your review. Iterate with PR comments mentioning `@copilot`. It reads the
issue as it was when assigned; later instructions go on the PR.

**Task template**

```markdown
## Goal
## Acceptance criteria (checkboxes, testable)
## Constraints (no new deps, do not touch X)
```

**Review checklist**

- [ ] only what the issue asked
- [ ] tests exist and pass (approve the workflows first)
- [ ] no data/config/workflow files touched unexpectedly
- [ ] new dependencies are real and needed
- [ ] commit messages and PR description make sense
- [ ] checked out and run locally

**Repo setup**

- `.github/copilot-instructions.md`
- `.github/workflows/copilot-setup-steps.yml` with a job named `copilot-setup-steps` that installs dependencies
- ruleset on `main`: require PR · 1 approval (requester cannot approve) · status checks · block force pushes
- keep **Approve and run workflows** on

## Scheduling AI work

| Approach | Notes |
|----------|-------|
| Actions cron → `gh issue create` → assign Copilot | needs a fine-grained PAT as a secret; `GITHUB_TOKEN` cannot assign Copilot |
| GitHub Agentic Workflows (`gh-aw`) | task in Markdown + cron front matter; `gh aw init`, `gh aw add`, `gh aw compile`; experimental |
| Claude Code, Codex and others | scheduled tasks / GitHub Actions of their own; same branch → PR → review flow |

## Orchestration rules

1. One issue, one branch, one PR. Never two agents on one branch.
2. Small tasks, ten-minute reviews.
3. `main` protected; tests required; humans approve.
4. Dependencies between tasks go through issues, not shared branches.
5. Least-privilege tokens; watch the usage meter.
