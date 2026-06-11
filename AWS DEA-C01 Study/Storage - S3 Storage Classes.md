---
title: Storage — S3 Storage Classes
exam: AWS Certified Data Engineer – Associate (DEA-C01)
section: Storage
type: topic-note
tags:
  - aws
  - dea-c01
  - storage
  - s3
created: 2026-06-10
---

# Storage — S3 Storage Classes

> [!info] Big picture
> **Durability = 11 nines (99.999999999%) for ALL classes.** What changes between classes = **availability, cost, retrieval time/fee, min storage duration, and # of AZs.** Pick a class by **how often you access the data and how fast you need it back.**

## 1. S3 Standard (General Purpose)
- **Availability 99.99%** · ≥3 AZs · no min duration · no retrieval fee.
- Frequently accessed data, **low latency + high throughput**.
- Sustains **2 concurrent facility (AZ) failures**.
- **Use cases:** big data analytics, mobile/gaming apps, content distribution.

## 2. S3 Standard-IA (Infrequent Access)
- **Availability 99.9%** · ≥3 AZs · **min 30 days** · 128 KB min billable · **per-GB retrieval fee**.
- For data **accessed less often but needs rapid (ms) access** when wanted.
- Cheaper storage than Standard, but you pay to retrieve.
- **Use cases:** disaster recovery, backups.

## 3. S3 One Zone-IA
- **Availability 99.5%** · **1 AZ only** · min 30 days · 128 KB min · per-GB retrieval.
- Still 11 nines durability *within that AZ*, but **data is LOST if the AZ is destroyed**.
- Cheapest of the IA tiers (no multi-AZ redundancy).
- **Use cases:** secondary backup copies, or data you **can recreate**.

## 4. S3 Glacier Instant Retrieval
- Archive class · **millisecond retrieval** · **min 90 days** · 128 KB min · per-GB retrieval.
- For archive data **accessed ~once a quarter** but needed instantly.

## 5. S3 Glacier Flexible Retrieval (formerly "Glacier")
- Archive · **min 90 days** · 40 KB min · per-GB retrieval.
- Retrieval options (retrievals are **free**, time varies):
  - **Expedited:** 1–5 minutes
  - **Standard:** 3–5 hours
  - **Bulk:** 5–12 hours

## 6. S3 Glacier Deep Archive (cheapest, long-term)
- Archive · **min 180 days** · 40 KB min · per-GB retrieval.
- Retrieval: **Standard 12 hours**, **Bulk 48 hours**.
- **Use cases:** long-term retention / compliance you rarely (if ever) touch.

## 7. S3 Intelligent-Tiering
- **Auto-moves objects between tiers based on usage.** Small **monthly monitoring + auto-tiering fee**, but **NO retrieval charges**.
- Tiers:
  - **Frequent Access** (default)
  - **Infrequent Access** — not accessed for **30 days** (auto)
  - **Archive Instant Access** — not accessed for **90 days** (auto)
  - **Archive Access** — optional, configurable **90 → 700+ days**
  - **Deep Archive Access** — optional, configurable **180 → 700+ days**
- **Use case:** unknown/changing/unpredictable access patterns — let AWS optimize cost automatically.

## Comparison table (from slide)
| | Standard | Intelligent-Tiering | Standard-IA | One Zone-IA | Glacier Instant | Glacier Flexible | Glacier Deep |
|---|---|---|---|---|---|---|---|
| Durability | 11 9's | 11 9's | 11 9's | 11 9's | 11 9's | 11 9's | 11 9's |
| **Availability** | 99.99% | 99.9% | 99.9% | **99.5%** | 99.9% | 99.99% | 99.99% |
| Availability SLA | 99.9% | 99% | 99% | 99% | 99% | 99.9% | 99.9% |
| **AZs** | ≥3 | ≥3 | ≥3 | **1** | ≥3 | ≥3 | ≥3 |
| **Min duration** | None | None | 30d | 30d | 90d | 90d | 180d |
| Min billable size | None | None | 128 KB | 128 KB | 128 KB | 40 KB | 40 KB |
| Retrieval fee | None | None | per GB | per GB | per GB | per GB | per GB |

> [!tip] Exam focus — how to pick
> - **Frequent access** → Standard.
> - **Infrequent but instant** → Standard-IA (multi-AZ) or One Zone-IA (single AZ, recreatable data).
> - **Unknown/changing pattern** → Intelligent-Tiering (no retrieval fees).
> - **Archive, instant** → Glacier Instant Retrieval.
> - **Archive, minutes-hours OK** → Glacier Flexible.
> - **Archive, cheapest, hours-days OK** → Glacier Deep Archive.
> - Only **One Zone-IA = single AZ** (data lost if AZ gone). Min durations: IA **30d**, Glacier **90d**, Deep **180d** — early delete still charges the minimum.

