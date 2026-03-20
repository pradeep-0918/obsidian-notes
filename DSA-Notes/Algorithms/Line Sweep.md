# Line Sweep

## 📌 Definition
An algorithmic technique that processes geometric or interval problems by sweeping a conceptual line across the input (usually sorted by coordinate), maintaining a data structure of active events.

## ⚙️ How It Works
1. Convert input into events (e.g., interval starts and ends)
2. Sort events by coordinate (x-position or time)
3. Sweep through events, maintaining active set
4. Query or update active set at each event

## 🧠 When to Use
- Interval overlap detection and counting
- Meeting room scheduling
- Rectangle area union
- Skyline problem
- Closest pair of points

## 🔥 Key Properties
- Sort-then-process paradigm
- Active set data structure varies (sorted set, heap, counter)
- Elegant O(n log n) solutions for geometric problems

## ⏱ Complexity
| Phase | Time |
|-------|------|
| Sort events | O(n log n) |
| Process events | O(n log n) |
| Total | O(n log n) |

## 🧪 Problems
- [[DSA Problem Bank#Meeting Rooms II]]
- [[DSA Problem Bank#The Skyline Problem]]
- [[DSA Problem Bank#Car Pooling]]
- [[DSA Problem Bank#My Calendar I]]
- [[DSA Problem Bank#Minimum Number of Arrows to Burst Balloons]]

## ❌ Mistakes
- Not handling tie-breaking in sort order (e.g., ends before starts at same point)
- Wrong event type encoding (start vs end)

## 🔗 Related Topics
[[Sorting]] | [[Priority Queue]] | [[Prefix Sum]] | [[Greedy Algorithms]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #LineSweep #Geometry #Algorithm
