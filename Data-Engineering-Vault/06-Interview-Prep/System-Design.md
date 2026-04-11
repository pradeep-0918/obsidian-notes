# 🏗️ System Design Interview — Data Engineering Guide
#interview #system-design #architecture

> How to design data systems in 45 minutes. The framework + real examples.

---

## 🎯 The 45-Minute Framework

```
Minutes  0-5:   Clarify requirements
Minutes  5-10:  Estimate scale
Minutes 10-20:  High-level design
Minutes 20-35:  Deep dive on critical components
Minutes 35-45:  Discuss trade-offs & improvements
```

---

## 📋 Requirements Clarification Template

Always ask these before designing:

**Functional Requirements:**
- What data sources? (databases, APIs, files, events)
- What queries need to be supported?
- What is the acceptable data latency?
- Who are the consumers? (BI, ML, other services)

**Non-Functional Requirements:**
- What is the data volume? (GB/day, events/second)
- What is the SLA? (99.9%? 99.99%?)
- What is the retention period?
- Budget constraints?
- Compliance? (GDPR, HIPAA)

---

## 📐 Scale Estimation

```
Quick math for system design:

Events per day → Events per second:
1M events/day = ~12 events/sec
100M events/day = ~1,200 events/sec
1B events/day = ~12,000 events/sec

Storage estimation:
1 event = 1KB average
1M events/day = 1GB/day = 365GB/year
1B events/day = 1TB/day = 365TB/year

Network:
1Gbps = 125 MB/s
10Gbps = 1.25 GB/s

Memory:
32GB RAM can hold ~30GB compressed Parquet data
```

---

## 🎯 Example: Design a URL Shortener Analytics Pipeline

**Question:** "Design a system to track and analyze clicks on shortened URLs in real-time."

**Step 1: Requirements**
```
Functional:
- Track every click (url, user_agent, referrer, country, timestamp)
- Real-time dashboard: clicks/minute per URL
- Historical analysis: daily/weekly reports
- Top URLs by clicks, geographic distribution

Non-functional:
- 10,000 clicks/second peak
- Dashboard latency < 5 seconds
- Historical queries < 2 seconds
- 2 years retention
```

**Step 2: Scale**
```
10K events/sec = ~1KB/event = 10MB/sec
Daily: 10MB/sec * 86400 = ~864GB/day
After Parquet compression (~10:1): ~86GB/day
2 years: ~63TB total
```

**Step 3: Architecture**
```
┌──────────────────────────────────────────────────────────────────────┐
│                     URL ANALYTICS PIPELINE                            │
│                                                                      │
│  Click Event ──► Load Balancer ──► API Gateway ──► Kafka            │
│  (from redirect)                                    Topic: clicks    │
│                                                          │           │
│                           ┌──────────────────────────────┤           │
│                           ▼                              ▼           │
│                    Flink (real-time)           Spark (batch)         │
│                    - 1-min windows            - Hourly Spark job     │
│                    - Count per URL            - Aggregate by day     │
│                    - Top URLs                 - Geographic rollups   │
│                           │                              │           │
│                           ▼                              ▼           │
│                       Redis                        Delta Lake         │
│                    (live dashboard)             (historical store)   │
│                           │                              │           │
│                           ▼                              ▼           │
│                    Dashboard API              Presto/Athena          │
│                    (WebSocket)                (ad-hoc queries)       │
└──────────────────────────────────────────────────────────────────────┘
```

**Step 4: Key Design Decisions**

| Decision | Choice | Why |
|---|---|---|
| Message queue | Kafka | Durable, replayable, handles 10K/sec easily |
| Real-time processing | Flink | <5s latency requirement, stateful aggregation |
| Real-time storage | Redis | Sub-millisecond reads for dashboard |
| Historical storage | Delta Lake on S3 | ACID + cheap storage |
| Historical queries | Presto/Athena | Serverless, pay-per-query |
| Batch processing | Spark | Standard for large-scale batch |

**Step 5: Trade-offs to Discuss**
- "Why not just use a single streaming system?"
  - Real-time + batch = different SLAs, different cost profiles
- "Why not just store in a database?"
  - 86GB/day would be very expensive in a relational DB
- "How do you handle late data?"
  - Watermarks in Flink + re-processing window in Spark

---

## 🎯 Example: Design a Data Warehouse for E-Commerce

**Question:** "Design a data warehouse for a mid-size e-commerce company."

**Answer framework:**
```
Sources: PostgreSQL (orders, customers), Stripe (payments),
         Shopify (inventory), Salesforce (CRM), GA4 (web events)

Ingestion:
- PostgreSQL → Debezium CDC → Kafka → Bronze Layer
- Stripe/Shopify/Salesforce → Fivetran → Raw schema
- GA4 → BigQuery export → S3

Storage:
- Raw/Bronze: S3 Parquet (as-is from source)
- Silver: Delta Lake (cleaned, typed, deduplicated)
- Gold: Snowflake (star schema for BI)

Transformation: dbt (SQL models, tested, documented)

Serving:
- BI: Looker/Metabase connected to Snowflake
- Self-serve: Superset
- APIs: Snowflake → dbt metrics layer → FastAPI

Orchestration: Airflow (batch) + Kafka (streaming events)
```

---

## 🎯 Example: Design a Real-Time Fraud Detection System

**See [[04-System-Design/Real-Time-Architecture]] for detailed implementation**

**Key talking points:**
1. Sub-500ms latency requires in-memory processing (Redis, Flink state)
2. Feature freshness trade-off (real-time vs batch features)
3. Model serving: sidecar microservice vs embedded in Flink
4. False positive rate: business decision, not technical
5. Feedback loop: how fraud labels come back to improve the model

---

## 💬 Behavioral Questions

**"Tell me about a pipeline you designed from scratch"**
Structure: STAR (Situation, Task, Action, Result)
```
Situation: Company had no historical data — analysts queried prod DB
Task: Build first data warehouse in 3 months
Action: 
  - Chose Airflow + Spark + Snowflake + dbt
  - Built Bronze/Silver/Gold architecture
  - Implemented data quality checks with Great Expectations
  - Trained analysts on new data model
Result:
  - Prod DB queries dropped 90%
  - Analyst time on data prep: 80% → 20%
  - New dashboards deployed in days, not weeks
```

**"Tell me about a time a pipeline failed in production"**
```
Situation: Daily pipeline failed silently — wrong data in warehouse for 3 days
Root cause: Schema change in source without coordination
How found: Analyst noticed numbers were off
Fix: Added schema validation checks at ingestion
Prevention: Set up Slack alerts on schema changes, weekly data quality review
```

---

*Related: [[06-Interview-Prep/SQL-Questions]] | [[06-Interview-Prep/Case-Studies]] | Back to [[00-Index]]*
