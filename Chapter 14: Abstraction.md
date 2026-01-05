# Chapter 14: Abstraction

![Cover - Chapter 14](Images/Covers/chapter-14-abstraction.jpg)

## 1. Concept Goal  
**What problem does this solve?**  
When every class exposes full implementation details, code becomes tightly coupled and fragile.  
Abstraction lets you **define *what* an object should do—without forcing *how* it does it**—enabling flexible, decoupled systems.

---

## 2. Logical Explanation  
Abstraction is about **hiding complexity behind a contract**.  
You define a set of capabilities (methods) that all conforming types must provide—but each type implements them in its own way.

In Dart, you achieve abstraction through:  
- **Abstract classes**: Partial implementation + required methods  
- **Implicit interfaces**: Every class defines an interface others can implement  

Why it matters:  
✅ **Decoupling**: Code depends on *roles*, not concrete types  
✅ **Testability**: Swap real objects with mocks easily  
✅ **Team scaling**: Frontend/backend can work against the same contract  

> Abstraction answers: “What can this thing do?”—not “How does it do it?”

---

## 3. Visual Representation  

```
        PaymentProcessor (Abstract Contract)
        ┌───────────────────────────────────┐
        │ + process(amount: double): void   │ ← Must be implemented
        └──────────────────▲────────────────┘
                           │
        ┌──────────────────┴────────────────┐
        │                                   │
  CreditCardProcessor                PayPalProcessor
  ┌────────────────────┐           ┌────────────────────┐
  │ process(amount) {  │           │ process(amount) {  │
  │   chargeCard(...)  │           │   callPayPal(...)  │
  │ }                  │           │ }                  │
  └────────────────────┘           └────────────────────┘
```

> Both fulfill the same **promise**—but with different **mechanisms**.

---

## 4. Dart Syntax  

```dart
// Abstract class (cannot be instantiated)
abstract class PaymentProcessor {
  void process(double amount); // no body → must be overridden
}

class CreditCardProcessor implements PaymentProcessor {
  @override
  void process(double amount) {
    print('Processing \$amount via credit card');
  }
}

// Every class defines an implicit interface
class MockProcessor implements PaymentProcessor {
  @override
  void process(double amount) {
    print('MOCK: would process \$amount');
  }
}
```

> - Use `abstract class` to define required methods  
> - Use `implements` to fulfill a contract  
> - Dart has **no explicit `interface` keyword**—classes *are* interfaces

---

## 5. Practical Examples  

### Example 1: Data Repository Pattern (Flutter)  
```dart
abstract class UserRepository {
  Future<User> getUser(String id);
  Future<void> saveUser(User user);
}

class RemoteUserRepository implements UserRepository {
  @override
  Future<User> getUser(String id) async {
    // Fetch from API
  }
}

class MockUserRepository implements UserRepository {
  @override
  Future<User> getUser(String id) async {
    // Return fake data for testing
  }
}
```

> Your UI code uses `UserRepository`—doesn’t care if data is real or fake.

### Example 2: Notification Service  
```dart
abstract class NotificationService {
  void send(String message);
}

class EmailService implements NotificationService {
  @override
  void send(String message) => print('Email: $message');
}

class PushService implements NotificationService {
  @override
  void send(String message) => print('Push: $message');
}
```

> Business logic calls `notificationService.send(...)`—delivery method is swappable.

---

## 6. Problem-Solving Exercises  

**Easy**  
1. Define an abstract class `Drawable` with a method `draw()`.  
   Create `Circle` and `Square` classes that implement it.

**Medium**  
2. You need to support multiple authentication methods:  
   - Email/password  
   - Google Sign-In  
   - Apple ID  
   Define an abstract `AuthProvider` and implement all three.

**Advanced**  
3. In a Flutter app, you want to load images from:  
   - Network  
   - Local assets  
   - Cached memory  
   Design an abstract `ImageLoader` interface and one concrete implementation.

---

## 7. Clean Solution & Explanation  

**Exercise 1**  
```dart
abstract class Drawable {
  void draw();
}

class Circle implements Drawable {
  @override
  void draw() => print('Drawing a circle');
}

class Square implements Drawable {
  @override
  void draw() => print('Drawing a square');
}
```
> Shared contract, independent implementations.

**Exercise 2**  
```dart
abstract class AuthProvider {
  Future<User> login();
}

class EmailAuthProvider implements AuthProvider {
  @override
  Future<User> login() async => // email flow
}

class GoogleAuthProvider implements AuthProvider {
  @override
  Future<User> login() async => // Google flow
}
```
> App uses `AuthProvider`—authentication strategy is configurable.

**Exercise 3**  
```dart
abstract class ImageLoader {
  Future<Image> load(String source);
}

class NetworkImageLoader implements ImageLoader {
  @override
  Future<Image> load(String url) async {
    // fetch from URL
  }
}

// Usage in widget
final loader = NetworkImageLoader();
final image = await loader.load('https://...');
```
> Image loading logic is abstracted—easy to test or replace.

---

## 8. Key Takeaways  
- **Abstraction = contract without implementation**  
- Use `abstract class` to define required behavior  
- Use `implements` to fulfill that contract  
- Dart treats **every class as an interface**  
- Abstraction enables **loose coupling**, **testability**, and **flexible architecture**
