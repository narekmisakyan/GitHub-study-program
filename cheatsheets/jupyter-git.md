# Jupyter + Git — cheat sheet

## One-time setup per machine

```bash
pip install nbstripout nbdime
nbdime config-git --enable --global      # readable notebook diffs and merges everywhere
```

## One-time setup per repository

```bash
cd my-repo
nbstripout --install                     # strip outputs on commit
git add .gitattributes
git commit -m "Strip notebook outputs on commit"
```

Verify: `nbstripout --is-installed && echo yes`.

## VS Code

- Click a changed `.ipynb` in Source Control for a cell-by-cell diff.
- Settings: `notebook.diff.ignoreOutputs: true`, `notebook.diff.ignoreMetadata: true`.
- Run Git from the integrated terminal (``Ctrl+` ``) or the Source Control view; `!git status` in a cell works for a quick look.

## Minimal `.gitignore` for a Python/Jupyter project

```
.ipynb_checkpoints/
__pycache__/
*.pyc
.venv/
venv/
.env
data/
*.csv
*.parquet
*.pkl
.DS_Store
.vscode/
.idea/
```

## Diffs and merges

```bash
git diff notebook.ipynb                 # cell-based once nbdime is enabled
nbdiff notebook.ipynb                   # terminal
nbdiff-web notebook.ipynb               # side-by-side in the browser
nbdiff-web <id1> <id2> notebook.ipynb   # between two commits
nbmerge-web notebook.ipynb              # resolve a conflicted notebook visually
```

One side of a conflicted notebook without tools:

```bash
git checkout --ours notebook.ipynb      # or --theirs
git add notebook.ipynb
```

## Rules of thumb

- **Outputs out, code in.** Commit code; regenerate outputs. Deliverable outputs: export HTML/PDF and commit that.
- **No nbstripout?** *Kernel → Restart Kernel and Clear All Outputs* before a real commit.
- **Number notebooks**: `01-load.ipynb`, `02-clean.ipynb`.
- **Reusable functions go to `.py` files**; `%load_ext autoreload` + `%autoreload 2` in the first cell.
- **Data stays out of Git.** `data/README.md` says where it comes from, or `download_data.py`. Large versioned files: Git LFS or DVC.
- **Secrets never in notebooks.** `os.environ["API_KEY"]` with a `.env` loaded by `python-dotenv`; ignore `.env`.
- **Commit the environment**: `requirements.txt` or `environment.yml`.
- **Check before committing**: `git diff --staged --stat`. A notebook at +50,000 lines means outputs slipped in.

## Notebooks in CI (GitHub Actions)

```yaml
- run: pip install jupyter nbconvert
- run: jupyter nbconvert --to html --execute notebooks/report.ipynb --output-dir out
- uses: actions/upload-artifact@v4
  with:
    name: report
    path: out/
```

`papermill in.ipynb out.ipynb -p date 2026-10-20` executes with parameters.

## Reviewing notebooks on GitHub

- GitHub renders `.ipynb` with outputs if present.
- PR *Files changed*: toggle **rich diff** at the top right of a notebook.
- Renderer gives up on huge notebooks; https://nbviewer.org renders any public notebook URL.

## Jupytext (optional)

```bash
pip install jupytext
jupytext --set-formats ipynb,py:percent notebook.ipynb
```

Commit the `.py`, ignore `*.ipynb`; Jupyter keeps both in sync. VS Code opens
`# %%` files as notebooks.
