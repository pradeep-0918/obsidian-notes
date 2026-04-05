# 08 — Graph in Java

#java #graph #bfs #dfs #syntax #dsa
← [[07_LinkedList]] | Next → [[09_Tree]]

---

## 🧠 Feynman Explain

A **Graph** is a map of cities (nodes) connected by roads (edges).  
Roads can be **one-way** (directed) or **two-way** (undirected).  
Roads can have **weights** (distances) or not.

---

## 🏗️ Graph Representation

### 1. Adjacency List (Most Common)

```java
import java.util.*;

// Unweighted
int n = 5; // number of nodes (0 to n-1)
List<List<Integer>> adj = new ArrayList<>();
for (int i = 0; i < n; i++) adj.add(new ArrayList<>());

// Add edge (undirected)
adj.get(u).add(v);
adj.get(v).add(u);

// Add edge (directed)
adj.get(u).add(v);

// Weighted
List<List<int[]>> adj = new ArrayList<>();  // int[] = {neighbor, weight}
for (int i = 0; i < n; i++) adj.add(new ArrayList<>());
adj.get(u).add(new int[]{v, w});
adj.get(v).add(new int[]{u, w});  // undirected
```

---

### 2. Adjacency Matrix

```java
int[][] matrix = new int[n][n];
matrix[u][v] = 1;  // directed edge
matrix[v][u] = 1;  // undirected: also add reverse
// matrix[u][v] = weight  for weighted graph
```

---

### 3. Edge List

```java
int[][] edges = {{0,1},{1,2},{2,3}};  // [from, to] pairs
```

---

## 🔍 BFS — Breadth First Search

```java
boolean[] visited = new boolean[n];
Queue<Integer> q = new LinkedList<>();

q.offer(start);
visited[start] = true;

while (!q.isEmpty()) {
    int node = q.poll();
    System.out.println("Visited: " + node);

    for (int neighbor : adj.get(node)) {
        if (!visited[neighbor]) {
            visited[neighbor] = true;
            q.offer(neighbor);
        }
    }
}
```

> ✅ Use BFS for: **Shortest path in unweighted graph**, level-order traversal.

---

## 🔍 DFS — Depth First Search

### Recursive

```java
boolean[] visited = new boolean[n];

void dfs(int node, List<List<Integer>> adj) {
    visited[node] = true;
    System.out.println("Visited: " + node);
    for (int neighbor : adj.get(node)) {
        if (!visited[neighbor]) dfs(neighbor, adj);
    }
}
```

### Iterative

```java
Deque<Integer> stack = new ArrayDeque<>();
boolean[] visited = new boolean[n];

stack.push(start);
while (!stack.isEmpty()) {
    int node = stack.pop();
    if (visited[node]) continue;
    visited[node] = true;
    for (int neighbor : adj.get(node)) {
        if (!visited[neighbor]) stack.push(neighbor);
    }
}
```

---

## 🔢 BFS Shortest Path (Unweighted)

```java
int[] dist = new int[n];
Arrays.fill(dist, -1);
dist[start] = 0;

Queue<Integer> q = new LinkedList<>();
q.offer(start);

while (!q.isEmpty()) {
    int node = q.poll();
    for (int nb : adj.get(node)) {
        if (dist[nb] == -1) {
            dist[nb] = dist[node] + 1;
            q.offer(nb);
        }
    }
}
```

---

## 🏆 Dijkstra's Algorithm (Weighted Shortest Path)

```java
int[] dist = new int[n];
Arrays.fill(dist, Integer.MAX_VALUE);
dist[src] = 0;

// {distance, node}
PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
pq.offer(new int[]{0, src});

while (!pq.isEmpty()) {
    int[] curr = pq.poll();
    int d = curr[0], u = curr[1];
    if (d > dist[u]) continue;  // Stale entry
    for (int[] edge : adj.get(u)) {
        int v = edge[0], w = edge[1];
        if (dist[u] + w < dist[v]) {
            dist[v] = dist[u] + w;
            pq.offer(new int[]{dist[v], v});
        }
    }
}
```

---

## 🔄 Cycle Detection (Undirected)

```java
boolean hasCycle(int node, int parent, boolean[] visited, List<List<Integer>> adj) {
    visited[node] = true;
    for (int nb : adj.get(node)) {
        if (!visited[nb]) {
            if (hasCycle(nb, node, visited, adj)) return true;
        } else if (nb != parent) return true;  // Back edge = cycle
    }
    return false;
}
```

---

## 🔄 Topological Sort (Directed Acyclic Graph)

### Kahn's Algorithm (BFS)

```java
int[] inDegree = new int[n];
for (int u = 0; u < n; u++)
    for (int v : adj.get(u)) inDegree[v]++;

Queue<Integer> q = new LinkedList<>();
for (int i = 0; i < n; i++) if (inDegree[i] == 0) q.offer(i);

List<Integer> topoOrder = new ArrayList<>();
while (!q.isEmpty()) {
    int node = q.poll();
    topoOrder.add(node);
    for (int nb : adj.get(node)) {
        if (--inDegree[nb] == 0) q.offer(nb);
    }
}
// If topoOrder.size() != n → cycle exists
```

---

## 🃏 Shortcut Cheatsheet

| Task | Code |
|------|------|
| Adj list | `List<List<Integer>> adj` |
| Add edge (undir) | `adj.get(u).add(v); adj.get(v).add(u);` |
| BFS | Queue + visited array |
| DFS | Recursive or Stack + visited |
| Shortest (unweighted) | BFS + dist[] |
| Shortest (weighted) | Dijkstra + PriorityQueue |
| Topo sort | Kahn's (in-degree BFS) |

---

## 🔗 Related Notes
- [[05_Queue]] — BFS uses Queue
- [[06_Stack]] — DFS uses Stack
- [[04_Hashing]] — Visited set, adjacency map
