# 08 — AWS Core Data Services
#aws #s3 #glue #athena #kinesis #lambda #redshift #emr #iam

**Roadmap Days:** 15–16 | [[Data-Engineering-Vault/DE_Vault/00_Master_Index]] | Prev → [[07_Data_Modeling]] | Next → [[09_Snowflake]]

---

## 🗺️ Topic Map

```
AWS Data Services
├── S3 (storage classes, lifecycle, partitioning)
├── IAM (roles, policies, least privilege)
├── AWS Glue (ETL, Data Catalog, crawlers)
├── Athena (CTAS, partition projection, serverless SQL)
├── EMR (managed Spark cluster)
├── Kinesis Data Streams (real-time ingest)
├── Kinesis Firehose (delivery to S3/Redshift)
├── Lambda (serverless ETL triggers)
├── Step Functions (pipeline orchestration)
├── Secrets Manager
└── Redshift (distribution, sort keys, COPY)
```

---

## 1. Amazon S3

### Storage Classes
| Class | Use Case | Retrieval | Price |
|-------|---------|-----------|-------|
| **Standard** | Frequently accessed | Instant | $$$ |
| **Standard-IA** | Infrequently accessed | Instant | $$ |
| **Intelligent-Tiering** | Unknown access pattern | Instant | $$ (+ monitoring fee) |
| **Glacier Instant** | Archive, quarterly access | Instant | $ |
| **Glacier Flexible** | Archive | 1-12 hours | $ |
| **Glacier Deep Archive** | Long-term archive (7+ years) | 12-48 hours | ¢ |

### Lifecycle Policies
```json
{
  "Rules": [{
    "ID": "archive-old-data",
    "Filter": {"Prefix": "raw/"},
    "Status": "Enabled",
    "Transitions": [
      {"Days": 30, "StorageClass": "STANDARD_IA"},
      {"Days": 90, "StorageClass": "GLACIER"}
    ],
    "Expiration": {"Days": 365}
  }]
}
```

### Partitioned S3 Prefixes
```
s3://datalake/events/
  year=2024/
    month=01/
      day=15/
        part-00000.parquet
```
> 💡 **Hive-style partitioning:** Athena and Glue auto-discover `key=value` prefixes as partition columns. Essential for query performance — filters skip unrelated partitions.

> 💡 **S3 prefix performance:** S3 can handle 3,500 PUT/s and 5,500 GET/s **per prefix**. For high-throughput, randomize prefixes (hash prefix before date) to avoid hotspots.

---

## 2. IAM — Identity & Access Management

```json
// Least-privilege policy for Glue job
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:PutObject"],
      "Resource": "arn:aws:s3:::my-datalake/processed/*"
    },
    {
      "Effect": "Allow",
      "Action": ["glue:GetTable", "glue:GetDatabase"],
      "Resource": "*"
    }
  ]
}
```

### Key Concepts
- **IAM Role:** Identity with policies, assumed by services (Glue, Lambda, EC2)
- **IAM Policy:** JSON document defining permissions (Allow/Deny + Action + Resource)
- **AssumeRole:** Cross-account access — account B assumes role in account A
- **Instance Profile:** Attaches IAM role to EC2/EMR instance
- **Least Privilege:** Grant minimum permissions needed — audit with IAM Access Analyzer

---

## 3. AWS Glue

```python
# Glue ETL Job (PySpark)
import sys
from awsglue.context import GlueContext
from awsglue.job import Job
from awsglue.utils import getResolvedOptions
from pyspark.context import SparkContext

args = getResolvedOptions(sys.argv, ['JOB_NAME', 'source_path', 'target_path'])
sc = SparkContext()
glueContext = GlueContext(sc)
spark = glueContext.spark_session
job = Job(glueContext)
job.init(args['JOB_NAME'], args)

# Read from Data Catalog
datasource = glueContext.create_dynamic_frame.from_catalog(
    database="raw_db",
    table_name="events",
    push_down_predicate="year='2024' and month='01'"
)

# Convert to DataFrame for complex transforms
df = datasource.toDF()
# ... transform ...

# Write to S3 with Data Catalog update
glueContext.write_dynamic_frame.from_options(
    frame=DynamicFrame.fromDF(df, glueContext, "output"),
    connection_type="s3",
    connection_options={"path": args['target_path'], "partitionKeys": ["year", "month"]},
    format="parquet"
)
job.commit()
```

### Glue Data Catalog
- Central metadata repository for all your data (schema, location, partitions)
- Compatible with Hive Metastore
- Athena, Redshift Spectrum, EMR all read from it
- **Crawlers** auto-discover schema from S3, JDBC, DynamoDB

---

## 4. Amazon Athena

