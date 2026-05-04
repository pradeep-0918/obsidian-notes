# 🔔 Observer Design Pattern — Stock Price Alert System

**Subject:** Software Engineering / Design Patterns | **Marks:** 15 | **Type:** Theory + Diagram

---

> 🧠 **Core Idea (1 Line):** The Observer pattern defines a **one-to-many dependency** so that when one object (Subject) changes state, all its dependents (Observers) are **notified and updated automatically**.

---

## 🔷 PART 1: Problem Statement

### 📌 The Problem — Stock Price Alert System

> **"In a stock trading platform, multiple components — a mobile app, a portfolio tracker, an email alert service, and a chart renderer — all need to react immediately when a stock's price changes. Without a proper pattern, each component must poll the stock data repeatedly, leading to tight coupling, redundant network calls, and fragile code that breaks whenever a new subscriber is added."**

### Without Observer Pattern — Problems:

- Each subscriber (Mobile App, Email, Chart) directly checks price every second → **Polling overhead**
- Adding a new subscriber (e.g., SMS Alert) requires **modifying the Stock class** → violates Open/Closed Principle
- All components are **tightly coupled** to the stock object's internals
- **Missed updates** possible if polling intervals don't align with price changes

### With Observer Pattern — Solution:

- Stock notifies all registered subscribers **automatically** on price change
- New subscribers can be **added/removed at runtime** without changing the Stock class
- Subscribers only know the `Observer` interface — **fully decoupled**

---
	
## 🔷 PART 2: What is the Observer Pattern?

### 📌 Definition

> The **Observer Pattern** is a **Behavioral Design Pattern** in which an object (called the **Subject** or **Publisher**) maintains a list of dependents (called **Observers** or **Subscribers**) and notifies them automatically of any state changes, usually by calling one of their methods.

Also known as: **Publish-Subscribe Pattern**, **Event-Listener Pattern**, **Dependents Pattern**

---

### 🔹 Classification

|Property|Value|
|---|---|
|**Type**|Behavioral Design Pattern|
|**GoF Category**|Gang of Four — Behavioral|
|**Core Principle**|Open/Closed Principle + Loose Coupling|
|**Relationship**|One-to-Many (1 Subject : Many Observers)|

---

## 🔷 PART 3: Participants in Observer Pattern

|Participant|Role|In Our Problem|
|---|---|---|
|**Subject** (interface)|Declares attach, detach, notify methods|`Subject` interface|
|**ConcreteSubject**|Implements Subject; holds state; notifies observers|`StockMarket` class|
|**Observer** (interface)|Declares the `update()` method|`Observer` interface|
|**ConcreteObserver**|Implements Observer; reacts to state change|`MobileApp`, `PortfolioTracker`, `EmailAlerts`, `ChartRenderer`|

---

## 🔷 PART 4: UML Class Diagram (ASCII — Exam Redrawable)

```
+---------------------+          observers         +------------------+
|    «interface»      |─────────────────────────►  |   «interface»    |
|       Subject       |                             |     Observer     |
+---------------------+                             +------------------+
| + attach(Observer)  |                             | + update(price)  |
| + detach(Observer)  |                             +------------------+
| + notifyObservers() |                                    ▲
+---------------------+                                    |
         ▲                                      (implements)|
         | (implements)                    ┌────────────────┼────────────────┐
         |                                 |                |                |
+----------------------+           +----------+    +----------+    +----------+
|    StockMarket       |           |MobileApp |    |Portfolio |    |EmailAlrt |
+----------------------+           +----------+    +----------+    +----------+
|- symbol: String      |  notifies | +update()|    | +update()|    | +update()|
|- price: float        |─────────► +----------+    +----------+    +----------+
|- observers: List     |
+----------------------+                                    +---------------+
| + setPrice(p: float) |                                    |ChartRenderer  |
| + notifyObservers()  |                                    +---------------+
+----------------------+                                    | + update()    |
                                                            +---------------+
```

---

## 🔷 PART 5: Sequence Diagram (ASCII — How It Works at Runtime)

```
Client        StockMarket       MobileApp    Portfolio   EmailAlerts   Chart
  |                |                |             |            |          |
  |─setPrice(2450)─►                |             |            |          |
  |                |─price=2450     |             |            |          |
  |                |─notifyObservers()            |            |          |
  |                |─update(2450)──►|             |            |          |
  |                |                |─push alert  |            |          |
  |                |◄───────────────|             |            |          |
  |                |─update(2450)───────────────► |            |          |
  |                |                |  recalc P&L |            |          |
  |                |◄───────────────────────────── |            |          |
  |                |─update(2450)───────────────────────────►  |          |
  |                |                |             |  send email |          |
  |                |◄────────────────────────────────────────── |          |
  |                |─update(2450)───────────────────────────────────────► |
  |                |                |             |            | redraw   |
  |                |◄──────────────────────────────────────────────────── |
  |◄───────────────|
```

---

## 🔷 PART 6: Implementation (Pseudocode / Java-style)

### Step 1: Define Interfaces

```java
// Subject Interface
interface Subject {
    void attach(Observer o);
    void detach(Observer o);
    void notifyObservers();
}

// Observer Interface
interface Observer {
    void update(float price);
}
```

### Step 2: Concrete Subject — StockMarket

