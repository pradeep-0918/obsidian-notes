# 🧠 Dynamic Programming (DP)

**← [[04_Linked_List]] | Next: [[06_Final_Revision_Sheet]] →**

---

## 1. 💡 INTUITION FIRST

> DP = **"Solve a big problem by breaking it into smaller overlapping subproblems and CACHING results."**
>
> **The mindset:** Can I solve THIS problem if I know the answer to a slightly SMALLER version?
> If YES → DP candidate.
>
> **The progression:** Recursion → Memoization (top-down) → Tabulation (bottom-up)

---

## 2. 🔑 PATTERNS

| Pattern | Signal Words | Examples |
|---|---|---|
| Fibonacci / Linear DP | "ways", "steps", "climb" | Climbing Stairs, House Robber |
| 0/1 Knapsack | "include/exclude", "pick/skip" | Partition Sum, Target Sum |
| Subsequence DP | "two strings", "common" | LCS, Edit Distance |
| Grid DP | "grid paths", "unique paths" | Unique Paths |

---

## 3. 🧩 PROBLEMS IN THIS NOTE

| # | Problem | Pattern | LeetCode |
|---|---|---|---|
| D1 | Climbing Stairs | Fibonacci DP | #70 |
| D2 | House Robber | Linear DP | #198 |
| D3 | Longest Common Subsequence | 2D DP | #1143 |

---
---

# 🪜 D1 — Climbing Stairs (LeetCode #70)

## ❓ Problem Statement

You are climbing a staircase with `n` steps. Each time you can climb **1 or 2 steps**.
Return the number of distinct ways to reach the top.

**Input:** `n = 5`
**Output:** `8`

---

## ✅ ANSWER

For n=5, there are **8** distinct ways:
```
1+1+1+1+1, 1+1+1+2, 1+1+2+1, 1+2+1+1, 2+1+1+1,
1+2+2, 2+1+2, 2+2+1
```

---

## 🧠 FEYNMAN TECHNIQUE

> You're on the ground (step 0) and need to reach step 5.
>
> **Think backwards:** To reach step 5, you came from either step 4 (took 1 step) or step 3 (took 2 steps). So ways(5) = ways(4) + ways(3).
>
> This is exactly Fibonacci! Each stair's answer is the sum of the previous two stairs' answers.
>
> **The evolution:**
> 1. **Recursion** → Clean logic but slow (recalculates same things)
> 2. **Memoization** → Save results in an array, don't recalculate
> 3. **Tabulation** → Build answers bottom-up (no recursion)
> 4. **Optimized** → Just keep 2 variables (like Fibonacci)

---

## 🔄 STEP-BY-STEP DRY RUN

Tabulation for n=5:

| Step | 0 | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|---|
| dp[] | 1 | 1 | 2 | 3 | 5 | **8** |

- dp[2] = dp[1] + dp[0] = 1+1 = 2
- dp[3] = dp[2] + dp[1] = 2+1 = 3
- dp[4] = dp[3] + dp[2] = 3+2 = 5
- dp[5] = dp[4] + dp[3] = 5+3 = **8** ✅

---

## ☕ JAVA — Full Code (All 4 Stages)

