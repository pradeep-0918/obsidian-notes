# 👥 Q4: GOF Design Patterns – Explained in Detail
**Subject:** CCS556 – Object Oriented Software Engineering  
**Topic:** Gang of Four (GoF) Design Patterns  
**Marks:** 10 | **Pages:** 3

---

> 💡 **Feynman Trick:** Imagine four experienced builders wrote a book of **23 construction blueprints** after solving the same building problems over and over. Now any new builder can use those blueprints instead of starting from scratch. That's exactly what the **Gang of Four** did — but for software!

---

## 📌 1. Introduction / Definition

### Who are the Gang of Four (GoF)?
> The **Gang of Four** refers to four computer scientists who co-authored the landmark book:  
> 📖 **"Design Patterns: Elements of Reusable Object-Oriented Software" (1994)**

| Author | Role |
|--------|------|
| **Erich Gamma** | Lead Author, Eclipse IDE contributor |
| **Richard Helm** | IBM Research Scientist |
| **Ralph Johnson** | University of Illinois professor |
| **John Vlissides** | IBM Research Scientist |

### What is a GoF Design Pattern?
> A **GoF Design Pattern** is a **standard, named solution** to a **recurring design problem** in object-oriented software, categorized into 3 groups with a total of **23 patterns**.

---

## 📌 2. Structure of GoF Patterns (1-4-7 Rule)

> 🧠 **Remember:** GoF = **3 Categories** → **23 Total Patterns**
> - Creational: **5 patterns**
> - Structural: **7 patterns**
> - Behavioral: **11 patterns**

---

## 📌 3. Category 1: Creational Patterns (5)
> **"Control OBJECT CREATION"**

These patterns abstract the instantiation process and make systems independent of how objects are created.

### 3.1 Singleton Pattern
> Ensures **only one instance** of a class exists.

```
+----------------------+
|      Singleton       |
|----------------------|
| - instance: Singleton|
|----------------------|
| - Singleton()        |
| + getInstance()      |
+----------------------+
```
**Example:** Logger, Database Connection, Config File Reader

---

### 3.2 Factory Method Pattern
> Defines an interface for creating objects, but **subclasses decide** which class to instantiate.

```
Creator (abstract)
    + factoryMethod(): Product

ConcreteCreator
    + factoryMethod(): ConcreteProduct
```
**Example:** Vehicle factory — Car, Bike, Truck all created by one factory method.

---

### 3.3 Abstract Factory Pattern
> Provides an interface for creating **families of related objects** without specifying concrete classes.

**Example:** UI Theme Factory — Light Theme (LightButton, LightTextField) vs Dark Theme (DarkButton, DarkTextField)

---

### 3.4 Builder Pattern
> Separates the **construction of a complex object** from its representation.

```
Director → uses → Builder Interface
                      ↓
              ConcreteBuilder → builds → Product
```
**Example:** Building a computer — choose CPU, RAM, GPU, Storage step by step.

---

### 3.5 Prototype Pattern
> Creates new objects by **copying/cloning** an existing object.

**Example:** Cloning a document template in MS Word.

---

## 📌 4. Category 2: Structural Patterns (7)
> **"ORGANIZE/COMPOSE classes and objects"**

### 4.1 Adapter Pattern
> Converts one interface to another that the client expects.
```
Client → Adapter → Adaptee (Incompatible interface)
```
**Example:** Phone charger adapter (converts 3-pin to 2-pin socket)

---

### 4.2 Bridge Pattern
> Decouples an **abstraction** from its **implementation** so both can vary independently.

**Example:** Remote control (abstraction) and TV/AC (implementation) — any remote can control any device.

---

### 4.3 Composite Pattern
> Composes objects into **tree structures** to represent part-whole hierarchies.

```
Component (abstract)
    ├── Leaf (no children)
    └── Composite (has children)
```
**Example:** File system — Folder contains Files and other Folders.

---

### 4.4 Decorator Pattern
> Adds new responsibilities to an object **dynamically** without modifying its class.

```
Coffee → MilkDecorator → SugarDecorator → ChocolateDecorator
```
**Example:** Java I/O streams — BufferedReader wraps FileReader.

---

### 4.5 Facade Pattern
> Provides a **simplified interface** to a complex subsystem.

