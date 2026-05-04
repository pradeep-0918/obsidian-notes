# 🧪 White Box, Black Box Testing & Debugging

**Subject:** Software Engineering | **Marks:** 15 | **Type:** Theory + Diagram

---

> 🧠 **Core Idea (1 Line):** White Box testing validates internal code logic; Black Box testing validates external behavior; Debugging is the process of locating and fixing the fault that causes a failure.

---

## 🔷 PART 1: White Box Testing

### 📌 Definition

> **White Box Testing** (also called Glass Box / Structural / Clear Box Testing) is a testing technique in which the tester has **full knowledge of the internal code structure**, logic, and implementation. Test cases are designed based on the **source code** to ensure every statement, branch, path, and condition is exercised.

### Key Points:

- Also known as: **Structural Testing, Clear Box Testing, Open Box Testing**
- Performed by: **Developers** (unit testing phase)
- Focus: Internal logic, code paths, loops, conditions
- Requires: Access to source code

---

### 🔹 White Box Testing Techniques

#### 1. Statement Coverage

- **Definition:** Every executable statement in the code is executed **at least once**.
- **Goal:** Ensure no dead/unreachable code exists.
- **Formula:** Coverage % = (Statements executed / Total statements) × 100
- **Weakness:** Does NOT guarantee all branches are tested.
- **Example:**

```
if (x > 0):       ← only testing x=5 covers this statement
    print("pos")  ← but NEVER exercises the false branch
```

#### 2. Branch Coverage (Decision Coverage)

- **Definition:** Every branch (True AND False) of every decision point is executed at least once.
- **Stronger than** statement coverage — catches more bugs.
- **Formula:** Coverage % = (Branches executed / Total branches) × 100
- **Example:** For `if (x > 0)` — need two tests: x=5 (True) and x=-1 (False)

#### 3. Path Coverage

- **Definition:** Every unique **path** through the program from entry to exit is executed.
- **Strongest form** of structural coverage — but impractical for large programs.
- **Problem:** Number of paths **grows exponentially** with loops and conditions.
- **Example:** 10 decisions → up to 2¹⁰ = 1024 paths

#### 4. Condition Coverage (Multiple Condition Coverage)

- **Definition:** Each Boolean sub-expression within a compound condition takes both True and False values.
- **Example:** `if (x > 0 AND y > 0)` — need to test all 4 combinations: TT, TF, FT, FF
- **MC/DC (Modified Condition/Decision Coverage):** Used in **safety-critical systems** (aviation, medical devices — DO-178C standard)

---

### 🔹 White Box Testing — Control Flow Graph (ASCII)

```
         [Start]
            |
       [x = input()]      ← Statement A
            |
       [x > 0 ?]          ← Decision node
        /       \
      T/         \F
[print(pos)]  [print(non-pos)]   ← Two branches
      \         /
       [  End  ]

Statement Coverage : 1 test (x=5) → covers all statements but NOT false branch
Branch Coverage    : 2 tests (x=5, x=-1) → covers both T and F branches
Path Coverage      : All paths from Start to End covered
```

---

### 🔹 Advantages of White Box Testing

- ✅ Finds hidden bugs, dead code, and unreachable statements
- ✅ Optimizes code by identifying redundant paths
- ✅ Enables thorough test case design based on actual logic
- ✅ Detects security vulnerabilities in implementation

### ❌ Disadvantages of White Box Testing