```java
import java.util.*;

public class ClimbingStairs {

    // ── STAGE 1: Pure Recursion (Too slow, for understanding only) ──
    static int climbRecursive(int n) {
        if (n == 0) return 1; // reached top
        if (n < 0) return 0;  // overshot
        return climbRecursive(n - 1) + climbRecursive(n - 2);
    }
    // Time: O(2^n) ❌ Very slow

    // ── STAGE 2: Memoization (Top-Down DP) ──
    static int[] memo;
    static int climbMemo(int n) {
        if (n == 0) return 1;
        if (n < 0) return 0;
        if (memo[n] != -1) return memo[n]; // use cached result
        memo[n] = climbMemo(n - 1) + climbMemo(n - 2);
        return memo[n];
    }
    // Time: O(n), Space: O(n) ✅

    // ── STAGE 3: Tabulation (Bottom-Up DP) ──
    static int climbTab(int n) {
        if (n == 1) return 1;
        int[] dp = new int[n + 1];
        dp[0] = 1; // 1 way to be at ground
        dp[1] = 1; // 1 way to reach step 1

        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
    // Time: O(n), Space: O(n) ✅

    // ── STAGE 4: Space Optimized ──
    static int climbOptimized(int n) {
        if (n <= 1) return 1;
        int prev2 = 1, prev1 = 1;
        for (int i = 2; i <= n; i++) {
            int curr = prev1 + prev2;
            prev2 = prev1;
            prev1 = curr;
        }
        return prev1;
    }
    // Time: O(n), Space: O(1) ✅✅

    public static void main(String[] args) {
        // ── INPUT ──
        int n = 5;
        memo = new int[n + 1];
        Arrays.fill(memo, -1);

        // ── OUTPUT ──
        System.out.println("n = " + n);
        System.out.println("Recursive:  " + climbRecursive(n));   // 8
        System.out.println("Memoized:   " + climbMemo(n));        // 8
        System.out.println("Tabulation: " + climbTab(n));         // 8
        System.out.println("Optimized:  " + climbOptimized(n));   // 8
    }
}
```

---

## 🐍 PYTHON — Full Code (All 4 Stages)

```python
import sys
from functools import lru_cache

# ── STAGE 1: Pure Recursion ──
def climb_recursive(n):
    if n == 0: return 1
    if n < 0: return 0
    return climb_recursive(n-1) + climb_recursive(n-2)

# ── STAGE 2: Memoization using decorator ──
@lru_cache(maxsize=None)
def climb_memo(n):
    if n == 0: return 1
    if n < 0: return 0
    return climb_memo(n-1) + climb_memo(n-2)

# ── STAGE 3: Tabulation ──
def climb_tab(n):
    if n <= 1: return 1
    dp = [0] * (n + 1)
    dp[0], dp[1] = 1, 1
    for i in range(2, n + 1):
        dp[i] = dp[i-1] + dp[i-2]
    return dp[n]

# ── STAGE 4: Space Optimized ──
def climb_optimized(n):
    if n <= 1: return 1
    prev2, prev1 = 1, 1
    for i in range(2, n + 1):
        curr = prev1 + prev2
        prev2, prev1 = prev1, curr
    return prev1


# ── INPUT & OUTPUT ──
n = 5
print(f"n = {n}")
print("Recursive: ", climb_recursive(n))   # 8
print("Memoized:  ", climb_memo(n))        # 8
print("Tabulation:", climb_tab(n))         # 8
print("Optimized: ", climb_optimized(n))   # 8
```

---

## 📖 STORY TO REMEMBER — "The Staircase Secret"

> **Every morning, Sam climbs the staircase at work, taking 1 or 2 steps at a time.**
>
> One day Sam asks: "How many ways can I climb 5 steps?"
>
> His clever friend says: *"Sam, to reach step 5 you came from step 4 or step 3. So ask how many ways to reach those!"*
>
> Sam realizes he's solving the same small problems over and over. He writes the answers on sticky notes (memo[]) on each step.
>
> Next morning, instead of re-calculating, he just reads the sticky notes (tabulation)!
>
> Eventually, he only needs the **last two sticky notes** to compute the next one (optimized).
>
> **Memory hook:** `"To reach N, you came from N-1 or N-2" → dp[n] = dp[n-1] + dp[n-2]`

---
---

# 🏠 D2 — House Robber (LeetCode #198)

## ❓ Problem Statement

You are a robber planning to rob houses along a street. Adjacent houses have security alarms. If you rob two adjacent houses → caught!

Given an integer array `nums` where `nums[i]` = amount of money in house `i`, return the **maximum amount** you can rob without alerting police.

**Input:** `nums = [2, 7, 9, 3, 1]`
**Output:** `12`

---

## ✅ ANSWER

Rob houses 1, 3, 5 (indices 0, 2, 4): 2 + 9 + 1 = **12**.

