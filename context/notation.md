# notation.md — Full Notation and Convention Reference

> Updated by Claude Code whenever new symbols are introduced during writing.
> The `writing-verify` workflow reads this file to check consistency with `main.tex`.
> Build this table incrementally as you write each section.

---

## Notation Table

Group symbols by the section or component where they are introduced.
Add a new category for each major model component (dimensions, channels, signals, etc.).

### [Category 1 — e.g., System Dimensions]

| Symbol | Meaning | Type | Defined in |
|---|---|---|---|
| [sym] | [meaning] | scalar / vector / matrix / set | Sec N |

### [Category 2 — e.g., Channel Model]

| Symbol | Meaning | Type | Defined in |
|---|---|---|---|
| [sym] | [meaning] | scalar / vector / matrix / set | Sec N |

### [Category 3 — e.g., Signal and Noise]

| Symbol | Meaning | Type | Defined in |
|---|---|---|---|
| [sym] | [meaning] | scalar / vector / matrix / set | Sec N |

### [Category 4 — e.g., Optimization Variables]

| Symbol | Meaning | Type | Defined in |
|---|---|---|---|
| [sym] | [meaning] | scalar / vector / matrix / set | Sec N |

### [Category 5 — e.g., Performance Metrics]

| Symbol | Meaning | Type | Defined in |
|---|---|---|---|
| [sym] | [meaning] | scalar / vector / matrix / set | Sec N |

---

## LaTeX Math Conventions

| Object | LaTeX command | Example |
|---|---|---|
| Scalar | italic (default) | `x`, `\alpha`, `\sigma^2` |
| Vector | `\mathbf{x}` (lowercase bold) | `\mathbf{h}`, `\mathbf{w}` |
| Matrix | `\mathbf{X}` (uppercase bold) | `\mathbf{H}`, `\mathbf{W}` |
| Set / index set | `\mathcal{K}` (calligraphic) | `\mathcal{K}_c`, `\mathcal{N}` |
| Operator / acronym | `\mathrm{SINR}` (roman upright) | `\mathrm{MSE}`, `\mathrm{CRB}` |
| Complex conjugate transpose | `^{\mathrm{H}}` | `\mathbf{A}^{\mathrm{H}}` |
| Real part | `\mathfrak{Re}\{...\}` or `\Re\{...\}` | `\Re\{\mathbf{h}^{\mathrm{H}}\mathbf{w}\}` |

---

## LaTeX Referencing Conventions

| Item | Correct form | Wrong form |
|---|---|---|
| Equation reference | `\eqref{eq:label}` | `(1)`, `Eq. (1)` |
| Figure reference | `Fig.~\ref{fig:label}` | `Figure 1`, `fig. 1` |
| Table reference | `Table~\ref{tab:label}` | `table 1`, `Tab. 1` |
| Section reference | `Section~\ref{sec:label}` | `section 3`, `Sec. 3` |
| Citation | `~\cite{Key}` (before period) | `, [1].` (citation after period) |
| Multiple citations | `~\cite{Key1, Key2}` | `[1], [2]` inline |

---

## Acronym Register

Track every acronym here. Each acronym must be defined exactly once (on first use) and used
consistently thereafter. The `writing-verify` workflow checks this table against `main.tex`.

| Acronym | Full name | First defined in |
|---|---|---|
| [ACR] | [Full Name] | Sec N / Abstract |

---

## Consistency Rules

- A symbol introduced in one section must not be redefined in another
- The same physical quantity must always use the same symbol
- Subscript conventions must be consistent (e.g., always `_k` for user index)
- If a symbol appears in a figure caption, it must be defined in the notation table
