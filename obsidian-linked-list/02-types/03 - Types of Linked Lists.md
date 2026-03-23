# 03 — Types of Linked Lists

> **MOC:** [[Linked List MOC]] | **Prev:** [[02 - Node Anatomy in Java]] | **Next:** [[04 - Core Operations]]

---

## 📋 Overview

| Type | Direction | Tail Points To | Use Case |
|------|-----------|---------------|---------|
| Singly Linked | One-way | null | Stack, Queue |
| Doubly Linked | Both ways | null or head | Browser history, LRU Cache |
| Circular Singly | One-way | head | Round-robin scheduler |
| Circular Doubly | Both ways | head | Josephus problem, deque |

---

## 1️⃣ Singly Linked List

```
head → [10|•] → [20|•] → [30|null]
```

```java
class SinglyLinkedList {
    ListNode head;

    // Insert at head — O(1)
    public void insertHead(int val) {
        ListNode node = new ListNode(val);
        node.next = head;
        head = node;
    }

    // Insert at tail — O(n)
    public void insertTail(int val) {
        ListNode node = new ListNode(val);
        if (head == null) { head = node; return; }
        ListNode curr = head;
        while (curr.next != null) curr = curr.next;
        curr.next = node;
    }

    // Delete by value — O(n)
    public void delete(int val) {
        if (head == null) return;
        if (head.val == val) { head = head.next; return; }

        ListNode curr = head;
        while (curr.next != null) {
            if (curr.next.val == val) {
                curr.next = curr.next.next;  // bypass the node
                return;
            }
            curr = curr.next;
        }
    }
}
```

---

## 2️⃣ Doubly Linked List

```
null ← [10|•↔•] ↔ [20|•↔•] ↔ [30|•] → null
```

```java
class DoublyLinkedList {
    DoublyNode head, tail;

    // Insert at head — O(1)
    public void insertHead(int val) {
        DoublyNode node = new DoublyNode(val);
        if (head == null) {
            head = tail = node;
            return;
        }
        node.next = head;
        head.prev = node;
        head = node;
    }

    // Insert at tail — O(1) because we keep tail pointer!
    public void insertTail(int val) {
        DoublyNode node = new DoublyNode(val);
        if (tail == null) {
            head = tail = node;
            return;
        }
        tail.next = node;
        node.prev = tail;
        tail = node;
    }

    // Delete a node — O(1) given the node reference
    public void delete(DoublyNode node) {
        if (node.prev != null) node.prev.next = node.next;
        else head = node.next;                  // deleting head

        if (node.next != null) node.next.prev = node.prev;
        else tail = node.prev;                  // deleting tail
    }
}
```

> 💡 **LRU Cache** uses a HashMap + Doubly Linked List. The DLL gives O(1) remove + O(1) insertHead.

---

## 3️⃣ Circular Singly Linked List

```
head → [10|•] → [20|•] → [30|•] ↩ (points back to head)
```

```java
class CircularLinkedList {
    ListNode head;

    // Check if a node in this circular list
    public boolean contains(int val) {
        if (head == null) return false;
        ListNode curr = head;
        do {
            if (curr.val == val) return true;
            curr = curr.next;
        } while (curr != head);   // ← KEY: stop when we loop back
        return false;
    }

    // Insert at end of circular list
    public void insert(int val) {
        ListNode node = new ListNode(val);
        if (head == null) {
            head = node;
            node.next = head;     // points to itself
            return;
        }
        ListNode curr = head;
        while (curr.next != head) curr = curr.next; // find last node
        curr.next = node;
        node.next = head;
    }
}
```

> ⚠️ **Infinite loop trap**: Never use `while (curr != null)` for circular lists — null never comes! Use `do-while` with `curr != head`.

---

## 4️⃣ Circular Doubly Linked List

```java
// Used internally by Java's LinkedList class!
// Perfect for LRU Cache and deque-based problems

class CircularDoublyNode {
    int val;
    CircularDoublyNode prev, next;

    CircularDoublyNode(int val) {
        this.val = val;
        this.prev = this;   // initially points to itself
        this.next = this;
    }
}
```

---

## 🔍 Identifying Which Type to Use in a Problem

```
Is order important AND you need to go backwards?
    YES → Doubly Linked List

Is the list endless / round-robin behavior needed?
    YES → Circular

Do you only traverse forward and need minimal memory?
    YES → Singly Linked List

Do you need O(1) insertions AND O(1) removals with node reference?
    YES → Doubly Linked List (e.g., LRU Cache)
```

---

## 🔗 Connected Notes
- [[02 - Node Anatomy in Java]] — node class for each type
- [[04 - Core Operations]] — operations on each type
- [[07 - Cycle Detection]] — circular list = intentional cycle
- [[12 - Pro Tips and Gotchas]] — common circular list bugs

---
`#DSA` `#LinkedList` `#Types` `#Java`
