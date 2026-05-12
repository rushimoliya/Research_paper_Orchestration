# workflow: write-update

**Invocation:**
```
Run the write-update workflow. I want to [describe the change you want].
```

**Purpose:** Write new content, edit an existing section, restructure, or fix style
issues in `our-paper/main.tex`. Enforces rules from `skills/writing.md` automatically.

---

## Example Invocations

```
Run the write-update workflow. Write Section III-A on the proposed algorithm.
Run the write-update workflow. Rewrite the Introduction to sharpen the gap statement.
Run the write-update workflow. Fix all instances where bare (N) is used instead of \eqref.
Run the write-update workflow. Add a transition paragraph between Sections II and III.
Run the write-update workflow. Tighten the abstract to under [N] words.
```

---

## What Claude Code Does (in order)

1. Read `MASTER_PLAN.md` — full project context, paper identity, notation
2. Read all `context/` files — domain.md, idea.md, notation.md, target.md
3. Read active `skills/` files — writing.md, and domain-specific skills as needed
4. Open the relevant section(s) of `our-paper/main.tex`
5. Make the requested changes
6. Verify consistency before saving:
   - Every acronym defined on first use, not redefined later
   - Every symbol matches `context/notation.md`
   - All equation refs use `\eqref{}`, no bare `(N)`
   - All figure refs use `Fig.~\ref{}`
   - Citations placed before punctuation with non-breaking space
   - Narrative connectivity matches `context/idea.md` arc
7. Update `CHANGELOG.md` — append a one-line description of changes
8. Summarize: what was changed, what was auto-fixed, what may need manual review

---

## Writing Rules Enforced

### Equation References
- `\eqref{eq:label}` always — never `(1)` alone
- Preceded by non-breaking space: `~\eqref{eq:label}`

### Figure References
- `Fig.~\ref{fig:label}` — never "Figure" in full, never "fig."

### Acronyms
- First use: `Full Name (ACRONYM)`
- Subsequent: acronym only, no re-expansion
- Check against `context/notation.md` Acronym Register

### Sentence Structure
- No sentence starts with a math symbol
- Passive voice for methodology steps

### Citations
- Before punctuation: `...framework~\cite{key}.`
- Non-breaking space: `~\cite{key}`

### Math Conventions
- `\mathrm{...}` for operator subscripts (metric names, function names)
- `\mathbf{x}` vectors, `\mathbf{X}` matrices, italic `x` scalars
- `\mathcal{K}` index sets

### Narrative Connectivity
- Read `context/idea.md` Narrative Arc table before writing transition paragraphs
- Abstract claims must appear as formal statements in Introduction
- Section N conclusion must set up Section N+1 opening

---

## Output

- Modified `our-paper/main.tex` (saved in place)
- Updated `CHANGELOG.md` (one entry appended)
- Summary: lines changed, auto-fixes applied, manual items flagged
