# Homework 1 — One table, the weekly numbers

**ECBS5294 — Working with Data · due Saturday 10 October 2026, 23:59 (slot on Moodle)**

Budget about **5 hours** for the seven questions that are the homework, more if SQL is new to you; the archive is on
top of that. Two more questions, 6 and 9, are **stretch**: do them after the seven are answered and checked, or not at
all. They are not graded. If you are well past the budget and still stuck, post on the Moodle forum. That tells us
something useful about the assignment, and it is not a mark against you.

## Start here

| | |
|---|---|
| **The question** | Nine numbers for the head of sales, from one table, each one answered in five parts: a sentence, the rows you expect, a query and its number, a check, and one line on how you would know if it were wrong. |
| **The file** | `data/raw/online_retail.parquet`: every invoice line of a UK online gift shop, 1 December 2009 to 9 December 2010. |
| **What can go wrong** | Every query you write will run. Some will be wrong, and none of them will say so. |
| **What you hand in** | `hw1-submission.zip`, on Moodle. |
| **First thing to do** | Read the brief. Then the notebook's section 0, which lists what to inspect, and the worked example, which shows what a finished answer looks like. |

## Get the project

In your terminal (Git Bash on Windows, Terminal on macOS), in the folder where you keep course work:

```bash
git clone https://github.com/earino/ecbs5294-hw01-one-table.git
cd ecbs5294-hw01-one-table
uv sync
```

Open **this folder** in VS Code (*File → Open Folder…*; trust the authors if asked), open `notebooks/report.ipynb`,
pick the `.venv` kernel, and run the first cell. It prints the folder it is working in: it must be this project's
folder. One row of the file is one line of an invoice; `Price` is in pounds per unit.

## The brief

Finance wrote these definitions. They are what the numbers mean, and they are not yours to change.

> **The period** is the whole file: 1 December 2009 to 9 December 2010.
>
> **Sales invoices.** A sales invoice has a six-digit number. An invoice number that starts with a letter is not a
> sale: `C` marks a cancellation, and `A` an accounting adjustment.
>
> **Revenue** is the sum of `Quantity * Price` over the lines of sales invoices.
>
> **Units sold** of a product is the sum of `Quantity` over its lines on sales invoices that carry a price above zero.
>
> **A product** is an item the shop stocks and sells, identified by its `StockCode`. Postage, carriage, discounts,
> manual adjustments, fees, bank charges, samples, and test lines have stock codes too. They are not products.
>
> **Identified revenue** is revenue from lines that carry a `Customer ID`. Lines with no `Customer ID` are
> *unidentified*: part of revenue, and part of no per-customer figure. **Average revenue per identified customer** is
> identified revenue divided by the number of distinct `Customer ID`s on those same lines.
>
> **Manual adjustments** are the lines whose `Description` is `Manual`.
>
> **Country** is the `Country` column as the file writes it. *Outside the United Kingdom* is every line whose
> `Country` is not `United Kingdom`.
>
> **A week** starts on a Monday: `date_trunc('week', InvoiceDate)` gives that Monday.
>
> **The lines as they are.** Every figure is computed on the lines the file has. Do not delete, merge, or de-duplicate
> lines unless a sentence above says so.

## The questions

The head of sales asked for these, in these words. The notebook has a section for each, and each heading names the
kind of check it wants.

1. What is one row of this table, and what identifies it? Three parts: the grain in one sentence; what section 0's
   key test showed, and what it tells you (this table has no key at all, so do not go looking for a longer one); and
   how many rows would disappear if each set of identical rows were collapsed to one, rows minus distinct rows. A
   measurement, not a repair: you are not collapsing them.
2. What was revenue over the whole file? *(a sum)*
3. Revenue by month, one row per month, in date order. *(a sum)*
4. Revenue by country outside the United Kingdom, largest first. How much came from Ireland? Check the census for
   how the file writes the country you are asked about. *(a sum)*
5. The ten products with the most units sold. *(a ranked list)*
6. The ten products that brought in the most revenue. *(a ranked list; stretch)*
7. The average revenue per identified customer, with the unidentified revenue beside it, and what fraction of
   question 2's revenue that is (two numbers and one division). *(a ratio)*
