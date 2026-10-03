# Homework 1 — One table, the weekly numbers

**ECBS5294 — Working with Data · due Saturday 10 October 2026, 23:59 (slot on Moodle)**

Budget about **5 hours** for the seven questions that are the homework, more if SQL is new to you; the note, the
video and the archive are on top of that. Two more questions, 6 and 9, are **stretch**: do them after the seven are
answered and checked, or not at all. They are not graded. If you are well past the budget and still stuck, post on
the Moodle forum. That tells us something useful about the assignment, and it is not a mark against you.

## Start here

| | |
|---|---|
| **The question** | Nine numbers for the head of sales, from one table, each one a sentence, a query, a number, and a check. |
| **The file** | `data/raw/online_retail.parquet`: every invoice line of a UK online gift shop, 1 December 2009 to 9 December 2010. |
| **What can go wrong** | Every query you write will run. Some will be wrong, and none of them will say so. |
| **What you hand in** | `hw1-submission.zip` and a 60–90 second video, on Moodle. |
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

The head of sales asked for these, in these words. The notebook has a section for each.

1. What is one row of this table, and what identifies it? State the grain in one sentence. Then prove whether
   (`Invoice`, `StockCode`) is a key. If it is not: how many rows would disappear if exact duplicates — rows identical
   in every column — were collapsed to one? (That is, rows minus distinct rows.)
2. What was revenue over the whole file?
3. Revenue by month, one row per month, in date order.
4. Revenue by country outside the United Kingdom, largest first. How much came from Ireland? (Check the census
   for how the file writes the country you are asked about.)
5. The ten products with the most units sold.
6. The ten products that brought in the most revenue. *Stretch.*
7. The average revenue per identified customer, with the unidentified revenue beside it, and what fraction of
   question 2's revenue that is (two numbers and one division).
8. Revenue without the manual adjustments, and the number of sales lines it comes from.
9. The five weeks with the highest revenue, each with its revenue, highest first. *Stretch.*

Questions 1 to 5, 7 and 8 are the homework. Questions 6 and 9 are stretch: the same four parts, the same trap log,
if you get there. Nothing in the rubric depends on them.

Each answer has four parts, in the cells under its question: **the sentence** (the filter, the measure, the
population), **the query** (with the number of rows you expect, written before you run it: a count where the brief
fixes it, a range with a reason where only the file can), **the number** (with its unit), and **the check** (one
query that would fail if the number were wrong, and one line saying what it shows). A sum is checked by an identity.
A ranked list is not a sum: check it by counting the rows it returned, by reaching its top row's number a second way,
and by showing the boundary — one thing the rule kept beside one it excluded — so the reader sees the rule at its
edge. For question 5 that is two rows: the units of one product in the ten, and the units of one code the product rule
threw out (postage, say), from the same lines. **The notebook opens with a worked example**, a tenth question that is
not graded, answered in all four parts: read it before you write question 1.
Every code cell starts with its ID — `Q3` for question 3's query, `Q3 check` for its check — so that your trap log can
cite it instead of pasting it again.

At the end of the notebook, **the reconciliation**: revenue by month added up, revenue by country (every country)
added up, and question 2's revenue. Three computations, one number, to the penny.

**"To the penny" means `abs(a - b) < 0.005`**, or both sides rounded to the cent — **never `==`**. Two correct sums of
the same lines, added in a different order, can differ in the tenth decimal place: in this file, revenue added up by
month and revenue added up by country differ around the sixth decimal place. `==` would call that a failure. Counts
are whole numbers; for them, `==` is right.

## What can go wrong

Nothing in this project is broken. The file is real, and the shop's system wrote it for its own purposes, not for
your brief. Some of its lines are not what a sentence in the brief counts, and a query that counts them anyway runs,
returns a number, and says nothing. The brief's definitions tell you what each number means. The file will not tell
you where it disagrees: the census and the checks will.

Every place where the data would have changed a number goes in your **trap log**, even when your own query was right
from the start. That is how the reader knows the number is right on purpose.

Where to look, so that the log can be complete: the invoice number, the price, the quantity, the stock code and the
description, the customer, the country name, and the rows themselves (is any row repeated?). A complete log has
looked at each of them; every one that changed a number has an entry. What each of them does is yours to find.

## What you must submit

In the repo, committed:

1. **`notebooks/report.ipynb`**, the seven core questions answered in their four parts (and the stretch ones, if you
   did them), and the reconciliation. It must run top
   to bottom after *Restart*, then *Run All*.
2. **`DIAGNOSIS.md`: the trap log.** One entry per data issue that changed a number (below).
3. **`README.md`**: add a section **How to run this** at the top, with the exact commands that take a stranger from a
   fresh clone to every number in your notebook. Leave the rest of this file as it is.
4. **Commits** whose messages name the cause, and **`GIT_LOG.txt`** (`SUBMITTING.md` makes it).
5. **`AI_USE.md`**: what you asked an assistant, what it got right or wrong, and how you checked.

Then make the archive with **`SUBMITTING.md`** — its own file, so that editing this README cannot delete the
instructions you need at the very end. Upload to the Homework 1 slot on **Moodle**:

