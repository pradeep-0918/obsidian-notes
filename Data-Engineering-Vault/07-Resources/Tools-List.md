# 🛠️ Complete Tools Reference
#tools #reference #comparison

> Every tool from the Awesome Data Engineering list, organized and expanded.

---

## 🗄️ Databases — Full Reference

### Relational
| Tool | Best For | Key Feature | When to Choose |
|---|---|---|---|
| PostgreSQL | Production OLTP, complex queries | JSONB, extensions, performance | Default choice for most projects |
| MySQL | Web apps, simple OLTP | Speed on simple queries | Legacy systems, LAMP stack |
| TiDB | Horizontal scale + MySQL compat | Distributed NewSQL | Outgrow single PostgreSQL |
| RQLite | Lightweight distributed SQL | Raft consensus | IoT, edge computing |
| MariaDB | MySQL alternative | Open source, Galera cluster | MySQL migration |
| Amazon RDS | Managed cloud DB | Zero maintenance | AWS-native projects |
| CrateDB | Scalable SQL + NoSQL | Full-text + geospatial | IoT analytics |

### NoSQL
| Tool | Type | Best For |
|---|---|---|
| Redis | Key-Value | Cache, sessions, leaderboards, pub/sub |
| MongoDB | Document | Flexible schemas, nested documents |
| Cassandra | Column-Wide | High write throughput, IoT, time-series |
| ScyllaDB | Column-Wide | Cassandra-compatible but 10x faster |
| HBase | Column-Wide | Hadoop ecosystem, sparse data |
| Elasticsearch | Search+Document | Full-text search, log analytics |
| Neo4j | Graph | Social networks, knowledge graphs |
| ArangoDB | Multi-model | Document + Graph + Key-Value |

### Analytics Databases
| Tool | Best For | Key Feature |
|---|---|---|
| ClickHouse | Real-time analytics | Fastest OLAP queries |
| DuckDB | Local analytics | In-process, no server needed |
| AWS Redshift | AWS data warehouse | Columnar, MPP |
| Snowflake | Cloud DWH | Separation of storage/compute |
| Google BigQuery | Serverless analytics | Pay-per-query |
| QuestDB | Time-series analytics | SQL + time-series functions |

---

## 📥 Data Ingestion Tools

| Tool | Type | Sources | Use Case |
|---|---|---|---|
| Airbyte | EL | 200+ connectors | SaaS → Warehouse |
| Fivetran | EL (managed) | 300+ connectors | Managed, no-code |
| dlt | EL (Python) | Custom | Python-native pipelines |
| Meltano | ELT | Singer taps | CLI-first |
| Kafka | Streaming | Events | Real-time event streaming |
| Debezium | CDC | Databases | Database replication |
| Apache NiFi | ETL | Visual flow | No-code, drag-and-drop |
| Apache Sqoop | Bulk transfer | Hadoop ↔ RDBMS | Legacy Hadoop migrations |
| Embulk | Bulk transfer | Files, DBs | Batch file loads |
| Artie | CDC | Databases | Real-time CDC |

---

## ⚡ Processing Tools

| Tool | Type | Language | Scale | Best For |
|---|---|---|---|---|
| Apache Spark | Batch+Stream | Python/Scala/SQL | PB | Large-scale batch analytics |
| Apache Flink | Stream+Batch | Python/Java/SQL | TB/hr | Stateful streaming |
| Apache Beam | Unified | Python/Java | Any | Multi-runner portability |
| dbt | SQL Transform | SQL+Python | Warehouse | Analytics transformations |
| Pandas | Local batch | Python | GB | Small-scale, exploration |
| Polars | Local batch | Python (Rust) | GB | Fast pandas alternative |

---

## 🎛️ Orchestration Tools

| Tool | Best For | Key Feature | Hosted Option |
|---|---|---|---|
| Apache Airflow | Complex workflows | Python DAGs, extensible | Astronomer, MWAA |
| Prefect | Modern pipelines | Python-first, easy local dev | Prefect Cloud |
| Dagster | Asset-based | Data assets, great testing | Dagster Cloud |
| Kestra | Declarative | YAML-based, event-driven | Kestra Cloud |
| Luigi | Simple pipelines | Lightweight, targets | Self-hosted |
| dbt Core | SQL transforms | SQL models, testing | dbt Cloud |

---

## 📊 Visualization & BI

| Tool | Type | Best For |
|---|---|---|
| Apache Superset | Open source BI | Self-hosted, SQL-first |
| Metabase | Simple BI | Non-technical users |
| Redash | SQL dashboards | Developer-friendly |
| Grafana | Metrics dashboards | Time-series, monitoring |
| Tableau | Enterprise BI | Rich visualizations |
| Looker | Governed BI | LookML semantic layer |
| Power BI | Microsoft ecosystem | Excel users |

---

## 🏔️ Data Lake / Table Formats

| Tool | Type | Best For |
|---|---|---|
| Delta Lake | Table format | Databricks ecosystem |
| Apache Iceberg | Table format | Multi-engine, Netflix-proven |
| Apache Hudi | Table format | Streaming upserts (Uber) |
| lakeFS | Version control | Git for data lakes |
| Project Nessie | Data catalog | Iceberg + Git semantics |

---

*Back to [[00-Index]]*
