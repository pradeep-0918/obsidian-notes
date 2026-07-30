
| Pattern                          | Representative Question                                       |     |     |
| -------------------------------- | ------------------------------------------------------------- | --- | --- |
| Graph Representation             | Build Graph (Adjacency List)                                  |     |     |
| BFS                              | Number of Islands (LC 200)                                    |     |     |
| DFS                              | Max Area of Island (LC 695)                                   |     |     |
| Connected Components             | Number of Provinces (LC 547)                                  |     |     |
| Flood Fill                       | Flood Fill (LC 733)                                           |     |     |
| Multi-source BFS                 | Rotting Oranges (LC 994)                                      |     |     |
| Cycle Detection (Undirected BFS) | Detect Cycle in Undirected Graph (GFG)                        |     |     |
| Cycle Detection (Undirected DFS) | Detect Cycle in Undirected Graph (GFG)                        |     |     |
| Cycle Detection (Directed)       | Course Schedule (LC 207)                                      |     |     |
| Bipartite Graph                  | Is Graph Bipartite? (LC 785)                                  |     |     |
| Topological Sort (BFS)           | Course Schedule II (LC 210)                                   |     |     |
| Topological Sort (DFS)           | Alien Dictionary (GFG)                                        |     |     |
| Shortest Path (Unweighted)       | Shortest Path in Binary Matrix (LC 1091)                      |     |     |
| Dijkstra                         | Network Delay Time (LC 743)                                   |     |     |
| Bellman-Ford                     | Cheapest Flights Within K Stops (LC 787)                      |     |     |
| Floyd Warshall                   | Find the City With the Smallest Number of Neighbors (LC 1334) |     |     |
| Minimum Spanning Tree (Kruskal)  | Min Cost to Connect All Points (LC 1584)                      |     |     |
| Minimum Spanning Tree (Prim)     | Min Cost to Connect All Points (Prim Version)                 |     |     |
| Union Find (DSU)                 | Redundant Connection (LC 684)                                 |     |     |
| Grid DFS/BFS                     | Surrounded Regions (LC 130)                                   |     |     |
| Word Search                      | Word Search (LC 79)                                           |     |     |
| Backtracking on Graph            | Word Ladder II (LC 126)                                       |     |     |
| Tarjan Bridge                    | Critical Connections in a Network (LC 1192)                   |     |     |
| Strongly Connected Components    | Kosaraju Algorithm (GFG)                                      |     |     |
| Trie + Graph                     | Word Search II (LC 212)                                       |     |     |
# Story based prb in Graph

|Problem|Pattern|
|---|---|
|Word Ladder|LC 127|
|Word Ladder II|LC 126|
|Open the Lock|LC 752|
|Snakes and Ladders|LC 909|
|Bus Routes|LC 815|
|Keys and Rooms|LC 841|
|Reorder Routes|LC 1466|
|Network Delay Time|LC 743|
|Cheapest Flights Within K Stops|LC 787|
|Path With Minimum Effort|LC 1631|
|Swim in Rising Water|LC 778|
|Minimum Genetic Mutation|LC 433|
|Shortest Bridge|LC 934|
|As Far from Land as Possible|LC 1162|
|Pacific Atlantic Water Flow|LC 417|



# 🌐 GRAPH PATTERN 1: BFS / DFS Traversal

### 🎯 Pattern Recognition

If the problem says:

- Visit all nodes
- Count components
- Explore graph
- Reachability
- Grid traversal

👉 Think **DFS / BFS**

## CSES (10)

1. Counting Rooms
2. Labyrinth
3. Building Roads
4. Building Teams
5. Message Route
6. Monsters
7. Round Trip
8. Round Trip II
9. Road Reparation
10. Flight Routes Check

## Codeforces (10)

1. 580C - Kefa and Park
2. 377A - Maze
3. 510B - Fox And Two Dots
4. 987D - Fair
5. 616C - The Labyrinth
6. 796D - Police Stations
7. 242C - King's Path
8. 920E - Connected Components?
9. 893C - Rumor
10. 217A - Ice Skating

---

# 🌐 GRAPH PATTERN 2: Connected Components

### Recognition

- Number of groups
- Provinces
- Islands
- Separate graphs

## CSES

1. Counting Rooms
2. Building Roads
3. Building Teams
4. Flight Routes Check
5. Road Construction
6. Road Reparation
7. Labyrinth
8. Monsters
9. Round Trip
10. Message Route

## Codeforces

1. 217A - Ice Skating
2. 893C - Rumor
3. 920E - Connected Components?
4. 755C - PolandBall and Forest
5. 977E - Cyclic Components
6. 246D - Colorful Graph
7. 731C - Socks
8. 659E - New Reform
9. 427C - Checkposts
10. 1243D - 0-1 MST

---

# 🌐 GRAPH PATTERN 3: Cycle Detection

### Recognition

- Detect cycle
- Infinite loop
- Can finish tasks?
- Valid graph?

## CSES

