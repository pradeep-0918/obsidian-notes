# ⚡ 2-Mark Q&A – OOSE CCS556
**Subject:** Object Oriented Software Engineering  
**Format:** 3–4 crisp points per answer  
**Exam Ready | Feynman Style**

---

## Q1. Give the importance of Software Design.

1. **Blueprint of the system** – Software design acts as a plan before coding begins, just like an architect's drawing before construction.
2. **Reduces complexity** – Breaks the system into smaller, manageable modules, making it easier to develop and maintain.
3. **Improves quality** – A good design ensures reliability, efficiency, reusability, and scalability of the final product.
4. **Saves cost and time** – Detecting design flaws early is far cheaper than fixing them after coding or deployment.

---

## Q2. State Adapter Pattern.

1. **Definition** – The Adapter pattern converts the **interface of a class into another interface** that the client expects, enabling incompatible interfaces to work together.
2. **Analogy** – Like a **phone charger adapter** that lets a 3-pin plug work in a 2-pin socket — no modification to either side.
3. **Components** – Involves a **Target** (expected interface), **Adaptee** (incompatible class), and **Adapter** (bridge between them).
4. **Use case** – Used when integrating **legacy systems** or third-party libraries into a new system without modifying their source code.

```
Client → Adapter → Adaptee (old/incompatible interface)
```

---

## Q3. What is meant by Model View Controller (MVC)?

1. **MVC** is an architectural design pattern that separates an application into **three components** to promote organized and maintainable code.
2. **Model** – Manages the **data and business logic** (e.g., database operations).
3. **View** – Handles the **UI and presentation** layer (what the user sees).
4. **Controller** – Acts as the **middleman**, receiving user input and updating the Model and View accordingly.

```
User → Controller → Model → View → User
```

> **Example:** In a shopping app — Model = product data, View = product page, Controller = add-to-cart logic.

---

## Q4. Differentiate Observer and Strategy Pattern.

| Feature | Observer Pattern | Strategy Pattern |
|---------|-----------------|------------------|
| **Purpose** | Notify dependents of state change | Switch between algorithms at runtime |
| **Relationship** | One-to-many (Subject → Observers) | One-to-one (Context → Strategy) |
| **Trigger** | State change event | Client decision/input |
| **Example** | YouTube notifications | Payment method: Card / UPI / Cash |

- **Observer** = "Tell everyone when something changes."
- **Strategy** = "Choose HOW to do something at runtime."

---

## Q5. Is Proxy Pattern a Gatekeeper? Justify.

1. **Yes**, the Proxy pattern acts as a **gatekeeper** because it controls access to a real object by sitting between the client and the actual object.
2. **Access control** – The proxy checks whether the client has **permission** before allowing access to the real subject (e.g., security proxy).
3. **Lazy initialization** – The proxy delays creating the real object until it is actually needed, saving resources (virtual proxy).
4. **Example** – An **ATM card** is a proxy for your bank account. You don't go directly to the bank vault — the ATM (proxy/gatekeeper) controls what you can access.

```
Client → Proxy (checks access) → Real Object
```

---

## Q6. List out the Testing Types.

1. **Unit Testing** – Testing individual modules/functions in isolation.
2. **Integration Testing** – Testing combined modules for interface correctness.
3. **System Testing** – Testing the complete system against requirements.
4. **Regression Testing** – Re-testing after changes to ensure old features still work.
5. **Black Box Testing** – Testing based on input/output without knowledge of code.
6. **White Box Testing** – Testing internal code logic and structure.
7. **User Acceptance Testing (UAT)** – End-user validates the system meets business needs.
8. **Performance Testing** – Load, stress, and spike testing.

---

## Q7. Define Black Box Testing.

1. **Definition** – Black Box Testing is a software testing technique where the tester validates the system's **functionality based only on inputs and expected outputs**, without any knowledge of the internal code or structure.
2. **Also called** Behavioral Testing or Specification-based Testing.
3. **Techniques include** – Equivalence Partitioning, Boundary Value Analysis, Decision Table Testing, and Use Case Testing.
4. **Who performs it** – Usually done by **QA testers** who are independent from the development team.

> **Analogy:** You press a button on a TV remote and check if the channel changes. You don't care how the signal is processed inside.

---

