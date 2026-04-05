# 🔗 Linked List

**← [[03_Stack]] | Next: [[05_Dynamic_Programming]] →**

---

## 1. 💡 INTUITION FIRST

> A Linked List is a **chain of nodes**. Each node holds a value and a pointer to the next node. There is **no index** — you must traverse from the `head`.
>
> **Core insight:** Most problems are solved with **two pointers** — one slow and one fast, or one ahead by `k` steps.
>
> Ask: *"Do I need two pointers at different speeds or distances?"* → Linked List pattern.

---

## 2. 🔑 PATTERNS

| Pattern | Problems |
|---|---|
| Reversal | Reverse list, reverse K-groups |
| Fast/Slow Pointers | Cycle detection, middle node |
| Dummy Node | Avoid edge cases at head |
| K-distance pointer | Remove Nth from end |
| Merge | Merge sorted lists |

---

## 3. 🧩 PROBLEMS IN THIS NOTE

| # | Problem | Pattern | LeetCode |
|---|---|---|---|
| L1 | Reverse Linked List | Reversal | #206 |
| L2 | Linked List Cycle | Fast/Slow | #141 |
| L3 | Remove Nth Node From End | Two pointers + dummy | #19 |

---

## 4. 📝 NODE STRUCTURE

```java
class ListNode {
    int val;
    ListNode next;
    ListNode(int val) { this.val = val; }
}
```

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
```

---
---

# 🔄 L1 — Reverse Linked List (LeetCode #206)

## ❓ Problem Statement

Given the head of a singly linked list, **reverse** it and return the new head.

**Input:** `1 → 2 → 3 → 4 → 5 → null`
**Output:** `5 → 4 → 3 → 2 → 1 → null`

---

## ✅ ANSWER

The reversed list is `5 → 4 → 3 → 2 → 1 → null`. The new head is node `5`.

---

## 🧠 FEYNMAN TECHNIQUE

> Imagine a line of people each holding a rope attached to the person in front (pointing forward). You want everyone to turn around and hold the rope of the person **behind** them instead.
>
> You stand at the start. You need **3 hands:**
> 1. `prev` — the person you've already turned around (starts as `null`, a wall)
> 2. `curr` — the person you're currently turning around
> 3. `next` — save the next person FIRST before curr lets go of their rope
>
> **Steps each iteration:**
> 1. Save `curr.next` (don't lose the chain!)
> 2. Point `curr.next` backward to `prev`
> 3. Move `prev` to `curr`
> 4. Move `curr` to saved `next`
>
> When `curr` is null, `prev` is your new head.

---

## 🔄 STEP-BY-STEP DRY RUN

List: `1 → 2 → 3 → null`

| Iteration | prev | curr | next | Action |
|---|---|---|---|---|
| Start | null | 1 | - | - |
| 1 | null | 1 | 2 | curr.next=null, prev→1, curr→2 |
| 2 | 1 | 2 | 3 | curr.next=1, prev→2, curr→3 |
| 3 | 2 | 3 | null | curr.next=2, prev→3, curr→null |
| End | **3** | null | - | Return prev=3 |

Result: `3 → 2 → 1 → null` ✅

---

## ☕ JAVA — Full Code

```java
public class ReverseLinkedList {

    static class ListNode {
        int val;
        ListNode next;
        ListNode(int val) { this.val = val; }
    }

    public static ListNode reverse(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;

        while (curr != null) {
            ListNode nextNode = curr.next; // 1. Save next
            curr.next = prev;             // 2. Reverse pointer
            prev = curr;                  // 3. Move prev forward
            curr = nextNode;              // 4. Move curr forward
        }

        return prev; // New head
    }

    // Helper: build list from array
    static ListNode build(int[] vals) {
        ListNode dummy = new ListNode(0);
        ListNode curr = dummy;
        for (int v : vals) { curr.next = new ListNode(v); curr = curr.next; }
        return dummy.next;
    }

