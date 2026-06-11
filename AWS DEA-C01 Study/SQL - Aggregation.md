---
title: SQL — Aggregation
exam: AWS Certified Data Engineer – Associate (DEA-C01)
section: Data Engineering Fundamentals — SQL Review
type: topic-note
tags:
  - aws
  - dea-c01
  - sql
  - fundamentals
created: 2026-06-09
---

# SQL — Aggregation

> [!info] What aggregation is
> Functions that collapse **many rows into a single summary value** (count, total, average, etc.). Often paired with `GROUP BY` to summarize *per group*. Building block of nearly every analytics query.

## COUNT

```sql
SELECT COUNT(*) AS total_rows FROM employees;
```
- **`COUNT(*)`** — counts **every row**, including rows containing NULLs. (`*` here = "every row", NOT "all columns" like in `SELECT *`.)
- **`AS total_rows`** — a **column alias**: names the output column (readable label, not a variable).
- **`FROM employees`** — the source table.
- → Counts how many **employee rows** exist. = number of employees *only if one row per employee* (duplicates would inflate it).

### Three flavors of COUNT

| Query | Counts | NULLs |
|---|---|---|
| `COUNT(*)` | every row | included |
| `COUNT(email)` | rows where `email` is not NULL | excluded |
| `COUNT(DISTINCT department)` | unique non-null values | excluded |

> Example — 5 employees, one missing email, 2 departments: `COUNT(*)`=5, `COUNT(email)`=4, `COUNT(DISTINCT department)`=2.

### Worked example (actually run in SQLite)

`employees` table:

| id | name | email | department |
|---|---|---|---|
| 1 | Alice | alice@co.com | Engineering |
| 2 | Bob | bob@co.com | Engineering |
| 3 | Carol | **NULL** | Sales |
| 4 | Dave | dave@co.com | Sales |
| 5 | Eve | eve@co.com | Engineering |

| Query | Result | Why |
|---|---|---|
| `COUNT(*)` | **5** | every row — Carol's NULL email still counts as a row |
| `COUNT(email)` | **4** | non-NULL emails only — Carol's NULL skipped (5−1) |
| `COUNT(DISTINCT department)` | **2** | unique depts — Engineering ×3, Sales ×2 → 2 distinct |

> [!tip] Exam focus
> `COUNT(*)` vs `COUNT(column)` — `COUNT(column)` silently **skips NULLs**. "How many customers have a phone number?" → `COUNT(phone)`, not `COUNT(*)`.

## Aggregate with CASE (conditional aggregation)

**CASE = SQL's if/then/else.** Setup (searched form, most common):
```sql
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    ELSE fallback        -- optional; no match & no ELSE → NULL
END
```
Evaluates top-to-bottom, returns the **first** matching `THEN`. Simple form matches one column:
```sql
CASE department WHEN 'Engineering' THEN 'Tech' WHEN 'Sales' THEN 'Revenue' ELSE 'Other' END
```

### Why use it with aggregates
A `WHERE` clause filters the **whole query** → only one condition at a time:
```sql
SELECT COUNT(*) FROM employees WHERE salary > 70000;   -- just the "high" bucket
```
To get **multiple buckets in one query**, move the conditions *inside* the aggregate:
```sql
SELECT
  COUNT(CASE WHEN salary > 70000 THEN 1 END)                 AS high_salary_count,
  COUNT(CASE WHEN salary BETWEEN 50000 AND 70000 THEN 1 END) AS medium_salary_count,
  COUNT(CASE WHEN salary < 50000 THEN 1 END)                 AS low_salary_count
FROM employees;
```

> [!important] The NULL trick (why this works)
> `CASE WHEN salary > 70000 THEN 1 END` returns `1` for matches and **NULL** for everyone else (no `ELSE`). `COUNT(...)` **skips NULLs**, so it counts only the `1`s. Each column filters the same rows independently → all buckets in one table scan.

### COUNT vs SUM variant
```sql
SUM(CASE WHEN salary > 70000 THEN 1 ELSE 0 END)   -- same result
```
- `COUNT(CASE … THEN 1 END)` → relies on **NULL-skipping** (no ELSE).
- `SUM(CASE … THEN 1 ELSE 0 END)` → relies on **adding zeros** (needs ELSE 0).

> Worked example (run in SQLite) — salaries 80k, 60k, 45k, 72k, 50k → **high=2, medium=2, low=1**.

> [!tip] Exam focus
> "Count/sum several categories in a single query" or "pivot-style summary without multiple queries" → **`CASE` inside the aggregate**. Remember `BETWEEN` is **inclusive** on both ends.

## Related
- [[Study Q&A Log]] · [[Data Validation & Profiling]] · Full reference: `dea-materials/study-notes/01_fundamentals.md`
