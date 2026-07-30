| Pattern               | Representative Problem |
| --------------------- | ---------------------- |
| DFS Traversal         | LC 94                  |
| BFS Traversal         | LC 102                 |
| Zigzag Traversal      | LC 103                 |
| Tree Height           | LC 104                 |
| Balanced Tree         | LC 110                 |
| Diameter              | LC 543                 |
| Path Sum              | LC 112                 |
| Maximum Path Sum      | LC 124                 |
| Same Tree             | LC 100                 |
| Symmetric Tree        | LC 101                 |
| Invert Tree           | LC 226                 |
| LCA                   | LC 236                 |
| Validate BST          | LC 98                  |
| Kth Smallest BST      | LC 230                 |
| BST Iterator          | LC 173                 |
| Recover BST           | LC 99                  |
| Build Tree            | LC 105                 |
| Serialize/Deserialize | LC 297                 |
| Flatten Tree          | LC 114                 |
| Boundary Traversal    | GFG                    |
| Vertical Traversal    | LC 987                 |
| Top/Bottom View       | GFG                    |
| Morris Traversal      | LC 94 (Morris)         |
| Tree DP               | LC 337                 |
| Binary Lifting        | LC 1483                |

# Story Based tree Prb

- LC 863 — All Nodes Distance K in Binary Tree
- LC 968 — Binary Tree Cameras
- LC 979 — Distribute Coins in Binary Tree
- LC 1530 — Number of Good Leaf Nodes Pairs
- LC 2458 — Height of Binary Tree After Subtree Removal Queries
- LC 834 — Sum of Distances in Tree
- LC 310 — Minimum Height Trees
- LC 1377 — Frog Position After T Seconds

# 🌳 TREE PATTERN 1: DFS Traversal (Preorder, Inorder, Postorder)

## 🎯 Pattern Recognition

If the problem says:

- Visit every node
- Count nodes
- Calculate height
- Sum values
- Validate property
- Root → Left → Right

Think **Recursive DFS**.

|CSES|Codeforces|
|---|---|
|Subordinates|763A - Timofey and a Tree|
|Tree Distances I|61D - Eternal Victory|
|Tree Distances II|219D - Choosing Capital for Treeland|
|Tree Matching|580C - Kefa and Park|
|Company Queries I|839C - Journey|
|Company Queries II|337D - Book of Evil|
|Tree Diameter|1187E - Tree Painting|
|Distinct Colors|274B - Zero Tree|
|Counting Paths|1118F1 - Tree Cutting|
|Finding a Centroid|600E - Lomsat Gelral|

---

# 🌳 TREE PATTERN 2: Level Order Traversal (BFS)

**Recognition**

- Level by level
- Distance from root
- Minimum moves
- Queue

|CSES|Codeforces|
|---|---|
|Tree Diameter|1593E - Gardener and Tree|
|Tree Distances I|796C - Bank Hacking|
|Company Queries|161D - Distance in Tree|
|Counting Paths|1829G|
|Tree Matching|734E|
|Subordinates|1076E|
|Tree Distances II|208E|
|Finding a Centroid|682C|
|Distinct Colors|570D|
|Fixed Length Paths I|1611E1|

---

# 🌳 TREE PATTERN 3: Height / Diameter

Recognition:

- Longest path
- Maximum distance
- Deepest node

|CSES|Codeforces|
|---|---|
|Tree Diameter|1187E|
|Tree Distances I|1294F|
|Tree Distances II|1092F|
|Finding a Centroid|61D|
|Subordinates|763A|
|Company Queries|592D|
|Fixed Length Paths I|734E|
|Fixed Length Paths II|1593E|
|Tree Matching|1328E|
|Counting Paths|337D|

---

# 🌳 TREE PATTERN 4: LCA (Lowest Common Ancestor)

Recognition:

- Common ancestor
- Distance between nodes
- Queries

|CSES|Codeforces|
|---|---|
|Company Queries II|208E|
|Distance Queries|191C|
|Counting Paths|1328E|
|Tree Distances II|609E|
|Tree Diameter|519E|
|Company Queries I|342E|
|Fixed Length Paths|519E|
|Path Queries|1702G|
|Subtree Queries|1702G2|
|Tree Algorithms|587C|

---

# 🌳 TREE PATTERN 5: Binary Search Tree

Recognition

- Sorted inorder
- kth smallest
- predecessor
- successor

**CSES doesn't focus on BST-specific problems**, so practice these on LeetCode:

1. LC 98
2. LC 230
3. LC 173
4. LC 99
5. LC 701
6. LC 450
7. LC 235
8. LC 108
9. LC 538
10. LC 669

---

# 🌳 TREE PATTERN 6: Tree DP

Recognition

- Take or don't take
- Independent set
- Maximum score
- Subtree answer

|CSES|Codeforces|
|---|---|
|Tree Matching|1324F|
|Subordinates|219D|
|Tree Distances II|1092F|
|Distinct Colors|600E|
|Counting Paths|1187E|
|Fixed Length Paths|161D|
|Finding Centroid|682C|
|Company Queries|274B|
|Path Queries|763A|
|Tree Diameter|274B|

---

# 🌳 TREE PATTERN 7: Rerooting DP

Recognition

- Answer for every node
- Change root
- Recompute efficiently

|CSES|Codeforces|
|---|---|
|Tree Distances II|1187E|
|Distinct Colors|1092F|
|Tree Diameter|1324F|
|Counting Paths|219D|
|Subordinates|763A|
|Tree Matching|682C|
|Fixed Length Paths|161D|
|Finding Centroid|274B|
|Company Queries|600E|
|Tree Algorithms|1294F|

