# 14 — GenAI & RAG Pipelines for Data Engineers
#rag #llm #embeddings #vector-db #langchain #chunking #evaluation

**Roadmap Days:** 25–26 | [[Data-Engineering-Vault/DE_Vault/00_Master_Index]] | Prev → [[11_14_Delta_Kafka_Flink]] | Next → [[15_System_Design_DE]]

---

## 🗺️ Topic Map

```
GenAI Data Engineering
├── LLM API Basics (OpenAI, Anthropic, tokens, cost)
├── Structured Output (JSON mode, function calling, Instructor)
├── Chunking Strategies
├── Embedding Models
├── Vector Databases (Qdrant, Chroma, Pinecone)
├── Similarity Search (cosine, HNSW)
├── Hybrid Search (dense + sparse BM25)
├── Re-ranking
├── RAG Pipeline (LangChain)
├── Async Batch Embedding
├── RAG Evaluation (Ragas)
└── Airflow DAG for Document Ingestion
```

---

## 1. LLM API Basics

```python
import anthropic

client = anthropic.Anthropic()

message = client.messages.create(
    model="claude-opus-4-5",
    max_tokens=1024,
    messages=[
        {"role": "user", "content": "Extract entities from this text: ..."}
    ]
)
print(message.content[0].text)

# Token cost calculation
# ~1 token ≈ 4 chars ≈ 0.75 words
# Always estimate: input_tokens * input_price + output_tokens * output_price
```

> 💡 **Rate limits:** Most LLM APIs have tokens-per-minute (TPM) limits. Use `asyncio.Semaphore` + exponential backoff for batch processing.

---

## 2. Chunking Strategies

| Strategy | How | Best For | Trade-offs |
|----------|-----|---------|-----------|
| **Fixed-size** | Split every N chars | Simple, fast | Splits mid-sentence |
| **Recursive character** | Split on `\n\n`, `\n`, `. `, space | Good general purpose | Still context-unaware |
| **Semantic** | Use embedding similarity to find natural break points | Best context preservation | Slow, expensive |
| **Document structure** | Split on headers (Markdown/HTML) | Structured docs | Requires clean structure |
| **Sentence** | NLTK/spaCy sentence boundaries | NLP tasks | Variable chunk size |

```python
from langchain.text_splitter import RecursiveCharacterTextSplitter

splitter = RecursiveCharacterTextSplitter(
    chunk_size=512,
    chunk_overlap=50,        # overlap for context continuity
    separators=["\n\n", "\n", ". ", " ", ""],
)
chunks = splitter.split_text(document_text)
```

> 💡 **Chunk overlap:** Prevents context being split at boundaries. ~10% of chunk_size is a good starting point. Too much overlap = redundant embeddings + storage cost.

> 💡 **Chunk size vs retrieval:** Smaller chunks (128-256) → more precise retrieval, less context. Larger (512-1024) → more context, less precise. Test with your domain.

---

## 3. Embedding Models

| Model | Dims | Context | Best For |
|-------|------|---------|---------|
| `text-embedding-3-small` (OpenAI) | 1536 | 8191 tokens | General, cheap |
| `text-embedding-3-large` (OpenAI) | 3072 | 8191 tokens | High accuracy |
| `bge-m3` (BAAI) | 1024 | 8192 tokens | Multilingual, local |
| `nomic-embed-text` | 768 | 8192 tokens | Open source, local |
| `all-MiniLM-L6-v2` | 384 | 256 tokens | Fast, local, small docs |

```python
from openai import OpenAI

client = OpenAI()

def embed_texts(texts: list[str], model="text-embedding-3-small") -> list[list[float]]:
    response = client.embeddings.create(input=texts, model=model)
    return [item.embedding for item in response.data]

# Batch embedding (send multiple texts in one call — much more efficient)
embeddings = embed_texts(chunks)  # up to 2048 inputs per call
```

---

## 4. Vector Databases

