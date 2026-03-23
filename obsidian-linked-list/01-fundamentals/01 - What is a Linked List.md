# 01 — What is a Linked List?

> **MOC:** [[Linked List MOC]] | **Next:** [[02 - Node Anatomy in Java]]

---

## 🧠 Core Mental Model

A linked list is a **chain of nodes** where each node holds:
1. A **value** (data)
2. A **pointer** to the next node (or null if it's the last)

Unlike arrays, nodes are **scattered in memory** — they are NOT stored contiguously. This single fact explains everything about why linked lists win on insertions/deletions and lose on random access.

```
Memory layout (Array):
[10][20][30][40]  ← contiguous, index math works

Memory layout (Linked List):
[10|0xA2] → [20|0xF3] → [30|null]   ← scattered, must follow pointers
```

---

## 🤔 When to Use Linked List vs Array

| Use Linked List when... | Use Array when... |
|------------------------|-------------------|
| Frequent insertions/deletions at head/middle | You need random access (arr[i]) |
| Size is unknown / dynamic | Cache performance matters |
| Implementing Stack, Queue, Deque | Binary search needed |
| You need O(1) insert with a reference | Memory locality is critical |

---

## 🧩 Real-World Analogies

- **Browser history** → doubly linked list (back/forward navigation)
- **Music playlist** → circular linked list (repeat mode)
- **Undo/Redo** → doubly linked list
- **Hash chaining** → singly linked list (collision resolution)
- **OS process scheduling** → circular linked list

---

## 🎯 Linked List in Java Collections

```java
// Java's built-in LinkedList implements both List and Deque
LinkedList<Integer> list = new LinkedList<>();

// As a Queue
list.offer(10);        // enqueue
list.poll();           // dequeue

// As a Stack
list.push(10);         // push to front
list.pop();            // pop from front

// As a Deque
list.addFirst(10);
list.addLast(20);
list.peekFirst();
list.peekLast();
```

> ⚠️ In interviews, you almost always implement your **own** linked list from scratch. Don't rely on `java.util.LinkedList` for DSA problems.

---

## 🔑 Key Insight (Google-Level Thinking)

> **The "dummy node" trick** is used in 80% of linked list problems. Always start by asking: *"Should I use a dummy head here?"*

A dummy (sentinel) node is a fake head node you create before the real list. It eliminates null checks at the head and makes edge cases disappear.

```java
ListNode dummy = new ListNode(0);
dummy.next = head;
// Now all operations are uniform — no special case for head
```

---

## 🔗 Connected Notes
- [[02 - Node Anatomy in Java]] — how to build a node in Java
- [[03 - Types of Linked Lists]] — singly, doubly, circular
- [[12 - Pro Tips and Gotchas]] — dummy node deep dive

---
`#DSA` `#LinkedList` `#Fundamentals` `#Java`
