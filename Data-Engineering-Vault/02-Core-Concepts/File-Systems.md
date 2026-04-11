# 📁 File Systems — Distributed Storage Guide
#concept #filesystem #hdfs #s3 #storage

> Where your data lives at rest. Choose wisely — migration is painful.

---

## 🧠 The Mental Model

Think of storage systems as different types of warehouses:
- **Local storage** = Your desk drawer (fast, limited, single location)
- **HDFS** = A massive warehouse with items distributed across many rooms
- **S3** = An infinitely large storage unit that you rent by the cubic foot
- **Delta Lake** = S3 but with a transaction log (ACID guarantees)

---

## 🐘 HDFS — Hadoop Distributed File System

### How HDFS Works

```
┌─────────────────────────────────────────────────────────┐
│                    HDFS CLUSTER                          │
│                                                         │
│  ┌──────────────┐    Data is split into 128MB blocks   │
│  │  NameNode    │    Each block replicated 3x           │
│  │  (Master)    │                                       │
│  │  - Metadata  │    File: BigFile.csv (400MB)          │
│  │  - Block map │    Block 1 → [DN1, DN2, DN4]          │
│  │  - Namespace │    Block 2 → [DN2, DN3, DN5]          │
│  └──────┬───────┘    Block 3 → [DN1, DN3, DN6]          │
│         │                                               │
│  ┌──────┴──────────────────────────────────────┐        │
│  │         Data Nodes (DN1, DN2...DNn)          │        │
│  │  - Store actual data blocks                 │        │
│  │  - Report heartbeat to NameNode             │        │
│  │  - Replicate blocks to peers                │        │
│  └──────────────────────────────────────────────┘        │
└─────────────────────────────────────────────────────────┘
```

### HDFS CLI Commands

```bash
# List files
hdfs dfs -ls /data/warehouse/

# Create directory
hdfs dfs -mkdir -p /data/warehouse/orders/year=2024/

# Copy local → HDFS
hdfs dfs -put local_file.parquet /data/warehouse/orders/

# Copy HDFS → local
hdfs dfs -get /data/warehouse/orders/orders.parquet ./

# Copy within HDFS
hdfs dfs -cp /data/source/ /data/destination/

# Check disk usage
hdfs dfs -du -h /data/

# Check replication
hdfs fsck /data/warehouse/ -files -blocks

# Change replication factor
hdfs dfs -setrep -R 2 /data/cold_archive/  # Reduce replication for cold data
```

### HDFS with Python (Snakebite / WebHDFS)

```python
# Using pyarrow for HDFS access (modern approach)
import pyarrow.hdfs as hdfs
import pyarrow.parquet as pq

# Connect
fs = hdfs.connect(host='namenode', port=8020)

# List files
files = fs.ls('/data/warehouse/')

# Read Parquet directly
dataset = pq.ParquetDataset('/data/warehouse/orders/', filesystem=fs)
df = dataset.read_pandas().to_pandas()

# Write Parquet
import pandas as pd
df = pd.DataFrame(...)
pq.write_to_dataset(
    pq.Table.from_pandas(df),
    root_path='/data/warehouse/new_table/',
    filesystem=fs,
    partition_cols=['year', 'month']
)
```

---

## ☁️ AWS S3 — Object Storage Standard

S3 is NOT a file system. It's an **object store** with a flat key namespace.

```
S3 is NOT:                    S3 IS:
/folder/subfolder/file        A dictionary:
                              {"folder/subfolder/file": bytes}
```

### Core S3 Concepts

```python
import boto3
from botocore.config import Config

# Client with retry configuration
s3 = boto3.client('s3', config=Config(
    retries={'max_attempts': 3, 'mode': 'adaptive'}
))

# ── BASIC OPERATIONS ──────────────────────────────────────────────

# Upload
s3.upload_file('local.parquet', 'my-bucket', 'data/orders/2024/orders.parquet')

# Download
s3.download_file('my-bucket', 'data/orders/2024/orders.parquet', 'local.parquet')

# List objects (paginated!)
paginator = s3.get_paginator('list_objects_v2')
pages = paginator.paginate(Bucket='my-bucket', Prefix='data/orders/2024/')
for page in pages:
    for obj in page.get('Contents', []):
        print(f"{obj['Key']}: {obj['Size']} bytes")

# Delete
s3.delete_object(Bucket='my-bucket', Key='data/old_file.csv')

# Copy (server-side, no data transfer costs within same region)
s3.copy_object(
    Bucket='my-bucket',
    Key='data/archive/orders.parquet',
    CopySource={'Bucket': 'my-bucket', 'Key': 'data/orders/orders.parquet'}
)

# ── MULTIPART UPLOAD (for large files) ────────────────────────────

from boto3.s3.transfer import TransferConfig

config = TransferConfig(
    multipart_threshold=1024 * 25,  # 25MB
    max_concurrency=10,
    multipart_chunksize=1024 * 25,
    use_threads=True
)

s3.upload_file(
    'huge_file.parquet',
    'my-bucket',
    'data/huge_file.parquet',
    Config=config
)

# ── PRESIGNED URLS (temporary access) ────────────────────────────

url = s3.generate_presigned_url(
    'get_object',
    Params={'Bucket': 'my-bucket', 'Key': 'data/report.pdf'},
    ExpiresIn=3600  # 1 hour
)
print(f"Share this URL: {url}")
```

