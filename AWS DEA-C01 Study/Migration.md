---
title: AWS DEA-C01 — Migration & Transfer
exam: AWS Certified Data Engineer – Associate (DEA-C01)
type: notes
section: Migration
tags:
  - aws
  - dea-c01
  - migration
created: 2026-06-29
updated: 2026-06-29
---

# Migration & Transfer

> [!info] What this covers
> Moving data and workloads from on-premises into AWS. Start here when planning a datacenter migration.

---

## AWS Application Discovery Service

**Purpose:** Gather information about your on-premises datacenter to **plan a migration** to AWS. Gives you visibility into what you have before you move it.

Two discovery modes:

| Mode | Also Called | What It Collects |
|---|---|---|
| **Agentless** | Discovery Connector | VM inventory, CPU/memory/disk performance history, configs |
| **Agent-based** | Discovery Agent | System config, running processes, network connections between systems |

- All data viewable in **AWS Migration Hub**

> [!tip] Exam focus
> **Agentless = VM-level info (what machines exist + performance).** **Agent-based = deeper: running processes + network connections between systems.** If the question asks about mapping dependencies between apps → agent-based.

---

## AWS Application Migration Service (MGN)

**Purpose:** Migrate physical, virtual, or cloud-based servers to AWS with minimal changes — take what you have and move it as-is.

**Strategy: Lift and Shift (Rehost)**
> Take your existing application exactly as it is and move it to AWS — no refactoring, no redesign. You're just changing where it runs. Fast and low-risk, but you don't get cloud-native benefits (yet).

- Physical servers, VMware VMs, or servers on another cloud → native **EC2 + EBS** on AWS
- Continuous **block-level replication** via a Replication Agent → staging area → production cutover
- Minimal downtime during cutover

> [!tip] Exam focus
> **MGN replaced CloudEndure Migration and AWS SMS (Server Migration Service)** — if you see either on the exam, MGN is the modern answer. MGN = lift-and-shift = rehost. Key differentiator vs Application Discovery Service: **Discovery = planning, MGN = executing** the move.

---

## AWS DataSync

**Purpose:** Move large amounts of data to and from AWS on a scheduled basis.

| Scenario | Agent Required? |
|---|---|
| On-premises / other cloud → AWS | Yes (DataSync agent installed on-prem) |
| AWS → AWS (between storage services) | No |
| AWS → On-premises (e.g. sync back to on-prem data warehouse) | Yes |

**Supported destinations:** S3 (any storage class including Glacier), EFS, FSx

> [!warning] Exam question — encryption in transit
> **DataSync encrypts data at rest and in transit using SSL/TLS.** Data confidentiality and integrity are maintained during transfer over the internet.

> [!warning] Exam question — file permissions & metadata
> **DataSync preserves file permissions and metadata** (NFS POSIX permissions, SMB permissions, timestamps, ownership). If a question asks about migrating data while keeping permissions/metadata intact → **DataSync**.

> [!tip] Exam focus
> **Agent = on-prem to AWS. No agent = AWS to AWS.** DataSync is scheduled (hourly/daily/weekly) — it is **not** a real-time streaming solution. If the question involves moving large data from on-prem to S3 on a schedule → DataSync.

---

## AWS Data Exchange

**Purpose:** A marketplace to **subscribe to third-party datasets** in the cloud and use them directly in AWS for analytics and ML.

- Data providers publish datasets (e.g. Reuters financial data, weather services, healthcare data)
- You subscribe and the data loads directly into **S3**
- **Data Exchange for Redshift:** query licensed third-party data directly in Redshift without moving it
- **Data Exchange for APIs:** consistent AWS-native access to third-party APIs

> [!tip] Exam focus
> Data Exchange = **buying/subscribing to external data**, not migrating your own. If a question involves enriching your data with third-party sources → Data Exchange.

---

## EC2 in Big Data

EC2 is the underlying compute for many big data services (EMR, DMS replication instances, etc.). Choosing the right purchasing option matters for cost.

### Purchasing Options

| Option | Use Case |
|---|---|
| **On-Demand** | Variable or unpredictable workloads — pay as you go, no commitment |
| **Reserved** (1–3 yr) | Long-running clusters or databases running for over a year (e.g. EMR master nodes, RDS) |
| **Spot** | Fault-tolerant, interruptible workloads — cheapest option but instance can be terminated randomly |

> [!warning] Exam focus — Spot instances
> **Spot = low cost but can be interrupted at any time.** Must use **checkpointing** so the job can resume from where it left off if the instance is terminated. Best for batch jobs, ML training, and other workloads that can tolerate interruption.

