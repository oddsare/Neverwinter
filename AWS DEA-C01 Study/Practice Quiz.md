---
title: AWS DEA-C01 — Practice Quiz
exam: AWS Certified Data Engineer – Associate (DEA-C01)
type: quiz
tags:
  - aws
  - dea-c01
  - quiz
created: 2026-06-09
---

# AWS DEA-C01 — Practice Quiz

> [!info] How to use
> Answers are in **collapsed** callouts — try the question first, then click to reveal. Topic: see the tag on each.

## Index
- [[#Q1 — Joins (RDS report)]] — SQL joins
- [[#Q2 — Git feature branch workflow]] — Git
- [[#Q3 — PIVOT (Redshift monthly sales report)]] — SQL / Redshift
- [[#Q4 — Sampling (diverse patient population)]] — Fundamentals / sampling
- [[#Q5 — Spark performance (uneven tasks/nodes)]] — Fundamentals / data skew
- [[#Q6 — Constraints on a linking table (referential integrity)]] — Database / integrity
- [[#Q7 — Impact analysis before disabling a pipeline]] — Fundamentals / data lineage
- [[#Q8 — Columnar format for cost + query performance]] — Fundamentals / file formats
- [[#Q9 — Storing diverse data for analytics + ML]] — Fundamentals / data lake
- [[#Q10 — Classifying multi-source data (data types)]] — Fundamentals / data types

---

## Q1 — Joins (RDS report)

A Data Engineer is tasked with creating a report on an Amazon RDS instance to support a customer loyalty analysis. The database contains two tables: **Customers** (listing all registered customers) and **Orders** (recording customer purchases). The report should retrieve all customers' information along with their order details if they have placed any orders. **Which SQL query ensures that every customer is included, and orders are shown only if they exist?**

- **A.** `SELECT * FROM Customers FULL JOIN Orders ON Customers.CustomerID = Orders.CustomerID`
- **B.** `SELECT * FROM Customers INNER JOIN Orders ON Customers.CustomerID = Orders.CustomerID`
- **C.** `SELECT * FROM Customers LEFT JOIN Orders ON Customers.CustomerID = Orders.CustomerID`
- **D.** `SELECT * FROM Customers RIGHT JOIN Orders ON Customers.CustomerID = Orders.CustomerID`

> [!success]- Answer & why
> **C — LEFT JOIN.**
> "Every customer included, orders only if they exist" = keep **all** rows from the left table (Customers) + matching Orders, NULL where none → **LEFT JOIN** with Customers on the left.
> - **B INNER** — drops customers with no orders ❌
> - **D RIGHT** — keeps all *orders*, not all customers ❌
> - **A FULL** — keeps all of *both* (would include orphan orders too) ❌
> Rule: *"list all X even if no match in Y" = LEFT JOIN, X on the left.* See [[SQL - Joins]] (if created) / Titanic join exercise.

---

## Q2 — Git feature branch workflow

A Data Engineer discovers a critical bug in the production data transformation pipeline managed in a Git repository on the `master` branch. To comply with the company's policy of using feature branches for bug fixes and enhancements, **which sequence of Git commands should the engineer use to set up their development environment to address this issue?**

- **A.** `git clone` followed by `git checkout -b`
- **B.** `git clone` followed by `git branch -b`
- **C.** `git pull` followed by `git branch -b`
- **D.** `git fetch` followed by `git switch`

> [!success]- Answer & why
> **A — `git clone` then `git checkout -b`.**
> "**Set up their development environment**" → they don't have the repo yet, so start with **`git clone`** (download the whole repo). Then **`git checkout -b <name>`** creates a new feature branch **and switches to it** in one step.
> - **B / C** — `git branch -b` is **not a valid command** (`-b` belongs to `checkout`; `git branch <name>` just creates, no `-b` flag) ❌
> - **C** also uses `git pull`, which assumes the repo already exists locally ❌
> - **D** — `git fetch` assumes an existing local repo, and **`git switch`** (without `-c`) switches to an *existing* branch; to create one you'd need `git switch -c` ❌
> Key facts: **clone = get a repo you don't have yet**; **`checkout -b` / `switch -c` = create + switch to a new branch**.

---

## Q3 — PIVOT (Redshift monthly sales report)

A Data Engineer is using AWS Redshift to analyze sales data from a retail company. The table `Monthly_Sales` includes columns for `SaleID`, `ProductCategory`, `SaleMonth`, and `SaleAmount`. The task is to generate a monthly sales report, where **each product category (Electronics, Clothing, Furniture) is displayed as a column header and each row represents a month** with the total sales for that category. Which SQL operation should be used in AWS Redshift to efficiently generate the report according to the requirement?

- **A.** GROUP BY
- **B.** PIVOT
- *(only these two options were shown in the screenshot)*

> [!success]- Answer & why
> **B — PIVOT.** ✅ (marked correct)
> PIVOT transforms **rows into columns** — exactly what's needed: aggregate `SaleAmount` for each `ProductCategory` into separate **columns**, with each row grouped by `SaleMonth`. Turns row-level data into a report-friendly cross-tab.
> - **GROUP BY** alone would summarize totals but keep categories **down the rows**, not spread across columns. To fake the pivot with GROUP BY you'd need `SUM(CASE WHEN ProductCategory='Electronics' THEN SaleAmount END)` per category.
> Rule: **"category values become column headers" = PIVOT (rows → columns).** See [[SQL - Pivoting]].

---

## Q4 — Sampling (diverse patient population)

A Data Engineer at a healthcare organization is tasked with analyzing patient satisfaction across various departments. The patient population is **diverse, with significant variations in age, treatment types, and outcomes.** To ensure the analysis is comprehensive and **accounts for the diverse characteristics of the patient groups**, the engineer must choose a sampling method. **What technique should the engineer use to accurately reflect the different segments of the patient population?**

- **A.** Random Sampling
- **B.** Systematic Sampling
- **C.** Minimax Sampling
- **D.** Stratified Sampling

> [!success]- Answer & why
> **D — Stratified Sampling.**
> "Reflect the **different segments** / account for **diverse characteristics**" = divide the population into homogeneous subgroups (strata: age, treatment type, etc.) and sample within **each** → guarantees every segment is represented.
> - **Random** — could under-represent small segments by chance ❌
> - **Systematic** — every k-th record; ignores subgroup structure ❌
> - **Minimax** — **not a real sampling technique** (made-up distractor) ❌
> Trigger: **"represent all subgroups / diverse segments" = Stratified.** See [[Data Validation & Profiling]] and Q3 in [[Study Q&A Log]].

---

## Q5 — Spark performance (uneven tasks/nodes)

A data engineering team is using Apache Spark to process a large dataset of user activity logs, distributed across multiple nodes. During processing, **some tasks take significantly longer than others**, causing delays, and **certain nodes are consistently under heavier load** than others. **Which is the most likely cause of the performance issue?**

- **A.** The network bandwidth between the nodes is insufficient
- **B.** The data is configured with a low number of partitions
- **C.** There is a data skew in the input dataset
- **D.** The cluster nodes have different hardware capacities

> [!success]- Answer & why
> **C — Data skew in the input dataset.** ✅
> The tells: **some tasks much slower than others** + **certain nodes consistently overloaded** = uneven data distribution → a few **hot partitions** carry most of the rows (the "celebrity problem"). Fix = **salting** / repartitioning.
> - **A network** — would slow things broadly, not pin load to *specific* nodes ❌
> - **B low partitions** — under-parallelizes the whole job (idle cluster), but load stays relatively even, not *certain* nodes hot ❌
> - **D different hardware** — possible, but not the "most likely" textbook cause of *task-level* skew ❌
> Trigger: **"some tasks slow + specific nodes hot" = data skew.** See [[Study Q&A Log]] Q4–Q6 (skew + salting) and `dea-materials/study-notes/01_fundamentals.md`.

---

## Q6 — Constraints on a linking table (referential integrity)

Tables: `games(game_id, name, time)`, `players(player_id, name)`, and `games_players(game_id, player_id)` — a **linking table** tracking which players participated in which games. **Each player can participate only once in a given game.** To ensure referential integrity and enforce this rule at the DB level, which constraints on `games_players` are correct?

- **A.** `FOREIGN KEY (game_id) REFERENCES games(game_id)`, `FOREIGN KEY (player_id) REFERENCES players(player_id)`, `UNIQUE (game_id, player_id)`
- **B.** `PRIMARY KEY (game_id)`, `FOREIGN KEY (player_id) REFERENCES players(player_id)`
- **C.** `FOREIGN KEY (game_id) REFERENCES players(player_id)`, `FOREIGN KEY (player_id) REFERENCES games(game_id)`
- **D.** `FOREIGN KEY (game_id, player_id) REFERENCES games(game_id, player_id)`

> [!success]- Answer & why
> **A.**
> - Two **foreign keys** each pointing to the *correct* parent (`game_id`→`games`, `player_id`→`players`) → referential integrity.
> - **`UNIQUE (game_id, player_id)`** → enforces "each player only once per game" (the composite pair can't repeat). *(A composite PRIMARY KEY on both columns would also work.)*
> - **B** — `PRIMARY KEY (game_id)` makes each **game** appear only once → only one player per game ❌; also no FK to games.
> - **C** — references are **swapped** (`game_id`→players, `player_id`→games) → broken referential integrity ❌
> - **D** — `games` has no `player_id` column, so the reference is invalid; and it enforces neither proper FKs nor uniqueness ❌
> Concept: **linking/junction table = two FKs + a UNIQUE (or composite PK) on the pair.** This is the **Integrity** quality dimension → see [[Data Validation & Profiling]].

---

## Q7 — Impact analysis before disabling a pipeline

A product owner intends to **disable an existing data pipeline** that aggregates sales data. Before proceeding, they want to **understand the impact** this action will have on **downstream processes and reports** that rely on this data. Which approach should the data engineering team implement?

- **A.** Introduce an additional layer of data validation.
- **B.** Enhance the data transformation logic.
- **C.** Create a backup of the existing data sets.
- **D.** Implement data lineage throughout the pipeline.

> [!success]- Answer & why
> **D — Implement data lineage.**
> **Data lineage** maps where data comes from and where it flows (its dependencies), so you can do **impact analysis** — see exactly which downstream processes/reports depend on this pipeline before disabling it.
> - **A validation** — checks data *quality*, doesn't reveal downstream dependencies ❌
> - **B transformation** — changes how data is processed; irrelevant to impact ❌
> - **C backup** — preserves the data, but tells you nothing about who *uses* it ❌
> Trigger: **"understand downstream impact / what depends on this data" = data lineage.**

---

## Q8 — Columnar format for cost + query performance

A financial analytics company wants to **reduce storage costs** and **improve SQL query performance** on large transaction datasets currently stored in a **row-oriented format** (high storage, slow queries). Which file format should they transition to?

- **A.** Migrate the data to a more efficient relational database
- **B.** Archive older data to cold storage
- **C.** Convert the datasets to a columnar storage format ✅
- **D.** Implement data deduplication across the datasets

> [!success]- Answer & why
> **C — Columnar storage format (Parquet / ORC).**
> Columnar stores values **by column** instead of by row, which wins on both goals:
> - **Storage cost** ↓ — same-type values sit together → far better compression (often 2–4×+ smaller than CSV).
> - **Query speed** ↑ — analytical queries read **only the needed columns** (column pruning) instead of whole rows; plus min/max stats per chunk enable predicate pushdown.
> - **AWS angle:** Athena & Redshift Spectrum **charge by data scanned** → Parquet cuts scan size → directly cheaper + faster. Combine with **partitioning + compression**.
>
> - **A relational DB** — still row-oriented; doesn't fix the format problem ❌
> - **B cold storage** — cheaper for *old* data only; no query-speed gain ❌
> - **D dedup** — minor savings; ignores the row-vs-column I/O issue ❌
> Trigger: **"row-oriented → high storage + slow analytical queries" = switch to columnar (Parquet/ORC).** See file formats in `dea-materials/study-notes/01_fundamentals.md`.

---

## Q9 — Storing diverse data for analytics + ML

An insurance company will launch a product using **diverse data sources** — transactional systems (structured), customer **emails** (unstructured), and **weblogs** (semi-structured). To support **analytics and machine learning**, which storage solution should the team choose?

- **A.** Relational database
- **B.** Data warehouse
- **C.** In-memory database
- **D.** Data lake

> [!success]- Answer & why
> **D — Data lake.**
> A data lake stores **all data types together** — structured, semi-structured, and unstructured — in raw form (**schema-on-read**), which is exactly what "diverse sources + emails + weblogs" needs. It's also the natural feed for **ML** (raw, flexible data). On AWS = **S3**.
> - **A relational DB** — structured/rigid schema only; can't handle emails/weblogs well ❌
> - **B data warehouse** — structured, **schema-on-write**; great for BI on clean structured data, not raw unstructured + ML ❌
> - **C in-memory DB** — for ultra-fast access/caching, not bulk diverse storage ❌
> Triggers: **"diverse / mixed data types + raw + ML" = data lake (schema-on-read).** vs **"structured + BI/reporting + schema-on-write" = data warehouse.** See storage paradigms in `dea-materials/study-notes/01_fundamentals.md`.

---

## Q10 — Classifying multi-source data (data types)

A digital marketing firm integrates data from **social media interactions, website logs, and customer feedback surveys**. Given the nature of data from these multiple sources, which describes the data correctly?

- **A.** Structured data
- **B.** Semi-structured data
- **C.** Unstructured data
- **D.** Free-form data

> [!success]- Answer & why
> **B — Semi-structured data.**
> Social media (JSON from APIs), **website logs**, and survey responses carry **self-describing structure** — tags/keys/fields (JSON, XML, log formats) — but **no rigid relational schema**. That's the definition of semi-structured. (Website logs are the textbook semi-structured example.)
> - **A structured** — fixed schema / relational tables; too rigid for this mix ❌
> - **C unstructured** — no inherent structure at all (raw images, video, free text/audio); these sources have *some* structure ❌
> - **D "free-form data"** — **not a real category** (made-up distractor, like "Minimax Sampling" in Q4) ❌
>
> Quick taxonomy:
> | Type | Examples |
> |---|---|
> | Structured | RDBMS tables, CSV with fixed schema |
> | **Semi-structured** | **JSON, XML, logs**, emails (headers) |
> | Unstructured | images, video, audio, raw text |

---