8. Revenue without the manual adjustments, and the number of sales lines it comes from. *(a sum)*
9. The five weeks with the highest revenue, each with its revenue, highest first. *(a ranked list; stretch)*

Questions 1 to 5, 7 and 8 are the homework. Questions 6 and 9 are stretch: the same five parts, if you get there.
Nothing in the rubric depends on them.

## The five parts of an answer

Every question is answered in the cells under it: one markdown cell with five lines, then the query cell, then the
check cell.

1. **The sentence.** In plain English: what the number measures, which rows it comes from, and which rows the query
   left out.
2. **The rows you expect, and why.** Written before you run the query, with its source: *the brief fixes it* ("one row
   per month: 13"), or *a section 0 cell shows it* ("`I5`: 40 values of `Country`, one of them the UK, so 39"), or **"neither
   fixes this"**. That last line is allowed. Nobody makes a number up.
3. **The query, and the number** with its unit.
4. **The check, in the kind the question names.** Three kinds, and each heading says which, so nobody has to invent a
   check:
   - **a sum**: split it into groups, and show they add back up to it;
   - **a ranked list**: count the rows, and reach the top row a second way;
   - **a ratio**: check the top and the bottom separately, on the same lines.
   If no query would catch the error, write instead how the number could be wrong, and what you would see if it were.
5. **One line: how would I know if this were wrong?** Which lines in the file would have changed this number, and
   which section 0 cell shows them. Write it **while the wrong number is still on screen** if a check fails: once you
   fix the query, that line is the only record of what the number was.

The notebook opens with a **worked example**, a tenth question that is not graded, answered in all five parts: read it
before you write question 1. Questions 3 and 4 have no check cell of their own: their check is **the reconciliation**
at the end of the notebook, revenue by month added up, revenue by country (every country) added up, and question 2's
revenue, three computations of one number, to the penny.

**"To the penny" means `abs(a - b) < 0.005`**, or both sides rounded to the cent — **never `==`**. Two correct sums of
the same lines, added in a different order, can differ in the tenth decimal place: in this file, revenue added up by
month and revenue added up by country differ around the sixth decimal place. `==` would call that a failure. Counts
are whole numbers; for them, `==` is right.

Every code cell starts with its ID — `I1`, `I2`, … for the inspection queries, `S` for the sales view, `Q3` for
question 3's query, `Q3 check` for its check, `R` for the reconciliation — so that a "how would I know" line can cite
a cell instead of pasting it again.

## What can go wrong

Nothing in this project is broken. The file is real, and the shop's system wrote it for its own purposes, not for
your brief. Some of its lines are not what a sentence in the brief counts, and a query that counts them anyway runs,
returns a number, and says nothing. The brief's definitions tell you what each number means. The file will not tell
you where it disagrees: the census and the checks will.

Where to look, so that no "how would I know" line is a guess: the invoice number, the price, the quantity, the stock
code and the description, the customer, the country name, and the rows themselves (is any row repeated?). Section 0
of the notebook lists them, one cell each. What each of them does is yours to find.

## What you must submit

In the repo, committed:

1. **`notebooks/report.ipynb`**, the seven core questions answered in their five parts (and the stretch ones, if you
   did them), and the reconciliation. It must run top to bottom after *Restart*, then *Run All*.
2. **Commits** whose messages name the cause, and **`GIT_LOG.txt`** (`SUBMITTING.md` makes it).
3. **`AI_USE.md`**: what you asked an assistant, what it got right or wrong, and how you checked.

Then make the archive with **`SUBMITTING.md`** — its own file, so that nothing you edit can delete the instructions you
need at the very end. Upload `hw1-submission.zip` (in the folder above the project) to the Homework 1 slot on
**Moodle**.

## The order of work

1. **Inspect.** Section 0 of the notebook: Block 1's four queries on the file, and the census of every column a
   question depends on, one query per cell, each starting with its ID (`I1`, `I2`, …). The section lists what to look
   at. Note in a comment what surprises you. A census of a few dozen values you read to the end. A column with
   thousands of values (a code, a description) you count, census in the shape the question uses (the first character
   of a code; the values that are missing), and inspect by group with `LIMIT 20`: pandas shows only the head and tail
   of a result longer than 400 rows, and the middle is where the surprises hide.
2. **Question 1**, on the file as it is, before the brief's filter.
3. **Write the brief's sales invoices once**, as a view, in the cell marked `S`. Questions 2 to 9 read from it.
4. **Answer questions 2 to 9 in order.** For each: the sentence; the rows you expect, and why; the query; the number;
   the check in the kind the heading names; the "how would I know" line.
5. **When a check fails, or a number surprises you, stop.** Write the "how would I know" line first, with the number
   you got and what showed it was wrong, **before** you touch the query. Then find the lines behind it, fix the query,
   and finish the line.
6. **Commit** after each question. The message names the cause.
7. **The reconciliation cell.** By month, by country, the grand total: to the penny, with `abs(a - b) < 0.005`.
8. **Restart, then Run All.** Every number must come from a cell that ran in order, from the top.
9. **`AI_USE.md`**, then **`SUBMITTING.md`**: commit everything, save the log, make the zip, check it. Upload it.

Step 9 comes last on purpose: the archive contains only what you have committed.

## Rules

- **Never edit `data/raw/`.** The raw data is the evidence. Every number comes from a query on it.
- **The brief is the definition.** Where a question and the file seem to disagree, the brief decides; say in the
  sentence which clause you applied.
- **SQL does the analysis.** Python may add, subtract, divide and compare the numbers you fetched with
  `.fetchone()[0]`; a sum, a count or an average *over rows* is SQL, a query or a view, never `.sum()` or `.mean()` on
  a dataframe. pandas displays a result; it does not compute the answer.
- **AI is allowed**, and you must be able to explain every line you submit. Give the assistant the schema
  (`DESCRIBE`), the brief's sentence, and the number you expected; run the checks it suggests yourself.

## Hints, if stuck

1. Before question 2, run the census on the first character of `Invoice`, with `SUM(Quantity * Price)` beside each
   count. Then the census of every column the questions filter on. Read every row of the short ones; the long ones
   (codes, descriptions) you count and slice by group, as in hint 3. The notebook's section 0 lists what to look at.
2. Find a second way to every number. Split the lines into groups that do not overlap: the groups must add back up to
   the whole. When they do not, the difference is a set of lines. List them.
3. When a check closes and the number still surprises you, look at the lines behind it: `SELECT *`, the `WHERE` that
   picks that group, `LIMIT 20`. The trap is usually visible in twenty rows: a price, a code, a name, an empty cell.
4. The brief says what a product is not, and gives no rule. Real product codes look alike: count the codes that do
   not look like the others, read them, and write the rule the brief implies. The rule is yours to state in the
   sentence.

## Stretch

Questions 6 and 9 are the stretch. So is this: cancellations by month: the value of the `C` lines each month, and as a
share of that month's revenue. Which kind of check does that share permit — a sum, a ratio, or a weighted mean — and
what would you reconcile to prove it?

## Git thread

Commit after each question. The message says what the cause was: "Q5: count units on priced lines only — zero-priced
lines are stock adjustments, not sales" — not "fix", not "update".

## Grading

The rubric is on the course site. Correct answers 30 · the checks and the "how would I know" lines 30 · the
sentences and the rows you expected, with their sources, 25 · clean submission and Git 10 · AI use 5. Late: one day
at −10%; nothing after Sunday 23:59.

## If you got lost: how to reset

Both of these **destroy work**. Read before running.

**Discard uncommitted changes (destructive)** — throw away edits and new files; keep your commits:

```bash
git restore --staged --worktree .    # every tracked file back to the last commit, staged or not
git clean -fd                        # and remove new, untracked files
```

> ⚠️ Permanently deletes uncommitted changes, staged or not, and any new untracked files.

**Full reset to the starter state (destructive)** — back to exactly what you cloned; throws away your commits too:

```bash
git reset --hard origin/main
git clean -fdx
```

> ⚠️ Discards your local commits and uncommitted changes. The `-x` also removes every ignored file — `data/silver/`, the
> `.venv/` environment, and anything else `.gitignore` lists, such as `.vscode/` and `.env` — so the folder matches a fresh clone. `uv sync` rebuilds the environment in a minute.
