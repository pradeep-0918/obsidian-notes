# 🏥 Q3: Healthcare System using StarUML
**Subject:** CCS556 – Object Oriented Software Engineering  
**Topic:** StarUML & Healthcare System Design  
**Marks:** 10 | **Pages:** 3

---

> 💡 **Feynman Trick:** Imagine you are an architect designing a hospital. Before laying any bricks, you draw a **blueprint**. In software, that blueprint is a **UML diagram**, and **StarUML** is the tool you use to draw it — digitally, professionally, and precisely.

---

## 📌 1. Introduction / Definition

### What is StarUML?
> **StarUML** is an **open-source UML modeling tool** used to create visual diagrams that represent the structure and behavior of software systems.

- Based on **UML 2.x standard**
- Supports multiple diagram types
- Used in **Object-Oriented Analysis and Design (OOAD)**
- Available for Windows, macOS, Linux

### What is a Healthcare System?
> A **Healthcare Management System (HMS)** is software that manages **patient records, appointments, billing, doctors, and pharmacy** operations in a hospital or clinic.

---

## 📌 2. Key Features of StarUML

| Feature | Description |
|---------|-------------|
| **Multi-diagram Support** | Use Case, Class, Sequence, Activity, State, Component, Deployment |
| **UML 2.x Compliant** | Follows latest UML standards |
| **Code Generation** | Generate Java/C++ code from class diagrams |
| **Reverse Engineering** | Generate diagrams from existing code |
| **Extension Support** | Add plugins for extra functionality |
| **Export Options** | Export as PNG, PDF, SVG, HTML |
| **MDA Support** | Model Driven Architecture support |
| **Cross-Platform** | Runs on Windows, Mac, Linux |

---

## 📌 3. UML Diagrams for Healthcare System

### ▶ Diagram 1: Use Case Diagram
> **Who does what in the system?**

**Actors in Healthcare System:**
- Patient
- Doctor
- Receptionist
- Pharmacist
- Admin

```
                  +-----------------------------+
                  |   Healthcare System         |
                  |                             |
   [Patient] ---->| Register / Book Appointment |
                  | View Medical Records        |
                  |                             |
   [Doctor]  ---->| View Patient Records        |
                  | Prescribe Medicine          |
                  | Update Diagnosis            |
                  |                             |
  [Receptionist]->| Manage Appointments         |
                  | Generate Bills              |
                  |                             |
  [Pharmacist] -->| Manage Medicine Inventory   |
                  | Dispense Prescriptions      |
                  |                             |
   [Admin]   ---->| Manage Users & Reports      |
                  +-----------------------------+
```

---

### ▶ Diagram 2: Class Diagram
> **What are the objects and their relationships?**

```
+------------------+        +------------------+
|     Patient      |        |     Doctor       |
|------------------|        |------------------|
| - patientID      |        | - doctorID       |
| - name           |        | - name           |
| - dob            |        | - specialization |
| - contact        |        | - qualification  |
|------------------|        |------------------|
| + register()     |1     * | + prescribe()    |
| + bookAppt()     |--------| + diagnose()     |
| + viewRecords()  |        | + viewSchedule() |
+------------------+        +------------------+
         |                           |
         |                           |
         v                           v
+------------------+        +------------------+
|   Appointment    |        |   Prescription   |
|------------------|        |------------------|
| - apptID         |        | - prescID        |
| - date           |        | - medicines      |
| - time           |        | - dosage         |
| - status         |        | - date           |
|------------------|        |------------------|
| + schedule()     |        | + generate()     |
| + cancel()       |        | + update()       |
+------------------+        +------------------+
```

---

### ▶ Diagram 3: Sequence Diagram
> **How do objects interact over time for "Book Appointment"?**

```
Patient       Receptionist    AppointmentSystem     Doctor
  |                |                  |                |
  |--Request Appt->|                  |                |
  |                |--Check Schedule->|                |
  |                |<--Slot Available-|                |
  |                |--Book Slot------>|                |
  |                |                  |--Notify------->|
  |<--Confirmation-|                  |                |
  |                |                  |                |
```

---

### ▶ Diagram 4: Activity Diagram
> **What is the flow for Patient Registration?**

```
[Start]
   ↓
Patient Arrives at Reception
   ↓
Receptionist Collects Information
   ↓
Is Patient Existing?
  ↙ Yes          ↘ No
Update Record   Create New Record
       ↘          ↙
    Book Appointment
          ↓
    Assign Doctor
          ↓
    Generate Token
          ↓
        [End]
```

---

## 📌 4. Modules of Healthcare System

| Module | Functionality |
|--------|--------------|
| **Patient Management** | Registration, Records, History |
| **Appointment Management** | Book, Cancel, Reschedule |
| **Doctor Management** | Schedules, Specialization, Availability |
| **Pharmacy Module** | Inventory, Prescriptions, Dispensing |
| **Billing Module** | Invoice Generation, Payment, Insurance |
| **Lab Module** | Test Requests, Reports, Results |
| **Admin Module** | User Management, Reports, Access Control |

---

## 📌 5. How to Develop in StarUML – Steps

```
Step 1: Install StarUML (staruml.io)
Step 2: Create New Project → Select UML 2.x
Step 3: Add Use Case Diagram → Define Actors & Use Cases
Step 4: Add Class Diagram → Define Classes, Attributes, Methods
Step 5: Add Sequence Diagram → Define Object Interactions
Step 6: Add Activity Diagram → Show Process Flow
Step 7: Add State Diagram → Show object state transitions
Step 8: Export as PNG/PDF or Generate Code
```

---

## 📌 6. Advantages and Disadvantages of StarUML

### ✅ Advantages
1. **Free and Open Source** – No licensing cost.
2. **Supports UML 2.x** – Industry-standard compliance.
3. **Multiple Diagram Types** – Complete system modeling.
4. **Code Generation** – Saves development time.
5. **User-Friendly UI** – Drag-and-drop interface.
6. **Cross-Platform** – Works on all major OS.
7. **Plugin Support** – Extend functionality.

### ❌ Disadvantages
1. **Limited free features** – Some advanced features are paid.
2. **Performance issues** – Slow with very large diagrams.
3. **No cloud collaboration** – Cannot work online with team.
4. **Steep learning curve** – for absolute beginners.
5. **Less support** compared to Enterprise tools like Rational Rose.

---

## 📌 7. Comparison: StarUML vs Other Tools

| Feature | StarUML | Rational Rose | Lucidchart |
|---------|---------|---------------|------------|
| Cost | Free/Paid | Expensive | Subscription |
| UML 2.x | ✅ | ✅ | Limited |
| Code Gen | ✅ | ✅ | ❌ |
| Cloud | ❌ | ❌ | ✅ |
| Easy to Use | Medium | Hard | Easy |

---

## 📌 8. Conclusion

> **StarUML** is a powerful and practical tool for designing software systems using UML diagrams. When applied to a **Healthcare System**, it helps visualize complex relationships between patients, doctors, appointments, and billing modules.

- Use Case diagrams define **what** the system does.
- Class diagrams define **how** the system is structured.
- Sequence diagrams show **when and how** objects interact.
- Activity diagrams show **process flow**.

> Together, these diagrams ensure a clear, structured, and error-free system before any code is written.

> 🎯 **Exam Tip:** Draw at least **Use Case + Class Diagram** for the healthcare system. List minimum 5 features of StarUML. Mention all 5 actors.

---
*Tags: #OOSE #CCS556 #StarUML #HealthcareSystem #UML #UseCaseDiagram #ClassDiagram*
