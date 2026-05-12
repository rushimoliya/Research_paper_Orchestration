# workflow: math-audit

**Invocation:**
```
Run the math-audit workflow on our-paper/supplement/supplement.tex.
```
Or for a specific section:
```
Run the math-audit workflow on Appendix A of our-paper/supplement/supplement.tex.
```

**Purpose:** Verify mathematical derivations in the supplementary appendix or proof sections.
Run before submission to catch missing steps, dimension errors, and notation mismatches.

---

## What Claude Code Does

1. Read `supplement.tex` (or the specified file) section by section
2. Read `context/notation.md` — verify all symbols are defined
3. Read `our-paper/main.tex` — cross-check equation labels and claimed results
4. For each **Lemma / Proposition / Theorem / Proof**, verify:
   - Each step follows from the previous by a named operation
     (substitution, chain rule, Schur complement, matrix inversion lemma, completing the square, etc.)
   - Dimensions are tracked consistently throughout (shape of every vector and matrix)
   - Complex conjugate / Hermitian transpose operations are correctly applied
   - Real-part operator ℜ{·} applied only to real-valued quantities
   - Final result matches what is claimed in `main.tex`
5. For each **equation**, verify:
   - LaTeX matches the notation table (correct macros, correct typography)
   - All symbols defined before use (or referenced to main.tex where defined)
   - Labels match cross-references in `main.tex`
6. Flag "skipped steps" — jumps between equations where intermediate algebra is missing

---

## Key Derivations to Check (Fill In Per Paper)

> Replace this table with the actual derivations in your supplement.
> Reference the equation numbers from your main.tex and supplement.tex.

| Derivation | Location | What to Verify |
|---|---|---|
| [Name — e.g., Optimal combiner derivation] | App A, eq (A.1)–(A.5) | [KKT conditions → closed-form; check rank/invertibility] |
| [Name — e.g., SOCP reformulation] | App B | [Epigraph intro; rotated SOC algebraic check] |
| [Name — e.g., Convergence proof] | App C | [Monotone non-increasing argument; boundedness below] |
| [Name — e.g., FIM derivation] | App D | [Block structure; Schur complement; final scalar trace] |

---

## Supplement Structure Enforced

```latex
\begin{lemma}
  [Statement — precise, mathematical, self-contained]
\end{lemma}

\begin{proof}
  Step 1: [Operation name] — ...
  Step 2: [Operation name] — ...
  ...
  \hfill\blacksquare
\end{proof}
```

Requirements:
- QED marker: `\hfill\blacksquare`
- Each step named (not just "we have")
- Cross-references to main.tex via `\eqref{eq:label}` (not hard-coded numbers)
- Every symbol defined before first use in supplement, or cross-referenced to main.tex

---

## Common Mathematical Errors to Flag

| Error type | Description |
|---|---|
| Dimension mismatch | `A ∈ ℂ^{m×n}` multiplied by `B ∈ ℂ^{n×k}` to give `ℂ^{m×k}` — track all shapes |
| Missing rank assumption | `A^{-1}` written without verifying A is invertible |
| Incorrect Hermitian | `A^T` written where `A^H` (conjugate transpose) is required |
| ℜ{·} on complex quantity | Taking real part where the full complex expression is needed |
| Schur complement sign | Off-diagonal block sign error in block matrix inversion |
| Improper KKT | Missing complementary slackness or dual feasibility condition |
| Circular reasoning | Step N relies on Step N+2 |

---

## Output Format

```
═══════════════════════════════════════════════
MATH AUDIT — [PAPER SHORT NAME] — [Date]
═══════════════════════════════════════════════

== NOTATION ERRORS ==
[NOTATION ERROR] supplement.tex:34 — symbol ρ used but not defined in supplement;
  add cross-reference to main.tex eq (X) where it is defined

== DIMENSION ERRORS ==
[MATH ERROR] supplement.tex:78 — Step 3: Tr(AB) written but A ∈ ℂ^{m×n}, B ∈ ℂ^{n×m};
  valid only if product is square — confirm dimensions

== MISSING STEPS ==
[MISSING STEP] supplement.tex:102 — Jump from eq (A.5) to (A.6): Schur complement
  of 4×4 block matrix applied without showing intermediate 2×2 result;
  add one intermediate equation

== VALID DERIVATIONS ==
✓ [Derivation name] (lines X–Y): all steps valid, dimensions consistent
✓ [Derivation name] (lines X–Y): KKT conditions correctly applied

─────────────────────────────────────────────
SUMMARY: N errors; M missing steps; K notation issues
═══════════════════════════════════════════════
```

Severity:
- `[MATH ERROR]` — incorrect step; must be fixed before submission
- `[MISSING STEP]` — correct but incomplete; reviewer may ask for clarification
- `[NOTATION ERROR]` — symbol mismatch; fix automatically where possible
- `✓` — derivation verified correct
