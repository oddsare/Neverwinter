---
title: SQL — Pivoting
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

# SQL — Pivoting

> [!info] Core idea (all you need)
> **Pivoting turns row-level data into columnar data** — you take values that were stored down the rows and spread them out across columns to build a summary report.

**Example:** a sales table has one row per sale (`salesperson`, `amount`). Pivoting produces one **column per salesperson** with their total — a cross-tab report.

> [!tip] Exam focus
> - How you pivot is **very database-specific**: some engines have a dedicated `PIVOT` command; otherwise you fake it with `SUM(CASE WHEN … THEN amount END)` per category (see [[SQL - Aggregation]]).
> - Just recognize the term: **rows → columns = pivot** (and columns → rows = *unpivot*).

## Related
- [[SQL - Aggregation]] · [[SQL - Grouping]] · [[Study Q&A Log]]
