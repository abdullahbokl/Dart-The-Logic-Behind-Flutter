# Chapter 19: Immutability

![Cover - Chapter 19](../.gitbook/assets/chapter-19-immutability.jpg)

## 1. Concept Goal

**What problem does this solve?**\
Mutable objects can change unexpectedly—leading to bugs when shared across functions, widgets, or threads.\
Immutability ensures that **once an object is created, it never changes**, making code predictable, easier to test, and safe for state management.

***

## 2. Logical Explanation

An **immutable object** has all its fields set at creation and **never modified**.\
If you need a “changed” version, you **create a new object** with the updated values.

Why it matters:\
✅ **Predictability**: no side effects when passing objects around\
✅ **Easier debugging**: state snapshots are reliable\
✅ **Performance**: Flutter can safely skip widget rebuilds if objects are identical\
✅ **Thread safety**: critical for isolates and async code

In Dart, immutability is achieved by:

* Making all fields `final`
* Avoiding public setters
* Providing `copyWith()` for safe updates

> Immutable objects are **values**, not containers.

***

## 3. Visual Representation

```
Before (Mutable)               After (Immutable)
┌──────────────────┐           ┌──────────────────┐
│ User             │           │ User v1          │
│ name: "Ali"      │           │ name: "Ali"      │
└─────────▲────────┘           └──────────────────┘
          │                               │
          │ (change name)                 │ (copyWith)
          ▼                               ▼
┌──────────────────┐           ┌──────────────────┐
│ User             │           │ User v2          │
│ name: "Ahmed"    │           │ name: "Ahmed"    │
└──────────────────┘           └──────────────────┘
```

> Mutation changes the original → risk of unintended side effects\
> Immutability creates a new version → original stays intact

***

## 4. Dart Syntax

```dart
// Immutable class
class User {
  final String name;
  final int age;

  const User({required this.name, required this.age});

  // Create a new instance with changes
  User copyWith({String? name, int? age}) {
    return User(
      name: name ?? this.name,
      age: age ?? this.age,
    );
  }
}

// Usage
final user1 = const User(name: 'Ali', age: 28);
final user2 = user1.copyWith(name: 'Ahmed');

print(user1.name); // Ali (unchanged)
print(user2.name); // Ahmed (new object)
```

> * `final` fields prevent mutation
> * `const` constructor enables compile-time constants
> * `copyWith` enables functional updates

***

## 5. Practical Examples

### Example 1: Flutter State with Provider / Riverpod

```dart
@immutable
class AppState {
  final List<String> todos;
  final bool isLoading;

  const AppState({required this.todos, required this.isLoading});

  AppState copyWith({List<String>? todos, bool? isLoading}) {
    return AppState(
      todos: todos ?? this.todos,
      isLoading: isLoading ?? this.isLoading,
    );
  }
}
```

> State updates create new `AppState` → widgets rebuild only if needed.

### Example 2: Configuration Object

```dart
const ApiConfig config = ApiConfig(
  baseUrl: 'https://api.example.com',
  timeout: Duration(seconds: 10),
);

class ApiConfig {
  final String baseUrl;
  final Duration timeout;
  const ApiConfig({required this.baseUrl, required this.timeout});
}
```

> Safe to share across the app—never changes.

***

## 6. Problem-Solving Exercises

**Easy**

1. Make the following class immutable:

```dart
class Point {
  int x = 0;
  int y = 0;
}
```

**Medium**\
2\. Add a `copyWith` method to your `Point` class that allows updating `x` and/or `y`.

**Advanced**\
3\. You have a `ShoppingCart` with a `List<Product> items`.\
How do you make it immutable while allowing “add item” functionality?\
(Hint: `List` itself is mutable—how do you protect it?)

***

## 7. Clean Solution & Explanation

**Exercise 1**

```dart
class Point {
  final int x;
  final int y;
  const Point({this.x = 0, this.y = 0});
}
```

> All fields `final`, initialized in constructor.

**Exercise 2**

```dart
Point copyWith({int? x, int? y}) {
  return Point(
    x: x ?? this.x,
    y: y ?? this.y,
  );
}
```

> Returns new instance—original unchanged.

**Exercise 3**

```dart
class ShoppingCart {
  final List<Product> _items;

  // Defend against external mutation
  ShoppingCart({required List<Product> items})
      : _items = List.unmodifiable(items);

  List<Product> get items => _items;

  ShoppingCart addItem(Product product) {
    return ShoppingCart(
      items: List.unmodifiable([..._items, product]),
    );
  }
}
```

> Key techniques:
>
> * Store a **copy** of input list
> * Wrap in `List.unmodifiable`
> * `addItem` returns **new cart** with new list

> Without this, a caller could do: `cart.items.add(...)` and break immutability.

***

## 8. Key Takeaways

* **Immutable** = all fields `final`, no setters
* Use `copyWith()` to “update” by creating new instances
* Protect collections with `List.unmodifiable` or `final` + defensive copying
* Immutability enables **safe state management** in Flutter
* Prefer `const` constructors when possible for performance
