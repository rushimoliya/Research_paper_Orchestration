# context/idea.md — Core Contributions and Technical Claims

> Fill this file before writing the Introduction contribution list.
> The `write-update`, `writing-verify`, and `review` workflows use this as ground truth
> for the paper's narrative arc and contribution coverage.
> Update if claims are refined during writing or verification.

---

## Main Novelty Claim

[One sentence: what is the single most important thing this paper contributes that no prior
work has done? This should match the first bullet of the Introduction contribution list.]

---

## Contributions (matching Introduction bullets exactly)

### Contribution 1 — [Short name, e.g., "Bidirectional Interference Model"]
[2–3 sentences. Describe what is proposed, what problem it solves, and what makes it
non-trivial. Should be specific enough to distinguish from related work.]

### Contribution 2 — [Short name, e.g., "Proposed Architecture / Algorithm"]
[2–3 sentences.]

### Contribution 3 — [Short name, e.g., "Convergence Analysis / Tractability Result"]
[2–3 sentences. If this is a theoretical contribution, state the theorem/lemma and where
in the paper it appears.]

### Contribution 4 — [Short name, e.g., "Simulation Validation"] (if applicable)
[2–3 sentences. State what figures validate which claims, and what the key quantitative
results are.]

---

## What Has Been Proved (and Where)

> Track the correspondence between paper claims and their evidence.
> The `review` workflow checks that every abstract claim appears in this table.

| Claim | Evidence type | Location in paper |
|---|---|---|
| [Claim from abstract, verbatim] | Theorem / Figure / Corollary | Sec N / Fig. X / Eq. (Y) |
| [Claim 2] | | |
| [Claim 3] | | |

---

## Narrative Arc (Section-to-Section Connectivity)

> Ground truth for `writing-verify` Check 10.
> Fill in after finalizing the paper structure. Claude uses this to enforce
> that each section logically sets up the next.

| Transition | Required bridge or callback |
|---|---|
| Abstract → Introduction | Abstract claim [C] appears as formal gap statement (Section I-A); role-separation insight in Section I-B |
| Introduction → System Model | Coupling / interference described in Intro appears as formal equation in System Model |
| System Model → Proposed Method | System decomposition (e.g., Tx/Rx independence) directly motivates the block structure of the algorithm |
| Proposed Method → Numerical Results | The "proposed" label in all figures refers to the full method described in Section III |
| Key figure → Method | Convergence figure or gain figure references the mechanism from Section III |
| Conclusion → Abstract | Conclusion paragraph 1 re-anchors to the core problem from Abstract; no verbatim copy; paragraph 2 contains forward-looking implication |

---

## Opening-Sentence Anchors

> Set these after writing each section. Used by `writing-verify` Check 10G.

| Section | Required opening anchor |
|---|---|
| Abstract | "[sentence describing the problem and system]" |
| Introduction | "[sentence establishing the domain and why the problem matters]" |
| System Model | "[sentence naming the proposed architecture and its components]" |
| Proposed Method | "[sentence connecting back to the system structure]" |
| Numerical Results | "[sentence naming which contribution is validated first]" |
| Conclusion | "[sentence re-anchoring to the core problem and solution]" |

---

## What Is Pending

> Keep this current. Use as input to MASTER_PLAN.md Section 15 (Open TODOs).

- [ ] [item — e.g., "Supplement derivation for Contribution 3"]
- [ ] [item — e.g., "Run writing-verify on Section IV"]
- [ ] [item — e.g., "Verify all abstract claims appear in results figures"]
