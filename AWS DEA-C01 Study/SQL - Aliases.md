---
title: SQL — Aliases (table & column shorthand)
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

# SQL — Aliases (table & column shorthand)

> [!info] Core idea
> An **alias** is a temporary short name you give a table (or column) for the duration of one query, so you can write `C.customerName` instead of `customers.customerName` every time.

## Table alias — the `C.customerName` shorthand

```sql
SELECT C.customerName, O.amount
FROM customers AS C            -- 'C' is now shorthand for the customers table
JOIN orders    AS O ON C.customerID = O.customerID;
```
- Define it once in `FROM`/`JOIN`: `customers AS C` (the **`AS` is optional** — `customers C` works the same).
- Then reference every column as `C.column`.

## Why use it
1. **Less typing / cleaner** — `C.customerName` beats `customers.customerName`.
2. **Disambiguation in joins** — if both tables have a `customerID`, you *must* qualify it (`C.customerID` vs `O.customerID`) or SQL errors on the ambiguity.
3. **Required for self-joins** — joining a table to itself needs two different aliases (e.g. `employees E1 JOIN employees E2`) so you can tell the two copies apart.

## Table alias vs column alias (don't confuse)
| Type | Syntax | Names… | Example |
|---|---|---|---|
| **Column alias** | `expr AS name` | the output **column** | `COUNT(*) AS total_rows` |
| **Table alias** | `table AS letter` | the **table** for the query | `customers AS C` → `C.col` |

## Worked example (run in SQLite)
`customers` (Alice, Bob) ⋈ `orders` → with aliases `C` and `O`:
```
Alice | 101 | 500
Alice | 102 | 300
Bob   | 103 | 150
```

> [!tip] Notes
> - Alias lives **only for that one query**; it doesn't rename anything permanently.
> - Once you alias a table, you generally must use the alias (not the original name) for the rest of the query.
> - Case of the alias letter is conventional (`C` or `c`) — keep it consistent.

## Related
- [[SQL - Aggregation]] · [[SQL - Grouping]] · [[Study Q&A Log]]
