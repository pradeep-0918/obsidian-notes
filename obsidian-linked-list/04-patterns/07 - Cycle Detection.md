# 07 — Cycle Detection

> **MOC:** [[Linked List MOC]] | **Prev:** [[06 - Reversal Patterns]] | **Next:** [[08 - Merge and Sort]]

---

## 🧠 Why Cycles Are Tricky

A cycle makes your traversal loop forever. `while (curr != null)` **never terminates** in a cyclic list.

Three things to know:
1. **Does a cycle exist?** → Floyd's algorithm
2. **Where does the cycle start?** → Floyd's Phase 2
3. **How long is the cycle?** → Count from meeting point

---

## ✅ Full Floyd's Cycle Detection — LC 141 & 142

```java
// ============================
// PART 1: Does a cycle exist?
// ============================
public boolean hasCycle(ListNode head) {
    ListNode slow = head, fast = head;

    while (fast != null && fast.next != null) {
        slow = slow.next;           // 1 step
        fast = fast.next.next;      // 2 steps

        if (slow == fast) return true;   // MUST check after moving
    }
    return false;
}

// ============================
// PART 2: Where does cycle start?
// ============================
public ListNode detectCycle(ListNode head) {
    ListNode slow = head, fast = head;

    // Phase 1: Find meeting point inside cycle
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) {
            // Phase 2: Find cycle entry
            ListNode entry = head;
            while (entry != slow) {
                entry = entry.next;
                slow = slow.next;
            }
            return entry;  // cycle start node
        }
    }
    return null;  // no cycle
}

// ============================
// PART 3: Cycle length
// ============================
public int cycleLength(ListNode head) {
    ListNode slow = head, fast = head;

    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;

        if (slow == fast) {
            // Count cycle length from meeting point
            int length = 1;
            ListNode curr = slow.next;
            while (curr != slow) {
                curr = curr.next;
                length++;
            }
            return length;
        }
    }
    return 0;  // no cycle
}
```

---

## 📐 The Mathematics (Important for Interviews)

```
Variables:
  F = distance from head to cycle start
  a = distance from cycle start to meeting point
  C = cycle length

At meeting point:
  Slow traveled: F + a
  Fast traveled: F + a + n*C  (n complete cycles)
  Fast = 2 × Slow:

  F + a + n*C = 2(F + a)
  n*C = F + a
  F = n*C - a

When n=1:  F = C - a

Key insight: distance from head to cycle start (F)
= distance from meeting point to cycle start (C - a)

That's why resetting one pointer to head and
moving both 1 step → they meet at cycle start!
```

---

## 🔧 Alternative: HashSet Approach (More Intuitive, O(n) Space)

```java
public ListNode detectCycleHashSet(ListNode head) {
    Set<ListNode> visited = new HashSet<>();
    ListNode curr = head;

    while (curr != null) {
        if (visited.contains(curr)) return curr;  // first repeated node = cycle start
        visited.add(curr);
        curr = curr.next;
    }
    return null;
}
```

> ✅ Use HashSet in interviews if you forget Floyd's math. Then mention Floyd's for O(1) space optimization.

---

## 🔁 Create a Cycle (for testing)

```java
// Create a cycle at position pos (0-indexed)
public ListNode createCycle(int[] arr, int pos) {
    ListNode dummy = new ListNode(0);
    ListNode curr = dummy;
    ListNode cycleNode = null;

    for (int i = 0; i < arr.length; i++) {
        curr.next = new ListNode(arr[i]);
        curr = curr.next;
        if (i == pos) cycleNode = curr;
    }
    if (pos >= 0) curr.next = cycleNode;  // create the cycle

    return dummy.next;
}
```

---

## 🔍 Common Cycle Problems

| Problem | Pattern | LC # |
|---------|---------|------|
| Detect cycle | Fast/slow meet | 141 |
| Find cycle start | Floyd Phase 2 | 142 |
| Happy number | Cycle in number sequence | 202 |
| Find duplicate in array | Cycle in value-as-pointer | 287 |
| Circular array loop | Cycle in array indices | 457 |

---

## 💡 Pro Tip: LC 287 — Find Duplicate Number

```java
// The array itself IS a linked list!
// arr[i] = value = index to jump to
// Finding duplicate = finding cycle start in Floyd's
public int findDuplicate(int[] nums) {
    int slow = nums[0], fast = nums[0];

    do {
        slow = nums[slow];
        fast = nums[nums[fast]];
    } while (slow != fast);

    slow = nums[0];
    while (slow != fast) {
        slow = nums[slow];
        fast = nums[fast];
    }
    return slow;
}
```

---

## 🔗 Connected Notes
- [[05 - Two Pointer Patterns]] — Floyd's algorithm originates here
- [[03 - Types of Linked Lists]] — circular list vs accidental cycle
- [[11 - Top 30 Problems Classified]] — LC 141, 142, 202, 287

---
`#DSA` `#LinkedList` `#CycleDetection` `#Floyd` `#Java`
