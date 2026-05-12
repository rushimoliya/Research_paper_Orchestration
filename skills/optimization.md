# skills/optimization.md — Optimization Methods Reference

> Generic reference for optimization-based papers.
> Covers: Alternating Optimization (AO), SOCP, SCA, Augmented Lagrangian, ADMM, gradient methods.
> Fill in Section 7 (Problem-Specific Patterns) with your paper's actual formulations.

---

## 1. Alternating Optimization (AO) Framework

### When to Use
- Optimization variable splits into blocks (e.g., beamforming + antenna positions + power)
- Problem is non-convex jointly but tractable block-by-block
- Each block has a known or derivable optimal solution (closed-form or convex sub-problem)

### Structure

```
Initialize: x1_0, x2_0, ..., xK_0
Repeat until convergence:
    x1_{t+1} = argmin f(x1; x2_t, ..., xK_t)   [Block 1 update]
    x2_{t+1} = argmin f(x1_{t+1}, x2; x3_t, ...) [Block 2 update]
    ...
    xK_{t+1} = argmin f(x1_{t+1}, ..., xK)       [Block K update]
Until |f_t - f_{t-1}| / |f_{t-1}| < ε_rel  or  iter > max_iter
```

### Convergence Guarantee Conditions
- Each block update is globally optimal for that block (not just a descent step)
- Objective is monotonically non-increasing across all block updates
- Objective is bounded below → sequence converges (but not necessarily to global optimum)

### Implementation Notes
```python
max_iter = 200
eps_rel = 1e-4
obj_prev = np.inf
for t in range(max_iter):
    x1 = update_block1(x2, x3, params)
    x2 = update_block2(x1, x3, params)
    x3 = update_block3(x1, x2, params)
    obj = compute_objective(x1, x2, x3, params)
    if abs(obj - obj_prev) / (abs(obj_prev) + 1e-10) < eps_rel:
        break
    obj_prev = obj
```

---

## 2. SOCP (Second-Order Cone Programming)

### When to Use
- Minimize a linear or convex objective
- Constraints involve quadratic or norm expressions: `‖Ax + b‖ ≤ c^T x + d`
- Fractional objectives (1/x, x²/y): use epigraph reformulation

### Epigraph Reformulation for Fractional Objectives

Original: minimize 1/P  (P is an optimization variable)
Reformulation:
- Introduce auxiliary t: minimize t, subject to 1/P ≤ t
- Rotated SOC form: 1 ≤ t * P, with t ≥ 0, P ≥ 0  →  `cp.quad_over_lin(1, P) ≤ t`
- Or: `[2; t - P]` lies in second-order cone with `t + P ≥ 0`

### SINR Constraint as SOC

SINR_k ≥ γ_k:
- `‖interference_stack‖ ≤ (1/√γ_k) * signal_term`
- This is: `cp.SOC(signal_term / np.sqrt(gamma_k), interference_stack)`
- Phase rotation trick: rotate beamformer so signal is real and positive

### CVXPY Pattern
```python
import cvxpy as cp
import numpy as np

def solve_socp_block(params):
    # Variables
    x = cp.Variable((n, 1), complex=True)
    t = cp.Variable(nonneg=True)  # epigraph variable

    # Objective
    obj = cp.Minimize(t)

    # Constraints
    constraints = [
        cp.SOC(t, A @ x + b),           # ‖Ax+b‖ ≤ t
        cp.quad_over_lin(1, cp.real(c.H @ x)) <= t2,  # fractional
        cp.real(d.H @ x) >= gamma,      # linear
    ]

    prob = cp.Problem(obj, constraints)
    try:
        prob.solve(solver=cp.MOSEK)
        if prob.status not in ['optimal', 'optimal_inaccurate']:
            prob.solve(solver=cp.CLARABEL)
        if prob.status not in ['optimal', 'optimal_inaccurate']:
            prob.solve(solver=cp.SCS)
    except cp.SolverError:
        prob.solve(solver=cp.SCS)

    if prob.status in ['optimal', 'optimal_inaccurate'] and x.value is not None:
        return x.value
    else:
        return x_prev  # fallback to previous iterate
```

### Solver Priority
1. MOSEK (most reliable for SOCP; requires license)
2. CLARABEL (good default; open-source)
3. SCS (fallback; less accurate but always available)

---

## 3. SCA (Successive Convex Approximation)

### When to Use
- Constraint or objective is non-convex in the optimization variable
- Function is smooth (differentiable) so Taylor linearization is applicable
- Example: channel gain h(x) = κ₀ exp(jφ(x)) / r(x)^(n/2) depends non-linearly on position x

### First-Order Taylor Linearization Template

Non-convex function f(x) at current point x_prev:
```
f(x) ≈ f(x_prev) + ∇f(x_prev)^T (x - x_prev)    [real case]
f(x) ≈ f(x_prev) + 2 Re{∇f(x_prev)^H (x - x_prev)}  [complex case]
```

In CVXPY, this becomes a linear constraint (affine in x).