1. Round Trip
2. Round Trip II
3. Building Teams
4. Flight Routes Check
5. Road Construction
6. Building Roads
7. Course Schedule
8. Game Routes
9. Planets Cycles
10. Planets Queries I

## Codeforces

1. 510B - Fox And Two Dots
2. 263D - Cycle in Graph
3. 1027D - Mouse Hunt
4. 103B - Cthulhu
5. 711D - Directed Roads
6. 131D - Subway
7. 1364D - Ehab's Last Corollary
8. 1679D
9. 1670C
10. 1209D

---

# 🌐 GRAPH PATTERN 4: Bipartite Graph

### Recognition

- Two-coloring
- Friends/Enemies
- Teams
- Odd cycle

## CSES

1. Building Teams
2. Round Trip
3. Labyrinth
4. Monsters
5. Message Route
6. Flight Routes Check
7. Road Construction
8. Building Roads
9. Course Schedule
10. Game Routes

## Codeforces

1. 687A - NP Hard Problem
2. 1093D - Beautiful Graph
3. 1702E - Split Into Two Sets
4. 1949I
5. 862B - Mahmoud and Ehab
6. 1144F - Graph Without Long Directed Paths
7. 360C - Levko and Table
8. 557D - Vitaly and Cycle
9. 1537F
10. 1705E

---

# 🌐 GRAPH PATTERN 5: Topological Sort

### Recognition

- Dependency
- Order
- Prerequisites
- DAG

## CSES

1. Course Schedule
2. Game Routes
3. Longest Flight Route
4. Flight Routes Check
5. Coin Collector
6. Giant Pizza
7. Planets and Kingdoms
8. Investigation
9. Flight Discount
10. High Score

## Codeforces

1. 510C - Fox and Names
2. 919D - Substring
3. 1385E - Directing Edges
4. 1572A - Book
5. 825E - Minimal Labels
6. 1463E - Plan of Lectures
7. 1573C - Book
8. 1037D
9. 129B
10. 1679D

---

# 🌐 GRAPH PATTERN 6: Shortest Path (BFS)

### Recognition

- Minimum moves
- Fewest edges
- Maze
- Grid

## CSES

1. Message Route
2. Labyrinth
3. Monsters
4. Building Roads
5. Building Teams
6. Flight Routes Check
7. Counting Rooms
8. Round Trip
9. Planets Queries
10. High Score

## Codeforces

1. 242C - King's Path
2. 796D - Police Stations
3. 877D - Olya and Energy Drinks
4. 1272E - Nearest Opposite Parity
5. 1063B - Labyrinth
6. 590C - Three States
7. 987D - Fair
8. 1307D - Cow and Fields
9. 1486E
10. 229B

---

# 🌐 GRAPH PATTERN 7: Dijkstra

### Recognition

- Weighted graph
- Minimum cost
- Positive weights

## CSES

1. Shortest Routes I
2. Flight Discount
3. Investigation
4. High Score
5. Flight Routes
6. Road Reparation
7. Download Speed
8. Mail Delivery
9. Coin Collector
10. Longest Flight Route

## Codeforces

1. 20C - Dijkstra?
2. 545E - Paths and Trees
3. 938D - Buy a Ticket
4. 449B - Jzzhu and Cities
5. 1915G
6. 229B
7. 1076D
8. 786B
9. 1486E
10. 1433G

---

# 🌐 GRAPH PATTERN 8: Union Find (DSU)

### Recognition

- Merge groups
- Connectivity
- Redundant edge

## CSES

1. Road Construction
2. Road Reparation
3. Building Roads
4. Flight Routes Check
5. Counting Rooms
6. Building Teams
7. Round Trip
8. Message Route
9. Planets Cycles
10. Planets Queries

## Codeforces

1. 25D - Roads not only in Berland
2. 755C - PolandBall and Forest
3. 920E
4. 939D - Love Rescue
5. 1167C - News Distribution
6. 1245D - Shichikuji
7. 292D
8. 1609D
9. 1466F
10. 891C

---

# 🌐 GRAPH PATTERN 9: Minimum Spanning Tree

### Recognition

- Minimum cost to connect
- Network
- Connect cities

## CSES

1. Road Reparation
2. Road Construction
3. Building Roads
4. Flight Routes Check
5. Download Speed
6. Coin Collector
7. Investigation
8. High Score
9. Shortest Routes I
10. Flight Routes

## Codeforces

1. 609E - Minimum Spanning Tree for Each Edge
2. 1245D
3. 888G
4. 160D
5. 545E
6. 17B
7. 1076D
8. 185A
9. 891C
10. 1108F

---

# 🌐 GRAPH PATTERN 10: Strongly Connected Components (SCC)

### Recognition

- Directed graph
- Mutual reachability
- Condensation graph

## CSES

1. Planets and Kingdoms
2. Coin Collector
3. Flight Routes Check
4. Giant Pizza
5. Game Routes
6. High Score
7. Investigation
8. Longest Flight Route
9. Course Schedule
10. Round Trip II

## Codeforces

