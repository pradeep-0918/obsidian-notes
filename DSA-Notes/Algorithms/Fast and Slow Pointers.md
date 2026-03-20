# ⚙️ Fast and Slow Pointers (Floyd's)

#Algorithm #Pattern #LinkedList

⬅️ [[Two Pointers]] | ➡️ [[Sliding Window]]

---

## 📌 Definition

**Fast & Slow Pointers** (Floyd's Tortoise and Hare) uses two pointers at different speeds to detect cycles, find middle nodes, or detect specific positions.

---

## ⚙️ How It Works

```python
# Detect cycle in linked list
def has_cycle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True
    return False

# Find middle of linked list
def find_middle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    return slow  # slow is at middle

# Find cycle start (Floyd's phase 2)
def detect_cycle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            break
    else:
        return None  # no cycle
    
    slow = head
    while slow != fast:
        slow = slow.next
        fast = fast.next
    return slow  # cycle start
```

---

## 🧠 When to Use

- Detect cycle in linked list or array
- Find middle of linked list
- Find start of cycle
- Happy number (cycle in number sequence)

---

## ⏱ Complexity

| Metric | Value |
|--------|-------|
| Time | O(n) |
| Space | O(1) |

---

## 🧪 Problems

### Easy
- [Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)
- [Happy Number](https://leetcode.com/problems/happy-number/)

### Medium
- [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

---

## 🔗 Related Topics

- [[Linked List]] — primary application
- [[Two Pointers]] — generalized pattern
- [[Cycle Detection]] — core use case

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
