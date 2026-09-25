# Trap log

One entry per data issue that changed a number, whether or not your own query was ever wrong. Copy the kind of entry
you need, once per issue, under a heading that names it: `## 1 — <what it was>`, `## 2 — …`.

**Cite, do not paste again.** Every code cell in the notebook starts with its ID: `I1`, `I2`, … (inspection), `S` (the
sales view), `Q1` … `Q9` (each question's query), `Q1 check` … `Q9 check`, and `R` (the reconciliation). Where the
evidence is still in the notebook, write the cell's ID and quote the one line of its output that matters. **Paste
only what the notebook no longer shows**: the query and its output from before a repair. Paste it *before* you change
the query; the fix erases it.

---

## A trap your query fell into: all five parts

1. **Symptom.** The wrong number, and what showed it was wrong (a check, a census, the brief).

2. **Cause.** Which lines, and why the query counted them. Not what you changed.

3. **Evidence.** The query and its output from before the repair, pasted; or an inspection cell's ID and the line that
   shows the cause.

```text

```

4. **Change.** The cell that holds the query now (`Q…`), and one sentence on what changed.

5. **Verification.** The check cell (`Q… check`) or `R`, the identity it tests (a sum, a ratio's two parts, a weighted
   mean), and the two numbers that now agree.

---

## A trap your query never fell into: four lines

- **Would have changed:** which question's number, with and without the clause that handles it, and the difference.
  Run the query without the clause once, and paste that one line.
- **Cause:** which lines, and why a query without the clause counts them.
- **Evidence:** the cell that shows those lines, by ID, with the line that matters.
- **Handled by:** the clause, quoted, and the cell it is in.
