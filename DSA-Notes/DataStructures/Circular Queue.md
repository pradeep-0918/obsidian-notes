# Circular Queue

## 📌 Definition
A [[Queue]] implementation using a fixed-size array where the rear wraps around to the front, efficiently reusing freed space — also called a Ring Buffer.

## ⚙️ How It Works
- Uses an array of size N with `head` and `tail` pointers
- `tail = (tail + 1) % N` on enqueue
- `head = (head + 1) % N` on dequeue
- Full: `(tail + 1) % N == head`; Empty: `head == tail`

## 🧠 When to Use
- Fixed-size buffering (audio streams, network packets)
- CPU scheduling (Round Robin)
- Producer-consumer with fixed capacity

## 🔥 Key Properties
- No wasted space (unlike simple array queue)
- O(1) enqueue and dequeue
- Fixed maximum capacity

## ⏱ Complexity
| Operation | Time |
|-----------|------|
| Enqueue | O(1) |
| Dequeue | O(1) |
| Peek | O(1) |
| Space | O(n) |

## 🆚 Comparison
| Queue Type | Resizable | Wasted Space | Use Case |
|-----------|-----------|-------------|----------|
| [[Simple Queue]] | Yes | Yes (array) | General |
| Circular Queue | No | No | Fixed buffer |
| [[Deque]] | Yes | No | Double-ended |

## 🧪 Problems
- [[DSA Problem Bank#Design Browser History]]
- [[DSA Problem Bank#Sliding Window Maximum]]

## ❌ Mistakes
- Off-by-one in full/empty conditions
- Not applying modulo correctly

## 🔗 Related Topics
[[Queue]] | [[Simple Queue]] | [[Deque]] | [[Monotonic Queue]]

## ⬅️ Previous
[[Simple Queue]]

## ➡️ Next
[[Deque]]

## ✅ Status
- [ ] Learned
- [ ] Practiced
- [ ] Mastered

#DSA #Queue #CircularQueue #DataStructure