> [!tip] Exam focus
> **Spot = cheapest, interruptible, needs checkpointing. Reserved = predictable long-running (>1 year). On-Demand = everything else.**

### Auto Scaling (light touch)
- **EMR** — can leverage auto scaling to add/remove nodes based on workload
- **DynamoDB** — automated auto scaling of read/write capacity
- **EC2 Auto Scaling Groups** — automatically adjusts number of EC2 instances based on demand

> [!note] Low exam weight — just know these services support auto scaling, don't over-drill.

---

## AWS Graviton

**What it is:** AWS's own custom-built **ARM-based processor** — instead of Intel or AMD, AWS designed their own chip.

**Why it matters:** Offers the **best price-to-performance ratio** on AWS. Same workload, lower cost.

**Supported services:** MSK, RDS, MemoryDB, ElastiCache, OpenSearch, EMR, Lambda, Fargate

> [!tip] Exam focus
> Graviton = **ARM-based, AWS-designed, best price/performance.** Primarily optimized for Linux workloads. If a question asks about cost-efficient compute on AWS → Graviton.

---

## Docker & Containers

**What it is:** A software development platform to deploy apps packaged into **containers** — self-contained units that include everything the app needs (code, runtime, dependencies, config).

**Why it matters:** Containers run the same way on any OS or machine — no compatibility issues, no setup required. An app built on Windows works on Linux, Mac, or any cloud without modification.

### Where Docker Images Are Stored
| Repository | Description |
|---|---|
| **Docker Hub** | Public registry — the default, community-maintained |
| **Amazon ECR** | AWS's container registry (Elastic Container Registry) — supports both **private** repos and a **public** ECR Public Gallery |

> [!tip] Exam focus
> **ECR = AWS's Docker image registry.** Access errors on ECR = check IAM policy. If a question asks where to store Docker images on AWS → ECR.

---

## Container Orchestration on AWS

**The problem:** When you have many containers running across many machines, you need something to manage them — deploying, scaling, restarting failed ones. That's **container orchestration**.

### The Three Services

| Service | What it is | Choose when |
|---|---|---|
| **ECS** (Elastic Container Service) | AWS's own container orchestration | Already on AWS, want simplicity |
| **EKS** (Elastic Kubernetes Service) | Managed Kubernetes on AWS | Company already uses Kubernetes on-prem or multi-cloud |
| **Fargate** | Serverless compute layer — no EC2 to manage | Want zero infrastructure management; works with both ECS and EKS |

### Kubernetes
- **Open-source** container orchestration system originally built by Google
- Cloud-agnostic — runs on AWS, Azure, GCP, or on-prem
- EKS = Kubernetes managed by AWS
- Used by many big platforms (Databricks, etc.) under the hood

> [!tip] Exam focus
> **ECS = AWS-native, simpler. EKS = Kubernetes compatibility/portability. Fargate = serverless, no EC2, works under both.** If a question mentions existing Kubernetes workloads → EKS. If it mentions no infrastructure management → Fargate.

---

## Amazon ECS — Elastic Container Service

**How it works:** Launch Docker containers on AWS as **ECS tasks** running on an **ECS cluster**.

### Two Launch Types

**EC2 Launch Type**
- You provision and maintain the EC2 instances in the cluster
- Each EC2 instance runs an **ECS Agent** (a Docker container itself) that registers the instance with the ECS cluster
- AWS handles starting/stopping containers; you manage the underlying instances

**Fargate Launch Type**
- Fully serverless — no EC2 instances to provision or maintain
- Define CPU and memory for your task, AWS decides where to run it
- AWS handles all placement, starting, and stopping of containers

### Load Balancer Integrations

| Load Balancer | Use Case |
|---|---|
| **ALB** (Application Load Balancer) | Recommended for most ECS use cases |
| **NLB** (Network Load Balancer) | High throughput / high traffic workloads only |

> [!tip] Exam focus
> **Default to ALB for ECS.** Only use NLB when the question specifically mentions high throughput or extreme traffic volumes.

### Data Volumes — EFS with ECS
- Mount **EFS file systems** directly onto ECS tasks
- Works with **both EC2 and Fargate** launch types
- Tasks running in **any AZ** share the same data in the EFS file system — persistent shared storage across the cluster

