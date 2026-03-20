# ⚙️ Greedy Algorithms

#Algorithm #Greedy

⬅️ [[Backtracking]] | ➡️ [[Dynamic Programming]]

---

## 📌 Definition

A **Greedy Algorithm** makes the locally optimal choice at each step, hoping to find the global optimum. Works when the **greedy choice property** holds.

---

## ⚙️ When Greedy Works

Greedy gives optimal solution when:
1. **Greedy Choice Property** — local best = part of global best
2. **Optimal Substructure** — optimal solution contains optimal sub-solutions

**Greedy vs DP:** If greedy works, prefer it (simpler, faster). Otherwise, use DP.

---

## 🧠 Classic Greedy Problems

| Problem | Strategy |
|---------|---------|
| Activity Selection | Pick earliest finishing time |
| Huffman Coding | Build from least frequent |
| Jump Game | Track max reachable |
| Interval Scheduling | Sort by end time |
| Gas Station | Track cumulative gas |

---

## ⏱ Complexity

Usually O(n log n) due to sorting, O(n) for the greedy pass.

---

## 🧪 Problems

### Medium
- [Jump Game II](https://leetcode.com/problems/jump-game-ii/)
- [Gas Station](https://leetcode.com/problems/gas-station/)
- [Task Scheduler](https://leetcode.com/problems/task-scheduler/)
- [Merge Intervals](https://leetcode.com/problems/merge-intervals/)
- [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)

### Hard
- [Candy](https://leetcode.com/problems/candy/)
- [Minimum Number of Refueling Stops](https://leetcode.com/problems/minimum-number-of-refueling-stops/)

---

## 🔗 Related Topics

- [[Dynamic Programming]] — when greedy doesn't work
- [[Heap]] — greedy with priority
- [[Sorting]] — greedy often needs sorted input

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
