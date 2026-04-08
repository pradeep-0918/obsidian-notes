# ⚖️ CAP Theorem — The Fundamental Trade-off

#distributed-systems #cap-theorem #concepts #interview

---

## 🍅 Pomodoro 8 — CAP: The Triangle You Can't Have All Of

---

## 🧒 Feynman Explanation — The Bank Branch Analogy

Imagine a bank with **two branches** in Chennai and Delhi that share the same accounts.

**Scenario:** You deposit ₹10,000 at Chennai branch at 10 AM.

Now three things could go wrong:

1. **Delhi branch immediately shows your balance as updated** → This is **Consistency** (both branches agree)
2. **The bank is always open, even if the network between branches is down** → This is **Availability**
3. **Even if the Chennai-Delhi phone line cuts off** → This is the **Partition** (network split)

The **CAP Theorem** says: **Pick any 2. You cannot guarantee all 3 simultaneously.**

---

## 📐 The CAP Triangle

```
                   Consistency
                       /\
                      /  \
                     /    \
                    /      \
                   /  ????  \
                  /          \
                 /____________\
      Availability            Partition Tolerance
```

You can only fully guarantee **two** vertices at once.

---

## 🔑 Definitions

### C — Consistency
> Every read receives the **most recent write** or an error.

Every node in the distributed system sees the same data at the same time. No stale reads.

### A — Availability
> Every request receives a **response** (not necessarily the most recent data).

The system is always up and responding, even if it might return slightly outdated data.

### P — Partition Tolerance
> The system continues operating even when **network messages are lost** or delayed between nodes.

Network partitions (one cluster of machines can't talk to another) WILL happen in production. This is non-negotiable in distributed systems.

---

## 🎯 The Real Implication

> **Because network partitions ALWAYS happen at scale, you MUST choose P.**

So the real choice is: **CP or AP** (P is always required).

```
CP (Consistency + Partition Tolerance)
→ When network splits, refuse to serve stale data
→ Returns error instead of wrong data
→ Examples: HBase, MongoDB (with certain settings), Zookeeper

AP (Availability + Partition Tolerance)
→ When network splits, still serve data (even if slightly stale)
→ Eventually consistent
→ Examples: Cassandra, CouchDB, Amazon DynamoDB
```

---

## 📊 Where Does HDFS Fit?

HDFS is a **CP system**:
- If NameNode goes down, reads/writes fail (not available)
- But when it IS available, all clients see the same data
- Prioritizes correctness over always-being-up

---

## 💡 Extended: PACELC (Beyond CAP)

CAP only applies during a partition. **PACELC** extends this:

> Even when there's **no partition (E)**, there's still a trade-off between **Latency (L)** and **Consistency (C)**.

```
If Partition:  Choose A or C  (CAP)
Else:          Choose L or C  (PACELC extension)
```

| System | Partition | Else |
|--------|-----------|------|
| MySQL | C | C (consistent, higher latency) |
| Cassandra | A | L (available, lower latency, eventual consistency) |
| DynamoDB | A | L |
| HBase | C | C |

---

## 🔗 Connected Notes

- [[Data Partitioning]] — How data is split across nodes
- [[HDFS Deep Dive]] — How HDFS handles consistency
- [[Top 30 Interview QA]] — CAP theorem interview questions
