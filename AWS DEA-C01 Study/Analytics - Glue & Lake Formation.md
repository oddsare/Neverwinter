---
title: AWS DEA-C01 — Glue & Lake Formation
exam: AWS Certified Data Engineer – Associate (DEA-C01)
type: notes
section: Analytics
tags:
  - aws
  - dea-c01
  - glue
  - analytics
created: 2026-07-01
updated: 2026-07-01
---

# AWS Glue & Lake Formation

---

## AWS Glue

**What it is:** Serverless, fully managed **ETL (Extract, Transform, Load)** service. Glue is the glue between your data sources — it extracts data, transforms it (applies structure, cleans it), and loads it to a destination.

**Key use cases:**
- **Transform** data — restructure, reformat, convert (CSV → Parquet, etc.)
- **Clean** data — remove nulls, fix bad records, deduplicate
- **Enrich** data — join with other sources before analysis
- Give structure to unstructured data
- Custom ETL jobs (Python or Scala, runs on Apache Spark under the hood)

### How ETL Jobs Are Triggered
| Trigger Type | Description |
|---|---|
| **Schedule** | Cron-based (e.g. every day at midnight) |
| **On Demand** | Manually triggered |
| **Event-driven** | Triggered by CloudWatch/EventBridge events |

---

## Glue Data Catalog & Crawler

**Data Catalog** = a **metadata repository** — stores information about where your data is, what schema it has, and how to access it. Does NOT move or copy data.

**Glue Crawler** = scans your data sources (S3, RDS, Redshift, DynamoDB), extracts the schema and partition structure, and populates the Data Catalog automatically.

Once cataloged, **unstructured data can be queried like structured data** by:
- Amazon Athena
- Redshift Spectrum
- EMR
- QuickSight

> [!tip] Exam focus
> **Glue Crawler populates the Data Catalog — it does NOT move or copy data.** The catalog is just metadata. The actual data stays where it is (e.g. in S3).
>
> **Glue + Hive integration:** Glue ETL cannot run Hive jobs (Spark only). However, the **Glue Data Catalog can serve as a Hive metastore** — EMR clusters running Hive can use the Glue Data Catalog for their metadata instead of maintaining a separate metastore. Connection = through the catalog, not the ETL engine.
>
> **HiveQL ≈ SQL** for exam purposes — HiveQL is just SQL-like syntax for querying data in Hadoop/Hive.
>
> **Existing Hive metastore migration:** If you already have an on-premises Hive metastore, you can **import it into the Glue Data Catalog** — no need to rebuild metadata from scratch when migrating to AWS.

### S3 Partitioning Strategy
The Glue Crawler extracts partitions based on **how your S3 data is organized** — so think about how you want to query your data lake first, then organize your folders to match.

| Query pattern | Folder structure |
|---|---|
| Query primarily by **time** | `yyyy/mm/dd/device` |
| Query primarily by **device** | `device/yyyy/mm/dd` |

**Why it matters:** Query engines (Athena, Redshift Spectrum) do **partition pruning** — they only scan the folders relevant to your query. If your partitions match your query pattern, you scan less data = faster + cheaper.

> [!tip] Exam focus
> **Design S3 partitions to match your dominant query pattern.** Wrong partition layout = full table scans = slow and expensive.

---

## Glue ETL — Key Details

- Runs on **serverless Apache Spark**
- Code is auto-generated in **Python or Scala** (you can modify it or bring your own Python/PySpark scripts)
- Targets: S3, RDS, Redshift, Glue Data Catalog
- Provision additional **DPUs (Data Processing Units)** to increase Spark job performance
- Enable **job metrics** to see how many DPUs you actually need — plot max needed vs max allocated in the **Glue Console** (not CloudWatch); also tracks ETL data movement
- 1 DPU = 2 executors; reserve **1 DPU for the Spark driver** + **1 executor for management overhead** — account for this when sizing jobs

### DynamicFrame
- Glue's core data abstraction — like a **Spark DataFrame but ETL-aware**
- Collection of **DynamicRecords** — each record is self-describing and carries its own schema
- Handles messy/inconsistent data better than a rigid DataFrame
- Scala and Python APIs
- Can convert between Spark DataFrame ↔ Glue DynamicFrame

