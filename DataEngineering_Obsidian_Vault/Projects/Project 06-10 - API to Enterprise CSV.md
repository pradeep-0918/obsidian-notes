# Project 06 - Public API Data Pipeline

#data-engineering #ETL #beginner #api #bigquery #finance #sports

---

## 📌 Overview

A pipeline that collects data from public REST APIs (weather, finance, sports), converts raw JSON responses into structured format, stores historical data for trend analysis, and displays patterns over time. API pipelines are foundational in every organization that relies on third-party data sources.

---

## 🎯 Scope
- **Ingestion**: Collect data from OpenWeather API, Alpha Vantage (Stock), Sports APIs
- **Processing**: Convert raw JSON → structured format using Python/PySpark
- **Storage**: Store historical data in AWS S3 / Google BigQuery / PostgreSQL
- **Visualization**: Display trends in Power BI / Streamlit / Tableau

---

## ⚙️ Tech Stack

| Layer | Tools | Purpose |
|-------|-------|---------|
| Data Source | OpenWeather, Alpha Vantage, Sports APIs | External data sources |
| Processing | Python (Requests, Pandas), PySpark | JSON parsing, transformation |
| Storage | AWS S3 / Google BigQuery / PostgreSQL | Historical data store |
| Visualization | Power BI / Streamlit / Tableau | Trend dashboards |

---

## 🔄 Data Pipeline Architecture

```
[External APIs]
    OpenWeather / Alpha Vantage / Sports APIs
         │ (HTTP GET with API Key)
         ▼
[Ingestion Layer - Python]
    Requests library + retry logic + caching
    - Rate limit handling (exponential backoff)
    - Response validation
    - JSON flattening (nested → tabular)
         │
         ▼
[Raw Storage]
    AWS S3 (JSON) — preserve raw API response
         │
         ▼
[Processing Layer]
    PySpark / Pandas
    - Flatten nested JSON
    - Type casting
    - Null imputation
    - Incremental deduplication
         │
         ▼
[Analytical Storage]
    BigQuery / PostgreSQL
    - Partitioned tables by date
         │
         ▼
[Visualization]
    Streamlit / Power BI / Tableau
```

---

## 📊 Use Cases
- **Weather Forecast Trends**: Historical weather analysis for agriculture, logistics
- **Stock Market Historical Analysis**: Backtesting trading strategies
- **Sports Performance Analytics**: Player and team statistics tracking

---

## ⚠️ Challenges

1. **API Rate Limits**: Free tiers limit requests per minute/day
2. **Data Inconsistency**: APIs change response structures between versions
3. **Handling Missing Data**: Not all fields are always present in API responses
4. **Pagination**: Large datasets returned across multiple API pages
5. **Authentication Management**: API keys rotating, expiring tokens

---

## ✅ Solutions

- **Implement caching & request throttling** — cache responses to avoid redundant API calls
- **Standardize formats using Pandas/PySpark** — flatten nested JSON into tabular format
- **Apply imputation techniques** for gaps in data (forward-fill, interpolation)
- **Schedule API calls efficiently** using APScheduler or Airflow to avoid rate limits
- **Use JSON parsing techniques** to extract relevant fields from API responses
- **Apply data cleaning steps** before storing for analytics

---

## 🧠 Key Learnings

- Understanding **REST API patterns** (authentication, pagination, rate limits)
- How to **flatten deeply nested JSON** into relational tables
- Implementing **exponential backoff** for resilient API calls
- Managing **API versioning** — pin to specific API versions to avoid breaking changes

---

## 🏢 Industry Insights

### How Bloomberg Handles Financial API Data
Bloomberg Terminal ingests data from thousands of financial APIs globally. They use a **message bus architecture** where all API data flows through a central broker before reaching analytics systems. Their latency for market data is sub-millisecond.

### How Weather.com Builds Data Pipelines
Weather.com ingests data from 250,000 weather stations globally via APIs. They use **Apache Storm** (real-time) and **Hadoop** (historical batch) in a Lambda Architecture, storing petabytes of historical weather data.

