# 📦 Hash Table

#DataStructure #HashBased #HashTable

⬅️ [[Priority Queue]] | ➡️ [[Hash Map]]

---

## 📌 Definition

A **Hash Table** maps keys to values using a **hash function** to compute an index into an array of buckets. Provides average O(1) operations.

---

## ⚙️ How It Works

```
key → hash_function(key) → index → bucket[index] → value

hash("apple") → 3 → bucket[3] → "fruit"
hash("dog")   → 7 → bucket[7] → "animal"
```

**Collision Resolution:**
1. **Chaining** — each bucket holds a linked list
2. **Open Addressing** — probe next empty slot

---

## 🧠 When to Use

- Frequency counting
- Lookup / existence check
- Grouping elements (anagrams)
- Caching (memoization)
- Two Sum type problems

---

## 🔥 Key Properties

- **Load factor** = n/capacity; rehash when too high
- Average O(1) but O(n) worst case (all collisions)
- Keys must be hashable (immutable in Python)

---

## ⏱ Complexity

| Operation | Average | Worst |
|-----------|---------|-------|
| Insert | O(1) | O(n) |
| Delete | O(1) | O(n) |
| Search | O(1) | O(n) |
| Space | O(n) | O(n) |

---

## 🧪 Problems

### Easy
- [Two Sum](https://leetcode.com/problems/two-sum/)
- [Ransom Note](https://leetcode.com/problems/ransom-note/)
- [Isomorphic Strings](https://leetcode.com/problems/isomorphic-strings/)

### Medium
- [Group Anagrams](https://leetcode.com/problems/group-anagrams/)
- [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)
- [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)
- [LRU Cache](https://leetcode.com/problems/lru-cache/)

---

## 🔗 Related Topics

- [[Hash Map]] — key-value store
- [[Hash Set]] — existence only
- [[Bloom Filter]] — probabilistic membership
- [[Array]] — underlying bucket array
- [[Linked List]] — chaining

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