```python
# Qdrant
from qdrant_client import QdrantClient
from qdrant_client.models import VectorParams, Distance, PointStruct

client = QdrantClient(url="http://localhost:6333")

# Create collection
client.create_collection(
    collection_name="documents",
    vectors_config=VectorParams(size=1536, distance=Distance.COSINE)
)

# Upsert vectors with metadata
client.upsert(
    collection_name="documents",
    points=[
        PointStruct(
            id=i,
            vector=embedding,
            payload={"text": chunk, "source": filename, "page": page_num}
        )
        for i, (embedding, chunk) in enumerate(zip(embeddings, chunks))
    ]
)

# Search
results = client.search(
    collection_name="documents",
    query_vector=query_embedding,
    limit=5,
    query_filter={"must": [{"key": "source", "match": {"value": "policy.pdf"}}]}
)
```

### HNSW Index
- Hierarchical Navigable Small World — graph-based approximate nearest neighbor
- Build time: O(n log n), Search time: O(log n)
- Parameters: `m` (connections per node, higher = better recall, more memory), `ef_construction` (build quality)

---

## 5. Hybrid Search

```python
# Dense (semantic) + Sparse (BM25 keyword) search
from qdrant_client.models import SparseVector, SparseIndexParams

# Qdrant supports hybrid natively
results = client.query_points(
    collection_name="documents",
    prefetch=[
        models.Prefetch(query=dense_embedding, using="dense", limit=20),
        models.Prefetch(query=sparse_vector, using="sparse", limit=20),
    ],
    query=models.FusionQuery(fusion=models.Fusion.RRF),  # Reciprocal Rank Fusion
    limit=5,
)
```

> 💡 **When hybrid wins:** Dense = semantic similarity ("what are the side effects of aspirin" → finds "adverse reactions of acetylsalicylic acid"). Sparse = keyword matching (product codes, names, exact phrases). Hybrid handles both.

---

## 6. Re-ranking

```python
from sentence_transformers import CrossEncoder

reranker = CrossEncoder("cross-encoder/ms-marco-MiniLM-L-6-v2")

# After initial retrieval (e.g., top-20 from vector search)
query = "what is the refund policy?"
candidates = [r.payload["text"] for r in initial_results]

# Score each (query, document) pair
scores = reranker.predict([(query, doc) for doc in candidates])

# Re-rank by score
reranked = sorted(zip(candidates, scores), key=lambda x: x[1], reverse=True)
top_5 = [doc for doc, _ in reranked[:5]]
```

> 💡 **Two-stage retrieval:** Stage 1 = fast approximate ANN (retrieve top-100). Stage 2 = slow but accurate cross-encoder re-rank (get top-5). Best accuracy vs speed trade-off.

---

## 7. RAG Pipeline

```python
from langchain.chains import RetrievalQA
from langchain_openai import ChatOpenAI, OpenAIEmbeddings
from langchain_community.vectorstores import Qdrant

# Setup
embeddings = OpenAIEmbeddings(model="text-embedding-3-small")
vectorstore = Qdrant(client=qdrant_client, collection_name="documents", embeddings=embeddings)
retriever = vectorstore.as_retriever(search_kwargs={"k": 5})
llm = ChatOpenAI(model="gpt-4o-mini", temperature=0)

# Chain
qa_chain = RetrievalQA.from_chain_type(
    llm=llm,
    chain_type="stuff",              # or "map_reduce" for many docs
    retriever=retriever,
    return_source_documents=True,
)

result = qa_chain.invoke({"query": "What is the return policy?"})
answer = result["result"]
sources = result["source_documents"]
```

---

## 8. RAG Evaluation (Ragas)

```python
from ragas import evaluate
from ragas.metrics import faithfulness, answer_relevancy, context_recall

dataset = {
    "question": questions,
    "answer": generated_answers,
    "contexts": retrieved_contexts,
    "ground_truth": reference_answers,   # for context_recall
}

scores = evaluate(dataset, metrics=[faithfulness, answer_relevancy, context_recall])
```

| Metric | Measures | Formula |
|--------|---------|---------|
| **Faithfulness** | Is answer grounded in retrieved context? | % answer claims supported by context |
| **Answer Relevancy** | Is answer relevant to question? | Semantic similarity of answer to question |
| **Context Recall** | Does retrieved context cover ground truth? | % ground truth covered by context |
| **Context Precision** | Are retrieved chunks useful? | % retrieved chunks relevant to question |

