# 02 — Arrays in Java

#java #array #syntax #dsa
← [[01_Input_Output]] | Next → [[03_Strings]]

---

## 🧠 Feynman Explain

An **array** is a row of labelled boxes. Box 0, box 1, box 2...  
All boxes hold the **same type** of thing and the size is **fixed** once created.

---

## 📦 Declaration & Initialization

```java
// 1D Array
int[] arr = new int[5];                   // [0, 0, 0, 0, 0]
int[] arr = {10, 20, 30, 40, 50};        // Direct init
int[] arr = new int[]{1, 2, 3};          // Explicit new

// 2D Array
int[][] matrix = new int[3][4];           // 3 rows, 4 cols
int[][] matrix = {{1,2},{3,4},{5,6}};    // Direct init

// Jagged Array (rows of different length)
int[][] jagged = new int[3][];
jagged[0] = new int[2];
jagged[1] = new int[4];
```

---

## 🔍 Access & Traverse

```java
arr[0]           // First element
arr[arr.length - 1]  // Last element

// Forward loop
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}

// Enhanced for-each
for (int x : arr) {
    System.out.println(x);
}

// 2D traversal
for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        System.out.print(matrix[i][j] + " ");
    }
}
```

---

## 🛠️ Key Array Operations

```java
import java.util.Arrays;

// Sorting
Arrays.sort(arr);                          // Ascending O(n log n)
Arrays.sort(arr, 0, 5);                   // Sort subarray [0..4]

// Sort descending (only works with Integer[], not int[])
Integer[] boxed = {5, 2, 8, 1};
Arrays.sort(boxed, (a, b) -> b - a);

// Binary Search (array must be sorted!)
int idx = Arrays.binarySearch(arr, target);  // Returns index or negative

// Fill
Arrays.fill(arr, 0);                       // Fill all with 0
Arrays.fill(arr, 2, 5, -1);              // Fill index 2..4 with -1

// Copy
int[] copy = Arrays.copyOf(arr, arr.length);        // Full copy
int[] part = Arrays.copyOfRange(arr, 1, 4);         // Index 1..3

// Compare
Arrays.equals(arr1, arr2);                // true if same content

// Print
System.out.println(Arrays.toString(arr));          // [1, 2, 3]
System.out.println(Arrays.deepToString(matrix));   // 2D print
```

---

## 🔄 Useful Array Patterns

```java
// Reverse an array
for (int l = 0, r = arr.length - 1; l < r; l++, r--) {
    int temp = arr[l];
    arr[l] = arr[r];
    arr[r] = temp;
}

// Find max/min
int max = arr[0];
for (int x : arr) max = Math.max(max, x);

// Prefix sum array
int[] prefix = new int[n + 1];
for (int i = 0; i < n; i++) prefix[i+1] = prefix[i] + arr[i];
// Sum from l..r = prefix[r+1] - prefix[l]

// Frequency count
int[] freq = new int[26];   // for lowercase letters
for (char c : s.toCharArray()) freq[c - 'a']++;
```

---

## 🃏 Shortcut Cheatsheet

| Task | Code |
|------|------|
| Length | `arr.length` |
| Sort asc | `Arrays.sort(arr)` |
| Fill | `Arrays.fill(arr, val)` |
| Copy | `Arrays.copyOf(arr, n)` |
| Print | `Arrays.toString(arr)` |
| Binary search | `Arrays.binarySearch(arr, key)` |
| Compare | `Arrays.equals(a, b)` |

---

## 🔗 Related Notes
- [[03_Strings]] — String ↔ char array conversion
- [[04_Hashing]] — When you need dynamic key-value storage
- [[10_Builtin_Functions]] — Math, Collections utility
