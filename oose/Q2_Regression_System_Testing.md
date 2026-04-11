# 🔁 Q2: Regression Testing & System Testing
**Subject:** CCS556 – Object Oriented Software Engineering  
**Topic:** Regression Testing & System Testing  
**Marks:** 10 | **Pages:** 3

---

> 💡 **Feynman Trick:** Imagine you repaired a car's brake system. Now you must test ALL the other parts too — steering, engine, lights — to make sure your repair didn't accidentally break something else. That's **Regression Testing**. Testing the whole car as one complete vehicle before delivery — that's **System Testing**.

---

## 📌 1. Introduction / Definition

### Regression Testing
> **Regression Testing** is the process of **re-testing** previously working software **after a change** (bug fix, enhancement, or new feature) to ensure the change has **not broken existing functionality**.

- "Regression" means "going back" — checking if old features still work.
- Performed **after every modification** to the codebase.

### System Testing
> **System Testing** is a level of testing where the **entire integrated software system** is tested against **specified requirements** to validate that it works as a whole.

- It is a **Black Box** testing technique.
- Done **after Integration Testing** and **before User Acceptance Testing (UAT)**.

---

## 📌 2. Regression Testing – In Detail

### 🧠 Feynman Explanation:
> You wrote a calculator app. It works perfectly. Now you add a "square root" feature. Regression testing checks: does addition still work? Does subtraction still work? Did my new feature break the old ones?

### Types of Regression Testing:

| Type | Description | When Used |
|------|-------------|-----------|
| **Unit Regression** | Re-test only the changed unit/module | Small bug fixes |
| **Partial Regression** | Test changed + related modules | Feature updates |
| **Complete Regression** | Re-test entire application | Major releases |
| **Selective Regression** | Select subset of test cases | Time-constrained testing |
| **Progressive Regression** | New test cases added for new features | Agile development |

### Regression Testing Process:
```
Code Change Occurs
       ↓
Identify Affected Modules
       ↓
Select Regression Test Cases
       ↓
Re-Execute Test Cases
       ↓
Compare with Previous Results
       ↓
Report Defects (if any)
       ↓
Fix → Re-Test (Cycle continues)
```

### Tools Used for Regression Testing:
- **Selenium** – Web automation
- **JUnit / TestNG** – Java testing frameworks
- **QTP / UFT** – HP's automation tool
- **Jenkins** – CI/CD pipeline automation

---

## 📌 3. System Testing – In Detail

### 🧠 Feynman Explanation:
> After building a rocket, you test each part separately (unit test). Then you assemble and test if the parts work together (integration). Finally, you launch it in a controlled environment to see if the entire rocket behaves as expected — that's System Testing.

### Characteristics:
- Tests the **complete and integrated** software
- Conducted by **independent testing team** (not developers)
- Based on **Software Requirements Specification (SRS)**
- A **Black Box** approach

---

## 📌 4. Types of System Testing (1-4-7 Rule)

> 🧠 **Remember 7 Key Types:**

### ▶ Type 1: Functional Testing
> Tests if the system does **what it is supposed to do**.
- Example: Does the login button actually log in the user?

### ▶ Type 2: Performance Testing
> Tests how the system behaves under **load and stress**.

| Sub-type | What it tests |
|----------|---------------|
| **Load Testing** | Normal expected load |
| **Stress Testing** | Beyond maximum capacity |
| **Spike Testing** | Sudden increase in users |

### ▶ Type 3: Security Testing
> Tests if the system is **protected from threats**.
- SQL injection, XSS attacks, unauthorized access.

### ▶ Type 4: Usability Testing
> Tests how **easy and intuitive** the system is for users.
- Is the UI clear? Can a new user navigate without help?

### ▶ Type 5: Compatibility Testing
> Tests if the system works across **different environments**.
- Browsers: Chrome, Firefox, Safari
- OS: Windows, Linux, MacOS

### ▶ Type 6: Recovery Testing
> Tests how well the system **recovers from crashes/failures**.
- Example: Does the banking app recover after a server crash without data loss?

### ▶ Type 7: Regression Testing (within System)
> After system-level changes, re-verify the whole system still works.

---

## 📌 5. Difference: Regression vs System Testing

| Feature | Regression Testing | System Testing |
|---------|-------------------|----------------|
| **Purpose** | Ensure old features not broken | Validate entire system vs requirements |
| **When** | After every code change | After integration testing |
| **Who** | Developer or QA team | Independent QA team |
| **Type** | Black/White Box | Black Box |
| **Scope** | Changed + related modules | Entire system |
| **Trigger** | Bug fix / new feature | End of integration phase |

---

## 📌 6. Diagram – Testing Levels

```
         +-------------------------------+
         |   Acceptance Testing (UAT)    |
         +-------------------------------+
         |       System Testing          |  ← Focus Here
         +-------------------------------+
         |     Integration Testing       |
         +-------------------------------+
         |        Unit Testing           |
         +-------------------------------+
               (Base Level - Dev)
```

---

## 📌 7. Application Example – E-Commerce Website

| Test Type | What is Tested |
|-----------|---------------|
| Regression | After adding coupon feature, test if cart, login, payment still work |
| Functional | Does checkout process complete correctly? |
| Performance | Can the site handle 10,000 users during sale? |
| Security | Is payment data encrypted? Any SQL injection possible? |
| Usability | Can elderly users navigate the site easily? |
| Compatibility | Does it work on iPhone + Android + Chrome + Firefox? |

---

## 📌 8. Advantages and Disadvantages

### ✅ Advantages of Regression Testing
1. Ensures **stability** after every code change.
2. Catches **unintended side effects** early.
3. Can be **automated** for efficiency.
4. Increases **confidence** in the software.

### ❌ Disadvantages of Regression Testing
1. **Time-consuming** if entire suite is re-run.
2. **Expensive** – needs test maintenance.
3. Difficult to identify which tests to re-run.

### ✅ Advantages of System Testing
1. Validates complete **end-to-end behavior**.
2. Performed from **user's perspective**.
3. Ensures compliance with **requirements**.
4. Catches **integration-level** bugs.

### ❌ Disadvantages of System Testing
1. **Expensive and time-consuming**.
2. Requires complete system to be ready.
3. Hard to **pinpoint** the exact failing component.

---

## 📌 9. Conclusion

> Both **Regression Testing** and **System Testing** are critical quality gates in the Software Development Life Cycle (SDLC).

- **Regression Testing** protects existing functionality from being broken by new changes.
- **System Testing** ensures the entire system meets business and technical requirements.
- Together, they form a **safety net** that delivers reliable, production-ready software.

> 🎯 **Exam Tip:** Mention the types of system testing (at least 5), explain regression testing lifecycle, draw the V-model or testing pyramid, and give a real-world example.

---
*Tags: #OOSE #CCS556 #RegressionTesting #SystemTesting #SoftwareTesting #QA*
