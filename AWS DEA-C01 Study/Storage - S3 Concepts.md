---
title: Storage — S3 Concepts
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

# Storage — S3 Concepts

Running notes from the Storage chapter (clarifications worked through). Core reference: `dea-materials/study-notes/02_storage.md`.

## Default encryption = SSE-S3
- Since **Jan 2023**, S3 **automatically encrypts every new object at rest** with **SSE-S3** (AES-256, keys fully managed by AWS) — no action, no extra cost.
- You can override per-bucket/object with SSE-KMS, SSE-C, or DSSE-KMS, but if you do nothing you still get SSE-S3.
- **Key idea:** encryption at rest is *always on* — the only question is **who manages the keys**.

## Bucket name = global · bucket data = regional
| Thing | Scope |
|---|---|
| Bucket **name** | **Global** — unique across all regions & accounts (Shared Global Namespace) |
| Bucket **itself / its data** | **One region** — chosen at creation; objects physically stored there (replicated across AZs *within* that region) |
- "Global" applies only to the **name**, not the data. A bucket does **not** span regions.
- To get data into another region → create a separate bucket there + use **Cross-Region Replication (CRR)**.
- ⚠️ Exam trap: the global namespace makes S3 *look* global, but **buckets are regional**.

## Bucket list = account-global (no region switching)
- The S3 **console / `ListBuckets`** shows **all your buckets across every region in one list** — unlike EC2/RDS/Lambda, you do **not** switch the region selector to see them.
- Each bucket still **lives in one region** (shown in the "AWS Region" column).
- This account-wide flat list is *why* bucket names must be globally unique — no two names can collide in that single list.
- **Reconcile:** the *list* is global; each *bucket's data* is regional. Both true at once.

## Object URLs: plain URL fails, presigned URL works
Observed: uploading an image, the **general/plain object URL gives Access Denied**, but the **"encrypted" URL for my account works**.

- **Why the plain URL fails:** objects are **private by default** (Block Public Access on + no public-read permission). The plain URL `https://bucket.s3.amazonaws.com/image.jpg` is an unauthenticated request → **403 Access Denied**.
- **Why the signed URL works:** that "encrypted" link is a **Presigned URL** — it carries your **account's temporary signature** (`X-Amz-Signature`, `X-Amz-Credential`, `X-Amz-Expires`…) baked into the query string. It grants **time-limited, authenticated access** using *your* permissions, so it works without making the object public.
- **Presigned URL facts:** generated via SDK/CLI/console; default expiry varies (CLI ~3600s); inherits the creator's permissions; great for **temporary download/upload** access without changing bucket policy or making data public.

> [!tip] Exam focus
> "Give a user temporary access to a private object **without making the bucket public**" → **Presigned URL**. Plain S3 URL on a private object = **403**.

## S3 Security: IAM policy vs Bucket policy
**IAM** = defines, per **identity**, which **actions (API calls)** are allowed/denied on which **resources** (with optional conditions).
- Identities = **users, groups, roles** (not just users). Roles matter most — EC2/Lambda *assume* a role to call S3.
- Allow **and** Deny; default is **implicit deny** (nothing allowed unless explicitly granted).
- Actions are tied to resources, e.g. `s3:GetObject` on `arn:aws:s3:::my-bucket/*`, optionally with conditions (IP, MFA…).

| | Attached to | Answers |
|---|---|---|
| **IAM policy** (identity-based) | a user / group / role | "what can *this identity* do?" |
| **Bucket policy** (resource-based) | the bucket itself | "who can access *this bucket*?" |

### Resource-based policies (explained)
A **resource-based policy** is attached **directly to the resource** (the S3 bucket, an SQS queue, SNS topic, Lambda function, KMS key…) instead of to an identity. For S3, the resource-based policy **is the Bucket Policy** (a JSON document on the bucket).

**The defining difference = the `Principal` element.**
- **Identity-based** (IAM) policy → **no `Principal`**; the "who" is implied by *what it's attached to* (the user/role). It says: *"this identity can do X on resource Y."*
- **Resource-based** policy → **must name a `Principal`** = *who* (which account, user, role, or AWS service) is allowed/denied. It says: *"principal Z can do X on me (this resource)."*

```json
{
  "Effect": "Allow",
  "Principal": { "AWS": "arn:aws:iam::111122223333:root" },  // <-- WHO (other account)
  "Action": "s3:GetObject",
  "Resource": "arn:aws:s3:::my-bucket/*"
}
```