---

## 9. Airflow DAG for Document Ingestion

```python
from airflow.decorators import dag, task
from datetime import datetime

@dag(schedule_interval="@daily", start_date=datetime(2024, 1, 1))
def document_ingestion_pipeline():

    @task
    def fetch_new_documents() -> list[dict]:
        # Poll S3, SharePoint, web scraper, etc.
        return list_new_documents(since=yesterday())

    @task
    def chunk_documents(docs: list[dict]) -> list[dict]:
        return [chunk for doc in docs for chunk in split_document(doc)]

    @task
    def embed_chunks(chunks: list[dict]) -> list[dict]:
        # Async batch embedding with rate limiting
        return asyncio.run(batch_embed(chunks, concurrency=10))

    @task
    def upsert_to_vectordb(embedded_chunks: list[dict]) -> None:
        upsert_qdrant(embedded_chunks)

    docs = fetch_new_documents()
    chunks = chunk_documents(docs)
    embedded = embed_chunks(chunks)
    upsert_to_vectordb(embedded)

dag_instance = document_ingestion_pipeline()
```

---

## 🃏 Interview Cheatsheet
- **Why chunking overlap?** Prevents context from being cut at boundaries — queries about content near a split find relevant context in overlapping chunks.
- **HNSW?** Graph-based approximate nearest neighbor index. O(log n) search. Trade-off: recall vs speed via `ef` parameter.
- **Hybrid search benefit?** Dense handles semantic similarity; sparse handles keyword/exact matching. Fusion (RRF) combines both rankings.
- **Faithfulness vs Relevancy?** Faithfulness: answer grounded in context (no hallucination). Relevancy: answer addresses the question.
- **Re-ranking?** Cross-encoder scores (query, doc) pairs — much more accurate than bi-encoder but slower. Use as 2nd stage after fast ANN retrieval.

---
---

# 15 — System Design for Data Engineers
#system-design #architecture #lakehouse #streaming #fraud-detection

**Roadmap Day:** 30 | [[Data-Engineering-Vault/DE_Vault/00_Master_Index]] | Prev → [[14_GenAI_RAG_Pipelines]] | Next → [[16_Interview_Prep]]

---

## 1. Design: 1TB/Day Batch Pipeline → Analytics Dashboard

```
Raw Sources (APIs, DBs)
        ↓
  Ingestion Layer
  S3 raw/ (partitioned: year/month/day)
        ↓
  Transform Layer
  AWS Glue / Spark (EMR)
  → S3 processed/ (Parquet, partitioned)
        ↓
  Serving Layer
  Snowflake / Redshift / BigQuery
  dbt models (staging → intermediate → marts)
        ↓
  BI Layer
  Tableau / Looker / Metabase
```

**Key Design Decisions:**
- Partition by date for pruning
- Use columnar Parquet throughout
- dbt incremental models for daily refresh
- Materialized views for dashboard queries
- Row-level access control in DWH

---

## 2. Design: Real-Time Fraud Detection (Kafka + Flink + Feature Store)

```
Transaction Events
        ↓
  Kafka (transactions topic)
        ↓
  Flink (real-time features)
  - velocity: count(transactions, user, 1hr)
  - geo-anomaly: distance from last transaction
  - amount percentile vs user history
        ↓
  Feature Store (Redis/Feast)
        ↓
  ML Model Serving (FastAPI)
  - < 50ms latency
        ↓
  Kafka (fraud-alerts topic)
        ↓
  Action Service (block card, alert user)
```

**Key Design Decisions:**
- Event time processing with Flink watermarks
- Feature store for real-time + batch feature consistency
- RocksDB state for user velocity counters
- Redis for sub-millisecond feature lookups

---

## 3. Design: Data Lakehouse from Scratch on AWS