    // Helper: print list
    static void print(ListNode head) {
        StringBuilder sb = new StringBuilder();
        while (head != null) {
            sb.append(head.val);
            if (head.next != null) sb.append(" → ");
            head = head.next;
        }
        System.out.println(sb + " → null");
    }

    public static void main(String[] args) {
        // ── INPUT ──
        ListNode head = build(new int[]{1, 2, 3, 4, 5});
        System.out.print("Input:  "); print(head);

        // ── PROCESS & OUTPUT ──
        ListNode reversed = reverse(head);
        System.out.print("Output: "); print(reversed);
        // Output: 5 → 4 → 3 → 2 → 1 → null
    }
}
```

---

## 🐍 PYTHON — Full Code

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reverse(head: ListNode) -> ListNode:
    prev = None
    curr = head

    while curr:
        next_node = curr.next  # 1. Save next
        curr.next = prev       # 2. Reverse pointer
        prev = curr            # 3. Advance prev
        curr = next_node       # 4. Advance curr

    return prev  # New head

# Helpers
def build(vals):
    dummy = ListNode(0)
    curr = dummy
    for v in vals:
        curr.next = ListNode(v)
        curr = curr.next
    return dummy.next

def to_list(head):
    result = []
    while head:
        result.append(head.val)
        head = head.next
    return result


# ── INPUT ──
head = build([1, 2, 3, 4, 5])
print("Input: ", to_list(head))

# ── OUTPUT ──
reversed_head = reverse(head)
print("Output:", to_list(reversed_head))
# Output: [5, 4, 3, 2, 1]
```

---

## 📖 STORY TO REMEMBER — "The Reverse Parade"

> **A parade is marching in the wrong direction. The marshal needs to reverse everyone.**
>
> The marshal (you) walks down the line. For each marcher (node), before telling them to face backward:
> 1. First, **tap the next person on the shoulder** (save `next`) — don't lose them!
> 2. Tell the current person to **face the person behind** (curr.next = prev)
> 3. Now the current person is "done" — they're your new "behind" person (prev = curr)
> 4. Move to the next saved person (curr = next)
>
> When the line ends, the last person you turned is the **new front**.
>
> **Memory hook:** `"Save next → Flip → Slide prev → Slide curr"` (like a 4-step dance move!)

---
---

# 🔁 L2 — Linked List Cycle (LeetCode #141)

## ❓ Problem Statement

Given the head of a linked list, determine if the linked list has a **cycle** in it.

**Input:** `3 → 2 → 0 → -4 → (back to 2)`
**Output:** `true`

---

## ✅ ANSWER

**`true`** — there is a cycle (node -4 points back to node 2).

---

## 🧠 FEYNMAN TECHNIQUE

> Imagine two runners on a circular track: one slow (jogs), one fast (sprints).
>
> If the track is circular (has a cycle), the **fast runner will eventually lap the slow one** — they'll be at the same position.
>
> If there's no cycle (straight track), the fast runner hits the end (null) first.
>
> **Floyd's Algorithm:** slow moves 1 step, fast moves 2 steps. If they ever meet → cycle exists.

---

## 🔄 STEP-BY-STEP DRY RUN

List: `3 → 2 → 0 → -4 → (→2 cycle)`

| Step | Slow | Fast |
|---|---|---|
| Start | 3 | 3 |
| 1 | 2 | 0 |
| 2 | 0 | 2 (looped back) |
| 3 | -4 | -4 |
| **Meet!** | -4 | -4 → **Cycle detected** ✅ |

---

## ☕ JAVA — Full Code

