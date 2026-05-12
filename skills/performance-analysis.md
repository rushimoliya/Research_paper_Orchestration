# skills/performance-analysis.md — Performance Analysis Reference

> Generic reference for analyzing, presenting, and writing about simulation results.
> Covers: KPI selection, baseline design, parameter sweeps, Monte Carlo, inference writing,
> figure design. Fill in Section 6 (Paper-Specific KPIs) with your actual metrics.

---

## 1. KPI Selection Framework

### What Metrics to Use

Choose metrics that directly reflect the paper's claimed benefit:

| Claim type | Primary KPI | Secondary KPI |
|---|---|---|
| Rate / throughput | Sum rate [bps/Hz] | Per-user rate fairness |
| Estimation accuracy | CRB / MSE [units²] | Bias |
| Interference suppression | SINR [dB] | Outage probability |
| Computation accuracy | Computation MSE | Convergence speed |
| Energy efficiency | EE [bits/J] | Power consumption |
| Trade-off | Pareto frontier | Both primary KPIs on axes |

### Primary vs Secondary KPIs

- **Primary:** The metric the optimization objective acts on directly (appears in cost function)
- **Secondary:** Confirms claims don't come at unexpected cost to another metric
- **Trade-off KPI:** When two objectives conflict, plot the Pareto frontier showing the achievable region

### When to Use a Pareto Front

Use a Pareto front (scatter/curve) when:
- The paper jointly optimizes two conflicting metrics (e.g., MSE vs CRB)
- The main contribution is demonstrating Pareto dominance over baselines
- A single operating point would not show the full advantage

Use a line plot (vs parameter sweep) when:
- Demonstrating how performance scales with a system parameter
- Comparing proposed vs baselines at a fixed operating point configuration

---

## 2. Baseline Scheme Design

### Fairness Rules (Critical)

Every comparison must be fair. Fair means:
- **Same power budget** across all schemes
- **Same degrees of freedom** (same number of antennas/variables where applicable)
- **Same channel realization** (use same random seed for each Monte Carlo trial)
- **Same noise level** and system parameters from `params.py`

### Baseline Categories

| Category | Description | Example |
|---|---|---|
| **Proposed** | Your full method | Full system with all components |
| **Ablation** | Remove one component of proposed | Remove Tx optimization; fix Rx only |
| **Fixed-design** | Replace flexible component with fixed | Replace movable antenna with fixed position |
| **Published benchmark** | Method from a cited paper | [Author20XX_Method] |
| **Naive** | Simple heuristic | Random configuration; uniform allocation |
| **Upper bound** | Relaxed / genie-aided | Perfect CSI; no interference; infinite power |

### Naming Convention

Use short, descriptive names that appear identically in:
1. `MASTER_PLAN.md` Section 12 (Baseline Schemes table)
2. Python code (legend labels, filename prefixes)
3. LaTeX (figure captions, description in Section IV)

Example naming: "Tx-Only", "Rx-Fixed", "Random-Pos", "Proposed", "Fixed-Array" (fixed antenna array)

### Baseline Table Format (for MASTER_PLAN Section 12)

| Name | Description | Distinguishing constraint |
|---|---|---|
| **Proposed** | Full method | None |
| **Baseline 1** | [One-line description] | [What it cannot do / what is fixed] |
| **Baseline 2** | [One-line description] | [What it cannot do / what is fixed] |

---

## 3. Parameter Sweep Design

### What to Sweep

For each figure, identify:
1. The variable that governs your claimed benefit (x-axis)
2. The metric that shows the benefit (y-axis)
3. The fixed parameters (everything else, from `params.py`)

**Principle: One figure → one claim.**

### Common Sweep Variables

| Parameter | Typical range | What it shows |
|---|---|---|
| Max transmit power P_max | [0, 40] dBm | Power-efficiency of proposed method |
| Number of antennas N | [4, 64] | Scalability with array size |
| Number of users K | [2, 20] | Multi-user handling |
| SNR | [−10, 30] dB | Performance across regimes |
| Weight / trade-off parameter | [0, 1] | Trade-off frontier |
| Geometry (distance, height) | domain-specific | Spatial sensitivity |

### Parameter Table in MASTER_PLAN

Always document which parameters are fixed and which are swept.
The table in MASTER_PLAN Section 11 is the ground truth — `params.py` must match it exactly.

---

## 4. Monte Carlo and Statistical Averaging

### Standard Practice