### How Spotify Ingests Third-Party Data
Spotify ingests music metadata, chart rankings, and social signals from external APIs via a managed ingestion platform. They use **Apache Kafka** as a buffer between APIs and processing — this decouples ingestion rate from processing rate.

---

## 🚀 Optimization Techniques

```python
import requests
import time
from functools import lru_cache

@lru_cache(maxsize=1000)
def get_stock_data(symbol: str, date: str) -> dict:
    """Cache API responses to avoid redundant calls."""
    url = f"https://www.alphavantage.co/query?function=TIME_SERIES_DAILY&symbol={symbol}"
    for attempt in range(5):
        try:
            response = requests.get(url, params={'apikey': API_KEY}, timeout=10)
            response.raise_for_status()
            return response.json()
        except requests.exceptions.RequestException:
            time.sleep(2 ** attempt)  # Exponential backoff
    raise Exception(f"Failed after 5 retries for {symbol}")
```

---

## 🔗 Related Projects

- [[Project 01 - CSV Data Pipeline]] — Batch file-based equivalent
- [[Project 17 - Stock Market Analytics with GCP]] — Advanced stock data pipeline with GCP
- [[ETL vs ELT]] — API pipelines often use ELT (store raw JSON, transform later)

---

*Tags: #data-engineering #ETL #beginner #api #python #bigquery #pandas #finance*

---
---

# Project 07 - Excel & Google Sheets Pipeline

#data-engineering #ETL #beginner #excel #google-sheets #pandas #business-ops

---

## 📌 Overview

A pipeline that extracts data from Excel spreadsheets and Google Sheets, cleans and merges multiple sheets, stores processed data for querying, and generates summary reports and dashboards. Despite sounding simple, spreadsheet ingestion is one of the most common real-world requirements — most SMEs and many large enterprises still use Excel as their primary data source.

---

## 🎯 Scope
- **Ingestion**: Extract data from Excel files and Google Sheets API
- **Processing**: Clean, merge, and standardize multiple sheets
- **Storage**: Store processed data in AWS S3 / Google Drive / PostgreSQL
- **Visualization**: Generate summary reports and dashboards

---

## ⚙️ Tech Stack

| Layer | Tools | Purpose |
|-------|-------|---------|
| Data Source | Excel Files, Google Sheets API | Spreadsheet data |
| Processing | Python (Pandas, OpenPyXL), PySpark | Data cleaning & merging |
| Storage | AWS S3 / Google Drive / PostgreSQL | Persistent storage |
| Visualization | Google Data Studio / Power BI / Tableau | Dashboards |

---

## 📊 Use Cases
- **Sales and Revenue Reporting**: Consolidate sales data from regional Excel files
- **Employee Performance Analysis**: Merge HR spreadsheets across departments
- **Budget and Expense Tracking**: Aggregate finance team's monthly budget sheets

---

## ⚠️ Challenges & Solutions

| Challenge | Solution |
|-----------|----------|
| Handling Multiple Sheet Formats | Standardize column names and data types |
| Dealing with Merged Cells | Normalize data structure using Pandas |
| Automating File Updates | Use Google Sheets API for real-time updates |
| Dates stored as serial numbers | Use `pd.to_datetime(df['date'], origin='1899-12-30', unit='D')` |
| Hidden rows/columns | Use OpenPyXL to access raw cell values |

---

## 🧠 Key Learnings

```python
import pandas as pd
import gspread
from oauth2client.service_account import ServiceAccountCredentials

# Google Sheets ingestion
scope = ['https://spreadsheets.google.com/feeds']
creds = ServiceAccountCredentials.from_json_keyfile_name('credentials.json', scope)
client = gspread.authorize(creds)
sheet = client.open("Sales Report").sheet1
df = pd.DataFrame(sheet.get_all_records())

# Excel ingestion with multiple sheets
xl = pd.ExcelFile('report.xlsx')
dfs = {sheet_name: xl.parse(sheet_name) for sheet_name in xl.sheet_names}
df_combined = pd.concat(dfs.values(), ignore_index=True)
```

---

## 🏢 Industry Insights

