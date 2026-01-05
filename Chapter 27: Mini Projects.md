# Chapter 27: Mini Projects

## 1. Concept Goal  
**What problem does this solve?**  
Understanding concepts in isolation isn’t enough. You need to **integrate logic, data, and Dart features** into working systems.  
These mini-projects simulate real Flutter-relevant tasks—without requiring UI—so you focus on **core Dart logic and structure**.

---

## 2. Logical Explanation  
Each project combines multiple concepts:  
- **State management** (immutable objects, updates)  
- **Control flow & loops** (validation, iteration)  
- **Error handling** (safe operations)  
- **Clean code** (separation of concerns)  

You’ll build:  
1. A **counter** with history and limits  
2. A **shopping cart** with pricing and validation  
3. A **form validation engine** with dynamic rules  

> No widgets. Just pure Dart logic—ready to plug into any Flutter app.

---

## 3. Visual Representation  

**Shopping Cart Flow**  
```
Add Item → Validate Stock → Update Total → Apply Discount? → Finalize
     ↑          ↓                ↓                ↓
  UI Call   Error Handling   Immutable Cart   Business Rule
```

> Each step is a pure Dart function or method.

---

## 4. Dart Syntax Patterns Used  

```dart
// Immutability
Cart copyWith({List<Item>? items});

// Validation
Result<Cart, String> addItem(Item item);

// Business logic
double applyDiscount(double total, User user);
```

> Structured, testable, and framework-agnostic.

---

## 5. Practical Examples  

### Project 1: Smart Counter  
```dart
class Counter {
  final int value;
  final List<int> history;
  final int max;

  Counter({this.value = 0, this.history = const [], this.max = 100});

  Result<Counter, String> increment() {
    if (value >= max) {
      return Result.error('Max limit reached');
    }
    return Result.success(
      Counter(
        value: value + 1,
        history: [...history, value + 1],
        max: max,
      ),
    );
  }

  int get peak => history.isEmpty ? value : history.reduce(math.max);
}
```

### Project 2: Shopping Cart  
```dart
class Cart {
  final List<CartItem> items;

  Cart({required this.items});

  Cart addItem(Product product) {
    // Add or increment existing
    final existingIndex = items.indexWhere((i) => i.product.id == product.id);
    if (existingIndex != -1) {
      final updated = List<CartItem>.from(items);
      updated[existingIndex] = updated[existingIndex].copyWith(
        quantity: updated[existingIndex].quantity + 1,
      );
      return Cart(items: updated);
    } else {
      return Cart(items: [...items, CartItem(product)]);
    }
  }

  double get total => items.fold(0, (sum, item) => sum + item.totalPrice);
}
```

---

## 6. Problem-Solving Exercises  

**Project 1: Counter with History**  
Build a `Counter` that:  
- Starts at 0  
- Has a max value (default: 10)  
- Tracks all past values in a history list  
- Prevents increment if at max  
- Provides `undo()` to revert to previous state  

**Project 2: Shopping Cart System**  
Build a cart that:  
- Holds `CartItem` (product + quantity)  
- Prevents adding out-of-stock items  
- Calculates total with tax (8.25%)  
- Applies 10% discount if user is premium  
- Supports `removeItem(productId)`  

**Project 3: Validation Engine**  
Create a `Validator` that:  
- Takes a `Map<String, dynamic>` of form data  
- Accepts a list of `Rule` objects (e.g., `Required('email')`, `MinLength('password', 8)`)  
- Returns `ValidationResult` with `isValid` and `errors: Map<String, String>`  

---

## 7. Clean Solution Highlights  

### Project 1: Counter (Key Parts)  
```dart
class Counter {
  final int value;
  final List<int> history;
  final int max;

  Counter._({required this.value, required this.history, this.max = 10});

  factory Counter({int max = 10}) => Counter._(value: 0, history: [], max: max);

  Result<Counter, String> increment() {
    if (value >= max) return Result.error('Max reached');
    return Result.success(Counter._(
      value: value + 1,
      history: [...history, value],
      max: max,
    ));
  }

  Counter? undo() {
    if (history.isEmpty) return null;
    final prev = history.last;
    return Counter._(
      value: prev,
      history: history.sublist(0, history.length - 1),
      max: max,
    );
  }
}
```

### Project 3: Validation Engine (Skeleton)  
```dart
abstract class Rule {
  String field;
  Rule(this.field);
  String? validate(dynamic value);
}

class RequiredRule extends Rule {
  RequiredRule(super.field);
  @override
  String? validate(dynamic value) {
    return (value == null || value.toString().isEmpty)
        ? '$field is required'
        : null;
  }
}

class Validator {
  final List<Rule> rules;
  Validator(this.rules);

  ValidationResult validate(Map<String, dynamic> data) {
    final errors = <String, String>{};
    for (final rule in rules) {
      final value = data[rule.field];
      final error = rule.validate(value);
      if (error != null) errors[rule.field] = error;
    }
    return ValidationResult(errors: errors);
  }
}

class ValidationResult {
  final Map<String, String> errors;
  ValidationResult({required this.errors});
  bool get isValid => errors.isEmpty;
}
```

> Rules are **composable**—easy to extend.

---

## 8. Key Takeaways  
- **Mini-projects integrate multiple Dart concepts**  
- **Immutability** enables safe state updates  
- **Validation** should be decoupled and rule-based  
- **Business logic** belongs in pure Dart classes—not widgets  
- These systems plug directly into Flutter state management (Provider, Riverpod, etc.)  
- Build logic first—UI is just a skin on top