```python
import numpy as np
np.random.seed(42)   # Always at top of each script

N_trials = 500       # Number of Monte Carlo realizations
results = []
for trial in range(N_trials):
    # Generate random channel realization
    H = generate_channel(params)
    # Run optimization
    result = run_proposed(H, params)
    results.append(result)

avg_result = np.mean(results)
```

### How Many Trials

| Paper type | Typical N_trials |
|---|---|
| Conference | 200–500 |
| Journal | 500–2000 |
| If curves are smooth | Can reduce to 200 |
| If curves are noisy | Increase to 1000+ |

### Reporting

- Report average over all Monte Carlo trials unless stated otherwise
- If variance matters (e.g., reliability, outage), also plot confidence intervals or CDF
- Never report a single realization as "typical" without Monte Carlo averaging

---

## 5. Inference Writing Patterns

### One Inference per Figure Paragraph

Each figure in Section IV (Results) should have exactly one main paragraph.
That paragraph makes ONE primary claim supported by ONE figure.

**Pattern:**
```
Fig. X shows [metric] versus [parameter] for [number] baselines.
[Proposed] achieves [X%] [gain/reduction] over [closest baseline] at [operating point],
owing to [physical or algorithmic reason].
As [parameter] increases, [observation], which is explained by [reason].
[If applicable: note a saturation or floor and explain why.]
```

### Observations That Need Physical Explanation

| Observation | What to explain |
|---|---|
| Proposed wins by larger margin at higher SNR | Why high SNR amplifies the architectural advantage |
| Curve saturates / plateaus | Which bottleneck is binding (interference floor, power limit, spatial DoF) |
| Baseline collapses at certain operating point | What assumption the baseline violates there |
| Diminishing returns with more antennas | Spatial degrees of freedom saturation |
| Convergence figure shows fast convergence | How many iterations; convergence speed advantage |

### Convergence Plot Inference

"Fig. X plots the objective value versus AO iteration. The proposed algorithm converges
within [N] iterations for all tested parameter settings, confirming the theoretical
monotone non-increasing property established in Section III-D."

---

## 6. Figure Design Checklist

### Axes and Labels
- [ ] x-axis: clearly labeled with units (e.g., "Transmit Power $P_{\max}$ [dBm]")
- [ ] y-axis: clearly labeled with units (e.g., "Sum Rate [bits/s/Hz]")
- [ ] Axis range justified in MASTER_PLAN (don't auto-scale without reason)
- [ ] Log scale used where dynamic range spans > 1 decade

### Legend and Curves
- [ ] Legend names match MASTER_PLAN Section 12 baseline names exactly
- [ ] Line styles distinguishable in black-and-white print (solid, dashed, dotted, dash-dot)
- [ ] Markers at data points where appropriate (not so dense they overlap)
- [ ] Font size ≥ 8pt at final column width (check by compiling the PDF)

### Caption
- [ ] Caption identifies: what metric, what parameter is swept, what is fixed
- [ ] Caption does not repeat what is obvious from axis labels
- [ ] Caption does not contain the main inference (that goes in the body text)

### Python / Matplotlib Standards
```python
import matplotlib
matplotlib.use('Agg')
import matplotlib.pyplot as plt
from pathlib import Path

fig, ax = plt.subplots(figsize=(3.5, 2.8))  # single-column width
ax.set_xlabel('Parameter Name [unit]', fontsize=9)
ax.set_ylabel('Metric Name [unit]', fontsize=9)
ax.tick_params(labelsize=8)
ax.legend(fontsize=8, framealpha=0.9)
ax.grid(True, linestyle='--', alpha=0.5)
plt.tight_layout()
fig.savefig(Path('our-paper/figures') / 'figN_name.pdf', dpi=300, bbox_inches='tight')
plt.close(fig)
```

---

## 7. Paper-Specific KPIs (Fill In)

> Replace this section with the actual metrics for your paper.

### Primary Performance Metrics

| Metric | Symbol | Formula | Units | Optimized (↑/↓) |
|---|---|---|---|---|
| [Metric 1] | [sym] | [formula] | [units] | ↑ / ↓ |
| [Metric 2] | [sym] | [formula] | [units] | ↑ / ↓ |

### Figure Plan

| Figure | x-axis | y-axis | Claim it supports |
|---|---|---|---|
| Fig. 1 | [parameter] | [metric] | [contribution claim] |
| Fig. 2 | [parameter] | [metric] | [contribution claim] |

### Expected Qualitative Outcomes

Before running simulations, write down what you expect to see and why.
If the results contradict expectations, investigate before concluding a bug is impossible.
