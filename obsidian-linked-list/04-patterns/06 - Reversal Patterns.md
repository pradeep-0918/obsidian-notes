  v# 06 — Reversal Patterns

> **MOC:** [[Linked List MOC]] | **Prev:** [[05 - Two Pointer Patterns]] | **Next:** [[07 - Cycle Detection]]

---

## 🧠 The Core Reversal Insight

Reversing a linked list is about **changing the direction of every pointer** while keeping track of the previous node (since you can't go backward).

```
Before: null ← prev  curr → next
After:  prev ← curr  next → ...

Three variables: prev, curr, next
```

---

## 🔑 The Standard Reversal (MEMORISE)

```java
// LC 206 - Reverse Linked List
public ListNode reverse(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;

    while (curr != null) {
        ListNode next = curr.next;   // 1. Save next
        curr.next = prev;            // 2. Reverse pointer
        prev = curr;                 // 3. Move prev forward
        curr = next;                 // 4. Move curr forward
    }
    return prev;  // prev is the new head
}

/*
Walk-through: 1→2→3→null

Step 1: prev=null, curr=1
  save next=2, 1.next=null, prev=1, curr=2

Step 2: prev=1, curr=2
  save next=3, 2.next=1, prev=2, curr=3

Step 3: prev=2, curr=3
  save next=null, 3.next=2, prev=3, curr=null

Return prev=3 (new head): 3→2→1→null ✓
*/
```

---

## 🔄 Recursive Reversal

```java
public ListNode reverseRecursive(ListNode head) {
    // Base case
    if (head == null || head.next == null) return head;

    // Recurse to end — newHead will be the last node
    ListNode newHead = reverseRecursive(head.next);

    // On the way back up: reverse current pointer
    head.next.next = head;  // next node now points back to current
    head.next = null;       // current's forward pointer becomes null

    return newHead;
}

/*
The magic: reverseRecursive reaches the end, then on the
way BACK UP each node gets its pointer reversed.

Call stack for 1→2→3:
recurse(1) → recurse(2) → recurse(3) → returns 3
Back at 2: 2.next.next = 2 (so 3→2), 2.next = null
Back at 1: 1.next.next = 1 (so 2→1), 1.next = null
Result: 3→2→1→null
*/
```

---

## 🔷 Reverse a Sublist (Between positions m and n) — LC 92

```java
public ListNode reverseBetween(ListNode head, int left, int right) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode prev = dummy;

    // Move prev to position just before 'left'
    for (int i = 1; i < left; i++) prev = prev.next;

    ListNode curr = prev.next;  // first node to reverse

    // Reverse (right - left) times using "insert at front" technique
    for (int i = 0; i < right - left; i++) {
        ListNode next = curr.next;
        curr.next = next.next;          // unlink next
        next.next = prev.next;          // insert next at front
        prev.next = next;               // update prev's next
    }

    return dummy.next;
}

/*
"Insert at front" technique explained:
List: prev → [curr] → [next] → rest
After: prev → [next] → [curr] → rest
Repeat (right-left) times.
*/
```

---

## 🔶 Reverse k-Group — LC 25 (Hard)

```java
public ListNode reverseKGroup(ListNode head, int k) {
    // Check if there are k nodes remaining
    ListNode check = head;
    for (int i = 0; i < k; i++) {
        if (check == null) return head;  // fewer than k nodes → don't reverse
        check = check.next;
    }

    // Reverse k nodes
    ListNode prev = null, curr = head;
    for (int i = 0; i < k; i++) {
        ListNode next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }

    // head is now the tail of the reversed group
    // Recursively reverse the rest and connect
    head.next = reverseKGroup(curr, k);

    return prev;  // prev is the new head of this group
}
```

---

## 🔁 Reverse in Pairs — LC 24

```java
// Special case of reverse k=2 group
public ListNode swapPairs(ListNode head) {
    ListNode dummy = new ListNode(0, head);
    ListNode prev = dummy;

    while (prev.next != null && prev.next.next != null) {
        ListNode first = prev.next;
        ListNode second = prev.next.next;

        // Swap
        first.next = second.next;
        second.next = first;
        prev.next = second;

        prev = first;  // move prev to first (which is now behind second)
    }
    return dummy.next;
}
```

---

## 🧩 When to Use Which Reversal

| Problem Type | Approach |
|-------------|----------|
| Reverse entire list | Standard 3-pointer iterative |
| Reverse from position m to n | Dummy node + insert-at-front trick |
| Reverse every k nodes | Recursive k-group reversal |
| Reverse pairs | Swap pairs iterative |
| Check palindrome | Reverse second half + compare |
| Reorder list (1→n→2→n-1→...) | Find mid + reverse half + merge |

---

## 🔗 Connected Notes
- [[05 - Two Pointer Patterns]] — find middle before reversing second half
- [[08 - Merge and Sort]] — merge after reversing
- [[09 - Recursion on Linked Lists]] — recursive reversal expanded
- [[11 - Top 30 Problems Classified]] — LC 206, 92, 25, 24, 234

---
`#DSA` `#LinkedList` `#Reversal` `#Patterns` `#Java`
