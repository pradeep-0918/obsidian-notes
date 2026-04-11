# 🗜️ Serialization Formats — Complete Guide
#concept #serialization #parquet #avro #protobuf

> How you store data determines how fast you can read it. Format choice = performance choice.

---

## 🧠 Row vs Columnar Storage

This is the most important concept in data format selection.

```
Data:
┌─────┬────────┬────────┬────────┐
│ ID  │  Name  │ Region │Amount  │
├─────┼────────┼────────┼────────┤
│  1  │ Alice  │   US   │  100   │
│  2  │  Bob   │   EU   │  200   │
│  3  │ Carol  │   US   │  300   │
└─────┴────────┴────────┴────────┘

ROW Storage (CSV, JSON):          COLUMNAR Storage (Parquet, ORC):
[1, Alice, US, 100]               IDs:     [1, 2, 3]
[2, Bob, EU, 200]                 Names:   [Alice, Bob, Carol]
[3, Carol, US, 300]               Regions: [US, EU, US]
                                  Amounts: [100, 200, 300]

Query: "SUM(Amount) WHERE Region='US'"

ROW: Read ALL data → filter        COLUMNAR: Read ONLY Region + Amount
     → 100% I/O                             → 50% I/O (skip Names, IDs)
     
COLUMNAR wins by 2x-10x for analytics!
```

---

## 📊 Apache Parquet

The **de facto standard** for analytical workloads. Use it for almost everything in data engineering.

### Why Parquet is Dominant

1. **Columnar** → Only reads needed columns
2. **Compressed per column** → Integers compress differently than strings
3. **Predicate pushdown** → Skip row groups that don't match filters
4. **Schema embedded** → No need for external schema registry
5. **Widely supported** → Spark, Pandas, Hive, Presto, Athena all support it natively

### Parquet Internals

```
Parquet File Layout:
┌──────────────────────────────────────────┐
│                  Header                   │
├──────────────────────────────────────────┤
│              Row Group 1                  │  ← ~128MB of rows
│  ┌──────────────────────────────────┐    │
│  │ Column Chunk: ID                 │    │
│  │  Page 1: [1, 2, 3, 4, 5...]     │    │  ← Compressed
│  │  Page 2: [1001, 1002, 1003...]   │    │
│  ├──────────────────────────────────┤    │
│  │ Column Chunk: Amount             │    │
│  │  Page 1: [100.0, 200.5, 99.9]   │    │
│  └──────────────────────────────────┘    │
├──────────────────────────────────────────┤
│              Row Group 2                  │
│              ...                          │
├──────────────────────────────────────────┤
│              Footer                       │  ← Statistics per column
│  - Min/Max values per row group          │  ← Schema
│  - Row counts                            │  ← Column offsets
└──────────────────────────────────────────┘
```

### Working with Parquet in Python

```python
import pandas as pd
import pyarrow as pa
import pyarrow.parquet as pq

# Write Parquet
df = pd.DataFrame({
    'id': range(1000),
    'name': ['user_' + str(i) for i in range(1000)],
    'amount': [i * 1.5 for i in range(1000)],
    'region': ['US', 'EU', 'APAC'] * 333 + ['US']
})

# Simple write
df.to_parquet('output.parquet', compression='snappy')

# Advanced write with partitioning
table = pa.Table.from_pandas(df)
pq.write_to_dataset(
    table,
    root_path='output_partitioned/',
    partition_cols=['region'],
    compression='snappy',
    row_group_size=100_000  # Tune for your use case
)

# Read with column selection (only reads those columns from disk!)
df_partial = pd.read_parquet('output.parquet', columns=['id', 'amount'])

# Read with filter pushdown (skips irrelevant row groups)
df_filtered = pd.read_parquet(
    'output_partitioned/',
    filters=[('region', '==', 'US'), ('amount', '>', 100)]
)

# Schema inspection
parquet_file = pq.ParquetFile('output.parquet')
print(parquet_file.schema)
print(f"Rows: {parquet_file.metadata.num_rows}")
print(f"Row groups: {parquet_file.metadata.num_row_groups}")

# Column statistics (for understanding data without reading it)
for rg in range(parquet_file.metadata.num_row_groups):
    for col in range(parquet_file.metadata.num_columns):
        stats = parquet_file.metadata.row_group(rg).column(col).statistics
        if stats:
            print(f"Column: {stats.min} to {stats.max}")
```

### Parquet Best Practices

```python
# 1. Target file size: 128MB-1GB
# Too small = too many files = slow listing
# Too large = hard to parallelize reads

# 2. Partition by commonly filtered columns
df.to_parquet(
    'data/',
    partition_cols=['year', 'month'],  # NOT day (too many partitions)
)

# 3. Use appropriate data types
# BAD: Store IDs as strings
# GOOD: Store IDs as int64 (4x less space)

# 4. Use dictionary encoding for low-cardinality strings
# Parquet does this automatically for columns with <1000 unique values

# 5. Sort data before writing (improves compression + query speed)
df_sorted = df.sort_values(['customer_id', 'order_date'])
df_sorted.to_parquet('sorted_orders.parquet')
```