```
┌──────────────────────────────────────────────────────┐
│                    INGESTION                          │
│  Firehose (streaming) │ Glue/Spark (batch) │ DMS(CDC)│
└──────────────────┬───────────────────────────────────┘
                   ↓
┌──────────────────────────────────────────────────────┐
│               STORAGE LAYERS (S3 + Delta)             │
│  Bronze (raw): s3://lake/bronze/                      │
│  Silver (clean): s3://lake/silver/ (Delta, partitioned)│
│  Gold (mart): s3://lake/gold/ (Delta, OPTIMIZE'd)     │
└──────────────────┬───────────────────────────────────┘
                   ↓
┌──────────────────────────────────────────────────────┐
│                 CATALOG & GOVERNANCE                  │
│  AWS Glue Data Catalog │ Lake Formation (security)    │
└──────────────────┬───────────────────────────────────┘
                   ↓
┌──────────────────────────────────────────────────────┐
│                   COMPUTE                             │
│  Databricks / EMR (Spark) │ Athena (ad-hoc SQL)      │
│  dbt (transformations)                                │
└──────────────────┬───────────────────────────────────┘
                   ↓
┌──────────────────────────────────────────────────────┐
│                    SERVING                            │
│  Redshift (BI) │ FastAPI (app) │ SageMaker (ML)      │
└──────────────────────────────────────────────────────┘
```

---

## 4. Design: RAG System for 10M Document Corpus

```
Ingestion Pipeline (Airflow):
  Document Store (S3) → Chunker → Embedder (batch async) → Vector DB (Qdrant)

Serving (FastAPI):
  User Query
      ↓
  Query Embedding (text-embedding-3-small)
      ↓
  Hybrid Search (dense + BM25) → top-100 candidates
      ↓
  Cross-encoder Reranker → top-5
      ↓
  LLM (with retrieved context) → answer + sources
      ↓
  Response + Source Documents
```

**Scaling 10M docs:**
- Qdrant: horizontal sharding across nodes
- Embedding pipeline: async batching, GPU acceleration
- Caching: cache embeddings for repeat queries (Redis)
- Collection segmentation: separate collections by domain/tenant

---

## 🃏 System Design Framework
1. **Clarify requirements:** Volume, velocity, latency SLA, consistency, users
2. **Identify data flows:** Source → Store → Process → Serve
3. **Choose components:** Match volume/latency/cost to tool
4. **Handle failures:** Dead letter queues, retries, idempotency
5. **Monitor:** Lag, row counts, data freshness, error rates
6. **Scale:** Identify bottlenecks, horizontal scaling strategy

---
---

# 16 — Interview Prep Reference
#interview #sql #spark #kafka #dbt #system-design

**[[Data-Engineering-Vault/DE_Vault/00_Master_Index]]** | Prev → [[15_System_Design_DE]]

---

## SQL Write-From-Memory Patterns

```sql
-- 1. Running total
SELECT date, revenue,
  SUM(revenue) OVER (ORDER BY date ROWS UNBOUNDED PRECEDING) AS running_total
FROM daily_revenue;

-- 2. Top-N per group
SELECT * FROM (
  SELECT *, ROW_NUMBER() OVER (PARTITION BY category ORDER BY revenue DESC) AS rn
  FROM sales
) WHERE rn <= 3;

-- 3. Gap detection
SELECT id + 1 AS gap_start, next_id - 1 AS gap_end
FROM (SELECT id, LEAD(id) OVER (ORDER BY id) AS next_id FROM orders) t
WHERE next_id > id + 1;

-- 4. Recursive CTE (org hierarchy)
WITH RECURSIVE org AS (
  SELECT id, name, manager_id, 0 AS depth FROM employees WHERE manager_id IS NULL
  UNION ALL
  SELECT e.id, e.name, e.manager_id, o.depth + 1
  FROM employees e JOIN org o ON e.manager_id = o.id
)
SELECT * FROM org ORDER BY depth;

-- 5. SCD Type 2 MERGE
MERGE INTO dim_customers AS t
USING staging AS s ON t.customer_id = s.customer_id AND t.is_current = TRUE
WHEN MATCHED AND (t.email != s.email) THEN
  UPDATE SET valid_to = CURRENT_DATE - 1, is_current = FALSE
WHEN NOT MATCHED THEN
  INSERT (customer_id, email, valid_from, valid_to, is_current)
  VALUES (s.customer_id, s.email, CURRENT_DATE, '9999-12-31', TRUE);
```

