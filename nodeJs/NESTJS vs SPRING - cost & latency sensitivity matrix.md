# NESTJS vs Spring - cost & latency sensitivity matrix

## Table of Contents

- [NESTJS vs Spring - cost \& latency sensitivity matrix](#nestjs-vs-spring---cost--latency-sensitivity-matrix)
  - [Table of Contents](#table-of-contents)
  - [TL;DR (one line)](#tldr-one-line)
  - [Assumptions (explicit — so you can change any of them)](#assumptions-explicit--so-you-can-change-any-of-them)
  - [Formulas (and how I computed things)](#formulas-and-how-i-computed-things)
  - [Worked example — **Medium** (100,000 invocations / month)](#worked-example--medium-100000-invocations--month)
    - [Node (256 MB, warm 100 ms, cold extra 200 ms, cold-rate = 1%)](#node-256-mb-warm-100-ms-cold-extra-200-ms-cold-rate--1)
    - [Java (1 GB, warm 80 ms, cold extra 1500 ms, cold-rate = 20%)](#java-1-gb-warm-80-ms-cold-extra-1500-ms-cold-rate--20)
  - [Compact results table (computed with the same formulas)](#compact-results-table-computed-with-the-same-formulas)
  - [Latency sensitivity (qualitative summary)](#latency-sensitivity-qualitative-summary)
  - [Things that materially change these numbers (i.e., knobs to tune)](#things-that-materially-change-these-numbers-ie-knobs-to-tune)
  - [Recommendations (practical)](#recommendations-practical)
  - [Sources (key references used)](#sources-key-references-used)
  - [References](#references)

---

## TL;DR (one line)

For **I/O-bound** GraphQL BFFs that fan out to many Apigee proxies, **Node** typically costs far less and has much smaller cold-start penalty by default; **Java** can be competitive at high, steady traffic but usually needs **SnapStart / provisioned concurrency / Quarkus/Graal** (extra cost/complexity) to avoid large cold-start latency/cost. ([Amazon Web Services, Inc.][1])

---

## Assumptions (explicit — so you can change any of them)

(keep these in mind; results scale linearly with them)

* **AWS pricing used**

  * Requests: **$0.20 per 1,000,000 requests**. ([Amazon Web Services, Inc.][1])
  * GB-second price (Lambda duration billing): **$0.0000166667 per GB-second**. (source used as a consolidated 2025 guide). ([Castaneda Networks][2])
  * Provisioned concurrency price: **$0.0000033334 per GB-second** (used when we model Java with provisioned concurrency). ([cloudchipr.com][3])
* **Runtime & sizing defaults (reasonable starting points)**

  * Node/NestJS Lambda: **256 MB memory**, average *warm* execution **100 ms**, additional *cold-init* overhead **+200 ms** when cold.
  * Java/Spring Lambda: **1024 MB (1 GB) memory**, average *warm* execution **80 ms**, additional *cold-init* overhead **+1500 ms** when cold (unless SnapStart/provisioned is used).
  * Cold-start rates (fraction of invocations that are served by a cold container): **Node** higher when traffic is very sporadic, but much lower at scale. I used conservative example rates per scenario (detailed below).
* **Scenarios (monthly invocations)**

  * **Low**: 30,000 invocations / month (≈1k/day).
  * **Medium**: 100,000 invocations / month.
  * **High**: 1,000,000 invocations / month.
* **Cold-start rates used in the model (example)**

  * Node: Low=5%, Medium=1%, High=0.1%
  * Java: Low=50%, Medium=20%, High=5% — *unless* you enable provisioned concurrency/SnapStart (we model High+prov for Java).
* **We do not include** data-transfer costs or Apigee costs; those should be added separately. We model only Lambda compute + request + (where applicable) provisioned concurrency charges.

---

## Formulas (and how I computed things)

For each runtime & scenario:

1. invocation_duration_ms_per_invocation = warm_ms + (cold_rate × cold_extra_ms)
2. total_duration_seconds = (invocations × invocation_duration_ms_per_invocation) / 1000
3. GB_seconds = total_duration_seconds × memory_GB
4. Compute_cost = GB_seconds × $per_GB_second
5. Request_cost = (invocations / 1,000,000) × $0.20
6. If using provisioned concurrency (prov): Prov_cost = memory_GB × seconds_in_month × $prov_per_GB_second

Total_cost = Compute_cost + Request_cost + Prov_cost (if any)

I ran the exact arithmetic (digit-by-digit internally) for each cell; below I show a **step-by-step worked example** for the **Medium** scenario (100k/month) so you can see the arithmetic.

---

## Worked example — **Medium** (100,000 invocations / month)

### Node (256 MB, warm 100 ms, cold extra 200 ms, cold-rate = 1%)

1. memory_GB = 256 / 1024 = **0.25 GB**.
2. invocation_duration_ms_per_invocation = 100 + (0.01 × 200) = 100 + 2 = **102 ms**.
3. total_duration_seconds = 100000 × 102 ms / 1000 = 100000 × 0.102 = **10,200 seconds**.
4. GB_seconds = 10,200 s × 0.25 GB = **2,550 GB-seconds**.
5. Compute_cost = 2,550 × $0.0000166667 = **$0.042500085**.

   * Calculation: 2,550 × 0.0000166667 = 2550 × 1.66667e-05 ≈ 0.042500085.
6. Request_cost = (100,000 / 1,000,000) × $0.20 = 0.1 × 0.20 = **$0.02**.
7. Prov_cost = $0 (we're not using provisioned concurrency for Node in this scenario).
   **Total Node cost ≈ $0.042500085 + $0.02 = $0.062500085 (~$0.063 / month).**

---

### Java (1 GB, warm 80 ms, cold extra 1500 ms, cold-rate = 20%)

1. memory_GB = 1024 / 1024 = **1.0 GB**.
2. invocation_duration_ms_per_invocation = 80 + (0.20 × 1500) = 80 + 300 = **380 ms**.
3. total_duration_seconds = 100000 × 380 ms / 1000 = 100000 × 0.38 = **38,000 seconds**.
4. GB_seconds = 38,000 s × 1.0 GB = **38,000 GB-seconds**.
5. Compute_cost = 38,000 × $0.0000166667 = **$0.6333346**.

   * Calculation: 38000 × 0.0000166667 ≈ 0.6333346.
6. Request_cost = (100,000 / 1,000,000) × $0.20 = **$0.02**.
7. Prov_cost = $0 (we are not using provisioned concurrency here).
   **Total Java cost ≈ $0.6333346 + $0.02 = $0.6533346 (~$0.65 / month).**

**Takeaway (Medium):** Node ≈ **$0.06 / month**, Java ≈ **$0.65 / month** — Java is ~10× more expensive in this Medium example because of bigger memory and higher cold-init overhead assumptions. (Exact numbers depend on your real durations, memory sizing, and cold-start rates.)

---

## Compact results table (computed with the same formulas)

(rounded to 3 significant cents for readability)

| Scenario (monthly invocations) | Node total/month |                                         Java total/month (no prov) | Java total/month (with prov/SnapStart for High) |
| -----------------------------: | ---------------: | -----------------------------------------------------------------: | ----------------------------------------------: |
|                  **Low (30k)** |       **$0.020** |                                                         **$0.421** |                                               — |
|              **Medium (100k)** |       **$0.063** |                                                         **$0.653** |                                               — |
|           **High (1,000,000)** |       **$0.618** | **$11.42** *(includes provisioned concurrency cost in this model)* |                                      **$11.42** |

* Details & assumptions: see the worked example for Medium. For **High**, I modeled Java with provisioned concurrency turned on (to avoid massive cold-start latency at scale), which adds **continuous GB-second charges** for the provisioned instances over the month — that is why Java total jumps significantly in High scenario. (Provisioned concurrency was priced and modeled using the additional GB-second rate noted earlier.) ([cloudchipr.com][3])

---

## Latency sensitivity (qualitative summary)

* **Cold-start latency**

  * **Node**: small extra init (tens–hundreds ms typical with optimized bundles). Easier to reduce by slimming bundle, keeping warm concurrency, using Arm64. ([Reddit][4])
  * **Java**: cold starts can be seconds for typical Spring Boot images unless you use **SnapStart / provisioned concurrency / Quarkus/Graal**. SnapStart can reduce cold starts dramatically but requires understanding snapshot semantics and is most useful at scale. ([AWS Documentation][5])
* **Warm latency**

  * Both are fine for I/O-heavy workloads; Node’s non-blocking model is ergonomic for many concurrent outbound calls. Java may be slightly faster per-CPU for heavy transforms but uses more baseline memory.
* **Throughput**

  * At very high sustained throughput, Java (JVM) can be cost-competitive *if* you can amortize cold-start costs (i.e., warm containers, provisioned concurrency, or long-lived containers in containers-based serverless).

---

## Things that materially change these numbers (i.e., knobs to tune)

1. **Memory allocation**: GB-seconds scale linearly with memory. Moving Node from 256 MB → 512 MB doubles compute cost.
2. **Average execution time** per request (your GraphQL resolver code + Apigee latencies). If your resolvers fan out to many Apigee endpoints synchronously, duration grows and compute costs grow linearly.
3. **Cold-start rate** (how often Lambda re-initializes). This depends on traffic pattern, concurrency, and runtime choice. Using **provisioned concurrency** or **SnapStart** reduces cold-start *latency* but may **increase cost** (provisioned is billed continuously). ([AWS Documentation][5])
4. **Runtime architecture (ARM/Graviton)**: switching to Arm64 (Graviton) often improves cost/perf (benchmarks show ~10–30% better price-performance and improved cold starts for many runtimes). Testing on Arm64 is highly recommended. ([Amazon Web Services, Inc.][6])
5. **Gateway memory footprint**: if your Apollo Gateway uses extra memory (some gateways / routers use hundreds of MB), set memory higher; that affects costs significantly. Some gateway implementations (Apollo Router, Apollo Server v2, or alternatives like Apollo Router native) have different footprints — check memory usage in a load test. ([The Guild][7])

---

## Recommendations (practical)

1. **If you must pick one now**: go **Node/NestJS** for fastest time-to-market, smaller Lambda packages, and lower serverless cost for typical BFF patterns. Instrument and measure real durations and memory; re-run the cost model with real numbers. ([apollographql.com][8])
2. **If enterprise policy forces Java**: use **Quarkus/Micronaut** or **Spring + SnapStart** and plan for provisioned concurrency or native images (GraalVM) for acceptable cold-start latency. Budget extra operational work. ([pt.quarkus.io][9])
3. **Test on Arm64 (Graviton)** early — it usually reduces cost and cold-starts. ([Amazon Web Services, Inc.][6])
4. **Measure real fan-out behavior** (how many Apigee calls per GraphQL request, average response times). Use those measured durations (in ms) and memory footprints to re-run this model — that will give *accurate* monthly cost.
5. **Optimizations with biggest ROI**: use per-resolver timeouts, parallelize outbound calls when safe, cache responses at the BFF or Apigee layer, push auth/rate-limit/caching into Apigee where appropriate, and use backpressure/circuit breakers. ([apollographql.com][8])

---

## Sources (key references used)

* AWS Lambda pricing (requests): AWS docs. ([Amazon Web Services, Inc.][1])
* Lambda GB-second price & provisioning guidance (2025 guides): consolidated pricing references. ([Castaneda Networks][2])
* AWS Lambda SnapStart docs (reducing Java cold starts). ([AWS Documentation][5])
* AWS/benchmarks on Arm64 (Graviton) cost/perf benefits. ([Amazon Web Services, Inc.][6])
* Apollo / GraphQL gateway memory & perf notes (gateway implementations vary). ([The Guild][7])

---

## References

- [1] https://aws.amazon.com/lambda/pricing/?utm_source=chatgpt.com "AWS Lambda Pricing"
- [2] https://castanedanetworks.com/blog/aws-lambda-cost-calculator-2025/?utm_source=chatgpt.com "AWS Lambda Cost Calculator 2025: Complete Pricing Guide"
- [3] https://cloudchipr.com/blog/aws-lambda-pricing?utm_source=chatgpt.com "AWS Lambda Pricing Breakdown: Ultimate Guide 2025"
- [4] https://www.reddit.com/r/webdev/comments/1fmjtpt/benchmarking_aws_lambda_cold_starts_across/?utm_source=chatgpt.com "Benchmarking AWS Lambda Cold Starts Across JavaScript ..."
- [5] https://docs.aws.amazon.com/pt_br/lambda/latest/dg/snapstart.html?utm_source=chatgpt.com "Aprimoramento da performance de inicialização com o ..."
- [6] https://aws.amazon.com/blogs/apn/comparing-aws-lambda-arm-vs-x86-performance-cost-and-analysis-2/?utm_source=chatgpt.com "Comparing AWS Lambda Arm vs. x86 Performance, Cost ..."
- [7] https://the-guild.dev/graphql/hive/blog/state-of-graphql-gateways-in-2023?utm_source=chatgpt.com "State of GraphQL Gateways in 2023 | Hive"
- [8] https://www.apollographql.com/docs/apollo-server/using-federation/gateway-performance?utm_source=chatgpt.com "Improving Gateway Performance - Apollo GraphQL Docs"
- [9] https://pt.quarkus.io/guides/aws-lambda-snapstart?utm_source=chatgpt.com "Configuração do AWS Lambda SnapStart"
