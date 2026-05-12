# context/domain.md — Research Domain and Landscape

> Fill this file before writing the Introduction. The `write-update`, `writing-verify`,
> and `lit-review-audit` workflows read this file as ground truth for the paper's framing.
> Update if the problem framing or literature understanding evolves during writing.

---

## Field and Sub-Domain

- **Field:** [e.g., Wireless Communications / Signal Processing / Machine Learning]
- **Sub-domain(s):** [e.g., ISAC, NOMA, RIS-aided systems, Over-the-Air Computation]
- **Key venues:** [e.g., IEEE TWC, IEEE TSP, IEEE ICC, NeurIPS, ICASSP]

---

## Core Problem

[2–3 sentences. Name the specific technical problem, the system architecture where it arises,
and what makes it hard. Be precise — a reader in a related field should understand this
without knowing your domain in depth.]

---

## Why It Is Unsolved

[What do existing works do, and where specifically do they fail?
Identify the architectural limitation, modeling gap, or algorithmic shortcoming.
Tie each limitation to a specific class of prior work (reference categories below).]

---

## Our Approach

[One sentence. What is the key idea — architectural, algorithmic, or analytical — that
enables this paper to address the problem where prior work could not?]

---

## What Our Approach Achieves

[One sentence on the main result. What performance gain, theoretical result, or
design principle does the paper demonstrate?]

---

## Why Our Approach Is Different

No prior work has:
1. [Gap 1 — precise, specific, defensible]
2. [Gap 2]
3. [Gap 3]

These gaps are verified against `lit-review/summaries.md`. Run `lit-review-audit` to check.

---

## Narrative Arc (Section-to-Section Connectivity)

> Ground truth for `writing-verify` Check 10. Fill in after deciding the paper structure.
> Each row describes what must be established in the From section to set up the To section.

| From | To | Bridge / Callback Required |
|---|---|---|
| Abstract | Introduction | Abstract claim (G1) appears as formal gap statement in Intro |
| Introduction | System Model | Coupling/constraint from Intro motivation is named in System Model |
| System Model | Proposed Method | System structure motivates the algorithm decomposition |
| Proposed Method | Numerical Results | Proposed scheme is labeled consistently in all figures |
| Numerical Results | Conclusion | Key figure result re-stated quantitatively in Conclusion |
| Conclusion | Abstract | Conclusion re-anchors to core problem; no verbatim copy |

Opening-sentence anchors (fill after writing each section):
- Abstract opens with: "[...]"
- Introduction opens with: "[...]"
- System Model opens with: "[...]"
- Conclusion opens with: "[...]"

---

## Existing Literature Categories

> Group all surveyed papers into categories. These categories are used by `lit-review-audit`
> to check coverage. Each category should have ≥ 3 papers in `lit-review/summaries.md`.

| Category | Description | Key papers (CitationKey) |
|---|---|---|
| [Category 1 — e.g., Background / Motivation] | [What this strand covers] | [Key1, Key2, Key3] |
| [Category 2] | | |
| [Category 3] | | |
| [Category 4 — e.g., Optimization Tools] | [General tools / theory] | |