---

# 🌳 TREE PATTERN 8: Euler Tour / Flatten Tree

Recognition

- Subtree queries
- Range queries
- Fenwick/Segment Tree on trees

|CSES|Codeforces|
|---|---|
|Subtree Queries|383C|
|Path Queries|620E|
|Company Queries|570D|
|Distinct Colors|600E|
|Counting Paths|191C|
|Tree Algorithms|208E|
|Tree Diameter|342E|
|Fixed Length Paths|274B|
|Finding Centroid|682C|
|Path Queries II|519E|

---

# 🌳 TREE PATTERN 9: Binary Lifting

Recognition

- Kth ancestor
- Jump by powers of two
- Fast ancestor queries

|CSES|Codeforces|
|---|---|
|Company Queries I|208E|
|Company Queries II|519E|
|Distance Queries|191C|
|Path Queries|342E|
|Fixed Length Paths|1702G|
|Tree Algorithms|609E|
|Tree Diameter|1328E|
|Counting Paths|587C|
|Subtree Queries|1702G2|
|Tree Matching|208E|

---

# 🌳 TREE PATTERN 10: Heavy-Light / Advanced Tree Queries

|CSES|Codeforces|
|---|---|
|Path Queries II|343D|
|Subtree Queries|620E|
|Path Queries|383C|
|Company Queries II|342E|
|Distance Queries|609E|
|Fixed Length Paths II|1702G2|
|Distinct Colors|600E|
|Tree Algorithms|191C|
|Counting Paths|587C|
|Tree Diameter|1294F|
## Phase 2: Pattern Recognition (Hidden Tree Problems)

Practice problems where you first identify that it is a tree.

|Concept|LeetCode|
|---|---|
|Count Good Nodes|LC 1448|
|Pseudo Palindromic Paths|LC 1457|
|Longest ZigZag Path|LC 1372|
|Deepest Leaves Sum|LC 1302|
|Even Odd Tree|LC 1609|
|Smallest String From Leaf|LC 988|
|Sum Root to Leaf Numbers|LC 129|
|Binary Tree Cameras|LC 968|
|House Robber III|LC 337|
|Distribute Coins|LC 979|

---

# Phase 3: Tree + Graph Mixed Problems

Many Juspay questions blur the line between trees and graphs.

|Problem|Pattern|
|---|---|
|Number of Good Leaf Nodes Pairs (LC 1530)|Tree DP|
|All Nodes Distance K (LC 863)|Tree → Graph|
|Delete Nodes And Return Forest (LC 1110)|DFS|
|Count Complete Tree Nodes (LC 222)|Complete Tree|
|Lowest Common Ancestor II|Variation|
|Path Sum III (LC 437)|Prefix Sum + DFS|

---

# Phase 4: Company-style Word Problems

These are excellent for interviews because they require understanding the story first.

### 1. Company Hierarchy

Given employees and managers, answer:

- number of subordinates
- kth manager
- common manager
- distance between employees

(CSES + Binary Lifting)

---

### 2. File System

```
/
├── home
│   ├── user
│   └── guest
└── var
```

Questions:

- total files
- deepest folder
- path to file
- delete subtree

Pattern:  
DFS

---

### 3. Family Tree

Questions:

- common ancestor
- cousin
- generation
- descendants

Pattern:  
LCA

---

### 4. Network of Routers

Tree because there are no cycles.

Questions:

- shortest communication
- failed router
- farthest router

Pattern:  
Diameter / LCA

---

### 5. Organization Structure

```
CEO
├── CTO
│   ├── Dev1
│   └── Dev2
└── CFO
```

Questions:

- reporting chain
- department size
- manager after k promotions

Pattern:  
DFS + Binary Lifting

---

### 6. Folder Permissions

Grant permission to a folder.

Need to update all descendants.

Pattern:  
Euler Tour

---

### 7. Game Skill Tree

Unlock skills.

Need:

- ancestor
- subtree
- path

Pattern:  
Tree DP

---

### 8. Road Network

Cities connected without cycles.

Need:

- distance
- rerooting
- longest road

Pattern:  
Diameter

---

### 9. Social Referral Tree

Who referred whom.

Need:

- total referrals
- best influencer
- kth referral

Pattern:  
DFS

---

### 10. DNA Evolution Tree

Need:

- common ancestor
- mutation count
- deepest species

Pattern:  
LCA

---

# CSES Problems (Must Do)

These are extremely valuable for interview preparation.

1. Subordinates ✅
2. Company Queries I
3. Company Queries II
4. Tree Distances I
5. Tree Distances II
6. Tree Diameter
7. Tree Matching
8. Tree Algorithms
9. Counting Paths
10. Fixed Length Paths

---

# Codeforces Problems

After CSES, solve around 20–30 tree problems from Codeforces, focusing on ratings **1200–1700**:

- DFS on trees
- Subtree sizes
- Rerooting
- Tree DP
- LCA
- Binary lifting

These improve implementation speed and expose you to varied problem statements.

---

# Interview-Level Challenge Set

After the core patterns, these are some of the hardest and most rewarding tree problems:

- LC 863 — All Nodes Distance K in Binary Tree
- LC 968 — Binary Tree Cameras
- LC 979 — Distribute Coins in Binary Tree
- LC 1530 — Number of Good Leaf Nodes Pairs
- LC 2458 — Height of Binary Tree After Subtree Removal Queries
- LC 834 — Sum of Distances in Tree
- LC 310 — Minimum Height Trees
- LC 1377 — Frog Position After T Seconds