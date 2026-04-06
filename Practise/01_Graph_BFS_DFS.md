# 🌐 Graph — BFS & DFS

**← [[00_DSA_INDEX]] | Next: [[02_Trees_Binary_Trees]] →**

---

## 1. 💡 INTUITION FIRST

> Think of a graph as a **map of cities connected by roads**.
> - **BFS** = Explore city by city, level by level (like **ripples in water**). Use for **shortest path**.
> - **DFS** = Go as deep as possible down one road before backtracking. Use for **connectivity**.

A graph has **nodes** and **edges**. Mark visited nodes to avoid infinite loops.

---

## 2. 🔑 PATTERNS

| Pattern | When to Use |
|---|---|
| BFS | Shortest path, level-order, minimum steps |
| DFS | Connected components, cycle detection, path existence |
| Visited array | Avoid infinite loops |
| Adjacency List | Standard graph representation |

---

## 3. 🧩 PROBLEMS IN THIS NOTE

| # | Problem | Pattern | Difficulty |
|---|---|---|---|
| G1 | Number of Islands | DFS on Grid | Medium |
| G2 | Rotting Oranges | Multi-source BFS | Medium |
| G3 | Shortest Path (BFS) | BFS Distance | Medium |

---

## 4. 📝 TEMPLATES

```java
// Adjacency List Setup
List<List<Integer>> adj = new ArrayList<>();
for (int i = 0; i < n; i++) adj.add(new ArrayList<>());
adj.get(u).add(v);
adj.get(v).add(u); // undirected

// BFS
void bfs(int start) {
    boolean[] visited = new boolean[n];
    Queue<Integer> queue = new LinkedList<>();
    visited[start] = true;
    queue.offer(start);
    while (!queue.isEmpty()) {
        int node = queue.poll();
        for (int neighbor : adj.get(node)) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                queue.offer(neighbor);
            }
        }
    }
}

// DFS (Recursive)
void dfs(int node, boolean[] visited) {
    visited[node] = true;
    for (int neighbor : adj.get(node)) {
        if (!visited[neighbor]) dfs(neighbor, visited);
    }
}
```

---
---

# 🏝️ G1 — Number of Islands (LeetCode #200)

## ❓ Problem Statement

Given a 2D grid of `'1'` (land) and `'0'` (water), count the number of islands.
An island is surrounded by water and formed by connecting adjacent lands horizontally or vertically.

**Input:**
```
grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
```

**Output:** `3`

---

## ✅ ANSWER

The answer is **3 islands**.
- Island 1: top-left cluster (4 cells)
- Island 2: middle cell
- Island 3: bottom-right cluster (2 cells)

---

## 🧠 FEYNMAN TECHNIQUE — Explain It Like I'm 10

> Imagine you're an explorer on a treasure map. Whenever you find land (`'1'`), you start exploring ALL connected land around you (up, down, left, right) and **mark each cell as visited** (turn it to `'0'`). Once you've explored every piece of that island, you count it as ONE island.
>
> Then you keep scanning the map. Every time you find new, **unmarked land**, that's a new island — start exploring again and count again.
>
> **The key:** You only start counting when you hit **fresh** (unvisited) land. DFS does all the "mark everything connected" work for you.

---

## 🔄 STEP-BY-STEP DRY RUN

Grid (simplified to integers, 1=land, 0=water):
```
1 1 0 0
1 1 0 0
0 0 1 0
0 0 0 1
```

| Step | Position | Action | Island Count |
|---|---|---|---|
| Scan (0,0) | Found 1 | DFS → marks (0,0)(0,1)(1,0)(1,1) as 0 | count = 1 |
| Scan (0,1) | Already 0 | Skip | 1 |
| Scan (2,2) | Found 1 | DFS → marks (2,2) as 0 | count = 2 |
| Scan (3,3) | Found 1 | DFS → marks (3,3) as 0 | count = 3 |

**Result: 3** ✅

---

## ☕ JAVA — Full Code

