# 📚 Stack

**← [[02_Trees_Binary_Trees]] | Next: [[04_Linked_List]] →**

---

## 1. 💡 INTUITION FIRST

> A Stack is **Last In, First Out (LIFO)** — like a pile of plates. You can only add or remove from the top.
>
> **Core insight:** Use a stack when you need to **remember the most recent unresolved thing** and compare it with something new.
>
> Ask yourself: *"Do I need to track the LAST thing I saw?"* → Stack.

---

## 2. 🔑 PATTERNS

| Pattern | Problems |
|---|---|
| Bracket Matching | Valid Parentheses |
| Monotonic Stack | Next Greater Element, Daily Temperatures |
| Min/Max Tracking | Min Stack |
| Expression Evaluation | RPN Calculator |

---

## 3. 🧩 PROBLEMS IN THIS NOTE

| # | Problem | Pattern | LeetCode |
|---|---|---|---|
| S1 | Valid Parentheses | Bracket Matching | #20 |
| S2 | Next Greater Element | Monotonic Stack | #496 |
| S3 | Daily Temperatures | Monotonic Stack | #739 |

---
---

# 🔗 S1 — Valid Parentheses (LeetCode #20)

## ❓ Problem Statement

Given a string `s` containing `(`, `)`, `{`, `}`, `[`, `]`, determine if the input string is **valid**.
Valid means: every open bracket is closed by the **same type** in the **correct order**.

**Input:** `s = "()[]{}"` → **Output:** `true`
**Input:** `s = "([)]"` → **Output:** `false`
**Input:** `s = "{[]}"` → **Output:** `true`

---

## ✅ ANSWER

- `"()[]{}"` → **true**
- `"([)]"` → **false**
- `"{[]}"` → **true**

---

## 🧠 FEYNMAN TECHNIQUE

> Imagine you're reading a recipe that has nested instructions: `{Mix [stir (beat) stir] mix}`.
>
> Every time you see an **opening instruction** (`{`, `[`, `(`), you write it on a notepad (push to stack).
>
> Every time you see a **closing instruction** (`)`, `]`, `}`), you check: does it match the **LAST** opening instruction on your notepad? If yes → cross it off (pop). If no → the recipe is invalid.
>
> At the end, if your notepad is empty → all instructions matched → valid!
>
> **Key insight:** The MOST RECENT opening bracket must be closed FIRST = LIFO = Stack.

---

## 🔄 STEP-BY-STEP DRY RUN

Input: `"{[]}"`

| Char | Action | Stack |
|---|---|---|
| `{` | Push `{` | `[{]` |
| `[` | Push `[` | `[{, []` |
| `]` | Pop `[`, matches `[` ✅ | `[{]` |
| `}` | Pop `{`, matches `{` ✅ | `[]` |
| End | Stack empty → **true** ✅ | `[]` |

Input: `"([)]"`

| Char | Action | Stack |
|---|---|---|
| `(` | Push | `[(]` |
| `[` | Push | `[(, []` |
| `)` | Pop `[`, mismatch! → **false** ❌ | - |

---

## ☕ JAVA — Full Code

```java
import java.util.*;

public class ValidParentheses {

    public static boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();

        for (char c : s.toCharArray()) {
            // Push all opening brackets
            if (c == '(' || c == '{' || c == '[') {
                stack.push(c);
            } else {
                // Closing bracket: stack must not be empty
                if (stack.isEmpty()) return false;

                char top = stack.pop();
                // Check for mismatch
                if (c == ')' && top != '(') return false;
                if (c == '}' && top != '{') return false;
                if (c == ']' && top != '[') return false;
            }
        }

        // Stack must be empty (all brackets matched)
        return stack.isEmpty();
    }

    public static void main(String[] args) {
        // ── INPUT & OUTPUT ──
        String[] inputs = {"()[]{}", "([)]", "{[]}", "(("};

        for (String s : inputs) {
            System.out.println("\"" + s + "\" → " + isValid(s));
        }
        // Output:
        // "()[]{}" → true
        // "([)]"   → false
        // "{[]}"   → true
        // "(("     → false
    }
}
```

---

## 🐍 PYTHON — Full Code

```python
def isValid(s: str) -> bool:
    stack = []
    mapping = {')': '(', '}': '{', ']': '['}

    for char in s:
        if char in '({[':
            stack.append(char)
        else:
            # Closing bracket
            if not stack or stack[-1] != mapping[char]:
                return False
            stack.pop()

    return len(stack) == 0


# ── INPUT & OUTPUT ──
test_cases = ["()[]{}", "([)]", "{[]}", "(("]
for s in test_cases:
    print(f'"{s}" → {isValid(s)}')

# Output:
# "()[]{}" → True
# "([)]"   → False
# "{[]}"   → True
# "(("     → False
```

---