---

## 🧠 FEYNMAN TECHNIQUE

> You walk down a street of houses. At each house, you make a decision:
>
> **Option A: Rob this house** → You get its money, but you must have skipped the previous house. So your total = money here + best you got up to 2 houses ago.
>
> **Option B: Skip this house** → Your total = whatever the best was at the previous house.
>
> You take the maximum of these two choices.
>
> `dp[i] = max(dp[i-1], nums[i] + dp[i-2])`

---

## 🔄 STEP-BY-STEP DRY RUN

`nums = [2, 7, 9, 3, 1]`

| i | nums[i] | skip (dp[i-1]) | rob (nums[i]+dp[i-2]) | dp[i] |
|---|---|---|---|---|
| 0 | 2 | 0 | 2 | **2** |
| 1 | 7 | 2 | 7 | **7** |
| 2 | 9 | 7 | 9+2=11 | **11** |
| 3 | 3 | 11 | 3+7=10 | **11** |
| 4 | 1 | 11 | 1+11=12 | **12** |

Answer: **12** ✅

---

## ☕ JAVA — Full Code

```java
public class HouseRobber {

    // ── Stage 1: Recursion with Memoization ──
    static int[] memo;
    static int robMemo(int[] nums, int i) {
        if (i < 0) return 0;
        if (memo[i] != -1) return memo[i];
        memo[i] = Math.max(robMemo(nums, i - 1),           // skip
                           nums[i] + robMemo(nums, i - 2)); // rob
        return memo[i];
    }

    // ── Stage 2: Tabulation (Clean Solution) ──
    static int rob(int[] nums) {
        int n = nums.length;
        if (n == 0) return 0;
        if (n == 1) return nums[0];

        int[] dp = new int[n];
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0], nums[1]);

        for (int i = 2; i < n; i++) {
            dp[i] = Math.max(dp[i - 1],              // skip house i
                             nums[i] + dp[i - 2]);    // rob house i
        }
        return dp[n - 1];
    }

    // ── Stage 3: Space Optimized ──
    static int robOptimized(int[] nums) {
        int prev2 = 0, prev1 = 0;
        for (int num : nums) {
            int curr = Math.max(prev1, num + prev2);
            prev2 = prev1;
            prev1 = curr;
        }
        return prev1;
    }

    public static void main(String[] args) {
        // ── INPUT ──
        int[] nums = {2, 7, 9, 3, 1};
        memo = new int[nums.length];
        java.util.Arrays.fill(memo, -1);

        // ── OUTPUT ──
        System.out.println("Input: [2, 7, 9, 3, 1]");
        System.out.println("Memoized:   " + robMemo(nums, nums.length - 1)); // 12
        System.out.println("Tabulation: " + rob(nums));                        // 12
        System.out.println("Optimized:  " + robOptimized(nums));               // 12
    }
}
```

---

## 🐍 PYTHON — Full Code

```python
from typing import List
from functools import lru_cache

def rob(nums: List[int]) -> int:
    n = len(nums)
    if n == 0: return 0
    if n == 1: return nums[0]

    # ── Tabulation ──
    dp = [0] * n
    dp[0] = nums[0]
    dp[1] = max(nums[0], nums[1])

    for i in range(2, n):
        dp[i] = max(dp[i-1], nums[i] + dp[i-2])

    return dp[-1]

def rob_optimized(nums: List[int]) -> int:
    prev2 = prev1 = 0
    for num in nums:
        curr = max(prev1, num + prev2)
        prev2, prev1 = prev1, curr
    return prev1


# ── INPUT & OUTPUT ──
nums = [2, 7, 9, 3, 1]
print("Input:", nums)
print("Tabulation:", rob(nums))          # 12
print("Optimized: ", rob_optimized(nums)) # 12
```

---

## 📖 STORY TO REMEMBER — "The Greedy Burglar's Dilemma"

