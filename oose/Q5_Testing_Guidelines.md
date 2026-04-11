# 🧪 Q5: User Satisfaction Test, White Box & Black Box Testing
**Subject:** CCS556 – Object Oriented Software Engineering  
**Topic:** Testing Guidelines  
**Marks:** 10 | **Pages:** 3

---

> 💡 **Feynman Trick:**
> - **Black Box Testing** = You are a customer. You press buttons on an ATM and check if correct money comes out. You don't care HOW it works inside.
> - **White Box Testing** = You are a bank engineer. You open the ATM, check every wire and circuit, and test the internal code.
> - **User Satisfaction Test** = You ask 100 customers: "Were you happy using this ATM?"

---

## 📌 1. Introduction / Definition

### Black Box Testing
> **Black Box Testing** (Behavioral Testing) tests the software's **functionality without knowledge of the internal code**. The tester only knows **inputs and expected outputs**.

### White Box Testing
> **White Box Testing** (Structural/Glass Box Testing) tests the **internal logic, code paths, and structure** of the software. The tester has full knowledge of the code.

### User Satisfaction Testing
> **User Satisfaction Testing** (Acceptance Testing) measures how well the software meets the **end user's needs, expectations, and comfort level** during real usage.

---

## 📌 2. Black Box Testing – In Detail

### 🧠 Feynman Explanation:
> Think of a **vending machine**. You press D5 expecting Coke. If Coke comes out → PASS. If water comes out → FAIL. You don't know or care what's happening inside the machine.

### Techniques of Black Box Testing:

#### 2.1 Equivalence Partitioning
> Divide input data into **equivalent classes**. Test one value from each class.

**Example – Age field (valid: 18–60):**
| Partition | Test Value | Expected Result |
|-----------|-----------|-----------------|
| Below range | 10 | Invalid |
| In range | 30 | Valid |
| Above range | 70 | Invalid |

---

#### 2.2 Boundary Value Analysis (BVA)
> Test values at the **edges/boundaries** of valid ranges.

**Example – Age: 18 to 60:**
```
Test: 17 (just below) | 18 (min) | 19 (just above min)
Test: 59 (just below max) | 60 (max) | 61 (just above)
```

---

#### 2.3 Decision Table Testing
> Create a table of **combinations of inputs and corresponding outputs**.

| Condition A | Condition B | Action |
|-------------|-------------|--------|
| True | True | Output X |
| True | False | Output Y |
| False | True | Output Z |
| False | False | Error |

---

#### 2.4 State Transition Testing
> Test how system moves between **different states**.

**Example – Traffic Light:**
```
Red → Green → Yellow → Red (cycle)
Test: Does pressing emergency button go to Red immediately?
```

---

#### 2.5 Use Case Testing
> Test based on **real user scenarios** (Use Cases).

**Example – Login Use Case:**
- Valid credentials → Dashboard shown ✅
- Wrong password → Error message ✅
- Account locked → Lock message ✅

---

### ✅ Advantages of Black Box Testing
1. Tester doesn't need to know programming.
2. Tests from **user's perspective**.
3. Effective for finding **functional bugs**.
4. Suitable for **large-scale systems**.

### ❌ Disadvantages of Black Box Testing
1. Cannot test **internal logic/paths**.
2. Redundant tests possible.
3. Not all code paths covered.

---

## 📌 3. White Box Testing – In Detail

### 🧠 Feynman Explanation:
> Imagine you are a mechanic who opens a car's engine. You check each wire, test each connection, and verify every path the electrical current can take. That's White Box — you see the **internals**.

### Techniques of White Box Testing:

#### 3.1 Statement Coverage
> Every **line/statement** of code must be executed at least once.
```java
int max(int a, int b) {
    if (a > b)        // Statement 1
        return a;     // Statement 2
    return b;         // Statement 3
}
// Test 1: a=5, b=3 → covers S1, S2
// Test 2: a=3, b=5 → covers S1, S3
```

#### 3.2 Branch Coverage
> Every **branch (if/else)** must be tested for both TRUE and FALSE.

