# B Tree

## 📌 Definition
A self-balancing search tree where each node can hold multiple keys and have multiple children. Designed for systems that read/write large blocks of data (databases, file systems).

## ⚙️ How It Works
- Every node has between ⌈m/2⌉ and m children (m = order)
- All leaf nodes are at the same depth
- Keys in each node are sorted
- Internal nodes guide search; leaves hold actual data pointers

## 🧠 When to Use
- Database indexing (MySQL InnoDB, PostgreSQL)
- File systems (NTFS, HFS+, ext4)
- Situations with large datasets that don't fit in memory

## 🔥 Key Properties
- Minimizes disk I/O (nodes sized to disk blocks)
- All leaves at same level (perfectly balanced)
- Order-m tree: max m children, max m-1 keys per node

## ⏱ Complexity
| Operation | Time |
|-----------|------|
| Search | O(log n) |
| Insert | O(log n) |
| Delete | O(log n) |
| Space | O(n) |

## 🆚 Comparison
| Tree | Branching | Leaf Data | Use Case |
|------|-----------|-----------|----------|
| [[Binary Search Tree]] | 2 | No | In-memory |
| B Tree | m | Yes | Disk storage |
| [[B+ Tree\|B+ Tree]] | m | Only leaves | Range queries |

## 🧪 Problems
- [[DSA Problem Bank#Range Sum Query - Mutable]]

## ❌ Mistakes
- Confusing B Tree with B+ Tree (B Tree stores data in all nodes)
- Forgetting that splits propagate upward during insertion

## 🔗 Related Topics
[[B+ Tree]] | [[Binary Search Tree]] | [[AVL Tree]] | [[Segment Tree]]

## ⬅️ Previous
[[N-ary Tree]]

## ➡️ Next
[[B+ Tree]]

## ✅ Status
- [ ] Learned
- [ ] Practiced
- [ ] Mastered

#DSA #Tree #BTree #DataStructure
