# 04 — Core Operations

> **MOC:** [[Linked List MOC]] | **Prev:** [[03 - Types of Linked Lists]] | **Next:** [[05 - Two Pointer Patterns]]

---

## 🧠 The Golden Rule for All LL Operations

> **Never lose a pointer before you've saved what you need.**
> Before `curr.next = newNode`, save `ListNode temp = curr.next`.

---

## 1️⃣ Traversal

```java
// Basic traversal — the foundation of everything
public void traverse(ListNode head) {
    ListNode curr = head;
    while (curr != null) {
        // Process curr.val here
        curr = curr.next;
    }
}

// Traversal with index
public void traverseWithIndex(ListNode head) {
    int idx = 0;
    ListNode curr = head;
    while (curr != null) {
        System.out.println("Index " + idx + ": " + curr.val);
        curr = curr.next;
        idx++;
    }
}
```

---

## 2️⃣ Insertion

### Insert at Head — O(1)

```java
public ListNode insertAtHead(ListNode head, int val) {
    ListNode node = new ListNode(val);
    node.next = head;
    return node;  // new head!
}
```

### Insert at Tail — O(n) without tail pointer

```java
public ListNode insertAtTail(ListNode head, int val) {
    ListNode node = new ListNode(val);
    if (head == null) return node;

    ListNode curr = head;
    while (curr.next != null) curr = curr.next;  // reach last node
    curr.next = node;
    return head;
}
```

### Insert at Position k — O(k)

```java
public ListNode insertAtPosition(ListNode head, int val, int k) {
    ListNode dummy = new ListNode(0);  // dummy node trick!
    dummy.next = head;
    ListNode curr = dummy;

    // Move to position k-1
    for (int i = 0; i < k && curr != null; i++) {
        curr = curr.next;
    }

    if (curr == null) return dummy.next; // k out of bounds

    ListNode node = new ListNode(val);
    node.next = curr.next;
    curr.next = node;

    return dummy.next;
}
```

### Insert Before a Given Node — O(n)

```java
public ListNode insertBefore(ListNode head, int target, int val) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode curr = dummy;

    while (curr.next != null && curr.next.val != target) {
        curr = curr.next;
    }

    if (curr.next == null) return dummy.next; // target not found

    ListNode node = new ListNode(val);
    node.next = curr.next;
    curr.next = node;

    return dummy.next;
}
```

---

## 3️⃣ Deletion

### Delete Head — O(1)

```java
public ListNode deleteHead(ListNode head) {
    if (head == null) return null;
    return head.next;
}
```

### Delete Tail — O(n)

```java
public ListNode deleteTail(ListNode head) {
    if (head == null || head.next == null) return null;

    ListNode curr = head;
    while (curr.next.next != null) curr = curr.next;  // stop at 2nd-to-last
    curr.next = null;
    return head;
}
```

### Delete by Value — O(n)

```java
public ListNode deleteByValue(ListNode head, int val) {
    ListNode dummy = new ListNode(0);  // dummy node makes head deletion uniform
    dummy.next = head;
    ListNode curr = dummy;

    while (curr.next != null) {
        if (curr.next.val == val) {
            curr.next = curr.next.next;  // bypass
        } else {
            curr = curr.next;
        }
    }
    return dummy.next;
}
```

### Delete kth Node from End — O(n) single pass

```java
// Classic two-pointer technique
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode fast = dummy, slow = dummy;

    // Move fast n+1 steps ahead
    for (int i = 0; i <= n; i++) fast = fast.next;

    // Move both until fast reaches end
    while (fast != null) {
        fast = fast.next;
        slow = slow.next;
    }

    // slow is now at (k-1)th from end
    slow.next = slow.next.next;

    return dummy.next;
}
```

---

## 4️⃣ Search

```java
public ListNode search(ListNode head, int target) {
    ListNode curr = head;
    while (curr != null) {
        if (curr.val == target) return curr;  // found!
        curr = curr.next;
    }
    return null;  // not found
}
```

---

## 5️⃣ Get kth Node from End (without knowing length)

```java
// Two pointer — fast is k steps ahead of slow
public ListNode kthFromEnd(ListNode head, int k) {
    ListNode fast = head, slow = head;

    // Move fast k steps
    for (int i = 0; i < k; i++) {
        if (fast == null) return null;  // k > length
        fast = fast.next;
    }

    // Move both until fast hits null
    while (fast != null) {
        fast = fast.next;
        slow = slow.next;
    }
    return slow;
}
```

---

## 🔗 Connected Notes
- [[05 - Two Pointer Patterns]] — fast/slow pointer mastery
- [[06 - Reversal Patterns]] — in-place reversal
- [[10 - Java Code Templates]] — complete working implementations
- [[12 - Pro Tips and Gotchas]] — dummy node explained

---
`#DSA` `#LinkedList` `#Operations` `#Java`
