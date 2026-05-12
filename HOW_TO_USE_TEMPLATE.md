# HOW_TO_USE_TEMPLATE.md — Complete Guide to This Research Workspace

---

## Overview: What This System Is

This workspace is an **AI-orchestrated research paper environment**. It structures the
collaboration between a researcher and Claude Code (an agentic AI) across the full lifecycle
of a technical paper: literature review → writing → simulation → verification → submission.

### The Three-Layer System

```
┌─────────────────────────────────────────────────────────────┐
│  LAYER 1: Foundation (YOU bring this — before starting)     │
│  Domain knowledge · Research gap · Methodology choice       │
│  Base paper reproduced · Target venue decided               │
└──────────────────────────┬──────────────────────────────────┘
                           │
┌──────────────────────────▼──────────────────────────────────┐
│  LAYER 2: Context (YOU fill in — first session)             │
│  context/domain.md · context/target.md · context/idea.md   │
│  context/notation.md · MASTER_PLAN.md                      │
└──────────────────────────┬──────────────────────────────────┘
                           │
┌──────────────────────────▼──────────────────────────────────┐
│  LAYER 3: Execution (Claude Code runs — every session)      │
│  skills/ → what Claude knows about your paper               │
│  workflows/ → what Claude does when invoked                  │
│  our-paper/ · lit-review/ · base-paper/ → the artifacts    │
└─────────────────────────────────────────────────────────────┘
```

**The system works because the context files give Claude a complete, always-current
picture of your paper. Without well-filled context files, the AI will produce generic output.**

---

## PREREQUISITES — Read Before Anything Else

> See `PREREQUISITES.md` for the full checklist.
> Summary: you must bring the following **yourself** before using this template.

### What You Must Have (non-negotiable)

- [ ] **Domain knowledge** — You can explain your field, the landscape of existing work,
      and what gap exists, without consulting an AI. The AI cannot find novelty for you.

- [ ] **Research gap identified** — Written in your own words: "No prior work has [X]."
      You must be able to defend this claim against the existing literature.

- [ ] **Methodology chosen** — You know whether this paper uses optimization, RL, deep
      learning, analytical bounds, or a hybrid. You understand why that approach works.

- [ ] **Base paper reproduced** — The most similar prior work's code runs on your machine
      and produces figures matching the paper. You understand its math end-to-end.

- [ ] **Target venue decided** — Venue, page limit, format, and deadline confirmed.

### What Claude Code CANNOT Do For You

- Identify research gaps or choose your novelty
- Read the domain literature autonomously
- Decide your methodology or system model
- Verify that your math is physically meaningful
- Replace peer review

---

## Step 0 — Verify Prerequisites

Before copying this template, open `PREREQUISITES.md` and check every box.
If any box is unchecked, **do not proceed** — do the domain work first.

---

## Step 1 — Copy the Template

```bash
cp -r ~/Documents/Code/full-pass-iscc/_template/ ~/Documents/Code/<new-paper-name>/
cd ~/Documents/Code/<new-paper-name>/
```

Replace `<new-paper-name>` with a short, lowercase, hyphen-separated identifier
(e.g., `ris-isac-2025`, `noma-rl-journal`, `mimo-crb-tvt`).

---

## Step 2 — Fill in MASTER_PLAN_template.md

Open `MASTER_PLAN_template.md` and fill in every section:

| Section | What to fill |
|---|---|
| 0. Environment | Your OS, Python version, LaTeX distribution |
| 1. Paper Identity | Title, venue, type, status, deadline |
| 2. Research Summary | One paragraph — what this paper does, for anyone |
| 3. Domain & Landscape | Field, sub-domain, core problem, why unsolved |
| 4. Novelty & Gaps | Gap table — what no prior work does, and why yours addresses it |
| 5. Goal | Objective, architecture, method, key claim |
| 6. Methodology | Check which type (optimization / RL / analysis / hybrid) |
| 7. Skills Inventory | Which skill files are active — see Step 4 below |
| 8. Notation | Leave mostly empty — build during writing |
| 9. Directory | Copy the directory tree once you've set up folders |
| 10. Paper Structure | Section titles and initial statuses (all Pending) |
| 11. Sim Parameters | Fill in before starting simulation code |
| 12. Baselines | List all comparison schemes before starting results section |
| 13. Workflow Checklist | All pending — will be checked off as sessions progress |
| 14. Git Setup | Add remote URL after Step 5 |
| 15. Open TODOs | Your initial task list |
| 16. Session Log | Will be auto-filled by sync-session workflow |

