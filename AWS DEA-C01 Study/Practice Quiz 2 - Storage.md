---
title: AWS DEA-C01 — Practice Quiz 2 (Storage)
exam: AWS Certified Data Engineer – Associate (DEA-C01)
type: quiz
section: Storage
tags:
  - aws
  - dea-c01
  - quiz
  - storage
created: 2026-06-11
updated: 2026-06-11
---

# AWS DEA-C01 — Practice Quiz 2 (Storage)

> [!info] How to use
> Quiz taken after finishing **Section 3 — Storage**. Answers are in **collapsed** callouts — try the question first, then click to reveal.

## Index
- [[#Q1 — S3 Lifecycle policy (photo-sharing platform)]] — S3 / lifecycle / storage classes
- [[#Q2 — Roll back to previous versions of a file]] — S3 / versioning
- [[#Q3 — Cross-region backup + low-latency for remote analysts]] — S3 / replication (CRR)
- [[#Q4 — Trigger Lambda when a new object lands in S3]] — S3 / event notifications
- [[#Q5 — Encryption: AWS-managed keys but flexibility to change]] — S3 / encryption (SSE-KMS)
- [[#Q6 — Lambda + concurrent NFS access to on-prem data]] — Storage / EFS
- [[#Q7 — Organizing a data lake on S3 for performance]] — S3 / partitioning / prefixes
- [[#Q8 — Restrict S3 access to only the company VPC]] — S3 / security / bucket policy + VPC endpoint
- [[#Q9 — Simplify shared-bucket permissions for many teams/apps]] — S3 / Access Points
- [[#Q10 — Centralized backup across RDS, EFS, DynamoDB]] — Storage / AWS Backup

---

## Q1 — S3 Lifecycle policy (photo-sharing platform)

You are an AWS architect at CloudTech Innovations. A client, MyPhotos Inc., runs a photo-sharing platform where users upload millions of photos daily. To optimize storage costs the client needs:

- Uploaded photos **immediately accessible for fast retrieval for 30 days**.
- After **30 days** → a storage class that's **cost-effective but still fairly quick access**.
- After **365 days** → photos seldom accessed, move to **most cost-effective archival storage**.
- If a photo hasn't been accessed for **5 years** → **delete** (GDPR compliance).

Which S3 lifecycle policy meets the requirements?

- **A.** Transition to **S3 Standard-IA** after 30 days, transition to **S3 Glacier** after 365 days, delete after 5 years.
- **B.** Transition to **S3 One Zone-IA** after 30 days, transition to **S3 Glacier** after 365 days, delete after 5 years.
- **C.** Transition to **S3 Intelligent-Tiering** after 30 days, transition to **S3 Glacier Deep Archive** after 365 days, delete after 5 years.
- **D.** Transition to **S3 Intelligent-Tiering** after 30 days, transition to **S3 Glacier** after 1 year, enable Object Expiration for objects older than 5 years.

> [!success]- Answer & why
> **A — Standard-IA @30d → Glacier @365d → delete @5y.** ✅ (marked correct)
> The intended decider is requirement #2, and the **Intelligent-Tiering trap**:
> - **Intelligent-Tiering is for UNKNOWN / unpredictable access patterns.** This question gives a **known** pattern (hot 30 days → infrequent after). With a known pattern you transition directly to the explicit tier; you don't pay Intelligent-Tiering's **per-object monitoring fee** — and "millions of photos daily" makes that fee huge → IT is actually **less** cost-effective here. This kills **C** and **D**.
> - **"infrequent access + needs rapid (ms) retrieval + cost-effective" = textbook S3 Standard-IA.** That's exactly requirement #2 → **A**.
> - **B** — **One Zone-IA** is single-AZ (lose the AZ = lose the data); wrong for millions of irreplaceable photos ❌
> - **C / D** — use **Intelligent-Tiering** despite a *known* access pattern → unnecessary monitoring cost ❌
>
> Triggers: **Intelligent-Tiering = unknown/changing access pattern.** **Known pattern → pick the explicit tier (Standard-IA).** **"infrequent but fast (ms) retrieval, cheaper" = Standard-IA.** **One Zone-IA = single AZ → reproducible/non-critical only.**
>
> ⚠️ Caveat (why this question is debatable): requirement #3 says **"most cost-effective archival,"** which *literally* = **Glacier Deep Archive** (cheapest tier, 12–48h retrieval) — that points at C. The author instead reads it as plain **Glacier (Flexible Retrieval)** generically. Know both: **cheapest archival = Deep Archive**; **archive needing occasional minutes-to-hours retrieval = Glacier Flexible.**
>
> Gotcha: S3 Lifecycle **expiration is based on object age (creation date), not last-access date** — "delete after 5 years" = 5 years after upload, not since last access. Exam still accepts lifecycle expiration as the GDPR-deletion mechanism.
>
> See [[Storage - S3 Storage Classes]] (transition waterfall + Lifecycle scenarios) and [[Storage - S3 Concepts]].

---

## Q2 — Roll back to previous versions of a file

A data engineering team uses Amazon S3 to store **critical configuration files** for their applications. These configs are **updated frequently**, and sometimes they need to **roll back to previous versions** due to unforeseen issues. To ensure they can **retrieve previous versions of a file**, which action should they take regarding S3?

- **A.** Enable S3 Object Lock for the bucket.
- **B.** Enable S3 Versioning for the bucket.
- **C.** Enable S3 Replication for the bucket.
- **D.** Enable S3 Transfer Acceleration for the bucket.

> [!success]- Answer & why
> **B — Enable S3 Versioning.**
> Versioning keeps **every version of an object** (each overwrite/delete creates a new version ID), so you can **restore/roll back** to any previous version — exactly the requirement. A delete just adds a **delete marker**, leaving prior versions recoverable.
> - **A Object Lock** — **WORM** (write-once-read-many) immutability for compliance/retention; *prevents* deletion/overwrite. It actually *requires* versioning to work, but it's about locking, not rollback ❌
> - **C Replication** — copies objects to another bucket/region (DR, latency). Doesn't give you version history of a file (and itself **requires versioning** enabled) ❌
> - **D Transfer Acceleration** — speeds **uploads/downloads** via CloudFront edge locations; nothing to do with versions ❌
> Trigger: **"roll back / retrieve previous versions / recover overwritten or deleted objects" = S3 Versioning.** Note: **Versioning is the prerequisite for both Object Lock and Replication.**
> See [[Storage - S3 Concepts]] (versioning, replication).

---

## Q3 — Cross-region backup + low-latency for remote analysts

A data engineering team at Company XYZ stores transaction logs in Amazon S3 (essential for operational reporting **and compliance**). Primary bucket is in **`us-east-1`**. To ensure they have a **backup in another geographic location** AND provide **low-latency access to Asia-based analysts**, which step should they take?

- **A.** Enable S3 Versioning on the primary bucket and set up a lifecycle policy to transfer logs to a bucket in **`ap-southeast-1`**.
- **B.** Set up S3 Transfer Acceleration on the primary bucket and direct the Asia-based analysts to the accelerated endpoint.
- **C.** Enable S3 Cross-Region Replication (CRR) on the primary bucket and replicate the logs to a bucket in **`ap-southeast-1`**.
- **D.** Enable S3 Multi-Region Access Point to allow Asia-based analysts to fetch logs from the closest region.

> [!success]- Answer & why
> **C — Cross-Region Replication (CRR) to `ap-southeast-1`.**
> CRR hits **both** requirements at once: it keeps a **continuously replicated copy in another region** (geographic backup / compliance / DR) **and** puts the data **physically in Asia** → low-latency reads for the Asia analysts. (Requires **versioning** on both buckets.)
> - **A** — Lifecycle policies only **transition between storage classes or expire** objects; they **cannot copy/move objects to another bucket or region**. Invalid mechanism ❌
> - **B Transfer Acceleration** — speeds transfers to/from the **single** `us-east-1` bucket via edge locations; creates **no second copy** → no geographic backup ❌
> - **D Multi-Region Access Point** — a single global endpoint that **routes** to the nearest bucket, but it **relies on replication (CRR) already being set up** to have data in Asia. On its own it provides no backup; it's a routing layer on top of C, not a substitute ❌
> Triggers: **"backup/copy in another region + low-latency local access" = Cross-Region Replication (CRR).** **Lifecycle ≠ copy between buckets.** **MRAP = routing layer that needs CRR underneath.** Same-region copy = **SRR**.
> See [[Storage - S3 Concepts]] (replication: CRR vs SRR).

---

## Q4 — Trigger Lambda when a new object lands in S3

Company ABC wants a pipeline where new files uploaded to an S3 bucket **trigger a Lambda function** for processing; processed data is stored in a different S3 bucket. The goal is to **simplify creation and maintenance**. Which configuration meets the requirements?

- **A.** Set up an S3 Lifecycle policy on the source bucket to trigger the Lambda upon object creation.
- **B.** Enable S3 Transfer Acceleration on the source bucket and use Lambda's scheduled events to periodically check for new files.
- **C.** Set up an **S3 event notification** on the source bucket to trigger the Lambda when a new object is created.
- **D.** Use AWS Data Pipeline with a data node for the source bucket and periodically poll for new files to trigger the Lambda.

> [!success]- Answer & why
> **C — S3 Event Notification (`s3:ObjectCreated:*`) → Lambda.**
> S3 event notifications fire on object events (create/delete/restore) and can target **Lambda, SNS, SQS, or EventBridge** directly. This is the **native, event-driven, no-polling, low-maintenance** way to run Lambda the instant a file lands — exactly "simplify creation and maintenance."
> - **A Lifecycle** — only **transitions storage classes / expires** objects; **cannot invoke Lambda**. Wrong mechanism ❌
> - **B Scheduled events (polling)** — periodic checks add **latency**, miss "instant," and are **more** to maintain; Transfer Acceleration is irrelevant (just speeds transfers) ❌
> - **D AWS Data Pipeline + polling** — heavyweight, **deprecated-style** orchestration; polling again = latency + maintenance overhead, the opposite of "simplify" ❌
> Triggers: **"run Lambda when a new object is uploaded / event-driven, no polling" = S3 Event Notification.** **Lifecycle can't invoke compute.** **Polling = latency + maintenance → wrong when the ask is real-time/simple.** (For advanced filtering/fan-out, route S3 events through **EventBridge**.)
> See [[Storage - S3 Concepts]] (event notifications).

---

## Q5 — Encryption: AWS-managed keys but flexibility to change

Company DEF mandates **all data at rest in S3 must be encrypted**. They want the encryption **keys managed by AWS**, but also want the **flexibility to change/control the keys** when required. Which encryption method best fits?

- **A.** Server-Side Encryption with Customer-Provided Keys (**SSE-C**).
- **B.** Server-Side Encryption with Amazon S3 Managed Keys (**SSE-S3**).
- **C.** Server-Side Encryption with AWS Key Management Service (**SSE-KMS**).
- **D.** Client-Side Encryption with a client-side master key.

> [!success]- Answer & why
> **C — SSE-KMS.**
> Two requirements: **(1) AWS manages the keys** and **(2) flexibility to control/rotate/change keys.** **SSE-KMS** stores keys in **AWS KMS** (AWS-managed infrastructure) while giving you a **customer-managed KMS key (CMK)** you control — create/disable/rotate keys, set key policies, and get **CloudTrail audit logs** of every key use. That's the "managed by AWS *but* with flexibility" sweet spot.
> - **B SSE-S3** — keys fully owned/rotated by AWS with **zero customer control** → fails the "flexibility to change keys" requirement ❌
> - **A SSE-C** — **you** provide and manage the keys (sent with every request); not "managed by AWS" ❌
> - **D Client-Side** — you encrypt before upload and manage keys yourself; not AWS-managed, more overhead ❌
> Triggers: **"AWS-managed keys + need control/rotation/audit/key policies = SSE-KMS."** **"fully AWS-managed, no control needed = SSE-S3."** **"I must supply my own keys = SSE-C (server-side) or Client-Side."** Note SSE-KMS has **KMS API limits + cost per request** (watch for high-throughput scenarios); **SSE-KMS is now the S3 default** for new buckets.
> See [[Storage - S3 Concepts]] (encryption: SSE-S3 / SSE-KMS / SSE-C / client-side).

---

## Q6 — Lambda + concurrent NFS access to on-prem data

An e-commerce company has large transaction data stored on-premises in an **NFS file share**. They want **AWS Lambda to analyze** this data while **continuing to use an NFS file system** for storage, ensuring Lambda can **concurrently access** the data. Best solution?

- **A.** Use **Amazon EFS** to create a file system and synchronize data from the on-prem NFS. Configure the Lambda function to access the EFS file system.
- **B.** Migrate data from on-prem NFS to an **Amazon S3** bucket. Configure Lambda to access data from S3.
- **C.** Establish a **VPN** and directly access the on-prem NFS file share from the Lambda function.
- **D.** Use **Amazon EC2 with an attached EBS volume**, synchronize data from on-prem NFS, then let Lambda access data via the EC2 instance.

> [!success]- Answer & why
> **A — Amazon EFS.**
> Two requirements: **(1) keep using an NFS file system** and **(2) Lambda concurrent access.** **EFS is a managed NFS (NFSv4.1)** file system, and **Lambda natively mounts EFS** with **many concurrent function executions sharing the same file system** — a direct match. Sync the on-prem NFS data into EFS (e.g. **AWS DataSync**) and point Lambda at it.
> - **B S3** — abandons the **NFS file system** requirement (S3 is object storage, not a file system/NFS). Great otherwise, but fails the stated constraint ❌
> - **C VPN to on-prem NFS** — fragile, adds latency, and on-prem NFS isn't built to serve **massively concurrent Lambda** invocations; not the clean managed answer ❌
> - **D EC2 + EBS** — **EBS is single-instance block storage**, can't be the shared NFS layer for many concurrent Lambdas; adds an EC2 middle-man = more to manage ❌
> Triggers: **"Lambda + shared/concurrent file access + NFS/POSIX file system = Amazon EFS"** (Lambda mounts EFS). **EBS = one instance, block, not shared.** **S3 = object storage, not a file system.** Move on-prem → AWS file data with **AWS DataSync**.
> See [[Storage - EBS, EFS, Instance Store]] (EFS = multi-AZ shared NFS) and [[Storage - S3 Concepts]].

---

## Q7 — Organizing a data lake on S3 for performance

A company is designing a **data lake on S3**. To ensure **high performance when accessing the data**, which best practice should they adopt for organizing data in the bucket?

- **A.** Store all data as a **single large file** and use Lambda to parse required segments.
- **B.** Use a **flat structure**, avoiding any prefix or "folder" hierarchy.
- **C.** **Partition data** based on commonly accessed attributes and use a **consistent naming scheme for prefixes**.
- **D.** Enable **S3 Transfer Acceleration** so data is quickly accessible from any location.

> [!success]- Answer & why
> **C — Partition by commonly accessed attributes + consistent prefix naming.**
> Partitioning (e.g. `s3://lake/year=2026/month=06/region=us/`) lets query engines (**Athena, Redshift Spectrum, Glue, EMR**) do **partition pruning** — scan only the relevant prefixes instead of the whole dataset → far less data scanned = **faster + cheaper**. Consistent prefixes also spread requests for high throughput.
> - **A single large file** — kills parallelism; every read pulls the whole file; no pruning ❌
> - **B flat structure** — no partitions → engines must scan **everything**; the opposite of high performance ❌
> - **D Transfer Acceleration** — speeds **long-distance uploads/downloads** over the internet; does nothing for **query/analytics performance** on a data lake ❌
> Triggers: **"data lake performance / reduce data scanned = partition by common query attributes (date, region, etc.) + consistent prefixes."** Pair with **columnar formats (Parquet/ORC) + compression** for the full win. Note: modern S3 scales automatically per prefix (**3,500 PUT / 5,500 GET per prefix per sec**), so more prefixes = more parallel throughput.
> See [[Storage - S3 Concepts]] and `dea-materials/study-notes/01_fundamentals.md` (partitioning, file formats).

---

## Q8 — Restrict S3 access to only the company VPC

A company stores sensitive data in an S3 bucket. Security mandates that **all access must originate only from within the company's VPC**. Which action ensures this?

- **A.** Enable **VPC Endpoints** for Amazon S3 and associate them with the company's VPC.
- **B.** Update the **S3 bucket policy to deny all requests that do not originate from the company's VPC**.
- **C.** Enable **AWS Shield** on the S3 bucket to protect data from outside the VPC.
- **D.** Use **AWS KMS** to encrypt the S3 bucket, ensuring only the company's VPC has the decryption key.

> [!success]- Answer & why
> **B — Bucket policy that denies requests not from the VPC.** ✅ (marked correct)
> Only a **bucket policy** can *enforce* "access only from the VPC." Add a **`Deny`** with a condition on **`aws:SourceVpce`** (the VPC endpoint ID) or **`aws:SourceVpc`** — any request not coming through that endpoint/VPC is blocked. This is the actual access-control mechanism.
> - **A VPC Endpoint** — **necessary prerequisite but not sufficient.** An endpoint just provides a **private route** to S3; it doesn't *block* requests arriving over the internet. Without the bucket-policy condition, the bucket is still reachable from elsewhere. (Real-world best practice = **A + B together**: endpoint for the private path, policy to enforce it.) ❌ *alone*
> - **C AWS Shield** — **DDoS protection**, not access control / origin restriction ❌
> - **D KMS** — encrypts data at rest; doesn't restrict *who/where* can call the S3 API ❌
> Triggers: **"restrict S3 access to a specific VPC/endpoint = bucket policy with `aws:SourceVpce` / `aws:SourceVpc` condition."** **VPC endpoint = the private network path; bucket policy = the enforcement.** Shield = DDoS; KMS = encryption — neither restricts origin.
> See [[Storage - S3 Concepts]] (IAM vs resource/bucket policies, security).

---

## Q9 — Simplify shared-bucket permissions for many teams/apps

In a data pipeline, a company has **multiple applications and teams** accessing a **shared S3 bucket**. To **streamline access and simplify permissions management** for these different entities, which S3 feature should they use?

- **A.** Enable **multiple IAM roles**, each corresponding to an application or team, granting access to the bucket.
- **B.** Use **S3 Access Points** to create unique endpoints with tailored permissions for each application or team.
- **C.** Activate **S3 Transfer Acceleration** for fast, differentiated access per app/team.
- **D.** Implement **S3 Lifecycle policies** per application/team to manage their data access and retention.

> [!success]- Answer & why
> **B — S3 Access Points.** ✅ (marked correct)
> Access Points give each app/team its **own named endpoint** with its **own access-point policy**, all on the **same shared bucket**. Instead of cramming every team's rules into one giant, hard-to-maintain bucket policy, you split permissions into **per-entity policies** → simpler to manage and scale.
> - **A multiple IAM roles** — workable but **doesn't scale cleanly**; you still manage a tangle of role + bucket policies, and the question specifically asks for the **S3 feature** built for this (Access Points) ❌
> - **C Transfer Acceleration** — speeds long-distance transfers; **nothing to do with permissions** ("differentiated access" is a red herring) ❌
> - **D Lifecycle policies** — manage **storage class transitions / expiration**, not access permissions ❌
> Triggers: **"many teams/apps sharing one bucket + simplify/scale permissions = S3 Access Points"** (each = unique hostname + own policy). Don't confuse with **Multi-Region Access Points** (global routing over replicated buckets, Q3-style).
> See [[Storage - S3 Concepts]] (access management).

---

## Q10 — Centralized backup across RDS, EFS, DynamoDB

A pipeline uses **Amazon RDS, Amazon EFS, and Amazon DynamoDB**. The company wants a **centralized backup solution** with **consistent backup and restore** across these services. Which AWS service?

- **A.** Use **AWS Glue** to replicate the data and create backups in an S3 bucket.
- **B.** Deploy **AWS Data Pipeline** to periodically back up data from these services.
- **C.** Use **AWS Backup** to centrally manage backups across the mentioned AWS resources.
- **D.** Enable **AWS Shield Advanced** to automatically back up data in response to threats.

> [!success]- Answer & why
> **C — AWS Backup.** ✅ (marked correct)
> **AWS Backup** is the fully-managed, **centralized** service purpose-built for this: one place to define **backup plans** (schedule, retention, lifecycle to cold storage) and apply them across **RDS, Aurora, EFS, DynamoDB, EBS, EC2, FSx, S3, DocumentDB, Neptune, Storage Gateway**, etc. — with consistent restore and cross-region/cross-account support.
> - **A Glue** — an **ETL** service, not a backup manager; you'd hand-build fragile copy jobs ❌
> - **B Data Pipeline** — legacy orchestration; manual, not a centralized/consistent backup solution (and largely superseded) ❌
> - **D Shield Advanced** — **DDoS protection**, has nothing to do with backups (the "in response to threats" framing is the giveaway distractor) ❌
> Triggers: **"centralized / single-pane backup + restore across multiple AWS services = AWS Backup."** Same service for **PITR, backup plans, vault lock (WORM), cross-region/cross-account copies.** Shield = DDoS; Glue = ETL — neither backs up.
> See [[Storage - EBS, EFS, Instance Store]] (AWS Backup section) and `dea-materials/study-notes/02_storage.md`.

---
