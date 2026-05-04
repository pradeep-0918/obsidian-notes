# 📝 2-Mark Questions — Set 2 | Software Engineering & OOP

> **Tags:** #2marks #exam #planning #UML #testing #OOAD #projectmanagement  
> **Subject:** Software Engineering / OOP / Project Management  
> **Format:** Definition + 2 Key Points (Exam Ready)

---

## Q1. List Out the Essential Steps Involved in Stepwise Planning

**Definition:**  
**Stepwise Planning** is an iterative project planning technique where the plan is progressively elaborated — near-term tasks are planned in detail, while future tasks are planned at a higher level.

**Essential Steps:**

1. **Identify project scope → break into stages → plan each stage in detail** (near stages) and at outline level (far stages)
2. **Review and refine the plan at each stage** — as the project progresses, future stages are re-planned with more detail based on current knowledge

---

## Q2. What Are the Problems with Software Projects from a Student's Point of View?

**Definition:**  
Student software projects face unique challenges due to **inexperience, resource limitations, and academic constraints**.

**Problems:**

1. **Poor planning and underestimation** — students underestimate effort, leading to last-minute rushed delivery and incomplete features
2. **Changing requirements and lack of domain knowledge** — unclear problem understanding leads to frequent redesigns, scope creep, and difficulty choosing the right tools/technologies

---

## Q3. What is an Attribute? Mention Its Types.

**Definition:**  
An **attribute** is a named property of a class that describes the **data or state** held by objects of that class in UML/OOP.

**Types of Attributes:**

|Type|Description|Example|
|---|---|---|
|**Simple**|Single indivisible value|`age: int`|
|**Composite**|Made up of multiple parts|`address` (street + city + pin)|
|**Derived**|Computed from other attributes|`/age` (from date of birth)|
|**Class-level (Static)**|Shared across all objects|`totalStudents: int`|
|**Multi-valued**|Holds multiple values|`phoneNumbers: [*]`|

---

## Q4. Differentiate Aggregation and Composition

**Definition:**  
Both are **"has-a" relationships** in UML but differ in the degree of ownership and lifecycle dependency.

|Feature|Aggregation|Composition|
|---|---|---|
|**Relationship**|Weak "has-a"|Strong "has-a"|
|**Ownership**|Child can exist independently|Child cannot exist without parent|
|**Lifecycle**|Child survives parent deletion|Child destroyed with parent|
|**Symbol**|Hollow diamond ◇|Filled diamond ◆|
|**Example**|Department has Professors|House has Rooms|

---

## Q5. Which OO Methodology is Suited for Design / Analysis / Full Life Cycle / Real-Time Systems?

**Definition:**  
Different OO methodologies are optimized for different phases or domains of software development.

|Phase / Domain|Best Suited Methodology|
|---|---|
|**(i) Design**|**Booch Method** — rich notation for module and class design|
|**(ii) Analysis**|**Rumbaugh's OMT (Object Modelling Technique)** — strong in domain modelling|
|**(iii) Full Life Cycle**|**Unified Process (UP) / RUP** — covers all phases from inception to deployment|
|**(iv) Real-Time Systems**|**Shlaer-Mellor / ROOM (Real-time OO Modelling)** — handles concurrency and timing|

---

## Q6. Identify When a Pattern is Said to Be a Good Pattern

**Definition:**  
A **Design Pattern** is a reusable solution to a recurring design problem. A pattern is considered **good** when it satisfies specific quality criteria.

**A pattern is a good pattern when:**

1. It is **general and reusable** — solves a class of problems, not just one specific case, and has been proven effective in multiple real-world projects
2. It has a **clear intent, well-defined trade-offs, and known consequences** — the pattern explicitly states when to use it, what it costs, and what alternatives exist

---

## Q7. Differentiate White Box Testing and Regression Testing

**Definition:**  
Both are software testing techniques but differ in **purpose, scope, and when they are applied**.

