# 📊 Case Studies — Real Company Architectures
#interview #casestudies #architecture #real-world

> How top companies actually built their data infrastructure. Study these to understand real trade-offs.

---

## 🏢 Netflix — Data Mesh at Scale

**Problem:** Central data team couldn't keep up with 10,000+ engineers' data needs.

**Solution:** Data Mesh
- Each team owns their data products
- Central platform provides tooling (not data)
- Metacat: unified metadata catalog across Hive, Iceberg, Redshift, S3
- Flink for real-time, Spark for batch

**Key learnings:**
1. Data ownership reduces bottlenecks dramatically
2. Metadata is as important as data itself
3. Self-serve requires excellent documentation

---

## 🏢 Uber — Real-Time Data at Massive Scale

**Stats:** 15 PB data lake, millions of events/second

**Architecture:**
```
Driver/Rider apps → Kafka → Apache Pinot (real-time OLAP)
                         → Hudi + HDFS (data lake)
                         → Flink (stream processing)
```

**Apache Pinot** — Uber's secret weapon:
- Sub-second OLAP queries on real-time data
- Used for: surge pricing, driver supply/demand dashboard
- Ingests directly from Kafka

**Key learnings:**
1. Standard tools (Spark) aren't always fast enough for real-time OLAP
2. Specialized tools (Pinot) for specific use cases
3. At scale, cost optimization is a full-time job

---

## 🏢 Airbnb — Data Quality as a Culture

**Problem:** Analysts didn't trust the data. "Which dashboard is correct?"

**Solution — Dataportal + Minerva:**
- Dataportal: data catalog with ownership, lineage, quality scores
- Minerva: centralized metrics layer (single definition of "revenue")
- Midas: data quality framework with automated testing

**Key learnings:**
1. Data without trust is worthless
2. Metrics definitions must be centralized and governed
3. Data catalog is foundational infrastructure

---

## 🎓 Interview Case Study Questions

**Q: Your daily pipeline starts failing at 3am. Walk me through debugging it.**

```
1. Check Airflow/orchestration for the failed task
   - Which task failed? What error?
   - Has it failed before? What changed?

2. Check logs (CloudWatch, Datadog, etc.)
   - OOM? Connection timeout? Data error?

3. Check upstream dependencies
   - Did the source data arrive?
   - Did another pipeline that feeds this one fail?

4. Check infrastructure
   - Cluster out of disk/memory?
   - Network issues to external services?

5. Triage
   - Can it be retried safely? (idempotent?)
   - Does anyone depend on this data urgently?
   - Manual fix or wait for root cause?

6. Post-mortem
   - Add monitoring to catch this earlier
   - Add alert before it becomes a failure
   - Document the fix
```

**Q: Business says "the numbers don't look right." What do you do?**

```
1. Understand what "wrong" means
   - What specific number? What time period?
   - What were they expecting vs what they see?
   - Is it consistently wrong or just today?

2. Trace the data lineage
   - Source → Bronze → Silver → Gold → Dashboard
   - At which layer does the discrepancy appear?

3. Check recent changes
   - Was there a code deployment recently?
   - Did the source schema change?
   - Did business logic change (new discount rules, etc.)?

4. Validate against source
   - Query the source system directly
   - Compare with warehouse numbers

5. Communicate
   - Update stakeholders on status
   - Don't blame other teams publicly
   - Be clear about when a fix will be available
```

---

*Related: [[06-Interview-Prep/SQL-Questions]] | [[06-Interview-Prep/System-Design]] | Back to [[00-Index]]*
