---
title: Storage — S3 Tables & Apache Iceberg
exam: AWS Certified Data Engineer – Associate (DEA-C01)
section: Storage
type: topic-note
tags:
  - aws
  - dea-c01
  - storage
  - s3
  - iceberg
  - analytics
created: 2026-06-11
---

# Storage — S3 Tables & Apache Iceberg

## Apache Iceberg (the foundation)
An **open table format for data lakes** — turns files in S3 into a real, queryable table. Petabyte-scale, originally from **Netflix**.
- **ACID compliance** — transactional consistency for adds/removals/modifications
- **Row-level updates & deletes** — GDPR compliance
- **Schema evolution** — safely change table structure as data changes
- **Hidden partitioning** — partition evolution without breaking queries
- **Time travel** — query historical versions, verify changes, easy rollback
- **Efficient metadata management**
- **Engine-agnostic:** works with **Spark, Flink, Trino, Presto, Hive**; integrated with **Glue, Redshift, EMR, Athena**. → *"SQL through anything that supports Iceberg"* = runs on many engines.

## S3 Tables (AWS's managed Iceberg storage)
**Purpose-built S3 storage for tabular/analytics data, built on Iceberg.** Better than storing Iceberg files in a plain bucket because AWS **manages maintenance**:
- **Up to 3× faster** queries, **10× higher** transaction rate — *but more expensive*.
- **Managed table optimization (automatic):** **compaction**, **snapshot management**, **unreferenced-file removal**.
- **IAM + SCP** (AWS Organizations) integration.
- New, distinct service namespace: **`s3tables`**.

### Table Buckets (a new bucket type)
- ARN under s3tables: `arn:aws:s3tables:Region:Account:bucket/bucket-name`
- **Structure:** Table Bucket → **Namespaces** (≈ a "database") → **Tables**.
```
TABLE BUCKET
├─ Namespace A → Table 1, Table 2
└─ Namespace B → Table 3, Table 4
```
- Types: **Customer-managed** and **AWS-managed** (S3 metadata tables, inventory tables).
- **Names only unique within your account** (Account Regional Namespace) · **must be lowercase** (or won't integrate with **Lake Formation / Glue**) · cannot be changed · can have IAM policies + tags.

### Integration
- **Spark can talk to S3 Tables directly.**
- The **Glue Data Catalog** or **SageMaker Lakehouse** connects AWS analytics services (Athena, Redshift, EMR, QuickSight, Data Firehose…) to S3 Tables.
- **Iceberg REST endpoint** lets 3rd-party tools (Spark, Flink, etc.) connect.

### S3 Tables Intelligent-Tiering
- Cost-optimization feature: **auto-moves table data between storage tiers based on access.**

> [!tip] Exam focus
> - **Iceberg** = open table format (ACID, time travel, schema evolution) for data lakes; engine-agnostic.
> - **S3 Tables** = managed Iceberg on AWS → faster + auto-maintenance (compaction/snapshots/cleanup), but pricier; lives in **Table Buckets** (`s3tables` namespace).
> - Table bucket names: **lowercase**, unique **within account**, integrate with Glue/Lake Formation.
> - "Want a managed, high-performance Iceberg table store with automatic compaction" → **S3 Tables**.

## Related
- `dea-materials/study-notes/02_storage.md` · [[Storage - S3 Concepts]] · [[Storage - S3 Storage Classes]]