- ❌ Requires programming expertise — testers must read code
- ❌ Cannot detect missing functionality (only tests what's coded)
- ❌ Time-consuming for large codebases
- ❌ Path explosion problem makes 100% path coverage infeasible

---

## 🔷 PART 2: Black Box Testing

### 📌 Definition

> **Black Box Testing** (also called Functional Testing / Behavioral Testing / Specification-based Testing) is a testing technique where the tester has **NO knowledge of the internal code**. Test cases are designed purely from the **requirements specification** by examining inputs and their expected outputs.

### Key Points:

- Also known as: **Functional Testing, Opaque Box Testing, Closed Box Testing**
- Performed by: **QA Testers** (independent of development)
- Focus: Input-output behavior, functional correctness
- Does NOT require access to source code

---

### 🔹 Black Box Testing Techniques

#### 1. Equivalence Partitioning (EP)

- **Definition:** Divide the input domain into **classes (partitions)** of equivalent data. One test case per partition is enough.
- **Classes:** Valid partitions (accepted inputs) and Invalid partitions (rejected inputs)
- **Benefit:** Reduces the number of test cases dramatically
- **Example:**

```
Age field accepts 1 to 100:
  Invalid partition 1: Age < 1  → test: Age = -5
  Valid partition   : 1 ≤ Age ≤ 100 → test: Age = 50
  Invalid partition 2: Age > 100 → test: Age = 200
```

#### 2. Boundary Value Analysis (BVA)

- **Definition:** Test cases are created at the **edges (boundaries)** of each partition. Defects cluster at boundaries.
- **Test values:** min, min+1, max-1, max (and just outside: min-1, max+1)
- **Extension:** Robustness testing also tests min-1 and max+1
- **Example:**

```
Age field (1 to 100):
Boundary test values: 0, 1, 2, 99, 100, 101
                       ↑             ↑
                    invalid       invalid
```

#### 3. Decision Table Testing

- **Definition:** A table that maps all combinations of **conditions (inputs)** to their corresponding **actions (outputs)**.
- **Best for:** Business rule logic with multiple conditions.
- **Example — Loan approval:**

|Customer employed?|Good credit?|Loan approved?|
|---|---|---|
|Yes|Yes|✅ Approved|
|Yes|No|❌ Rejected|
|No|Yes|❌ Rejected|
|No|No|❌ Rejected|

#### 4. State Transition Testing

- **Definition:** Tests the system by moving through different **states** via events/inputs, verifying correct transitions.
- **Used for:** UI flows, protocols, embedded systems, ATMs, vending machines
- **Example — ATM state machine:**

```
[Idle] ──card inserted──► [Card Read] ──PIN entered──► [Authenticated]
                                                              |
                                               ──cash request──► [Dispensing]
                                                              |
                                               ──cancel──────► [Idle]
```

#### 5. Error Guessing

- **Definition:** Tester uses **experience and intuition** to guess likely error-prone areas.
- **Based on:** Common mistakes, past bug reports, domain knowledge.
- **Example:** Test empty inputs, null values, very large numbers, special characters in text fields.

---

### 🔹 White Box vs Black Box — Comparison Table

|Feature|White Box|Black Box|
|---|---|---|
|**Knowledge needed**|Full internal code|Specification only|
|**Also known as**|Structural testing|Functional testing|
|**Performed by**|Developers|QA testers|
|**Based on**|Source code|Requirements|
|**Finds**|Logic errors, dead code|Missing features, wrong behavior|
|**Coverage metric**|Statement, branch, path|Test cases per requirement|
|**Phase**|Unit, integration testing|System, acceptance testing|
|**Tools**|GDB, JaCoCo, Clover|Selenium, QTP, JUnit (functional)|

---

## 🔷 PART 3: Debugging

### 📌 Definition

> **Debugging** is the process of **identifying, locating, analyzing, and removing** the fault (defect/bug) in a program that causes an observed failure. Testing reveals that a failure exists; debugging finds out **where and why** it happens.

> Key distinction: **Testing detects failures. Debugging finds and fixes faults.**

---

### 🔹 Key Terminology

|Term|Definition|Example|
|---|---|---|
|**Error**|Human mistake during coding/design|Developer writes `>` instead of `>=`|
|**Fault / Bug / Defect**|The incorrect code that results from the error|The wrong condition in the code|
|**Failure**|The incorrect observable behavior at runtime|Program crashes or gives wrong output|

> Error → Fault → Failure (cause-effect chain)

---

### 🔹 Debugging Process — Step by Step

```
Step 1: Failure observed        ← Testing finds unexpected output
           |
Step 2: Reproduce the failure   ← Create a minimal, consistent test case
           |
Step 3: Locate the fault        ← Debugger, breakpoints, logs, print statements
           |
Step 4: Understand root cause   ← WHY does this fault produce this failure?
           |
Step 5: Fix the fault           ← Correct the code
           |
Step 6: Regression test         ← Re-run ALL tests to ensure fix didn't break anything
           |
       If PASS → Done
       If FAIL → Return to Step 3
```

---

### 🔹 Debugging Strategies

#### 1. Brute Force Debugging

- **Definition:** Insert memory dumps or `print` statements throughout the code and examine all output.
- **When used:** Last resort; programmer has no clue where the bug is.
- **Downside:** Very inefficient; generates excessive irrelevant output.
- **Example:** Adding `printf("reached here: x = %d", x)` at every line.

#### 2. Backtracking

- **Definition:** Start from the point of failure and **trace backwards** through the code to find the faulty statement.
- **When used:** Small programs with linear flow.
- **Downside:** Number of backward paths grows rapidly with program size.
- **Example:** Output is wrong at line 80 → trace back through lines 79, 78, 77... until incorrect state found.

#### 3. Cause Elimination (Binary Partitioning)

- **Definition:** Form a **hypothesis** about the cause, design a test to confirm or eliminate it. Systematically halve the search space.
- **Most efficient** strategy — like binary search applied to debugging.
- **Example:**

```
Bug somewhere in 100-line function:
→ Put breakpoint at line 50. Bug before 50? Search lines 1-50. After 50? Search 51-100.
→ Repeat. Each step halves the search space.
```

#### 4. Automated / Tool-Assisted Debugging

- **Definition:** Use specialized debugging tools that provide breakpoints, watch variables, stack traces, memory analysis.
- **Tools:**
    - **GDB** — GNU debugger for C/C++
    - **Valgrind** — memory leak and error detection
    - **IDE Debuggers** — Eclipse, IntelliJ, VS Code built-in debuggers
    - **Log analysis tools** — ELK Stack, Splunk

---

### 🔹 ASCII Diagram — Debugging Lifecycle

```
  [Failure observed by test]
            |
  [Reproduce the failure]
            |
  [Locate the fault]  ←──────────────────────────┐
            |                                     |
  [Understand root cause]                         |
            |                                     |
  [Apply fix]                                     |
            |                                     |
  [Regression test] ─── FAIL ────────────────────┘
            |
           PASS
            |
     [Bug resolved]
```

---

### 🔹 Advantages of Systematic Debugging

- ✅ Systematic approach reduces time-to-fix
- ✅ Root cause analysis prevents recurrence
- ✅ Regression testing ensures fix doesn't introduce new bugs
- ✅ Automated tools catch errors humans miss (memory leaks, race conditions)

### ❌ Challenges in Debugging

- ❌ **Heisenbug** — bug disappears when you try to observe it (e.g., timing-dependent)
- ❌ **Intermittent bugs** — appear only under specific runtime conditions
- ❌ **Cascading failures** — one fault triggers many failures; finding the root is hard
- ❌ Fixing one bug may introduce another (regression)

---

## 🔷 Comparison: White Box vs Black Box vs Debugging

|Aspect|White Box Testing|Black Box Testing|Debugging|
|---|---|---|---|
|**Goal**|Verify internal logic|Verify functional behavior|Find and fix a fault|
|**When**|During/after coding|After integration|After a test failure|
|**Who**|Developer|QA tester|Developer|
|**Input**|Source code|Requirements|Failure report|
|**Output**|Coverage report|Pass/Fail results|Fixed code|
|**Techniques**|Statement, branch, path|EP, BVA, decision table|Backtracking, cause elimination|

---

## ⚡ QUICK REVISION — 1-4-7 Rule

### 1️⃣ One Line:

> White Box = test internal code logic; Black Box = test external behavior from specs; Debugging = locate and fix the specific fault causing the failure.

---

### 4️⃣ Four Key Points:

1. **White Box techniques:** Statement → Branch → Path → Condition coverage (increasing strength)
2. **Black Box techniques:** Equivalence Partitioning, Boundary Value Analysis, Decision Table, State Transition
3. **Error → Fault → Failure** chain: error is human mistake, fault is wrong code, failure is observed wrong behavior
4. **Best debugging strategy:** Cause Elimination (binary search approach) — most systematic and efficient

---

### 7️⃣ Seven Deep Points:

1. **Statement coverage** is the weakest — executing all statements doesn't guarantee all branches are tested
2. **Branch coverage** needs at least 2 tests per decision (True AND False paths)
3. **BVA** extends EP by testing values exactly at, just inside, and just outside boundaries
4. **Decision table testing** is ideal for systems with complex business rules and multiple conditions
5. **Fault ≠ Failure** — a fault (wrong code) may exist without causing a visible failure under normal inputs
6. **Backtracking** is efficient for small programs; cause elimination scales to large codebases
7. **Regression testing** after every fix ensures no side-effects — critical in CI/CD pipelines

---

## 📝 3 Possible Exam Questions

1. **"Discuss White Box and Black Box testing techniques in detail with diagrams and examples."** _(15 Marks)_ ← This note answers this!
2. **"Explain Equivalence Partitioning and Boundary Value Analysis with suitable examples."** _(7 Marks)_
3. **"What is debugging? Explain the debugging process and various debugging strategies."** _(10 Marks)_

---

_📁 Save in Obsidian: `Software Engineering > Testing > White Box Black Box Debugging`_ _🏷️ Tags: #WhiteBoxTesting #BlackBoxTesting #Debugging #SoftwareTesting #15Marks #SoftwareEngineering_