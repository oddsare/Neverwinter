---
title: Storage — EBS, EFS, Instance Store
exam: AWS Certified Data Engineer – Associate (DEA-C01)
section: Storage
type: topic-note
tags:
  - aws
  - dea-c01
  - storage
  - ebs
  - efs
created: 2026-06-11
---

# Storage — EBS, EFS, Instance Store

> [!info] Mental model
> **EC2 = the brain (compute: CPU + memory).** Storage options differ in how they attach:
> - **EBS** = a virtual hard drive for **one** instance (in **one AZ**).
> - **EFS** = a shared network file system for **many** instances (**across AZs**).
> - **Instance Store** = physically-attached, super-fast, but **ephemeral** (lost on stop).

## EBS (Elastic Block Store)
A **network-attached virtual disk** (block storage) for an EC2 instance — like an external/USB hard drive you can detach and reattach.

| Fact | Detail |
|---|---|
| Attach scope | **One EC2 instance at a time** (exception: io1/io2 **Multi-Attach**) |
| Location | **Locked to one AZ** — move via **snapshot → restore** in another AZ/region |
| Persistence | **Survives** instance stop/terminate (if "delete on termination" is off) |
| Backups | **Snapshots** = incremental backups stored in **S3** |
| Acts like | A raw disk the OS formats and reads/writes |

**Volume types:** **gp3/gp2** (general-purpose SSD) · **io1/io2** (high-performance, provisioned IOPS SSD) · **st1** (throughput-optimized HDD) · **sc1** (cold HDD).

## EFS (Elastic File System) — preview
- A **managed shared file system** — **many EC2 instances** can mount it **at the same time, across multiple AZs**.
- Like a shared network folder (NFS). Scales automatically; pay per use.
- Use when **multiple instances need shared access** to the same files.

## Instance Store — preview
- **Physically attached** to the host → **highest performance**.
- **Ephemeral**: data is **lost when the instance stops/terminates**. Good for cache/scratch/temp data only.

> [!tip] Exam focus
> - **EBS** = 1 instance, 1 AZ, persistent, snapshot to move/back up.
> - **EFS** = shared by many instances across AZs.
> - **Instance Store** = fastest but ephemeral (don't store anything you need to keep).
> - Move an EBS volume to another AZ → **snapshot it, restore in the target AZ**.

## Related
- `dea-materials/study-notes/02_storage.md` · [[Storage - S3 Concepts]]
