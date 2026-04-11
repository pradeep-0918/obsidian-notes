# 🛠️ Beginner Projects — Portfolio Guide
#project #beginner #portfolio #career

> Build these to prove you can do the job. Each one is a complete, demonstrable pipeline.

---

## 🎯 Project 1: Weather Data Pipeline

**What you'll build:** Automated pipeline that fetches weather data daily and stores it for analysis.

**Skills demonstrated:** API integration, Python ETL, PostgreSQL, scheduling

**Time:** 1 weekend

### Architecture
```
OpenWeatherMap API → Python Script → PostgreSQL → Report CSV
```

### Implementation

```python
# weather_pipeline.py
import requests
import psycopg2
import pandas as pd
from datetime import datetime
import os
import logging

logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s %(levelname)s %(message)s'
)
log = logging.getLogger(__name__)

API_KEY = os.environ.get("OPENWEATHER_API_KEY")
CITIES = ["Chennai", "Mumbai", "Delhi", "Bangalore", "Hyderabad"]

# ── EXTRACT ─────────────────────────────────────────────────────

def fetch_weather(city: str) -> dict:
    url = "https://api.openweathermap.org/data/2.5/weather"
    params = {"q": city, "appid": API_KEY, "units": "metric"}
    resp = requests.get(url, params=params, timeout=10)
    resp.raise_for_status()
    return resp.json()

def extract_all_cities() -> list[dict]:
    records = []
    for city in CITIES:
        try:
            raw = fetch_weather(city)
            records.append({
                "city": city,
                "country": raw["sys"]["country"],
                "temp_celsius": raw["main"]["temp"],
                "feels_like": raw["main"]["feels_like"],
                "humidity_pct": raw["main"]["humidity"],
                "pressure_hpa": raw["main"]["pressure"],
                "wind_speed_mps": raw["wind"]["speed"],
                "weather_main": raw["weather"][0]["main"],
                "weather_desc": raw["weather"][0]["description"],
                "visibility_m": raw.get("visibility"),
                "recorded_at": datetime.utcfromtimestamp(raw["dt"]),
                "ingested_at": datetime.utcnow()
            })
            log.info(f"Fetched weather for {city}")
        except Exception as e:
            log.error(f"Failed to fetch {city}: {e}")
    return records

# ── TRANSFORM ────────────────────────────────────────────────────

def transform(records: list[dict]) -> pd.DataFrame:
    df = pd.DataFrame(records)
    # Derive heat index
    df["heat_index"] = df["temp_celsius"] * 0.1 + df["humidity_pct"] * 0.2
    # Categorize wind
    df["wind_category"] = pd.cut(
        df["wind_speed_mps"],
        bins=[0, 2, 5, 10, 100],
        labels=["Calm", "Breeze", "Windy", "Storm"]
    )
    return df

# ── LOAD ─────────────────────────────────────────────────────────

DDL = """
CREATE TABLE IF NOT EXISTS weather_readings (
    id              SERIAL PRIMARY KEY,
    city            VARCHAR(100) NOT NULL,
    country         CHAR(2),
    temp_celsius    NUMERIC(5,2),
    feels_like      NUMERIC(5,2),
    humidity_pct    INT,
    pressure_hpa    INT,
    wind_speed_mps  NUMERIC(6,2),
    wind_category   VARCHAR(20),
    weather_main    VARCHAR(50),
    weather_desc    VARCHAR(100),
    heat_index      NUMERIC(6,2),
    visibility_m    INT,
    recorded_at     TIMESTAMP,
    ingested_at     TIMESTAMP DEFAULT NOW()
);
CREATE INDEX IF NOT EXISTS idx_weather_city_time 
    ON weather_readings(city, recorded_at DESC);
"""

def load(df: pd.DataFrame, conn_str: str):
    conn = psycopg2.connect(conn_str)
    cur = conn.cursor()
    cur.execute(DDL)
    
    for _, row in df.iterrows():
        cur.execute("""
            INSERT INTO weather_readings 
                (city, country, temp_celsius, feels_like, humidity_pct,
                 pressure_hpa, wind_speed_mps, wind_category, weather_main,
                 weather_desc, heat_index, visibility_m, recorded_at)
            VALUES (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)
        """, tuple(row[[
            "city","country","temp_celsius","feels_like","humidity_pct",
            "pressure_hpa","wind_speed_mps","wind_category","weather_main",
            "weather_desc","heat_index","visibility_m","recorded_at"
        ]]))
    
    conn.commit()
    cur.close()
    conn.close()
    log.info(f"Loaded {len(df)} records")

# ── REPORT ────────────────────────────────────────────────────────

def generate_report(conn_str: str):
    conn = psycopg2.connect(conn_str)
    df = pd.read_sql("""
        SELECT city,
               AVG(temp_celsius) AS avg_temp,
               MAX(temp_celsius) AS max_temp,
               MIN(temp_celsius) AS min_temp,
               AVG(humidity_pct) AS avg_humidity,
               COUNT(*) AS readings
        FROM weather_readings
        WHERE ingested_at > NOW() - INTERVAL '7 days'
        GROUP BY city
        ORDER BY avg_temp DESC
    """, conn)
    df.to_csv("weekly_weather_report.csv", index=False)
    log.info("Report generated: weekly_weather_report.csv")

if __name__ == "__main__":
    DB = os.environ.get("DATABASE_URL", "postgresql://user:pass@localhost/weather_db")
    records = extract_all_cities()
    df = transform(records)
    load(df, DB)
    generate_report(DB)
```

