---
title: Data Validation & Profiling
exam: AWS Certified Data Engineer – Associate (DEA-C01)
section: Data Engineering Fundamentals
type: topic-note
tags:
  - aws
  - dea-c01
  - fundamentals
  - data-quality
created: 2026-06-09
---

# Data Validation & Profiling

> [!info] What it is
> **Validation** = checking data meets defined rules/expectations. **Profiling** = inspecting data to understand its structure, content, and quality (null counts, distributions, ranges). Together they answer *"can I trust this data?"* before it flows downstream.

The course defines **4 quality dimensions**. For each: what it means, how you check it, why it matters.

## The 4 dimensions

| Dimension | Ensures… | Checks | Why it matters |
|---|---|---|---|
| **Completeness** | All required data is present; nothing essential missing | Missing values, null counts, % of populated fields | Missing data → inaccurate analyses & insights |
| **Consistency** | Values agree across datasets and don't contradict each other | Cross-field validation; compare across sources / time periods | Inconsistent data → confusion & wrong conclusions |
| **Accuracy** | Data is correct and represents what it's supposed to | Compare vs trusted sources; validate against known standards/rules | Inaccurate data → false insights & poor decisions |
| **Integrity** | Correctness & consistency are maintained over the data's lifecycle and across systems | Referential integrity (foreign-key checks), relationship validations | Keeps relationships intact & data trustworthy over time |

## Memory hook
**"Is it all there, does it agree, is it right, does it stay right?"**
- All there → **Completeness**
- Does it agree → **Consistency**
- Is it right → **Accuracy**
- Does it stay right (over time / across systems) → **Integrity**

## Seen in our worked examples
- **Completeness + Accuracy** — dropping/imputing null & 0 salaries → [[Study Q&A Log#Q7 — Cleaning bad data drop/impute null & 0 salaries (PySpark + SQL)|Q7]]
- **Consistency** — combining 1–5 stars with 1–10 ratings means reconciling scales before merge → [[Study Q&A Log#Q9 — Combining two tables on different scales (normalize before merge)|Q9]]
- **Integrity** — joining on a stable `movie_id` (not fragile `title`) preserves the key relationship → [[Study Q&A Log#Q9 — Combining two tables on different scales (normalize before merge)|Q9]]

> [!tip] Exam focus
> - Map the scenario to the dimension: "nulls / missing fields" = **Completeness**; "two sources disagree / same field different values" = **Consistency**; "value is wrong vs source of truth" = **Accuracy**; "broken foreign key / orphaned record / drift over time" = **Integrity**.
> - **Consistency vs Integrity** is the common trap: Consistency = values *agree right now*; Integrity = relationships/correctness *hold up over the lifecycle & across systems* (referential integrity / FKs).
> - AWS service angle: **AWS Glue Data Quality** (DQDL rules), **Glue DataBrew** (profiling), and **Deequ** (on Spark/EMR) are how you *enforce/measure* these dimensions at scale.

## Related
- Up: [[Study Q&A Log]] · Full reference: `dea-materials/study-notes/01_fundamentals.md`