## Moving between storage classes (transition waterfall)
You can **transition objects between classes** (via Lifecycle rules). Transitions flow **downward** — from warmer/pricier → colder/cheaper:

```
Standard
  ↓
Standard-IA
  ↓
Intelligent-Tiering
  ↓
One-Zone IA
  ↓
Glacier Instant Retrieval
  ↓
Glacier Flexible Retrieval
  ↓
Glacier Deep Archive   (coldest / cheapest)
```
- From any class you can transition **down to any class below it** in this order (not just the next one).
- **One direction only:** Lifecycle transitions go **down** (warmer → colder). To bring archived data *back up* (e.g., Glacier → Standard) you must **restore** the object, not "transition" it.
- Pairs with **[[Storage - S3 Concepts|Lifecycle rules]]** (age-based) — e.g., Standard → Standard-IA @30d → Glacier @90d → Deep Archive.

> [!tip] Exam focus
> Transitions are **one-way down** the hierarchy. Retrieving from Glacier = a **restore** operation (takes minutes–hours depending on tier), not a free instant move back.

## Lifecycle Scenarios (practice)

### Scenario 1
> Your application on EC2 creates image thumbnails after profile photos are uploaded to Amazon S3. These thumbnails can be easily recreated, and only need to be kept for **60 days**. The source images should be able to be **immediately retrieved for these 60 days**, and afterwards, the user **can wait up to 6 hours**. How would you design this?

**My answer (attempt):** cheaper storage for recreatable thumbnails; Glacier Flexible for the 6-hour wait; Standard-IA for first 60 days.

> [!success]- Worked answer
> **Two separate objects → two lifecycle paths:**
>
> **Source images** (immediate for 60 days, then ≤6 hr wait):
> - **S3 Standard** (or Standard-IA) for first 60 days — millisecond retrieval.
> - **Transition → Glacier Flexible Retrieval** at 60 days (Standard retrieval 3–5 hrs ≤ 6 hrs). ✅
>
> **Thumbnails** (easily recreated, keep 60 days):
> - **S3 One-Zone IA** — "easily recreated" is the keyword → single-AZ data-loss risk is fine, and it's **cheaper than Standard-IA** (which pays for multi-AZ durability you don't need).
> - **Expire (delete)** at 60 days.
>
> **My slip:** used Standard-IA for the recreatable thumbnails — the keyword "recreatable" points to **One-Zone IA** specifically. Glacier Flexible call was correct.
>
> **Keyword map:** "recreatable / reproducible copy" → **One-Zone IA** · "wait minutes–hours" → **Glacier Flexible**.

### Scenario 2
> A rule in your company states that you should be able to **recover your deleted S3 objects immediately for 30 days** (although this may happen rarely). After this time, and **for up to 365 days**, deleted objects should be **recoverable within 48 hours**.

**My answer (attempt):** "recoverable deletions = Versioning"; Standard-IA for the rare immediate-access 30 days; Glacier Deep Archive for the 48-hr phase; permanently delete after 365.

> [!success]- Worked answer
> **Key trigger: "recover deleted objects" → enable S3 Versioning** (deleted objects become recoverable *noncurrent versions*). All lifecycle rules below apply to **noncurrent versions**.
>
> ```
> Enable Versioning
> Noncurrent versions:
>   → Standard-IA           (immediate retrieval, first 30 days, rare access)
>   → Glacier Deep Archive  (after 30 days; recoverable within 48 hrs)
>   → permanently delete    (after 365 days)
> ```
>
> - **Phase 1 "immediate but rarely"** → **Standard-IA** (ms retrieval, cheaper storage; "rarely" rules out Standard).
> - **Phase 2 "within 48 hours"** → **Glacier Deep Archive** (Bulk retrieval = **48 hrs**, Standard = 12 hrs; no 24-hr option). The "48 hours" is the Deep Archive tell.
>
> **Keyword tells:** "recover deleted" → **Versioning + noncurrent-version lifecycle" · "rarely + immediate" → **Standard-IA** · "within 48 hours" → **Glacier Deep Archive**.
>
> **Standard vs Standard-IA:** identical durability/AZs/ms-retrieval; Standard-IA = cheaper storage **but** per-GB retrieval fee + 30-day min → use when access is **rare**.

## Related
- `dea-materials/study-notes/02_storage.md` · [[Storage - S3 Concepts]] · [[Practice Quiz]]
