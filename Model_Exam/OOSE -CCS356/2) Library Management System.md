# 📚 Library Management System — OOSD & Use Case Model

**Subject:** Object-Oriented System Development | **Marks:** 15 | **Type:** Theory + Diagram

---

> 🧠 **Core Idea (1 Line):** The Library Management System (LMS) uses OOSD to model real-world library entities as objects and interactions as use cases, enabling efficient book management, member services, and administrative control.

---

## 🔷 PART 1: Problem Statement

### 📌 Problem Statement

> **"The existing manual or semi-automated library system is inefficient in managing books, members, borrowing records, fines, and reservations. There is a need for an Object-Oriented automated Library Management System that can handle book cataloguing, member registration, book issue/return, fine calculation, reservations, and report generation — ensuring accurate records, reduced manual effort, and improved user experience for both members and librarians."**

### Key Problems Identified:

- **Manual record-keeping** leads to errors in book tracking and fine calculation
- **No real-time availability** check for books
- **Difficulty managing** multiple members, loans, and due dates simultaneously
- **No automated alerts** for overdue books or reservations
- **Inconsistent fine** calculation due to manual processes

---

## 🔷 PART 2: Object-Oriented System Development (OOSD)

### 📌 What is OOSD?

**OOSD** is a methodology that models a system using **objects** (real-world entities), each having **attributes** (data) and **methods** (behaviors). It uses concepts like encapsulation, inheritance, polymorphism, and abstraction.

> Think of it like modeling the real library world in code — a Book is an object, a Member is an object, and the Librarian manages the interaction between them.

---

### 🔹 Step 1: Identify the Objects (Classes)

|Class|Attributes|Methods|
|---|---|---|
|**Book**|bookID, title, author, ISBN, genre, availableCopies|searchBook(), checkAvailability()|
|**Member**|memberID, name, email, phone, membershipType|register(), updateProfile()|
|**Librarian**|staffID, name, role|issueBook(), returnBook(), addBook()|
|**Loan**|loanID, issueDate, dueDate, returnDate|calculateFine(), renewLoan()|
|**Reservation**|reservationID, reservedDate, status|reserveBook(), cancelReservation()|
|**Fine**|fineID, amount, paidStatus|calculateFine(), payFine()|
|**Catalogue**|catalogueID, bookList|addBook(), removeBook(), searchBook()|

---

### 🔹 Step 2: Identify Relationships

```
Member ──────────< Loan >────────── Book
   |                                  |
   |──── Reservation ─────────────────|
   |
   └──── Fine (generated from Loan)

Librarian ──── manages ──── Catalogue
                                 |
                              contains Book
```

- **Association:** Member borrows Book (through Loan)
- **Aggregation:** Catalogue contains many Books
- **Inheritance:** PremiumMember extends Member
- **Dependency:** Fine depends on Loan

---

### 🔹 Step 3: Identify Actors

|Actor|Role|
|---|---|
|**Member**|Searches, borrows, returns, reserves books; pays fines|
|**Librarian**|Manages books, processes issue/return, generates reports|
|**Admin**|Manages members, configures system settings|
|**System**|Sends notifications, calculates fines automatically|

---

### 🔹 Step 4: Key OO Concepts Applied

- **Encapsulation:** Book's internal details (stock count) are hidden; accessed only through methods
- **Inheritance:** PremiumMember inherits from Member and gets extended borrowing limits
- **Polymorphism:** `calculateFine()` works differently for regular and premium members
- **Abstraction:** Member sees only search/borrow interface; internal DB logic is hidden

---

## 🔷 PART 3: Use Case Model

### 📌 What is a Use Case Model?

A **Use Case Model** describes **what a system does** from the user's perspective. It captures functional requirements using **actors** and **use cases** (interactions between actors and the system).

---

### 🔹 Actors in LMS

|Actor|Type|Description|
|---|---|---|
|**Member**|Primary|Registers, searches, borrows, returns, reserves books|
|**Librarian**|Primary|Issues books, manages catalogue, generates reports|
|**Admin**|Primary|Manages members, configures system|
|**System/Timer**|Secondary|Auto-calculates fines, sends notifications|

---

### 🔹 Use Cases List

#### Member Use Cases:

1. Register / Login
2. Search Book
3. Borrow Book _(includes: Check Availability)_
4. Return Book _(includes: Calculate Fine)_
5. Reserve Book _(extends: Borrow Book when unavailable)_
6. Pay Fine
7. View Loan History
8. Renew Loan _(extends: Borrow Book)_

#### Librarian Use Cases:

1. Login
2. Issue Book to Member _(includes: Verify Member)_
3. Process Book Return _(includes: Calculate Fine)_
4. Add / Remove Book
5. Update Book Details
6. Generate Reports _(includes: View Loan History)_
7. Manage Reservations

#### Admin Use Cases:

1. Login
2. Register Member _(generalizes: Register Student / Register Faculty)_
3. Deactivate Member
4. Configure System Settings
5. View All Reports

---

### 🔹 Use Case Relationships Explained

#### ✅ <<include>> Relationship

- A use case **always calls** another use case as a mandatory step
- **Example:** "Borrow Book" **always includes** "Check Availability"

```
Borrow Book ──<<include>>──► Check Availability
Return Book ──<<include>>──► Calculate Fine
Issue Book  ──<<include>>──► Verify Member
```

#### ✅ <<extend>> Relationship

- A use case **optionally extends** another — happens only under certain conditions
- **Example:** "Reserve Book" extends "Borrow Book" only when the book is NOT available

