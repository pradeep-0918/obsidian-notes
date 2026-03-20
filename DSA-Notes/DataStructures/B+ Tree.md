# B+ Tree

## 📌 Definition
A variant of the [[B Tree]] where all actual data records are stored only in **leaf nodes**, and internal nodes contain only keys for routing. Leaf nodes are linked for fast sequential access.

## ⚙️ How It Works
- Internal nodes: routing keys only (no data)
- Leaf nodes: store all key-value pairs + pointer to next leaf
- Leaf linked list enables efficient range queries
- Same balancing rules as B Tree

## 🧠 When to Use
- Database primary indexes (range queries)
- File systems requiring sequential access
- Any scenario needing both point lookup AND range scan

## 🔥 Key Properties
- Higher fan-out than B Tree (internal nodes have more keys)
- Range queries: traverse leaf linked list
- All data at uniform depth (leaves)

## ⏱ Complexity
| Operation | Time |
|-----------|------|
| Search | O(log n) |
| Insert | O(log n) |
| Delete | O(log n) |
| Range Query | O(log n + k) |

## 🆚 Comparison
| Feature | [[B Tree]] | B+ Tree |
|---------|--------|---------|
| Data location | All nodes | Leaves only |
| Range query | Slower | O(log n + k) |
| Internal node size | Larger | Smaller (more keys) |

## 🧪 Problems
- [[DSA Problem Bank#Count of Smaller Numbers After Self]]

## ❌ Mistakes
- Forgetting that internal nodes in B+ Tree don't hold actual data
- Missing the leaf-level linked list during range traversal

## 🔗 Related Topics
[[B Tree]] | [[Binary Search Tree]] | [[Segment Tree]] | [[Fenwick Tree]]

## ⬅️ Previous
[[B Tree]]

## ➡️ Next
[[Heap]]

## ✅ Status
- [ ] Learned
- [ ] Practiced
- [ ] Mastered

#DSA #Tree #BPlusTree #DataStructure
