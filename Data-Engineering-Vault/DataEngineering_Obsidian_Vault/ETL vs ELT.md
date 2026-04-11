# 🔄 ETL vs ELT

> **Level:** Beginner-Intermediate | **Stage:** 2 of 5
> Understanding ETL vs ELT is fundamental — it determines *where and when* data transformation happens in your pipeline.

---

## 🤔 What's the Difference?

| | ETL | ELT |
|-|-----|-----|
| **Stands for** | Extract → Transform → Load | Extract → Load → Transform |
| **Transform happens** | Before loading | After loading |
| **Where transform runs** | Separate server/tool | Inside the data warehouse |
| **Best for** | Legacy systems, sensitive data | Modern cloud warehouses |
| **Speed** | Slower to set up | Faster iteration |

---

## 🧠 ETL — Extract, Transform, Load

### How it works:
```
[Source DB] → Extract → [Transform in Python/Spark] → Load → [Data Warehouse]
```

1. **Extract:** Pull raw data from sources (APIs, databases, files)
2. **Transform:** Clean, reshape, enrich data *before* it hits the warehouse
3. **Load:** Push clean data into the target database

### When to use ETL:
- Data needs to be cleaned/masked *before* it enters the warehouse (privacy/compliance)
- Source data is very messy and needs heavy processing
- Working with legacy systems

### Example:
```python
# Extract
df = pd.read_csv("raw_sales.csv")

# Transform
df.drop_duplicates(inplace=True)
df["revenue"] = df["quantity"] * df["unit_price"]
df = df[df["revenue"] > 0]

# Load
df.to_sql("clean_sales", engine, if_exists="replace", index=False)
```

---

## 🧠 ELT — Extract, Load, Transform

### How it works:
```
[Source DB] → Extract → Load Raw → [Data Warehouse] → Transform with SQL/dbt
```

1. **Extract:** Pull raw data from sources
2. **Load:** Load raw data *as-is* into the warehouse (cheap storage!)
3. **Transform:** Use SQL or dbt inside the warehouse to build clean tables

### When to use ELT:
- Using a modern cloud warehouse (Snowflake, BigQuery, Redshift)
- You want to preserve raw data for re-processing
- Transformations change frequently (just re-run SQL)

### Example (using dbt):
```sql
-- models/clean_sales.sql (dbt model)
SELECT
  order_id,
  customer_id,
  quantity * unit_price AS revenue,
  order_date
FROM {{ source('raw', 'sales') }}
WHERE quantity > 0
```

---

## 🏭 Real-World Tools

### ETL Tools:
| Tool | Type |
|------|------|
| Apache Spark | Code-based (Python/Scala) |
| Talend | GUI-based |
| Informatica | Enterprise GUI |
| AWS Glue | Managed Spark (cloud) |

### ELT Tools:
| Tool | Type |
|------|------|
| dbt (data build tool) | SQL-based transformations |
| Fivetran / Airbyte | Managed EL (Extract + Load) |
| Snowflake / BigQuery | Cloud warehouse with built-in compute |

---

## 🔁 Modern Pattern: ELT is Winning

> With cheap cloud storage and powerful warehouses, **ELT is the modern default**.
> Raw data lands in S3 → Loaded into Snowflake → dbt transforms it into models.

```
S3 (raw zone) → Fivetran → Snowflake (raw schema) → dbt → Snowflake (analytics schema)
```

---

## ✅ Mini Practice Tasks

- [ ] Write a Python ETL script: read CSV → clean → load to SQLite
- [ ] Set up a basic dbt project with one model (ELT approach)
- [ ] Compare: what would change if you switched your pipeline from ETL to ELT?
- [ ] List 3 real-world scenarios where ETL is safer than ELT

---

## 🔗 Related Notes

- [[Python for Data Engineering]] — Used in the Extract and Transform steps
- [[SQL for Data Engineering]] — Core language for the Transform step in ELT
- [[Data Pipelines]] — ETL/ELT is the core pattern inside every pipeline
- [[Data Warehousing]] — The destination in both approaches
- [[Data-Engineering-Vault/DataEngineering_Obsidian_Vault/Airflow]] — Orchestrates the steps in ETL/ELT workflows

---

## ➡️ Next Step

[[Data Pipelines]] — Learn how to build, schedule, and monitor the systems that run ETL/ELT automatically.