```java
public class LinkedListCycle {

    static class ListNode {
        int val;
        ListNode next;
        ListNode(int val) { this.val = val; }
    }

    public static boolean hasCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;

        // Fast needs itself and next to not be null
        while (fast != null && fast.next != null) {
            slow = slow.next;       // Move 1 step
            fast = fast.next.next;  // Move 2 steps

            if (slow == fast) return true; // They met! Cycle exists.
        }

        return false; // fast hit null → no cycle
    }

    public static void main(String[] args) {
        // ── INPUT: Build cycle manually ──
        // 3 → 2 → 0 → -4 → (back to node 2)
        ListNode n1 = new ListNode(3);
        ListNode n2 = new ListNode(2);
        ListNode n3 = new ListNode(0);
        ListNode n4 = new ListNode(-4);
        n1.next = n2; n2.next = n3; n3.next = n4;
        n4.next = n2; // Create cycle here

        // ── OUTPUT ──
        System.out.println("Has Cycle: " + hasCycle(n1));
        // Output: Has Cycle: true

        // Test without cycle
        ListNode noLoop = new ListNode(1);
        noLoop.next = new ListNode(2);
        System.out.println("Has Cycle (no loop): " + hasCycle(noLoop));
        // Output: Has Cycle (no loop): false
    }
}
```

---

## 🐍 PYTHON — Full Code

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def hasCycle(head: ListNode) -> bool:
    slow = head
    fast = head

    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow is fast:  # same object in memory
            return True

    return False


# ── INPUT: Build cycle ──
n1, n2, n3, n4 = ListNode(3), ListNode(2), ListNode(0), ListNode(-4)
n1.next = n2; n2.next = n3; n3.next = n4
n4.next = n2  # Cycle: -4 → 2

# ── OUTPUT ──
print("Has Cycle:", hasCycle(n1))       # True
print("Has Cycle (linear):", hasCycle(ListNode(1)))  # False
```

---

## 📖 STORY TO REMEMBER — "The Track Race"

> **Usain (fast) and Sam (slow) are both jogging on an unknown track.**
>
> If the track is a loop, Usain (2x speed) will eventually lap Sam and they'll be at the **same spot** at the same time. "Hey Sam! We've met again!"
>
> If the track is straight with an end, Usain hits the **finish line** (null) first without ever meeting Sam again.
>
> **Memory hook:** `"Two runners → If loop → Fast laps slow → They meet"`
> `fast.next.next, slow.next` → if `slow == fast` → cycle!

---
---

# ✂️ L3 — Remove Nth Node From End (LeetCode #19)

## ❓ Problem Statement

Given a linked list, remove the **nth node from the end** and return the head.

**Input:** `1 → 2 → 3 → 4 → 5`, n = 2
**Output:** `1 → 2 → 3 → 5`

---

## ✅ ANSWER

Remove node `4` (2nd from end). Result: `1 → 2 → 3 → 5`.

---

## 🧠 FEYNMAN TECHNIQUE

> You want to find the 2nd person from the END of a line without knowing how long the line is.
>
> **Trick:** Send a scout ahead by `n` steps. Then walk both the scout and yourself together. When the scout reaches the end, YOU are exactly n steps from the end!
>
> But you need to stop ONE before the node to delete (to update `next`). So move the scout `n+1` steps ahead.
>
> **Dummy node:** Attach a fake "0" node before the head. This handles the edge case of removing the actual head.

---

## 🔄 STEP-BY-STEP DRY RUN

List: `dummy → 1 → 2 → 3 → 4 → 5`, n=2

```
Fast moves n+1=3 steps: dummy→1→2→3, Fast=3
Now both move until Fast=null:
  Slow=1, Fast=4
  Slow=2, Fast=5
  Slow=3, Fast=null ← stop!
Slow is at 3, slow.next=4 (the target)
slow.next = slow.next.next (skip 4)
Result: 1→2→3→5 ✅
```

---

## ☕ JAVA — Full Code

```java
public class RemoveNthFromEnd {

    static class ListNode {
        int val; ListNode next;
        ListNode(int val) { this.val = val; }
    }

