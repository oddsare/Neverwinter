---
title: DynamoDB — Capacity (RCU & WCU)
exam: AWS Certified Data Engineer – Associate (DEA-C01)
section: Databases
type: topic-note
tags:
  - aws
  - dea-c01
  - dynamodb
  - capacity
  - rcu
  - wcu
created: 2026-06-15
---

# DynamoDB — Capacity (RCU & WCU)

> [!info] Mental model
> Capacity is billed in **units per second**. The whole game is: **how big is the item** (round it up to a block) **× how many ops/sec**, then adjust for the **operation type**.
> - **RCU** block = **4 KB**
> - **WCU** block = **1 KB**

## Read Capacity Units (RCU)

| Read type | Cost per 4 KB item | Note |
|---|---|---|
| **Eventually consistent** (default) | **0.5 RCU** = 1 read/sec | cheapest; data may be slightly stale |
| **Strongly consistent** | **1 RCU** = 1 read/sec | always the latest write |
| **Transactional** | **2 RCU** = 1 read/sec | all-or-nothing ACID reads |

### The formula (do it in this order)
1. **units = roundup(item size ÷ 4 KB)** — denominator is **always 4**, then round **up**
2. **× reads per second**
3. **read-type adjust:** ÷ 2 if **eventual**, × 2 if **transactional**, ×1 if **strong**
4. **round the final number up** (RCUs are whole numbers)

> [!warning] The two traps
> 1. **Divide by 4 KB, not by the item size.** A 9 KB item = roundup(9 ÷ 4) = **3** units (not 9/9 = 1). A 12 KB item = 12 ÷ 4 = **3**.
> 2. **Round up twice:** once on units-per-item, once on the final total. `7.5 → 8`.

### Worked examples
| Item | Reads/sec | Type | Work | RCUs |
|---|---|---|---|---|
| 3 KB | 50 | strong | roundup(3/4)=1 → 1×50 | **50** |
| 9 KB | 100 | strong | roundup(9/4)=3 → 3×100 | **300** |
| 8 KB | 100 | eventual | 8/4=2 → 2×100=200, ÷2 | **100** |
| 4 KB | 20 | transactional | 1×20=20, ×2 | **40** |
| 2 KB | 15 | eventual | roundup(2/4)=1 → 15, ÷2 = 7.5 ↑ | **8** |
| 11 KB | 40 | strong | roundup(11/4)=3 → 3×40 | **120** |
| 12 KB | 60 | eventual | 12/4=3 → 180, ÷2 | **90** |
| 5 KB | 10 | transactional | roundup(5/4)=2 → 20, ×2 | **40** |

## Write Capacity Units (WCU) — preview
- Block size is **1 KB** (not 4 KB).
- **1 WCU** = one standard write/sec for an item up to **1 KB**.
- **Transactional write** = **2 WCU** per write. (No eventual/strong distinction for writes.)
- **units = roundup(item size ÷ 1 KB)** → × writes/sec → ×2 if transactional → round up.

## Partitions & throughput distribution
- DynamoDB splits a table's data across many **partitions** (assigned by hashing the **partition key**).
- **Provisioned RCUs and WCUs are divided _evenly_ across all partitions.** Example: 300 RCU over 3 partitions = **100 RCU each**.
- **Hot-partition / hot-key trap:** if traffic skews onto one partition key, that partition can be **throttled even while the table's total capacity sits unused** — it only owns its even slice.
- **Fix:** pick a **high-cardinality partition key** so requests spread evenly (and/or write-shard with a suffix).
- Per-partition hard limits: **10 GB** storage · **3,000 RCU** · **1,000 WCU**.

## Throttling & hot-partition fixes
When a request exceeds a partition's share of capacity, DynamoDB throttles it (`ProvisionedThroughputExceededException`). Fixes:
1. **Exponential backoff** — retry the throttled request with increasing delays (+ jitter); built into the AWS SDKs automatically.
2. **Distribute partition keys** — use a **high-cardinality** key and/or **write-sharding** (append a random suffix) so load spreads across partitions instead of hammering one.
3. **DAX (DynamoDB Accelerator)** — managed in-memory cache (microsecond reads) that offloads **read** traffic → relieves **RCU** pressure for read-heavy / hot-read workloads.
   - ⚠️ **Reads only:** DAX does **not** help write (WCU) throttling, and **strongly consistent reads bypass the cache** (they hit the table directly).
4. *(Bonus)* **Adaptive capacity** is automatic — DynamoDB shifts throughput toward hot partitions and isolates hot items. **On-demand mode** removes capacity planning for spiky/unknown traffic.

## Capacity modes — provisioned (RCU/WCU) vs on-demand (RRU/WRU)
The unit name tells you the billing mode: **RCU/WCU = provisioned**, **RRU/WRU = on-demand**.

| | Provisioned (RCU/WCU) | On-demand (RRU/WRU) |
|---|---|---|
| You set | a per-second capacity rate | nothing — scales instantly |
| Billing | pay for provisioned capacity (used or not) | pay **per request** |
| Best for | **predictable / steady** traffic; cheaper at scale | **spiky / unknown / new** workloads; zero planning |
| Sizing math | 4 KB read / 1 KB write, eventual ½, txn ×2 | **identical** |

- **RRU = Read Request Unit, WRU = Write Request Unit** — the on-demand twins of RCU/WCU. **Same** 4 KB / 1 KB rounding and eventual-½ / transactional-×2 rules; you're just billed **per request** instead of provisioning a per-second rate.
- Choose by **throughput predictability, not data volume**: unpredictable/bursty traffic → **on-demand**; steady/forecastable → **provisioned** (optionally + **auto-scaling**).
- ⚠️ **Storage is billed separately** (per GB-month) in *both* modes — capacity mode is about **request throughput**, not how many TB you store.
- **Auto-scaling** (provisioned) reacts to *gradual* trends but lags sudden spikes; on-demand absorbs spikes instantly.
- You can **switch modes once per 24 hours**.

> [!tip] Exam focus
> - **RCU = 4 KB block, WCU = 1 KB block.** Mixing these up is the #1 mistake.
> - Eventual read = **half** the cost of strong; transactional read = **double**.
> - Reads are **eventually consistent by default** — you opt in to strong consistency.
> - Always **round up**: per-item units *and* the final total.
> - Throughput is **spread evenly across partitions** → a **hot partition** can throttle even with spare table capacity. Choose a high-cardinality partition key.
> - Throttled? → **exponential backoff** (handles transient spikes) + **better key distribution** (fixes the root cause). **DAX** fixes hot *reads* only — not writes, not strongly consistent reads.
> - **RRU/WRU = on-demand**, **RCU/WCU = provisioned.** Unpredictable/spiky traffic → **on-demand**; steady/predictable → **provisioned (+ auto-scaling)**. Decision is about **throughput pattern, not storage size**; same sizing math either way.

## Related
- [[Practice Quiz 2 - Storage]] · [[Study Q&A Log]]
