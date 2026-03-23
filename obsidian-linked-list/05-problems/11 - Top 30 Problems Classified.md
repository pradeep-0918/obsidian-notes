# 11 — Top 30 Problems Classified

> **MOC:** [[Linked List MOC]] | **Prev:** [[10 - Java Code Templates]] | **Next:** [[12 - Pro Tips and Gotchas]]

---

## 🟢 Easy

| # | Problem | Pattern | Key Insight |
|---|---------|---------|-------------|
| LC 206 | Reverse Linked List | [[06 - Reversal Patterns\|Reversal]] | prev/curr/next 3-pointer |
| LC 21 | Merge Two Sorted Lists | [[08 - Merge and Sort\|Merge]] | Dummy node + compare heads |
| LC 141 | Linked List Cycle | [[07 - Cycle Detection\|Floyd's]] | Fast meets slow → cycle |
| LC 83 | Remove Duplicates from Sorted List | [[04 - Core Operations\|Traversal]] | Skip if curr.val == curr.next.val |
| LC 203 | Remove Linked List Elements | [[04 - Core Operations\|Delete]] | Dummy node + bypass |
| LC 234 | Palindrome Linked List | [[05 - Two Pointer Patterns\|Two Ptr]] | Find mid + reverse second half |
| LC 876 | Middle of Linked List | [[05 - Two Pointer Patterns\|Two Ptr]] | Fast/slow → slow is middle |
| LC 160 | Intersection of Two Lists | [[05 - Two Pointer Patterns\|Two Ptr]] | Redirect pointer to other list |
| LC 237 | Delete Node in a Linked List | Trick | Copy next value, delete next |
| LC 1290 | Convert Binary Number in LL to Integer | Math | Shift and add |

```java
// LC 83 - Remove Duplicates
public ListNode deleteDuplicates(ListNode head) {
    ListNode curr = head;
    while (curr != null && curr.next != null) {
        if (curr.val == curr.next.val)
            curr.next = curr.next.next;  // skip duplicate
        else
            curr = curr.next;
    }
    return head;
}

// LC 237 - Delete Node (no access to head!)
public void deleteNode(ListNode node) {
    node.val = node.next.val;    // overwrite current with next's value
    node.next = node.next.next;  // skip next
}
```

---

## 🟡 Medium

| # | Problem | Pattern | Key Insight |
|---|---------|---------|-------------|
| LC 2 | Add Two Numbers | [[08 - Merge and Sort\|Merge]] | Digit-by-digit with carry |
| LC 19 | Remove Nth from End | [[05 - Two Pointer Patterns\|Two Ptr]] | Gap of n+1, dummy node |
| LC 24 | Swap Nodes in Pairs | [[06 - Reversal Patterns\|Reversal]] | Swap every 2 nodes |
| LC 82 | Remove Duplicates II (all) | [[04 - Core Operations\|Delete]] | Dummy + skip whole duplicate group |
| LC 92 | Reverse Linked List II | [[06 - Reversal Patterns\|Reversal]] | Insert-at-front technique |
| LC 143 | Reorder List | [[06 - Reversal Patterns\|Reversal]] + [[08 - Merge and Sort\|Merge]] | Mid + Reverse + Interleave |
| LC 148 | Sort List | [[08 - Merge and Sort\|Merge Sort]] | Merge sort on LL |
| LC 328 | Odd Even Linked List | Rearrange | Two chains: odd, even → connect |
| LC 430 | Flatten Multilevel DLL | DFS | Flatten nested list |
| LC 725 | Split Linked List in Parts | Math | Distribute extras to first parts |

```java
// LC 82 - Remove All Duplicates
public ListNode deleteDuplicates(ListNode head) {
    ListNode dummy = new ListNode(0, head);
    ListNode prev = dummy;

    while (prev.next != null) {
        ListNode curr = prev.next;
        // Check if duplicates start here
        if (curr.next != null && curr.val == curr.next.val) {
            int dupVal = curr.val;
            while (prev.next != null && prev.next.val == dupVal)
                prev.next = prev.next.next;  // skip all with dupVal
        } else {
            prev = prev.next;
        }
    }
    return dummy.next;
}

// LC 328 - Odd Even Linked List
public ListNode oddEvenList(ListNode head) {
    if (head == null) return null;
    ListNode odd = head, even = head.next, evenHead = even;

    while (even != null && even.next != null) {
        odd.next = even.next;
        odd = odd.next;
        even.next = odd.next;
        even = even.next;
    }
    odd.next = evenHead;  // connect odd tail to even head
    return head;
}
```

---

## 🔴 Hard

| # | Problem | Pattern | Key Insight |
|---|---------|---------|-------------|
| LC 25 | Reverse Nodes in k-Group | [[06 - Reversal Patterns\|Reversal]] | Recursive k-reversal |
| LC 23 | Merge k Sorted Lists | [[08 - Merge and Sort\|Merge]] | Min-heap or divide & conquer |
| LC 142 | Linked List Cycle II | [[07 - Cycle Detection\|Floyd's]] | Phase 2: reset + meet at start |
| LC 146 | LRU Cache | DLL + HashMap | O(1) get and put |
| LC 460 | LFU Cache | DLL + HashMap×2 | Frequency-based eviction |
| LC 432 | All O(1) Data Structure | DLL | Inc/Dec with key grouping |

```java
// LC 25 - Reverse k Group (see full code in [[06 - Reversal Patterns]])
// LC 23 - Merge k Lists (see full code in [[08 - Merge and Sort]])
// LC 146 - LRU Cache (see full code in [[10 - Java Code Templates]])
```

---

## 🎯 Problem-to-Pattern Map

```
You see:                          Think:
"k-th from end"           →  [[05 - Two Pointer Patterns]] gap-of-k
"detect/find cycle"       →  [[07 - Cycle Detection]] Floyd's
"reverse between m-n"     →  [[06 - Reversal Patterns]] insert-at-front
"merge sorted lists"      →  [[08 - Merge and Sort]] dummy + compare
"palindrome"              →  [[05 - Two Pointer Patterns]] mid + reverse
"reorder list"            →  mid + reverse + interleave
"sort linked list"        →  [[08 - Merge and Sort]] merge sort
"LRU cache"               →  DLL + HashMap
"flatten multilevel"      →  DFS / recursive flatten
```

---

## 🏆 Interview Frequency (Google/Meta/Amazon)

High frequency: LC 206, 21, 141, 142, 19, 143, 146, 25, 23
Medium frequency: LC 92, 148, 82, 234, 2, 160
Low but asked: LC 328, 430, 460

---

## 🔗 Connected Notes
- [[05 - Two Pointer Patterns]] — covers LC 19, 141, 234, 876, 160
- [[06 - Reversal Patterns]] — covers LC 206, 92, 25, 24, 143
- [[07 - Cycle Detection]] — covers LC 141, 142
- [[08 - Merge and Sort]] — covers LC 21, 23, 148, 2
- [[10 - Java Code Templates]] — full implementations

---
`#DSA` `#LinkedList` `#LeetCode` `#Problems` `#Interview`
