# MASTER_PLAN.md — [PAPER SHORT NAME] Manuscript Workspace
## Claude Code Session Entry Point — Read This First, Every Session

---

> **How to use this file at the start of every Claude Code session:**
> ```
> Read MASTER_PLAN.md first. Then run the session-audit workflow.
> ```
> The session-audit will report current status and recommend the next action.

---

## 0. ENVIRONMENT SNAPSHOT

| Item | Value |
|---|---|
| OS | [e.g., Ubuntu 24.04 LTS] |
| Python | [e.g., 3.13.3 — command: `python`, NOT `python3`] |
| Virtual Env | `.venv/` in project root — activate: `source .venv/bin/activate` |
| LaTeX | [e.g., TeX Live 2023 / MacTeX 2024] |
| Git | SSH login configured |
| IDE | VS Code with Claude Code extension |

---

## 1. PAPER IDENTITY

| Field | Value |
|---|---|
| **Working title** | [FULL TITLE] |
| **Short name** | [SHORT NAME — used in commit messages, filenames] |
| **Target venue** | [JOURNAL / CONFERENCE NAME] |
| **Venue acronym** | [e.g., TWC / ICC / GLOBECOM / TSP] |
| **Paper type** | [Journal / Conference / Workshop] |
| **Page limit** | [N pages, format — e.g., 12 pages double-column IEEEtran] |
| **Submission deadline** | [YYYY-MM-DD] |
| **Current status** | [Draft vN — phase, e.g., "Draft v1 — Introduction complete"] |

---

## 2. RESEARCH SUMMARY

> Write 1–2 paragraphs that explain what this paper does and why it matters.
> This should be understandable to a researcher in an adjacent field.
> Claude Code uses this to frame all writing and verification tasks.

[One paragraph: the system, the problem, and what you propose.]

[One paragraph: why the proposal is non-trivial and what results demonstrate the gain.]

---

## 3. DOMAIN AND RESEARCH LANDSCAPE

### 3.1 Field and Sub-Domain

- **Field:** [e.g., Wireless Communications / Signal Processing / Machine Learning]
- **Sub-domain:** [e.g., ISAC, NOMA, RIS-aided systems, Federated Learning]
- **Key journals/conferences:** [e.g., IEEE TWC, IEEE TSP, IEEE ICC, NeurIPS]

### 3.2 The Core Problem

[2–3 sentences describing the fundamental technical problem this paper addresses.
Be specific: name the system architecture, the conflicting objectives, and why they conflict.]

### 3.3 Why It Is Unsolved

[Explain what existing approaches lack. Be specific:
- What system model assumptions do they make that are too restrictive?
- What coupling or interaction do they ignore?
- What computational or architectural limitation prevents them from solving it?]

### 3.4 Our Approach (One Sentence)

[This paper proposes [method / architecture / algorithm] to [objective], which achieves
[key result] by [mechanism].]

---

## 4. NOVELTY AND RESEARCH GAPS

> Fill this table yourself — do not ask the AI to generate it.
> The gap claims must be defensible against the literature in `lit-review/summaries.md`.

| Gap # | Claimed gap | Why prior work misses it | How this paper addresses it |
|---|---|---|---|
| G1 | No prior work has [X] | [Because existing papers assume / use / ignore Y] | [We propose Z which handles X by...] |
| G2 | No prior work has [X] | [Because...] | [We propose...] |
| G3 | No prior work has [X] | [Because...] | [We propose...] |

**Note:** Run `lit-review-audit` workflow to verify gap authenticity against the literature.

---

## 5. GOAL OF THIS PAPER

| Item | Value |
|---|---|
| **Primary objective** | [What is being optimized / analyzed — e.g., "Minimize weighted sum of MSE and CRB"] |
| **System architecture** | [What is being designed — e.g., "Dual-waveguide PASS with Tx and Rx pinching antennas"] |
| **Method** | [Optimization / RL / Analysis — e.g., "AO with SOCP and SCA sub-problems"] |
| **Key claim** | [One sentence: "The proposed [X] achieves [Y] gain over [Z] baseline by [mechanism]."] |
| **Simulation** | [e.g., "Monte Carlo over N=500 channel realizations; Python + CVXPY"] |
| **Figures to produce** | [List figure names: fig1_pareto, fig2_convergence, fig3_vs_power, ...] |

---

## 6. METHODOLOGY TYPE