### S3 Storage Classes

| Class | Use Case | Retrieval | Cost |
|---|---|---|---|
| Standard | Frequently accessed | Instant | $$$ |
| Standard-IA | Monthly access | Instant | $$ |
| Glacier Instant | Quarterly access | Instant | $ |
| Glacier Flexible | Archive, any time | 1-12 hours | ¢ |
| Glacier Deep Archive | 7+ year retention | 12-48 hours | ¢¢ |
| Intelligent Tiering | Unknown patterns | Varies | Auto |

```python
# Apply lifecycle rules via code
s3.put_bucket_lifecycle_configuration(
    Bucket='my-bucket',
    LifecycleConfiguration={
        'Rules': [{
            'ID': 'Move old data to cheaper storage',
            'Status': 'Enabled',
            'Filter': {'Prefix': 'data/raw/'},
            'Transitions': [
                {'Days': 30, 'StorageClass': 'STANDARD_IA'},
                {'Days': 90, 'StorageClass': 'GLACIER'},
                {'Days': 365, 'StorageClass': 'DEEP_ARCHIVE'}
            ],
            'Expiration': {'Days': 2555}  # Delete after 7 years
        }]
    }
)
```

### Reading S3 Directly with Pandas / PySpark

```python
# Pandas (small files)
import pandas as pd
df = pd.read_parquet("s3://my-bucket/data/orders.parquet")
df.to_parquet("s3://my-bucket/data/processed/orders.parquet")

# PySpark (large files)
df = spark.read.parquet("s3a://my-bucket/data/orders/")
df.write.parquet("s3a://my-bucket/data/processed/orders/")

# Configure S3A in Spark
spark = SparkSession.builder \
    .config("spark.hadoop.fs.s3a.access.key", "ACCESS_KEY") \
    .config("spark.hadoop.fs.s3a.secret.key", "SECRET_KEY") \
    .config("spark.hadoop.fs.s3a.impl", "org.apache.hadoop.fs.s3a.S3AFileSystem") \
    .config("spark.hadoop.fs.s3a.aws.credentials.provider",
            "com.amazonaws.auth.InstanceProfileCredentialsProvider") \
    .getOrCreate()
```

---

## 📊 Table Formats — The New Layer

Table formats add database features on top of file storage.

### Apache Iceberg

```python
# Spark with Iceberg
spark = SparkSession.builder \
    .config("spark.sql.extensions", 
            "org.apache.iceberg.spark.extensions.IcebergSparkSessionExtensions") \
    .config("spark.sql.catalog.spark_catalog",
            "org.apache.iceberg.spark.SparkSessionCatalog") \
    .getOrCreate()

# Create Iceberg table
spark.sql("""
    CREATE TABLE IF NOT EXISTS catalog.db.orders (
        order_id    BIGINT,
        customer_id BIGINT,
        amount      DOUBLE,
        status      STRING,
        order_date  DATE
    )
    USING iceberg
    PARTITIONED BY (months(order_date))
    TBLPROPERTIES (
        'write.target-file-size-bytes' = '134217728',
        'history.expire.min-snapshots-to-keep' = '10'
    )
""")

# Time travel
df_old = spark.read \
    .option("snapshot-id", "1234567890") \
    .table("catalog.db.orders")

df_yesterday = spark.read \
    .option("as-of-timestamp", "2024-01-01 00:00:00") \
    .table("catalog.db.orders")

# Schema evolution
spark.sql("ALTER TABLE catalog.db.orders ADD COLUMN discount DOUBLE")
spark.sql("ALTER TABLE catalog.db.orders RENAME COLUMN status TO order_status")
```

### Delta Lake

```python
from delta.tables import DeltaTable

# Create
df.write.format("delta").save("s3://bucket/delta/orders/")

# ACID merge (upsert)
delta = DeltaTable.forPath(spark, "s3://bucket/delta/orders/")
delta.alias("target").merge(
    new_data.alias("source"),
    "target.order_id = source.order_id"
).whenMatchedUpdateAll() \
 .whenNotMatchedInsertAll() \
 .execute()

# Time travel
spark.read.format("delta") \
    .option("versionAsOf", 5) \
    .load("s3://bucket/delta/orders/")

# Optimize (compaction + Z-ordering)
spark.sql("OPTIMIZE delta.`s3://bucket/delta/orders/` ZORDER BY (customer_id)")

# Vacuum (clean old files)
spark.sql("VACUUM delta.`s3://bucket/delta/orders/` RETAIN 168 HOURS")
```

---

## 📏 File Format Comparison

| Format | Type | Compression | Schema | Best For |
|---|---|---|---|---|
| CSV | Row | None/optional | No | Simple exchange, small data |
| JSON | Row | None/optional | No | APIs, nested data |
| Avro | Row | Yes | Yes (embedded) | Schema evolution, Kafka |
| Parquet | Columnar | Yes | Yes | Analytics, Spark, most DE |
| ORC | Columnar | Yes | Yes | Hive, heavy analytics |
| Delta | Columnar+log | Yes | Yes | ACID on lakes |
| Iceberg | Columnar+catalog | Yes | Yes | Multi-engine, large scale |

---

*Related: [[02-Core-Concepts/Serialization]] | [[04-System-Design/Data-Lake]] | Back to [[00-Index]]*
