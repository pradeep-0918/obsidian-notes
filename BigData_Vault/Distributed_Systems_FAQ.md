# 📂 Distributed Systems & File Systems — FAQ Notes

> **Method:** Feynman Technique — Explain it like you're teaching a 10-year-old, then go slightly deeper.

---

## 1. 🗂️ What is a File System?

**Simple:** Imagine your bedroom. Without a system, clothes are everywhere. A file system is the **organizer** — it decides where things go on your hard drive and how to find them later.

**Technically:** A file system manages how data is **stored and retrieved** on storage media.

> **Examples:** `NTFS` (Windows), `EXT` (Linux), `HDFS` (Hadoop), `S3` (Amazon)

**Tags:** #filesystem #storage #basics

---

## 2. 🧱 What is a Block?

**Simple:** Imagine a big book. Instead of storing the whole book in one place, you tear it into equal-sized pages and store each page separately. That's a block.

**Technically:** A block is the **smallest unit of storage** a file system reads/writes at once. Large files are split into fixed-size chunks.

| File System | Block Size |
|-------------|------------|
| NTFS        | 16 KB      |
| EXT         | 512 KB     |

**Tags:** #block #storage #filesystem 

---

## 3. 🤝 Client and Server?

**Simple:** Think of a restaurant. You (client) order food. The kitchen (server) prepares and delivers it.

**Technically:**
- **Client** → Sends a **request**
- **Server** → Processes and sends back a **response**

> The client always initiates. The server always listens.

**Tags:** #client #server #networking

---

## 4. 🌐 Types of File Systems

**Simple:** Some file systems live on one machine. Others spread data across many machines like a team.

**Technically:**

| Type            | Description                            | Examples          |
|-----------------|----------------------------------------|-------------------|
| **Standalone**  | Runs on a single machine               | NTFS, EXT         |
| **Distributed** | Data spread across multiple machines   | HDFS, S3, CFS     |

> Distributed file systems allow many machines to work on the same data simultaneously.

**Tags:** #filesystem #distributed #standalone

---

## 5. 🏗️ Types of Distributed Systems

**Simple:**
- **Master-Slave:** Like a boss (master) giving tasks to workers (slaves). Boss controls everything.
- **Peer-to-Peer:** Like a group project where everyone is equal — no one is the boss.

**Technically:**

| Type              | How it works                             | Example    |
|-------------------|------------------------------------------|------------|
| **Master-Slave**  | One node controls, others follow         | Hadoop     |
| **Peer-to-Peer**  | All nodes are equal, share responsibility | Cassandra  |

**Tags:** #distributed #master-slave #peer-to-peer

---

## 6. ⚙️ What is a Process?

**Simple:** A program is like a recipe written on paper. A process is what happens **when you actually cook it** — it's the recipe in action.

**Technically:** A **process** is a program currently being executed by the CPU, with its own memory space and resources.

> `Program (static) + Execution = Process (dynamic)`

**Tags:** #process #OS #execution

---

## 7. 👻 What is a Daemon Process?

**Simple:** Think of a security guard working silently at night — you don't see them, but they're always running in the background keeping things safe.

**Technically:** A **daemon** is a background process that runs continuously, waiting to handle tasks (e.g., web server, print spooler, scheduler).

> Daemons start at boot and run without user interaction.

**Tags:** #daemon #background-process #OS

---

## 8. 🖥️ Cluster and Node?

**Simple:**
- A **node** is one computer.
- A **cluster** is a team of computers working together.

**Technically:**

| Term        | Definition                                    |
|-------------|-----------------------------------------------|
| **Node**    | Individual physical or virtual machine         |
| **Cluster** | Group of nodes working as a unified system    |

> A cluster gives you more power, reliability, and storage than a single node.

**Tags:** #cluster #node #distributed

---

## 9. 🔌 What is a Client API?

**Simple:** Imagine a TV remote. You press a button and the TV responds — you don't need to know how the TV works inside. The remote is the API.

**Technically:** An **API (Application Programming Interface)** is a set of rules and functions that lets one software talk to another, without exposing internal details.

> **Client API** = the interface a client uses to communicate with a server or service.

**Tags:** #API #client #interface

---

## 🗺️ Concept Map

```
File System
├── Standalone (NTFS, EXT)
└── Distributed (HDFS, S3, CFS)
        ├── Master-Slave (Hadoop)
        └── Peer-to-Peer (Cassandra)

Storage Basics
├── Block = smallest storage unit
└── File System = manages blocks

Computing Basics
├── Process = program in execution
├── Daemon = background process
├── Client → Request → Server → Response
└── API = interface between systems

Infrastructure
├── Node = single machine
└── Cluster = group of nodes
```

---

*Notes made with ❤️ using Feynman Technique — understand it simply, then go deep.*
