---
title: AWS DEA-C01 — Study Q&A Log
exam: AWS Certified Data Engineer – Associate (DEA-C01)
type: study-log
tags:
  - aws
  - dea-c01
  - study
created: 2026-06-09
updated: 2026-06-09
---

# AWS DEA-C01 — Study Q&A Log

> [!info] How this works
> A running log of every question I ask while studying, each with a **condensed** answer. Newest entries at the bottom. Full reference notes live in `Downloads/dea-materials/study-notes/`.

## Topics covered
- [[#Q1 — Data sampling techniques (overview)]]
- [[#Q2 — Random Sampling]]
- [[#Q3 — Stratified, Systematic, Cluster, Convenience, Judgmental]]
- [[#Q4 — Data Skew (vs statistical skew) + balancing partitions]]
- [[#Q5 — How to use salting (worked examples + runnable demo)]]
- [[#Q6 — Is the salting code SQL? (PySpark DataFrame API vs Spark SQL)]]
- [[#Q7 — Cleaning bad data: drop/impute null & 0 salaries (PySpark + SQL)]]
- [[#Q8 — Mean vs Median vs Mode (impute the value, not the count)]]
- [[#Q9 — Combining two tables on different scales (normalize before merge)]]
- [[#Q10 — Pivot: CASE vs native PIVOT — performance?]]

---

### Q1 — Data sampling techniques (overview)
> [!question] What are the different types of data sampling?

The DEA-C01 course lists **6** techniques. Two are emphasized, four are grouped as "Others":

| Technique | One-line idea |
|---|---|
| **Random** ⭐ | Every record has an equal, independent chance of selection |
| **Stratified** ⭐ | Split population into homogeneous subgroups (strata), random-sample *within* each → guarantees subgroup representation |
| Systematic | Pick every *k*-th record from an ordered list |
| Cluster | Split into clusters, randomly pick whole clusters |
| Convenience | Sample whatever's easiest to reach (biased) |
| Judgmental | Expert hand-picks records (biased) |

> [!tip] Exam focus
> Random vs Stratified is the key distinction. "Equal chance for all" = Random. "Must keep minority subgroups proportionally represented" = Stratified.

---

### Q2 — Random Sampling
> [!question] Explain random sampling.

**Idea:** every record has an *equal, independent* chance of being picked — no rules or structure. (Mental model: names in a hat.)

- **Use it for:** fast, cheap, *unbiased* shrinking of a huge dataset for exploration, profiling, or testing a transform before a full run. It's the unbiased baseline.
- **Watch out:** with **imbalanced data**, rare subgroups can be under-sampled or missed by chance (e.g., 2% fraud rows may barely appear). → that's what **Stratified** fixes.

> [!tip] Exam focus
> Fast/unbiased, no subgroup concern → **Random**. Scenario stresses a minority class must stay proportionally represented → **not** Random; pick **Stratified**.

---

### Q3 — Stratified, Systematic, Cluster, Convenience, Judgmental
> [!question] Walking through the remaining five sampling types (with my own understanding + corrections).

| Technique | What it is | Key nuance / correction |
|---|---|---|
| **Stratified** | Split into homogeneous groups (strata), random-sample *within* each | Sample from **every** stratum, often proportionally — that's what guarantees representation |
| **Systematic** | Pick every *k*-th record by a fixed rule | Start at a **random** point, then step by *k*. Gotcha: hidden periodicity aligned to *k* → bias |
| **Cluster** | Split into clusters (natural groups), **randomly pick whole clusters, take everyone inside** | NOT "pick around samples". Chosen for cost when groups are scattered |
| **Convenience** | Sample whatever's easiest to reach (e.g., poll your own class) | Cheap, fast, **biased** — not representative |
| **Judgmental** (purposive) | **Expert hand-picks** records judged most relevant | **No randomization at all**; bias = reliance on human judgment |

> [!tip] Exam focus — Stratified vs Cluster (the classic confusion)
> **Stratified** = split into groups, take a *little from every group* → goal is **accuracy/representation**.
> **Cluster** = split into groups, take *all of a few randomly chosen groups* → goal is **cost/convenience** when groups are geographically/logically scattered.

---

### Q4 — Data Skew (vs statistical skew) + balancing partitions
> [!question] Data skew mechanisms and balancing traffic to partitions — is this the same as left/right skew on a graph?

**Not the same thing** (but related):
- **Statistical skew** (graph) = asymmetric *shape* of a value distribution (right/positive skew = long right tail, mean > median).
- **Data skew** (exam topic) = uneven *volume* of data/traffic across **partitions/nodes** in a distributed system.
- Link: a heavy-tailed (statistically skewed) key distribution is what *causes* hot partitions — but the exam means the distributed-systems problem.

**Definition:** unequal data distribution across nodes/partitions (Spark, EMR, Kinesis, DynamoDB, Redshift).

**"Celebrity problem":** even partitioning ≠ even traffic. Partition IMDb by actor → Brad Pitt's partition gets hammered while others idle.

**Causes:** non-uniform distribution · inadequate partition strategy · temporal skew (time-concentrated bursts).
**Impact:** hot partition = bottleneck (throttling, slowest task gates the stage); other nodes waste capacity. Monitor + alert.

**5 fixes:**

| Fix | What it does |
|---|---|
| **Salting** ⭐ | Add random prefix/suffix to the key → spreads hot keys across partitions |
| Adaptive Partitioning | Dynamically adjust partitioning to observed data |
| Repartitioning | Periodically redistribute to rebalance |
| Sampling | Diagnose the distribution first, then choose a strategy |
| Custom Partitioning | Domain-specific partition rules |

> [!tip] Exam focus
> "Hot partition / one node overloaded / uneven throughput (Spark, DynamoDB)" → **Salting**. Note **sampling** reappears here as a *diagnostic* step before fixing skew.

---

### Q5 — How to use salting (worked examples + runnable demo)
> [!question] Show me how to use salting.

**Core idea:** a salt = a small random number (`0..N-1`) appended to a hot key so its rows scatter across `N` partitions. You pay later by **un-salting** (recombining buckets) on read/aggregate.

**1. Spark skewed aggregation (two-phase):**
```python
N = 100
phase1 = (df.withColumn("salt", (F.rand()*N).cast("int"))
            .groupBy("actor","salt").agg(F.count("*").alias("partial")))   # spread
result = phase1.groupBy("actor").agg(F.sum("partial").alias("total"))      # un-salt
```

**2. Spark skewed join (salt + explode):** salt the BIG side; replicate the SMALL side `N`× (one copy per bucket) so no match is lost; join on `(key, salt)`.

**3. DynamoDB write sharding:** `PK = "2026-06-09#" + random(0..9)` spreads writes; **read = scatter-gather** all shards. Deterministic salt (`hash(id)%N`) lets point-reads skip the fan-out.

> [!note] Trade-off
> Salting always trades **write/compute simplicity for read complexity**. Pick `N` big enough to break the hotspot, small enough that un-salt/fan-out stays cheap.
> Spark 3.0+ **AQE skew-join** (`spark.sql.adaptive.skewJoin.enabled`) auto-handles skewed *joins*; manual salting still needed for skewed *aggregations*.

> [!example] Runnable demo (proved it works)
> `dea-materials/code/salting/salting_demo.py` (pure Python, no deps) — simulated `hash(key)%8` partitioning on 500k rows where Brad Pitt = 70%:
> **Before salting:** one partition 23.8× overloaded. **After salting:** 2.7× (balanced).
> Real PySpark version: `salting_spark.py`.

---

### Q6 — Is the salting code SQL? (PySpark DataFrame API vs Spark SQL)
> [!question] For case 1 and 2, what type of code is that — SQL?

**No — it's Python (PySpark), using the Spark DataFrame API** (method chaining: `df.withColumn(...).groupBy(...).agg(...)`, with `F = pyspark.sql.functions`).

The module name `pyspark.sql` refers to Spark's **structured/relational engine**, not to writing SQL text. The DataFrame API and real SQL both compile to the **same plan** (Catalyst optimizer) → equivalent performance, two syntaxes.

Same logic, three ways:
- **(A) DataFrame API** = `df.groupBy("actor","salt").agg(...)` ← what cases 1 & 2 are
- **(B) Spark SQL** = `spark.sql("SELECT actor, ... GROUP BY actor, salt")` (SQL string handed to Spark)
- **(C) Pure SQL** = the `SELECT ... GROUP BY` statement itself

> [!tip] Exam focus — which service speaks which
> **Glue ETL / EMR** → code: **PySpark (DataFrame API)** or Scala.
> **Athena / Redshift (Spectrum)** → pure **SQL**.
> Recognizing the language tells you which service a scenario is describing.

#### Annotated: case-1 salted aggregation, step by step

```python
phase1 = (df.withColumn("salt", (F.rand()*100).cast("int"))
            .groupBy("actor","salt").agg(F.count("*").alias("partial")))
result = phase1.groupBy("actor").agg(F.sum("partial").alias("total"))
```

**Goal:** count rows per `actor` without one hot actor (Brad Pitt) overloading a single task.

| Step | Code | What it does |
|---|---|---|
| 1 | `df` | The skewed input DataFrame — e.g. one row per click, with an `actor` column. |
| 2 | `F.rand()` | Spark function: a random float in `[0.0, 1.0)`, **one value per row**. |
| 3 | `* 100` | Scales it to `[0.0, 100.0)`. |
| 4 | `.cast("int")` | Truncates to an integer `0–99` → **100 salt buckets**. |
| 5 | `.withColumn("salt", …)` | Adds the result as a new column named `salt`. Returns a *new* DataFrame (DataFrames are immutable). |
| 6 | `.groupBy("actor","salt")` | Groups by the **composite key** (actor, salt). Brad Pitt's rows now split across ~100 groups instead of 1. |
| 7 | `.agg(F.count("*").alias("partial"))` | Counts rows in each (actor, salt) group; names it `partial`. **This is the shuffle** — the hot key's work is now spread across up to 100 tasks. |
| → | `phase1` | Holds partial counts: `(Brad Pitt, 0, 7012)`, `(Brad Pitt, 1, 6988)`, … |
| 8 | `phase1.groupBy("actor")` | Regroup by `actor` **only** — drop the salt = the **"un-salt"** step. |
| 9 | `.agg(F.sum("partial").alias("total"))` | Sum the partials back into the true per-actor total. This 2nd shuffle is tiny (~100 rows per actor). |
| → | `result` | Same answer as a naive `groupBy("actor")` — but the heavy work was spread out, no single hot task. |

> [!note] Why two phases
> Phase 1 (`actor+salt`) does the expensive count in parallel; phase 2 (`actor`) just adds up a handful of partials. The naive one-phase `groupBy("actor")` would funnel all 700k Brad Pitt rows into one task — the bottleneck salting removes.

---

### Q7 — Cleaning bad data: drop/impute null & 0 salaries (PySpark + SQL)
> [!question] In Spark, 10 people have null or 0 salary — how do I get rid of this bad data?

**Bad data = two cases:** `null` (missing) and `0` (present but invalid).

**Option A — Drop the bad rows:**
```python
clean = df.filter((F.col("salary").isNotNull()) & (F.col("salary") > 0))
```
```sql
SELECT * FROM employees WHERE salary IS NOT NULL AND salary > 0;
```

> [!warning] Gotcha — null comparisons
> Any comparison with `null` returns `null` (not `true`), and `filter`/`WHERE` keep only strictly-`true` rows. So `salary > 0` **already drops nulls too** (plus zeros/negatives). Keeping the explicit `isNotNull()` is just clearer intent.
> Null-only shortcut (does NOT catch zeros): `df.na.drop(subset=["salary"])`.

**Option B — Impute instead of drop** (don't lose rows / avoid bias):
```python
median = df.filter(F.col("salary") > 0).approxQuantile("salary", [0.5], 0.01)[0]
filled = (df.withColumn("salary", F.when(F.col("salary") <= 0, None).otherwise(F.col("salary")))
            .fillna(median, subset=["salary"]))
```
Use **median**, not mean — salary distributions are right-skewed.

> [!tip] Exam focus
> This is the **Completeness + Accuracy** quality dimensions. Exam rewards *deliberate* handling: "remove invalid records" → **filter/drop**; "don't lose data / handle missing values" → **impute**. Drop = simple but loses data & can bias; impute = keeps rows but invents values.

---

### Q8 — Mean vs Median vs Mode (impute the value, not the count)
> [!question] Clarifying: median is not the average; and what number do we actually impute?

**Median ≠ average.** Three different measures:

| Measure | Definition | Example `1, 3, 7, 8, 100` |
|---|---|---|
| **Mean** (average) | sum ÷ count | (1+3+7+8+100)/5 = **23.8** |
| **Median** | the **middle value** when sorted | **7** |
| **Mode** | the **most frequent** value | most-repeated |

The single `100` drags the **mean** up to 23.8, but the **median** stays at 7 → that's why salaries (right-skewed) are imputed with the **median**, which ignores outliers.

**You impute the median VALUE, not the COUNT.** Example: 100 salaries (1–10), 25 of them are `7`, median = `7`.
- Fill the 10 missing people with **7** (the value).
- The "25" is only *how many* earn 7 (that also makes 7 the **mode**) — it is **not** imputed anywhere.
- Result: 35 people at 7 (original 25 + 10 filled).

> [!note] In code
> `approxQuantile("salary", [0.5], …)` returns the value at the 50% mark — the median value (e.g. `7.0`) — which is what you pass to `fillna`.

---

### Q9 — Combining two tables on different scales (normalize before merge)
> [!question] Two tables of 5 movies — one rated 1–5 stars, one rated 1–10. How do I combine them?

**Key insight:** can't mash columns together — different **scales** (1–5 vs 1–10) must be reconciled first.

**Process:**
1. **Reliable join key** — join on `movie_id`, NOT `title` (titles break on typos/casing). → *Integrity dimension.*
2. **Join type** — inner = only movies in both; **full outer** = keep movies missing from one side.
3. **Normalize to a common scale** (the crux). Don't just ×2 (maps 1→2, wrong floor). Use **min-max**:
   - stars: `(stars-1)/4` → 0..1
   - rating: `(rating-1)/9` → 0..1
4. **Combine** — average the normalized values (×10 for a 0–10 final).

```sql
SELECT a.movie_id, a.title,
       ROUND(((a.stars-1)/4.0 + (b.rating-1)/9.0)/2*10, 2) AS combined_0_10
FROM stars a JOIN ratings b ON a.movie_id = b.movie_id;
```
```python
joined = stars.join(ratings, on="movie_id")
combined = joined.withColumn("combined_0_10",
    F.round((((F.col("stars")-1)/4.0)+((F.col("rating")-1)/9.0))/2*10, 2))
```

Result: Inception 9.44 · The Room 1.11 · Interstellar 7.64 · Cats 1.81 · Parasite 10.00.

> [!tip] Exam focus
> Combining sources on different scales/units = **Consistency** quality problem. Pattern: **align key → align units/scale (normalize) → merge.** "Two sources, different scales/units" → normalize first. Know join types: inner drops non-matches, outer keeps them (common distractor).

#### Annotated: the combine code, line by line

```sql
SELECT a.movie_id, a.title,
       ROUND(((a.stars-1)/4.0 + (b.rating-1)/9.0)/2*10, 2) AS combined_0_10
FROM stars a JOIN ratings b ON a.movie_id = b.movie_id;
```

| Piece | What | Why |
|---|---|---|
| `SELECT a.movie_id, a.title` | id + title from `stars` (alias `a`) | Identify the row; take once from one side |
| `(a.stars-1)/4.0` | Normalize stars → 0–1 | `-1` sets floor to 0; `/4` = range (5−1) |
| `(b.rating-1)/9.0` | Normalize rating → 0–1 | range = 10−1 = 9 → now same scale as stars |
| `/2` | Average the two | Equal weight per source |
| `*10` | Rescale 0–1 → 0–10 | Human-friendly final score |
| `ROUND(…,2)` | 2 decimals | Readability |
| `JOIN … ON a.movie_id=b.movie_id` | Inner-join on shared key | Pair each movie's two scores; inner = only movies in both |

> [!warning] Why `4.0`/`9.0` not `4`/`9`
> In many SQL engines (Postgres etc.) integer ÷ integer **truncates** — `(2-1)/4` → `0`, silently breaking normalization. `.0` forces float division. (Spark `/` returns double anyway, but it's a safe habit.)

```python
joined   = stars.join(ratings, on="movie_id")            # inner join; string key de-dups movie_id
combined = joined.withColumn("combined_0_10",            # add new immutable column
    F.round((((F.col("stars")-1)/4.0)+((F.col("rating")-1)/9.0))/2*10, 2))  # normalize→avg→×10→round
```

> Same work as the SQL — join, normalize, average, rescale — two syntaxes, one engine (see Q6).

#### Min-max normalization — the intuition

Formula: **`(value − min) / (max − min)`** → maps any scale onto **0→1**.
- **Subtract min** → shift the floor to 0 (worst score = 0).
- **Divide by range (max − min)** → squeeze the span into 0→1 (best score = 1).
- **Numerator = how far *this* value is above the floor; denominator = total height of the scale.** The denominator is **fixed** per scale (4 for stars, 9 for ratings); only the numerator changes with the value.

| stars | `(s-1)/4` | | rating | `(r-1)/9` |
|---|---|---|---|---|
| 1 | 0.00 | | 1 | 0.00 |
| 3 | 0.50 | | 7 | 0.67 |
| 5 | 1.00 | | 10 | 1.00 |

> Why it matters: afterwards a 5-star and a 10-rating both = **1.0** ("max praise"), while a raw 5-*rating* correctly = 0.44 ("middling") — not equal to 5 stars. Without normalizing, both raw `5`s would look equal despite meaning opposite things.

> [!question]- Why not just double the stars (1→2, 5→10)?
> Doubling only **stretches**; min-max also **shifts** (`− min`). The floor breaks:
>
> | stars | ×2 | min-max→0–10 |
> |---|---|---|
> | 1 | **2** | **0** |
> | 5 | 10 | 10 |
>
> Top aligns (5×2=10), but a 1-star (worst) becomes **2** while the worst 1–10 rating is **1** → worst star movie looks *better* than worst rated movie. Doubling can't go below 2.
> **Root cause:** the star scale starts at **1**, not 0; the `− 1` corrects that offset. Doubling would only work if both scales started at **0** (e.g. 0–5 → ×2 → 0–10).

---

### Q10 — Pivot: CASE vs native PIVOT — performance?
> [!question] Is `SUM(CASE…)` pivoting more taxing than a native `PIVOT` command, or about the same?

**About the same.** Native `PIVOT` is mostly **syntactic sugar** — the optimizer rewrites it into the same scan + conditional aggregation that `SUM(CASE WHEN…)` expresses → similar/identical execution plans.

Real cost drivers = **data scanned · the GROUP BY/aggregation · indexes/partitioning · columnar engine** — not the keyword.

| | `SUM(CASE…)` | native `PIVOT` |
|---|---|---|
| Portability | works everywhere (standard SQL) | engine-specific |
| Flexibility | high (multi-aggregate, custom buckets) | rigid, fixed shape |
| Readability | verbose | concise |
| Performance | ≈ same | ≈ same |

**Genuine wrinkle:** *dynamic* pivoting (categories unknown ahead of time) needs **dynamic SQL** in both — cost is in query **compilation**, not the pivot mechanics, and hits both equally.

> [!tip] Exam focus / principle
> Don't choose on performance — they're equivalent. Choose on **portability/flexibility (CASE)** vs **conciseness (PIVOT)**. General DE principle: *surface syntax usually compiles to the same plan; performance comes from data volume, scan, and aggregation — not which keyword you typed.*

---
