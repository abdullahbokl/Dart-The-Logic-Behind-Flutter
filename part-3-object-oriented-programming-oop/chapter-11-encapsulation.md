# Chapter 11: Encapsulation

![Cover - Chapter 11](../.gitbook/assets/chapter-11-encapsulation.jpg)

## 1. Concept Goal

**What problem does this solve?**\
Without boundaries, any part of your code can change an object’s internal data—leading to bugs, inconsistent states, and fragile logic.\
Encapsulation **protects an object’s internal state** and exposes only what’s necessary through a clear, controlled interface.

***

## 2. Logical Explanation

Encapsulation means:

> “Bundle data with the methods that operate on it, and **hide the internal details** from the outside world.”

Why it matters:

* **Prevents invalid state**: e.g., a `BankAccount` balance shouldn’t go negative unexpectedly
* **Reduces coupling**: Other code depends on _what_ your object does, not _how_ it does it
* **Makes refactoring safe**: Change internal logic without breaking callers

The rule:

> **Public interface = promises. Private implementation = your secret.**

***

## 3. Visual Representation

```
          +---------------------------+
          |      BankAccount          |
          |---------------------------|
          | - _balance: double = 0    | ← Hidden (private)
          |---------------------------|
          | + deposit(amount)         | ← Public interface
          | + withdraw(amount): bool  |
          | + get balance             |
          +---------------------------+
                    ↑
    External code can ONLY interact
    through public methods/getters
```

**Without encapsulation**:

```dart
account._balance = -1000; // 💥 Disaster!
```

**With encapsulation**:

```dart
account.withdraw(1000); // → safely rejected if insufficient funds
```

> Control the gate—never hand out the keys.

***

## 4. Dart Syntax

```dart
class BankAccount {
  // Private field (starts with _)
  double _balance = 0;

  // Public read-only access
  double get balance => _balance;

  // Controlled mutation
  void deposit(double amount) {
    if (amount > 0) _balance += amount;
  }

  bool withdraw(double amount) {
    if (amount > 0 && amount <= _balance) {
      _balance -= amount;
      return true;
    }
    return false;
  }
}
```

> * Prefix with `_` to make members **library-private**
> * Use **getters** for read access
> * Use **methods** for safe mutation

***

## 5. Practical Examples

### Example 1: Email Validator

```dart
class User {
  String _email = '';

  set email(String value) {
    if (!value.contains('@')) {
      throw ArgumentError('Invalid email');
    }
    _email = value;
  }

  String get email => _email;
}
```

> Setter validates input—email can never be invalid.

### Example 2: Read-Only ID

```dart
class Product {
  final String _id = DateTime.now().millisecondsSinceEpoch.toString();
  String get id => _id; // public read, no write
}
```

> ID is set once, exposed safely—no external tampering.

***

## 6. Problem-Solving Exercises

**Easy**

1. A `Temperature` class stores Celsius internally.\
   Add a getter `fahrenheit` that returns the converted value.\
   Make the internal field private.

**Medium**\
2\. A `Password` class should only accept strings with length ≥ 8.\
Use a setter to enforce this rule. What happens if an invalid password is assigned?

**Advanced**\
3\. Design a `Counter` that:

* Starts at 0
* Can only be incremented (no decrement/reset)
* Exposes only the current value (no direct access to internal count)

***

## 7. Clean Solution & Explanation

**Exercise 1**

```dart
class Temperature {
  final double _celsius;
  Temperature(this._celsius);
  double get fahrenheit => (_celsius * 9 / 5) + 32;
}
```

> Internal state hidden. Conversion is computed on demand.

**Exercise 2**

```dart
class Password {
  String _value = '';

  set value(String input) {
    if (input.length < 8) {
      throw ArgumentError('Password too short');
    }
    _value = input;
  }

  String get value => _value;
}
```

> Assignment fails early with clear error—no invalid state stored.

**Exercise 3**

```dart
class Counter {
  int _count = 0;
  int get value => _count;
  void increment() => _count++;
}
```

> Minimal public interface. Internal state fully protected.

> ✅ External code can’t reset or peek at `_count`\
> ✅ Only allowed action: `increment()`

***

## 8. Key Takeaways

* **Hide internal state** with private fields (`_name`)
* **Expose controlled access** via getters and methods
* **Validate in setters** to reject invalid data early
* **Never expose mutable internals**—return copies if needed
* Encapsulation = **trustworthy objects** that enforce their own rules
