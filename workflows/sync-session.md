# workflow: sync-session

**Invocation:**
```
Run the sync-session workflow. Session summary: [brief description of what was done].
```

**Purpose:** End-of-session sync. Updates context files, overwrites SESSION_README.md,
appends to CHANGELOG.md, archives a snapshot, and commits + pushes to GitHub.

Run at the end of **every** working session.

---

## Example Invocations

```
Run the sync-session workflow. Session summary: Wrote Section III (Proposed Method); added 8 symbols to notation.md; fixed 2 acronym errors from writing-verify.

Run the sync-session workflow. Session summary: Implemented all 4 baselines in code; ran code-audit; fixed 3 issues; all figures generate successfully.
```

---

## What Claude Code Does (in order)

### Step 1 — Update context/ Files

Reflect the current state of the manuscript:
- `context/domain.md` — if problem framing or literature categories changed
- `context/idea.md` — if contributions, claims, or narrative arc changed
- `context/notation.md` — if new symbols were introduced or definitions changed
- `context/target.md` — update section status table to reflect what was completed

Only update a file if something actually changed. Do not touch files that are current.

### Step 2 — Overwrite SESSION_README.md

Use the `SESSION_README_template.md` format:

```markdown
# SESSION_README.md — Last Updated: YYYY-MM-DD Session NNN

## What Was Done This Session
[Bullet summary from the session summary provided]

## Current Manuscript Status
| Section | Title | Status |
|---|---|---|
[From MASTER_PLAN.md Section 10 — current statuses]

## Open TODOs
[From MASTER_PLAN.md Section 15 — pending items]

## Next Session: Start Here
Run the session-audit workflow.
Then: [specific next task]

## Last Git Commit
[hash — message]
```

### Step 3 — Append to CHANGELOG.md

Add one entry:

```markdown
## Session NNN — YYYY-MM-DD — [One-line summary]

### Added
- [New files, sections, figures]

### Changed
- [Modified existing content]

### Fixed
- [Errors corrected, audit issues resolved]

### Git
- Commit: `[hash]` — "[message]"
```

### Step 4 — Archive the Session

```bash
SESSION_DATE=$(date +%Y-%m-%d)
SESSION_NUM=$(printf "%03d" $(ls archive/ | grep -c session))
FOLDER="archive/session_${SESSION_DATE}_${SESSION_NUM}"
mkdir -p "$FOLDER/code_snapshot"

cp our-paper/main.tex "$FOLDER/main_snapshot.tex"
cp -r our-paper/figures/ "$FOLDER/figures_snapshot/" 2>/dev/null || true
# Supplement (if it exists)
cp our-paper/supplement/supplement.tex "$FOLDER/supplement_snapshot.tex" 2>/dev/null || true
# Code
cp -r our-paper/code/ "$FOLDER/code_snapshot/" 2>/dev/null || true

echo "Archived to $FOLDER"
```

Archive entries are **read-only** after creation — never modify files in `archive/`.
To revert: `cp archive/session_YYYY-MM-DD_NNN/main_snapshot.tex our-paper/main.tex`

### Step 5 — Git Commit and Push

```bash
git add -A
git commit -m "Session $SESSION_NUM: [summary from invocation]"
git push origin main
```

Commit message format: `"Session NNN: [one-line summary]"`

### Step 6 — Report

Output:
- Files changed (list)
- Git commit hash
- Archive folder name
- Any context files that were updated

---

## Notes

- Git provides fine-grained per-file tracking; archives provide complete session snapshots
- If git push fails (no remote configured), log the error and continue — commit is still saved locally
- Always update MASTER_PLAN.md Section 16 (Session Log) as part of Step 1
