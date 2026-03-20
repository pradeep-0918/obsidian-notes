# 📦 Bloom Filter

#DataStructure #HashBased #Probabilistic

⬅️ [[Hash Set]] | ➡️ [[Disjoint Set Union]]

---

## 📌 Definition

A **Bloom Filter** is a space-efficient probabilistic data structure that tests whether an element is in a set. It may have **false positives** but never false negatives.

---

## ⚙️ How It Works

- Uses k hash functions and a bit array
- Insert: set bits at all k hash positions
- Query: check if all k bits are set

---

## 🔥 Properties

- **No false negatives** — if says "not in set", definitely not
- **Possible false positives** — if says "in set", might not be
- Space: O(m) bits, much smaller than hash set

---

## 🧠 When to Use

- Database lookups (avoid expensive disk reads)
- Web crawlers (seen URL?)
- Spell checkers

---

## 🔗 Related Topics

- [[Hash Set]] — exact membership (no false positives)
- [[Hash Table]] — full key-value storage

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