```java
import java.util.*;

public class NumberOfIslands {

    // 4-directional movement: up, down, left, right
    static int[] dr = {-1, 1, 0, 0};
    static int[] dc = {0, 0, -1, 1};

    public static int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0) return 0;

        int rows = grid.length;
        int cols = grid[0].length;
        int count = 0;

        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                if (grid[r][c] == '1') {
                    count++;           // Found a new island
                    dfs(grid, r, c);   // Sink all connected land
                }
            }
        }
        return count;
    }

    static void dfs(char[][] grid, int r, int c) {
        // Boundary check + water check
        if (r < 0 || r >= grid.length || c < 0 || c >= grid[0].length
                || grid[r][c] != '1') return;

        grid[r][c] = '0'; // Mark as visited (sink the land)

        // Explore all 4 directions
        for (int d = 0; d < 4; d++) {
            dfs(grid, r + dr[d], c + dc[d]);
        }
    }

    public static void main(String[] args) {
        // ── INPUT ──
        char[][] grid = {
            {'1','1','0','0','0'},
            {'1','1','0','0','0'},
            {'0','0','1','0','0'},
            {'0','0','0','1','1'}
        };

        // ── PROCESS ──
        int result = numIslands(grid);

        // ── OUTPUT ──
        System.out.println("Number of Islands: " + result);
        // Output: Number of Islands: 3
    }
}
```

---

## 🐍 PYTHON — Full Code

```python
from typing import List

def numIslands(grid: List[List[str]]) -> int:
    if not grid:
        return 0

    rows, cols = len(grid), len(grid[0])
    count = 0

    def dfs(r, c):
        # Boundary check + water check
        if r < 0 or r >= rows or c < 0 or c >= cols or grid[r][c] != '1':
            return
        grid[r][c] = '0'  # Sink the land (mark visited)
        dfs(r - 1, c)     # Up
        dfs(r + 1, c)     # Down
        dfs(r, c - 1)     # Left
        dfs(r, c + 1)     # Right

    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == '1':
                count += 1
                dfs(r, c)

    return count


# ── INPUT ──
grid = [
    ["1","1","0","0","0"],
    ["1","1","0","0","0"],
    ["0","0","1","0","0"],
    ["0","0","0","1","1"]
]

# ── PROCESS & OUTPUT ──
print("Number of Islands:", numIslands(grid))
# Output: Number of Islands: 3
```

---

## 📖 STORY TO REMEMBER — "The Island Explorer"

> **Meet Annie, the island explorer.**
>
> Annie flies over an ocean in a helicopter looking at a map full of land patches. Every time she spots **untouched green land** from above, she shouts "NEW ISLAND!" and **lands her helicopter**.
>
> She then walks in all 4 directions (North, South, East, West), painting every piece of land **grey** as she walks so she won't count it again. She only stops when she hits water or grey land.
>
> Then she flies back up and keeps scanning.
>
> **Annie = your outer loop.** Her walking = DFS. Grey paint = marking `'0'`. Each "NEW ISLAND!" shout = `count++`.

**Memory hook:** `"Spot, Shout, Sink, Scan again"` → Find 1 → count++ → DFS sinks it → continue scanning.

---
---

# 🍊 G2 — Rotting Oranges (LeetCode #994)

## ❓ Problem Statement

Grid: `0` = empty, `1` = fresh orange, `2` = rotten orange.
Each minute, rotten oranges infect fresh adjacent oranges.
Return **minimum minutes** until no fresh oranges remain. Return `-1` if impossible.

**Input:** `[[2,1,1],[1,1,0],[0,1,1]]`
**Output:** `4`

---

## ✅ ANSWER

It takes **4 minutes** for all fresh oranges to rot.

---

## 🧠 FEYNMAN TECHNIQUE

> Imagine a zombie outbreak. Some zombies (rotten oranges) already exist. Every minute, **all zombies simultaneously infect** their neighbors.
>
> The trick is: you don't start BFS from ONE zombie — you start from **ALL zombies at once** (multi-source BFS). You add all initial rotten oranges to the queue FIRST, then do BFS together.
>
> After BFS finishes, if any fresh orange remains → return `-1`. Otherwise, return how many "rounds" (minutes) BFS took.

---

## 🔄 STEP-BY-STEP DRY RUN

```
Initial: 2 1 1
         1 1 0
         0 1 1

Minute 0 (start): queue = [(0,0)]
Minute 1: (0,0) infects (0,1) and (1,0) → [2,2,1 / 2,1,0 / 0,1,1]
Minute 2: (0,1) infects (0,2),(1,1); (1,0) done
Minute 3: (0,2) infects nothing new; (1,1) infects (2,1)
Minute 4: (2,1) infects (2,2)
All fresh oranges rotted. Answer = 4 ✅
```

---

## ☕ JAVA — Full Code

