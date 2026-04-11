# 💱 Q6: Unit, Integration & System Testing – Currency Converter
**Subject:** CCS556 – Object Oriented Software Engineering  
**Topic:** Levels of Software Testing  
**Marks:** 10 | **Pages:** 3

---

> 💡 **Feynman Trick:** Think of building a car.
> - **Unit Testing** = Test each part separately (engine, brakes, wheels)
> - **Integration Testing** = Test engine + brakes together — do they work when connected?
> - **System Testing** = Drive the full assembled car on the road and check everything

That's exactly how we test a **Currency Converter application**.

---

## 📌 1. Introduction / Definition

### Unit Testing
> **Unit Testing** is the testing of **individual components/functions/modules** in isolation to verify they work correctly on their own.
- Smallest testable unit of code
- Done by **developers** during coding
- Uses **mocking** to isolate dependencies

### Integration Testing
> **Integration Testing** tests **combined modules** to verify they interact correctly when connected together.
- Finds interface defects between modules
- Done **after unit testing**
- Can be done incrementally (top-down / bottom-up)

### System Testing
> **System Testing** tests the **complete integrated system** against requirements from the user's perspective.
- Tests the whole application end-to-end
- Done by **independent QA team**
- Black Box approach

---

## 📌 2. Currency Converter Application – Overview

### What does it do?
> A **Currency Converter** app takes:
- Input: Amount + Source Currency + Target Currency
- Process: Fetch live exchange rate → Apply conversion formula
- Output: Converted Amount

### Modules of Currency Converter:
```
+---------------------+
| Currency Converter  |
|---------------------|
| 1. Input Module     | ← User enters amount & currencies
| 2. Validation Module| ← Validates input data
| 3. Rate Fetcher     | ← Fetches live exchange rates via API
| 4. Conversion Engine| ← Applies conversion formula
| 5. Output Module    | ← Displays converted result
| 6. History Module   | ← Stores conversion history
+---------------------+
```

---

## 📌 3. Level 1: Unit Testing

### 🧠 Feynman Explanation:
> You built an engine (Conversion Engine). Before connecting it to the car, you test the engine ALONE on a test bench. You give it fuel and see if it runs. That's Unit Testing.

### Unit Tests for Currency Converter:

#### Test Module 1: Input Validation Function
```
Function: validateInput(amount, fromCurrency, toCurrency)

Test Case 1:
  Input: amount = 100, from = "USD", to = "INR"
  Expected: Valid → true ✅

Test Case 2:
  Input: amount = -50, from = "USD", to = "INR"
  Expected: Invalid → Error "Amount must be positive" ✅

Test Case 3:
  Input: amount = "abc", from = "USD", to = "INR"
  Expected: Invalid → Error "Amount must be numeric" ✅

Test Case 4:
  Input: amount = 100, from = "", to = "INR"
  Expected: Invalid → Error "Select source currency" ✅
```

---

#### Test Module 2: Conversion Engine Function
```
Function: convert(amount, rate)

Test Case 1:
  Input: amount = 100, rate = 83.5 (1 USD = 83.5 INR)
  Expected Output: 8350.00 ✅

Test Case 2:
  Input: amount = 0, rate = 83.5
  Expected Output: 0.00 ✅

Test Case 3:
  Input: amount = 100, rate = 0
  Expected: Error "Exchange rate cannot be zero" ✅

Test Case 4:
  Input: amount = 1000000, rate = 83.5
  Expected Output: 83500000.00 ✅
```

---

#### Test Module 3: Rate Fetcher (with Mock)
```
Function: getExchangeRate("USD", "INR")

Since this calls a live API, we MOCK the response:
  Mock Response: { rate: 83.5 }

Test Case 1:
  Expected: Returns 83.5 ✅

Test Case 2 (API Failure):
  Mock Response: 500 Error
  Expected: Returns default rate OR throws exception ✅
```

---

#### Unit Testing Tools:
| Language | Tool |
|----------|------|
| Java | JUnit, Mockito |
| Python | PyTest, unittest |
| JavaScript | Jest, Mocha |
| C# | NUnit, xUnit |

---

## 📌 4. Level 2: Integration Testing

### 🧠 Feynman Explanation:
> Your engine works alone. Your dashboard works alone. But when you connect them — does the fuel gauge on the dashboard actually show the engine's fuel level? That's Integration Testing.

### Integration Testing Approaches:

#### Approach 1: Top-Down Integration
```
Start from Input Module → Validation Module → Rate Fetcher → Conversion Engine → Output
              ↓ test each connection as you add modules downward
```

#### Approach 2: Bottom-Up Integration
```
Start from Conversion Engine → Validation → Input → Output
              ↑ test from bottom modules upward
```