> Check one or more. This determines which skills files to enable (see Section 7).

- ☐ **Convex Optimization** — SOCP, SCA, AO, ADMM → enable `optimization.md`
- ☐ **Non-convex Optimization** — gradient, metaheuristics, evolutionary
- ☐ **Reinforcement Learning** — DQN, PPO, SAC, DDPG → create `skills/rl.md`
- ☐ **Deep Learning** — supervised/unsupervised, neural architecture → create `skills/dl.md`
- ☐ **Analytical / Performance Analysis** — closed-form bounds, capacity, FIM → enable `performance-analysis.md`
- ☐ **Hybrid** — describe: [...]

---

## 7. SKILLS INVENTORY

> List which skill files are active for this paper and why.
> Remove skills that are not relevant. Add domain-specific ones as needed.

| Skill File | Purpose | Active? |
|---|---|---|
| `skills/writing.md` | IEEE/venue style, acronyms, citation format | ✅ Always |
| `skills/coding.md` | Python conventions, CVXPY, plotting | ✅ Always |
| `skills/math.md` | Matrix ops, solver patterns, convergence | ✅ Always |
| `skills/optimization.md` | AO, SOCP, SCA, ADMM patterns | ☐ Enable if using convex opt |
| `skills/channel-model.md` | MIMO channels, path loss, array response | ☐ Enable if wireless channels |
| `skills/performance-analysis.md` | KPIs, baselines, Monte Carlo, figures | ☐ Enable if simulation-based |
| `skills/convex.md` | Domain-specific reformulation example | ☐ Replace with your patterns |
| `skills/[domain].md` | [Your domain — e.g., skills/ris.md] | ☐ Create and enable |

**Domain-specific skill file:** If your system model is specialized (RIS, PASS, NOMA, FD, etc.),
create `skills/[domain].md` containing the key equations, channel model, and notation conventions.

---

## 8. NOTATION TABLE

> Built incrementally during writing. Add each new symbol when you first use it.
> The `writing-verify` workflow checks this table against `main.tex`.
> See `context/notation.md` for the full, categorized table.

### Summary of Key Symbols (quick reference)

| Symbol | Meaning | Defined in |
|---|---|---|
| [sym] | [meaning] | Sec N |

**LaTeX conventions:** See `context/notation.md` for full rules.
Short summary: `\mathbf{x}` vectors, `\mathbf{X}` matrices, `\mathcal{K}` sets,
`\mathrm{SINR}` operators, `\eqref{eq:label}` always, `Fig.~\ref{fig:label}` always.

---

## 9. DIRECTORY STRUCTURE

```
[new-paper-name]/
├── MASTER_PLAN.md
├── SESSION_README.md
├── CHANGELOG.md
├── .gitignore
│
├── context/
│   ├── domain.md           ← Field, problem, novelty, literature categories
│   ├── target.md           ← Venue, format, section status, submission checklist
│   ├── idea.md             ← Contributions, narrative arc
│   └── notation.md         ← Symbol table, LaTeX conventions
│
├── skills/
│   ├── writing.md          ← Writing style and LaTeX conventions
│   ├── coding.md           ← Python coding conventions
│   ├── math.md             ← Mathematical computation patterns
│   ├── optimization.md     ← AO, SOCP, SCA patterns [if applicable]
│   ├── channel-model.md    ← Channel modeling [if applicable]
│   ├── performance-analysis.md  ← KPIs, baselines, figures [if applicable]
│   └── [domain].md         ← Domain-specific skill [create per paper]
│
├── workflows/
│   ├── session-audit.md    ← Start-of-session status check
│   ├── write-update.md     ← Write/edit main.tex
│   ├── writing-verify.md   ← Style and consistency audit
│   ├── lit-review-audit.md ← Literature review coverage and gap check
│   ├── code-audit.md       ← Python code standards check
│   ├── math-audit.md       ← Derivation verifier for supplement
│   ├── review.md           ← Simulated peer review
│   └── sync-session.md     ← Session sync, archive, git push
│
├── base-paper/
│   ├── [AuthorYear].pdf
│   └── base_code/
│
├── ref-papers/             ← 5–15 curated reference PDFs
│
├── lit-review/
│   ├── summaries.md        ← One entry per surveyed paper
│   └── [papers].pdf
│
├── our-paper/
│   ├── main.tex
│   ├── main.pdf
│   ├── supplement/
│   │   ├── supplement.tex
│   │   └── supplement.pdf
│   ├── figures/            ← All figure PDFs
│   ├── code/
│   │   ├── params.py       ← ALL simulation parameters (no inline hardcoding)
│   │   ├── main_sim.py     ← Main simulation script
│   │   ├── requirements.txt
│   │   └── tests/
│   ├── references.bib
│   └── [venue-template].cls
│
└── archive/                ← Read-only session snapshots
    └── session_YYYY-MM-DD_NNN/
```

