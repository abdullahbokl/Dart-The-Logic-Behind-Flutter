# Chapter 2: Variables & Data Types

## 1. Concept Goal  
**What problem does this solve?**  
Programs need to store and manage changing and unchanging values: user names, scores, settings, etc.  
Variables let you **name and reuse data**. Data types ensure your program **doesn’t mix apples with oranges**—like adding text to a number.

---

## 2. Logical Explanation  
Think of a variable as a **labeled box**:
- The **label** is the variable name (e.g., `userName`).
- The **contents** is the value (e.g., `"Ali"`).
- The **type** is what kind of thing the box can hold: text, number, true/false, etc.

Dart is **type-safe**: once you say a box holds only numbers, you can’t accidentally put text in it. This catches bugs early.

Dart gives you three ways to declare a box:
- `var`: “I’ll put something here now, and maybe change it later.”
- `final`: “I’ll set this once, and never change it.”
- `const`: “This value is known at compile time—it’s a true constant.”

> **Memory thinking**: `final` is set at runtime (e.g., user input). `const` is baked into the app before it runs (e.g., app version).

---

## 3. Visual Representation  

```
+------------------+     +------------------+     +------------------+
|      var         |     |      final       |     |      const       |
|  (mutable)       |     | (immutable,      |     | (compile-time    |
|                  |     |  runtime-known)  |     |  immutable)      |
+------------------+     +------------------+     +------------------+
| count = 5        |     | name = getUser() |     | pi = 3.14159     |
| count = 6  ✅    |     | name = "Ahmed" ❌|     | pi = 3.2 ❌      |
+------------------+     +------------------+     +------------------+
```

> Boxes with locks (`final`, `const`) prevent accidental changes—making your code safer.

---

## 4. Dart Syntax  

```dart
// var: type inferred, value can change
var score = 100;
score = 150; // OK

// final: set once (runtime)
final String email = 'user@example.com';
// email = 'new@example.com'; // ❌ Error

// const: compile-time constant
const double gravity = 9.8;
// gravity = 10.0; // ❌ Error

// Common primitive types:
int age = 25;
double price = 19.99;
String name = 'Dart';
bool isActive = true;
```

> Prefer `final` or `const` over `var` unless you truly need to change the value.

---

## 5. Practical Examples  

### Example 1: User Profile  
```dart
final String username = 'flutter_dev';
const int maxRetries = 3;
var loginAttempts = 0; // changes during app use
```

### Example 2: App Configuration  
```dart
const String appName = 'My Flutter App';
const double defaultPadding = 16.0;

// Loaded from API (runtime → final)
final DateTime userBirthday = fetchBirthdayFromServer();
```

> `const` for design tokens. `final` for user data. `var` only for state that changes.

---

## 6. Problem-Solving Exercises  

**Easy**  
1. Which keyword (`var`, `final`, `const`) should you use for:  
   a) The number of days in a week?  
   b) A user’s current location?  
   c) A counter that increases when a button is tapped?

**Medium**  
2. What’s wrong with this code?  
```dart
var name = 'Ali';
name = 42;
```

**Advanced**  
3. You’re building a theme system.  
   - Primary color never changes and is known at compile time.  
   - User’s preferred font size is loaded from settings (once, at startup).  
   - Current screen brightness changes during use.  
   Choose the right keyword for each.

---

## 7. Clean Solution & Explanation  

**Exercise 1**  
a) `const` (always 7)  
b) `final` (set once when fetched)  
c) `var` (changes repeatedly)  

> Why: Match the keyword to how the data behaves in reality.

**Exercise 2**  
Error: Dart infers `name` as `String`. You can’t assign an `int` (`42`) to it.  
Type safety prevents this bug before the app runs.

**Exercise 3**  
- Primary color → `const`  
- Font size → `final`  
- Brightness → `var`  

> Why: This reflects real-world mutability and improves performance (`const` values are reused).

---

## 8. Key Takeaways  
- Variables are **named containers for data**.  
- Use `const` for compile-time constants (design tokens, math constants).  
- Use `final` for runtime values you **won’t change** (user data, API responses).  
- Use `var` **only** when the value must change (UI state, counters).  
- Dart’s type system **protects you**—lean into it, don’t fight it.
