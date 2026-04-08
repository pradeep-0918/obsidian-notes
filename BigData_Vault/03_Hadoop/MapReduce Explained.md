# ⚙️ MapReduce Explained — Divide, Conquer, Combine

#hadoop #mapreduce #processing #feynman

---

## 🍅 Pomodoro 6 — MapReduce: How Data Gets Processed

---

## 🧒 Feynman Explanation — The Election Counting Analogy

Imagine counting votes in a national election with 1 billion ballots.

**The old way (one computer):**
One person reads all 1 billion ballots. Takes forever.

**The MapReduce way:**
1. Split ballots into 10,000 boxes (one per polling station)
2. **MAP:** Assign 10,000 counters, one per box. Each counter reports: `{CandidateA: 47000, CandidateB: 53000}`
3. **SHUFFLE:** Group all results by candidate name across all counters
4. **REDUCE:** Add up all the counts for each candidate → Final result

```
Box 1 counter: {A: 47000, B: 53000}
Box 2 counter: {A: 62000, B: 38000}
Box 3 counter: {A: 29000, B: 71000}
         ↓ SHUFFLE (group by name) ↓
{A: [47000, 62000, 29000], B: [53000, 38000, 71000]}
         ↓ REDUCE ↓
{A: 138000, B: 162000}
Winner: Candidate B!
```

**That's MapReduce.**

---

## 🔄 The 4 Phases of MapReduce

```
Input Data
    ↓
[1. SPLIT]     → Break input into chunks (one per mapper)
    ↓
[2. MAP]       → Each mapper processes its chunk independently
    ↓                (key, value) → list of (key, value) pairs
[3. SHUFFLE    → Group intermediate results by key
   & SORT]        Sort within each group
    ↓
[4. REDUCE]    → Combine/aggregate values for each key
    ↓
Output Data
```

---

## 📝 Classic Example — Word Count

**Problem:** Count how many times each word appears in 1 billion web pages.

### Input
```
Page 1: "big data is big"
Page 2: "hadoop is great"
Page 3: "big data hadoop"
```

### Step 1: MAP Phase

Each mapper reads its page and emits `(word, 1)` for each word:

```
Mapper 1 (Page 1):
  (big, 1), (data, 1), (is, 1), (big, 1)

Mapper 2 (Page 2):
  (hadoop, 1), (is, 1), (great, 1)

Mapper 3 (Page 3):
  (big, 1), (data, 1), (hadoop, 1)
```

### Step 2: SHUFFLE & SORT

Results are grouped by key:
```
big:    [1, 1, 1]
data:   [1, 1]
hadoop: [1, 1]
is:     [1, 1]
great:  [1]
```

### Step 3: REDUCE Phase

Each reducer sums up the values for its key:
```
big:    3
data:   2
hadoop: 2
is:     2
great:  1
```

### Final Output
```
big    3
data   2
hadoop 2
is     2
great  1
```

---

## 🧩 MapReduce in Code (Pseudo-code)

```python
# MAP FUNCTION
def map(key, value):
    # key = document name
    # value = document content
    for word in value.split():
        emit(word, 1)

# REDUCE FUNCTION
def reduce(key, values):
    # key = word
    # values = list of 1s
    total = sum(values)
    emit(key, total)
```

---

## 🏗️ MapReduce Architecture

```
┌──────────────────────────────────────────────┐
│                  Client                      │
│           (submits MR job)                   │
└──────────────────┬───────────────────────────┘
                   │
          ┌────────▼────────┐
          │   JobTracker    │  (Hadoop 1.x)
          │  (or YARN RM)   │  ← Master: tracks job progress
          └────────┬────────┘
                   │
      ┌────────────┼────────────┐
      │            │            │
┌─────▼────┐ ┌─────▼────┐ ┌────▼─────┐
│TaskTracker│ │TaskTracker│ │TaskTracker│
│  Node 1  │ │  Node 2  │ │  Node 3  │
│ [Mapper] │ │ [Mapper] │ │[Reducer] │
└──────────┘ └──────────┘ └──────────┘
```

---

## ⏱️ MapReduce — The Timing Problem

MapReduce reads and writes to disk **between every step**:

```
Read HDFS → Map → Write to disk → Shuffle → Read from disk → Reduce → Write HDFS
```

This disk I/O is the reason MapReduce is slow — and why **Spark** (which keeps data in RAM) is much faster.

### MapReduce vs Spark Speed

| Operation | MapReduce | Spark |
|-----------|-----------|-------|
| **Storage during processing** | Disk (HDFS) | RAM |
| **Speed** | Slower | 10-100x faster |
| **Iterative algorithms** | Very slow (ML needs many passes) | Fast |
| **Fault tolerance** | Recompute from HDFS | Lineage-based recomputation |

---

## 🔧 MapReduce Optimizations

### 1. Combiner (Mini-Reducer)

A Combiner runs **locally on each mapper** before the shuffle, reducing network traffic.

```
Without combiner: Mapper 1 sends (big,1), (big,1), (big,1) to Reducer
With combiner:    Mapper 1 locally combines → sends (big,3) to Reducer
```

> Combiner = pre-aggregation on the map side. Reduces data transferred across the network.

### 2. Partitioner

Controls which reducer gets which key. Default: hash(key) % numReducers.

Custom partitioner example: Route all keys starting with A-M to Reducer 1, N-Z to Reducer 2.

### 3. Number of Mappers

- Automatically determined by number of HDFS blocks
- 1 mapper per 128MB block = parallelism matches storage

---

## 🚫 When NOT to Use MapReduce

| Use Case | Why MapReduce is Bad | Better Tool |
|----------|---------------------|-------------|
| Real-time processing | Too slow (batch only) | Apache Kafka + Flink |
| Interactive SQL queries | Minutes for simple queries | Apache Hive with Tez, Presto |
| Machine Learning (iterative) | Disk reads per iteration kill performance | Spark MLlib |
| Graph processing | Not designed for graphs | Apache Giraph, Spark GraphX |

---

## 🔗 Connected Notes

- [[HDFS Deep Dive]] — Where MapReduce reads its input from
- [[YARN Explained]] — What replaced JobTracker/TaskTracker in Hadoop 2.x
- [[Hadoop Origin Story]] — Why MapReduce was invented
- [[Top 30 Interview QA]] — MapReduce interview questions

---

## 🧠 Quick Recall Check

1. What are the 4 phases of MapReduce?
2. What does a Combiner do and why does it help?
3. Why is MapReduce slower than Spark?
4. In the word count example, what does the Mapper output?
5. How many mappers run for a 512MB file with 128MB block size?
