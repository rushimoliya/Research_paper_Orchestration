# skills/writing.md — Academic Writing Standards (IEEE / Technical Venues)

> Generic writing rules for technical papers. Applies to journal and conference submissions.
> Venue-specific rules (word limits, style class) go in `context/target.md`.

---

## Voice and Style

- Passive voice for methodology (standard IEEE norm): "is formulated", "is derived", "is solved"
- No "we" in abstract — use "this paper" or passive voice
- "we" is acceptable in the body text
- No sentence starts with a math symbol — always precede with a noun phrase
  - Wrong: "$\mathbf{W}$ is the beamformer"
  - Correct: "The beamformer $\mathbf{W}$ is..."
- Numbers in prose: spell out one through nine, use numerals for 10+

## Acronyms

- All acronyms: defined exactly once on first use — `Full Name (ACRONYM)`
- Never redefine an acronym after it has been introduced
- Subsequent occurrences: acronym only, no re-expansion
- Abstract: spell out all acronyms in full (even if defined later in intro)
- Track all acronyms in `context/notation.md` Acronym Register

## Figures and Tables

- Figures: `Fig.~\ref{fig:label}` — never "Figure X", never "fig. X"
- Tables: `Table~\ref{tab:label}` — never "TABLE X"
- Caption: self-contained; identifies what is swept vs. fixed
- Non-breaking space `~` always before `\ref`

## Equations

- `\eqref{eq:label}` always — never bare `(1)`, never `Eq. (1)`
- No period or comma inside `\eqref`
- In text: "in~\eqref{eq:crb}" or "by~\eqref{eq:sinr}"

## Abstract Rules

- ≤ [venue word limit — fill in from context/target.md] words
- No citations
- No math notation (exception: very standard symbols like "dB" or specific scalars)
- No footnotes
- Passive voice throughout

## Section Headers

- `\section{}`, `\subsection{}`, `\subsubsection{}` — never go deeper
- IEEE style: capitalize all major words in section headers
- Capitalize first word of subsubsection headers

## Units and Values

- dB values: always "X dB" with a space
- Units always paired with values: "5 m", "[freq] GHz", "[power] dBm"

## Citations

- Placement: before punctuation — `...framework~\cite{key}.` not `...framework.~\cite{key}`
- Non-breaking space before `\cite`: `~\cite{key}`
- Multiple citations: `~\cite{key1, key2, key3}`

## LaTeX Math Conventions

| Object | Command | Example |
|---|---|---|
| Scalar | plain italic | `x`, `\alpha`, `\sigma^2` |
| Vector | `\mathbf{x}` (lowercase bold) | `\mathbf{h}`, `\mathbf{w}` |
| Matrix | `\mathbf{X}` (uppercase bold) | `\mathbf{H}`, `\mathbf{W}` |
| Set / index set | `\mathcal{K}` (calligraphic) | `\mathcal{K}_c`, `\mathcal{N}` |
| Operator name | `\mathrm{SINR}` (roman upright) | `\mathrm{MSE}`, `\mathrm{Tr}` |
| Text in math | `\text{...}` | `\text{s.t.}` |
| Operator subscript | `\mathrm{...}` not `\text{...}` | `x_{\mathrm{opt}}` |

## Terminology Lock (Fill In Per Paper)

> Add paper-specific terminology rules here.
> The `writing-verify` workflow will flag violations.

- Preferred term: "[term]" — avoid synonym "[synonym]"
- Preferred abbreviation: "[ABBREV]" — defined once, used consistently
- [Add rows for each methodology term, architecture name, etc.]

## Redundancy Rules

- Cap at 2 total uses of any key phrase without bridging language
- Callbacks (conclusion ↔ abstract) are allowed even if word-for-word, but mark them as intentional
- "Highly non-convex", "challenging", "intractable" — use sparingly; quantify when possible

## Narrative Connectivity (Fill In from context/idea.md)

> Ground truth for the `writing-verify` Check 10.
> Fill in the section-to-section callback anchors after writing context/idea.md.

The paper follows this connectivity arc:

| From | To | Required callback / bridge |
|---|---|---|
| Abstract | Introduction | [Abstract claim must appear in Intro as formal statement] |
| Introduction | System Model | [Problem motivation must reference the model component] |
| System Model | Proposed Method | [System structure motivates the algorithm decomposition] |
| Proposed Method | Numerical Results | [Algorithm is the proposed scheme in all figures] |
| Numerical Results | Conclusion | [Key figure results are re-stated in Conclusion] |
| Conclusion | Abstract | [Conclusion re-anchors to opening motivation; no verbatim copy] |

Opening sentence anchors (fill from context/idea.md):
- Abstract opens with: "[...]"
- Introduction opens with: "[...]"
- System Model opens with: "[...]"
- Conclusion opens with: "[...]"

## Common Mistakes to Avoid

| Mistake | Correct form |
|---|---|
| `(1)` instead of `\eqref{eq:label}` | `~\eqref{eq:label}` always |
| "Figure 3" | "Fig.~\ref{fig:label}" |
| Starting sentence with symbol | Add noun before symbol |
| Redefining acronym | Define once, use consistently |
| `\mathit{MSE}` for operators | `\mathrm{MSE}` |
| Missing `~` before `\cite` | `~\cite{key}` |
| Citation after period | Citation before period |