```java
import java.util.*;

public class RottingOranges {

    public static int orangesRotting(int[][] grid) {
        int rows = grid.length, cols = grid[0].length;
        Queue<int[]> queue = new LinkedList<>();
        int freshCount = 0;

        // ── Step 1: Add ALL rotten oranges to queue, count fresh ones ──
        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                if (grid[r][c] == 2) queue.offer(new int[]{r, c});
                if (grid[r][c] == 1) freshCount++;
            }
        }

        if (freshCount == 0) return 0; // No fresh oranges

        int minutes = 0;
        int[] dr = {-1, 1, 0, 0};
        int[] dc = {0, 0, -1, 1};

        // ── Step 2: Multi-source BFS ──
        while (!queue.isEmpty() && freshCount > 0) {
            minutes++;
            int size = queue.size(); // Process all current rotten oranges

            for (int i = 0; i < size; i++) {
                int[] curr = queue.poll();
                for (int d = 0; d < 4; d++) {
                    int nr = curr[0] + dr[d];
                    int nc = curr[1] + dc[d];

                    if (nr >= 0 && nr < rows && nc >= 0 && nc < cols
                            && grid[nr][nc] == 1) {
                        grid[nr][nc] = 2; // Infect!
                        freshCount--;
                        queue.offer(new int[]{nr, nc});
                    }
                }
            }
        }

        // ── Step 3: Check result ──
        return freshCount == 0 ? minutes : -1;
    }

    public static void main(String[] args) {
        // ── INPUT ──
        int[][] grid = {{2,1,1},{1,1,0},{0,1,1}};

        // ── OUTPUT ──
        System.out.println("Minutes to rot all: " + orangesRotting(grid));
        // Output: Minutes to rot all: 4
    }
}
```

---

## 🐍 PYTHON — Full Code

```python
from collections import deque
from typing import List

def orangesRotting(grid: List[List[int]]) -> int:
    rows, cols = len(grid), len(grid[0])
    queue = deque()
    fresh_count = 0

    # ── Step 1: Seed queue with all rotten oranges ──
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 2:
                queue.append((r, c))
            elif grid[r][c] == 1:
                fresh_count += 1

    if fresh_count == 0:
        return 0

    minutes = 0
    directions = [(-1,0),(1,0),(0,-1),(0,1)]

    # ── Step 2: BFS level by level (each level = 1 minute) ──
    while queue and fresh_count > 0:
        minutes += 1
        for _ in range(len(queue)):
            r, c = queue.popleft()
            for dr, dc in directions:
                nr, nc = r + dr, c + dc
                if 0 <= nr < rows and 0 <= nc < cols and grid[nr][nc] == 1:
                    grid[nr][nc] = 2
                    fresh_count -= 1
                    queue.append((nr, nc))

    return minutes if fresh_count == 0 else -1


# ── INPUT ──
grid = [[2,1,1],[1,1,0],[0,1,1]]

# ── OUTPUT ──
print("Minutes to rot all:", orangesRotting(grid))
# Output: Minutes to rot all: 4
```

---

## 📖 STORY TO REMEMBER — "The Zombie Fruit Apocalypse"

> **It's a zombie fruit outbreak in Orange City.**
>
> Day 0: The infected oranges (rotten ones) are already placed all around the grid. The CDC (your BFS queue) rounds up ALL zombies at the same time.
>
> Every minute, ALL zombies simultaneously breathe on their neighbors. If a fresh orange is next door, it turns rotten and **joins next minute's zombie squad**.
>
> The CDC counts how many "rounds" (minutes) it took. If any orange is on an island with no zombies reachable → return -1 (it survived).
>
> **Memory hook:** `"All Zombies First → Infect Together → Count Rounds"` = Multi-source BFS.

---

## ⚠️ COMMON MISTAKES

- ❌ Starting BFS from only one rotten orange → wrong! Start from ALL rotten ones simultaneously
- ❌ Forgetting to check freshCount > 0 in while loop → extra minutes added
- ❌ Not capturing `queue.size()` before the inner for loop → processes wrong batch
- ❌ Returning 0 without checking for isolated fresh oranges first

---

## 🎯 INTERVIEW TIPS

> 🔑 **"Minimum time / steps?"** → BFS
> 🔑 **"Multiple starting points?"** → Multi-source BFS (add ALL sources first)
> 🔑 **Grid problems** → 4-directional neighbors with boundary checks
> 🔑 **Can't reach all nodes?** → Count remaining targets, check > 0 at end

---

## 📋 LEETCODE PRACTICE

| # | Problem | Level | Pattern |
|---|---|---|---|
| 200 | Number of Islands | Medium | DFS on Grid |
| 994 | Rotting Oranges | Medium | Multi-source BFS |
| 542 | 01 Matrix | Medium | BFS Shortest Path |
| 133 | Clone Graph | Medium | BFS + HashMap |
| 997 | Find the Town Judge | Easy | Degree Counting |

---

**← [[00_DSA_INDEX]] | Next: [[02_Trees_Binary_Trees]] →**

*Tags: #graph #bfs #dfs #grid #java #python #dsa*