> [!warning] Exam favourite — Fargate + EFS
> **Fargate + EFS = fully serverless.** No EC2 instances to manage, no file server to manage. If a question asks for serverless containers with persistent shared storage → **Fargate + EFS**.
>
> **Use case example:** Video processing app with ECS tasks running across multiple AZs — all tasks read/write to the same EFS file system. If a task fails and restarts in a different AZ, the data is still there. Without EFS, each container only has local storage that disappears when the container stops.
>
> ⚠️ **S3 cannot be mounted as a file system on ECS tasks** — S3 is object storage, not a file system. EFS is the answer for shared persistent storage.

### IAM Roles for ECS

| Role | Applied To | Purpose |
|---|---|---|
| **EC2 Instance Profile** | The EC2 instance (EC2 launch type only) | Used by the ECS Agent — pull images from ECR, send logs to CloudWatch |
| **ECS Task Role** | Each individual task (defined in task definition) | Gives the container permission to access AWS services |

**Example:** Task A has a role to write to S3. Task B has a role to write to DynamoDB. Each task only has the permissions it needs (least privilege).

> [!tip] Exam focus
> **Instance Profile = infrastructure level (ECS Agent). Task Role = container level (what the app can access).** Always define Task Role in the task definition — don't rely on the instance profile for app permissions.

> [!warning] Exam favourite — Fargate
> **The exam loves Fargate.** Any question mentioning "no infrastructure management", "serverless containers", or "without provisioning EC2" → **Fargate is almost always the answer.** Define the task (CPU + RAM), AWS runs it. No EC2 instances to manage.

> [!tip] Exam focus
> **EC2 launch type = you manage instances + ECS Agent registers each one. Fargate = no infrastructure management, just define the task.** If a question says "without managing infrastructure" → Fargate.

---

## Amazon EKS — Elastic Kubernetes Service

Managed Kubernetes on AWS. Use when company already uses Kubernetes on-prem or wants cloud-agnostic portability.

### Node Types
| Type | Description | Management |
|---|---|---|
| **Managed Node Groups** | EKS creates and manages EC2 nodes in an Auto Scaling Group | AWS-managed |
| **Self-Managed Nodes** | You create/register your own EC2 nodes (can use EKS Optimized AMI) | You manage |
| **Fargate** | No nodes at all — fully serverless | No management |

> [!tip] Exam focus
> **Managed Node Groups = AWS handles the EC2 ASG. Self-Managed = full control, more work. Fargate = zero node management.** Same pattern as ECS — Fargate is the "no infrastructure" answer.

### EC2/VM vs Docker Structure
```
EC2/VM:                          Docker:
Infrastructure                   Infrastructure
Host OS                          Host OS
Hypervisor                       Docker Daemon (manages all containers)
Guest OS | Guest OS              Container | Container | Container
App      | App                   App       | App       | App
```
- **EC2/VM** — each VM has its own full Guest OS (heavy, slow to start). Hypervisor emulates hardware for each VM.
- **Docker** — containers share the host OS kernel via the Docker Daemon. No Guest OS per container → lighter, faster, less resources.

### Docker Lifecycle
1. Write a **Dockerfile** — instructions for how to build the image
2. **Build** the Dockerfile → creates a **Docker image**
3. **Push** the image to a repository (Docker Hub or ECR)
4. **Run** the image → becomes a running **container**

---

## AWS Lambda

**What it is:** Run snippets of code in the cloud without managing any servers — **serverless and event-driven**. Lambda is the glue that connects AWS services together, sitting in the middle as the processing layer.

**Example:** Kinesis Data Streams → Lambda (processes records) → DynamoDB

### Key Characteristics
- **Serverless** — no EC2 to manage, AWS handles everything
- **Auto-scaling** — scales automatically per request/invocation
- **Event-driven** — triggered by events (S3 upload, Kinesis record, DynamoDB Stream, API call, etc.)
- **Max timeout: 15 minutes (900 seconds)** — not for long-running jobs
- Supports multiple languages: Python, Node.js, Java, Go, Ruby, .NET

### Main Use Cases
| Use Case | Example |
|---|---|
| **Real-time file processing** | S3 upload triggers Lambda to process/transform the file |
| **Real-time stream processing** | Kinesis Data Streams → Lambda processes records as they arrive |
| **ETL (Extract, Transform, Load)** | Lambda transforms data between services |
| **Cron replacement** | CloudWatch Events/EventBridge triggers Lambda on a schedule (no server needed) |
| **Process AWS events** | React to events from any AWS service (DynamoDB changes, SNS messages, etc.) |