```
Client → Facade → [SubsystemA, SubsystemB, SubsystemC]
```
**Example:** TV Remote — one button to control complex electronics.

---

### 4.6 Flyweight Pattern
> Shares common parts of objects to efficiently support **large numbers of fine-grained objects**.

**Example:** Text editor — each character object shares font/color data instead of storing separately.

---

### 4.7 Proxy Pattern
> Provides a **surrogate or placeholder** for another object to control access.

**Example:** ATM card is a proxy for your bank account.

---

## 📌 5. Category 3: Behavioral Patterns (11)
> **"COMMUNICATION between objects"**

| # | Pattern | Purpose | Example |
|---|---------|---------|---------|
| 1 | **Chain of Responsibility** | Pass request along chain | Customer support escalation |
| 2 | **Command** | Encapsulate request as object | Undo/Redo button |
| 3 | **Interpreter** | Define grammar and interpret sentences | SQL parser |
| 4 | **Iterator** | Access elements of collection sequentially | For-each loop |
| 5 | **Mediator** | Object that controls communication | Air Traffic Controller |
| 6 | **Memento** | Save and restore object's state | Game Save/Load |
| 7 | **Observer** | Notify dependents of state change | YouTube notifications |
| 8 | **State** | Change behavior when state changes | Vending machine states |
| 9 | **Strategy** | Switch between algorithms at runtime | Sorting algorithm selector |
| 10 | **Template Method** | Skeleton of algorithm in base class | Recipe template |
| 11 | **Visitor** | Add operations without changing classes | Tax calculator on products |

---

## 📌 6. GoF Pattern Template Structure

Every GoF pattern is described with:

| Element | Description |
|---------|-------------|
| **Name** | Pattern identifier |
| **Intent** | What problem it solves |
| **Motivation** | Example scenario |
| **Applicability** | When to use it |
| **Structure** | UML class diagram |
| **Participants** | Classes and roles |
| **Collaborations** | How they interact |
| **Consequences** | Trade-offs |
| **Implementation** | Code hints |

---

## 📌 7. GoF Pattern Summary Diagram

```
              GoF Design Patterns (23)
             /          |             \
   Creational        Structural      Behavioral
   (5 Patterns)     (7 Patterns)    (11 Patterns)
   - Singleton       - Adapter       - Observer
   - Factory         - Bridge        - Strategy
   - Abstract Fac    - Composite     - Command
   - Builder         - Decorator     - Iterator
   - Prototype       - Facade        - State
                     - Flyweight     - Template
                     - Proxy         - Mediator
                                     - Memento
                                     - Chain
                                     - Visitor
                                     - Interpreter
```

---

## 📌 8. Advantages and Disadvantages of GoF Patterns

### ✅ Advantages
1. **Proven Solutions** – Battle-tested by real-world experience.
2. **Common Vocabulary** – All developers know "Observer pattern" instantly.
3. **Maintainability** – Code is easier to modify and extend.
4. **Scalability** – Systems built on patterns scale better.
5. **Reduces complexity** – Complex problems solved elegantly.
6. **OOP Aligned** – Supports SOLID principles.

### ❌ Disadvantages
1. **Over-engineering** – Can make simple code unnecessarily complex.
2. **Learning curve** – 23 patterns are hard to master initially.
3. **Performance overhead** – Extra abstraction layers slow down some patterns.
4. **Wrong pattern misuse** – Applying wrong pattern makes code worse.
5. **Documentation heavy** – Patterns require thorough documentation.

---

## 📌 9. Conclusion

> The **Gang of Four** revolutionized software engineering by cataloging **23 design patterns** that solve recurring OOP design problems. These patterns are the foundation of **enterprise-grade software development**.

- **Creational** patterns control object creation.
- **Structural** patterns organize classes and objects.
- **Behavioral** patterns manage communication between objects.

> Every professional developer must understand GoF patterns — they are the language of software architecture.

> 🎯 **Exam Tip:** List all 3 categories with at least 3-4 pattern names each. Explain one Creational (Singleton), one Structural (Facade/Decorator), one Behavioral (Observer/Strategy) in detail with diagrams.

---
*Tags: #OOSE #CCS556 #GoF #GangOfFour #DesignPatterns #Creational #Structural #Behavioral*
