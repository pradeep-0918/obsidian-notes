# ⚡ Final Revision Sheet

**← [[05_Dynamic_Programming]] | Home: [[00_DSA_INDEX]] →**

> 🔁 **Read this LAST — after going through all topic notes.**
> This is your 10-minute pre-interview power review.

---

## 🗺️ MASTER PATTERN TABLE

```
╔══════════════════════════════════════════════════════════════════════╗
║                    DSA PATTERN CHEAT SHEET                          ║
╠══════════════════════════════════════════════════════════════════════╣
║ GRAPH                                                                ║
║  BFS  → Queue + visited[] → Shortest path, level traversal          ║
║  DFS  → Stack/Recursion + visited[] → Connectivity, paths           ║
║  Grid → Treat cell as node, 4 directions as edges                   ║
╠══════════════════════════════════════════════════════════════════════╣
║ TREES                                                                ║
║  Recursion  → if(null) return; solve left; solve right              ║
║  Height     → postorder (bottom-up): 1 + max(left, right)           ║
║  Level Order→ BFS with size capture before inner loop               ║
║  Path Sum   → DFS, subtract val, check at leaf                      ║
║  LCA        → if found p or q, return that node                     ║
╠══════════════════════════════════════════════════════════════════════╣
║ STACK                                                                ║
║  Parentheses  → push open, pop+check on close, isEmpty at end       ║
║  Next Greater → Monotonic stack with INDICES                        ║
║  Daily Temps  → Monotonic stack: answer = i - popped_index          ║
╠══════════════════════════════════════════════════════════════════════╣
║ LINKED LIST                                                          ║
║  Reverse  → prev=null, save next, flip, advance both                ║
║  Cycle    → fast/slow: if they meet → cycle exists                  ║
║  Middle   → fast/slow: slow stops at middle                         ║
║  Remove N → dummy + fast goes n+1 steps ahead                       ║
╠══════════════════════════════════════════════════════════════════════╣
║ DYNAMIC PROGRAMMING                                                  ║
║  Fibonacci → dp[i] = dp[i-1] + dp[i-2]                             ║
║  Robber    → dp[i] = max(dp[i-1], nums[i]+dp[i-2])                 ║
║  Knapsack  → dp[i][w] = max(skip, take if fits)                     ║
║  LCS       → match: 1+dp[i-1][j-1] | no match: max(left,above)     ║
╚══════════════════════════════════════════════════════════════════════╝
```

---

## ⚡ QUICK SIGNAL → PATTERN MAP

| If problem says... | Use this |
|---|---|
| Shortest path / minimum steps | BFS |
| Is there a path? Connected? | DFS |
| Level by level / row by row | BFS on tree (level order) |
| Height / depth / balanced? | Postorder recursion |
| Next greater / next smaller | Monotonic Stack |
| Matching brackets / pairs | Stack |
| Cycle in linked list | Fast/Slow pointers |
| Middle of linked list | Fast/Slow pointers |
| Count ways / max value | DP |
| Two strings → common pattern | 2D DP (LCS) |
| Pick or skip each item | 0/1 Knapsack DP |
| Grid with 0s and 1s | BFS/DFS — cells are nodes |
| Multiple starting points | Multi-source BFS |

---

## 🧱 CODE SKELETONS (memorize these)

### BFS Skeleton
```java
Queue<Integer> q = new LinkedList<>();
boolean[] visited = new boolean[n];
q.offer(start); visited[start] = true;
while (!q.isEmpty()) {
    int node = q.poll();
    for (int nb : adj.get(node))
        if (!visited[nb]) { visited[nb]=true; q.offer(nb); }
}
```

### Tree Recursion Skeleton
```java
int solve(TreeNode root) {
    if (root == null) return BASE_VALUE;
    int left = solve(root.left);
    int right = solve(root.right);
    return COMBINE(left, right, root.val);
}
```

### Monotonic Stack Skeleton
```java
Stack<Integer> stack = new Stack<>(); // stores indices
for (int i = 0; i < n; i++) {
    while (!stack.isEmpty() && nums[i] > nums[stack.peek()])
        result[stack.pop()] = nums[i]; // or i - popped
    stack.push(i);
}
```

### Linked List Reversal Skeleton
```java
ListNode prev = null, curr = head;
while (curr != null) {
    ListNode next = curr.next;
    curr.next = prev;
    prev = curr; curr = next;
}
return prev;
```

### DP 1D Skeleton
```java
int[] dp = new int[n + 1];
dp[0] = BASE; dp[1] = BASE;
for (int i = 2; i <= n; i++)
    dp[i] = RECURRENCE(dp[i-1], dp[i-2]);
```