#### Approach 3: Big Bang Integration
> All modules integrated at once and tested together.
- **Risk:** Hard to identify which module caused failure.

---

### Integration Test Cases for Currency Converter:

#### Test 1: Input Module + Validation Module
```
Scenario: User enters "100 USD to INR"
  Input Module passes data → Validation Module validates
  Expected: Validation returns "Valid" 
  Test Result: PASS ✅
```

#### Test 2: Validation Module + Rate Fetcher
```
Scenario: Valid input triggers Rate Fetcher
  Validation passes → Rate Fetcher called with ("USD", "INR")
  Expected: Returns rate 83.5
  Test Result: PASS ✅
```

#### Test 3: Rate Fetcher + Conversion Engine
```
Scenario: Rate received → Conversion calculated
  Rate = 83.5, Amount = 100
  Expected: 100 × 83.5 = 8350.00
  Test Result: PASS ✅
```

#### Test 4: Conversion Engine + History Module
```
Scenario: Conversion result saved to history
  After conversion: 100 USD = 8350 INR
  Expected: History table updated with this record
  Test Result: PASS ✅
```

#### Test 5: Full Chain (Excluding Output)
```
Input(100, USD, INR) → Validate → GetRate(83.5) → Convert → History
Expected end-to-end result: 8350.00 stored in system ✅
```

---

## 📌 5. Level 3: System Testing

### 🧠 Feynman Explanation:
> Your car is fully assembled now. You take it to a test track. You test acceleration, braking, fuel efficiency, AC, radio, GPS — everything together in real conditions. That's System Testing.

### System Test Cases for Currency Converter:

| Test ID | Test Scenario | Input | Expected Output | Status |
|---------|--------------|-------|----------------|--------|
| ST-01 | Basic Conversion | 100 USD → INR | 8350.00 | PASS |
| ST-02 | Same currency | 100 USD → USD | 100.00 | PASS |
| ST-03 | Decimal input | 50.5 USD → EUR | Correct value | PASS |
| ST-04 | Very large amount | 9999999 USD → INR | Correct value | PASS |
| ST-05 | Invalid input | "abc" USD → INR | Error shown | PASS |
| ST-06 | API down | 100 USD → INR | "Rate unavailable" | PASS |
| ST-07 | Multiple conversions | 5 rapid conversions | All correct | PASS |
| ST-08 | History display | Convert 3 times | Last 3 shown in history | PASS |
| ST-09 | Performance | 100 concurrent users | Response < 2 seconds | PASS |
| ST-10 | Mobile responsiveness | Open on phone | UI adapts correctly | PASS |

---

## 📌 6. V-Model – Testing Lifecycle Diagram

```
Requirements Analysis ────────────────── System Testing
        ↓                                      ↑
   System Design ──────────── Integration Testing
        ↓                              ↑
  Module Design ───── Unit Testing
        ↓              ↑
      Coding ──────────
```

---

## 📌 7. Comparison Table

| Feature | Unit Testing | Integration Testing | System Testing |
|---------|-------------|---------------------|----------------|
| **Scope** | Single function | Multiple modules | Entire system |
| **Who tests** | Developer | Dev / QA | Independent QA |
| **When** | During development | After unit testing | After integration |
| **Type** | White Box | Grey Box | Black Box |
| **Tools** | JUnit, PyTest | Postman, TestNG | Selenium, QTP |
| **Goal** | Function correctness | Interface correctness | Requirement compliance |

---

## 📌 8. Advantages and Disadvantages

### ✅ Advantages of All Three Levels
1. **Unit Testing** → Catches bugs early, cheap to fix.
2. **Integration Testing** → Detects interface and data flow errors.
3. **System Testing** → Validates complete user requirements.

### ❌ Disadvantages
1. **Unit Testing** → Cannot catch integration errors.
2. **Integration Testing** → Needs all modules to be ready.
3. **System Testing** → Expensive, hard to pinpoint failures.

---

## 📌 9. Conclusion

> For the **Currency Converter Application**, a structured 3-level testing approach ensures every component works correctly:

1. **Unit Testing** → Every function (validate, convert, fetch rate) works alone.
2. **Integration Testing** → Modules pass data correctly between each other.
3. **System Testing** → The complete application converts currency accurately, handles errors, performs well, and satisfies user requirements.

> This layered approach follows the **V-Model** of software development and ensures **high quality** software delivery.

> 🎯 **Exam Tip:** Write at least 5 unit test cases, 4 integration test scenarios, and 8+ system test cases. Draw the V-Model diagram. Include the comparison table. Mention tools like JUnit, Selenium, Postman.

---
*Tags: #OOSE #CCS556 #UnitTesting #IntegrationTesting #SystemTesting #CurrencyConverter #VModel*
