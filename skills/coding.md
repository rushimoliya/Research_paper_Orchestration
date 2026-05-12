# skills/coding.md — Python Simulation Coding Standards

> Generic coding conventions for simulation-based research papers.
> Python + CVXPY + NumPy + Matplotlib stack.
> Adjust library list for RL (PyTorch/JAX/Gymnasium) or other frameworks.

---

## Environment

- Python 3.13.3, command: `python` (NOT `python3`)
- Always activate virtual environment: `source .venv/bin/activate`
- Virtual environment location: `.venv/` in project root

## Core Libraries

- **CVXPY**: all convex optimization sub-problems
- **SciPy**: linear algebra (`scipy.linalg.solve`), special functions
- **NumPy**: array operations — use `np.array`, NEVER `np.matrix` (deprecated)
- **Matplotlib**: all plots (PDF output, 300 DPI minimum)

Add domain-specific libraries to `requirements.txt` as needed (e.g., `torch`, `gymnasium`).

## Parameters

- All simulation parameters: centralized in `params.py` — NEVER hardcode inline
- Import pattern: `from params import *` or explicit named imports
- Every parameter in `params.py` must appear in `MASTER_PLAN.md` Section 11 (Simulation Parameters)

## Reproducibility

```python
import numpy as np
np.random.seed(42)   # top of EVERY simulation script
```

## Logging

```python
import logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)
# Use logger.info(), logger.warning(), logger.error()
# NO bare print() in production code
```

## Error Handling

- No bare `except:` — always `except SpecificError as e:`
- CVXPY: always check `problem.status` before accessing `.value`
- Matrix inversion: check `np.linalg.cond()` before inverting

## Type Hints

```python
# All function signatures must have type hints
def compute_channel(position: np.ndarray, params: dict) -> np.ndarray:
    ...
```

## Docstrings

Google style — one line for simple functions, full format for complex ones:

```python
def solve_subproblem(variable_prev: np.ndarray, params: dict) -> np.ndarray:
    """Solve the [sub-problem name] block in the AO algorithm.

    Args:
        variable_prev: Previous iterate, shape (N,).
        params: Simulation parameter dictionary.

    Returns:
        Updated variable, shape (N,).
    """
```

## File Structure and Naming

- `snake_case` for all Python files and function names
- `UPPER_CASE` for constants in `params.py`
- Suggested module layout for optimization-based papers:

```
our-paper/code/
├── params.py           ← All simulation parameters (source of truth)
├── main_sim.py         ← Main loop: Monte Carlo + calls sub-modules
├── channel.py          ← Channel model (path loss, array response)
├── algorithm.py        ← Proposed algorithm (AO master loop + sub-problems)
├── baselines.py        ← All baseline scheme implementations
├── metrics.py          ← KPI computation (rate, MSE, CRB, etc.)
├── plots/
│   └── plot_results.py ← Reads saved results; generates figures
├── utils/
│   ├── math_utils.py   ← Shared matrix ops, projections
│   └── io_utils.py     ← Save/load (numpy .npz or pickle)
└── tests/
    ├── test_channel.py
    └── test_algorithm.py
```

Adjust module names to match your paper's architecture.

## Tests

- pytest framework: `pytest our-paper/code/tests/`
- File prefix: `test_` (e.g., `test_channel.py`)
- Test functions: `test_` prefix
- At minimum, test: channel model limits, algorithm convergence, KPI computation

## Plotting Standards

```python
import matplotlib
matplotlib.use('Agg')  # non-interactive backend
import matplotlib.pyplot as plt
from pathlib import Path

FIGURES_DIR = Path("our-paper/figures")

fig, ax = plt.subplots(figsize=(3.5, 2.8))  # single-column width
ax.set_xlabel('Parameter Name [unit]', fontsize=9)
ax.set_ylabel('Metric Name [unit]', fontsize=9)
ax.tick_params(labelsize=8)
ax.legend(fontsize=8, framealpha=0.9)
ax.grid(True, linestyle='--', alpha=0.5)

plt.tight_layout()   # ALWAYS before savefig
fig.savefig(FIGURES_DIR / "figN_description.pdf", dpi=300, bbox_inches='tight')
plt.close(fig)       # prevents memory leaks in loops
```

- Format: **PDF only** (vector graphics for publication)
- DPI: 300 minimum
- Output: `our-paper/figures/` — always use `pathlib.Path`
- `plt.tight_layout()` before every `plt.savefig()`
- `plt.close(fig)` after saving

## CVXPY Problem Template

```python
import cvxpy as cp
import logging
logger = logging.getLogger(__name__)

def solve_block(variable_prev: np.ndarray, params: dict) -> np.ndarray:
    """Solve one AO block as a convex sub-problem."""
    N = params['N']

    # Variables
    x = cp.Variable((N, 1), complex=True)

    # Objective and constraints
    obj = cp.Minimize(...)
    constraints = [...]

    problem = cp.Problem(obj, constraints)

    try:
        problem.solve(solver=cp.MOSEK)
        if problem.status not in ['optimal', 'optimal_inaccurate']:
            logger.warning(f"MOSEK: {problem.status}. Trying CLARABEL.")
            problem.solve(solver=cp.CLARABEL)
    except cp.SolverError as e:
        logger.error(f"SolverError: {e}. Falling back to CLARABEL.")
        problem.solve(solver=cp.CLARABEL)

    if problem.status not in ['optimal', 'optimal_inaccurate']:
        logger.error(f"Sub-problem failed: {problem.status}. Using previous iterate.")
        return variable_prev

    return x.value
```

## Figure Naming Convention

Name figures by what they show, not by index:

```
figN_[metric]_vs_[parameter].pdf
```

Examples:
- `fig1_pareto_tradeoff.pdf`
- `fig2_convergence.pdf`
- `fig3_rate_vs_power.pdf`
- `fig4_mse_vs_N_antennas.pdf`

Update `MASTER_PLAN.md` Section 5 (Goal → Figures to produce) with the final figure list.
