# 05 — Queue & Deque in Java

#java #queue #deque #bfs #syntax #dsa
← [[04_Hashing]] | Next → [[06_Stack]]

---

## 🧠 Feynman Explain

A **Queue** is a line at a ticket counter — **first in, first out (FIFO)**.  
A **Deque** (Double-Ended Queue) is a line where you can join or leave from **both ends**.  
A **PriorityQueue** is a smart queue — smallest (or largest) always comes out first.

---

## 🚦 Queue — FIFO

```java
import java.util.Queue;
import java.util.LinkedList;

Queue<Integer> q = new LinkedList<>();

q.offer(10);         // Add to back (safe — returns false if full)
q.add(20);           // Add to back (throws exception if full)

q.peek();            // View front WITHOUT removing (null if empty)
q.element();         // View front (throws exception if empty)

q.poll();            // Remove & return front (null if empty)
q.remove();          // Remove & return front (throws if empty)

q.size();            // Number of elements
q.isEmpty();         // Check empty
q.contains(10);      // Check element

// Traverse (destructive — avoid unless intentional)
while (!q.isEmpty()) {
    System.out.println(q.poll());
}
```

---

## 🔁 Deque — Double-Ended Queue

```java
import java.util.Deque;
import java.util.ArrayDeque;

Deque<Integer> dq = new ArrayDeque<>();  // Faster than LinkedList

// Add
dq.offerFirst(10);    // Add to FRONT
dq.offerLast(20);     // Add to BACK
dq.addFirst(5);
dq.addLast(25);

// Peek (without removing)
dq.peekFirst();       // Front
dq.peekLast();        // Back

// Remove
dq.pollFirst();       // Remove front
dq.pollLast();        // Remove back
dq.removeFirst();
dq.removeLast();

dq.size();
dq.isEmpty();
```

> 💡 `ArrayDeque` is preferred over `LinkedList` for both Queue and Stack — it's **faster** (no pointer overhead).

---

## ⚡ Using Deque as Stack (Alternative to Stack class)

```java
Deque<Integer> stack = new ArrayDeque<>();
stack.push(10);       // = addFirst
stack.pop();          // = removeFirst
stack.peek();         // = peekFirst
```

---

## 🏆 PriorityQueue — Min-Heap by Default

```java
import java.util.PriorityQueue;

// Min-Heap (smallest on top)
PriorityQueue<Integer> minPQ = new PriorityQueue<>();

// Max-Heap (largest on top)
PriorityQueue<Integer> maxPQ = new PriorityQueue<>((a, b) -> b - a);
// OR:
PriorityQueue<Integer> maxPQ = new PriorityQueue<>(Collections.reverseOrder());

minPQ.offer(30);
minPQ.offer(10);
minPQ.offer(20);

minPQ.peek();          // 10 (min, not removed)
minPQ.poll();          // 10 (min, removed)
minPQ.size();

// Custom object — sort by second element of int[]
PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[1] - b[1]);
pq.offer(new int[]{1, 50});
pq.offer(new int[]{2, 10});
pq.poll();             // [2, 10] — smallest second element
```

---

## 🔄 BFS Template using Queue

```java
Queue<Integer> q = new LinkedList<>();
boolean[] visited = new boolean[n];

q.offer(start);
visited[start] = true;

while (!q.isEmpty()) {
    int node = q.poll();
    for (int neighbor : adj.get(node)) {
        if (!visited[neighbor]) {
            visited[neighbor] = true;
            q.offer(neighbor);
        }
    }
}
```

---

## 🔄 Sliding Window Maximum (Monotonic Deque)

```java
Deque<Integer> dq = new ArrayDeque<>();  // Stores indices
for (int i = 0; i < nums.length; i++) {
    // Remove elements outside window
    while (!dq.isEmpty() && dq.peekFirst() < i - k + 1) dq.pollFirst();
    // Remove smaller elements from back
    while (!dq.isEmpty() && nums[dq.peekLast()] < nums[i]) dq.pollLast();
    dq.offerLast(i);
    if (i >= k - 1) result[i - k + 1] = nums[dq.peekFirst()];
}
```

---

## 🃏 Shortcut Cheatsheet

| Task | Code |
|------|------|
| FIFO Queue | `new LinkedList<>()` |
| Fast Queue/Stack | `new ArrayDeque<>()` |
| Enqueue | `q.offer(x)` |
| Dequeue | `q.poll()` |
| Front peek | `q.peek()` |
| Min-Heap | `new PriorityQueue<>()` |
| Max-Heap | `new PriorityQueue<>((a,b)->b-a)` |
| PQ insert | `pq.offer(x)` |
| PQ top | `pq.poll()` |

---

## 🔗 Related Notes
- [[06_Stack]] — LIFO structure
- [[08_Graph]] — BFS uses Queue
- [[09_Tree]] — Level-order traversal uses Queue