### Encryption
- **Server-side encryption** — data at rest in S3
- **SSL/TLS** — data in transit
- Managed via KMS

### Job Bookmarks
- Prevents reprocessing old data — remembers where the last job left off
- Only processes **new data** on re-run
- **Does NOT track updated rows** — only detects new rows

> [!tip] Exam focus
> **Job bookmarks = incremental processing (new data only). Does NOT detect updates.**

### Glue Flex Jobs
- Use spare/leftover AWS compute capacity for non-urgent jobs
- ~30–35% cheaper than standard
- **Not for:** streaming jobs, ML jobs, or hard deadlines

---

## Glue Studio
- Visual **drag-and-drop** ETL workflow builder — no code required
- Creates DAGs (directed acyclic graphs) for multi-step pipelines
- Sources: S3, Kinesis, Kafka, JDBC
- Output: S3 or Glue Data Catalog

---

## Glue DataBrew
- Visual **data preparation** tool (no-code/low-code)
- 250+ ready-made transformations; save reusable "recipes"
- Great for **PII handling** — can mask, encrypt, hash, delete sensitive fields

> [!tip] Exam focus
> **DataBrew = visual data prep and PII masking. NOT for complex Spark-based ETL** — use Glue ETL for that.

---

## Glue Data Quality (DQDL)
- Define data quality rules in **DQDL (Data Quality Definition Language)**
- Rules can fail the job or report to CloudWatch

```
Rules = [
    RowCount >= 100,
    IsComplete "customer_id",
    IsUnique "customer_id",
    Completeness "email" > 0.99,
    ColumnValues "age" between 0 and 120
]
```

> [!tip] Exam focus
> The exam explicitly tests DQDL syntax. Know `IsComplete`, `IsUnique`, `Completeness`, `ColumnValues`.

---

## Glue vs EMR

| Dimension | Glue | EMR |
|---|---|---|
| Management | Fully **serverless** | Managed cluster (EC2) |
| Engine | **Spark only** | Spark, Hive, Pig, Presto, HBase, Flink, etc. |
| Use case | ETL pipelines, Data Catalog | Complex big data, multi-engine, ML |
| Best for | Simple-medium ETL | Full Hadoop ecosystem, custom configs |

> [!tip] Exam focus
> **Glue = serverless Spark ETL. EMR = when you need Hive, Pig, or non-Spark engines, or full cluster control.**

---

## AWS Lake Formation

**What it is:** Simplifies building a **secure data lake on S3**. Sits on top of Glue and adds centralized access control, auditing, and fine-grained permissions.

**Architecture:**
```
On-premises / AWS sources
         ↓
   Lake Formation
   (Crawlers, ETL, Data Catalog, Security, Cleaning)
         ↓
Athena   Redshift   EMR   QuickSight
```

### What Lake Formation Adds Over Glue
- **Cell-level security** — restrict access by row, column, or specific cells
- Centralized permissions across all consumers
- Auditing

### Fine-Grained Security
| Security type | How |
|---|---|
| Row-level | All columns + row filter |
| Column-level | All rows + specific columns |
| Cell-level | Specific rows + specific columns |

> [!tip] Exam focus
> **Lake Formation = cell-level security that IAM alone cannot do.** Use Lake Formation when you need row/column/cell-level access control on a data lake. **No charge for Lake Formation itself** — underlying services (Glue, S3, Athena) are billed separately.

---

## Quick Reference

| Scenario | Answer |
|---|---|
| ETL pipeline, serverless, Spark | AWS Glue |
| Visual no-code ETL builder | Glue Studio |
| Visual data prep + PII masking | Glue DataBrew |
| Metadata catalog for S3/RDS/Redshift | Glue Data Catalog + Crawler |
| Incremental ETL (skip old data) | Glue Job Bookmarks |
| Row/column/cell-level security on data lake | AWS Lake Formation |
| Non-Spark engines (Hive, Pig, Presto) | EMR |
