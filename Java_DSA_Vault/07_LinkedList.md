# 07 — Linked List in Java

#java #linkedlist #syntax #dsa
← [[06_Stack]] | Next → [[08_Graph]]

---

## 🧠 Feynman Explain

A **Linked List** is a chain — each link (node) knows where the **next link** is.  
Unlike arrays, there are **no fixed boxes** — nodes are scattered in memory and connected by pointers.  
Fast for insert/delete in middle. Slow for random access.

---

## 🔧 Define a Node

```java
class ListNode {
    int val;
    ListNode next;

    ListNode(int val) {
        this.val = val;
        this.next = null;
    }
}
```

---

## 📦 Build a Linked List (Manual)

```java
ListNode head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(3);
// 1 -> 2 -> 3 -> null
```

---

## 🔍 Traverse

```java
ListNode curr = head;
while (curr != null) {
    System.out.print(curr.val + " -> ");
    curr = curr.next;
}
```

---

## 🛠️ Core Operations

```java
// Insert at beginning
ListNode newNode = new ListNode(0);
newNode.next = head;
head = newNode;

// Insert at end
ListNode curr = head;
while (curr.next != null) curr = curr.next;
curr.next = new ListNode(99);

// Delete by value
if (head.val == target) { head = head.next; }
else {
    ListNode curr = head;
    while (curr.next != null && curr.next.val != target)
        curr = curr.next;
    if (curr.next != null) curr.next = curr.next.next;
}

// Get length
int len = 0;
ListNode curr = head;
while (curr != null) { len++; curr = curr.next; }
```

---

## 🔄 Classic Patterns

### 1. Reverse a Linked List

```java
ListNode prev = null, curr = head;
while (curr != null) {
    ListNode next = curr.next;
    curr.next = prev;
    prev = curr;
    curr = next;
}
return prev;  // New head
```

---

### 2. Floyd's Cycle Detection (Slow & Fast Pointers)

```java
ListNode slow = head, fast = head;
while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;
    if (slow == fast) return true;  // Cycle detected
}
return false;
```

---

### 3. Find Middle Node

```java
ListNode slow = head, fast = head;
while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;
}
return slow;  // Middle node
```

---

### 4. Merge Two Sorted Lists

```java
ListNode dummy = new ListNode(0);
ListNode curr = dummy;
while (l1 != null && l2 != null) {
    if (l1.val <= l2.val) { curr.next = l1; l1 = l1.next; }
    else                  { curr.next = l2; l2 = l2.next; }
    curr = curr.next;
}
curr.next = (l1 != null) ? l1 : l2;
return dummy.next;
```

---

### 5. Remove N-th from End (Two Pointers)

```java
ListNode dummy = new ListNode(0);
dummy.next = head;
ListNode fast = dummy, slow = dummy;
for (int i = 0; i <= n; i++) fast = fast.next;
while (fast != null) { slow = slow.next; fast = fast.next; }
slow.next = slow.next.next;
return dummy.next;
```

---

## 🏗️ Java's Built-in LinkedList

```java
import java.util.LinkedList;

LinkedList<Integer> ll = new LinkedList<>();
ll.addFirst(10);
ll.addLast(20);
ll.add(1, 15);         // Insert at index 1
ll.removeFirst();
ll.removeLast();
ll.getFirst();
ll.getLast();
ll.get(1);             // O(n) — slower than ArrayList
ll.size();
```

> ⚠️ In interviews, you almost always build your **own node class** rather than using built-in LinkedList.

---

## 🃏 Shortcut Cheatsheet

| Task | Code |
|------|------|
| Define node | `class ListNode { int val; ListNode next; }` |
| Traverse | `while (curr != null) curr = curr.next;` |
| Reverse | prev/curr/next pointer swap |
| Cycle detect | Slow/fast (Floyd's) |
| Find middle | Slow/fast — slow stops at mid |
| Merge sorted | Dummy node + two pointers |

---

## 🔗 Related Notes
- [[06_Stack]] — Stack can be built on linked list
- [[05_Queue]] — Queue can be built on linked list
- [[09_Tree]] — Tree nodes are similar to ListNode