## Q8. Sketch the Debugging Model in Software Testing.

1. **Debugging** is the process of **locating and fixing defects** identified during testing.

```
Testing Reveals Failure
         ↓
   Identify the Bug
  (Locate in source)
         ↓
   Diagnose the Cause
  (Understand why it failed)
         ↓
   Fix / Correct the Bug
         ↓
   Re-test (Regression)
         ↓
  Bug Confirmed Fixed ✅
```

2. **Debugging approaches** – Brute force (print statements), backtracking, cause elimination, and program slicing.
3. **Debugging ≠ Testing** – Testing finds that a bug exists; debugging finds WHERE and WHY it exists.
4. **Tools** – Eclipse Debugger, GDB, Chrome DevTools, IntelliJ Debugger.

---

## Q9. List the Types of Integration Testing.

1. **Big Bang Integration** – All modules are integrated at once and tested together (risky, hard to isolate failures).
2. **Top-Down Integration** – Testing starts from the **top-level module** down to sub-modules; stubs replace lower modules.
3. **Bottom-Up Integration** – Testing starts from the **lowest-level modules** up; drivers replace upper modules.
4. **Sandwich (Hybrid) Integration** – Combines Top-Down and Bottom-Up; tests middle layer from both sides simultaneously.

> **Extra:** Incremental Integration = modules added and tested one by one (can be top-down or bottom-up).

---

## Q10. Apply Symbolic Execution in Real-Time Systems.

1. **Symbolic Execution** is a testing technique where **symbolic values** (instead of actual data) are used as inputs to analyze all possible execution paths of a program.
2. **In real-time systems** (like flight control, ABS brakes, medical devices), symbolic execution is used to verify that **all time-critical paths** execute within deadline constraints.
3. **Application example** – In an **antilock braking system (ABS)**, symbolic execution checks all paths in the braking algorithm to ensure the brake releases within safe time limits under all conditions.
4. **Benefit** – It can detect **timing violations, race conditions, and unreachable code** in safety-critical real-time embedded systems without needing actual hardware.

---

## Q11. Give the Importance of Functional Independence.

1. **Definition** – Functional Independence means each module performs **one and only one well-defined function** with minimal interaction with other modules.
2. **Measured by** – **Cohesion** (how focused a module is) and **Coupling** (how dependent modules are on each other). Goal: High Cohesion + Low Coupling.
3. **Easier maintenance** – An independent module can be **modified, tested, or replaced** without affecting other modules.
4. **Reduces error propagation** – A bug in one module does not cascade into other modules due to loose coupling.

---

## Q12. Define Modular Design.

1. **Modular Design** is a software design principle where a large system is **divided into smaller, self-contained units called modules**, each handling a specific function.
2. Each module has a **clearly defined interface** (inputs/outputs) and hides its internal implementation (information hiding).
3. **Benefits** – Promotes reusability, parallel development, easier debugging, and simplified maintenance.
4. **Example** – An e-commerce app divided into: Login Module, Product Module, Cart Module, Payment Module, Order Module.

---

## Q13. What are the Steps for Mapping Design to Code?

1. **Identify classes and objects** – Translate each design class into a programming language class with attributes and methods.
2. **Map relationships** – Convert UML associations, inheritance, and aggregations into code (extends, implements, references).
3. **Implement methods** – Write the actual logic for each operation defined in the design diagrams (sequence/activity).
4. **Integrate modules** – Connect all classes/modules according to the component and deployment diagrams, then test the implementation against the design.

```
Class Diagram → Java Classes
Sequence Diagram → Method call chains
State Diagram → switch/if-else logic
Activity Diagram → Loop and condition code
```

---

## Q14. Analyze the Situation to Use Factory Method Pattern and Its Advantages.

**When to use Factory Method:**
1. Use it when you don't know **which class of objects** needs to be created until runtime (e.g., the type of payment method is decided by user choice).
2. Use it when **subclasses should control** which object gets created (e.g., different report generators: PDF, Excel, HTML).

**Advantages:**
3. **Loose coupling** – The client doesn't need to know which concrete class is being instantiated.
4. **Easy to extend** – Adding a new product type (e.g., new payment method) requires only adding a new subclass, not modifying existing code (Open/Closed Principle).

