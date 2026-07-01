---
title: AWS DEA-C01 — Progress Handoff
exam: AWS Certified Data Engineer – Associate (DEA-C01)
type: handoff
tags:
  - aws
  - dea-c01
  - handoff
created: 2026-06-16
updated: 2026-06-16
---

# AWS DEA-C01 — Progress Handoff

> [!info] What this is
> A single "where am I" anchor for the whole exam prep. Newest progress at the top of each section. Detailed notes are linked throughout.

## Exam snapshot
- **65 questions / 130 min**, pass = **720 / 1000**
- Domains: **Ingestion & Transformation 34%** · **Data Store Mgmt 26%** · **Data Ops & Support 22%** · **Security & Governance 18%**
- Highest-yield = **Analytics** (Glue, Athena, Kinesis, Redshift, EMR, OpenSearch, QuickSight, MSK) — not deeply hit yet.

## Where I am right now
- **Just finished:** Migration chapter (Application Discovery, MGN, DMS/SCT, DataSync, Snowball, Transfer Family, Data Exchange) + started Compute (EC2 purchasing options, Graviton, Lambda deep dive up to Kinesis shard behavior).
- **Next up:** Pick up at **video 122** — continuing Compute/Lambda or next section.
- **Databases — deferred drill:** will come back to do a proper quiz/notes session on DynamoDB (partition/sort keys, GSI vs LSI, Streams, DAX), RDS, Aurora, ElastiCache.
- **Quiz 3 — skipped**, will come back to it.
- **Storage weak spots to re-drill:** Known access pattern → Standard-IA (not IT), Lambda + NFS = EFS, VPC bucket policy enforcement, S3 Access Points.

## Topics covered

### Fundamentals & SQL — done
- Sampling (random ⭐, stratified ⭐, systematic, cluster, convenience, judgmental), data skew vs statistical skew, **salting** (worked demo), PySpark DataFrame API vs Spark SQL, data cleaning (drop/impute null & 0), mean/median/mode imputation, normalizing before merge, PIVOT (CASE vs native) performance.
- 4 data-quality dimensions → [[Data Validation & Profiling]]
- SQL notes: [[SQL - Aggregation]] · [[SQL - Grouping]] · [[SQL - Pivoting]] · [[SQL - Aliases]] · [[SQL - Regular Expressions]] · [[SQL - Titanic Exercise]]

### Storage — DONE
- **S3:** buckets/naming, objects/keys, encryption, presigned URLs, IAM vs resource/bucket policies + access-eval rule, public access (policy **+** Block Public Access off), versioning, replication, durability vs availability, all 7 **storage classes** + transitions + Lifecycle, S3 Tables & Iceberg.
  - → [[Storage - S3 Concepts]] · [[Storage - S3 Storage Classes]] · [[Storage - S3 Tables & Iceberg]]
- **Block/file/ephemeral:** EBS (gp2/gp3/io1-io2/st1/sc1, snapshot→S3, snapshot = cross-AZ/region move, Multi-Attach io1/io2 same-AZ, delete-on-termination root ON/extra OFF), EFS (shared NFS, multi-AZ, Lambda mounts), Instance Store (ephemeral), AWS Backup.
  - → [[Storage - EBS, EFS, Instance Store]]

### Databases — IN PROGRESS
- **DynamoDB capacity** (full note): RCU/WCU formula, 8 worked RCU examples, partitions & even throughput spread, hot-partition trap, throttling fixes, provisioned vs on-demand. → [[DynamoDB - Capacity (RCU & WCU)]]

## Quizzes
- [[Practice Quiz]] — Q1–Q10 (fundamentals/SQL): joins, git branch, PIVOT, sampling, skew, junction-table FKs, lineage, columnar, data lake, data types.
- [[Practice Quiz 2 - Storage]] — Q1–Q10: got **Q2–Q7 correct live**, **missed Q1** (chose Intelligent-Tiering; correct = Standard-IA — *known* access pattern → explicit tier).
- Full running log: [[Study Q&A Log]] (Q1–Q10).

## Key gotchas drilled
- **S3 max object size = 50 TB** (changed Dec 2025), not 5 TB.
- **Durability = 11 9's always**; **availability varies** by storage class.
- **Intelligent-Tiering = UNKNOWN/changing** access patterns; *known* pattern → pick the explicit tier.
- **Lifecycle ≠** copy-between-buckets / invoke-compute. Watch "right service, wrong job" decoys (Transfer Acceleration / Shield / KMS / Glue).
- **DynamoDB:** divide item by **4 KB (RCU) / 1 KB (WCU)**, **round up twice** (per-item + total); eventual = **½**, transactional = **×2**.
- **Hot partition** can throttle even with spare table capacity → **high-cardinality** partition key.
- **DAX = reads only** (not writes, not strongly consistent reads).
- **RRU/WRU = on-demand, RCU/WCU = provisioned** — choose by **throughput predictability, not storage size**.

## How we work (style)
- Watch a video → state my understanding / paste a quiz screenshot / ask for notes → Claude **verifies accuracy & gently corrects** → log to notes.
- **Quiz screenshots:** keep the answer **hidden in chat** (collapsed `[!success]-` callout in the note); I attempt first, then it's confirmed. Pre-revealed screenshots can be discussed openly.
- Live demos with Python (sqlite3 / pypdf) when useful.

## Source materials
- Stephane Maarek's 778-slide course; condensed section notes in `Downloads/dea-materials/study-notes/` (`00_INDEX.md` + `01_fundamentals … 12_ml_and_examtips`), raw slide text + lab code alongside.
- **Primary review folder (build here):** `AWS DEA-C01 Study/` in this vault.