**Why it matters — cross-account:** because the resource itself names the principal, a bucket policy can grant **another AWS account** access *directly*, without you having a user in their account. That's why **cross-account = resource-based policy**. (Identity-based policies can't reference a principal from a different account on their own.)

Other resource-based policies you'll meet: **S3 bucket policy, SQS, SNS, Lambda, KMS key policy, API Gateway**.

> [!tip] Exam focus
> - **Cross-account** access → **bucket policy** (resource-based, names the `Principal`).
> - Grant **EC2 / Lambda** access → use an **IAM role** (don't hardcode keys).
> - **Force encryption** or **make public** → bucket policy.
> - **Has a `Principal`? → resource-based. No `Principal`? → identity-based.**
> - Access is granted if *either* an IAM policy **or** a resource policy allows it **and** nothing explicitly denies it. An explicit **Deny always wins**.

### S3 access evaluation rule (slide, verbatim)
> An IAM principal can access an S3 object **if**:
> - the user **IAM permissions ALLOW** it **OR** the **resource (bucket) policy ALLOWS** it
> - **AND** there's **no explicit DENY**

- **OR** = union: *either* an IAM allow **or** a bucket-policy allow is enough to grant access.
- **AND no explicit Deny** = a single explicit `Deny` in *either* policy **overrides** any Allow and blocks access.
- Mental model: start at **implicit deny** → an Allow (from either side) opens it → an explicit **Deny** slams it shut no matter what.

## S3 Bucket Policies (JSON anatomy)
JSON-based, resource-based policies on a bucket. **Resources = buckets and objects. Effect = Allow / Deny.**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicRead",
      "Effect": "Allow",
      "Principal": "*",
      "Action": [ "s3:GetObject" ],
      "Resource": [ "arn:aws:s3:::examplebucket/*" ]
    }
  ]
}
```

| Field | Meaning |
|---|---|
| `Version` | Policy language version — always `2012-10-17` |
| `Statement` | Array of one or more permission rules |
| `Sid` | Statement ID — optional label (here `"PublicRead"`) |
| `Effect` | **Allow** or **Deny** |
| `Principal` | **WHO** it applies to. `"*"` = **everyone / anonymous (public)** |
| `Action` | Which API call(s), e.g. `s3:GetObject` (read an object) |
| `Resource` | Which bucket/objects (ARN) the rule covers |

### Resource ARNs — `bucket` vs `bucket/*`
- `arn:aws:s3:::examplebucket` → the **bucket itself** (bucket-level actions like `s3:ListBucket`)
- `arn:aws:s3:::examplebucket/*` → **every object inside the bucket** (object-level actions like `s3:GetObject`)
- The `/*` is a **wildcard = "all keys in the bucket."** Since S3 has no real folders (keys are flat — see "fake directories" above), `/*` means *everything in the bucket*, not a literal folder. A narrower prefix like `examplebucket/images/*` = all objects whose key starts with `images/`.

> [!warning] This example = public read
> `Effect: Allow` + `Principal: "*"` + `s3:GetObject` on `bucket/*` makes **all objects publicly readable**. In practice **Block Public Access** (on by default) would **override** this and still block it.

> [!tip] Exam focus
> - Object-level permission (GetObject/PutObject) needs the **`/*`** ARN; bucket-level (ListBucket) needs the **bucket ARN without `/*`**. Mixing them up = denied. A very common exam gotcha.
> - `Principal: "*"` = public. `Action: s3:GetObject` = download/read.

## Three ways something accesses S3
| Who's accessing | What grants access | Policy type |
|---|---|---|
| **Anonymous / public user** wants a file | The **bucket** needs an **S3 bucket policy** allowing public access | Resource-based |
| **IAM user** in your account | That user has an **IAM policy** allowing the bucket | Identity-based |
| **EC2 instance** (app on it) | The instance uses an **EC2 instance role** with IAM permissions | Identity-based (via role) |

### What is an EC2 instance?
- **EC2 = Elastic Compute Cloud.** An EC2 instance is a **virtual server (VM) in AWS** — a computer in the cloud. Pick CPU/memory/OS/storage, run apps on it, shut it down when done.
- **In S3 security:** an app on EC2 that needs S3 should **attach an IAM Role** to the instance (an *instance profile*) → the instance gets **temporary, auto-rotating credentials** to call S3.
- ❌ Never hardcode AWS access keys on the server (leak risk, hard to rotate). ✅ Use an instance role.

> [!tip] Exam focus
> "How should an application on EC2 access S3?" → **Attach an IAM role to the instance** (never embed access keys). One of the most-tested best practices.

## Generating a bucket policy (AWS Policy Generator)
Tool: **AWS Policy Generator** — https://awspolicygen.s3.amazonaws.com/policygen.html (official, legacy; the S3 console also has a built-in editor).

Workflow: **type → effect → principal → action → ARN (+`/*`) → generate**
1. **Type** → select **S3 Bucket Policy** (tool also does IAM / SQS / SNS / VPC endpoint).
2. **Effect** → Allow or Deny.
3. **Principal** → who (`*` = public, or a specific user/account ARN).
4. **Action** → e.g. `s3:GetObject` (read/download an object).
5. **ARN** → the resource location, then **add `/*`** → because `GetObject` is **object-level**, it needs `arn:aws:s3:::bucket/*` (the objects), not the bucket ARN.
6. **Add Statement** → **Generate Policy** → paste the JSON into the bucket's **Permissions** tab.

> [!tip] Always review the generated JSON
> Confirm the ARN, the `/*`, and that **`Principal: "*"` is intentional** (that means public). Object-level action → `/*`; bucket-level action (`ListBucket`) → no `/*`.

## Making an object public = two gates
```
Public access  =  Bucket policy ALLOWS it   AND   Block Public Access (BPA) is OFF
```
- **Both must open.** Public policy + BPA **on** = still blocked. BPA **off** + no public policy = still private.
- **BPA is ON by default** and **overrides** bucket policies — when you add a public policy the console warns you and makes you disable BPA.
- Tested example (worked): `Principal:"*"` + `s3:GetObject` on `aws-start-practice/*` → objects publicly readable *because BPA was off*.
- ⚠️ Public = readable by the **entire internet**, no auth. Fine for throwaway practice buckets; the #1 cause of real-world S3 data leaks otherwise (why AWS defaults BPA on).

> [!tip] Exam focus
> "Bucket policy allows public but objects still not accessible" → **Block Public Access is still ON.** Need BPA off **and** a permissive policy.

## S3 Versioning
- Keeps **multiple versions** of an object — same key overwrite bumps the version (1, 2, 3…).
- **Enabled at the bucket level.** Best practice for protection.
- **Protects against unintended deletes** (restore a previous version) and gives **easy rollback**.
- ⚠️ **Files that existed *before* versioning was enabled have version `"null"`** — enabling versioning doesn't retro-assign version IDs to old objects; only **new** uploads/overwrites get real version IDs.
- **Suspending** versioning does **NOT delete** existing previous versions — it just stops creating new ones (new overwrites get version `"null"`).
- **Deleting** a versioned object adds a **delete marker** (the object is hidden, not truly gone) → can be recovered by removing the marker.

> [!tip] Exam focus
> - Pre-versioning objects = version **`null`**.
> - Suspend ≠ delete (old versions stay).
> - Delete on a versioned bucket = **delete marker** (recoverable), not a real delete.
> - **CRR/SRR replication requires versioning** on *both* source and destination buckets.

## S3 Replication (CRR & SRR)
**Setup requirements:**
1. **Versioning ON** in *both* source and destination buckets.
2. Give S3 an **IAM Role** with replication permissions (console can auto-create it).
3. Copying is **asynchronous**.

**The IAM permissions (NOT public — it's a service role S3 assumes):**
- Read source: `s3:GetReplicationConfiguration`, `s3:ListBucket`, `s3:GetObjectVersionForReplication`, `s3:GetObjectVersionAcl`, `s3:GetObjectVersionTagging`
- Write destination: `s3:ReplicateObject`, `s3:ReplicateDelete`, `s3:ReplicateTags`
- ❗ This is **least-privilege**, internal — the *opposite* of `Principal:"*"` public access.
- **Cross-account:** the *destination* bucket also needs a **bucket policy** allowing the source account's replication role to write in.

**Two types:**
| | Meaning | Use cases |
|---|---|---|
| **CRR** | **Cross-Region** Replication | Compliance, **lower-latency** access in other regions, **cross-account** replication |
| **SRR** | **Same-Region** Replication | **Log aggregation**, live replication between **prod ↔ test** accounts |

**Replication behavior & limits:**
- **Only NEW objects** replicate (after enabling). **Old/existing objects are NOT replicated** → use **S3 Batch Replication** to copy existing objects.
- **Delete markers** → replication is **optional**. If enabled, a delete in source copies the **delete marker** → object is **hidden / soft-deleted on the destination too** (still recoverable, versions remain underneath). It replicates the *soft delete*, **not** a permanent delete.
- **Permanent deletions (delete by version ID) are NEVER replicated** → the destination copy stays intact (prevents malicious/accidental version wipes from propagating).
- **No chaining** (replication isn't transitive): if **bucket1 → bucket2** and **bucket2 → bucket3**, the objects bucket1 sent into bucket2 will **NOT** flow on to bucket3 — each rule is a single hop.

> [!tip] Exam focus
> Replication needs **versioning on both** buckets + an **IAM role** (not public access). New objects only · no chaining · version-ID deletes not replicated.

## Durability vs Availability (two different metrics)
| Metric | Question it answers | Value |
|---|---|---|
| **Durability** | Will my data be **lost**? | **11 nines (99.999999999%)** — same across **ALL** storage classes |
| **Availability** | Can I **access** it right now (uptime)? | **Varies by class** — S3 Standard = **99.99%** |

- **Durability is always 11 nines**, every class (data stored redundantly across multiple AZs). AWS's analogy: store **10,000,000 objects → lose ~1 every 10,000 years**.
- **Availability changes** by class: Standard **99.99%**, One Zone-IA **99.5%** (single AZ), etc.
- Memory hook: **Durability = "won't lose it" (fixed 11 9's). Availability = "can reach it" (varies).**
- **What the % means in downtime:** S3 Standard **99.99% ≈ 52.6 min/year** of allowed unavailability. (99.9% ≈ 8.8 hrs/yr; 99.999% ≈ 5.3 min/yr.) Availability is the per-**storage-class** SLA target.

> [!tip] Exam focus
> Don't confuse them. Durability = 11 9's for everything. The number that differs between storage classes is **availability** (and cost/retrieval).

## S3 Lifecycle Rules (automate storage-class moves / deletion)
**Where:** bucket → **Management** tab → **Lifecycle rules** → create rule → choose action.

**Two action types:**
- **Transition** → move objects to a cheaper class after **N days** (e.g., Standard → Standard-IA @30d → Glacier @90d → Deep Archive).
- **Expiration** → delete objects after **N days** (also: delete old noncurrent versions, clean up incomplete multipart uploads).
- Scope a rule by **prefix** or **object tags**; can target current and/or noncurrent versions.

> [!warning] Lifecycle = AGE-based, NOT access-based
> Lifecycle transitions trigger on **object age (days since creation)** on a schedule **you** define — they do **NOT** track whether an object was accessed. The "move it when it stops being accessed" behavior is **Intelligent-Tiering**, a different feature.

| | **Lifecycle Rules** | **Intelligent-Tiering** |
|---|---|---|
| Moves based on | **Object age** (you set schedule) | **Access patterns** (AWS auto-monitors) |
| Control | You (fixed rules) | AWS (automatic) |
| Fee | none | monthly monitoring + auto-tier fee |

> [!tip] Exam focus
> "Move to cheaper storage **after N days**" → **Lifecycle rule (transition)**. "Optimize cost when access is **unknown/changing**" → **Intelligent-Tiering**. Don't mix them up.

## S3 Event Notifications
React to **specific object-level events** in a bucket — *"when X happens to an object, trigger something."* (Not literally "anything" — you choose the event types.)

**Events you can react to:**
- `s3:ObjectCreated:*` — PUT, POST, COPY, multipart-complete
- `s3:ObjectRemoved:*` — deletes
- `s3:ObjectRestore:*` — Glacier restore finished
- Replication events, object tagging events, etc.

**Targets (where the notification goes):**
- **SNS** (fan-out), **SQS** (queue for processing), **Lambda** (run code)
- **Amazon EventBridge** → advanced filtering + route to **18+ AWS services**

**Classic use case:** photo uploaded → `s3:ObjectCreated` → triggers **Lambda** → generates thumbnail (event-driven pipeline).

**Permissions required (each target needs its own resource-based policy granting S3 `s3.amazonaws.com`):**
- **SNS** → SNS resource (access) policy allowing S3 to **publish**
- **SQS** → SQS resource policy allowing S3 to **send messages**
- **Lambda** → Lambda resource policy allowing S3 to **invoke**
- These are **resource-based policies on the destination** (not an IAM role on S3). Console usually auto-creates them; without them, delivery silently fails.

> [!tip] Exam focus
> S3 events → **SNS / SQS / Lambda / EventBridge**. "Run code when a file lands in S3" → ObjectCreated → **Lambda**. EventBridge adds richer filtering and many more destinations.

## Related
- `dea-materials/study-notes/02_storage.md` · [[Storage - S3 Storage Classes]] · [[Practice Quiz]]