### DP 2D Skeleton (LCS)
```java
int[][] dp = new int[m+1][n+1];
for (int i = 1; i <= m; i++)
    for (int j = 1; j <= n; j++)
        dp[i][j] = (match) ? 1+dp[i-1][j-1] : max(dp[i-1][j], dp[i][j-1]);
```

---

## 📚 ALL STORIES IN ONE PLACE

| Topic | Story Character | Memory Hook |
|---|---|---|
| Graph DFS (Islands) | Annie the Explorer | "Spot → Shout → Sink → Scan" |
| Graph BFS (Oranges) | Zombie Fruit Outbreak | "All Zombies First → Infect Together → Count Rounds" |
| Tree Depth | Deep-sea Diver Dan | "Split → Dive → Pick Deeper → Add 1 → Return Up" |
| Tree Level Order | School Photo Day | "Count class → Photo → Add kids → Next class" |
| Tree Path Sum | Hiker Harry | "Pick berries → Need less → Dead end check" |
| Stack Parentheses | Nest-Checker Nina | "Open → Push. Close → Pop & Match. End → Empty?" |
| Stack Next Greater | Height-Jealous Queue | "Taller arrives → resolve shorter waiting ones" |
| Stack Daily Temp | Beach-Waiting Sam | "Cold → Stack index. Warm → Pop & Calculate distance" |
| Linked List Reverse | Reverse Parade Marshal | "Save next → Flip → Slide prev → Slide curr" |
| Linked List Cycle | Track Runners | "Two runners → If loop → Fast laps slow → They meet" |
| Linked List Remove N | Gap Runner Marshals | "Scout n+1 ahead → Walk together → Scout ends → Skip target" |
| DP Climbing Stairs | Sam's Staircase | "To reach N = from N-1 or N-2" |
| DP House Robber | Robbie the Robber | "Rob = this + 2 back. Skip = 1 back. Max. Remember last 2." |
| DP LCS | Alice and Bob's Word Game | "Match → diagonal+1. No match → max(left, above)" |

---

## 🎯 LEETCODE MUST-DO LIST

### 🟢 Easy (Do these first)
- [ ] #200 Number of Islands
- [ ] #104 Maximum Depth of Binary Tree
- [ ] #226 Invert Binary Tree
- [ ] #112 Path Sum
- [ ] #20 Valid Parentheses
- [ ] #496 Next Greater Element I
- [ ] #206 Reverse Linked List
- [ ] #141 Linked List Cycle
- [ ] #876 Middle of the Linked List
- [ ] #70 Climbing Stairs
- [ ] #509 Fibonacci Number

### 🟡 Medium (Do these second)
- [ ] #994 Rotting Oranges
- [ ] #102 Binary Tree Level Order Traversal
- [ ] #236 Lowest Common Ancestor
- [ ] #155 Min Stack
- [ ] #739 Daily Temperatures
- [ ] #19 Remove Nth Node From End
- [ ] #198 House Robber
- [ ] #1143 Longest Common Subsequence
- [ ] #416 Partition Equal Subset Sum

---

## 💡 TOP 10 INTERVIEW TIPS

1. **Say your approach first** before touching the keyboard
2. **Ask clarifying questions:** directed/undirected? cycles possible? null inputs?
3. **Always handle null/empty** at the start of every function
4. **Use a dummy node** in linked list problems when head might change
5. **Check isEmpty() before peek()/pop()** in stack problems
6. **In BFS level order:** capture `queue.size()` BEFORE the inner for loop
7. **In DP:** always define what `dp[i]` means as a comment — examiners love this
8. **In monotonic stack:** push INDICES not VALUES (unless values are unique)
9. **Fast/slow pointer:** condition must be `fast != null && fast.next != null`
10. **Stuck?** Think brute-force first → optimize → this shows problem-solving process

---

## 🧠 THE DP CONVERSION CHECKLIST

```
□ 1. Write recursive solution first (identify base cases)
□ 2. Which parameters change? → Those are your dp indices
□ 3. Create memo[] with size = range of those parameters
□ 4. Initialize memo to -1 (not 0!)
□ 5. Check memo at function start, store before returning
□ 6. (Optional) Reverse loops → tabulation
□ 7. (Optional) Do you only need last 1-2 values? → Space optimize
```

---

*🚀 You've got this. Pattern recognition + calm thinking = success.*

**← [[05_Dynamic_Programming]] | Home: [[00_DSA_INDEX]] →**

*Tags: #revision #cheatsheet #patterns #dsa #interview*
