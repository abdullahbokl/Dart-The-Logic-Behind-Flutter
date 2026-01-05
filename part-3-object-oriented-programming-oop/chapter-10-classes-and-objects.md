# Chapter 10: Classes & Objects

![Cover - Chapter 10](../.gitbook/assets/chapter-10-classes-objects.jpg)

## 1. Concept Goal

**What problem does this solve?**\
Without a clear structure, object creation is messy and error-prone.\
Classes provide a **reliable blueprint** to create objects with consistent data and behavior—ensuring every `User`, `Product`, or `Button` behaves as expected.

***

## 2. Logical Explanation

A **class** defines _what an object will be_.\
An **object** is a _real instance_ of that class, living in memory.

Key parts of a class:

* **Fields**: Store the object’s state (`name`, `price`, `isActive`)
* **Constructors**: Initialize the object when created
* **Methods**: Define what the object can do (`save()`, `calculateTotal()`)

Dart makes object creation predictable:

* Use `required` for fields that **must** be provided
* Use default values for optional ones
* Initialize everything **at creation time** → no half-baked objects

> A well-designed class ensures every object starts in a **valid state**.

***

## 3. Visual Representation

```
        Class (Blueprint)
┌───────────────────────────────┐
│  User                         │
│  --------------------------   │
│  Fields:                      │
│    - String name              │
│    - String email             │
│                               │
│  Constructor:                 │
│    User(name, email)          │
│                               │
│  Methods:                     │
│    + validate() → bool        │
└───────────────────────────────┘
               │
               ▼ (instantiates)
        Object (Instance)
┌───────────────────────────────┐
│  User instance                │
│  name: "Ali"                  │
│  email: "ali@example.com"     │
│  validate() → true            │
└───────────────────────────────┘
```

> One class → many independent objects.

***

## 4. Dart Syntax

```dart
class Product {
  // Fields
  final String name;
  final double price;
  bool _inStock = true; // private field (starts with _)

  // Constructor (with required named parameters)
  Product({
    required this.name,
    required this.price,
  });

  // Method
  bool get isInStock => _inStock;

  void markOutOfStock() {
    _inStock = false;
  }
}

// Create object
final laptop = Product(name: 'Laptop', price: 999.99);
print(laptop.isInStock); // true
laptop.markOutOfStock();
```

> * `final` fields must be set in constructor
> * Named parameters (`{...}`) improve readability
> * Private members start with `_`

***

## 5. Practical Examples

### Example 1: Immutable Data Model (Common in Flutter)

```dart
class Todo {
  final String title;
  final bool completed;

  Todo({required this.title, this.completed = false});

  // Create a copy with changes (used in state management)
  Todo copyWith({String? title, bool? completed}) {
    return Todo(
      title: title ?? this.title,
      completed: completed ?? this.completed,
    );
  }
}
```

### Example 2: Configurable Widget Logic

```dart
class ApiClient {
  final String baseUrl;
  final Duration timeout;

  ApiClient({
    required this.baseUrl,
    this.timeout = const Duration(seconds: 10),
  });

  Future<String> get(String path) async {
    // ... fetch from $baseUrl$path with timeout
  }
}
```

> Objects encapsulate configuration and behavior together.

***

## 6. Problem-Solving Exercises

**Easy**

1. Create a `Circle` class with a `radius` field. Add a method `area()` that returns `π × r²`.

**Medium**\
2\. A `Counter` object starts at 0. It has `increment()`, `decrement()`, and `reset()`.\
Implement it with a private `_count` field and a public getter.

**Advanced**\
3\. Design a `UserProfile` class that:

* Takes `firstName` and `lastName` (required)
* Has a computed `fullName`
* Is **immutable** (all fields `final`)
* Can create a copy with updated first/last name (`copyWith`)

***

## 7. Clean Solution & Explanation

**Exercise 1**

```dart
class Circle {
  final double radius;
  Circle(this.radius);
  double area() => 3.14159 * radius * radius;
}
```

> Simple, focused. No mutability needed.

**Exercise 2**

```dart
class Counter {
  int _count = 0;
  int get value => _count;
  void increment() => _count++;
  void decrement() => _count--;
  void reset() => _count = 0;
}
```

> Private field protects internal state. Public getter exposes read-only value.

**Exercise 3**

```dart
class UserProfile {
  final String firstName;
  final String lastName;
  UserProfile({required this.firstName, required this.lastName});
  String get fullName => '$firstName $lastName';
  UserProfile copyWith({String? firstName, String? lastName}) {
    return UserProfile(
      firstName: firstName ?? this.firstName,
      lastName: lastName ?? this.lastName,
    );
  }
}
```

> Immutable → safe for state management. `copyWith` enables functional updates (used in `Provider`, `Riverpod`, etc.).

***

## 8. Key Takeaways

* A **class** is a template; an **object** is a live instance.
* Use **named constructors** with `required` for clarity and safety.
* Make fields `final` when possible—favor immutability.
* Hide internal state with **private fields** (`_name`).
* Provide **public interfaces** (getters/methods) to interact safely.