> **Robbie the Robber walks down a street of houses, eyeing each one.**
>
> At every house Robbie thinks: "Should I rob THIS house? Or skip it?"
>
> If he robs it, he can't rob the one next door (alarm!). So he gets **this house's money + the best haul from 2 houses back**.
>
> If he skips it, he keeps the **best haul from the previous house**.
>
> He makes this choice at EVERY house, always picking the max.
>
> **Robbie only needs to remember his last two decisions** (prev1, prev2) — he doesn't need to rethink the whole street.
>
> **Memory hook:** `"Rob = this + 2 back. Skip = 1 back. Pick max. Remember only last 2."`

---
---

# 📝 D3 — Longest Common Subsequence (LeetCode #1143)

## ❓ Problem Statement

Given two strings `text1` and `text2`, return the length of their **longest common subsequence**.

A subsequence is derived by deleting some characters without changing order.

**Input:** `text1 = "abcde"`, `text2 = "ace"`
**Output:** `3` (LCS = "ace")

---

## ✅ ANSWER

**3** — the subsequence "ace" appears in both strings.

---

## 🧠 FEYNMAN TECHNIQUE

> You have two strings. You want the longest sequence of characters that appears in BOTH, **in order** (but not necessarily contiguous).
>
> At each pair of characters (one from each string), you make a choice:
>
> - **If they MATCH** → great! This character is part of the LCS. Take `1 + LCS of what remains` (move both pointers back by 1).
>
> - **If they DON'T MATCH** → pick the better of two options:
>   - Skip the current char from string 1 (move pointer in text1 back)
>   - Skip the current char from string 2 (move pointer in text2 back)
>
> **Build a 2D table** where `dp[i][j]` = LCS length of first `i` chars of text1 and first `j` chars of text2.

---

## 🔄 STEP-BY-STEP DRY RUN

`text1 = "abcde"`, `text2 = "ace"`

```
     ""  a  c  e
""  [ 0  0  0  0 ]
a   [ 0  1  1  1 ]
b   [ 0  1  1  1 ]
c   [ 0  1  2  2 ]
d   [ 0  1  2  2 ]
e   [ 0  1  2  3 ]
```

- `dp[1][1]`: a==a → `1 + dp[0][0]` = 1
- `dp[3][2]`: c==c → `1 + dp[2][1]` = 2
- `dp[5][3]`: e==e → `1 + dp[4][2]` = 3

Answer: **3** ✅

---

## ☕ JAVA — Full Code (Recursion → Memo → Tab)

```java
public class LongestCommonSubsequence {

    // ── Stage 1: Recursion (for understanding) ──
    static int lcsRecursive(String s1, String s2, int i, int j) {
        if (i == 0 || j == 0) return 0;
        if (s1.charAt(i-1) == s2.charAt(j-1))
            return 1 + lcsRecursive(s1, s2, i-1, j-1); // match!
        return Math.max(lcsRecursive(s1, s2, i-1, j),  // skip s1 char
                        lcsRecursive(s1, s2, i, j-1)); // skip s2 char
    }

    // ── Stage 2: Memoization ──
    static int[][] memo;
    static int lcsMemo(String s1, String s2, int i, int j) {
        if (i == 0 || j == 0) return 0;
        if (memo[i][j] != -1) return memo[i][j];

        if (s1.charAt(i-1) == s2.charAt(j-1))
            memo[i][j] = 1 + lcsMemo(s1, s2, i-1, j-1);
        else
            memo[i][j] = Math.max(lcsMemo(s1, s2, i-1, j),
                                  lcsMemo(s1, s2, i, j-1));
        return memo[i][j];
    }

    // ── Stage 3: Tabulation (Clean, Interview-Ready) ──
    static int lcs(String text1, String text2) {
        int m = text1.length(), n = text2.length();
        int[][] dp = new int[m + 1][n + 1];
        // dp[0][...] = 0, dp[...][0] = 0 (empty string base cases)

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (text1.charAt(i-1) == text2.charAt(j-1)) {
                    dp[i][j] = 1 + dp[i-1][j-1];      // characters match
                } else {
                    dp[i][j] = Math.max(dp[i-1][j],    // skip text1 char
                                        dp[i][j-1]);   // skip text2 char
                }
            }
        }
        return dp[m][n];
    }

    public static void main(String[] args) {
        // ── INPUT ──
        String text1 = "abcde", text2 = "ace";
        int m = text1.length(), n = text2.length();

        memo = new int[m + 1][n + 1];
        for (int[] row : memo) java.util.Arrays.fill(row, -1);

        // ── OUTPUT ──
        System.out.println("text1 = \"" + text1 + "\", text2 = \"" + text2 + "\"");
        System.out.println("Recursive:  " + lcsRecursive(text1, text2, m, n)); // 3
        System.out.println("Memoized:   " + lcsMemo(text1, text2, m, n));      // 3
        System.out.println("Tabulation: " + lcs(text1, text2));                // 3
    }
}
```

