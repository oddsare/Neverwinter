---
title: SQL — Titanic Aggregation Exercise
exam: AWS Certified Data Engineer – Associate (DEA-C01)
section: Data Engineering Fundamentals — SQL Review
type: worked-example
tags:
  - aws
  - dea-c01
  - sql
  - fundamentals
created: 2026-06-09
---

# SQL — Titanic Aggregation Exercise

> **Question:** "Women and children first" — did women & children have better survival odds on the Titanic? Table `titanic` has `age`, `survived` (1/0), and `sex`. Compute as 4 separate queries (no grouping).

> [!important] The key insight
> `survived` is **1 (lived) / 0 (died)**, so **`AVG(survived)` = the survival rate** — the average of a 0/1 column equals the fraction of 1s = survivors ÷ passengers. Every query below is the same `AVG(survived)`; only the `WHERE` (which rows) changes.

## Q1 — Explore the table first
```sql
SELECT * FROM titanic LIMIT 10;
```
`*` = **all columns** (so you see every column name); `LIMIT 10` = **first 10 rows**. Look before you compute — confirm column names (`sex` vs `gender`) and that `survived` is `1/0`.

## Q2 — Overall survival rate
```sql
SELECT AVG(survived) AS overall_rate
FROM titanic;
```
No `WHERE` → all passengers. The baseline to compare groups against. `AS overall_rate` labels the output.

## Q3 — Women and children
```sql
SELECT AVG(survived) AS women_children_rate
FROM titanic
WHERE sex = 'female' OR age <= 12;
```
"Women **and** children" = **OR** in SQL — a passenger qualifies if female **or** age ≤ 12 (either is enough). ⚠️ Don't write `AND` here just because the English says "and" — `AND` would require *both*, which no one can be (you can't be female *and* simultaneously the child test only). `age <= 12` includes 12 (inclusive).

## Q4 — Everyone else
```sql
SELECT AVG(survived) AS others_rate
FROM titanic
WHERE sex = 'male' AND age > 12;
-- > 12 excludes age 12 itself; must be BOTH male AND over 12
```
This is the **logical opposite** of Q3. Flipping `(female OR age<=12)` gives `(male AND age>12)` — `OR`→`AND` and each test inverts (**De Morgan's law**). Equivalent safe form: `WHERE NOT (sex='female' OR age<=12)`.

> [!warning] NULL age caveat
> If any `age` is NULL, `age <= 12` and `age > 12` are both *unknown*, so that row lands in **neither** group. On real data, check for null ages or the two groups won't sum to the whole.

## Answer
| Group | Rate (sample run) |
|---|---|
| Overall | 0.42 |
| **Women & children** | **0.68** |
| Others | 0.21 |

**Yes** — women & children survived at ~3× the rate of others (≈68% vs ≈21%), confirming "women and children first."

> [!tip] Reusable takeaways
> - **`AVG(0/1 column)` = a rate/proportion.** (Longhand `SUM(x)*1.0/COUNT(*)` needs `*1.0` to avoid integer division.)
> - "A or B" group → `WHERE A OR B`; "everyone else" → `NOT (A OR B)` or the De Morgan `NOT A AND NOT B`.
> - `<=` / `>=` include the boundary; `<` / `>` exclude it.

---

# Part 2 — Did class matter? (GROUP BY)

> **Question:** survival rate **per passenger class** (`Pclass` = 1, 2, 3). Output columns `Pclass`, `survival_rate`, sorted ascending by class. → "per category" = use **GROUP BY**.

```sql
SELECT Pclass, AVG(Survived) AS survival_rate
FROM titanic
GROUP BY Pclass
ORDER BY Pclass ASC;
```

**My annotation (refined):** select `Pclass` so the output **shows which class each rate belongs to**, and because the golden rule requires it (any non-aggregated SELECT column must be grouped). `AVG(Survived)` is the rate, aliased to `survival_rate`. `GROUP BY Pclass` → one row per class. `ORDER BY Pclass ASC` → sort 1 → 2 → 3.

> [!note] One refinement to the reasoning
> It's not that we select `Pclass` *"because we need it for grouping"* — you can `GROUP BY` a column without selecting it. We **select** it to *display* the label; we **group by** it to bucket the rows. They're two separate jobs that happen to use the same column. The golden rule just links them: *if* it's in SELECT (un-aggregated), it *must* also be in GROUP BY.

| Line | Why |
|---|---|
| `SELECT Pclass, AVG(Survived) AS survival_rate` | show the class label + its rate (same `AVG(0/1)` trick) |
| `GROUP BY Pclass` | collapse 100 rows → one per class; `AVG` computed *within* each class |
| `ORDER BY Pclass ASC` | sort by class ascending (`ASC` is default, but explicit = clearer) |

**Answer (sample run):**

| Pclass | survival_rate |
|---|---|
| 1 | 0.73 |
| 2 | 0.54 |
| 3 | 0.31 |

**Yes — class mattered a lot.** 1st class survived at ~2× the rate of 3rd (≈73% vs ≈31%), 2nd in between.

> [!tip] Aggregation vs Grouping — the contrast across both parts
> Same `AVG(Survived)` both times. **Part 1** (no GROUP BY) → *one* number per query (whole filtered group). **Part 2** (GROUP BY Pclass) → *one number per group*. That's the entire effect of `GROUP BY`. See [[SQL - Grouping]].

## Related
- [[SQL - Aggregation]] · [[SQL - Grouping]] · [[Study Q&A Log]]