---

## 10. PAPER STRUCTURE

> Update statuses each session. Use: `Pending` / `In Progress` / `Draft` / `Complete`

| Section | Title | Status | Notes |
|---|---|---|---|
| Abstract | | Pending | |
| I | Introduction | Pending | |
| II | System Model | Pending | |
| III | Proposed Method | Pending | |
| IV | Numerical Results | Pending | |
| V | Conclusion | Pending | |
| — | Appendix / Supplement | Pending | |
| — | Bibliography | Pending | 0 refs |

---

## 11. SIMULATION PARAMETERS

> Fill this before starting simulation code. `params.py` must match this table exactly.
> Every parameter used in code must appear here; no inline hardcoding.

| Parameter | Symbol | Value | Notes |
|---|---|---|---|
| [param name] | [symbol] | [value] | [units / source] |

---

## 12. BASELINE SCHEMES

> List all comparison schemes before starting the Results section.
> Each baseline must be fair: same power budget, same channel, same noise.

| Name | Description | Distinguishing constraint |
|---|---|---|
| **Proposed** | [Full method — all components] | None |
| **Baseline 1** | [One-line description] | [What this baseline cannot do] |
| **Baseline 2** | [One-line description] | [What this baseline cannot do] |
| **Upper Bound** | [Relaxed / genie-aided version] | [What relaxation is applied] |

---

## 13. WORKFLOW CHECKLIST

> Track which workflows have been run. Update after each run.

| Workflow | Status | Last run | Notes |
|---|---|---|---|
| `session-audit` | ⬜ Pending | — | Run every session start |
| `write-update` | ⬜ Pending | — | Run after each writing block |
| `lit-review-audit` | ⬜ Pending | — | Run after adding papers; before writing intro |
| `writing-verify` | ⬜ Pending | — | Run after completing each major section |
| `code-audit` | ⬜ Pending | — | Run after implementing simulation |
| `math-audit` | ⬜ Pending | — | Run before submission (if supplement exists) |
| `review` (3-panel) | ⬜ Pending | — | Run before final submission |
| `sync-session` | ⬜ Pending | — | Run every session end |

Status: ⬜ Pending / 🔄 In Progress / ✅ Done (score if applicable) / ⚠️ Issues found

---

## 14. GITHUB AND GIT SETUP

| Item | Value |
|---|---|
| **Remote URL** | `git@github.com:<username>/<repo-name>.git` |
| **Branch** | `main` |
| **Push command** | `git push origin main` |

### Commit Convention

Every commit: `"Session NNN: [one-line summary]"`

Examples:
- `"Session 001: Add manuscript files and lit-review PDFs"`
- `"Session 004: Write Results section; fix 3 notation errors from writing-verify"`

### Initial Setup Commands

```bash
git init
git remote add origin git@github.com:<username>/<repo-name>.git
git branch -M main
git add MASTER_PLAN.md context/ skills/ workflows/ .gitignore
git commit -m "Session 000: Initial workspace setup"
git push -u origin main
```

---

## 15. OPEN TODOS

> Numbered in priority order. Update each session.

1. [ ] Fill in all MASTER_PLAN.md sections
2. [ ] Fill in context/ files (domain.md, target.md, idea.md)
3. [ ] Add base paper to base-paper/ and verify code reproduces results
4. [ ] Add reference papers to ref-papers/ and lit-review/
5. [ ] Run lit-review-audit workflow
6. [ ] Copy or create our-paper/main.tex skeleton
7. [ ] Set up Python venv and verify requirements.txt
8. [ ] Push Session 000 to GitHub

---

## 16. SESSION LOG

> Auto-updated by `sync-session` workflow. Do not edit manually.

| Session # | Date | What was done | Files changed | Next task |
|---|---|---|---|---|
| 000 | [YYYY-MM-DD] | Workspace created from template | All folders | Fill context files |
