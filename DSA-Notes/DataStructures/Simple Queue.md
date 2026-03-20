# Simple Queue

## 📌 Definition
A basic FIFO (First In, First Out) linear data structure where elements are inserted at the rear and removed from the front.

## ⚙️ How It Works
- Enqueue: add element to rear — O(1)
- Dequeue: remove element from front — O(1)
- Peek/Front: view front element without removal — O(1)
- Implemented with array (with head/tail pointers) or linked list

## 🧠 When to Use
- BFS traversal
- Task scheduling (CPU, print jobs)
- Producer-consumer pipelines
- Request buffering

## 🔥 Key Properties
- FIFO order preserved
- Fixed-size (array) or dynamic (linked list)
- Front and rear pointers

## ⏱ Complexity
| Operation | Time |
|-----------|------|
| Enqueue | O(1) |
| Dequeue | O(1) |
| Peek | O(1) |
| Search | O(n) |
| Space | O(n) |

## 🆚 Comparison
| Type | Order | Use Case |
|------|-------|----------|
| Simple Queue | FIFO | BFS, task scheduling |
| [[Stack]] | LIFO | DFS, undo |
| [[Deque]] | Both ends | Sliding window |
| [[Priority Queue]] | By priority | Heap-based scheduling |

## 🧪 Problems
- [[DSA Problem Bank#Number of Recent Calls]]
- [[DSA Problem Bank#Time Needed to Buy Tickets]]
- [[DSA Problem Bank#Rotting Oranges]]

## ❌ Mistakes
- Array queue: wasting space after dequeue (use circular queue)
- Not checking empty before dequeue

## 🔗 Related Topics
[[Queue]] | [[Circular Queue]] | [[Deque]] | [[Breadth First Search]]

## ⬅️ Previous
[[Queue]]

## ➡️ Next
[[Circular Queue]]

## ✅ Status
- [ ] Learned
- [ ] Practiced
- [ ] Mastered

#DSA #Queue #DataStructure
