# GitHub Actions — cheat sheet

Workflows live in `.github/workflows/*.yml` and run on GitHub's machines.

## Skeleton

```yaml
name: Tests                      # shown in the Actions tab
on:                              # triggers
  pull_request:
  push:
    branches: [main]
  schedule:
    - cron: "15 6 * * 1-5"       # UTC
  workflow_dispatch:             # manual "Run workflow" button
permissions:
  contents: read                 # least privilege for GITHUB_TOKEN
jobs:
  test:                          # job id (used as the status-check name)
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.12"
      - run: pip install -r requirements.txt pytest
      - run: pytest
```

## Triggers

| `on:` | Fires when |
|-------|-----------|
| `push` | commits pushed (filter with `branches:`, `paths:`) |
| `pull_request` | PR opened, updated, reopened |
| `schedule` | cron timer, UTC, default branch only |
| `workflow_dispatch` | you press *Run workflow* (can take `inputs:`) |
| `issues` / `issue_comment` | issue opened, labelled, commented… |
| `release` | a release is published |

## Cron

```
minute hour day-of-month month day-of-week
  *     *        *         *        *
 0-59  0-23     1-31      1-12     0-6 (Sun=0)
```

| Expression | Meaning |
|------------|---------|
| `*/15 * * * *` | every 15 minutes |
| `0 * * * *` | hourly |
| `0 6 * * *` | daily 06:00 UTC |
| `15 6 * * 1-5` | weekdays 06:15 UTC |
| `0 2 * * 0` | Sundays 02:00 UTC |
| `0 9 1 * *` | first of the month 09:00 UTC |

Gotchas: UTC only · may start late · minimum 5 min · default branch only ·
disabled after 60 days of inactivity in public repos · always add
`workflow_dispatch` for testing.

## Common steps

| Step | Does |
|------|------|
| `actions/checkout@v4` | clone the repo into the runner |
| `actions/setup-python@v5` | install Python; add `cache: pip` to cache packages |
| `actions/upload-artifact@v4` | save files from the run (downloadable for 90 days) |
| `actions/download-artifact@v4` | fetch them in a later job |
| `run: jupyter nbconvert --to html --execute nb.ipynb` | execute a notebook |
| `run: papermill in.ipynb out.ipynb -p param value` | execute with parameters |

## Secrets and variables

- *Settings → Secrets and variables → Actions*: `secrets.NAME` (hidden) and `vars.NAME` (plain).
- Use: `env: API_KEY: ${{ secrets.API_KEY }}`.
- `GITHUB_TOKEN` is automatic, scoped to the repo, and cannot assign Copilot or trigger other workflows; use a fine-grained PAT secret for those.

## Useful expressions

| Expression | Value |
|------------|-------|
| `${{ github.repository }}` | `owner/repo` |
| `${{ github.ref_name }}` | branch or tag name |
| `${{ github.event_name }}` | which trigger fired |
| `${{ github.actor }}` | who triggered it |
| `if: github.event_name == 'schedule'` | run a step only on the timer |

## Commit from a workflow

```yaml
permissions:
  contents: write
steps:
  - uses: actions/checkout@v4
  - run: |
      git config user.name "github-actions[bot]"
      git config user.email "github-actions[bot]@users.noreply.github.com"
      git add out/ && git commit -m "Update report" || echo "nothing to commit"
      git push
```

## Reading results

- **Actions** tab → the run → the job → expand a step for its log.
- PR page → *Checks* → *Details*.
- Red cross on a commit = a workflow failed on it.
- Re-run: *Re-run jobs* on the run page.

## Making a check required

*Settings → Rules → Rulesets → New branch ruleset* for `main` → **Require
status checks to pass** → add the job name (`test`).

## Agents and Actions

- Workflows on the Copilot cloud agent's PRs wait for **Approve and run workflows** by default.
- `copilot-setup-steps.yml` (job named `copilot-setup-steps`) preinstalls dependencies for the agent.
- GitHub Agentic Workflows (`gh-aw`) compiles Markdown tasks into `.lock.yml` workflows that run an agent on a schedule.

## Debugging

- Validate YAML indentation first; most failures are indentation.
- Add `- run: env | sort` or `- run: ls -R` steps to see what the runner has.
- Set repository secret `ACTIONS_STEP_DEBUG=true` for verbose logs.
- `act` (https://github.com/nektos/act) runs workflows locally with Docker.