Then rename: `mv MASTER_PLAN_template.md MASTER_PLAN.md`

---

## Step 3 — Fill in the Context Files

The `context/` folder is what Claude reads every session to understand your paper.
Fill each file carefully — this is the most important setup step.

| File | What to fill | When |
|---|---|---|
| `context/domain.md` | Field, problem, why unsolved, literature categories | Before first session |
| `context/target.md` | Venue, format, section status table | Before first session |
| `context/idea.md` | Novelty claim, 4 contributions, narrative arc | Before writing intro |
| `context/notation.md` | Symbol table — build as you write | Updated each session |

---

## Step 4 — Choose and Customize Skills

Skills files tell Claude what conventions and methods you are using.
The template provides generic skills. Choose which to enable and add domain-specific ones.

### Built-In Generic Skills (always active)

| File | Purpose |
|---|---|
| `skills/writing.md` | IEEE/venue LaTeX style, acronym rules, citation format, narrative structure |
| `skills/coding.md` | Python conventions, CVXPY, NumPy, plotting, testing |
| `skills/math.md` | Matrix operations, solver patterns, convergence checks |

### Optional Generic Skills (enable by keeping in skills/)

| File | Enable if... |
|---|---|
| `skills/optimization.md` | Using AO, SOCP, SCA, ADMM, or gradient methods |
| `skills/channel-model.md` | Working with MIMO channels, array response, path loss models |
| `skills/performance-analysis.md` | Comparing baselines, running Monte Carlo, designing figures |
| `skills/convex.md` | Reference example — replace with your paper's reformulation patterns |

### Domain-Specific Skills (create per paper)

If your paper involves a domain-specific system model or technique, create a skills file for it:

| Example | File to create | What to put in it |
|---|---|---|
| Pinching antennas / PASS | `skills/pass.md` | Waveguide model, dielectric coupling, position-dependent channel |
| RIS / IRS | `skills/ris.md` | Phase shift model, cascaded channel, passive beamforming constraints |
| NOMA | `skills/noma.md` | SIC ordering, power allocation constraints, decoding conditions |
| Deep learning | `skills/dl.md` | Framework (PyTorch/JAX), architecture, training loop, loss function |
| Reinforcement learning | `skills/rl.md` | Environment definition, state/action/reward, algorithm (PPO/SAC/DQN) |
| [Your domain] | `skills/[domain].md` | System model conventions, standard equations, notation |

The domain-specific skills file should contain:
- The key equations of the domain model (so Claude can write correct LaTeX)
- Notation conventions specific to that domain
- Known identities or simplifications specific to the system
- References to background papers

---

## Step 5 — Create GitHub Repo and Initialize Git

On GitHub: create a new private repository named `<new-paper-name>`.

```bash
git init
git remote add origin git@github.com:<your-username>/<new-paper-name>.git
git branch -M main
git add MASTER_PLAN.md context/ skills/ workflows/ .gitignore
git commit -m "Session 000: Initial workspace setup"
git push -u origin main
```

The `.gitignore` should exclude: `.venv/`, `__pycache__/`, `*.aux`, `*.log`, `*.fls`,
`*.fdb_latexmk`, `*.out` (but NOT `*.pdf` for figures — only build artifacts).

---

## Step 6 — Set Up Python Virtual Environment

```bash
python -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install -r our-paper/code/requirements.txt
```

Add domain-specific packages to `requirements.txt` as needed (e.g., `torch`, `gymnasium`).

---

## Step 7 — Add Base Paper Files

Copy your base paper into the `base-paper/` folder:
```
base-paper/
├── [AuthorYear_ShortTitle].pdf
└── base_code/
    └── main.py          ← Your reproduced simulation
```

See `base-paper/README.md` for the checklist before proceeding.

---

## Step 8 — Add Reference Papers

Copy your 5–15 curated reference PDFs into `ref-papers/`.
See `ref-papers/README.md` for organization guidance.

---

## Step 9 — Add Literature Review

Copy all surveyed PDFs into `lit-review/`.
Add a summary entry to `lit-review/summaries.md` for each paper using the 5-field format.
Run the `lit-review-audit` workflow after adding the first batch.

---

## Step 10 — Let Claude Code Complete Setup

In Claude Code, paste:
```
Read MASTER_PLAN.md completely. Then set up the workspace:
create any missing directories, stub files, and verify the
context files are consistent with what I filled in.
Tell me what I still need to add manually.
```

