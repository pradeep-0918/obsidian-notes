# 08 — Merge and Sort

> **MOC:** [[Linked List MOC]] | **Prev:** [[07 - Cycle Detection]] | **Next:** [[09 - Recursion on Linked Lists]]

---

## 🧠 Core Insight

Merging linked lists is easier than merging arrays because you're just **relinking pointers** — no extra array needed.

---

## 1️⃣ Merge Two Sorted Lists — LC 21

```java
// Iterative (preferred in interviews — O(1) space)
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0);  // anchor
    ListNode curr = dummy;

    while (l1 != null && l2 != null) {
        if (l1.val <= l2.val) {
            curr.next = l1;
            l1 = l1.next;
        } else {
            curr.next = l2;
            l2 = l2.next;
        }
        curr = curr.next;
    }

    // Attach remaining list (at most one will be non-null)
    curr.next = (l1 != null) ? l1 : l2;

    return dummy.next;
}

// Recursive (elegant, but O(n) stack space)
public ListNode mergeTwoListsRecursive(ListNode l1, ListNode l2) {
    if (l1 == null) return l2;
    if (l2 == null) return l1;

    if (l1.val <= l2.val) {
        l1.next = mergeTwoListsRecursive(l1.next, l2);
        return l1;
    } else {
        l2.next = mergeTwoListsRecursive(l1, l2.next);
        return l2;
    }
}
```

---

## 2️⃣ Merge k Sorted Lists — LC 23 (Hard)

### Approach 1: Min-Heap (Priority Queue) — O(n log k)

```java
public ListNode mergeKLists(ListNode[] lists) {
    // Min-heap ordered by node value
    PriorityQueue<ListNode> pq = new PriorityQueue<>(
        (a, b) -> a.val - b.val
    );

    // Add first node of each list
    for (ListNode node : lists) {
        if (node != null) pq.offer(node);
    }

    ListNode dummy = new ListNode(0);
    ListNode curr = dummy;

    while (!pq.isEmpty()) {
        ListNode min = pq.poll();    // get smallest
        curr.next = min;
        curr = curr.next;

        if (min.next != null) pq.offer(min.next);  // add next from same list
    }
    return dummy.next;
}
```

### Approach 2: Divide and Conquer — O(n log k)

```java
public ListNode mergeKListsDivide(ListNode[] lists) {
    if (lists == null || lists.length == 0) return null;
    return mergeRange(lists, 0, lists.length - 1);
}

private ListNode mergeRange(ListNode[] lists, int left, int right) {
    if (left == right) return lists[left];

    int mid = left + (right - left) / 2;
    ListNode l1 = mergeRange(lists, left, mid);
    ListNode l2 = mergeRange(lists, mid + 1, right);
    return mergeTwoLists(l1, l2);
}
```

---

## 3️⃣ Sort a Linked List — LC 148 — O(n log n) with O(1) space

```java
// Merge Sort on Linked List — the ONLY sorting algorithm that works well on LL
public ListNode sortList(ListNode head) {
    if (head == null || head.next == null) return head;

    // Step 1: Find middle (split list in two)
    ListNode mid = findMidAndSplit(head);
    ListNode right = mid.next;
    mid.next = null;  // disconnect left half

    // Step 2: Sort both halves recursively
    ListNode left = sortList(head);
    right = sortList(right);

    // Step 3: Merge sorted halves
    return mergeTwoLists(left, right);
}

// Find middle AND cut the list there
private ListNode findMidAndSplit(ListNode head) {
    ListNode slow = head, fast = head.next;  // note: fast starts at head.next
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow;  // slow is the end of the left half
}
```

> 💡 Why Merge Sort? Quick sort on LL is O(n²) average due to bad pivot choices. Merge sort on LL is O(n log n) naturally.

---

## 4️⃣ Reorder List — LC 143

```java
// 1→2→3→4→5 becomes 1→5→2→4→3
public void reorderList(ListNode head) {
    if (head == null || head.next == null) return;

    // Step 1: Find middle
    ListNode slow = head, fast = head;
    while (fast.next != null && fast.next.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    // Step 2: Reverse second half
    ListNode secondHalf = reverse(slow.next);
    slow.next = null;  // cut list in half

    // Step 3: Interleave
    ListNode first = head, second = secondHalf;
    while (second != null) {
        ListNode tmp1 = first.next;
        ListNode tmp2 = second.next;

        first.next = second;
        second.next = tmp1;

        first = tmp1;
        second = tmp2;
    }
}

private ListNode reverse(ListNode head) {
    ListNode prev = null, curr = head;
    while (curr != null) {
        ListNode next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}
```

---

## 5️⃣ Add Two Numbers (LL as number) — LC 2

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0);
    ListNode curr = dummy;
    int carry = 0;

    while (l1 != null || l2 != null || carry != 0) {
        int sum = carry;
        if (l1 != null) { sum += l1.val; l1 = l1.next; }
        if (l2 != null) { sum += l2.val; l2 = l2.next; }

        carry = sum / 10;
        curr.next = new ListNode(sum % 10);
        curr = curr.next;
    }
    return dummy.next;
}
```

---

## 🔗 Connected Notes
- [[06 - Reversal Patterns]] — reverse second half in reorderList
- [[05 - Two Pointer Patterns]] — find middle using fast/slow
- [[09 - Recursion on Linked Lists]] — recursive merge sort
- [[11 - Top 30 Problems Classified]] — LC 21, 23, 143, 148

---
`#DSA` `#LinkedList` `#Merge` `#Sort` `#Java`
