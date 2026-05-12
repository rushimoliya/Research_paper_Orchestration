# base-paper/ — Most Similar Prior Work

This folder holds **one paper**: the single most similar prior work to your paper.
It is the baseline you will compare against and the system model you are extending.

---

## What Goes Here

```
base-paper/
├── [AuthorYear_ShortTitle].pdf       ← The paper PDF
├── [AuthorYear_ShortTitle].tex       ← LaTeX source (if publicly available)
└── base_code/
    ├── main.py                       ← Reproduced simulation code
    ├── params.py                     ← Parameters matching the paper's Table I
    └── requirements.txt              ← Dependencies for base code
```

---

## Why This Folder Exists

Your paper builds on or extends a specific prior work. Before writing a single line of LaTeX:

1. **Read the base paper end-to-end** — every equation, every assumption, every figure.
2. **Reproduce the code** — your simulation must produce figures that match the paper's.
3. **Understand the math** — if you cannot re-derive a key equation from scratch, you are not ready.

If your reproduced figures do not match the paper's, find out why before proceeding.
A mismatch means either a misunderstood assumption or a different parameter setting.
Both will cause problems when you compare your proposed scheme against this baseline.

---

## Checklist Before Starting Your Paper

- [ ] Base paper PDF added to this folder
- [ ] Key equations understood (annotated or summarized)
- [ ] Code in `base_code/` reproduces the paper's main result figure
- [ ] Simulation parameters in `params.py` match the paper's Table I or equivalent
- [ ] You can explain the base paper's methodology in 3 sentences without referring to it

---

## Note on Citing

The base paper will become your primary citation and comparison baseline.
Add it to `MASTER_PLAN.md Section 12` (Baseline Schemes) as "Base (AuthorYear)" with a
one-line description of what it does and what it does NOT do (your gap).
