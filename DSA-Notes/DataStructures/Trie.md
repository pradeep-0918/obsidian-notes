# 🔡 Trie (Prefix Tree)
#DataStructure #NonLinear #Tree #String

⬅️ [[Fenwick Tree]] | ➡️ [[Heap]]

---

## 📌 Definition
A **trie** (retrieval tree) is a tree where each node represents a character, and paths from root to nodes represent strings. Used for efficient prefix-based operations.

---

## ⚙️ How It Works
```
Insert: "cat", "car", "cart", "card"

        root
         |
         c
         |
         a
        / \
       t   r
      (✓)  |
           t — (✓)
           |
           d (✓)
```
- `✓` marks end of word
- Children accessed via character (26-length array or dict)

---

## 🧠 When to Use
- Autocomplete / search suggestions
- Spell checking
- IP routing tables
- Word puzzle solving
- Dictionary-based problems

---

## ⏱ Complexity
Let `m` = length of word, `n` = number of words

| Operation | Time | Space |
|---|---|---|
| Insert | O(m) | O(m) |
| Search | O(m) | — |
| Starts With | O(m) | — |
| Total Space | — | O(n·m) |

---

## 🔥 Implementation
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end = False

class Trie:
    def __init__(self):
        self.root = TrieNode()
    
    def insert(self, word):
        node = self.root
        for ch in word:
            if ch not in node.children:
                node.children[ch] = TrieNode()
            node = node.children[ch]
        node.is_end = True
    
    def search(self, word):
        node = self.root
        for ch in word:
            if ch not in node.children: return False
            node = node.children[ch]
        return node.is_end
```

---

## 🧪 Problems
- [ ] [Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/) — Medium
- [ ] [Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/) — Medium
- [ ] [Search Suggestions System](https://leetcode.com/problems/search-suggestions-system/) — Medium
- [ ] [Maximum XOR of Two Numbers in an Array](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/) — Medium
- [ ] [Word Search II](https://leetcode.com/problems/word-search-ii/) — Hard

---

## 🔗 Related Topics
- [[String]]
- [[Hash Map]] — Alternative for children
- [[Backtracking]] — Word search
- [[Bit Manipulation]] — XOR Trie

---

## ✅ Status
- [ ] Learned
- [ ] Practiced
- [ ] Mastered
