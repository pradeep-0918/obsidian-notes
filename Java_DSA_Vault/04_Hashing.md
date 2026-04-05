# 04 — Hashing in Java (HashMap & HashSet)

#java #hashmap #hashset #hashing #dsa
← [[03_Strings]] | Next → [[05_Queue]]

---

## 🧠 Feynman Explain

A **HashMap** is like a dictionary — you look up a **word (key)** to get its **meaning (value)**.  
A **HashSet** is a bag where **duplicates are not allowed** and order doesn't matter.  
Both give you **O(1) average** for insert, delete, lookup.

---

## 🗺️ HashMap — Key → Value

```java
import java.util.HashMap;
import java.util.Map;

HashMap<String, Integer> map = new HashMap<>();

// --- INSERT ---
map.put("apple", 3);
map.put("banana", 5);
map.putIfAbsent("apple", 99);  // Won't overwrite existing

// --- ACCESS ---
map.get("apple");               // 3
map.getOrDefault("mango", 0);  // 0 (safe default)

// --- CHECK ---
map.containsKey("apple");       // true
map.containsValue(5);           // true
map.size();                     // 2
map.isEmpty();                  // false

// --- UPDATE ---
map.put("apple", 10);          // Overwrite
map.replace("apple", 10, 20); // Replace only if current value is 10

// --- DELETE ---
map.remove("banana");
map.remove("apple", 10);       // Remove only if value matches

// --- TRAVERSE ---
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    System.out.println(entry.getKey() + " -> " + entry.getValue());
}
for (String key : map.keySet()) { ... }
for (int val : map.values())   { ... }

// --- FREQUENCY PATTERN (most common!) ---
map.put(key, map.getOrDefault(key, 0) + 1);
// Java 8+ shorthand:
map.merge(key, 1, Integer::sum);
```

---

## 🔢 LinkedHashMap — Insertion Order Preserved

```java
import java.util.LinkedHashMap;
LinkedHashMap<String, Integer> lhm = new LinkedHashMap<>();
// Same API as HashMap, but iteration order = insertion order
```

---

## 🌲 TreeMap — Sorted by Key

```java
import java.util.TreeMap;
TreeMap<Integer, String> tm = new TreeMap<>();
tm.put(3, "c"); tm.put(1, "a"); tm.put(2, "b");
tm.firstKey();          // 1
tm.lastKey();           // 3
tm.floorKey(2);         // 2 (largest key ≤ 2)
tm.ceilingKey(2);       // 2 (smallest key ≥ 2)
tm.headMap(3);          // keys < 3
tm.tailMap(2);          // keys ≥ 2
```

---

## 🧺 HashSet — Unique Elements

```java
import java.util.HashSet;

HashSet<Integer> set = new HashSet<>();

// --- OPERATIONS ---
set.add(10);
set.add(20);
set.add(10);            // Ignored — already present
set.contains(10);       // true
set.remove(10);
set.size();             // 1
set.isEmpty();          // false

// --- TRAVERSE ---
for (int x : set) System.out.println(x);

// --- SET MATH ---
HashSet<Integer> a = new HashSet<>(Arrays.asList(1,2,3,4));
HashSet<Integer> b = new HashSet<>(Arrays.asList(3,4,5,6));

// Union
HashSet<Integer> union = new HashSet<>(a);
union.addAll(b);              // {1,2,3,4,5,6}

// Intersection
HashSet<Integer> inter = new HashSet<>(a);
inter.retainAll(b);           // {3,4}

// Difference
HashSet<Integer> diff = new HashSet<>(a);
diff.removeAll(b);            // {1,2}
```

---

## 🌲 TreeSet — Sorted Unique Elements

```java
import java.util.TreeSet;
TreeSet<Integer> ts = new TreeSet<>();
ts.add(5); ts.add(1); ts.add(3);
ts.first();           // 1
ts.last();            // 5
ts.floor(4);          // 3 (largest ≤ 4)
ts.ceiling(4);        // 5 (smallest ≥ 4)
ts.headSet(4);        // {1, 3} (less than 4)
ts.tailSet(3);        // {3, 5} (≥ 3)
```

---

## 🔄 Classic Patterns

```java
// 1. Two Sum using HashMap
Map<Integer, Integer> seen = new HashMap<>();
for (int i = 0; i < nums.length; i++) {
    int complement = target - nums[i];
    if (seen.containsKey(complement)) return new int[]{seen.get(complement), i};
    seen.put(nums[i], i);
}

// 2. Group Anagrams
Map<String, List<String>> groups = new HashMap<>();
for (String s : strs) {
    char[] arr = s.toCharArray(); Arrays.sort(arr);
    String key = new String(arr);
    groups.computeIfAbsent(key, k -> new ArrayList<>()).add(s);
}

// 3. Sliding Window — count unique
Set<Character> window = new HashSet<>();
```

---

## 🃏 Shortcut Cheatsheet

| Task | Code |
|------|------|
| Put | `map.put(k, v)` |
| Get safe | `map.getOrDefault(k, def)` |
| Frequency | `map.merge(k, 1, Integer::sum)` |
| Has key | `map.containsKey(k)` |
| Iterate | `map.entrySet()` |
| Add to set | `set.add(x)` |
| Has element | `set.contains(x)` |
| Sorted map | `TreeMap<>` |
| Sorted set | `TreeSet<>` |

---

## 🔗 Related Notes
- [[03_Strings]] — String as key
- [[05_Queue]] — BFS uses visited HashSet
- [[08_Graph]] — Adjacency list uses Map
