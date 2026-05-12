# skills/math.md — Mathematical and Numerical Computation Patterns

> Generic patterns for numerical simulation of optimization-based papers.
> Covers: CVXPY solver usage, SciPy linear algebra, convergence, closed-form solutions.
> For problem-specific reformulations, see `skills/optimization.md` and `skills/convex.md`.

---

## CVXPY Patterns

```python
import cvxpy as cp
import numpy as np

# Standard problem structure
problem = cp.Problem(cp.Minimize(objective), constraints)

# Norm constraints: use cp.SOC(t, x) for ‖x‖ ≤ t
cp.SOC(t, x)

# Rotated SOC: ‖x‖² ≤ t·s  →  cp.quad_over_lin(cp.norm(x), s) ≤ t
# Or manually: if a, b ≥ 0 and a*b ≥ ‖x‖², use cp.SOC(a + b, cp.vstack([2, a - b]))

# Preferred solver order (performance and reliability)
# 1. MOSEK — best for SOCP; requires license
# 2. CLARABEL — good default; open-source; ships with CVXPY
# 3. SCS — last resort; less accurate but always available

# Always check status before accessing .value
assert problem.status in ['optimal', 'optimal_inaccurate'], \
    f"Solver failed: {problem.status}"

# Always wrap in try/except
try:
    problem.solve(solver=cp.MOSEK)
    if problem.status not in ['optimal', 'optimal_inaccurate']:
        problem.solve(solver=cp.CLARABEL)
except cp.SolverError:
    problem.solve(solver=cp.CLARABEL)
```

---

## SciPy Linear Algebra Preferences

```python
import scipy.linalg
import numpy as np

# PREFERRED: solve linear system (avoids explicit inversion)
x = scipy.linalg.solve(A, b)   # solves Ax = b for x

# Matrix inversion: always check condition number first
cond = np.linalg.cond(A)
if cond > 1e12:
    raise ValueError(f"Matrix ill-conditioned: cond = {cond:.2e}")
A_inv = np.linalg.inv(A)       # only after cond check passes

# Eigendecomposition (for generalized Rayleigh quotient problems)
eigvals, eigvecs = np.linalg.eigh(A)   # for Hermitian/symmetric A
# Note: eigh not eig — guarantees real eigenvalues for Hermitian A

# For Cholesky (when A is known positive definite)
L = np.linalg.cholesky(A)
x = scipy.linalg.cho_solve((L, True), b)
```

---

## Proximal Gradient Step (Generic)

For updating a continuous variable `x` subject to box constraints `[lb, ub]`:

```python
# Gradient step
x_new = x - step_size * gradient

# Box projection
x_new = np.clip(x_new, lb, ub)

# Optional: enforce ordering constraint (e.g., sorted positions)
# x[0] ≤ x[1] ≤ ... ≤ x[M-1] with minimum gap delta
for m in range(1, len(x_new)):
    x_new[m] = max(x_new[m], x_new[m-1] + delta)
```

---

## Augmented Lagrangian — Equality / Inequality Constraint Enforcement

For constraint `h(x) ≤ 0` (positive when violated):

```python
lam = np.zeros(constraint_dim)   # dual variables (Lagrange multipliers)
mu_al = 1e-2                     # initial penalty
beta = 1.5                        # growth factor
mu_max = 50.0                     # max penalty

for outer_iter in range(max_al_iter):
    # Inner problem: minimize f(x) + lam^T h(x) + (mu_al/2) * sum(max(0, h(x))^2)
    x = solve_inner(lam, mu_al, params)

    # Compute constraint violation
    h = compute_violation(x, params)

    # Dual update (only for violated constraints)
    lam = np.maximum(0, lam + mu_al * h)

    # Penalty growth
    mu_al = min(mu_al * beta, mu_max)

    # Convergence check
    if np.linalg.norm(np.maximum(0, h)) < 1e-4:
        break
```

---

## Generic Closed-Form Solutions

### Wiener Filter (Minimum MSE Linear Combiner)

For MSE = E[‖y - s‖²] minimized over linear combiner u:

```
u* = A^{-1} b
where A = noise_cov + signal_interference_cov
      b = cross-correlation vector
```

```python
u_opt = scipy.linalg.solve(A, b)
```

### MVDR Beamformer (Maximum SINR / Minimum Variance Distortionless Response)

For maximizing SINR = (u^H g)² / (u^H R_I u), subject to ‖u‖ = 1:

```
u* = R_I^{-1} g / ‖R_I^{-1} g‖
```

```python
v = scipy.linalg.solve(R_I, g_target)
u_opt = v / np.linalg.norm(v)
```

### Power Allocation (Projection onto Simplex or Box)

Box-constrained power: clip to [0, P_max]:

```python
p_opt = np.clip(p_unconstrained, 0, P_max)
```

Sum-power constrained (simplex projection):

```python
def project_simplex(v: np.ndarray, P_max: float) -> np.ndarray:
    """Project v onto {p ≥ 0, sum(p) ≤ P_max}."""
    u = np.sort(v)[::-1]
    cssv = np.cumsum(u)
    rho = np.nonzero(u * np.arange(1, len(u) + 1) > (cssv - P_max))[0][-1]
    theta = (cssv[rho] - P_max) / (rho + 1.0)
    return np.maximum(v - theta, 0)
```

---

## Convergence Monitoring

```python
import logging
logger = logging.getLogger(__name__)

obj_history = []
for t in range(max_iter):
    # ... block updates ...
    obj = compute_objective(params)
    obj_history.append(obj)
    logger.info(f"Iter {t:3d}: obj = {obj:.6f}")

    # Relative convergence check
    if t > 0:
        rel_change = abs(obj_history[-1] - obj_history[-2]) / (abs(obj_history[-2]) + 1e-10)
        if rel_change < eps_rel:
            logger.info(f"Converged at iter {t} (rel_change = {rel_change:.2e})")
            break
```

---

## Key Rules (Non-Negotiable)

- All parameters from `params.py` — never hardcoded inline
- `np.linalg.solve(A, b)` preferred over `np.linalg.inv(A) @ b`
- Check condition number before any matrix inversion
- `np.random.seed(42)` at top of every simulation script
- `problem.status` checked before every CVXPY `.value` access
- `try/except cp.SolverError` around every `problem.solve()` call