---

## 🐍 PYTHON — Full Code

```python
def lcs(text1: str, text2: str) -> int:
    m, n = len(text1), len(text2)
    # dp[i][j] = LCS of text1[:i] and text2[:j]
    dp = [[0] * (n + 1) for _ in range(m + 1)]

    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i-1] == text2[j-1]:
                dp[i][j] = 1 + dp[i-1][j-1]       # match → extend
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])  # skip one

    return dp[m][n]


# ── INPUT & OUTPUT ──
text1, text2 = "abcde", "ace"
print(f'text1="{text1}", text2="{text2}"')
print("LCS Length:", lcs(text1, text2))
# Output: LCS Length: 3
```

---

## 📖 STORY TO REMEMBER — "The Common Letter Friend"

> **Alice and Bob both love the word game. They each have a secret word.**
>
> They compare their words character by character using a **scoreboard (2D grid)**.
>
> At each cell, Alice and Bob check: "Do we have the same letter right now?"
>
> - **YES** → "Common letter! Score = 1 + whatever we scored with our remaining letters" (diagonal + 1)
> - **NO** → "Skip one of our letters and keep the best score we had before" (max of left or above)
>
> They fill the whole grid this way. The bottom-right corner has the final score.
>
> **Memory hook:**
> - Match → `diagonal + 1`
> - No match → `max(left, above)`
> - Answer is always `dp[m][n]`

---

## ⚠️ COMMON MISTAKES (ALL DP PROBLEMS)

- ❌ **Wrong memo initialization** → use -1 (not 0) since 0 can be a valid answer
- ❌ **Off-by-one when indexing into string** → in 1-indexed DP, use `s.charAt(i-1)` not `s.charAt(i)`
- ❌ **Forgetting base cases** → dp[0] and dp[1] need special handling in linear DP
- ❌ **Not defining what dp[i] means** → always write this comment first in interviews!
- ❌ **Missing the weight check** in knapsack → `if (weight[i-1] <= w)` before taking the item

---

## 🔁 HOW TO CONVERT RECURSION → DP (Step-by-Step)

```
Step 1: Write the recursive solution with clear base cases
Step 2: Identify parameters that CHANGE in each recursive call
        (those become your dp array indices)
Step 3: Create memo array of the same dimensions as those parameters
Step 4: At start of function, check if memo[params] != -1 → return it
Step 5: At end of function, store result in memo[params] before returning
Step 6: (Optional) Convert to tabulation: loop in reverse order of recursion
```

---

## 📋 LEETCODE PRACTICE

| # | Problem | Level | Pattern |
|---|---|---|---|
| 70 | Climbing Stairs | Easy | Fibonacci DP |
| 509 | Fibonacci Number | Easy | Base DP |
| 198 | House Robber | Medium | 1D Linear DP |
| 1143 | Longest Common Subsequence | Medium | 2D DP |
| 416 | Partition Equal Subset Sum | Medium | 0/1 Knapsack |

---

**← [[04_Linked_List]] | Next: [[06_Final_Revision_Sheet]] →**

*Tags: #dp #dynamic-programming #memoization #tabulation #fibonacci #knapsack #lcs #java #python #dsa*
