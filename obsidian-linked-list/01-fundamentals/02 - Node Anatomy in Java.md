# 02 — Node Anatomy in Java

> **MOC:** [[Linked List MOC]] | **Prev:** [[01 - What is a Linked List]] | **Next:** [[03 - Types of Linked Lists]]

---

## 🏗️ The ListNode Class (Memorise This)

```java
// The universal definition used in LeetCode, HackerRank, etc.
class ListNode {
    int val;
    ListNode next;

    // Constructors
    ListNode() {}
    ListNode(int val) { this.val = val; }
    ListNode(int val, ListNode next) {
        this.val = val;
        this.next = next;
    }
}
```

> ✅ LeetCode provides this. But you **must** be able to write it from scratch in interviews.

---

## 🔁 Doubly Linked Node

```java
class DoublyNode {
    int val;
    DoublyNode prev;
    DoublyNode next;

    DoublyNode(int val) {
        this.val = val;
    }
}
```

---

## 🔧 Building a Linked List from Scratch

```java
// Method 1: Manual chaining
ListNode head = new ListNode(10);
head.next = new ListNode(20);
head.next.next = new ListNode(30);
// Result: 10 → 20 → 30 → null

// Method 2: Inline chaining (compact)
ListNode head = new ListNode(10, new ListNode(20, new ListNode(30, null)));

// Method 3: Helper method
public ListNode buildList(int[] arr) {
    ListNode dummy = new ListNode(0);
    ListNode curr = dummy;
    for (int val : arr) {
        curr.next = new ListNode(val);
        curr = curr.next;
    }
    return dummy.next;
}
```

---

## 🖨️ Printing / Traversing a Linked List

```java
// Standard traversal pattern — BURN THIS INTO MEMORY
public void printList(ListNode head) {
    ListNode curr = head;
    while (curr != null) {
        System.out.print(curr.val);
        if (curr.next != null) System.out.print(" → ");
        curr = curr.next;
    }
    System.out.println(" → null");
}
```

---

## 📏 Length of a Linked List

```java
public int length(ListNode head) {
    int count = 0;
    ListNode curr = head;
    while (curr != null) {
        count++;
        curr = curr.next;
    }
    return count;
}
```

---

## ⚠️ NullPointerException — The #1 Bug

```java
// WRONG: curr.next.val before null check
while (curr.next.val != target) { ... }  // 💥 NPE if curr.next is null

// CORRECT: Always check curr != null before accessing curr.val
//          Check curr.next != null before accessing curr.next
while (curr != null && curr.next != null) {
    // safe to access curr.val AND curr.next.val
}
```

---

## 🧪 Generic Linked List (Advanced)

```java
class GenericNode<T> {
    T val;
    GenericNode<T> next;

    GenericNode(T val) {
        this.val = val;
    }
}
```

---

## 🔗 Connected Notes
- [[01 - What is a Linked List]] — why nodes are structured this way
- [[03 - Types of Linked Lists]] — how node structure changes per type
- [[04 - Core Operations]] — using nodes to insert/delete
- [[10 - Java Code Templates]] — full implementation reference

---
`#DSA` `#LinkedList` `#Java` `#NodeStructure`