---

## Spark Quick Answers

| Question | Answer |
|----------|--------|
| What triggers a shuffle? | `groupBy`, `join` (non-broadcast), `orderBy`, `distinct`, `repartition` |
| Broadcast join threshold? | Default 10MB; `spark.sql.autoBroadcastJoinThreshold` |
| `cache()` vs `persist(DISK_ONLY)`? | cache = MEMORY_AND_DISK. DISK_ONLY = never in memory |
| What does AQE do? | Auto coalesces partitions, handles skew joins, upgrades SortMerge to Broadcast |
| How to debug slow job? | Spark UI → find stage with high task variance (skew) or GC time or disk spill |
| `repartition` vs `coalesce`? | repartition: full shuffle, balanced. coalesce: no shuffle, reduce only |

---

## Data Modeling Quick Answers

| Question | Answer |
|----------|--------|
| Star vs Snowflake? | Star = faster queries (fewer joins), more storage. Snowflake = normalized, less storage |
| SCD Type 2 columns? | `valid_from`, `valid_to` (9999-12-31), `is_current` |
| Grain definition? | The level of detail in each fact table row. Define before designing |
| Idempotency? | Running pipeline twice gives same result. Use MERGE, upsert, or partition overwrite |
| ETL vs ELT? | ETL: transform before load (legacy DWH). ELT: load raw, transform in-warehouse (cloud) |

---

## Kafka Quick Answers

| Question | Answer |
|----------|--------|
| Exactly-once semantics? | Idempotent producer + transactional API + consumer `read_committed` isolation |
| `acks=all` meaning? | Producer waits for all in-sync replicas to acknowledge |
| Consumer group rebalancing? | Triggered by consumer join/leave or heartbeat timeout. All pause during rebalance |
| Compacted topic? | Retains only latest value per key. Used for changelog/key-value semantics |
| ISR? | In-Sync Replicas — replicas caught up to leader's log |

---

## dbt Quick Answers

| Question | Answer |
|----------|--------|
| `is_incremental()` block? | Only evaluated during incremental runs (not first run or `--full-refresh`) |
| `unique_key` in incremental? | Used for upsert (MERGE). Required for `merge` strategy |
| Snapshot strategy difference? | `timestamp`: uses `updated_at`. `check`: hashes specified columns |
| `dbt build` vs `dbt run`? | `dbt build` = run + test + seed + snapshot in one command |
| Ephemeral model? | Becomes a CTE in downstream models. No DB object created |

---
---

# 17 — Hidden Concepts, Tips & Tricks
#hidden #tips #tricks #advanced #gotchas

**[[Data-Engineering-Vault/DE_Vault/00_Master_Index]]** | The stuff tutorials skip.

---

## SQL Hidden Gems

- **`FILTER` clause on aggregates** (PostgreSQL, DuckDB):
  ```sql
  COUNT(*) FILTER (WHERE status = 'active') AS active_count
  ```
- **`DISTINCT ON`** (PostgreSQL — get first row per group without subquery):
  ```sql
  SELECT DISTINCT ON (user_id) * FROM events ORDER BY user_id, ts DESC;
  ```
- **`LATERAL` joins** — correlated subquery that can reference outer table per row
- **`GROUPING SETS`** — multiple GROUP BY in one pass:
  ```sql
  GROUP BY GROUPING SETS ((country, city), (country), ())  -- all 3 levels
  -- Equivalent to: ROLLUP(country, city) for hierarchical
  ```
- **DuckDB for local analytics** — runs SQL on Parquet/CSV directly, much faster than pandas for large files. Use for local development.
- **`EXPLAIN (ANALYZE, BUFFERS)`** — shows buffer cache hits/misses, more info than plain EXPLAIN.

---

## Python Hidden Gems

