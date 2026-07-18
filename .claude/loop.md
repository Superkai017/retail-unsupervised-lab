# Autonomous loop guidance — retail-unsupervised-lab

This is a single-notebook **learning lab** (`unsupervised.ipynb`), not a production system.
The user drives the modeling work themselves, cell by cell. Steward their work; do not do it for them.

## Do

- Fix things the user explicitly asked for and left unfinished in the conversation.
- Diagnose and fix errors the user surfaced (e.g. dtype/parse errors), following the transcript.
- Keep `README.md`, `CLAUDE.md`, and `requirements.txt` accurate when code changes make them stale.
- When asked to commit: stage **named files only** (never `git add -A`), and always exclude `data/`.

## Don't

- **Don't write the modeling pipeline for them** — scaling, k-selection, K-Means, PCA, cluster
  interpretation are the learning exercise. Building these uninvited defeats the point.
- **Don't commit or push without explicit approval.** The user controls commits deliberately.
  Uncommitted notebook edits are their live work-in-progress — leave them untouched.
- **Don't commit `data/`** — the raw CSV (~44 MB) is gitignored on purpose.
- **Don't make modeling decisions** (choice of k, cluster labels) autonomously.

## Notebook gotchas to respect

- Cell order is load-bearing; `df` is mutated in place across cells. The correct fix after edits is
  **Kernel → Restart & Run All**, not re-running one cell.
- `InvoiceDate` is parsed at load with `format='%m/%d/%y %H:%M'` (2-digit year, month-first).

## When quiet

There's no PR and no CI here (direct pushes to `main`, `gh` not installed). If the conversation has
nothing unfinished, say so in one line and stop — don't invent work.
