# workflow: code-audit

**Invocation:**
```
Run the code-audit workflow on our-paper/code/.
```
Or for a specific file:
```
Run the code-audit workflow on our-paper/code/main_sim.py.
```

**Purpose:** Audit Python simulation code for standards compliance, correctness, and
reproducibility. Always activate the virtual environment first:
```
source .venv/bin/activate && Run the code-audit workflow on our-paper/code/.
```

---

## What Claude Code Auto-Fixes (Minor Issues)

| Issue | Fix Applied |
|---|---|
| Import ordering | Sort: stdlib → third-party → local |
| Trailing whitespace | Remove |
| Missing docstring stubs | Add Google-style stub |
| Unused imports | Remove |
| `np.matrix` usage | Replace with `np.array` (deprecated) |
| Hardcoded magic number | Move to named constant in `params.py` |
| Missing `plt.tight_layout()` before `plt.savefig()` | Insert |
| `print()` in production code | Replace with `logger.info()` |
| Missing `matplotlib.use('Agg')` | Insert before pyplot import |
| Figure saved as PNG | Change to PDF (for publication quality) |

---

## What Claude Code Flags for Manual Review (Major Issues)

| Issue | Why Manual |
|---|---|
| Matrix inversion without condition check | Could silently produce garbage on ill-conditioned matrices |
| Convergence loop with no `max_iter` guard | Could hang indefinitely |
| CVXPY `.value` accessed without checking `problem.status` | Could silently use None |
| Dimension mismatch between arrays | Requires checking math in main.tex |
| Missing `np.random.seed(...)` at top of script | Confirm seed value |
| Missing entries in `requirements.txt` | Confirm package and version |
| Parameter value not matching `MASTER_PLAN.md` Section 11 | Confirm which is correct |
| Baseline scheme uses different power budget | Breaks fairness — requires redesign |

---

## Code Standards Enforced (from skills/coding.md)

- All CVXPY problems: `try/except cp.SolverError` with fallback solver
- All matrix inversions: `scipy.linalg.solve` preferred; condition check before `np.linalg.inv`
- Plots: `plt.tight_layout()` before `plt.savefig()`, `plt.close(fig)` after
- Figure output: PDF format, 300 DPI, saved to `our-paper/figures/`
- Random seeds: `np.random.seed(42)` at top of every simulation script
- Logging: `import logging` — no bare `print()` in production code
- All parameters: loaded from `params.py` — no inline hardcoding
- Type hints on all function signatures
- Docstrings: Google style

---

## Generic File Checklist

Adapt this list to your paper's code structure. The module names should match
what you defined in `MASTER_PLAN.md` Section 9 (Directory Structure).

```
params.py           ← All parameters defined; values match MASTER_PLAN Section 11
main_sim.py         ← Monte Carlo loop; calls sub-modules; saves results
channel.py          ← Channel model; matches equations in main.tex
algorithm.py        ← Proposed algorithm; AO master loop; convergence check
baselines.py        ← All baselines; same power/channel/noise as proposed
metrics.py          ← KPI computation; matches MASTER_PLAN Section 11 definitions
plots/              ← Each script reads saved results; saves PDF to figures/
utils/              ← Shared math utilities; no paper-specific logic
tests/              ← pytest tests for channel model, convergence, KPI
requirements.txt    ← All dependencies with pinned versions
```

---

## Output Format

```
═══════════════════════════════════════════════
CODE AUDIT — [PAPER SHORT NAME] — [Date]
═══════════════════════════════════════════════

== AUTO-FIXES APPLIED ==
- algorithm.py:34 — added np.linalg.cond() check before np.linalg.inv
- params.py:12 — moved inline constant 0.5 to DELTA_MIN
- plots/plot_pareto.py:88 — added plt.tight_layout() before savefig

== MANUAL REVIEW REQUIRED ==
[MAJOR] algorithm.py:67 — convergence loop has no max_iter guard
  → Add: if iter > params['MAX_ITER']: break
[MAJOR] channel.py:45 — np.linalg.inv called without condition check
  → Replace with scipy.linalg.solve(A, np.eye(N))
[MINOR] main_sim.py — np.random.seed not set; add at line 1

== PARAMETER CONSISTENCY ==
✓ All params.py values match MASTER_PLAN.md Section 11
[MANUAL] params.py:P_MAX = 30 but MASTER_PLAN says P_MAX = [value] — confirm

== REQUIREMENTS.TXT ==
✓ numpy, scipy, matplotlib, cvxpy present
[MANUAL] mosek not in requirements.txt — add if using MOSEK solver

═══════════════════════════════════════════════
```
