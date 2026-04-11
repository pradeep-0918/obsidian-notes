# 🏔️ Data Lake Architecture
#design #datalake #lakehouse #architecture

> Store everything, transform later. The foundation of modern data platforms.

---

## Data Lake vs Data Warehouse vs Lakehouse

```
DATA WAREHOUSE              DATA LAKE                LAKEHOUSE
─────────────────────       ─────────────────────    ─────────────────────
Schema-on-write             Schema-on-read           Best of both
Structured data only        All data types           All data types
Fast queries                Slow queries             Fast queries
Expensive storage           Cheap storage            Cheap storage
Limited flexibility         High flexibility         High flexibility
e.g., Redshift, Snowflake  e.g., S3 + Hive         e.g., Delta Lake, Iceberg
```

## Lakehouse Architecture

```
┌──────────────────────────────────────────────────────────────┐
│                       LAKEHOUSE                               │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐   │
│  │                  METADATA LAYER                       │   │
│  │  Apache Iceberg / Delta Lake / Apache Hudi           │   │
│  │  - ACID transactions                                 │   │
│  │  - Schema evolution                                  │   │
│  │  - Time travel                                       │   │
│  │  - Partition evolution                               │   │
│  └──────────────────────────────────────────────────────┘   │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐   │
│  │              FILE STORAGE LAYER                       │   │
│  │  S3 / ADLS / GCS                                    │   │
│  │  Parquet files (columnar, compressed)               │   │
│  └──────────────────────────────────────────────────────┘   │
│                                                              │
│  Query engines: Spark | Trino/Presto | Athena | Flink      │
└──────────────────────────────────────────────────────────────┘
```

## Table Format Comparison

| Feature | Delta Lake | Apache Iceberg | Apache Hudi |
|---|---|---|---|
| ACID | Yes | Yes | Yes |
| Time travel | Yes | Yes | Yes |
| Schema evolution | Yes | Yes | Yes |
| Partition evolution | Limited | Full | Limited |
| Engine support | Spark-first | Multi-engine | Spark-first |
| Ecosystem | Databricks | Broad | Uber-originated |
| Best for | Databricks shops | Multi-engine | Streaming upserts |

---

*Related: [[04-System-Design/ETL-Pipelines]] | [[02-Core-Concepts/File-Systems]] | Back to [[00-Index]]*
