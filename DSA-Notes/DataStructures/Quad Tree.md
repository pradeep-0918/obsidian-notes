# Quad Tree

## 📌 Definition
A tree where each internal node has exactly **four children**, used to partition 2D space by recursively subdividing each region into four quadrants.

## ⚙️ How It Works
- Root represents the entire 2D space
- Each node subdivides its region into NW, NE, SW, SE quadrants
- Subdivision continues until a stopping criterion (max points, min size)
- Leaf nodes hold actual data points

## 🧠 When to Use
- 2D spatial queries (range search, nearest neighbour in 2D)
- Image compression and processing
- Collision detection in 2D games
- Geographic information systems (GIS)

## 🔥 Key Properties
- Depth proportional to log(area / min_region)
- Unbalanced when data is clustered
- Efficient for sparse 2D data

## ⏱ Complexity
| Operation | Average | Worst |
|-----------|---------|-------|
| Insert | O(log n) | O(n) |
| Search | O(log n) | O(n) |
| Range Query | O(√n) | O(n) |

## 🆚 Comparison
| Structure | Dimensions | Use Case |
|-----------|-----------|----------|
| Quad Tree | 2D | 2D spatial data |
| [[KD Tree]] | k-D | General k-dim |
| [[Octree]] | 3D | 3D spatial data |

## 🧪 Problems
- [[DSA Problem Bank#Construct Quad Tree]]
- [[DSA Problem Bank#K Closest Points to Origin]]

## ❌ Mistakes
- Not handling boundary cases (points on quadrant borders)
- Using when data is highly clustered (use [[KD Tree]] instead)

## 🔗 Related Topics
[[KD Tree]] | [[Octree]] | [[Binary Tree]] | [[Segment Tree]]

## ⬅️ Previous
[[KD Tree]]

## ➡️ Next
[[Octree]]

## ✅ Status
- [ ] Learned
- [ ] Practiced
- [ ] Mastered

#DSA #QuadTree #SpatialDS #DataStructure