- **`functools.cache` (Python 3.9+)** — simpler than `lru_cache()`:
  ```python
  @functools.cache
  def expensive_lookup(key: str) -> dict: ...
  ```
- **`dataclasses.field(default_factory=list)`** — mutable defaults in dataclasses
- **`contextlib.suppress`** — clean alternative to empty try/except:
  ```python
  with contextlib.suppress(FileNotFoundError):
      os.remove("temp.txt")
  ```
- **`itertools.batched` (Python 3.12+)** — chunk an iterable:
  ```python
  for batch in itertools.batched(records, 100):
      insert_batch(batch)
  ```
- **`pathlib.Path` over `os.path`** — always. Cleaner and cross-platform.
- **`__slots__`** in classes — reduces memory 40-60% for millions of instances.
- **`walrus operator :=`** (Python 3.8+) — assign in expression:
  ```python
  while chunk := file.read(8192):
      process(chunk)
  ```

---

## Spark Hidden Gems

- **`spark.sql.files.maxPartitionBytes`** — controls partition size when reading. Default 128MB.
- **`spark.default.parallelism`** — default RDD parallelism. Set to `2-4 × cores`.
- **`spark.sql.shuffle.partitions`** — default 200 for shuffle. Too many = overhead on small data. Too few = skew on large data. Set dynamically via AQE.
- **`df.explain("formatted")`** — readable plan (Spark 3.x).
- **`spark.hadoop.mapreduce.fileoutputcommitter.algorithm.version=2`** — faster S3 writes (skips rename operation).
- **Delta `OPTIMIZE` with `WHERE`** — partial optimization:
  ```sql
  OPTIMIZE events WHERE event_date = '2024-01-15' ZORDER BY user_id;
  ```
- **Speculative execution** — `spark.speculation=true` re-launches straggler tasks. Helps with transient failures but doubles load. Use in production carefully.
- **`df.hint("skewjoin")`** — Spark 3.x AQE hint for specific skewed joins.

---

## Airflow Hidden Gems

- **`ShortCircuitOperator`** — stops downstream tasks if callable returns False (no "skipped" mess)
- **`TriggerDagRunOperator`** — trigger another DAG from a DAG (cross-DAG orchestration)
- **`@task.branch`** — TaskFlow-compatible branching
- **`params` + `dag.test(run_conf={"date": "2024-01-01"})`** — run DAG locally for testing
- **`execution_date` vs `data_interval_start`** — Airflow 2.2+ uses data interval. Use `{{ data_interval_start }}` Jinja template for clarity.
- **Avoid dynamic task generation per-run** — it breaks DAG parsing performance. Use task groups or dynamic task mapping (`expand()`) instead.
- **`pool`** — limit concurrency of certain task types:
  ```python
  PythonOperator(task_id="api_call", pool="api_pool", pool_slots=1)
  ```
  Create pool in Airflow UI with slot limit = API rate limit.

---

## Kafka Hidden Gems

- **`kafka-dump-log.sh`** — inspect the actual log files on broker disk
- **`kafka-producer-perf-test.sh`** — built-in load testing tool
- **`auto.create.topics.enable=false`** in production — prevent accidental topic creation
- **`log.retention.bytes` vs `log.retention.hours`** — use both; whichever triggers first wins
- **Key `null` = round-robin partition** — if you don't need ordering, send null key for better load balance
- **Header metadata** — Kafka messages support headers (key-value pairs) for routing metadata without parsing the payload
- **Consumer group isolation level** — `isolation.level=read_committed` skips uncommitted transactional messages

---

## Snowflake Hidden Gems

- **`RESULT_SCAN(LAST_QUERY_ID())`** — query results of your last query as a table
- **`GENERATOR(ROWCOUNT => 1000000)`** — generate a million rows instantly for testing
- **`PARSE_JSON` + `FLATTEN`** — handle nested JSON without preprocessing:
  ```sql
  SELECT f.value:name::STRING AS name
  FROM raw_table, LATERAL FLATTEN(input => PARSE_JSON(json_col)) f;
  ```