Most Fortune 500 companies have **"Excel to Database" migration** as a perpetual project. The standard approach is to build an ingestion layer that accepts Excel files via email or shared drive and automatically ingests them — this is called a **"spreadsheet-to-warehouse"** pattern.

---

## 🔗 Related Projects

- [[Project 01 - CSV Data Pipeline]] — Similar flat-file pattern
- [[Project 10 - Multi-Department CSV Integration]] — Enterprise multi-source version

---

*Tags: #data-engineering #ETL #beginner #excel #google-sheets #pandas #openpyxl #business-ops*

---
---

# Project 08 - IoT Sensor Data Pipeline

#data-engineering #ETL #beginner #iot #kafka #spark-streaming #grafana

---

## 📌 Overview

An IoT data pipeline that collects real-time sensor data from edge devices via MQTT brokers, processes it through Apache Kafka and Spark Streaming, stores structured sensor data for analysis, and monitors IoT device performance in real-time dashboards.

---

## 🎯 Scope
- **Ingestion**: Collect IoT sensor data from edge devices via MQTT
- **Processing**: Clean and transform real-time sensor readings
- **Storage**: Store structured sensor data (AWS IoT Core / Azure IoT Hub / PostgreSQL)
- **Visualization**: Monitor IoT device performance in real-time with Grafana / Power BI

---

## ⚙️ Tech Stack

| Layer | Tools | Purpose |
|-------|-------|---------|
| Data Source | IoT Sensors, MQTT Brokers | Hardware sensor data |
| Processing | Apache Kafka, Spark Streaming | Real-time stream processing |
| Storage | AWS IoT Core / Azure IoT Hub / PostgreSQL | Device data store |
| Visualization | Grafana / Power BI | Real-time monitoring |

---

## 🔄 Data Pipeline Architecture

```
[IoT Devices / Edge Sensors]
    MQTT Publish (topic: sensors/device_id/temperature)
         │
         ▼
[MQTT Broker]
    Eclipse Mosquitto / AWS IoT Core / HiveMQ
         │
         ▼
[Apache Kafka]
    Topic: iot-sensor-data
    Partitioned by: device_id
         │
         ▼
[Spark Streaming]
    - Anomaly detection (threshold-based rules)
    - Aggregation (5-min avg temperature)
    - Device health scoring
         │
         ├──▶ [Time-Series DB]  InfluxDB / TimescaleDB
         │        (for Grafana dashboards)
         │
         └──▶ [PostgreSQL / AWS RDS]
                  (historical analytics)
                       │
                       ▼
              [Grafana Dashboard]
              Real-time device monitoring
```

---

## 📊 Use Cases
- **Real-time Temperature & Humidity Monitoring**: Smart factories, data centers, agriculture
- **Predictive Maintenance for Industrial Equipment**: Detect bearing failure before it happens
- **Smart Home Energy Consumption Tracking**: Optimize HVAC and appliance usage

---

## ⚠️ Challenges

1. **High-Frequency Data Streams**: Industrial sensors can publish 1000 readings/second/device
2. **Handling Sensor Failures**: Sensors go offline, send erratic readings (spikes, nulls)
3. **Storage Scalability**: Millions of sensor readings per day require time-series optimized storage
4. **Device Management**: Tracking firmware versions, connectivity status across thousands of devices
5. **Edge Processing**: Some IoT architectures require processing AT the device (edge computing)

---

## ✅ Solutions

- **Optimize Kafka partitions** for better throughput (1 partition per device cluster)
- **Implement retry mechanisms & alert systems** for sensor failures
- **Use time-series databases** (InfluxDB, TimescaleDB) for optimized sensor data storage
- **Implement batch + streaming processing** for efficient analytics
- **Set up anomaly detection models** to detect faulty sensors in real-time

---

## 🧠 Key Learnings

- Understanding **MQTT protocol** (lightweight pub/sub for constrained devices)
- How **InfluxDB's time-series storage model** differs from relational databases
- Why **Kafka partition strategy** matters for IoT (partition by device_id = ordered messages per device)
- Implementing **stateful stream processing** in Spark (track device state across time windows)

---

