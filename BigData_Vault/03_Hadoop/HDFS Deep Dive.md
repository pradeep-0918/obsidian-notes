# 💾 HDFS Deep Dive — Hadoop Distributed File System

#hadoop #hdfs #storage #feynman #distributed-systems

---

## 🍅 Pomodoro 5 — HDFS: How Data Is Stored

---

## 🧒 Feynman Explanation — HDFS as a Book Library

Imagine the world's largest book (1 million pages). No single bookshelf can hold it. So:

1. You **tear the book into chunks** — 128 pages each
2. You **store each chunk in 3 different libraries** (in case one burns down)
3. You keep a **master list** (in a special notebook) saying: "Chunk 1 is in Library A, B, and C. Chunk 2 is in Library D, E, and F..."

When someone wants to read page 500,000:
1. Ask the master notebook: "Which library has the chunk containing page 500,000?"
2. Go to that library and read it

**That's HDFS.**

---

## 🏗️ HDFS Architecture

```
                    ┌──────────────────┐
                    │   NameNode       │ ← The Master Notebook
                    │  (Master)        │   Knows WHERE data is
                    └────────┬─────────┘   (NOT what the data says)
                             │
            ┌────────────────┼────────────────┐
            │                │                │
     ┌──────┴──────┐  ┌──────┴──────┐  ┌──────┴──────┐
     │  DataNode 1 │  │  DataNode 2 │  │  DataNode 3 │
     │  (Machine A)│  │  (Machine B)│  │  (Machine C)│
     │  Block 1✓   │  │  Block 1✓   │  │  Block 2✓   │
     │  Block 3✓   │  │  Block 2✓   │  │  Block 3✓   │
     └─────────────┘  └─────────────┘  └─────────────┘
          Library A        Library B        Library C
```

---

## 🔑 HDFS Key Components -> Master Slave Architecture

### 1. NameNode (The Brain / Master)

- **What it does:** Stores metadata — the "table of contents" of where every block is
- **What it does NOT store:** The actual data
- **Single point of failure?** YES — which is why Hadoop 2.x introduced **Secondary NameNode** and **HA NameNode**

> 🧠 Think of NameNode as the **index page of a textbook** — it tells you which page to go to, but it doesn't have the content itself.

### 2. DataNode (The Workers / Storage)

- **What it does:** Actually stores the data blocks
- **How many?** Hundreds to thousands of DataNodes in production
- **Heartbeat:** Every DataNode sends a heartbeat to NameNode every **3 seconds** to say "I'm alive!"
- **If no heartbeat?** NameNode marks it as dead and triggers **replication** to maintain 3 copies

### 3. Secondary NameNode (The Backup Helper)

> ⚠️ **Common Interview Trap:** Secondary NameNode is NOT a backup NameNode!

| What People Think | What It Actually Does |
|------------------|----------------------|
| Backup NameNode ready to take over | Periodically merges the edit log with the FS image to reduce NameNode startup time |

It's more like a **janitor that tidies up the NameNode's files**, not a standby replacement.

---

## 📦 Data Blocks — The Core Concept

### Default Block Size

| Version     | Default Block Size |
| ----------- | ------------------ |
| Hadoop 1.x  | 64 MB              |
| Hadoop 2.x+ | **128 MB**         |

### Why So Large?

Normal file systems use 4KB blocks. HDFS uses 128MB blocks because:
- **Large files** dominate Big Data (logs, datasets)
- Fewer blocks = fewer entries in NameNode's metadata = faster lookups
- Reduces network overhead (one large transfer is more efficient than many small ones)

### Block Example

```
File: access_log.csv (500MB)
Block size: 128MB

Block 1: bytes 0–128MB       → DataNodes: A, B, C
Block 2: bytes 128MB–256MB   → DataNodes: D, E, A
Block 3: bytes 256MB–384MB   → DataNodes: B, F, G
Block 4: bytes 384MB–500MB   → DataNodes: C, D, H
```
32
---

## 🔁 Replication — Why 3 Copies?

### The Google Insight That Changed Everything

At the scale of 10,000 machines running 24/7:
- **Probability of one disk failure per day: ~100%**
- So assume failures will happen — design around it

### Replication Factor = 3 (Default)

```
Original → Copy 1: Same rack (for speed)
         → Copy 2: Different rack, same data center (rack failure protection)
         → Copy 3: Different data center (disaster protection)
```

### Rack Awareness

HDFS is "rack-aware" — it knows which machines are in the same physical rack. Why? Because if a rack loses power, you don't want ALL 3 copies to go down.

```
Rack 1: [Machine A] [Machine B]
Rack 2: [Machine C] [Machine D]

Smart placement: Copy 1 → Machine A (Rack 1)
                 Copy 2 → Machine B (Rack 1) — close for fast writes
                 Copy 3 → Machine C (Rack 2) — different rack for safety
```

---

## 📝 Write and Read Operations

### Writing a File

```
Step 1: Client contacts NameNode: "I want to write a 500MB file"
Step 2: NameNode returns list of DataNodes for each block
Step 3: Client writes Block 1 to DataNode A
Step 4: DataNode A PIPELINES the block to DataNode B
Step 5: DataNode B PIPELINES the block to DataNode C
Step 6: Acknowledgment travels back: C → B → A → Client
Step 7: Client writes Block 2... (repeat)
Step 8: Client sends "write complete" to NameNode
```

### Reading a File

```
Step 1: Client asks NameNode: "Where is file X?"
Step 2: NameNode returns block locations
Step 3: Client contacts NEAREST DataNode for each block
Step 4: Data streams directly from DataNode to Client
(NameNode is NOT involved in the actual data transfer!)
```

---

## ⚡ HDFS vs Traditional File Systems

| Feature | Traditional (ext4, NTFS) | HDFS |
|---------|--------------------------|------|
| **Designed for** | Single machine | Thousands of machines |
| **File size** | KBs to GBs | GBs to PBs |
| **Block size** | 4 KB | 128 MB |
| **Fault tolerance** | No | Yes (3x replication) |
| **Random writes** | Yes | NO (append-only) |
| **Access pattern** | Random | Sequential reads |
| **Best for** | OS files, small files | Large logs, datasets |

> 🔑 **HDFS does NOT support random writes.** Once a file is written, you can only **append** to it or **delete** it. This is why HDFS is perfect for log files and batch data, but bad for databases.



## 🚫 HDFS Limitations

1. **Bad for small files** — Millions of tiny files overwhelm the NameNode's memory
2. **High latency** — Not designed for real-time queries (use HBase or Kudu instead)
3. **No random write support** — Append-only model
4. **NameNode is a bottleneck** — Single point of metadata management (mitigated by HDFS Federation in Hadoop 3.x)

---

## 🔗 Connected Notes

- [[MapReduce Explained]] — How data stored in HDFS gets processed
- [[YARN Explained]] — How resources are managed during processing
- [[History of Big Data]] — GFS, which inspired HDFS
- [[CAP Theorem]] — Why HDFS prioritizes consistency and partition tolerance
- [[Top 30 Interview QA]] — HDFS interview questions

---

## 🧠 Quick Recall Check

1. What does the NameNode store? What does it NOT store?
2. What is the default block size in Hadoop 2.x?
3. Why does HDFS use 3 replicas?
4. What is the common misconception about Secondary NameNode?
5. Why is HDFS bad for small files?
6. Can you randomly update a record in an HDFS file?
