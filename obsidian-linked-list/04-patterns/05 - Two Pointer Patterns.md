# 05 — Two Pointer Patterns

> **MOC:** [[Linked List MOC]] | **Prev:** [[04 - Core Operations]] | **Next:** [[06 - Reversal Patterns]]

---

## 🧠 Why Two Pointers?

Linked lists have no random access. Two pointers let you:
- Find the middle in **O(n) time, O(1) space**
- Detect cycles without using a Set
- Find kth from end in one pass
- Solve problems that seem to need knowing the length

---

## Pattern 1: Fast & Slow (Floyd's Tortoise & Hare) 🐢🐇

```
Slow moves 1 step per iteration.
Fast moves 2 steps per iteration.

If there's a cycle: fast catches up to slow (they meet).
If no cycle: fast hits null.
When fast reaches end: slow is at the MIDDLE.
```

### Find Middle of Linked List

```java
// LC 876 - Middle of the Linked List
public ListNode findMiddle(ListNode head) {
    ListNode slow = head, fast = head;

    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow;  // slow is at middle

    /*
    List: 1→2→3→4→5
    After loop: slow=3 (middle)

    List: 1→2→3→4
    After loop: slow=3 (second middle — for even length)
    Use fast.next != null for first middle on even length:
        fast=null check gives second-middle
        swap condition to get first-middle
    */
}
```

### Cycle Detection — LC 141

```java
public boolean hasCycle(ListNode head) {
    ListNode slow = head, fast = head;

    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;

        if (slow == fast) return true;  // they met → cycle!
    }
    return false;
}
```

### Find Cycle Start — LC 142

```java
public ListNode detectCycleStart(ListNode head) {
    ListNode slow = head, fast = head;

    // Phase 1: detect meeting point
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) break;
    }

    if (fast == null || fast.next == null) return null;  // no cycle

    // Phase 2: find entry point
    // MATH: distance from head to cycle start = distance from meeting point to cycle start
    slow = head;
    while (slow != fast) {
        slow = slow.next;
        fast = fast.next;  // fast now moves 1 step too
    }
    return slow;  // cycle start!
}
```

> 💡 **The Math Behind Phase 2:**
> Let: `F` = distance head → cycle start, `C` = cycle length, `a` = extra steps past cycle start when they meet.
> Slow traveled: `F + a`. Fast traveled: `F + a + C`.
> Since fast = 2×slow: `F + a + C = 2(F + a)` → `C - a = F`.
> So resetting slow to head and moving both 1-step → they meet at cycle start!

---

## Pattern 2: Fixed Distance (k-apart pointers)

### Remove kth Node from End — LC 19

```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0, head);
    ListNode fast = dummy, slow = dummy;

    // Create gap of n+1 between fast and slow
    for (int i = 0; i <= n; i++) fast = fast.next;

    // Move together until fast hits null
    while (fast != null) {
        fast = fast.next;
        slow = slow.next;
    }

    slow.next = slow.next.next;  // delete the nth node
    return dummy.next;
}
```

---

## Pattern 3: Two Pointers for Palindrome Check — LC 234

```java
public boolean isPalindrome(ListNode head) {
    // Step 1: Find middle
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    // Step 2: Reverse second half
    ListNode prev = null, curr = slow;
    while (curr != null) {
        ListNode next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }

    // Step 3: Compare both halves
    ListNode left = head, right = prev;
    while (right != null) {
        if (left.val != right.val) return false;
        left = left.next;
        right = right.next;
    }
    return true;
}
```

---

## Pattern 4: Two Pointers on Two Lists

### Intersection of Two Lists — LC 160

```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    ListNode a = headA, b = headB;

    // Elegant trick: when a reaches end, redirect to headB; vice versa
    // They'll meet at intersection after traveling same total distance
    while (a != b) {
        a = (a == null) ? headB : a.next;
        b = (b == null) ? headA : b.next;
    }
    return a;  // null if no intersection
}
```

> 💡 **Why this works:** Both pointers travel (lenA + lenB) total steps. If they intersect, they'll meet at the intersection. If not, they both reach null simultaneously.

---

## 🧩 Two Pointer Decision Template

```
Q: Need to find middle?            → Fast/slow, stop when fast hits end
Q: Detect cycle?                   → Fast/slow, check if they meet
Q: Find cycle start?               → Fast/slow → reset slow to head
Q: kth from end?                   → Create gap of k, move until fast = null
Q: Intersection of two lists?      → Redirect each pointer to other list's head
Q: Palindrome check?               → Find middle + reverse second half
```

---

## 🔗 Connected Notes
- [[04 - Core Operations]] — traversal foundation
- [[06 - Reversal Patterns]] — used inside palindrome check
- [[07 - Cycle Detection]] — deep dive on Floyd's algorithm
- [[11 - Top 30 Problems Classified]] — all problems using these patterns

---
`#DSA` `#LinkedList` `#TwoPointers` `#Patterns` `#Floyd`
