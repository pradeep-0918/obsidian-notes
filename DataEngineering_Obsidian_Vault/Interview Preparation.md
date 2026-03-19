# 🎯 Interview Preparation

> **Level:** All Levels | **Stage:** 5 of 5
> Data Engineering interviews test SQL, Python, system design, and your understanding of the data stack. This note is your complete preparation guide.

---

## 🗂️ Interview Types You'll Face

| Round | What's Tested |
|-------|--------------|
| **Recruiter Screen** | Background, experience, basic DE concepts |
| **SQL Round** | Window functions, CTEs, aggregations, optimization |
| **Python / Coding Round** | Data processing, OOP, algorithms |
| **System Design Round** | Design a pipeline, data model, or warehouse |
| **Conceptual / Behavioral** | "Tell me about a pipeline you built" |
| **Take-home / Case Study** | Build a mini pipeline or model |

---

## 🧠 Top SQL Interview Questions

### Window Functions (Always Asked)
```sql
-- Q: Rank employees by salary within each department
SELECT name, department, salary,
  RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS rank
FROM employees;

-- Q: Find the 2nd highest salary in each department
WITH ranked AS (
  SELECT *, DENSE_RANK() OVER (PARTITION BY dept ORDER BY salary DESC) AS rk
  FROM employees
)
SELECT * FROM ranked WHERE rk = 2;

-- Q: Calculate 7-day rolling average
SELECT date, amount,
  AVG(amount) OVER (ORDER BY date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) AS rolling_avg
FROM sales;
```

### Common Patterns
```sql
-- Deduplicate (keep latest record per user)
WITH deduped AS (
  SELECT *, ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY updated_at DESC) AS rn
  FROM users
)
SELECT * FROM deduped WHERE rn = 1;

-- Year-over-year growth
SELECT year,
  revenue,
  LAG(revenue) OVER (ORDER BY year) AS prev_year,
  ROUND((revenue - LAG(revenue) OVER (ORDER BY year)) / LAG(revenue) OVER (ORDER BY year) * 100, 2) AS yoy_growth
FROM annual_revenue;
```

---

## 🐍 Top Python Interview Questions

### Data Processing
```python
# Q: Find duplicates in a list
from collections import Counter
dupes = [k for k, v in Counter(my_list).items() if v > 1]

# Q: Flatten a nested list
flat = [item for sublist in nested for item in sublist]

# Q: Read large CSV in chunks
for chunk in pd.read_csv("large.csv", chunksize=10000):
    process(chunk)
```

### Conceptual Questions & Answers

**Q: What is a generator and when would you use one in DE?**
> A: Generators yield one item at a time, using less memory. Use for processing large files line by line without loading everything into RAM.

**Q: How do you handle failures in a pipeline?**
> A: Try/except blocks, logging, retries with exponential backoff, dead-letter queues for failed events, idempotent operations so re-runs are safe.

**Q: What's the difference between `==` and `is` in Python?**
> A: `==` checks value equality; `is` checks identity (same object in memory).

---

## 🏗️ System Design Interview

### Common Prompts:
- "Design a pipeline that ingests 10TB of daily clickstream data"
- "Design a data warehouse for an e-commerce company"
- "How would you build a real-time fraud detection system?"

### Framework to Answer:
```
1. CLARIFY requirements
   - How many records/day?
   - Batch or real-time?
   - Who are the consumers (analysts, ML, dashboards)?
   - SLA (how fresh does the data need to be)?

2. HIGH-LEVEL ARCHITECTURE
   - Source → Ingestion → Storage → Processing → Serving

3. TOOL CHOICES + JUSTIFICATION
   - "I'd use Kafka because... vs Kinesis because..."

4. TRADE-OFFS
   - "This approach optimizes for cost over latency"

5. FAILURE HANDLING
   - "If the Spark job fails at step 3, it can be re-run because..."
```

### Example Answer Sketch:
**"Design a daily sales reporting pipeline"**
```
Sources: Postgres (orders) + Stripe API (payments)
Ingestion: Fivetran → S3 (daily snapshot)
Transform: dbt on Snowflake (staging → marts)
Orchestration: Airflow (6 AM daily trigger)
Serving: Tableau dashboard
Monitoring: dbt tests + Airflow alerts → Slack
Failure recovery: Idempotent dbt models, Airflow retries
```

---

## 💬 Behavioral Questions (STAR Method)

**S**ituation · **T**ask · **A**ction · **R**esult

| Question | How to Answer |
|----------|--------------|
| "Tell me about a pipeline you built" | Use Project 1 or 2 from [[Projects for Data Engineering]] |
| "Describe a time data quality was an issue" | How you detected, fixed, and prevented it |
| "How do you handle ambiguous requirements?" | Ask clarifying questions, prototype, iterate |
| "Tell me about a failure" | Own it, explain what you learned |

---

## 📋 Key Concepts to Know Cold

### Fundamentals
- [ ] ACID properties (Atomicity, Consistency, Isolation, Durability)
- [ ] CAP theorem
- [ ] Idempotency and why it matters
- [ ] Batch vs streaming — when to use which
- [ ] Star schema vs snowflake schema
- [ ] Normalization (1NF, 2NF, 3NF)
- [ ] Slowly Changing Dimensions (Type 1, 2, 3)

### Tools
- [ ] What is dbt and how does it work?
- [ ] How does Airflow schedule DAGs?
- [ ] What is partitioning and why does it improve performance?
- [ ] How does Kafka guarantee message ordering?
- [ ] What is the difference between `map` and `flatMap` in Spark?
- [ ] What is broadcast join in Spark?

### SQL Must-Haves
- [ ] Window functions: `RANK`, `DENSE_RANK`, `ROW_NUMBER`, `LAG`, `LEAD`
- [ ] `GROUP BY` vs `PARTITION BY`
- [ ] CTEs vs subqueries vs temp tables — trade-offs
- [ ] Index types and when to use them
- [ ] `EXPLAIN ANALYZE` — how to read a query plan

---

## 📚 Resources to Use

| Resource | What For |
|----------|---------|
| LeetCode (Easy/Medium SQL) | SQL practice |
| StrataScratch | DE-specific SQL problems |
| DataLemur | SQL + DE interview questions |
| Designing Data-Intensive Applications (book) | System design deep dive |
| GitHub (your projects!) | Portfolio and talking points |

---

## ✅ Interview Prep Checklist

- [ ] Solve 30+ SQL problems on LeetCode/StrataScratch
- [ ] Practice explaining a project in 2 minutes (elevator pitch)
- [ ] Do 2 mock system design interviews with a friend
- [ ] Review all [[Data Engineering Roadmap]] topics
- [ ] Prepare 3 STAR stories from your projects
- [ ] Know your resume cold — every line should be explainable
- [ ] Research the company's data stack before the interview

---

## 🔗 Related Notes

- [[Data Engineering Roadmap]] — Full learning path overview
- [[SQL for Data Engineering]] — Most-tested technical skill
- [[Python for Data Engineering]] — Second most-tested
- [[Data Modeling]] — System design interview core
- [[Projects for Data Engineering]] — Your interview talking points

---

## ➡️ You're Ready!

Return to [[Data Engineering Roadmap]] and make sure you've completed all stages. Then start applying — you've got this! 🎉