### Running with Cron / Airflow
```bash
# cron (every hour)
0 * * * * cd /app && python weather_pipeline.py >> /var/log/weather.log 2>&1

# Or as an Airflow DAG (see [[03-Tools/Airflow]])
```

### GitHub README Structure
```markdown
# Weather Data Pipeline

## Overview
Automated pipeline that collects weather data for 5 Indian cities 
every hour and stores it for trend analysis.

## Architecture
OpenWeatherMap API → Python → PostgreSQL → CSV Reports

## Tech Stack
- Python 3.11 (requests, pandas, psycopg2)
- PostgreSQL 15
- Apache Airflow 2.8 (scheduling)
- Docker Compose (local development)

## Setup
...

## Sample Output
[Screenshot of dashboard/report]
```

---

## 🎯 Project 2: Reddit Trend Tracker

**What you'll build:** Track trending topics on Reddit by subreddit over time.

**Skills demonstrated:** API rate limiting, data modeling, trend analysis, visualization

```python
import praw
import pandas as pd
import sqlite3
from datetime import datetime, timezone

reddit = praw.Reddit(
    client_id=os.environ["REDDIT_CLIENT_ID"],
    client_secret=os.environ["REDDIT_CLIENT_SECRET"],
    user_agent="TrendTracker/1.0"
)

def fetch_subreddit_posts(subreddit_name: str, limit: int = 100) -> list[dict]:
    subreddit = reddit.subreddit(subreddit_name)
    posts = []
    for post in subreddit.hot(limit=limit):
        posts.append({
            "post_id": post.id,
            "title": post.title,
            "score": post.score,
            "upvote_ratio": post.upvote_ratio,
            "num_comments": post.num_comments,
            "author": str(post.author),
            "created_utc": datetime.fromtimestamp(post.created_utc, tz=timezone.utc),
            "url": post.url,
            "flair": post.link_flair_text,
            "subreddit": subreddit_name,
            "fetched_at": datetime.now(timezone.utc)
        })
    return posts

SUBREDDITS = ["dataengineering", "datascience", "MachineLearning", "Python"]

all_posts = []
for sr in SUBREDDITS:
    all_posts.extend(fetch_subreddit_posts(sr))

df = pd.DataFrame(all_posts)

# Store in SQLite for simplicity
conn = sqlite3.connect("reddit_trends.db")
df.to_sql("posts", conn, if_exists="append", index=False)

# Analysis: Top topics by engagement
trends = pd.read_sql("""
    SELECT 
        DATE(created_utc) as date,
        subreddit,
        COUNT(*) as post_count,
        AVG(score) as avg_score,
        SUM(num_comments) as total_comments
    FROM posts
    GROUP BY DATE(created_utc), subreddit
    ORDER BY date DESC, avg_score DESC
""", conn)
print(trends)
```

---

## 🎯 Project 3: E-Commerce Sales Dashboard

**What you'll build:** End-to-end pipeline from CSV files to a live Metabase dashboard.

**Skills demonstrated:** Data modeling, transformation, BI tool integration

**Dataset:** Use the Kaggle Brazilian E-Commerce dataset (free, realistic)

```python
# Generate synthetic e-commerce data for the project
import faker
import pandas as pd
import numpy as np
from datetime import datetime, timedelta
import random

fake = faker.Faker()

def generate_orders(n: int = 10000) -> pd.DataFrame:
    return pd.DataFrame([{
        "order_id": f"ORD-{i:06d}",
        "customer_id": f"CUST-{random.randint(1, 2000):05d}",
        "product_id": f"PROD-{random.randint(1, 500):04d}",
        "order_date": fake.date_between(start_date="-1y", end_date="today"),
        "quantity": random.randint(1, 5),
        "unit_price": round(random.uniform(5, 500), 2),
        "discount_pct": random.choice([0, 0, 0, 0.05, 0.10, 0.15, 0.20]),
        "status": random.choices(
            ["delivered","processing","shipped","cancelled"],
            weights=[70, 10, 15, 5]
        )[0],
        "region": random.choice(["North", "South", "East", "West"]),
        "channel": random.choice(["web","mobile","store"])
    } for i in range(1, n+1)])

orders = generate_orders(50000)
orders.to_csv("orders.csv", index=False)
print(f"Generated {len(orders)} orders")
print(orders.head())
print(orders.describe())
```

---

*Next: [[05-Projects/Intermediate-Projects]] | Back to [[00-Index]]*