### Common Integrations
| Trigger | Use Case |
|---|---|
| S3 | Run Lambda when a new file is uploaded |
| Kinesis Data Streams | **Lambda polls Kinesis** (not the other way) — pulls batches of records per shard |
| DynamoDB Streams | Trigger actions on table changes |
| Redshift | Lambda uses the **COPY command** to load new data into Redshift; uses **DynamoDB to track what's already been loaded** |
| SNS / SQS / EventBridge | Event-driven workflows |

### Why Lambda over EC2?
- **No management** — no patching, hardware failures, or monitoring servers
- **Cost** — EC2 runs 24/7 and costs money even when idle; Lambda charges only for actual execution time (milliseconds)
- **Development** — easier to split frontend and backend into separate independent functions

### Key Limits (exam numbers)
| Limit | Value |
|---|---|
| Max timeout | **15 minutes (900 seconds)** ⚠️ exam question — anything over this → use EC2 or Batch |
| Kinesis batch size | Up to 10,000 records ⚠️ large batches risk timeout (max 15 min) |
| Payload limit | 6 MB |
| Concurrency (default throttle) | 1,000 concurrent executions per region |
| /tmp ephemeral storage | Up to 10,240 MB |

### Anti-Patterns (when NOT to use Lambda)
- Jobs running **longer than 15 minutes** → use EC2 or AWS Batch instead
- Stateful applications without an external state store

> [!warning] Exam question — Lambda performance tuning
> **Lambda allocates CPU power proportionally to memory.** If a Lambda function is slow or degrading under load → **increase memory allocation**. More memory = more CPU = faster execution. This is the primary lever for Lambda performance tuning.

> [!warning] Exam question — batch size timeout trap
> **If the Kinesis batch size is too large, Lambda can timeout (max 15 min).** Reduce batch size if Lambda is timing out on Kinesis processing. Batches over **6 MB must be split** into smaller chunks.

> [!warning] Exam question — shard stalling
> Lambda retries a failed batch **until it succeeds or the data expires** from the stream. This can **stall the entire shard** (a shard = one lane of the Kinesis stream, handling a portion of the data independently). If errors aren't handled properly, one bad batch blocks all new records on that shard. Fix: use dead letter queues or bisect-on-error to skip bad records. **Use more shards** to ensure errors on one shard don't bottleneck the entire stream — other shards continue processing independently.

> [!tip] Exam focus
> **Lambda = serverless, event-driven, max 15 min.** If a question has a workload over 15 min → Lambda is wrong. Lambda is the glue between services — not a replacement for long-running compute.

---

## AWS Transfer Family

**Purpose:** Fully managed service for file transfers into and out of **Amazon S3 or Amazon EFS** using legacy file protocols. Lets old systems that can't be rewritten keep using FTP while the files land in AWS on the backend.

### Supported Protocols
| Protocol | What it is |
|---|---|
| **FTP** | File Transfer Protocol — unencrypted, VPC-only (not public-facing) |
| **FTPS** | FTP over **TLS** encryption |
| **SFTP** | SSH File Transfer Protocol — uses SSH, entirely different from FTP |

> [!note] FTPS vs SFTP
> Easy to confuse — **FTPS = FTP + TLS encryption. SFTP = SSH-based, completely different protocol.** Both are secure but different technologies.

### Pricing
- Pay per **provisioned endpoint per hour**
- Pay per **GB transferred**

> [!tip] Exam focus
> Transfer Family = the answer when **legacy FTP/SFTP clients need to interact with S3 or EFS**. **FTP is VPC-only** — not public-facing. If a question mentions an old enterprise system using SFTP that needs to land data in S3 → Transfer Family.

---

## AWS Snowball

**Purpose:** A highly secure, portable physical device to collect, process, and migrate data into and out of AWS — used when network transfer isn't practical (too slow, too costly, or no connectivity at all). Can migrate **up to petabytes** of data.

### Snow Family — Three Devices
| Device | Size | Use Case |
|---|---|---|
| **Snowcone** | Up to 14 TB | Small, portable, edge locations |
| **Snowball Edge** | Up to 210 TB (Storage) / 28 TB (Compute) | Large-scale migration, edge computing |
| **Snowmobile** | Up to 100 PB | Exabyte-scale — a literal truck/shipping container |

> [!tip] Exam focus
> **Petabytes → Snowball Edge (multiple devices). Exabytes (100 PB+) → Snowmobile.** Snowmobile is an actual truck AWS drives to your datacenter.

