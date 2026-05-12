# workflow: writing-verify

**Invocation:**
```
Run the writing-verify workflow on our-paper/main.tex.
```

**Purpose:** Comprehensive style and consistency audit of the manuscript.
Run after completing any major section, and always before submission.

---

## What Claude Code Checks (10 checks)

### Check 1 — Acronyms
- Every acronym defined exactly once on first use: `Full Name (ACRONYM)`
- Never redefined later in the paper
- Every acronym in the paper appears in `context/notation.md` Acronym Register
- Flag: `"[ACRONYM] defined at line X and again at line Y — delete second definition"`

### Check 2 — Notation
- Every symbol used in main.tex appears in `context/notation.md`
- No undefined symbols (used without prior definition)
- No symbol used with two different meanings
- Flag: `"Symbol [X] at line Y not in notation table — add to context/notation.md"`

### Check 3 — LaTeX Style
- All equation references use `\eqref` — flags bare `(N)` patterns
- Vectors use `\mathbf` lowercase; matrices use `\mathbf` uppercase; scalars italic
- Sets use `\mathcal`; operator names use `\mathrm{...}`
- Non-breaking space `~` before `\cite`, `\ref`, `\eqref`
- No "Figure" — only `Fig.~\ref{...}`
- Citations placed before punctuation, not after

### Check 4 — Section Structure
- Section/subsection headers follow venue conventions
- No heading deeper than `\subsubsection`
- No orphan subsections (a section with only one subsection)

### Check 5 — Bibliography
- Every `\cite{key}` has a matching entry in `references.bib`
- No orphan bibliography entries (defined but never cited)
- Citation keys in main.tex match `lit-review/summaries.md` entries

### Check 6 — Terminology Consistency
- Read `skills/writing.md` Terminology Lock section
- Flag any use of deprecated synonyms for locked terms
- Flag any inconsistency in methodology name (e.g., different names for same algorithm)

### Check 7 — Redundancy
- Flag any phrase used ≥ 3 times without intentional bridging
- Callbacks (Conclusion ↔ Abstract) do not count as redundancy
- Flag: `"Phrase '[X]' appears N times at lines [...] — consider bridging"`

### Check 8 — Bibliography Style
- Verify venue format (IEEE: vol. X, no. Y, pp. A--B, Mon. Year)
- Non-breaking tildes: `vol.~X`, `no.~Y`, `pp.~A--B`
- Double-hyphen for page ranges: `pp.~10--20`
- No commented-out bibliography entries left in the file

### Check 9 — Sentence Length
- Flag prose sentences exceeding 4 lines in estimated compiled output
- Flag: `"Long sentence at line X ([word count] words) — consider splitting"`

### Check 10 — Narrative Connectivity
Read `context/idea.md` Narrative Arc table as ground truth. Verify:

**A. Abstract ↔ Introduction:** Claims in abstract appear as formal statements in intro gap/contribution list.

**B. Introduction → System Model:** Motivation from intro references at least one system component defined in System Model.

**C. System Model → Proposed Method:** System structure (coupling, constraint, architecture) motivates the algorithm decomposition in Method section.

**D. Results Opening → Introduction Claims:** The first paragraph of Numerical Results names which contribution (from Section I) is being validated.

**E. Convergence / Key Figure → Algorithm:** Figure that shows convergence or key gain is traced back to the mechanism described in Proposed Method.

**F. Conclusion ↔ Abstract:**
- F1: Conclusion re-anchors to the core problem from abstract
- F2: Role-separation or key insight from Method appears in Conclusion as earned callback
- F3: Conclusion does not copy abstract verbatim
- F4: Conclusion contains one broader implication or future direction

**G. Abstract ↔ System Model:** Prose descriptions in abstract map to formal terms in System Model; abstract contains no math notation.

---

## Output Format

```
═══════════════════════════════════════════════
WRITING VERIFY — [PAPER SHORT NAME] — [Date]
═══════════════════════════════════════════════

== CHECK 1: ACRONYMS ==
[AUTO-FIX] Line 45: "XYZ" used before definition at line 12 — moving definition
[MANUAL] Line 102: "ABC" redefined; original at line 8 — delete?

== CHECK 2: NOTATION ==
[AUTO-FIX] Line 67: \mathit{MSE} → \mathrm{MSE}
[MANUAL] Line 134: Symbol ρ used but not in notation.md — add to table?

== CHECK 3: LATEX STYLE ==
[AUTO-FIX] Line 89: "(12)" → "~\eqref{eq:sinr}"
[AUTO-FIX] Line 201: "Figure 3" → "Fig.~\ref{fig:pareto}"

== CHECK 4: STRUCTURE ==
✓ No headings deeper than \subsubsection

== CHECK 5: BIBLIOGRAPHY ==
[MANUAL] \cite{Smith2024} cited but no matching \bibitem — add to references.bib

== CHECK 6: TERMINOLOGY ==
[MANUAL] Line 77: "block coordinate descent" — use "[locked term]" per skills/writing.md

== CHECK 7: REDUNDANCY ==
[MANUAL] "highly non-convex" appears 4 times at lines 12, 45, 89, 201

== CHECK 8: BIBLIOGRAPHY STYLE ==
[AUTO-FIX] Line 312: "pp. 100-120" → "pp.~100--120"

== CHECK 9: SENTENCE LENGTH ==
[MANUAL] Line 156: 68-word sentence — consider splitting

== CHECK 10: NARRATIVE CONNECTIVITY ==
[MANUAL] A: Abstract claims X but Introduction lists Y — align contribution wording
✓ B: System Model intro references the coupling structure from Introduction
✓ C: Method section opens by referencing the system decomposition
[MANUAL] F3: Conclusion paragraph 1 copies abstract lines 2–3 verbatim — paraphrase

─────────────────────────────────────────────
SUMMARY: N auto-fixes applied; M manual items require your decision
═══════════════════════════════════════════════
```

Severity:
- `[AUTO-FIX]` — applied immediately
- `[MANUAL]` — requires researcher decision before fixing
- `✓` — check passed