```sql
-- CTAS (Create Table As Select) — materialize query results to S3
CREATE TABLE processed_events
WITH (
  external_location = 's3://bucket/processed/events/',
  format = 'PARQUET',
  parquet_compression = 'SNAPPY',
  partitioned_by = ARRAY['year', 'month']
)
AS SELECT user_id, event_type, amount,
          year(event_time) AS year, month(event_time) AS month
FROM raw_events
WHERE amount > 0;

-- Partition projection (no need to run MSCK REPAIR TABLE)
ALTER TABLE events SET TBLPROPERTIES (
  'projection.enabled' = 'true',
  'projection.year.type' = 'integer',
  'projection.year.range' = '2020,2030',
  'projection.month.type' = 'integer',
  'projection.month.range' = '1,12'
);
```

> 💡 **Athena pricing:** $5 per TB scanned. Use Parquet + partitioning to minimize data scanned. Columnar Parquet can reduce scan by 90%+ vs CSV.

> 💡 **Athena vs Redshift:** Athena = serverless, ad-hoc, no load step. Redshift = high-performance, structured, concurrent users, BI workloads.

---

## 5. Amazon Kinesis

### Kinesis Data Streams
```python
import boto3, json

kinesis = boto3.client('kinesis', region_name='us-east-1')

# Producer
kinesis.put_record(
    StreamName='events-stream',
    Data=json.dumps({"user_id": "123", "event": "click"}),
    PartitionKey="user-123"   # determines shard
)

# Batch produce
kinesis.put_records(
    StreamName='events-stream',
    Records=[
        {"Data": json.dumps(r), "PartitionKey": r["user_id"]}
        for r in records
    ]
)
```

| Concept | Detail |
|---------|--------|
| **Shard** | Unit of throughput (1MB/s in, 2MB/s out) |
| **Retention** | 24 hours default, up to 7 days (365 with extended) |
| **Sequence number** | Unique per record within shard |
| **Partition key** | Determines which shard receives the record |

### Kinesis Firehose
- Fully managed, no consumer code
- Delivers to S3, Redshift, OpenSearch, Splunk
- Buffering: by size (1-128MB) or time (60-900 sec) — whichever comes first
- Supports Lambda transform inline

---

## 6. AWS Lambda for ETL

```python
import boto3, json

def handler(event, context):
    """S3-triggered Lambda — validate new file and write metadata."""
    s3 = boto3.client('s3')
    
    for record in event['Records']:
        bucket = record['s3']['bucket']['name']
        key = record['s3']['object']['key']
        
        # Get file metadata
        obj = s3.get_object(Bucket=bucket, Key=key)
        size = obj['ContentLength']
        
        # Validate
        if size == 0:
            raise ValueError(f"Empty file: {key}")
        
        # Write metadata to DynamoDB
        dynamodb = boto3.resource('dynamodb')
        table = dynamodb.Table('file-registry')
        table.put_item(Item={
            "file_key": key,
            "bucket": bucket,
            "size_bytes": size,
            "processed_at": context.aws_request_id
        })
    
    return {"statusCode": 200, "body": "OK"}
```

---

## 7. Amazon Redshift

### Distribution Styles
| Style | How | Best For |
|-------|-----|---------|
| `KEY(col)` | Rows with same key value → same node | Large tables joined on same key |
| `ALL` | Full copy on every node | Small dimension tables |
| `EVEN` | Round-robin | No clear join key, or fact table with many join keys |
| `AUTO` | Redshift decides (default) | Let Redshift optimize |

### Sort Keys
```sql
-- Compound sort key — filter on first col benefits most
CREATE TABLE orders (
  order_id BIGINT,
  order_date DATE,
  user_id BIGINT,
  amount DECIMAL
)
DISTKEY(user_id)
SORTKEY(order_date, user_id);   -- compound

-- Interleaved sort key — equal weight on multiple columns
INTERLEAVED SORTKEY(order_date, user_id, region);
```

### COPY Command (bulk load)
```sql
COPY orders FROM 's3://bucket/orders/'
IAM_ROLE 'arn:aws:iam::123:role/RedshiftS3'
FORMAT AS PARQUET;  -- or CSV DELIMITER ',' IGNOREHEADER 1
```

> 💡 **WLM (Workload Management):** Queue-based query prioritization. ETL jobs in low-priority queue; BI queries in high-priority. Use `concurrency_scaling` for burst.

---

## 🃏 Interview Cheatsheet
- **S3 storage classes?** Standard → Standard-IA (30d) → Glacier (90d) → Deep Archive (long term).
- **Athena cost optimization?** Parquet format + Hive partitioning + columnar projections = minimize data scanned.
- **Glue Crawler vs manual schema?** Crawler auto-discovers; manual more reliable for production (crawlers can mistype).
- **Kinesis shard throughput?** 1MB/s write, 2MB/s read per shard. Scale by adding shards.
- **Redshift DIST KEY selection?** Key used in most joins. Avoids data movement during joins when both tables share same distkey.
- **Lambda trigger for ETL?** S3 event → Lambda → validate/route → trigger Glue or write metadata.
