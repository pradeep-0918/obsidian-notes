# 12 — Pro Tips & Gotchas

> **MOC:** [[Linked List MOC]] | **Prev:** [[11 - Top 30 Problems Classified]]

---

## 🥇 The 7 Golden Rules of Linked List Problems

### Rule 1: Always Consider a Dummy Node

```java
// WITHOUT dummy — special case for head deletion:
if (head.val == target) {
    head = head.next;
} else {
    ListNode curr = head;
    while (curr.next != null) { ... }
}

// WITH dummy — uniform treatment:
ListNode dummy = new ListNode(0, head);
ListNode curr = dummy;
while (curr.next != null) { ... }
return dummy.next;
```

> Use dummy when: deleting nodes, inserting at any position, or when the head might change.

---

### Rule 2: Save Next Before Overwriting

```java
// WRONG — loses rest of list
curr.next = newNode;  // 💥 lost curr.next.next...

// CORRECT
ListNode savedNext = curr.next;    // save first
curr.next = newNode;               // then overwrite
newNode.next = savedNext;          // reconnect
```

---

### Rule 3: Check for null AND null.next

```java
// One-level null check (traversal)
while (curr != null) { ... }

// Two-level null check (fast/slow)
while (fast != null && fast.next != null) { ... }
//         ↑ fast itself   ↑ fast.next for fast.next.next

// Why? fast.next.next requires both fast AND fast.next to be non-null
```

---

### Rule 4: Circular Lists Need do-while or != head

```java
// WRONG — infinite loop!
ListNode curr = head;
while (curr != null) { curr = curr.next; }  // null never comes

// CORRECT
ListNode curr = head;
do {
    // process curr
    curr = curr.next;
} while (curr != head);
```

---

### Rule 5: Return the Correct Head

```java
// Common mistake: returning old head when head might have changed
public ListNode deleteHead(ListNode head) {
    return head.next;  // ← correct, old head is invalid
}

// With dummy: always return dummy.next
public ListNode process(ListNode head) {
    ListNode dummy = new ListNode(0, head);
    // ... operations ...
    return dummy.next;  // ← always correct
}
```

---

### Rule 6: In-place Operations Don't Need Extra Space

```java
// Most LL problems are O(1) space — don't reach for an ArrayList
// unless the problem actually asks you to

// BAD: O(n) space
List<Integer> vals = new ArrayList<>();
// ... collect all values ...
// ... rebuild list ...

// GOOD: O(1) space via pointer manipulation
// Reversal, merge, rearrange — all doable in-place
```

---

### Rule 7: After Modification, Reconnect Orphaned Nodes

```java
// Example: splitting a list
ListNode slow = head, fast = head;
while (fast.next != null && fast.next.next != null) {
    slow = slow.next; fast = fast.next.next;
}
ListNode second = slow.next;
slow.next = null;  // ← DON'T FORGET to sever the connection!
// Now: head ... slow → null    second ... end
```

---

## 🐛 Top 10 Bugs in Interview

1. **NPE on `fast.next.next`** → always check `fast != null && fast.next != null`
2. **Losing the rest of list** → save `next` before overwriting `.next`
3. **Infinite loop on circular list** → don't use `while (curr != null)`
4. **Off-by-one in kth from end** → use `n+1` gap with dummy, not `n`
5. **Forgetting to sever connection** after splitting a list
6. **Returning wrong head** after operations → use dummy node
7. **Not handling empty list** → always check `if (head == null)`
8. **Cycle in Floyd's: checking before moving** → check AFTER `slow = slow.next`
9. **Reversing: prev starts as null**, not head → `ListNode prev = null`
10. **Recursive depth** → iterative is safer for very long lists

---

## 🧠 Interview Communication Template

```
When asked a linked list problem, say:

1. "I'll use a dummy node to handle edge cases at the head."
2. "My approach is [two pointers / reversal / merge] because..."
3. "Time: O(n), Space: O(1)" — unless recursion or extra DS used
4. Walk through with example: 1→2→3→4
5. Code, then trace manually
6. "Edge cases: empty list, single node, all same values"
```

---

## ⚡ Quick Lookup: Which Pattern?

```
Contains "middle"?          → Fast/slow pointer (LC 876)
Contains "cycle"?           → Floyd's (LC 141, 142)
Contains "reverse"?         → 3-pointer iterative (LC 206, 92, 25)
Contains "merge"?           → Dummy + compare heads (LC 21, 23)
Contains "sort"?            → Merge sort (LC 148)
Contains "palindrome"?      → Mid + reverse + compare (LC 234)
Contains "kth from end"?    → Gap-of-k two pointer (LC 19)
Contains "intersection"?    → Redirect pointers (LC 160)
Contains "LRU"?             → DLL + HashMap (LC 146)
Contains "duplicate"?       → Dummy + skip (LC 82, 83)
Contains "flatten"?         → DFS / recursive (LC 430)
```

---

## 🔬 Tricky Edge Cases to Always Test

```java
// 1. Null list
head = null

// 2. Single node
head = [5]

// 3. Two nodes
head = [1, 2]

// 4. Even-length list (affects "middle" problems)
head = [1, 2, 3, 4]

// 5. All same values (affects duplicate problems)
head = [3, 3, 3]

// 6. Cycle at tail vs middle vs head
// 7. The target node IS the head or tail
```

---

## 🔗 Connected Notes
- [[01 - What is a Linked List]] — dummy node explained
- [[05 - Two Pointer Patterns]] — all fast/slow gotchas
- [[06 - Reversal Patterns]] — prev = null rule
- [[07 - Cycle Detection]] — Floyd's gotchas
- [[11 - Top 30 Problems Classified]] — apply these rules to each problem

---
`#DSA` `#LinkedList` `#ProTips` `#Gotchas` `#Interview`
