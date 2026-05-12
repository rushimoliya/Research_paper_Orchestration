# workflow: lit-review-audit

**Invocation:** `Run the lit-review-audit workflow.`

**Purpose:** Systematic audit of the literature review. Checks inventory coverage,
gap authenticity, citation accuracy, and whether key papers are actually cited in main.tex.
Run after adding papers and before writing the Introduction.

---

## Execution Steps

### Phase 1 — Inventory Check

1. List all PDF files in `lit-review/`
2. List all entries in `lit-review/summaries.md`
3. Report:
   - PDFs with no summary entry → **[MISSING SUMMARY]**
   - Summary entries with no corresponding PDF → **[MISSING PDF]**
   - Total: N papers with summaries / M PDFs in folder

**Score: /10** — subtract 2 points per missing summary; 1 point per missing PDF.

### Phase 2 — Coverage Audit

Read `context/domain.md` to find the list of literature categories.

For each category:
- Count how many papers in `summaries.md` belong to it
- Flag categories with < 3 papers as **[SPARSE COVERAGE]**
- Flag categories listed in domain.md but with zero entries as **[UNCOVERED CATEGORY]**

**Score: /10** — subtract 2 points per uncovered category; 1 point per sparse category.

### Phase 3 — Gap Authenticity Check

Read `context/domain.md` and `context/idea.md` to find the claimed research gaps.

For each claimed gap (e.g., "No prior work has addressed X"):
- Search `summaries.md` for papers that might address X
- If a paper summary says it addresses X: **[GAP CHALLENGED: CitationKey — reason]**
- If no paper challenges it: **[GAP CONFIRMED: X]**

This is the most important phase. A challenged gap requires either:
(a) Revising the gap claim to be more specific, or
(b) Explaining why that paper does not actually close the gap.

**Score: /10** — subtract 3 points per challenged gap without resolution.

### Phase 4 — Citation Accuracy Check

For a random sample of 10 papers (or all if < 10), verify:
- Paper title in `summaries.md` matches the PDF filename / actual paper
- Author and year correct
- Venue correct (journal name, conference name, year)

Report: **[INACCURATE: CitationKey — what is wrong]**

**Score: /10** — subtract 1 point per inaccuracy found.

### Phase 5 — Usage in main.tex

Read `our-paper/main.tex` (if it exists).

For each literature category in `context/domain.md`:
- Is there at least one citation from that category in the Introduction?
- Are the most important papers in the category cited?

Flag: **[UNCITED CATEGORY: category name — no citations in Introduction]**
Flag: **[KEY PAPER UNCITED: CitationKey — this is the foundational paper for its category]**

**Score: /10** — subtract 2 points per uncited category in Introduction.

---

## Scoring Rubric

| Phase | Max Score | Description |
|---|---|---|
| Inventory | 10 | All PDFs have summaries; all summaries have PDFs |
| Coverage | 10 | All literature categories populated |
| Gap authenticity | 10 | No claimed gaps contradicted by existing papers |
| Citation accuracy | 10 | Titles, authors, years, venues verified |
| Usage in main.tex | 10 | Key categories cited in Introduction |
| **Total** | **50** | |

Adjust the rubric weights in MASTER_PLAN.md if needed for your paper's priorities.

---

## Output Format

```
═══════════════════════════════════════════════
LIT-REVIEW AUDIT — [PAPER SHORT NAME] — [Date]
═══════════════════════════════════════════════

PHASE 1 — INVENTORY
Papers with summaries: N / PDFs in folder: M
[MISSING SUMMARY] list
[MISSING PDF] list
Score: X/10

PHASE 2 — COVERAGE
[Coverage report per category]
[SPARSE COVERAGE] / [UNCOVERED CATEGORY] list
Score: X/10

PHASE 3 — GAP AUTHENTICITY
Gap 1: "[gap statement]" → [CONFIRMED / CHALLENGED: reason]
Gap 2: "[gap statement]" → [CONFIRMED / CHALLENGED: reason]
Score: X/10

PHASE 4 — CITATION ACCURACY
[INACCURATE] items
Score: X/10

PHASE 5 — USAGE IN MAIN.TEX
[UNCITED CATEGORY] / [KEY PAPER UNCITED] items
Score: X/10

─────────────────────────────────────────────
TOTAL SCORE: XX/50
ACTION ITEMS: [numbered list of fixes, highest priority first]
═══════════════════════════════════════════════
```

---

## After the Audit

1. Apply [AUTO-FIX] items immediately (typos, missing entries in summaries.md)
2. Review [GAP CHALLENGED] items carefully — these require researcher judgment
3. Add missing PDFs to lit-review/ and summaries.md entries as needed
4. Log changes in CHANGELOG.md under "Fixed"
5. Re-run audit if score < 40/50 before writing the Introduction