- **`QUALIFY`** — filter on window function result without subquery:
  ```sql
  SELECT * FROM orders QUALIFY ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY ts DESC) = 1;
  ```
- **Snowflake Scripting** — procedural SQL (like PL/SQL) for complex stored procs
- **`SHOW TABLES LIKE '%order%'`** — search objects quickly
- **Query Tags** — tag queries for cost attribution:
  ```sql
  ALTER SESSION SET QUERY_TAG = 'dbt-run-2024-01-15';
  ```

---

## dbt Hidden Gems

- **`dbt-codegen`** package — auto-generate source YAML and base model SQL from DB schema
- **`dbt-expectations`** package — 50+ additional test types (expect_column_values_to_be_between, etc.)
- **`store_failures = true`** — failed test rows saved to a table for debugging
- **`meta:` field** — add custom metadata to models/columns (owner, steward, PII flag):
  ```yaml
  models:
    - name: fct_orders
      meta:
        owner: data-team
        pii: false
  ```
- **`--select state:modified+`** — only run modified models + all downstream. Huge CI time saver.
- **`dbt retry`** — re-run only failed models from last run (Airflow-style retry)
- **Semantic layer** — dbt MetricFlow for defining business metrics centrally (sum of revenue, DAU, etc.)

---

## GenAI / RAG Hidden Gems

- **`max_marginal_relevance_search`** (LangChain) — MMR reduces redundancy in retrieved chunks (avoids 5 nearly-identical chunks)
- **Metadata filtering is crucial** — always store doc type, date, source, department in vector DB payload. Filter before similarity search.
- **Embed queries differently** — some models use different prefix for query vs document: `"Represent this query: " + query` vs `"Represent this document: " + doc`
- **Chunking with sentence boundaries + overlap** → best general-purpose approach for most RAG tasks
- **`instructor` library** — forces LLM to return validated Pydantic models (structured extraction)
- **`LLMChainFilter`** — LLM-based context relevance filter after retrieval (removes irrelevant chunks before stuffing into prompt)
- **Context window stuffing order** — most relevant chunks at START and END (LLMs suffer from "lost in the middle" problem)

---

## Resources to Unlock More Knowledge

### Books
- **"Fundamentals of Data Engineering"** — Joe Reis & Matt Housley (the DE bible)
- **"Designing Data-Intensive Applications"** — Martin Kleppmann (DDIA — distributed systems bible)
- **"The Data Warehouse Toolkit"** — Kimball (data modeling classics)
- **"Learning Spark 3.x"** — O'Reilly

### Courses / Sites
- [DataEngineer.io](https://www.dataengineer.io) — DE-focused learning
- [Databricks Academy](https://academy.databricks.com) — free Spark + Delta courses
- [dbt Learn](https://learn.getdbt.com) — free official dbt courses
- [Kafka Tutorials](https://developer.confluent.io/tutorials/) — Confluent official
- [Mode SQL Tutorial](https://mode.com/sql-tutorial/)

### GitHub Repos
- [Apache Spark Examples](https://github.com/apache/spark/tree/master/examples)
- [dbt-project-evaluator](https://github.com/dbt-labs/dbt-project-evaluator) — audit your dbt project
- [Data Engineering Zoomcamp](https://github.com/DataTalksClub/data-engineering-zoomcamp) — free DE bootcamp

### Papers / Blogs
- [Delta Lake Paper](https://databricks.com/wp-content/uploads/2020/08/p975-armbrust.pdf)
- [Spark: Resilient Distributed Datasets](https://www.usenix.org/system/files/conference/nsdi12/nsdi12-final138.pdf)
- [The Log (Jay Kreps)](https://engineering.linkedin.com/distributed-systems/log-what-every-software-engineer-should-know-about-real-time-datas-unifying) — foundational Kafka essay

### Practice Datasets
- NYC Taxi Data (Parquet): `s3://nyc-tlc/`
- GitHub Archive: [gharchive.org](https://www.gharchive.org)
- Stack Overflow dump: [archive.org](https://archive.org/details/stackexchange)
- IMDB datasets: [imdb.com/interfaces](https://www.imdb.com/interfaces/)