## 🏢 Industry Insights

### How Tesla Uses IoT Pipelines
Tesla vehicles send telemetry data (speed, battery temp, motor stats) to Tesla's cloud continuously. They process **4 billion data points per day**. Their architecture uses Kafka for ingestion, custom stream processors for real-time alerting, and a data lake for ML model training.

### How GE Predix (Industrial IoT) Works
GE built the Predix platform for industrial IoT (jet engines, wind turbines, MRI machines). They use **Apache Kafka** for edge-to-cloud data streaming and **InfluxDB** for time-series sensor storage. Predictive maintenance models run on top of this data.

### How Smart Grid Companies Process IoT
Electric utilities use IoT pipelines to process smart meter data. They use **Apache Storm** or **Flink** for real-time demand forecasting and anomaly detection, preventing grid failures before they occur.

---

## 🚀 Optimization Techniques

- **InfluxDB Continuous Queries**: Pre-aggregate raw sensor data into 1-min, 5-min, 1-hour rollups
- **Kafka Consumer Groups**: Scale consumers horizontally by adding more instances
- **Edge Computing**: Use AWS Greengrass or Azure IoT Edge to filter/aggregate at the device
- **Data Compression**: Use MQTT QoS levels and payload compression (Protocol Buffers)

```python
# Spark Streaming IoT pipeline
from pyspark.sql.functions import avg, col, window, when

df_stream = spark.readStream.format("kafka") \
    .option("kafka.bootstrap.servers", "localhost:9092") \
    .option("subscribe", "iot-sensor-data") \
    .load()

df_parsed = df_stream.select(
    from_json(col("value").cast("string"), sensor_schema).alias("data")
).select("data.*")

# 5-minute rolling average with anomaly flag
df_windowed = df_parsed.groupBy(
    window(col("timestamp"), "5 minutes"),
    col("device_id")
).agg(avg("temperature").alias("avg_temp")) \
 .withColumn("is_anomaly", when(col("avg_temp") > 85, True).otherwise(False))

query = df_windowed.writeStream.format("jdbc") \
    .option("url", "jdbc:postgresql://db:5432/iot") \
    .outputMode("append").start()
```

---

## 🔗 Related Projects

- [[Project 12 - IoT Data Lake & Warehouse]] — Advanced IoT with full data lake
- [[Project 21 - Azure IoT Data Pipeline]] — Azure-specific IoT architecture
- [[Streaming vs Batch Processing]] — Why IoT data needs streaming
- [[Big Data Tools Overview]] — Kafka deep dive

---

*Tags: #data-engineering #ETL #beginner #iot #mqtt #kafka #spark-streaming #grafana #time-series*

---
---

# Project 09 - JSON & XML Processing Pipeline

#data-engineering #ETL #beginner #json #xml #pyspark #mongodb

---

## 📌 Overview

A pipeline for processing semi-structured JSON and XML data from multiple sources, normalizing nested structures, storing in both relational and NoSQL databases, and generating business insights. Handling nested/semi-structured data is one of the most underestimated skills in data engineering.

---

## 🎯 Scope
- **Ingestion**: Process JSON and XML files from APIs, local storage
- **Processing**: Parse, clean, and normalize nested data structures
- **Storage**: Store structured data in PostgreSQL / MongoDB / HDFS
- **Visualization**: Generate insights from transformed data

---

## ⚙️ Tech Stack

| Layer | Tools | Purpose |
|-------|-------|---------|
| Data Source | JSON / XML Files from APIs, Local Storage | Semi-structured data |
| Processing | Python (Pandas, BeautifulSoup, json module), PySpark | Parsing & flattening |
| Storage | PostgreSQL / MongoDB / HDFS | Relational + NoSQL storage |
| Visualization | Power BI / Tableau | Business insights |

---

## 🧠 Key Techniques

