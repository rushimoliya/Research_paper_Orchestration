# target.md — Submission Target and Manuscript Status

> Fill this in before starting. Update the Status table and Checklist each session.
> The `session-audit` and `writing-verify` workflows read this file.

---

## Venue

| Field | Value |
|---|---|
| **Journal / Conference** | [e.g., IEEE Transactions on Wireless Communications] |
| **Acronym** | [e.g., TWC] |
| **Paper type** | [Journal / Conference / Workshop] |
| **Submission deadline** | [YYYY-MM-DD] |
| **Author list** | [Author 1, Author 2, ...] |

---

## Format Requirements

| Field | Value |
|---|---|
| **Page limit** | [e.g., 12 pages double-column, or 6 pages single-column] |
| **LaTeX template** | [e.g., IEEEtran.cls v1.8b / ACM SigConf / Springer LNCS] |
| **Bibliography style** | [e.g., IEEEbib / ACM Ref / APA] |
| **Source file** | `our-paper/main.tex` |
| **Supplement / Appendix** | [Allowed / Not allowed / Max N pages] |
| **Abstract word limit** | [e.g., 150 words] |
| **Keywords required** | [Yes — N to M words / No] |
| **Author bios required** | [Yes / No] |

---

## Current Manuscript Status

| Section | Title | Status |
|---|---|---|
| Abstract | | Pending |
| I | Introduction | Pending |
| II | System Model | Pending |
| III | Proposed Method | Pending |
| IV | Numerical Results | Pending |
| V | Conclusion | Pending |
| — | Appendix / Supplement | Pending |
| — | Bibliography | Pending |

Status options: `Pending` / `In Progress` / `Draft` / `Complete`

---

## Submission Checklist

### Writing
- [ ] Abstract ≤ [venue word limit] words, no citations, no math notation
- [ ] All acronyms defined exactly once on first use
- [ ] All equations referenced with `\eqref{}`
- [ ] All figures referenced as `Fig.~\ref{}` (not "Figure")
- [ ] No section heading deeper than level 3
- [ ] Passive voice for methodology (no "we propose" in abstract)
- [ ] Numbers 1–9 spelled out in prose; 10+ as numerals

### Notation and Math
- [ ] All symbols in `context/notation.md` used consistently throughout
- [ ] No symbol used with two different meanings
- [ ] All claims in abstract are validated by a figure or theorem in results

### Code and Figures
- [ ] All figures are PDF format, ≥ 300 DPI
- [ ] All figure captions are self-contained and match main text references
- [ ] Simulation code in `our-paper/code/` reproduces all figures from `params.py`
- [ ] Baseline simulations use same power budget and channel realizations as proposed

### Bibliography
- [ ] Every `\cite{}` has a matching `\bibitem{}`
- [ ] Bibliography follows venue style (vol., no., pp., year format for IEEE)
- [ ] No orphan citations (in bibliography but not cited in text)
- [ ] Citation keys match summaries in `lit-review/summaries.md`

### Final Pre-Submission
- [ ] LaTeX compiles cleanly (zero errors, zero undefined reference warnings)
- [ ] Page count is within limit (including figures, tables, bibliography)
- [ ] Author bios / photos added (if required by venue)
- [ ] Keywords list added (if required)
- [ ] Cover letter drafted (for journal submissions)
- [ ] Running `writing-verify` workflow: all 10 checks passed
- [ ] Running `review` workflow in 3-panel mode: major concerns addressed
