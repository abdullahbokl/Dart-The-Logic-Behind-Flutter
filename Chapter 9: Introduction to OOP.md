# Chapter 9: Introduction to OOP

## 1. Concept Goal  
**What problem does this solve?**  
As apps grow, managing data and behavior with loose variables and functions becomes chaotic.  
Object-Oriented Programming (OOP) **bundles data and logic together into cohesive units** that model real-world concepts—making code organized, reusable, and easier to reason about.

---

## 2. Logical Explanation  
OOP is based on one core idea: **everything is an object**.  
An object is a **self-contained unit** that holds:
- **Data** (its state): e.g., a `Car` has `color`, `speed`
- **Behavior** (its methods): e.g., `car.accelerate()`, `car.brake()`

Why this works:  
- **Real-world alignment**: A “user” in your app behaves like a real user.  
- **Encapsulation**: Internal details are hidden; you interact via clear interfaces.  
- **Modularity**: Change one object without breaking others.

OOP isn’t about fancy theory—it’s about **reducing mental load** by grouping what belongs together.

> Think: “What does this thing *have*? What can it *do*?”

---

## 3. Visual Representation  

```
          +---------------------+
          |      Car Object     |
          +---------------------+
          | - color: String     |
          | - speed: int        |
          +---------------------+
          | + accelerate()      |
          | + brake()           |
          | + currentSpeed()    |
          +---------------------+
```

**Before OOP (chaotic)**:  
```dart
String carColor = 'red';
int carSpeed = 0;
void accelerate() { carSpeed += 10; } // Which car?
```

**After OOP (organized)**:  
```dart
Car myCar = Car(color: 'red');
myCar.accelerate(); // Clearly acts on myCar
```

> Objects give **context** to data and actions.

---

## 4. Dart Syntax  

```dart
// Define a class (blueprint)
class Car {
  String color;
  int speed = 0;

  Car({required this.color});

  void accelerate() {
    speed += 10;
  }

  int currentSpeed() => speed;
}

// Create an object (instance)
final myCar = Car(color: 'blue');
myCar.accelerate();
print(myCar.currentSpeed()); // 10
```

> A **class** is the template. An **object** is a real thing built from it.

---

## 5. Practical Examples  

### Example 1: User Model in Flutter  
```dart
class User {
  final String name;
  final String email;

  User({required this.name, required this.email});

  bool get isValid => email.contains('@');
}
```

### Example 2: Shopping Cart Item  
```dart
class CartItem {
  final Product product;
  int quantity = 1;

  CartItem(this.product);

  double get totalPrice => product.price * quantity;

  void increment() => quantity++;
}
```

> Each object **owns its state and logic**—no global variables needed.

---

## 6. Problem-Solving Exercises  

**Easy**  
1. A `Book` has a `title` and `author`. It can `describe()` itself (return “*title* by *author*”).  
   Write the class.

**Medium**  
2. A `BankAccount` has a `balance`. It supports `deposit(amount)` and `withdraw(amount)`.  
   Withdrawal should fail if balance is too low. How would you model this?

**Advanced**  
3. In a game, a `Player` has `health` (starts at 100).  
   - `takeDamage(int points)` reduces health  
   - If health ≤ 0, player is dead  
   Should `isAlive` be a field or a method? Why?

---

## 7. Clean Solution & Explanation  

**Exercise 1**  
```dart
class Book {
  final String title;
  final String author;
  Book(this.title, this.author);
  String describe() => '$title by $author';
}
```
> Simple, focused, immutable data.

**Exercise 2**  
```dart
class BankAccount {
  double balance = 0;

  void deposit(double amount) {
    if (amount > 0) balance += amount;
  }

  bool withdraw(double amount) {
    if (amount > 0 && amount <= balance) {
      balance -= amount;
      return true; // success
    }
    return false; // insufficient funds
  }
}
```
> Returns `bool` so caller knows if withdrawal worked—no silent failure.

**Exercise 3**  
```dart
class Player {
  int health = 100;

  void takeDamage(int points) {
    health -= points;
  }

  bool get isAlive => health > 0; // computed, not stored
}
```
> `isAlive` is a **computed property** (getter), not a field—always up-to-date, no sync bugs.

---

## 8. Key Takeaways  
- OOP **groups related data and behavior** into objects.  
- Objects model real-world entities (User, Car, CartItem).  
- A **class** is a blueprint; an **object** is an instance.  
- Methods operate on the object’s own data—no external dependencies needed.  
- Start simple: identify “things” in your app, then define what they *have* and *do*.