---

## 🔄 Apache Avro

Best for **data serialization and streaming** (especially with Kafka). Row-based with schema evolution.

### Key Features
- Row-based (good for write, not analytics reads)
- Schema stored separately in Schema Registry
- Excellent schema evolution support
- Used heavily in Kafka ecosystems

```python
import avro.schema
from avro.datafile import DataFileWriter, DataFileReader
from avro.io import DatumWriter, DatumReader

# Define schema
schema = avro.schema.parse(json.dumps({
    "type": "record",
    "name": "Order",
    "namespace": "com.mycompany.events",
    "fields": [
        {"name": "order_id", "type": "long"},
        {"name": "customer_id", "type": "long"},
        {"name": "amount", "type": "double"},
        {"name": "status", "type": "string"},
        # New field with default = backward compatible
        {"name": "discount", "type": ["null", "double"], "default": None}
    ]
}))

# Write
with DataFileWriter(open("orders.avro", "wb"), DatumWriter(), schema) as writer:
    writer.append({
        "order_id": 1001,
        "customer_id": 42,
        "amount": 99.99,
        "status": "completed",
        "discount": None
    })

# Read
with DataFileReader(open("orders.avro", "rb"), DatumReader()) as reader:
    for order in reader:
        print(order)
```

### Schema Evolution Rules

```
BACKWARD COMPATIBLE (new schema can read old data):
✅ Add field with default value
✅ Remove field with default value
✅ Change field type to wider type (int → long)

BREAKING CHANGES (avoid):
❌ Remove required field
❌ Rename field
❌ Change field type incompatibly (string → int)
```

---

## 🔢 Protocol Buffers (Protobuf)

Google's format. Extremely efficient binary encoding. Used in gRPC.

```protobuf
// Define schema in .proto file
syntax = "proto3";
package orders;

message Order {
    int64 order_id = 1;
    int64 customer_id = 2;
    double amount = 3;
    string status = 4;
    repeated OrderItem items = 5;
    google.protobuf.Timestamp created_at = 6;
}

message OrderItem {
    int64 product_id = 1;
    int32 quantity = 2;
    double unit_price = 3;
}
```

```python
# After generating Python code from .proto:
from order_pb2 import Order, OrderItem
from google.protobuf import timestamp_pb2

order = Order(
    order_id=1001,
    customer_id=42,
    amount=99.99,
    status="completed",
    items=[
        OrderItem(product_id=501, quantity=2, unit_price=49.99)
    ]
)

# Serialize (very compact binary)
serialized = order.SerializeToString()
print(f"Size: {len(serialized)} bytes")

# Deserialize
order_back = Order()
order_back.ParseFromString(serialized)
```

---

## 📋 Format Comparison Table

| Format | Storage | Schema | Compression | Best For | Avoid |
|---|---|---|---|---|---|
| CSV | Row | External | Optional | Exchange, small data | Analytics at scale |
| JSON | Row | None | Optional | APIs, flexibility | Storage efficiency |
| Avro | Row | Embedded | Yes | Kafka, streaming | Analytics queries |
| Parquet | Columnar | Embedded | Yes (per column) | Analytics, most DE | Streaming writes |
| ORC | Columnar | Embedded | Yes | Hive ecosystem | Non-Hive |
| Protobuf | Row | .proto file | Yes | gRPC, microservices | Data lakes |
| Delta | Columnar+log | Embedded | Yes | ACID data lakes | Simple use cases |

---

## 🗜️ Compression Algorithms

| Algorithm | Ratio | Speed | CPU | Use When |
|---|---|---|---|---|
| Snappy | Medium | Very fast | Low | Default for Parquet (balanced) |
| GZIP | High | Slow | High | Storage constrained, cold data |
| LZ4 | Low | Fastest | Lowest | Speed critical, hot data |
| ZSTD | High | Fast | Medium | Best all-around (Parquet 2.0) |
| Brotli | Highest | Slow | High | Web assets |
| Uncompressed | None | Fastest | None | Debugging only |

```python
# Benchmark compression for your data
import time
import os

for compression in ['snappy', 'gzip', 'lz4', 'zstd', None]:
    start = time.time()
    df.to_parquet(f'test_{compression}.parquet', compression=compression)
    elapsed = time.time() - start
    size = os.path.getsize(f'test_{compression}.parquet')
    print(f"{compression}: {size/1024/1024:.1f}MB, wrote in {elapsed:.2f}s")
```

---

*Related: [[02-Core-Concepts/File-Systems]] | [[02-Core-Concepts/Batch-Processing]] | Back to [[00-Index]]*