|Feature|White Box Testing|Regression Testing|
|---|---|---|
|**Definition**|Tests internal code structure/logic|Re-tests after code changes|
|**Knowledge Required**|Full code knowledge needed|No code knowledge required|
|**Focus**|Code paths, branches, statements|Previously working features|
|**When Used**|During development/unit testing|After bug fix or new feature added|
|**Goal**|Maximize code coverage|Ensure no new bugs introduced|
|**Example**|Testing all branches of payment validation|Re-testing login after adding OTP feature|

---

## Q8. Illustrate the Different Kinds of Errors When You Run Your Program

**Definition:**  
A **program error (bug)** is a fault that causes the software to behave incorrectly. Errors are classified by when and how they occur.

**Types of Errors:**

|Error Type|Description|Example|
|---|---|---|
|**Syntax Error**|Violates language grammar rules (caught at compile time)|Missing semicolon `;`|
|**Runtime Error**|Occurs during execution, crashes program|Division by zero, null pointer|
|**Logical Error**|Program runs but gives wrong output|Using `+` instead of `*` in formula|
|**Semantic Error**|Code is syntactically correct but meaningless|Assigning wrong variable type|
|**Linker Error**|Unresolved references between compiled modules|Missing library function definition|

---

## Q9. What Are the Different Stages in Classic Project Life Cycle?

**Definition:**  
The **Classic (Traditional) Project Life Cycle** is a sequential model where each phase must be completed before the next begins — also known as the **Waterfall Model**.

**Stages:**

```
[Feasibility] → [Requirements] → [Design] → [Implementation] → [Testing] → [Deployment] → [Maintenance]
```

1. **Feasibility Study** — Is the project worth doing?
2. **Requirements Analysis** — What should the system do?
3. **System Design** — How should it be built?
4. **Implementation (Coding)** — Build the system
5. **Testing & Integration** — Verify correctness
6. **Deployment** — Deliver to users
7. **Maintenance** — Fix bugs, add enhancements

---

## Q10. Highlight the Levels of Decision Making and Information Systems

**Definition:**  
Organizations have **three levels of management**, each requiring different types of information systems to support decision making.

|Level|Decision Type|Information System|Example|
|---|---|---|---|
|**Strategic (Top)**|Unstructured, long-term|**Executive Information System (EIS)**|CEO deciding to enter new market|
|**Tactical (Middle)**|Semi-structured, medium-term|**Management Information System (MIS)**|Manager analysing monthly sales trends|
|**Operational (Bottom)**|Structured, day-to-day|**Transaction Processing System (TPS)**|Cashier processing a sale at POS|

```
        ┌────────────────────────┐
        │  STRATEGIC LEVEL (EIS) │  ← Top Management
        ├────────────────────────┤
        │  TACTICAL LEVEL (MIS)  │  ← Middle Management
        ├────────────────────────┤
        │ OPERATIONAL LEVEL (TPS)│  ← Supervisors / Staff
        └────────────────────────┘
```

---

## ⚡ Quick Revision Snapshot — All 10 Questions

|Q|Topic|1-Line Answer|
|---|---|---|
|1|Stepwise Planning|Plan near stages in detail, far stages at outline; refine at each stage|
|2|Student Project Problems|Poor planning + lack of domain knowledge → rushed, incomplete work|
|3|Attributes & Types|Named property of class; types: simple, composite, derived, static, multi-valued|
|4|Aggregation vs Composition|Aggregation = weak has-a (child survives); Composition = strong has-a (child dies with parent)|
|5|OO Methodology Fit|Design=Booch; Analysis=OMT; Full lifecycle=RUP; Real-time=Shlaer-Mellor|
|6|Good Pattern|General, reusable, proven, with clear intent and known trade-offs|
|7|White Box vs Regression|White box = internal code paths; Regression = re-test after changes|
|8|Program Errors|Syntax, Runtime, Logical, Semantic, Linker errors|
|9|Classic Life Cycle|Feasibility → Requirements → Design → Code → Test → Deploy → Maintain|
|10|Decision Levels|Strategic(EIS) / Tactical(MIS) / Operational(TPS)|

---

> ✅ **Status:** All 10 answered | 2-mark exam format  
> 📌 **Exam Tip:** Q4 and Q7 are **differentiation questions** — always use a table format. Q5 is a **mapping question** — present as a clean table. Examiners award full marks for structured answers!