```
if (x > 0)   → Test x=5 (True branch)
               → Test x=-1 (False branch)
```

#### 3.3 Path Coverage
> Every **possible execution path** through the code is tested.

#### 3.4 Loop Testing
> Specifically tests **loop boundaries**:
- Zero iterations
- One iteration
- Typical number of iterations
- Maximum iterations

#### 3.5 Condition Coverage
> Every **Boolean sub-expression** must be tested as TRUE and FALSE.

---

### ✅ Advantages of White Box Testing
1. Thorough — **100% code coverage** possible.
2. Identifies **hidden bugs and dead code**.
3. Optimizes code — finds inefficiencies.
4. Helps in **security testing** — detects vulnerabilities.

### ❌ Disadvantages of White Box Testing
1. Requires deep **programming knowledge**.
2. Time-consuming and **expensive**.
3. May miss **user perspective** issues.
4. Not suitable for large systems — too many paths.

---

## 📌 4. User Satisfaction Testing – Guidelines

### 🧠 Feynman Explanation:
> You built a recipe app. Even if all buttons work (black box) and all code runs (white box), if users find it **confusing, ugly, or slow** — it fails. User Satisfaction Testing catches these issues.

### Guidelines for Developing User Satisfaction Test:

| Step | Guideline | Details |
|------|-----------|---------|
| 1 | **Define Objectives** | What satisfaction aspects to measure? (ease of use, speed, UI) |
| 2 | **Select Participants** | Choose representative users (beginners, experts, elderly) |
| 3 | **Create Test Scenarios** | Real tasks users would perform (login, search, buy) |
| 4 | **Design Questionnaire** | Use Likert scale (1=Very Dissatisfied to 5=Very Satisfied) |
| 5 | **Conduct Testing** | Let users perform tasks without assistance |
| 6 | **Observe & Record** | Note confusion points, errors, time taken |
| 7 | **Collect Feedback** | Post-test survey / interview |
| 8 | **Analyze Results** | Calculate satisfaction scores, identify problem areas |
| 9 | **Report Findings** | Document issues with severity ratings |
| 10 | **Iterate** | Fix issues and re-test |

---

### Sample Satisfaction Questionnaire:
```
Q1. How easy was it to register? [1–5]
Q2. Did you find the navigation intuitive? [1–5]
Q3. How satisfied are you with response speed? [1–5]
Q4. Would you recommend this app? [Yes/No]
Q5. What would you improve? [Open text]
```

---

## 📌 5. Comparison Table

| Feature | Black Box | White Box | User Satisfaction |
|---------|-----------|-----------|-------------------|
| **Who tests** | QA Tester | Developer | End Users |
| **Knowledge needed** | Functional | Code/Internal | None |
| **What is tested** | Input/Output | Code paths | User experience |
| **Tools** | Selenium, QTP | JUnit, NUnit | Surveys, SUS |
| **When** | After integration | During development | Pre-deployment |

---

## 📌 6. Diagram – Testing Coverage

```
                Software Testing
               /        |        \
         Black Box   White Box   User Satisfaction
         (External)  (Internal)   (Experience)
              |           |              |
         Functional    Coverage       Surveys
           Input       Branches       Tasks
          /Output       Paths       Feedback
```

---

## 📌 7. Conclusion

> The three testing approaches complement each other:
- **Black Box** ensures correct **functional behavior**.
- **White Box** ensures correct **internal logic and coverage**.
- **User Satisfaction Testing** ensures the software is **usable, pleasant, and meets expectations**.

> A complete testing strategy uses **all three** to deliver software that works correctly AND satisfies users.

> 🎯 **Exam Tip:** Explain all techniques of Black Box (Equivalence Partitioning, BVA, Decision Table) and White Box (Statement, Branch, Path coverage). List 10 guidelines for user satisfaction test. Draw comparison table.

---
*Tags: #OOSE #CCS556 #BlackBoxTesting #WhiteBoxTesting #UserSatisfactionTesting #SoftwareTesting*
