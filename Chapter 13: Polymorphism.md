# Chapter 13: Polymorphism

## 1. Concept Goal  
**What problem does this solve?**  
Without polymorphism, you’d need separate logic for every object type—even if they do the same thing.  
Polymorphism lets you **treat different objects uniformly** through a common interface, reducing code duplication and increasing flexibility.

---

## 2. Logical Explanation  
Polymorphism means:  
> “One interface, multiple implementations.”

If `Dog`, `Cat`, and `Bird` all extend `Animal`, you can write code that works with **any `Animal`**—without knowing or caring about the specific type.

Why it matters:  
- **Write generic logic**: “Feed all animals” instead of “feed dog, feed cat…”  
- **Easily add new types**: Add `Fish` → existing code still works  
- **Decouple code**: Your logic depends on **abstraction**, not concrete classes

Think:  
> “I don’t need to know *what* it is—only that it can `makeSound()`.”

---

## 3. Visual Representation  

```
         Animal
         ┌───────────────┐
         │ + makeSound() │
         └───────▲───────┘
                 │
   ┌─────────────┴─────────────┐
   │                           │
Dog.makeSound() → "Woof!"   Cat.makeSound() → "Meow!"
```

**Polymorphic Call**:  
```dart
void announce(Animal a) {
  a.makeSound(); // → calls the right version automatically
}
```

> The same line of code behaves differently based on the **actual object type**.

---

## 4. Dart Syntax  

```dart
class Animal {
  void makeSound() {
    print('Some generic sound');
  }
}

class Dog extends Animal {
  @override
  void makeSound() {
    print('Woof!');
  }
}

class Cat extends Animal {
  @override
  void makeSound() {
    print('Meow!');
  }
}

// Polymorphic usage
final animals = [Dog(), Cat(), Animal()];
for (final animal in animals) {
  animal.makeSound(); // calls the correct overridden method
}
```

> Dart uses **dynamic dispatch**: the method called is based on the **runtime type**, not the variable type.

---

## 5. Practical Examples  

### Example 1: UI Rendering  
```dart
abstract class Widget {
  void render();
}

class Button extends Widget {
  @override
  void render() => print('Rendering button');
}

class TextField extends Widget {
  @override
  void render() => print('Rendering text field');
}

// Generic renderer
void renderAll(List<Widget> widgets) {
  for (final w in widgets) w.render();
}
```

### Example 2: Payment Processors  
```dart
abstract class PaymentMethod {
  void process(double amount);
}

class CreditCard extends PaymentMethod {
  @override
  void process(double amount) => print('Paid \$amount via card');
}

class PayPal extends PaymentMethod {
  @override
  void process(double amount) => print('Paid \$amount via PayPal');
}

// Checkout logic works with ANY payment method
void checkout(PaymentMethod method, double total) {
  method.process(total);
}
```

> Add `CryptoWallet` tomorrow—checkout stays unchanged.

---

## 6. Problem-Solving Exercises  

**Easy**  
1. You have `Square` and `Triangle` extending `Shape` (from Chapter 12).  
   Both override `area()`. Write code that calculates total area of a `List<Shape>`.

**Medium**  
2. A `Logger` base class has `log(String message)`.  
   Create `FileLogger` (logs to file) and `ConsoleLogger` (prints).  
   Write a function that accepts any `Logger` and uses it.

**Advanced**  
3. In a game, `Enemy`, `Player`, and `NPC` all have `takeDamage(int)`.  
   But they react differently:  
   - `Player`: shows health bar  
   - `Enemy`: plays explosion  
   - `NPC`: runs away  
   How would you model this polymorphically?

---

## 7. Clean Solution & Explanation  

**Exercise 1**  
```dart
double totalArea(List<Shape> shapes) {
  return shapes.map((s) => s.area()).reduce((a, b) => a + b);
}
```
> No `if (shape is Square)`—just call `area()` on each.

**Exercise 2**  
```dart
void performAction(Logger logger) {
  logger.log('Action started');
  // ... do work ...
  logger.log('Action completed');
}

// Usage
performAction(ConsoleLogger());
performAction(FileLogger());
```
> Same logic, different logging strategies.

**Exercise 3**  
```dart
abstract class Character {
  void takeDamage(int points);
}

class Player extends Character {
  @override
  void takeDamage(int points) {
    updateHealthBar();
  }
}

class Enemy extends Character {
  @override
  void takeDamage(int points) {
    playExplosion();
  }
}

// Game engine
void hit(Character target) {
  target.takeDamage(10); // correct behavior auto-selected
}
```
> The game engine doesn’t care *who* is hit—only that they can `takeDamage()`.

---

## 8. Key Takeaways  
- Polymorphism = **one interface, many behaviors**  
- Use base class or abstract class as the **contract**  
- Override methods in subclasses to provide specific behavior  
- Write logic against the **abstraction**, not concrete types  
- Enables **extensibility**: new types plug in without changing existing code
