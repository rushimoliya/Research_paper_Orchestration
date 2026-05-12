# PREREQUISITES.md — What You Must Know Before Starting

> **Read this before copying the template.**
> This workspace uses Claude Code as an agentic AI partner for writing, coding, and verification.
> The AI cannot replace domain expertise. The items below must come from you.

---

## The Core Principle

This system is an **orchestration layer**, not a research substitute. It helps you write faster,
catch errors, and maintain consistency — once you have already done the intellectual work.

**The research gap, the novelty claim, and the methodology are yours to identify. The AI executes.**

---

## Prerequisite Checklist

### 1. Domain Foundation

You must be able to answer these questions without consulting an AI:

- [ ] What is the specific research field and sub-field of this paper?
- [ ] What are the 10–20 most important papers in this space? What does each one do?
- [ ] What does NO existing paper do? (This is the gap — you must identify it yourself.)
- [ ] Why has no one solved it? Is it a modeling gap, an algorithmic gap, or an architectural gap?
- [ ] What trends and system models are standard in this domain? (e.g., channel models, metrics used)
- [ ] What performance metrics does this community care about and why?

> **Warning:** An AI can summarize papers you give it. It cannot read the field for you,
> identify what is missing, or judge whether a claimed gap is real. If you ask Claude
> "what is a good research gap in [domain]?", the answer will be generic and likely already
> published. Domain knowledge cannot be outsourced.

---

### 2. Research Gaps and Novelty

Before starting, you must have clearly articulated (in your own words, on paper):

- [ ] The specific gap(s) in existing literature — stated as: "No prior work has [X]"
- [ ] Why those gaps matter — what system performance suffers as a result
- [ ] How your paper addresses each gap — at the level of a one-sentence contribution statement
- [ ] Why your approach is non-trivial (what makes it technically challenging)

These statements go into `context/domain.md` and `MASTER_PLAN.md Section 4`. The AI will use
them as ground truth — it will not generate them for you.

---

### 3. Methodology Decision

You must have chosen your technical approach before starting:

- [ ] **Optimization-based?** Which formulation? (SOCP, SCA, AO, ADMM, gradient?)
       Have you verified the problem is actually solvable with this approach?
- [ ] **RL / Deep Learning?** Which algorithm? Which framework? (PyTorch, TF, JAX?)
       Have you verified the state/action/reward structure makes sense?
- [ ] **Analytical / Bounds?** Which tools? (FIM, capacity bounds, stochastic geometry?)
       Have you verified tractability?
- [ ] **Hybrid?** Clearly define which part uses which approach and why.

The methodology choice determines which skills files you activate (see HOW_TO_USE_TEMPLATE.md).

---

### 4. Base Paper Ready

> This is the single most important prerequisite.

- [ ] You have identified **one base paper** — the most similar prior work to yours
- [ ] You have the base paper's PDF, and ideally its LaTeX source
- [ ] You have **reproduced the base paper's key results** in code (your simulation matches their figures)
- [ ] You understand every equation in the base paper: where it comes from, what approximations are made
- [ ] You understand the optimization methods / algorithms used in the base paper
- [ ] You understand the optimization methods used in 3–5 closely related papers in your sub-domain

> **Why this matters:** The base paper defines your coordinate system. Your proposed system
> will be compared against it. If you haven't reproduced it, you don't yet know whether your
> gains are real or an artifact of different simulation assumptions.

---

### 5. Target Venue Decided

- [ ] Journal or conference name selected
- [ ] Page limit known (and whether a supplementary appendix is allowed)
- [ ] LaTeX template downloaded (IEEEtran, ACM, Springer, etc.)
- [ ] Submission deadline confirmed
- [ ] Author list and contribution assignments agreed upon

---

### 6. What Claude Code CAN and CANNOT Do

| CAN | CANNOT |
|---|---|
| Write and edit LaTeX to your specifications | Identify research gaps for you |
| Implement optimization algorithms you describe | Choose your methodology |
| Audit notation, acronym, and citation consistency | Judge whether your novelty is real |
| Run simulation code and interpret output | Read domain literature autonomously |
| Enforce IEEE/venue style rules | Replace peer review |
| Maintain session-to-session context via MASTER_PLAN | Know if your math is correct without checking |
| Generate figures from your code | Verify physical intuition |
| Simulate 3-panel IEEE peer review | Replace actual peer review |

---

### 7. Skills You Will Need to Add

Based on your methodology, decide which skills files to add or activate:

| Methodology | Skills to enable |
|---|---|
| Convex optimization (AO, SOCP, SCA) | `optimization.md` (provided) |
| Wireless / MIMO channel models | `channel-model.md` (provided) |
| Performance / capacity analysis | `performance-analysis.md` (provided) |
| Reinforcement learning | Create `skills/rl.md` with your env/algo conventions |
| Deep learning | Create `skills/dl.md` with your framework and architecture conventions |
| Domain-specific system model | Create `skills/[domain].md` (e.g., `skills/ris.md`, `skills/noma.md`) |

See `HOW_TO_USE_TEMPLATE.md` Section 4 for the skills selection guide.

---

### 8. Ready to Start?

Check all boxes below before running Step 1 of HOW_TO_USE_TEMPLATE.md:

- [ ] I can describe my research gap in 2 sentences without AI assistance
- [ ] I have chosen my methodology and understand why it works for this problem
- [ ] My base paper code runs and produces results matching the paper's figures
- [ ] I understand the math in the base paper at the level of being able to re-derive it
- [ ] My target venue, page limit, and deadline are confirmed
- [ ] I know which skills files I will need

If you cannot check all of these: **stop here and do the domain work first.**

---

*This template is designed for researchers who bring domain expertise and use AI to execute faster.
It is not designed to substitute for domain knowledge.*
