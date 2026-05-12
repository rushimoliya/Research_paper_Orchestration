# skills/convex.md — Domain-Specific Convex Reformulation Patterns (EXAMPLE FILE)

> **This is an EXAMPLE of a domain-specific convex reformulation reference.**
> It contains reformulation patterns specific to a particular paper (Full-PASS ISCC).
> **Replace the contents of this file with your own paper's specific reformulation patterns.**
>
> For generic AO / SOCP / SCA / ADMM patterns, see `skills/optimization.md`.
> For generic CVXPY solver usage, see `skills/math.md`.
>
> When creating your own version, follow this structure:
> - Pattern name (one per major sub-problem)
> - When to use it
> - Mathematical reformulation
> - CVXPY code snippet
> - Convergence or feasibility note (if applicable)

---

# skills/convex.md — Convex Reformulation Patterns

## 1. Non-Convex: 1/P_s in Objective → SOCP

**Problem:** CRB_loc ∝ 1/P_s is non-convex in beamformers.

**Reformulation:**
1. Introduce epigraph variable: τ_crb ≥ 1/P_s via constraint τ_crb · P_s ≥ 1
2. Linearize P_s around current iterate: P̲_s = 2Re{∑_k ν_k^* h_t^H w_k + ν_0^* h_t^H w_0} − P_s^(i)
   where ν_k = w_k^(i)^H h_t and ν_0 = w_0^(i)^H h_t
3. Rotated SOC for τ_crb · P̲_s ≥ 1:
   `‖[2; τ_crb − P̲_s]‖ ≤ τ_crb + P̲_s`

**CVXPY:**
```python
tau_crb = cp.Variable(nonneg=True)
P_s_lb = 2 * cp.real(nu_k_conj @ h_t.conj().T @ W_c) + ...  # linearized
constraints += [cp.SOC(tau_crb + P_s_lb, cp.vstack([2, tau_crb - P_s_lb]))]
```

---

## 2. Non-Convex: SINR Constraint → SOC

**Problem:** SINR_k ≥ γ_k is a ratio constraint (non-convex).

**Reformulation:**
1. WLOG phase rotation: h_k^H w_k ∈ ℝ_{≥0} (optimal beamformer can always be rotated)
2. SOC form:
   `h_k^H w_k ≥ √γ_k · ‖[h_k^H w_{i≠k}, h_k^H w_0, √σ_k²]‖`

**CVXPY:**
```python
sinr_lhs = cp.real(h_k.conj().T @ W_c[:, k])
interference = cp.vstack([
    *[h_k.conj().T @ W_c[:, i] for i in range(K_c) if i != k],
    h_k.conj().T @ w_0,
    np.sqrt(sigma_k_sq)
])
constraints += [cp.SOC(sinr_lhs / np.sqrt(gamma_k), interference)]
```

---

## 3. Non-Convex: Oscillatory Channel h_n(q; x_n) → SCA

**Problem:** Near-field LoS channel is oscillatory in PA positions (non-convex).

**Reformulation (SCA):**
Linearize h_n around current iterate x_n^(i):
```
h_n(q; x_n) ≈ h_n(q; x_n^(i)) + ∇_{x_n} h_n^(i) · (x_n − x_n^(i))
```
Gradient (eq 34 in main.tex): both amplitude (1/r³) and phase (κ₀/r² + κ_g) terms retained.

**Proximal term:** add ϱ/2 · ‖x_n − x_n^(i)‖² to sub-problem objective.
- ϱ initialized at κ₀² (matching phase curvature scale)
- Decay: ϱ ← max(ϱ_min, ϱ / something) across SCA iterations
- Ensures local approximation validity

---

## 4. Non-Convex: Spacing Constraint → Augmented Lagrangian

**Constraint:** x_m^n − x_{m-1}^n ≥ δ for all m, n

**Why NOT hard projection:** alternating hard projection oscillates and does not converge reliably.

**Augmented Lagrangian approach:**
```
s_m^n = δ − (x_m^n − x_{m-1}^n)   [positive when violated]

AL penalty in objective:
  ∑_{m,n} [ λ_m^n · s_m^n + (μ_AL/2) · max(0, s_m^n)² ]

After inner proximal gradient converges:
  Dual update:  λ_m^n ← max(0, λ_m^n + μ_AL · s_m^n)
  Penalty growth: μ_AL ← β · μ_AL
```

**Parameters:**
- Initial μ_AL^(0) = 10⁻²
- Growth factor β = 1.5

---

## 5. Closed-Form: AirComp Combiner (Wiener Filter)

**Derivation:** Minimize MSE over u_ac with all other variables fixed.

**Result:**
```
A = σ²_BS · I + H_t R_x H_t^H + ∑_j p_j g_j g_j^H
b = ∑_j √p_j · g_j
u_ac* = A^{-1} b
```

**Code:**
```python
u_ac = scipy.linalg.solve(A, b)
```

---

## 6. Closed-Form: Sensing Combiner (MVDR)

**Derivation:** Maximize sensing SNR = (u_sn^H g_t)² / (u_sn^H R_I u_sn) s.t. ‖u_sn‖=1.

**Result (MVDR):**
```
u_sn* = R_I^{-1} g_t / ‖R_I^{-1} g_t‖
```

**Code:**
```python
v = scipy.linalg.solve(R_I, g_t)
u_sn = v / np.linalg.norm(v)
```

---

## 7. Closed-Form: CompU Uplink Powers

**Derivation:** Optimal transmit scaling factors.

**Result:**
```
q_j* = clip(Re{ζ_j} / |ζ_j|², 0, √P_j^max)
p_j* = (q_j*)²
```

**Code:**
```python
q_j = np.clip(np.real(zeta_j) / np.abs(zeta_j)**2, 0.0, np.sqrt(P_j_max))
p_j = q_j**2
```

---

## SCA Convergence Conditions

For the SCA sub-problems (Tx and Rx geometry) to produce feasible iterates:
1. The proximal parameter ϱ must be large enough relative to the Hessian of the channel (κ₀² provides a safe initialization)
2. The AL penalty μ_AL must grow to infinity (in principle); practically β=1.5 growth per outer AO step is sufficient
3. The outer AO objective is monotone non-increasing since each block minimize (or doesn't increase) the objective given the others fixed