6. `hw1-submission.zip` (in the folder above the project).
7. A **60–90 second video** (below).

## The order of work

The list above is what you hand in. This is the order to do it in.

**Open `DIAGNOSIS.md` before step 1, and keep it open.** A trap's evidence is the query that showed it and what that
query returned *before you changed anything*. The moment you fix the query, the wrong number is gone, and with it the
proof of what was wrong. That is the one thing you paste. Everything the notebook still shows, you cite by its ID.

1. **Inspect.** Section 0 of the notebook: Block 1's four queries on the file — `DESCRIBE`, `SUMMARIZE`, the key test,
   the census of every column a question filters on — one query per cell, each starting with its ID (`I1`, `I2`, …).
   Note in the trap log what surprises you, citing the cell. A census of a few dozen values you read to the end. A
   column with thousands of values (a code, a description) you count, census in the shape the question uses (the
   first character of a code; the values that are missing), and inspect by group with `LIMIT 20`: pandas shows only
   the head and tail of a result longer than 400 rows, and the middle is where the surprises hide.
2. **Write the brief's sales invoices once**, as a view, in the cell marked `S`. Questions 2 to 9 read from it.
   Question 1 is about the table as the file holds it, so it reads `data/raw/online_retail.parquet` directly.
3. **Answer the questions in order.** For each: the sentence; the rows you expect; the query; the number; the check.
4. **When a check fails, or a number surprises you, stop.** Paste the query and its output into a new trap entry,
   parts 1 and 3, **before** you touch the query. Then find the lines behind it, fix the query, and finish the entry:
   parts 2, 4 and 5, citing the cells.
5. **Commit** after each question, or each trap. The message names the cause.
6. **The reconciliation cell.** By month, by country, the grand total: to the penny, with `abs(a - b) < 0.005`.
7. **Restart, then Run All.** Every number must come from a cell that ran in order, from the top.
8. **Finish the words:** the trap log's remaining parts, `AI_USE.md`, and *How to run this* in the README. Commit.
9. **`SUBMITTING.md`**: commit everything, save the log, make the zip, check it. Record the video. Upload both.

Steps 8 and 9 come last on purpose: the archive contains only what you have committed.

## The trap log

`DIAGNOSIS.md` is the trap log: **one entry per data issue that changed one of the seven core numbers**, whether or
not your own query was ever wrong (a stretch question you answered brings its trap with it), each under a heading that names it: `## 1 — …`, `## 2 — …`. The file has both kinds of entry ready to copy.

**Cite, do not paste again.** Every code cell in the notebook starts with its ID: `I1`, `I2`, … (inspection), `S` (the
sales view), `Q1` … `Q9` (each question's query), `Q1 check` … `Q9 check`, and `R` (the reconciliation). Where the
evidence is still in the notebook, write the cell's ID and quote the one line of its output that matters. **Paste only
what the notebook no longer shows**: the query and the output from before a repair.

**A trap your query fell into** — a check failed, or a number surprised you — gets all five parts:

1. **Symptom:** the wrong number, and what showed it was wrong.
2. **Cause:** which lines, and why the query counted them. Not what you changed.
3. **Evidence:** the query and its output from before the repair, pasted; or an inspection cell's ID and the line that
   shows the cause.
4. **Change:** the cell that holds the query now (`Q8`), and one sentence on what changed.
5. **Verification:** the check cell (`Q8 check`) or `R`, the identity it tests, and the two numbers that now agree.

**A trap your query never fell into** gets a short entry, four lines:

- **Would have changed:** which question's number, with and without the clause that handles it, and the difference.
  Run the query without the clause once, and paste that one line.
- **Cause:** which lines, and why a query without the clause counts them.
- **Evidence:** the cell that shows those lines, by ID, with the line that matters.
- **Handled by:** the clause, quoted, and the cell it is in.

A right number with no entry for the trap that would have broken it is a number that could be right by accident.
The rubric grades it that way.

## The video

60–90 seconds, your voice, screen recorded; your face does not need to be in it. Start by saying "Homework 1" and the
repo name. Show *Restart* then *Run All* finishing. Then take **one trap** that changed a number and say, pointing at
the screen: the **symptom** (the number you first got, and what showed it was wrong); the **cause** (which lines, and
why the query counted them); the **change** (the query now); the **verification** (the check, and the two numbers
that now agree). Do not read a script.

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
   sentence; the boundary check shows it holds at its edge.

## Stretch

Questions 6 and 9 are the stretch. So is this: cancellations by month: the value of the `C` lines each month, and as a share of that month's revenue. Which identity
does that share have — a sum, a ratio, or a weighted mean — and what would you reconcile to prove it?

## Git thread

Commit after each question, or after each trap you find. The message says what the cause was: "Q5: count units on
priced lines only — zero-priced lines are stock adjustments, not sales" — not "fix", not "update".

## Grading

The rubric is on the course site. Correct answers 30 · verification 20 · the trap log 20 · the video 15 · clean
submission and Git 10 · AI use 5. Late: one day at −10%; nothing after Sunday 23:59.

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
