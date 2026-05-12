# lit-review/ — Comprehensive Literature

This folder holds **all PDFs reviewed** during your literature survey, plus the master
summary file `summaries.md`. The goal is a systematic, auditable literature coverage.

---

## Structure

```
lit-review/
├── summaries.md          ← Master summary file (one entry per paper)
├── [paper1].pdf
├── [paper2].pdf
└── ...
```

---

## Workflow

1. **Add PDFs** as you find them — any paper related to your domain, even tangentially.
2. **Add a summaries.md entry** immediately after adding each PDF (use the 5-field format).
3. **Run `lit-review-audit`** after adding a batch of papers to check coverage and accuracy.

---

## Organization of summaries.md

Group entries by literature category (defined in `context/domain.md` Section "Existing Literature
Categories"). Each category represents a strand of prior work your paper relates to.

Example categories:
- Background / Motivation
- [System Type A] fundamentals
- [System Type A] for [Application]
- [System Type B] for comparison
- Optimization Tools
- Estimation Theory

---

## When to Run lit-review-audit

- After adding 3+ new papers
- Before writing the Introduction (ensures gap claims are accurate)
- Before submission (final accuracy check)

Invocation: `Run the lit-review-audit workflow.`

---

## Note on Comprehensiveness

The lit-review/ folder is for YOUR awareness and gap analysis.
The papers you actually cite go into `references.bib`.
Not every paper in lit-review/ will be cited — that is fine and expected.
