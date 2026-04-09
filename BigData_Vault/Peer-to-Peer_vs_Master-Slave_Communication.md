# 📡 Peer-to-Peer Communication

**Tags:** #networking #distributed-systems #decentralized #architecture  
**Created:** 2026-04-09  
**Links:** [[Master-Slave Communication]] · [[CAP Theorem]] · [[DHT - Distributed Hash Table]] · [[Raft Consensus]]

---

## 📝 Definition

> A communication model where every node (peer) acts as both a **client** and a **server** simultaneously. No central authority — nodes communicate directly with each other on **equal footing**.

---

## 🔑 Key Properties

| Property | Value |
|---|---|
| **Topology** | Decentralized / distributed mesh |
| **Roles** | Every node = client + server (equal) |
| **Control** | No single point of control |
| **Scalability** | Scales horizontally as peers join |
| **Failure Mode** | Partial — system survives node loss |
| **Discovery** | DHT, bootstrapping, trackers |

---

## ⚡ How It Works

1. Node **joins** the network → discovers peers via bootstrapping or DHT
2. Establishes **direct connections** to multiple peers
3. Simultaneously *requests* resources AND *serves* resources to others
4. If a peer leaves, traffic **routes around it** automatically

---

## ✅ Pros & ❌ Cons

### ✅ Strengths
- No single point of failure
- Highly scalable
- No central server cost
- Censorship resistant
- Bandwidth distributes across peers

### ❌ Weaknesses
- Hard to enforce consistency
- Security / trust issues between unknown peers
- Complex to implement
- Unpredictable latency
- Difficult to manage / audit

---

## 🏗️ Real-World Use Cases

| System | Use Case |
|---|---|
| **BitTorrent** | File sharing across peers |
| **Bitcoin / Ethereum** | Blockchain nodes |
| **WebRTC** | Browser-to-browser video/audio calls |
| **IPFS** | Decentralized file storage |
| **P2P Gaming** | Direct player-to-player connections |

---

## 💡 Key Insight

> [!tip] Remember
> P2P systems gain resilience by removing the center — but this makes *coordination* the hard problem. Consensus algorithms (Raft, PBFT) exist to solve exactly this.

---

## 🔗 Linked Notes

- [[Master-Slave Communication]] → opposite paradigm
- [[CAP Theorem]] → consistency vs availability tradeoffs
- [[DHT - Distributed Hash Table]] → how peer discovery works
- [[Raft Consensus]] → consensus in distributed systems

---
---

# 🖥️ Master-Slave Communication

**Tags:** #networking #centralized #architecture #databases  
**Created:** 2026-04-09  
**Links:** [[Peer-to-Peer Communication]] · [[Raft Consensus]] · [[Database Replication Patterns]]

---

## 📝 Definition

> A hierarchical communication model where one node — the **master** — controls one or more **slave** nodes. The master issues commands; slaves execute and report back. Control flows **top-down**.

---

## 🔑 Key Properties

| Property | Value |
|---|---|
| **Topology** | Centralized / hierarchical star |
| **Roles** | Master = controller · Slave = worker |
| **Control** | Single master has full authority |
| **Scalability** | Add slaves for workload distribution |
| **Failure Mode** | Critical — master failure halts system |
| **Sync** | Master pushes state to replicas |

---

## ⚡ How It Works

1. **Master** receives all write / control requests
2. Master processes the request and updates its own state
3. Master **replicates** changes to all connected slaves
4. **Slaves** handle read requests / execute delegated tasks
5. Slaves **report** results or status back to master

---

## ⚠️ Single Point of Failure

> [!danger] Critical Risk
> If the master node crashes, the entire system stops accepting writes. Solutions:
> - **Master election** (Raft, ZooKeeper)
> - **Hot standby** masters
> - **Automatic slave promotion**

---

## ✅ Pros & ❌ Cons

### ✅ Strengths
- Simple, predictable control flow
- Strong consistency (master = source of truth)
- Easy to reason about and debug
- Low coordination overhead
- Clear authority and audit trail

### ❌ Weaknesses
- Single point of failure (master)
- Master becomes a bottleneck under load
- Poor fault tolerance without failover
- Slaves are passive / underutilized
- Does not self-organize

---

## 🏗️ Real-World Use Cases

| System | Use Case |
|---|---|
| **MySQL / PostgreSQL** | Primary-replica DB replication |
| **I²C / SPI** | Embedded hardware communication buses |
| **Kubernetes** | Control plane → worker nodes |
| **MapReduce** | Job tracker → task trackers |
| **SCADA / PLC** | Industrial control systems |

---

## 💡 Key Insight

> [!tip] Remember
> Modern systems often use master-slave for *consistency* (all writes go one place) while adding *failover* via leader election — getting the simplicity of centralization with less brittleness.

---

## 🔗 Linked Notes

- [[Peer-to-Peer Communication]] → decentralized alternative
- [[Raft Consensus]] → master election algorithm
- [[Database Replication Patterns]] → replication strategies
- [[CAP Theorem]] → consistency vs availability

---
---

# ⚖️ Comparison: P2P vs Master-Slave

**Tags:** #comparison #networking #architecture  
**Created:** 2026-04-09  
**Links:** [[Peer-to-Peer Communication]] · [[Master-Slave Communication]]

---

## 📊 Side-by-Side Comparison

| Attribute | 📡 Peer-to-Peer | 🖥️ Master-Slave |
|---|---|---|
| **Authority** | Equal — no hierarchy | Hierarchical — master controls |
| **Fault Tolerance** | High — peers route around failure | Low — master failure is critical |
| **Consistency** | Eventual (hard to guarantee) | Strong (master is source of truth) |
| **Scalability** | Very high — peers add capacity | Limited by master throughput |
| **Complexity** | High — discovery, sync, trust | Low — simple request/response |
| **Latency** | Variable — depends on peer proximity | Predictable — single hop to master |
| **Security** | Harder — no trusted authority | Easier — master enforces policy |
| **Cost** | Low — no central infra needed | Higher — master infra must be robust |
| **Best For** | File sharing, blockchain, media | DB replication, embedded, job scheduling |

---

## 🧠 Decision Guide

> [!tip] Use **Peer-to-Peer** when...
> - You need resilience with no central infrastructure
> - Censorship resistance is a requirement
> - Load should be shared across all nodes
> - Examples: torrenting, blockchain, WebRTC

> [!warning] Use **Master-Slave** when...
> - You need strong consistency
> - Centralized control and predictability matter
> - A single authoritative source of truth is required
> - Examples: primary DB, hardware buses, orchestration

---

## 🗺️ Quick Mental Model

```
P2P:
  [Node A] ←→ [Node B]
     ↕              ↕
  [Node C] ←→ [Node D]
  (everyone talks to everyone — equal)

Master-Slave:
        [Master]
       /    |    \
  [Slave] [Slave] [Slave]
  (one controls, many obey)
```

---

## 🔗 Linked Notes

- [[Peer-to-Peer Communication]]
- [[Master-Slave Communication]]
- [[CAP Theorem]] — consistency vs availability
- [[Raft Consensus]] — leader election in distributed systems
- [[Database Replication Patterns]]
