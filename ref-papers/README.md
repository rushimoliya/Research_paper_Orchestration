# ref-papers/ — Curated Reference Papers

This folder holds **5–15 key reference PDFs** that directly support your paper's claims,
methodology, or baselines. This is a curated selection — not a comprehensive literature dump.

---

## Distinction from lit-review/

| `ref-papers/` | `lit-review/` |
|---|---|
| 5–15 papers, hand-selected | 20–50+ papers, comprehensive sweep |
| Papers you cite directly in main.tex | Papers surveyed for gap analysis |
| Each paper is load-bearing for a claim | Includes papers you may not cite |
| Used during writing (quick lookup) | Used during lit-review-audit workflow |

---

## Organization

Optionally organize by category using a naming prefix:

```
ref-papers/
├── bg_[AuthorYear_Title].pdf         ← Background / motivation
├── bg_[AuthorYear_Title].pdf
├── method_[AuthorYear_Title].pdf     ← Methods you build on
├── method_[AuthorYear_Title].pdf
├── baseline_[AuthorYear_Title].pdf   ← Baseline schemes
└── tool_[AuthorYear_Title].pdf       ← Optimization / estimation tools
```

Or use flat naming if the set is small:

```
ref-papers/
├── Smith2023_RIS_ISAC.pdf
├── Zhang2024_MIMO_AO.pdf
└── Liu2022_CRB_Sensing.pdf
```

---

## What Makes a Good ref-papers Selection

- **Background (2–3):** Papers that establish why your problem exists and why it matters
- **Methods (2–4):** Papers whose algorithms or models you directly use or adapt
- **Baselines (2–4):** Papers that propose the schemes you compare against
- **Tools (1–2):** Optimization theory or estimation bounds papers you cite for supporting results

---

## Usage During Writing

When Claude Code writes or edits LaTeX, mention which papers support each claim by CitationKey.
The AI will look for these keys in `references.bib` and use them appropriately.

Add all papers here first to `references.bib` before starting writing.