```python
# Flatten deeply nested JSON with PySpark
from pyspark.sql.functions import explode, col

# Handle array fields
df_exploded = df.withColumn("order_item", explode(col("order_items"))) \
                .select(
                    col("order_id"),
                    col("customer_id"),
                    col("order_item.product_id"),
                    col("order_item.quantity"),
                    col("order_item.price")
                )

# XML parsing with Python
import xml.etree.ElementTree as ET
tree = ET.parse('transactions.xml')
root = tree.getroot()
records = [{'id': child.find('id').text, 'amount': child.find('amount').text}
           for child in root.findall('transaction')]
```

---

## ⚠️ Challenges & Solutions

| Challenge | Solution |
|-----------|----------|
| Nested & Unstructured Data | Flatten using Pandas / PySpark `explode()` |
| Schema Mismatch Issues | Implement schema validation before ingestion |
| Handling Large File Sizes | Use streaming or batch processing |

---

## 🔗 Related Projects

- [[Project 01 - CSV Data Pipeline]] — Structured equivalent
- [[Project 06 - Public API Data Pipeline]] — Primary source of JSON data

---

*Tags: #data-engineering #ETL #beginner #json #xml #pyspark #mongodb #semi-structured*

---
---

# Project 10 - Multi-Department CSV Integration

#data-engineering #ETL #beginner #enterprise #azure #pandas #pyspark

---

## 📌 Overview

An enterprise-scale data integration pipeline that collects CSV files from multiple internal departments (HR, Sales, Finance), standardizes and merges them, stores in a structured format, and creates interactive cross-department dashboards.

---

## 🎯 Scope
- **Ingestion**: Collect CSV files from HR, Sales, and Finance departments
- **Processing**: Standardize, clean, and merge departmental data
- **Storage**: Store processed data in AWS S3 / Azure Blob / Local PostgreSQL
- **Visualization**: Create interactive dashboards for reporting

---

## ⚙️ Tech Stack

| Layer | Tools | Purpose |
|-------|-------|---------|
| Data Source | CSV Files (HR, Sales, Finance) | Department data |
| Processing | Python (Pandas, PySpark), SQL | Standardization & merge |
| Storage | AWS S3 / Azure Blob / PostgreSQL | Enterprise data store |
| Visualization | Power BI / Tableau / Excel | Cross-department dashboards |

---

## 📊 Use Cases
- **Company-wide Performance Analysis**: Revenue per employee, department efficiency metrics
- **Cross-department Data Unification**: Single source of truth for the entire organization
- **Financial & HR Data Reporting**: Automated monthly reports

---

## ⚠️ Challenges & Solutions

| Challenge | Solution |
|-----------|----------|
| Inconsistent Column Names & Formats | Standardize headers before merging |
| Missing or Duplicate Records | Handle using deduplication techniques |
| Scalability Issues | Optimize data storage and retrieval |

---

## 🧠 Key Technique — Standardization

```python
import pandas as pd

# Standardize column names across departments
def standardize_columns(df: pd.DataFrame, mapping: dict) -> pd.DataFrame:
    """Apply column name mapping to unify department schemas."""
    return df.rename(columns=mapping)

hr_mapping = {'emp_id': 'employee_id', 'dept': 'department', 'sal': 'salary'}
sales_mapping = {'salesperson_id': 'employee_id', 'team': 'department'}

df_hr = standardize_columns(pd.read_csv('hr.csv'), hr_mapping)
df_sales = standardize_columns(pd.read_csv('sales.csv'), sales_mapping)
df_merged = pd.merge(df_hr, df_sales, on='employee_id', how='left')
```

---

## 🏢 Industry Insights

This pattern is the foundation of **Enterprise Data Warehousing (EDW)**. Companies like Workday, SAP, and Oracle sell entire product suites around this exact problem — pulling data from disparate business systems into a unified analytical layer. The modern version of this uses **dbt (Data Build Tool)** for the transformation layer.

---

## 🔗 Related Projects

- [[Project 01 - CSV Data Pipeline]] — Single-source CSV version
- [[Project 03 - Retail POS ETL Pipeline]] — Add orchestration to this pattern
- [[Data Lake vs Data Warehouse vs Lakehouse]] — Where to store merged enterprise data

---

*Tags: #data-engineering #ETL #beginner #enterprise #azure #pandas #pyspark #csv #data-warehouse #integration*
