# workflow: session-audit

**Invocation:** `Run the session-audit workflow.`

**Purpose:** Start-of-session status check. Reads MASTER_PLAN.md and SESSION_README.md,
verifies context files are current, and produces a structured status report with the
recommended action for this session.

Run this at the beginning of every session before doing any writing or coding.

---

## Execution Steps

### Step 1 — Load Project State

Read the following files completely:
1. `MASTER_PLAN.md` — full content (paper identity, section statuses, workflow checklist, TODOs)
2. `SESSION_README.md` — last session summary and open TODOs
3. `context/domain.md` — domain, problem, novelty
4. `context/target.md` — venue, format, checklist
5. `context/idea.md` — contributions and narrative arc
6. `context/notation.md` — symbol table

### Step 2 — Manuscript Section Status

From MASTER_PLAN.md Section 10 (Paper Structure), report each section's status:

```
MANUSCRIPT STATUS
─────────────────────────────────────────────
Abstract          [status]
I. Introduction   [status]
II. System Model  [status]
III. Method       [status]
IV. Results       [status]
V. Conclusion     [status]
Supplement        [status]
Bibliography      [status refs count]
─────────────────────────────────────────────
```

Use ✅ for Complete, ⚠️ for In Progress / Draft, ❌ for Pending.

### Step 3 — Open TODOs

List all open TODOs from SESSION_README.md in priority order:

```
OPEN TODOs
─────────────────────────────────────────────
1. [todo item] — [why it's high priority]
2. [todo item]
3. [todo item]
─────────────────────────────────────────────
```

### Step 4 — Workflow Checklist

From MASTER_PLAN.md Section 13 (Workflow Checklist), report which workflows are pending:

```
WORKFLOW CHECKLIST
─────────────────────────────────────────────
✅ session-audit      — Running now
⚠️ write-update       — [last run / pending]
❌ writing-verify     — Not yet run
❌ lit-review-audit   — Not yet run
❌ code-audit         — Not yet run
❌ math-audit         — Not yet run
❌ review (3-panel)   — Not yet run
✅ sync-session       — Last run: Session [N]
─────────────────────────────────────────────
```

### Step 5 — Context File Currency Check

Verify that the context files are up-to-date relative to the current manuscript state:

- **domain.md:** Does it reflect the current problem framing and literature categories?
- **target.md:** Is the section status table current? Is the checklist up to date?
- **idea.md:** Do the contributions match what is actually in main.tex?
- **notation.md:** Does it contain all symbols currently in main.tex?

Flag any discrepancies as ⚠️ [STALE: reason].

### Step 6 — Recommended Next Action

Output a single, specific recommendation for this session:

```
RECOMMENDED ACTION FOR THIS SESSION
─────────────────────────────────────────────
[One specific task, e.g.:]
"Run the writing-verify workflow on our-paper/main.tex.
 Then address the top 3 issues from the report."

[Or:]
"Complete Section III (Proposed Method).
 Context: the problem formulation in main.tex ends at Eq. (5).
 Next: write the AO algorithm derivation (Sec. III-A)."
─────────────────────────────────────────────
```

---

## Output Format

```
═══════════════════════════════════════════════
SESSION AUDIT — [PAPER SHORT NAME] — [Date]
═══════════════════════════════════════════════

[Step 2 output: Manuscript Status]

[Step 3 output: Open TODOs]

[Step 4 output: Workflow Checklist]

[Step 5 output: Context Currency Check]

[Step 6 output: Recommended Action]

═══════════════════════════════════════════════
```

---

## After the Audit

Once the audit is complete and you have reviewed the report:
- Accept the recommended action or specify a different task
- Begin that task using the appropriate workflow or direct instruction
- End the session with: `Run the sync-session workflow. Session summary: [what you did].`