## 📖 STORY TO REMEMBER — "The Nest-Checker Nina"

> **Nina builds bird nests and HATES mismatched brackets.**
>
> Every time she opens a nest layer `{`, `[`, `(`, she puts a tag on her clipboard (push).
>
> Every time she closes a layer, she checks: does the closing tag match the **TOP** tag on her clipboard? If yes → peel it off (pop) and continue. If no → the nest is BROKEN.
>
> If she finishes and the clipboard is clean → perfect nest! If clipboard still has tags → unclosed brackets.
>
> **Memory hook:** `"Open → Push. Close → Pop & Match. End → Empty?"`

---
---

# ⬆️ S2 — Next Greater Element (LeetCode #496)

## ❓ Problem Statement

Given two arrays `nums1` (subset of `nums2`), for each element in `nums1`, find the **next greater element** in `nums2` (next element to the right that is larger). Return `-1` if none.

**Input:** `nums1 = [4,1,2]`, `nums2 = [1,3,4,2]`
**Output:** `[-1,3,-1]`

---

## ✅ ANSWER

- 4 → no element > 4 to its right in nums2 → **-1**
- 1 → next greater to right is 3 → **3**
- 2 → no element > 2 to its right → **-1**

---

## 🧠 FEYNMAN TECHNIQUE

> You're in a queue watching people's heights. For each person, you want to know: who is the **next taller person** behind them?
>
> You walk from left to right. Keep a stack of "unresolved" people (indices). When you encounter someone **taller** than the person at the top of the stack — THAT'S their next greater!
>
> Pop that person out, record their answer, and keep going.
>
> **The stack stays in decreasing order** (that's why it's called a monotonic stack).

---

## 🔄 STEP-BY-STEP DRY RUN

`nums2 = [1, 3, 4, 2]`

| i | nums2[i] | Stack (indices) | Action | result |
|---|---|---|---|---|
| 0 | 1 | [] | push 0 | {0: -1} |
| 1 | 3 | [0] | 3>1 → pop 0, result[0]=3; push 1 | {0:3} |
| 2 | 4 | [1] | 4>3 → pop 1, result[1]=4; push 2 | {1:4} |
| 3 | 2 | [2] | 2<4, push 3 | unchanged |
| end | - | [2,3] | remaining → -1 | {2:-1, 3:-1} |

Map: `{1→3, 3→4, 4→-1, 2→-1}`
For `nums1=[4,1,2]` → `[-1, 3, -1]` ✅

---

## ☕ JAVA — Full Code

```java
import java.util.*;

public class NextGreaterElement {

    public static int[] nextGreaterElement(int[] nums1, int[] nums2) {
        // Map each value in nums2 to its next greater element
        Map<Integer, Integer> ngeMap = new HashMap<>();
        Stack<Integer> stack = new Stack<>(); // stores values, not indices

        for (int num : nums2) {
            // While current num is greater than top of stack
            while (!stack.isEmpty() && num > stack.peek()) {
                ngeMap.put(stack.pop(), num); // found next greater
            }
            stack.push(num);
        }

        // Remaining elements in stack have no next greater
        while (!stack.isEmpty()) {
            ngeMap.put(stack.pop(), -1);
        }

        // Build result for nums1
        int[] result = new int[nums1.length];
        for (int i = 0; i < nums1.length; i++) {
            result[i] = ngeMap.get(nums1[i]);
        }
        return result;
    }

    public static void main(String[] args) {
        // ── INPUT ──
        int[] nums1 = {4, 1, 2};
        int[] nums2 = {1, 3, 4, 2};

        // ── OUTPUT ──
        int[] result = nextGreaterElement(nums1, nums2);
        System.out.println("Next Greater Elements: " + Arrays.toString(result));
        // Output: Next Greater Elements: [-1, 3, -1]
    }
}
```

---

## 🐍 PYTHON — Full Code

```python
from typing import List

def nextGreaterElement(nums1: List[int], nums2: List[int]) -> List[int]:
    nge_map = {}
    stack = []  # monotonic decreasing stack (stores values)

    for num in nums2:
        while stack and num > stack[-1]:
            nge_map[stack.pop()] = num
        stack.append(num)

    # Remaining have no next greater
    while stack:
        nge_map[stack.pop()] = -1

    return [nge_map[n] for n in nums1]


# ── INPUT ──
nums1 = [4, 1, 2]
nums2 = [1, 3, 4, 2]

# ── OUTPUT ──
print("Next Greater Elements:", nextGreaterElement(nums1, nums2))
# Output: Next Greater Elements: [-1, 3, -1]
```

---

## 📖 STORY TO REMEMBER — "The Height-Jealous Queue"

