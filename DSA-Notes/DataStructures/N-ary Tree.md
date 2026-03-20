# N-ary Tree

## 📌 Definition
A tree where each node can have at most **N children** (as opposed to 2 in a binary tree). Used to represent hierarchies with variable branching factors.

## ⚙️ How It Works
- Each node stores a value and a list of children (up to N)
- Traversal is similar to binary trees but iterates over a children list
- Common serializations use level-order or child-sibling representation

## 🧠 When to Use
- File system directories (unlimited children)
- Organizational charts
- XML/HTML DOM trees
- Game trees with branching > 2

## 🔥 Key Properties
- Root has no parent
- Leaf nodes have empty children lists
- Height = O(log_N(n)) for balanced trees

## ⏱ Complexity
| Operation | Time |
|-----------|------|
| Traversal | O(n) |
| Search | O(n) |
| Insert | O(1) amortized |

## 🆚 Comparison
| Tree | Max Children | Use Case |
|------|-------------|----------|
| [[Binary Tree]] | 2 | Expression trees |
| N-ary Tree | N | File systems |
| [[Trie]] | 26 (alphabet) | String prefix |

## 🧪 Problems
- [[DSA Problem Bank#All Nodes Distance K in Binary Tree]]
- [[DSA Problem Bank#Time Needed to Inform All Employees]]

## ❌ Mistakes
- Not handling nodes with zero children (leaves)
- Forgetting to iterate over all children during traversal

## 🔗 Related Topics
[[Tree]] | [[Binary Tree]] | [[Depth First Search]] | [[Breadth First Search]]

## ⬅️ Previous
[[Binary Tree]]

## ➡️ Next
[[B Tree]]

## ✅ Status
- [ ] Learned
- [ ] Practiced
- [ ] Mastered

#DSA #Tree #NaryTree #DataStructure
