# 💰 Pricing & Discount Engine

A Java project built with **Gradle** demonstrating the full software engineering workflow:
Git, refactoring, unit testing with JUnit 5.

---

## 📌 Project Overview

This engine calculates the **final price** of a customer order by applying:
- Customer type discounts (VIP / REGULAR)
- Promotional discount codes (SAVE5, SAVE10, SAVE20)
- Tax (19% TVA)

### Inputs
| Parameter | Description | Example |
|---|---|---|
| `prices` | List of item prices | `[100.0, 50.0]` |
| `quantities` | Quantity for each item | `[2, 3]` |
| `customerType` | REGULAR or VIP | `VIP` |
| `discountCode` | Promotional code | `SAVE10` |

### Outputs
| Output | Description |
|---|---|
| Subtotal | Sum of (price × quantity) for all items |
| Discount Amount | Total discount applied |
| Tax | 19% applied after discount |
| Final Price | Amount the customer pays |

---

## 🧮 Calculation Formula

```
Subtotal       = Σ (price[i] × quantity[i])
Discount       = VIP discount (10%) + Code discount (5/10/20%)
After Discount = Subtotal - Discount
Tax            = After Discount × 19%
Final Price    = After Discount + Tax
```

**Discount rates:**
- `REGULAR` customer → 0%
- `VIP` customer → 10%
- `SAVE5` code → 5%
- `SAVE10` code → 10%
- `SAVE20` code → 20%

> ⚠️ The total discount can never exceed the subtotal (final price ≥ 0)

---

## 🏗️ Project Structure

```
pricing-engine/
├── build.gradle
├── README.md
└── src/
    ├── main/java/
    │   ├── PricingEngine.java            ← Initial bad design (single class)
    │   └── PricingEngineRefactored.java  ← Refactored version (separated concerns)
    └── test/java/
        └── PricingEngineTest.java        ← JUnit 5 unit tests
```

---

## 🔨 Bad Design vs Refactored Design

### ❌ Bad Design (`PricingEngine.java`)
- All logic crammed in a **single method** `calc()`
- Uses `==` for String comparison (bug in Java)
- No separation between subtotal, discount, tax logic
- Hard to test individual parts
- Hard to extend or maintain

### ✅ Refactored Design (`PricingEngineRefactored.java`)

Each class has **one single responsibility**:

| Class | Responsibility |
|---|---|
| `Order` | Holds order data (model) |
| `PriceResult` | Holds calculation results |
| `SubtotalCalculator` | Calculates subtotal only |
| `DiscountCalculator` | Applies discounts only |
| `TaxCalculator` | Calculates tax only |
| `PricingEngineRefactored` | Orchestrates all calculators |
| `CustomerType` | Enum for customer types |

**Benefits of refactoring:**
- ✅ Easy to test each class independently
- ✅ Easy to add new discount codes or tax rates
- ✅ Readable and maintainable code
- ✅ Uses `enum` instead of raw Strings for customer type

---

## 🧪 Running the Tests

```bash
./gradlew test
```

Tests are located in `src/test/java/PricingEngineTest.java` and cover:

- ✅ Basic subtotal calculation (single and multiple items)
- ✅ VIP customer discount (10%)
- ✅ REGULAR customer (no discount)
- ✅ Discount codes: SAVE5, SAVE10, SAVE20
- ✅ Combined discounts (VIP + code)
- ✅ Edge case: discount never exceeds subtotal

---

## 🚀 How to Run

### Prerequisites
- Java 17+
- Gradle (or use the included `./gradlew` wrapper)

### Build the project
```bash
./gradlew build
```

### Run tests
```bash
./gradlew test
```

### Run manually (demo)
Open `PricingEngineRefactored.java` and run the `main()` method from IntelliJ.

---

## 📝 Git Workflow

Commits followed a logical progression:

```
1. Initial commit - Add README
2. Add bad design PricingEngine (single class)
3. Add JUnit tests for PricingEngine
4. Refactor: separate into SubtotalCalculator, DiscountCalculator, TaxCalculator
5. Refactor: add Order and PriceResult models
6. Refactor: use enum for CustomerType
7. Update README with project documentation
```

---

## 👤 Author

- **Student:** Abdelhakim Berrim
- **Module:** Outils Libres
- **Forked from:** [mbeggas/outils-libre-final-lab](https://github.com/mbeggas/outils-libre-final-lab)