```java
class StockMarket implements Subject {
    private String symbol;
    private float price;
    private List<Observer> observers = new ArrayList<>();

    public void attach(Observer o)  { observers.add(o); }
    public void detach(Observer o)  { observers.remove(o); }

    public void setPrice(float newPrice) {
        this.price = newPrice;
        notifyObservers();          // automatically tells everyone
    }

    public void notifyObservers() {
        for (Observer o : observers) {
            o.update(price);        // calls update on each observer
        }
    }
}
```

### Step 3: Concrete Observers

```java
class MobileApp implements Observer {
    public void update(float price) {
        System.out.println("PUSH ALERT: Stock price updated to ₹" + price);
    }
}

class PortfolioTracker implements Observer {
    public void update(float price) {
        System.out.println("Portfolio recalculated at ₹" + price);
    }
}

class EmailAlerts implements Observer {
    public void update(float price) {
        System.out.println("EMAIL SENT: Price alert at ₹" + price);
    }
}

class ChartRenderer implements Observer {
    public void update(float price) {
        System.out.println("Chart updated with new point: ₹" + price);
    }
}
```

### Step 4: Client Code

```java
StockMarket reliance = new StockMarket("RELIANCE");

Observer app      = new MobileApp();
Observer tracker  = new PortfolioTracker();
Observer email    = new EmailAlerts();
Observer chart    = new ChartRenderer();

// Register observers
reliance.attach(app);
reliance.attach(tracker);
reliance.attach(email);
reliance.attach(chart);

// Price change → all four observers notified automatically
reliance.setPrice(2450.75f);

// Output:
// PUSH ALERT: Stock price updated to ₹2450.75
// Portfolio recalculated at ₹2450.75
// EMAIL SENT: Price alert at ₹2450.75
// Chart updated with new point: ₹2450.75

// Detach one observer dynamically
reliance.detach(email);
reliance.setPrice(2510.00f);  // EmailAlerts no longer notified
```

---

## 🔷 PART 7: Real-World Applications of Observer Pattern

|Domain|Subject|Observers|
|---|---|---|
|**Stock Trading**|StockMarket|Mobile App, Chart, Email|
|**Social Media**|User (Publisher)|Followers' Feeds|
|**MVC Architecture**|Model|View, Controller|
|**Event Systems**|Button (UI Element)|Click Handlers|
|**Weather App**|WeatherStation|Phone Display, Dashboard, Logger|
|**News Agency**|NewsChannel|Subscribers|

---

## 🔷 PART 8: Advantages and Disadvantages

### ✅ Advantages

- **Loose Coupling** — Subject knows only the `Observer` interface, not concrete types
- **Open/Closed Principle** — Add new observers without modifying Subject
- **Dynamic Subscription** — Observers can attach/detach at runtime
- **Broadcast Communication** — One change notifies many subscribers simultaneously
- **Reusability** — Observer interface can be reused across different subjects

### ❌ Disadvantages

- **Unexpected Updates** — Observers may be notified even for irrelevant changes
- **Memory Leaks** — Forgotten observers stay in the list (Lapsed Listener Problem)
- **Order Not Guaranteed** — Notification order among observers is implementation-dependent
- **Cascading Updates** — One notification can trigger a chain of further notifications
- **Debugging Difficulty** — Hard to trace which observer caused what side-effect

---

## 🔷 PART 9: Observer vs Related Patterns

|Feature|Observer|Mediator|Event Bus|
|---|---|---|---|
|**Coupling**|Subject knows Observer interface|All know Mediator|Fully decoupled|
|**Direction**|One-to-many|Many-to-many|Many-to-many|
|**Control**|Subject drives|Mediator drives|Event system drives|
|**Example**|Stock alerts|Air traffic control|Message queues|

---

## ⚡ QUICK REVISION — 1-4-7 Rule

### 1️⃣ One Line:

> Observer = one Subject notifies many Observers automatically when state changes — the classic Publish-Subscribe pattern.

---

### 4️⃣ Four Key Points:

1. **Four participants:** Subject (interface), ConcreteSubject, Observer (interface), ConcreteObserver
2. **Core method:** `notifyObservers()` loops through observer list and calls `update()` on each
3. **Loose coupling:** Subject only depends on the `Observer` interface, never on concrete classes
4. **Dynamic:** Observers can be attached or detached at runtime without modifying Subject

---

### 7️⃣ Seven Deep Points:

1. Observer is a **Behavioral Design Pattern** from Gang of Four (GoF)
2. Relationship is **one-to-many**: 1 Subject → N Observers
3. `setPrice()` triggers `notifyObservers()` → loops → `update()` on each observer
4. Follows **Open/Closed Principle** — Subject is closed for modification, open for extension
5. **Lapsed Listener Problem** — biggest pitfall; always detach unused observers
6. Used in **MVC pattern**: Model is Subject, View is Observer
7. Java has built-in support via `java.util.Observable` (deprecated) and modern event listeners

---

## 📝 3 Possible Exam Questions

1. **"Explain the Observer Design Pattern with a suitable problem and solution. Draw UML diagrams."** _(15 Marks)_ ← This note answers this!
2. **"Compare Observer pattern with Mediator pattern. When would you use each?"** _(7 Marks)_
3. **"Implement the Observer pattern for a weather monitoring system."** _(10 Marks)_

---

_📁 Save in Obsidian: `Software Engineering > Design Patterns > Behavioral > Observer Pattern`_ _🏷️ Tags: #DesignPatterns #Observer #Behavioral #GoF #StockSystem #15Marks #UML_