### Snowball Edge — Two Options
| Model | vCPUs | Memory | Storage (SSD) | Use Case |
|---|---|---|---|---|
| **Storage Optimized** | 104 | 416 GB | **210 TB** | Large-scale data migration |
| **Compute Optimized** | 104 | 416 GB | **28 TB** | Edge computing |

> [!tip] Exam focus
> **Storage Optimized = more disk (210 TB), for bulk data migration.** **Compute Optimized = less disk (28 TB) but built for running EC2/Lambda workloads at the edge.** If the question is about *moving lots of data* → Storage Optimized. If it's about *processing data on-site* → Compute Optimized.

### When to Use Snowball — Network Transfer Time Comparison

> [!warning] Common misread
> These numbers are **NOT how long Snowball takes.** They're how long it would take to transfer the data **over the network** at a given bandwidth — i.e., the time you'd spend *without* Snowball. **Gbps = Gigabits per second** (network bandwidth speed).

| Data Size | 100 Mbps | 1 Gbps | 10 Gbps |
|---|---|---|---|
| 10 TB | 12 days | 30 hrs | 3 hrs |
| 100 TB | 124 days | 12 days | 30 hrs |
| 1 PB | 3 years | 124 days | 12 days |

**Rule of thumb:** If network transfer would take **more than ~1 week**, ship a Snowball instead — it's faster than waiting on the network, even with shipping/logistics time.

> [!note] Physical device
> Snowball is a **physical device** — it has to be shipped to your site and shipped back to AWS using traditional means (couriers/freight), not a network connection. This is why it's called "offline" data transfer — the bottleneck is logistics, not bandwidth.

### Edge Computing — what "data at the edge" means
Processing data **physically close to where it's generated**, instead of shipping it to AWS first.

Use case: locations with limited/no internet — oil rigs, ships, mines, factories, military bases. These sites generate huge amounts of data (sensors, video, logs) but can't reliably send it all over the network.

Snowball Edge devices run **EC2 instances or Lambda directly on the device**, so you can:
- Preprocess/filter data on-site (send only the useful summary, not raw terabytes)
- Run ML inference locally (e.g., live video analysis)
- Do media transcoding on-site

The device is then physically shipped back to AWS to load the processed data in.

> [!tip] Exam focus
> Snowball isn't just "ship a hard drive" — it's also a **compute platform at the edge**. If a question mentions remote/disconnected locations needing local compute before data reaches AWS → Snowball Edge.

---

## AWS DMS — Database Migration Service

**Purpose:** Migrate databases to/from AWS. **Source database stays available during the migration** — no downtime for the business.

Requires an **EC2 replication instance** to run DMS tasks (DMS doesn't run in a vacuum — it needs this middleman).

### Migration Types

| Type | Example | SCT needed? |
|---|---|---|
| **Homogeneous** | Oracle → Oracle, Postgres → RDS Postgres | No |
| **Heterogeneous** | SQL Server → Aurora, Oracle → Redshift | Yes |

### Replication Modes
- **Full Load** — one-time copy of all existing data
- **CDC (Change Data Capture)** — continuously replicates ongoing changes after the initial load
- **Full Load + CDC** — copy everything first, then keep syncing changes

### SCT — Schema Conversion Tool
- Only needed for **heterogeneous** migrations (different DB engines)
- Converts the schema (table structures, data types) from one engine's format to another
- **Not needed** if source and target are the same engine

**Examples:**
| Source | Target | Why SCT |
|---|---|---|
| SQL Server / Oracle | Aurora / PostgreSQL | Different OLTP engines |
| Oracle / Teradata | Redshift | Row-based → columnar (data warehouse) |

> [!note] Row → Columnar
> Moving from a traditional row-based database (SQL Server, Oracle) to Redshift is a classic SCT use case — the schema structure needs to be converted to something columnar that Redshift can understand. Same concept as moving from OLTP to a data warehouse. PostgreSQL is **not** columnar — it's still row-based, just a different engine.

### Multi-AZ DMS
- DMS can provision a **synchronous standby replica** of the replication instance in a second AZ
- If the primary replication instance fails, DMS automatically fails over to the standby
- Provides **redundancy and high availability** for the migration itself

> [!tip] Exam focus
> **Source DB stays available during migration** — key differentiator vs other approaches. **CDC = ongoing replication of changes** (the EC2 replication instance must keep running for this). **SCT = heterogeneous only** — same engine migrations don't need it. **Multi-AZ DMS = redundancy for the replication instance, not the database itself.** S3 is a valid DMS *source*, not just a target.

---

