# Chapter 12: Inheritance

![Cover - Chapter 12](resources/images/Covers/chapter-12-inheritance.jpg)

## 1. Concept Goal  
**What problem does this solve?**  
When multiple classes share common behavior (e.g., `Dog` and `Cat` both `eat()` and `sleep()`), copying code leads to duplication and maintenance nightmares.  
Inheritance lets you **define shared logic once** and **reuse it across related classes**.

---

## 2. Logical Explanation  
Inheritance models an **“is-a” relationship**:  
- A `Dog` **is a** `Animal`  
- A `SavingsAccount` **is a** `BankAccount`

You create a **base class** (e.g., `Animal`) with common fields and methods.  
Then, **subclasses** (e.g., `Dog`, `Cat`) **inherit** that logic and add their own specifics.

Benefits:  
✅ Avoid code duplication  
✅ Centralize shared behavior  
✅ Enable polymorphism (covered next)

But beware:  
⚠️ Don’t inherit just to reuse code—only when the **“is-a” relationship is true**  
⚠️ Deep inheritance trees become hard to manage

> Inheritance = **specialization**, not just convenience.

---

## 3. Visual Representation  

```
        Animal (Base Class)
        ┌───────────────────┐
        │ - name: String    │
        │ + eat()           │
        │ + sleep()         │
        └─────────▲─────────┘
                  │
      ┌───────────┴───────────┐
      │                       │
  Dog (Subclass)        Cat (Subclass)
  ┌──────────────┐      ┌──────────────┐
  │ + bark()     │      │ + meow()     │
  └──────────────┘      └──────────────┘
```

> Both `Dog` and `Cat` automatically get `name`, `eat()`, and `sleep()`—no copying needed.

---

## 4. Dart Syntax  

```dart
// Base class
class Animal {
  String name;
  Animal(this.name);

  void eat() => print('$name is eating');
  void sleep() => print('$name is sleeping');
}

// Subclass
class Dog extends Animal {
  Dog(String name) : super(name); // call parent constructor

  void bark() => print('$name says woof!');
}

// Usage
final dog = Dog('Buddy');
dog.eat();   // inherited
dog.bark();  // own method
```

> - Use `extends` to inherit  
> - Use `super()` to call parent constructor  
> - Subclasses **automatically** get all parent members

---

## 5. Practical Examples  

### Example 1: UI Widgets (Conceptual)  
```dart
class Button {
  String label;
  Button(this.label);
  void render() => print('Rendering button: $label');
}

class PrimaryButton extends Button {
  PrimaryButton(String label) : super(label);
  @override
  void render() {
    print('🎨 Styling as primary...');
    super.render();
  }
}
```

### Example 2: API Response Types  
```dart
class ApiResponse {
  final bool success;
  ApiResponse(this.success);
}

class UserResponse extends ApiResponse {
  final User user;
  UserResponse(this.user) : super(true);
}
```

> Shared structure (`success`) + specific data (`user`)

---

## 6. Problem-Solving Exercises  

**Easy**  
1. Create a `Vehicle` class with a `brand` field and a `start()` method.  
   Then create a `Car` subclass that adds a `honk()` method.

**Medium**  
2. A `Shape` base class has a method `area()` that throws `UnimplementedError`.  
   Create `Circle` and `Rectangle` subclasses that override `area()` with correct formulas.

**Advanced**  
3. You have `Animal`, `Dog`, and `Puppy`.  
   - `Animal` has `name`  
   - `Dog` adds `breed`  
   - `Puppy` adds `age` (in months)  
   Write the class hierarchy with proper constructors.

---

## 7. Clean Solution & Explanation  

**Exercise 1**  
```dart
class Vehicle {
  String brand;
  Vehicle(this.brand);
  void start() => print('$brand engine started');
}

class Car extends Vehicle {
  Car(String brand) : super(brand);
  void honk() => print('$brand says beep!');
}
```
> `Car` reuses `brand` and `start()`, adds only what’s unique.

**Exercise 2**  
```dart
abstract class Shape {
  double area(); // no implementation
}

class Circle extends Shape {
  final double radius;
  Circle(this.radius);
  @override
  double area() => 3.14159 * radius * radius;
}

class Rectangle extends Shape {
  final double width, height;
  Rectangle(this.width, this.height);
  @override
  double area() => width * height;
}
```
> Base defines contract; subclasses fulfill it.

**Exercise 3**  
```dart
class Animal {
  String name;
  Animal(this.name);
}

class Dog extends Animal {
  String breed;
  Dog(String name, this.breed) : super(name);
}

class Puppy extends Dog {
  int age;
  Puppy(String name, String breed, this.age) : super(name, breed);
}
```
> Each level adds its own data, delegates to parent.

> ✅ Chain of `super()` calls ensures full initialization  
> ✅ `Puppy` “is a” `Dog`, which “is a” `Animal` → valid hierarchy

---

## 8. Key Takeaways  
- Use inheritance for **true “is-a” relationships**  
- Subclasses inherit **all** fields and methods from parent  
- Call `super()` in constructor to initialize parent  
- Override methods with `@override` for clarity and safety  
- Avoid deep hierarchies—favor composition when appropriate (see Chapter 15)