```
Reserve Book ──<<extend>>──► Borrow Book    (when book unavailable)
Renew Loan   ──<<extend>>──► Borrow Book    (when renewal is requested)
Send Overdue Notice ──<<extend>>──► Calculate Fine  (when overdue)
```

#### ✅ Generalization Relationship

- One actor or use case **inherits** behavior from a parent
- **Example:** "Register Student" and "Register Faculty" are specializations of "Register Member"

```
Register Member (parent)
       ├──────► Register Student (child)
       └──────► Register Faculty (child)

Member (parent actor)
       └──────► Premium Member (child actor — gets extended borrowing)
```

---

### 🔹 Use Case Descriptions (Detailed)

#### UC-01: Borrow Book

|Field|Detail|
|---|---|
|**Actor**|Member|
|**Precondition**|Member is logged in; book is available|
|**Main Flow**|Member searches book → selects → system checks availability → loan record created → book issued|
|**Alternate Flow**|Book unavailable → Member reserves book (<<extend>>)|
|**Includes**|Check Availability|
|**Postcondition**|Loan record created; book availability decremented|

#### UC-02: Return Book

|Field|Detail|
|---|---|
|**Actor**|Member / Librarian|
|**Precondition**|Book is currently on loan|
|**Main Flow**|Member returns book → system calculates fine → fine paid (if any) → book availability updated|
|**Includes**|Calculate Fine|
|**Postcondition**|Loan record closed; book availability restored|

---

## 🔷 PART 4: UML Use Case Diagram (ASCII Version — Exam Redrawable)

```
+============================================================+
|              LIBRARY MANAGEMENT SYSTEM                     |
+============================================================+
|                                                            |
|   [Search Book]                                            |
|   [Borrow Book] ──<<include>>──► [Check Availability]     |
|   [Return Book] ──<<include>>──► [Calculate Fine]         |
|        │                               │                   |
|        │                    <<extend>> │                   |
|        ▼                               ▼                   |
|   [Reserve Book]──<<extend>>─► [Borrow Book]              |
|   [Renew Loan]  ──<<extend>>─► [Borrow Book]              |
|   [Pay Fine]                                               |
|   [View History]                                           |
|                                                            |
|   [Issue Book] ──<<include>>──► [Verify Member]           |
|   [Process Return]─<<include>>► [Calculate Fine]          |
|   [Add/Remove Book]                                        |
|   [Generate Report]──<<include>>──► [View History]        |
|                                                            |
|   [Register Member]                                        |
|        ├──generalization──► [Register Student]            |
|        └──generalization──► [Register Faculty]            |
|   [Deactivate Member]                                      |
|   [Configure System]                                       |
|                                                            |
+============================================================+

        ACTORS:
        O        O          O           O
       /|\      /|\        /|\         /|\
       / \      / \        / \         / \
     Member  Librarian    Admin      System
       |         |          |
       |─────────|──────────|── interact with use cases above
```

---

## 🔷 Advantages of OOSD for LMS

- ✅ **Modularity** — Each class (Book, Member, Loan) is independent and reusable
- ✅ **Maintainability** — Changes in one class don't ripple across the system
- ✅ **Reusability** — Member class can be extended for PremiumMember without rewriting
- ✅ **Real-world mapping** — Objects mirror actual library entities
- ✅ **Scalability** — New features (e-books, online reservations) easily added as new classes

## ❌ Disadvantages

- ❌ **Complexity** — More planning required upfront compared to procedural approach
- ❌ **Overhead** — Small libraries may not need full OO architecture
- ❌ **Learning curve** — Team needs OO expertise to design and implement correctly

---

## ⚡ QUICK REVISION — 1-4-7 Rule

### 1️⃣ One Line:

> LMS uses OOSD to model books, members, and librarians as objects, connected through use cases with include, extend, and generalization relationships.

---

### 4️⃣ Four Key Points:

1. **Actors:** Member, Librarian, Admin, System — each with distinct use cases
2. **<<include>>:** Mandatory sub-use case (Borrow Book includes Check Availability)
3. **<<extend>>:** Optional use case triggered by condition (Reserve Book extends Borrow Book)
4. **Generalization:** Register Student and Register Faculty inherit from Register Member

---

### 7️⃣ Seven Deep Points:

1. **Problem Statement** focuses on automating manual library operations
2. **7 Main Classes:** Book, Member, Librarian, Loan, Reservation, Fine, Catalogue
3. **OO Concepts:** Encapsulation (Book data hidden), Inheritance (PremiumMember), Polymorphism (calculateFine)
4. **<<include>>** = always executed = mandatory dependency between use cases
5. **<<extend>>** = conditionally executed = optional behavior added to base use case
6. **Generalization** = IS-A relationship = child actor/use case inherits from parent
7. **Use Case Descriptions** include actor, precondition, main flow, alternate flow, postcondition

---

## 📝 3 Possible Exam Questions

1. **"Draw and explain the UML Use Case Diagram for a Library Management System."** _(15 Marks)_ ← This note answers this!
2. **"Explain include, extend, and generalization relationships in Use Case Modeling with examples."** _(7 Marks)_
3. **"Perform Object-Oriented System Development for a Library Management System identifying classes, attributes, and relationships."** _(10 Marks)_

---

_📁 Save in Obsidian: `Software Engineering > OOSD > Library Management System`_ _🏷️ Tags: #OOSD #UseCase #UML #LibrarySystem #SoftwareEngineering #15Marks_