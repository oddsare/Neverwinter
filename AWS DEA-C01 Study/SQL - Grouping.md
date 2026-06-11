---
title: SQL — Grouping (GROUP BY / HAVING)
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

# SQL — Grouping (GROUP BY / HAVING)

> [!info] What GROUP BY does
> Collapses rows that **share the same value(s)** into **one row per group**, computing aggregates (COUNT, SUM, AVG, MIN, MAX) **per group**. Turns "summarize the whole table" into "summarize per category."

## Basic setup

```sql
SELECT department, COUNT(*) AS headcount, ROUND(AVG(salary),2) AS avg_salary
FROM employees
GROUP BY department;
```
- One output row per distinct `department`.
- Aggregates are computed within each group.

> [!important] The golden rule
> Every column in `SELECT` **not** inside an aggregate **must** appear in `GROUP BY`.
> (Can't SELECT `name` when grouping by `department` — which name would represent the whole group?)

## WHERE vs HAVING (key exam distinction)

SQL execution order:
```
FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY
```
| Clause | Filters | When | Aggregates allowed? |
|---|---|---|---|
| `WHERE` | individual **rows** | **before** grouping | ❌ no (groups don't exist yet) |
| `HAVING` | whole **groups** | **after** aggregation | ✅ yes |

```sql
SELECT department, COUNT(*) AS headcount
FROM employees
WHERE salary > 0          -- row filter (before grouping)
GROUP BY department
HAVING COUNT(*) > 2;      -- group filter (after aggregation)
```

> Mnemonic: **WHERE filters rows, HAVING filters groups.** A condition on an aggregate ("avg > X", "more than N rows") → **HAVING**.

## Nested grouping (multiple columns)
```sql
SELECT department, job_title, COUNT(*) FROM employees GROUP BY department, job_title;
```
One row per **unique combination** of the grouped columns.

## With ORDER BY
`ORDER BY` runs last, so it can sort by an aggregate (or its alias):
```sql
SELECT department, ROUND(AVG(salary),2) AS avg_salary
FROM employees GROUP BY department ORDER BY avg_salary DESC;
```

## Worked example (run in SQLite)

`employees`: Engineering = Alice 80k, Bob 60k, Eve 50k · Sales = Carol 45k, Dave 72k

| Query | Result |
|---|---|
| count + avg per dept | Engineering → 3, 63333.33 · Sales → 2, 58500.0 |
| `HAVING COUNT(*) > 2` | only Engineering (3) |
| group + `ORDER BY avg_salary DESC` | Engineering (63333.33), Sales (58500.0) |

> [!tip] Exam focus
> - Aggregate-condition filter → **HAVING**, not WHERE.
> - Any non-aggregated SELECT column must be in GROUP BY (else error).
> - `COUNT(*)` counts rows per group incl. NULLs; `COUNT(col)` skips NULLs (see [[SQL - Aggregation]]).

## Related
- [[SQL - Aggregation]] · [[Study Q&A Log]] · Full reference: `dea-materials/study-notes/01_fundamentals.md`