    public static ListNode removeNthFromEnd(ListNode head, int n) {
        // Dummy node handles edge case of removing the head
        ListNode dummy = new ListNode(0);
        dummy.next = head;

        ListNode fast = dummy;
        ListNode slow = dummy;

        // Move fast n+1 steps ahead
        for (int i = 0; i <= n; i++) {
            fast = fast.next;
        }

        // Move both until fast reaches end
        while (fast != null) {
            slow = slow.next;
            fast = fast.next;
        }

        // slow.next is the node to remove
        slow.next = slow.next.next;

        return dummy.next;
    }

    static ListNode build(int[] vals) {
        ListNode dummy = new ListNode(0), curr = dummy;
        for (int v : vals) { curr.next = new ListNode(v); curr = curr.next; }
        return dummy.next;
    }

    static void print(ListNode head) {
        while (head != null) {
            System.out.print(head.val + (head.next != null ? " → " : " → null\n"));
            head = head.next;
        }
    }

    public static void main(String[] args) {
        // ── INPUT ──
        ListNode head = build(new int[]{1, 2, 3, 4, 5});
        System.out.print("Input:  "); print(head);

        // ── OUTPUT ──
        ListNode result = removeNthFromEnd(head, 2);
        System.out.print("Output: "); print(result);
        // Output: 1 → 2 → 3 → 5 → null
    }
}
```

---

## 🐍 PYTHON — Full Code

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def removeNthFromEnd(head: ListNode, n: int) -> ListNode:
    dummy = ListNode(0)
    dummy.next = head
    fast = slow = dummy

    # Move fast n+1 steps ahead
    for _ in range(n + 1):
        fast = fast.next

    # Move both to end
    while fast:
        slow = slow.next
        fast = fast.next

    # Remove nth node
    slow.next = slow.next.next
    return dummy.next

# Helpers
def build(vals):
    dummy = ListNode(0); curr = dummy
    for v in vals:
        curr.next = ListNode(v); curr = curr.next
    return dummy.next

def to_list(head):
    res = []
    while head: res.append(head.val); head = head.next
    return res


# ── INPUT ──
head = build([1, 2, 3, 4, 5])
print("Input: ", to_list(head))

# ── OUTPUT ──
result = removeNthFromEnd(head, 2)
print("Output:", to_list(result))
# Output: [1, 2, 3, 5]
```

---

## 📖 STORY TO REMEMBER — "The Gap Runner"

> **Race marshals want to mark the 2nd-to-last runner for a penalty.**
>
> They don't know how many runners are in the race (list length unknown).
>
> **Trick:** Marshal A sprints ahead by `n+1` runners. Then Marshal B and A both walk at the same speed.
>
> When A reaches the end, B is EXACTLY at the person BEFORE the target runner. B marks: "next person is penalized" and skips them.
>
> The dummy node is like a fake "0th runner" that ensures even the first runner can be removed cleanly.
>
> **Memory hook:** `"Send scout n+1 ahead → Walk together → Scout ends → You're before target → Skip target"`

---

## ⚠️ COMMON MISTAKES (ALL LINKED LIST PROBLEMS)

- ❌ **Not saving `curr.next` before reversing** → you lose the rest of the list forever!
- ❌ **Fast pointer check:** must be `fast != null && fast.next != null` → else NPE
- ❌ **Off-by-one in remove Nth:** use `n+1` steps for fast, not `n`
- ❌ **Forgetting dummy node** when the head itself might be deleted

---

## 📋 LEETCODE PRACTICE

| # | Problem | Level | Pattern |
|---|---|---|---|
| 206 | Reverse Linked List | Easy | Reversal |
| 141 | Linked List Cycle | Easy | Fast/Slow |
| 21 | Merge Two Sorted Lists | Easy | Merge + dummy |
| 19 | Remove Nth Node From End | Medium | Two pointers |
| 876 | Middle of the Linked List | Easy | Fast/Slow |

---

**← [[03_Stack]] | Next: [[05_Dynamic_Programming]] →**

*Tags: #linked-list #two-pointers #fast-slow #reversal #java #python #dsa*
