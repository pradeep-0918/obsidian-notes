# 09 — Recursion on Linked Lists

> **MOC:** [[Linked List MOC]] | **Prev:** [[08 - Merge and Sort]] | **Next:** [[10 - Java Code Templates]]

---

## 🧠 The Recursive Mental Model

Recursion on a linked list = **trusting the recursive call handles the rest**.

```
Function(head):
  if base case → return
  result = Function(head.next)   ← trust this handles the tail
  do something with head + result
  return
```

The call stack implicitly stores all nodes as it goes forward, and on the way BACK you process them in reverse order.

---

## The Stack Frames Trick

```
For list: 1 → 2 → 3 → null

Call stack builds:
  frame(1) → frame(2) → frame(3) → hits null → returns

On the way BACK UP:
  frame(3) processes 3 first
  frame(2) processes 2 second
  frame(1) processes 1 last

This is how you traverse backwards without a doubly linked list!
```

---

## 1️⃣ Recursive Traversal (Forward & Backward)

```java
// Forward traversal
public void printForward(ListNode head) {
    if (head == null) return;
    System.out.print(head.val + " ");
    printForward(head.next);  // recurse first, then nothing after
}

// Backward traversal — same code, just print AFTER the recursive call
public void printBackward(ListNode head) {
    if (head == null) return;
    printBackward(head.next);  // recurse first
    System.out.print(head.val + " ");  // then process — this is the magic
}
```

---

## 2️⃣ Recursive Reversal

```java
public ListNode reverseRecursive(ListNode head) {
    if (head == null || head.next == null) return head;

    ListNode newHead = reverseRecursive(head.next);

    // On the way back: fix the pointer
    head.next.next = head;
    head.next = null;

    return newHead;  // always the original tail
}
```

---

## 3️⃣ Recursive Delete All Occurrences — LC 203

```java
public ListNode removeElements(ListNode head, int val) {
    if (head == null) return null;

    // After this call, head.next points to a list with val removed
    head.next = removeElements(head.next, val);

    // Should we include head?
    return (head.val == val) ? head.next : head;
}
```

---

## 4️⃣ Recursive Palindrome Check

```java
// Uses the global "left" pointer trick
private ListNode left;

public boolean isPalindromeRecursive(ListNode head) {
    left = head;
    return check(head);
}

private boolean check(ListNode right) {
    if (right == null) return true;

    // Recurse to end, then on the way back compare left & right
    boolean result = check(right.next);

    if (!result) return false;
    if (left.val != right.val) return false;

    left = left.next;  // move left forward as right unwinds
    return true;
}
```

---

## 5️⃣ Recursion with Return Value Accumulation

```java
// Sum of all nodes
public int sumList(ListNode head) {
    if (head == null) return 0;
    return head.val + sumList(head.next);
}

// Count nodes
public int countNodes(ListNode head) {
    if (head == null) return 0;
    return 1 + countNodes(head.next);
}

// Find max
public int findMax(ListNode head) {
    if (head.next == null) return head.val;
    return Math.max(head.val, findMax(head.next));
}
```

---

## 6️⃣ Recursion for Merge Sort

```java
// See full implementation in [[08 - Merge and Sort]]
// Key pattern: split → sort each half recursively → merge
public ListNode sortList(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode mid = findMid(head);
    ListNode right = mid.next;
    mid.next = null;
    return merge(sortList(head), sortList(right));
}
```

---

## ⚠️ When NOT to Use Recursion

| Situation | Problem | Solution |
|-----------|---------|----------|
| Very long list (10^5 nodes) | Stack Overflow | Use iterative |
| Space-constrained | O(n) stack space | Use iterative |
| Tail recursion needed | Java doesn't optimize TCO | Use iteration |
| Clarity needed | Recursion confuses reviewers | Use iterative + comment |

---

## 🧩 Recursion Pattern Classifier

```
Do you need to process in REVERSE order?
    → Process AFTER the recursive call (backward traversal)

Do you need to process in FORWARD order?
    → Process BEFORE the recursive call

Do you need to build the result from the end?
    → Accumulate on the way back (reversal, merge sort)

Do you need to compare from both ends simultaneously?
    → Palindrome trick with global left pointer
```

---

## 🔗 Connected Notes
- [[06 - Reversal Patterns]] — iterative counterpart to recursive reversal
- [[08 - Merge and Sort]] — recursive merge sort full code
- [[05 - Two Pointer Patterns]] — iterative palindrome check
- [[10 - Java Code Templates]] — all patterns compiled

---
`#DSA` `#LinkedList` `#Recursion` `#Patterns` `#Java`