1. 427C - Checkposts
2. 999E - Reachability from the Capital
3. 22E - Scheme
4. 402E - Strictly Positive Matrix
5. 1900E
6. 427D
7. 894E
8. 1239D
9. 653D
10. 190E

# Graph Pattern Roadmap

|Pattern|Representative Problem|
|---|---|
|Graph Representation|LC 1971|
|DFS Traversal|LC 841|
|BFS Traversal|LC 841|
|Connected Components|LC 547|
|Number of Islands|LC 200|
|Flood Fill|LC 733|
|Rotten Oranges|LC 994|
|Multi Source BFS|LC 542|
|Bipartite Graph|LC 785|
|Cycle Detection (Undirected)|LC 684|
|Cycle Detection (Directed)|LC 207|
|Topological Sort (BFS)|LC 210|
|Topological Sort (DFS)|LC 210|
|Course Schedule|LC 207|
|Shortest Path (Unweighted)|LC 1091|
|Dijkstra|LC 743|
|Bellman Ford|LC 787|
|Floyd Warshall|LC 1334|
|Union Find (DSU)|LC 547|
|Kruskal MST|LC 1584|
|Prim MST|LC 1584|
|Bridges|LC 1192|
|Articulation Point|GFG|
|SCC (Kosaraju)|LC 802|
|Binary Lifting on Tree|LC 1483|

---

# After Completing These (Hidden Graph Problems)

These are where interviewers hide graph concepts inside real-world stories.

|Problem|Pattern|
|---|---|
|Word Ladder|LC 127|
|Word Ladder II|LC 126|
|Open the Lock|LC 752|
|Snakes and Ladders|LC 909|
|Bus Routes|LC 815|
|Keys and Rooms|LC 841|
|Reorder Routes|LC 1466|
|Network Delay Time|LC 743|
|Cheapest Flights Within K Stops|LC 787|
|Path With Minimum Effort|LC 1631|
|Swim in Rising Water|LC 778|
|Minimum Genetic Mutation|LC 433|
|Shortest Bridge|LC 934|
|As Far from Land as Possible|LC 1162|
|Pacific Atlantic Water Flow|LC 417|

---

# CSES Graph Problems (Must Do)

1. Counting Rooms
2. Labyrinth
3. Building Roads
4. Message Route
5. Building Teams
6. Round Trip
7. Round Trip II
8. Course Schedule
9. Flight Routes Check
10. Longest Flight Route
11. Shortest Routes I
12. Shortest Routes II
13. High Score
14. Flight Discount
15. Investigation

---

# Codeforces (1200–1700)

After CSES, solve around **30 graph problems** covering:

- BFS
- DFS
- Topological Sort
- DSU
- Dijkstra
- Trees as graphs
- Shortest paths
- Greedy + Graph
- Bitmask + Graph

---

# Juspay-Style Word Problems

Instead of saying "Find the shortest path," the interviewer might ask:

### 1. Metro Network 🚇

- Minimum stations to travel
- Closed station handling
- Alternate route

**Pattern:** BFS / Dijkstra

---

### 2. Computer Network 🌐

- Can every computer communicate?
- Which cable should be repaired?
- Minimum cables needed

**Pattern:** Connected Components / DSU / MST

---

### 3. Social Network 👥

- Friend recommendations
- Mutual friends
- Communities

**Pattern:** BFS / DFS

---

### 4. Flight Booking ✈️

- Cheapest ticket
- Maximum stop limit
- Fastest route

**Pattern:** Dijkstra / Bellman-Ford

---

### 5. Food Delivery 🚚

- Nearest restaurant
- Multiple delivery hubs
- Minimum delivery time

**Pattern:** Multi-Source BFS

---

### 6. Dependency Manager 📦

- Package installation order
- Circular dependencies
- Build system

**Pattern:** Topological Sort

---

### 7. Road Construction 🛣️

- Connect all cities
- Minimum construction cost
- Redundant roads

**Pattern:** MST / DSU

---

### 8. Virus Spread 🦠

- Infection time
- Safe computers
- First infected node

**Pattern:** BFS

---

### 9. Water Pipeline 💧

- Can water reach every house?
- Broken pipes
- Critical pipe

**Pattern:** DFS / Bridges

---

### 10. Online Gaming 🎮

- Match players
- Guild connectivity
- Team balancing

**Pattern:** Bipartite / DSU

---

# Advanced Interview Problems

Once you've mastered the basics, these are excellent for Juspay-level interviews:

- LC 269 — Alien Dictionary
- LC 685 — Redundant Connection II
- LC 847 — Shortest Path Visiting All Nodes
- LC 882 — Reachable Nodes In Subdivided Graph
- LC 2203 — Minimum Weighted Subgraph With the Required Paths
- LC 1514 — Path With Maximum Probability
- LC 1293 — Shortest Path in a Grid with Obstacles Elimination
- LC 2290 — Minimum Obstacle Removal to Reach Corner
- LC 1786 — Number of Restricted Paths From First to Last Node
- LC 1631 — Path With Minimum Effort