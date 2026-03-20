# 🔙 Backtracking
#Algorithm #Paradigm

⬅️ [[Recursion]] | ➡️ [[Divide and Conquer]]

---

## 📌 Definition
**Backtracking** is a recursive algorithm that builds solutions incrementally, **abandoning** partial solutions (backtracking) when they cannot lead to a valid solution.

---

## ⚙️ Template
```python
def backtrack(path, choices):
    if is_solution(path):
        result.append(path[:])
        return
    
    for choice in choices:
        if is_valid(choice):
            path.append(choice)      # Make choice
            backtrack(path, next_choices)
            path.pop()               # Undo choice (backtrack)
```

---

## 🔥 Classic Problems
1. **Permutations** — all orderings
2. **Subsets** — all possible subsets
3. **Combinations** — choose k from n
4. **N-Queens** — constraint satisfaction
5. **Sudoku Solver** — fill grid
6. **Word Search** — path in grid

---

## 🧠 When to Use
- Find all possible solutions
- Constraint satisfaction (Sudoku, N-Queens)
- Combinatorial problems (permutations, subsets)
- Path finding with constraints

---

## ⏱ Complexity
Typically exponential — O(n! ), O(2^n), or O(n^k) depending on the problem. Pruning reduces practical runtime.

---

## 🧪 Problems
- [ ] [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/) — Medium
- [ ] [Permutations](https://leetcode.com/problems/permutations/) — Medium
- [ ] [Subsets](https://leetcode.com/problems/subsets/) — Medium
- [ ] [Combination Sum](https://leetcode.com/problems/combination-sum/) — Medium
- [ ] [Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/) — Medium
- [ ] [N-Queens](https://leetcode.com/problems/n-queens/) — Hard
- [ ] [Word Search II](https://leetcode.com/problems/word-search-ii/) — Hard

---

## ❌ Common Mistakes
- Forgetting to undo the choice (backtrack step)
- Not adding pruning conditions
- Modifying path in place without copying when saving

---

## 🔗 Related Topics
- [[Recursion]]
- [[Depth First Search]]
- [[Dynamic Programming]] — When subproblems overlap

---

## ✅ Status
- [ ] Learned
- [ ] Practiced
- [ ] Mastered
