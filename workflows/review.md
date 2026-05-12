# workflow: review

**Invocation:**

Single reviewer:
```
Run the review workflow in single-reviewer mode on our-paper/main.pdf.
```

Full 3-reviewer panel:
```
Run the review workflow in 3-reviewer panel mode on our-paper/main.pdf.
```

**Purpose:** Simulated peer review of the manuscript. Run when the draft is substantially
complete, before submitting to the target venue. The 3-panel mode gives three independent
perspectives (Methods, Novelty, Experiments) and an Area Chair summary.

---

## Single Reviewer Mode

Generates a structured reviewer report for the target venue (from `context/target.md`):

```
== SUMMARY ==
[What the paper claims vs. what it delivers. 2–3 sentences.]

== STRENGTHS ==
1. ...
2. ...
3. ...

== MAJOR CONCERNS ==
1. [Concern title]:
   [Detailed description of the issue]
   [Required response or revision needed]

2. [Concern title]:
   ...

== MINOR COMMENTS ==
- [Line or section reference]: [comment]
- ...

== DECISION ==
[ ] Accept
[ ] Minor Revision
[X] Major Revision
[ ] Reject

Justification: [1–2 sentences explaining the decision]
```

---

## 3-Panel Reviewer Mode

Three independent reports with different foci, followed by Area Chair summary.

### Reviewer 1 — Methods
Focus: algorithmic correctness, convergence guarantees, computational complexity.

Questions examined:
- Is the problem formulation correct? Are all constraints captured?
- Is each sub-problem tractable? Is the reformulation tight (no approximation gap)?
- Is convergence of the algorithm proved or only empirically shown?
  - If proved: is the proof in the supplement? Are all conditions stated?
  - If empirical: is the convergence figure included? Does it show consistent behavior?
- What is the per-iteration computational complexity? Is it stated explicitly?
- Are there degenerate cases where the algorithm may fail (e.g., ill-conditioning, rank deficiency)?
- Are closed-form solutions derived or simply stated?

### Reviewer 2 — Novelty and Positioning
Focus: gap analysis, comparison with state-of-the-art, contribution clarity.

Questions examined:
- Are the claimed gaps real? Read `context/domain.md` and check against literature.
  - Would a [domain] expert agree that no paper has addressed gap G1?
- Are the contributions (Section I-B) specific and verifiable?
  - "We propose X" must be verifiable in Section [N]. "We show Y" must appear in a theorem or figure.
- Is the paper properly positioned against the 3–5 most related works?
  - Are differences with each related work stated precisely?
- Is the proposed method clearly differentiated from ablation baselines?
  - What exactly does the full method have that Baseline 1 lacks?
- Are there any recent papers (within 12 months) that close the claimed gaps that are not cited?

### Reviewer 3 — Experiments and Reproducibility
Focus: simulation setup, baseline fairness, statistical rigor, figure quality.

Questions examined:
- Are all baselines fair?
  - Same total transmit power budget?
  - Same number of antennas / degrees of freedom?
  - Same channel realization for each Monte Carlo trial?
- Is Monte Carlo averaging sufficient?
  - How many realizations? Is this stated in the paper?
  - Are curves smooth, or do they show noise suggesting insufficient averaging?
- Is the simulation setup physically justified?
  - Are the parameter values in Table I (or equivalent) explained or cited?
  - Do the parameter choices match the claimed application scenario?
- Figure quality:
  - Axes labeled with units?
  - Legend entries match baseline names in text?
  - Captions self-contained and accurate?
  - Font size legible at column width?
- Reproducibility:
  - Is the random seed stated?
  - Are all parameters in `MASTER_PLAN.md` Section 11?
- Claims coverage:
  - Does every claim in the abstract have a corresponding result in Section IV?
  - Does every contribution in Section I-B have a corresponding figure or theorem?

### Area Chair Summary

Synthesizes Reviewer 1–3 reports into:
- **Points of consensus:** Issues all reviewers agree must be addressed
- **Points of disagreement:** Trade-offs or matters of opinion between reviewers
- **Minimum required revisions:** Ordered list of what must change for acceptance
- **Recommended decision:** Accept / Minor Revision / Major Revision / Reject

---

## What Claude Code Evaluates (All Modes)

- Problem motivation: is the gap clear and specific?
- Contribution specificity: are claims verifiable in the paper?
- Algorithm correctness: reformulation tight, convergence stated
- Baseline fairness: same resources, same channel
- Simulation parameter justification: physically meaningful values
- Abstract-to-results coverage: every abstract claim has a figure or theorem
- Page limit: within the venue limit from `context/target.md`
- LaTeX quality: no overfull hboxes, no undefined references, no missing labels

---

## After the Review

For each [MAJOR CONCERN]:
1. Add it to `MASTER_PLAN.md` Section 15 (Open TODOs) if not already there
2. Determine if it requires: (a) additional text, (b) new experiments, (c) math addition
3. Address in the next `write-update` or `code-audit` session
4. Re-run the review workflow once major concerns are resolved