---

## Step 11 — Copy Your LaTeX Source

```
cp path/to/your/main.tex our-paper/main.tex
cp path/to/your/figures/* our-paper/figures/
cp path/to/your/references.bib our-paper/references.bib
cp path/to/your/IEEEtran.cls our-paper/IEEEtran.cls  # or venue template
```

If starting from scratch (no existing .tex): Claude Code will create the skeleton
during the first `write-update` workflow run.

---

## Step 12 — Start Working

Every session begins with:
```
Read MASTER_PLAN.md first. Then run the session-audit workflow.
```

The session-audit will tell you the current state and the recommended next action.

---

## Session Workflow

### Every Session Start
```
Read MASTER_PLAN.md first. Then run the session-audit workflow.
```

### During the Session

Use workflows for specific tasks:

| Task | Paste into Claude Code |
|---|---|
| Write or edit a section | `Run the write-update workflow. I want to [description].` |
| Add a new paper | `Add [CitationKey] to lit-review/summaries.md: [title, venue, method, result, relevance].` |
| Implement code | `Run the code-audit workflow on our-paper/code/. Then implement [description].` |
| Check style consistency | `Run the writing-verify workflow on our-paper/main.tex.` |
| Audit literature | `Run the lit-review-audit workflow.` |
| Verify math | `Run the math-audit workflow on our-paper/supplement/supplement.tex.` |
| Get peer review | `Run the review workflow in 3-reviewer panel mode on our-paper/main.pdf.` |

### Every Session End
```
Run the sync-session workflow. Session summary: [one sentence on what you did].
```

The sync-session workflow will:
1. Update all context files to reflect current manuscript state
2. Overwrite `SESSION_README.md` with current status
3. Append to `CHANGELOG.md`
4. Create an archive snapshot in `archive/`
5. Commit and push to GitHub

---

## Skills Selection Guide (Quick Reference)

| Your methodology | Enable these skills |
|---|---|
| Convex optimization (SOCP, SCA, AO) | `optimization.md` + `math.md` |
| MIMO channel model | `channel-model.md` |
| Performance comparison / figures | `performance-analysis.md` |
| Wireless domain-specific model | Create `skills/[domain].md` |
| Reinforcement learning | Create `skills/rl.md` |
| Deep learning / neural network | Create `skills/dl.md` |
| Analytical bounds (capacity, FIM) | `math.md` + `performance-analysis.md` |

---

## Workflow Quick Reference

| Workflow | Invocation | Run when |
|---|---|---|
| `session-audit` | `Run the session-audit workflow.` | Every session start |
| `write-update` | `Run the write-update workflow. I want to [X].` | Writing / editing |
| `writing-verify` | `Run the writing-verify workflow on our-paper/main.tex.` | After major writing |
| `lit-review-audit` | `Run the lit-review-audit workflow.` | After adding papers |
| `code-audit` | `Run the code-audit workflow on our-paper/code/.` | After coding |
| `math-audit` | `Run the math-audit workflow on our-paper/supplement/supplement.tex.` | Before submission |
| `review` | `Run the review workflow in 3-reviewer panel mode on our-paper/main.pdf.` | Pre-submission |
| `sync-session` | `Run the sync-session workflow. Session summary: [X].` | Every session end |

---

## GitHub Integration

### Commit Convention
Every commit message follows: `"Session NNN: [one-line summary of what was done]"`

Examples:
- `"Session 003: Write System Model section; add 12 symbols to notation.md"`
- `"Session 007: Run writing-verify; fix 4 acronym errors; add Fig. 3"`

### Pushing Changes
The `sync-session` workflow handles commits and push automatically.
If you need to push manually:
```bash
git add -A
git commit -m "Session NNN: [summary]"
git push origin main
```

### Never Push to Main Without Reviewing
For collaborative papers: use feature branches and pull requests.
For solo papers: push directly to main is fine.

---

## Troubleshooting

| Problem | Solution |
|---|---|
| Claude gives generic output | Verify context/ files are filled in completely |
| Session-audit says stale context | Run sync-session from previous session; update context files manually |
| Writing-verify flags many issues | Run write-update first to fix known issues; then re-verify |
| CVXPY solver failure | Check solver installation (`pip install clarabel mosek`); fall back to SCS |
| LaTeX doesn't compile | Check for undefined references with `\usepackage{nag}`; check .log file |
| Git push fails | Check SSH key is added to GitHub; check remote URL with `git remote -v` |