### Proximal Regularization

Add `+ ρ * cp.sum_squares(x - x_prev)` to the objective to prevent large steps:
- ρ should be large enough to ensure the linearization is accurate
- ρ too large → slow convergence; ρ too small → oscillation
- Typical: ρ = 0.01 to 1.0 (tune per problem)

### SCA Loop Pattern
```python
rho = 0.1  # proximal weight
x_prev = x_init.copy()
for t in range(max_iter):
    # Compute gradient of non-convex term at x_prev
    grad = compute_gradient(x_prev, params)

    # Build linearized CVXPY problem
    x = cp.Variable(shape)
    linearized_term = f_val_prev + 2 * cp.real(grad.conj().T @ (x - x_prev))
    prox = rho * cp.sum_squares(x - x_prev)
    prob = cp.Problem(cp.Minimize(linearized_term + prox), constraints)
    prob.solve(solver=cp.MOSEK)

    if prob.status in ['optimal', 'optimal_inaccurate'] and x.value is not None:
        x_prev = x.value
    else:
        break  # solver failed; keep previous iterate

    # Convergence check
    if np.linalg.norm(x.value - x_prev) < 1e-5:
        break
```

---

## 4. Augmented Lagrangian (AL) Method

### When to Use
- Equality constraint h(x) = 0 that cannot be expressed as a cone constraint
- Example: minimum spacing constraint between discrete positions

### AL Formulation

Original: minimize f(x), subject to h(x) = 0

Augmented Lagrangian:
```
L_ρ(x, λ) = f(x) + λ^T h(x) + (ρ/2) ‖h(x)‖²
```

### Update Rules
```python
lambda_dual = np.zeros(constraint_dim)
rho_al = 1.0
beta = 1.5     # penalty growth factor
rho_max = 50.0

for t in range(max_al_iter):
    # Primal update: minimize L_rho over x (inner CVXPY solve)
    x = solve_inner_problem(lambda_dual, rho_al, params)

    # Dual update
    violation = compute_constraint_violation(x, params)  # h(x)
    lambda_dual = lambda_dual + rho_al * violation

    # Penalty growth
    rho_al = min(rho_al * beta, rho_max)

    # Convergence: constraint satisfied
    if np.linalg.norm(violation) < 1e-4:
        break
```

---

## 5. ADMM (Alternating Direction Method of Multipliers)

### When to Use
- Problem is separable: minimize f(x) + g(z), subject to Ax + Bz = c
- Useful when f and g are easy to minimize separately but the constraint couples them

### ADMM Updates
```
x-update: x^{t+1} = argmin f(x) + (ρ/2) ‖Ax + Bz^t - c + u^t‖²
z-update: z^{t+1} = argmin g(z) + (ρ/2) ‖Ax^{t+1} + Bz - c + u^t‖²
u-update: u^{t+1} = u^t + Ax^{t+1} + Bz^{t+1} - c   (scaled dual variable)
```

### Stopping Criteria
- Primal residual: `‖Ax + Bz - c‖ < ε_pri`
- Dual residual: `‖ρ A^T B (z^{t+1} - z^t)‖ < ε_dual`

---

## 6. Gradient-Based Methods

### Projected Gradient Descent (Box Constraints)

```python
step_size = 0.01
lb, ub = params['x_lb'], params['x_ub']

for t in range(max_iter):
    grad = compute_gradient(x, params)
    x = x - step_size * grad
    x = np.clip(x, lb, ub)     # projection onto box constraint
```

### Backtracking Line Search
```python
alpha, beta_ls = 1.0, 0.5
c_ls = 0.01
f_curr = compute_objective(x, params)
grad = compute_gradient(x, params)

while compute_objective(x - alpha * grad, params) > f_curr - c_ls * alpha * np.dot(grad, grad):
    alpha *= beta_ls
```

---

## 7. Problem-Specific Patterns (Fill In Per Paper)

> Replace this section with the actual reformulation patterns for your paper.
> Follow the style of Sections 2–6 above: name the pattern, state when to use it,
> give the math, and give the CVXPY code.

### [Pattern Name — e.g., "Tx Beamforming SOCP"]
- **When:** [describe the sub-problem]
- **Variables:** [list CVXPY variables]
- **Reformulation:** [mathematical form]
- **CVXPY code:** [snippet]

---

## 8. Convergence Monitoring

Always log and plot the objective value per outer AO iteration:

```python
import logging
logger = logging.getLogger(__name__)

obj_history = []
for t in range(max_iter):
    # ... block updates ...
    obj = compute_objective(x1, x2, params)
    obj_history.append(obj)
    logger.info(f"AO iter {t}: obj = {obj:.6f}")

# Check monotone non-increasing (debugging)
for i in range(1, len(obj_history)):
    if obj_history[i] > obj_history[i-1] + 1e-8:
        logger.warning(f"Non-monotone at iter {i}: {obj_history[i-1]:.6f} → {obj_history[i]:.6f}")
```
