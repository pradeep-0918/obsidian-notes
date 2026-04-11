# ⚡ Real-Time Architecture — System Design
#design #realtime #streaming #lambda #kappa

> Designing systems that process data in seconds, not hours.

---

## 🧠 When Do You Actually Need Real-Time?

First, challenge the assumption. Real-time is expensive and complex.

```
Ask: What decision changes if data is 1 minute late vs 1 hour late?

MUST be real-time:
- Fraud detection (transaction already processed if delayed)
- Stock trading (prices change in milliseconds)
- Emergency alerts (lives at stake)
- Live leaderboards/dashboards

CAN be near-real-time (5-15 minutes):
- E-commerce recommendation refresh
- Inventory warning alerts
- Customer support priority scoring

Batch is fine:
- Monthly revenue reports
- Annual customer analysis
- Historical trend analysis
```

---

## 🏗️ Architecture Patterns

### Lambda Architecture

```
                     ┌──────────────────────────────────────┐
                     │          Speed Layer                   │
                     │  Kafka → Flink → Redis/ES             │
Raw Events ─────────►│  Latency: < 1 second                  │──►  Serving Layer
(from Kafka)    │    │  Accuracy: Approximate                │    (merge results)
                │    └──────────────────────────────────────┘         │
                │                                                      │
                └───►┌──────────────────────────────────────┐         │
                     │          Batch Layer                   │─────────┘
                     │  S3 → Spark → Data Warehouse          │
                     │  Latency: Hours                        │
                     │  Accuracy: Exact                       │
                     └──────────────────────────────────────┘

Pros: Best of both worlds
Cons: Two codebases to maintain, eventual consistency is tricky
```

### Kappa Architecture

```
Raw Events ──► Kafka (long retention) ──► Flink ──► Output Store
                        │
                        └─ Replay entire history for reprocessing
                           (when you need to fix a bug or add new logic)

Pros: One codebase, simpler operations
Cons: Replay can be slow/expensive, Kafka retention costs money
```

### Modern Streaming-First Architecture

```
┌──────────────────────────────────────────────────────────────────┐
│                    MODERN REAL-TIME PLATFORM                      │
│                                                                  │
│  Sources          Ingestion          Processing       Serving    │
│  ────────        ──────────         ──────────       ────────    │
│                                                                  │
│  App events ──► Kafka ──────────► Flink ────────► Redis         │
│  DB changes ──► (Debezium CDC)  │  (Stateful)  │  (hot cache)  │
│  API calls ───► Kafka ──────────┘              │                │
│                                                ▼                │
│                 Kafka ──────────► Spark  ────► Delta Lake       │
│                 (S3 backup)      (Batch)       (cold store)      │
│                                                │                │
│                                                ▼                │
│                                           Snowflake             │
│                                           (analytics)           │
└──────────────────────────────────────────────────────────────────┘
```

---

## 🔧 Real-Time Use Case: Fraud Detection System

```
Design requirements:
- Process 10,000 transactions/second
- Flag suspicious transactions in < 500ms
- 99.99% accuracy (false positives are costly)
- Zero transaction loss

Architecture:
┌──────────────────────────────────────────────────────────────┐
│              FRAUD DETECTION PIPELINE                         │
│                                                              │
│  Transaction ──► Kafka ──► Flink ──► Rules Engine ──► Alert │
│  Service         Topic     (100ms)   + ML Scoring   Kafka   │
│                               │                             │
│                               ▼                             │
│                         Feature Store                        │
│                         (Redis - 1ms)                        │
│                         - Customer velocity                  │
│                         - Device fingerprint                 │
│                         - Location history                   │
│                               │                             │
│                         Batch ML Training                    │
│                         (Spark, daily)                       │
│                         Updates model in Feature Store       │
└──────────────────────────────────────────────────────────────┘
```