> **Example:** A vehicle factory where `createVehicle()` returns Car, Bike, or Truck based on input — the client just calls `createVehicle()`.

---

## Q15. Identify the Benefits of Bridge Pattern.

1. **Decouples abstraction from implementation** – Both can vary and evolve independently without affecting each other.
2. **Improves extensibility** – You can extend both the abstraction and implementation hierarchies separately without code changes.
3. **Hides implementation details** – Clients interact only with the abstraction; implementation changes are transparent.
4. **Reduces class explosion** – Without Bridge, combining 3 shapes × 3 colors = 9 classes. With Bridge: 3 + 3 = 6 classes only.

> **Example:** A universal **TV remote (abstraction)** that can control any **brand of TV (implementation)** — Samsung, Sony, or LG — without changing the remote.

---

## Q16. List the Four Phases of Object-Oriented Modeling Techniques (OMT).

1. **System Analysis** – Understand the real-world problem; build an **Object Model** (classes, attributes, relationships) and **Dynamic Model** (state diagrams).
2. **System Design** – Define the overall **architecture**, subsystems, and hardware/software mapping. Choose design patterns and strategies.
3. **Object Design** – Refine the object model, add details like algorithms, data structures, interfaces, and optimize for implementation.
4. **Implementation** – Translate the refined object design into actual **programming language code** (Java, C++, etc.) and test it.

> **OMT was proposed by James Rumbaugh** and is one of the predecessors to UML.

---

## Q17. Define Test Plan. What are its Components?

1. **Test Plan** is a **formal document** that describes the scope, approach, resources, schedule, and activities of a testing effort for a software project.

**Components of a Test Plan:**
2. **Test Scope** – What will be tested and what will NOT be tested.
3. **Test Objectives** – Goals of testing (e.g., verify login, validate payment flow).
4. **Test Schedule** – Timeline for each testing phase and milestones.
5. **Resources** – Team members, tools, hardware, and environments required.
6. **Risk & Mitigation** – Identified risks (e.g., API unavailability) and how to handle them.
7. **Entry/Exit Criteria** – Conditions to start and stop testing.

---

## Q18. Illustrate the Impact of Object Orientation in Testing.

1. **Encapsulation challenges** – Internal state is hidden, making it harder to observe object behavior during testing; requires special test methods or reflection.
2. **Inheritance complexity** – Subclasses inherit parent behavior, so test cases for parent class must be **re-run for all subclasses** (regression).
3. **Polymorphism testing** – A single method call can behave differently at runtime; all **polymorphic variations** must be tested separately.
4. **Object interaction testing** – OOP systems consist of many interacting objects; integration testing must verify **message passing and collaboration** between objects is correct.

---

## Q19. Define the Term Object Interoperability.

1. **Object Interoperability** is the ability of objects from **different systems, platforms, or programming languages** to communicate, exchange data, and work together seamlessly.
2. Achieved through **standard interfaces and middleware** such as CORBA (Common Object Request Broker Architecture), DCOM, or REST APIs.
3. **Example** – A Java object on a server communicating with a Python object on another server via a REST API — both interoperate despite language differences.
4. **Importance** – Enables **distributed systems, microservices, and cross-platform applications** to function as a unified system without rewriting code.

---

## Q20. Illustrate the Steps Needed to Create a Test Plan.

1. **Analyze the product** – Understand the software requirements (SRS), architecture, and features to identify what needs testing.
2. **Define test scope and objectives** – Decide what IS and IS NOT in scope; set measurable test goals.
3. **Define test strategy** – Choose testing types (unit, integration, system), testing techniques (BVA, equivalence partitioning), and tools (Selenium, JUnit).
4. **Identify resources and schedule** – Assign testers, allocate environments, and create a timeline with milestones and entry/exit criteria.

```
Step 1: Understand Requirements (SRS)
Step 2: Define Scope & Objectives
Step 3: Select Testing Types & Tools
Step 4: Create Test Cases & Test Data
Step 5: Assign Resources & Set Schedule
Step 6: Define Entry/Exit Criteria
Step 7: Review & Approve Test Plan
Step 8: Execute & Report
```

---

*Tags: #OOSE #CCS556 #2Marks #QuickRevision #ExamPrep #SoftwareTesting #DesignPatterns*