> **At a concert, everyone wants to know: who is the next TALLER person behind me?**
>
> You walk the queue left to right. You keep a notepad of "still waiting" people (the stack — decreasing heights).
>
> When you meet someone tall, you look at your notepad. Anyone **shorter** than this new person? — they finally found their answer! Cross them off with "your next greater is this tall person."
>
> Anyone still on the notepad at the end? Their answer is -1 (no one taller came).
>
> **Memory hook:** `"Taller arrives → resolve shorter waiting ones → LIFO order"`

---
---

# 🌡️ S3 — Daily Temperatures (LeetCode #739)

## ❓ Problem Statement

Given an array `temperatures`, return an array `answer` where `answer[i]` = how many days until a **warmer temperature**. If no warmer day exists, `answer[i] = 0`.

**Input:** `temperatures = [73,74,75,71,69,72,76,73]`
**Output:** `[1,1,4,2,1,1,0,0]`

---

## ✅ ANSWER

`[1, 1, 4, 2, 1, 1, 0, 0]`

---

## 🧠 FEYNMAN TECHNIQUE

> Each day, you're waiting for a warmer day to go to the beach. You keep a list of "pending" days (stack of indices). When a warmer day arrives, you calculate: "how many days did those pending days wait?"
>
> **Answer[i] = current_index - pending_index**
>
> Same monotonic stack pattern: stack stores INDICES, we compare VALUES.

---

## ☕ JAVA — Full Code

```java
import java.util.*;

public class DailyTemperatures {

    public static int[] dailyTemperatures(int[] temperatures) {
        int n = temperatures.length;
        int[] answer = new int[n]; // default 0
        Stack<Integer> stack = new Stack<>(); // stores indices

        for (int i = 0; i < n; i++) {
            // Current temp is warmer than temp at stack's top index
            while (!stack.isEmpty() && temperatures[i] > temperatures[stack.peek()]) {
                int prevDay = stack.pop();
                answer[prevDay] = i - prevDay; // days waited
            }
            stack.push(i);
        }
        // Remaining indices in stack → answer stays 0 (no warmer day)
        return answer;
    }

    public static void main(String[] args) {
        // ── INPUT ──
        int[] temps = {73, 74, 75, 71, 69, 72, 76, 73};

        // ── OUTPUT ──
        System.out.println("Days until warmer: " + Arrays.toString(dailyTemperatures(temps)));
        // Output: Days until warmer: [1, 1, 4, 2, 1, 1, 0, 0]
    }
}
```

---

## 🐍 PYTHON — Full Code

```python
from typing import List

def dailyTemperatures(temperatures: List[int]) -> List[int]:
    n = len(temperatures)
    answer = [0] * n
    stack = []  # stores indices

    for i, temp in enumerate(temperatures):
        while stack and temp > temperatures[stack[-1]]:
            prev_day = stack.pop()
            answer[prev_day] = i - prev_day
        stack.append(i)

    return answer


# ── INPUT ──
temps = [73, 74, 75, 71, 69, 72, 76, 73]

# ── OUTPUT ──
print("Days until warmer:", dailyTemperatures(temps))
# Output: Days until warmer: [1, 1, 4, 2, 1, 1, 0, 0]
```

---

## 📖 STORY TO REMEMBER — "The Beach-Waiting Diary"

> **Sam keeps a diary of days he wants to go to the beach but can't because it's too cold.**
>
> Every cold day, he writes that day's index on a sticky note and puts it in a pile (stack).
>
> When a **warm day finally arrives**, Sam goes through the pile from top to bottom. For each note, he calculates: "this person waited `today - their day` days!" He writes that in their diary slot and removes the note.
>
> At the end, any notes still in the pile? They never got their warm day → answer stays 0.
>
> **Memory hook:** `"Cold → Stack Index. Warm → Pop & Calculate Distance"`

---

## ⚠️ COMMON MISTAKES (ALL STACK PROBLEMS)

- ❌ **Not checking `isEmpty()` before `peek()` or `pop()`** → EmptyStackException
- ❌ **Pushing values instead of indices** in monotonic stack problems → can't calculate position/distance
- ❌ **Forgetting `stack.isEmpty()`** check at END in parentheses → `"((("` returns true wrongly
- ❌ **Not handling remaining stack elements** → those have no next greater → should be -1 or 0

---

## 📋 LEETCODE PRACTICE

| # | Problem | Level | Pattern |
|---|---|---|---|
| 20 | Valid Parentheses | Easy | Bracket matching |
| 155 | Min Stack | Medium | Auxiliary stack |
| 496 | Next Greater Element I | Easy | Monotonic stack |
| 739 | Daily Temperatures | Medium | Monotonic stack |
| 150 | Evaluate Reverse Polish Notation | Medium | Stack evaluation |

---

**← [[02_Trees_Binary_Trees]] | Next: [[04_Linked_List]] →**

*Tags: #stack #monotonic-stack #parentheses #java #python #dsa*