```python
# Flink fraud detection implementation
from pyflink.datastream import StreamExecutionEnvironment
from pyflink.datastream.functions import KeyedProcessFunction
from pyflink.datastream.state import ValueStateDescriptor, MapStateDescriptor
from pyflink.common.typeinfo import Types
import json
import redis

r = redis.Redis(host='redis', port=6379)

class FraudDetectionProcessor(KeyedProcessFunction):
    """
    Stateful fraud detection with both real-time rules
    and ML model scoring.
    """
    
    def open(self, runtime_context):
        # Flink state: transaction count in last 60 seconds
        self.txn_count_60s = runtime_context.get_state(
            ValueStateDescriptor("txn_count_60s", Types.INT())
        )
        # Flink state: total amount in last 60 seconds
        self.total_amount_60s = runtime_context.get_state(
            ValueStateDescriptor("total_amount_60s", Types.DOUBLE())
        )
        # Timer cleanup
        self.window_start = runtime_context.get_state(
            ValueStateDescriptor("window_start", Types.LONG())
        )
    
    def process_element(self, txn, ctx, out):
        current_time = ctx.timestamp()
        
        # Get customer features from Redis (pre-computed)
        customer_features = self._get_customer_features(txn['customer_id'])
        
        # Update real-time state
        count = self.txn_count_60s.value() or 0
        amount = self.total_amount_60s.value() or 0.0
        
        count += 1
        amount += txn['amount']
        
        self.txn_count_60s.update(count)
        self.total_amount_60s.update(amount)
        
        # ── RULE-BASED CHECKS ───────────────────────────────
        alerts = []
        
        # Rule 1: High frequency
        if count > 10:
            alerts.append({
                'rule': 'HIGH_FREQUENCY',
                'severity': 'HIGH',
                'value': count,
                'threshold': 10
            })
        
        # Rule 2: Large single transaction
        if txn['amount'] > 10000:
            avg_txn = customer_features.get('avg_transaction', 100)
            if txn['amount'] > avg_txn * 20:
                alerts.append({
                    'rule': 'AMOUNT_SPIKE',
                    'severity': 'CRITICAL',
                    'value': txn['amount'],
                    'avg': avg_txn
                })
        
        # Rule 3: New country
        known_countries = customer_features.get('known_countries', [])
        if txn.get('country') and txn['country'] not in known_countries:
            alerts.append({
                'rule': 'NEW_COUNTRY',
                'severity': 'MEDIUM',
                'country': txn['country']
            })
        
        # ── ML SCORING ──────────────────────────────────────
        ml_score = self._get_ml_score(txn, customer_features, count, amount)
        
        is_fraud = len(alerts) > 0 or ml_score > 0.85
        
        # ── OUTPUT ──────────────────────────────────────────
        result = {
            **txn,
            'is_flagged': is_fraud,
            'alerts': alerts,
            'ml_fraud_score': ml_score,
            'processed_at': current_time
        }
        
        out.collect(result)
        
        if is_fraud:
            # Side output for immediate action
            ctx.output(fraud_alert_tag, result)
        
        # Register timer to reset window (60 seconds from now)
        ctx.timer_service().register_event_time_timer(current_time + 60_000)
    
    def on_timer(self, timestamp, ctx, out):
        """Reset counters periodically."""
        self.txn_count_60s.update(0)
        self.total_amount_60s.update(0.0)
    
    def _get_customer_features(self, customer_id: str) -> dict:
        """Fetch pre-computed features from Redis."""
        key = f"customer:features:{customer_id}"
        data = r.get(key)
        return json.loads(data) if data else {}
    
    def _get_ml_score(self, txn, features, count, amount) -> float:
        """Score transaction with ML model (loaded from Redis/feature store)."""
        # In production: load model from Redis or call a sidecar service
        # Simplified scoring for illustration
        score = 0.0
        
        if count > 5:
            score += 0.3
        if txn['amount'] > features.get('avg_transaction', 100) * 5:
            score += 0.4
        if txn.get('is_international'):
            score += 0.2
        if txn.get('device_new'):
            score += 0.1
        
        return min(score, 1.0)
```

---

## 📊 Real-Time Metrics Dashboard Architecture

```
Browser ──WebSocket──► Dashboard Server ──► Redis Pub/Sub
                                                  ▲
Flink ──────────────► Redis ────────────────────────
(computes           (stores latest metrics,
metrics in           publishes on change)
5-second windows)
```

```python
# FastAPI WebSocket endpoint for real-time dashboard
from fastapi import FastAPI, WebSocket
import redis.asyncio as aioredis
import json

app = FastAPI()

@app.websocket("/ws/dashboard")
async def dashboard_ws(websocket: WebSocket):
    await websocket.accept()
    
    redis = aioredis.from_url("redis://redis:6379")
    pubsub = redis.pubsub()
    await pubsub.subscribe("metrics:realtime")
    
    try:
        async for message in pubsub.listen():
            if message["type"] == "message":
                await websocket.send_text(message["data"])
    except Exception:
        await websocket.close()
    finally:
        await pubsub.unsubscribe()
```

---

## 🔑 Real-Time Architecture Principles

| Principle | Why | How |
|---|---|---|
| Backpressure | Don't overwhelm downstream | Kafka as buffer |
| Idempotency | Safe replay | Use idempotent sinks |
| Exactly-once | Accurate counts | Flink + transactional sinks |
| Partial results early | Better UX | Emit before window closes |
| Dead letter queues | Handle bad data | Route errors aside |
| Circuit breakers | Cascade failure prevention | Fail fast, not slow |
| Checkpointing | Survive failures | Flink checkpoint to S3 |

---

*Related: [[03-Tools/Flink]] | [[03-Tools/Kafka]] | [[02-Core-Concepts/Stream-Processing]] | Back to [[00-Index]]*
