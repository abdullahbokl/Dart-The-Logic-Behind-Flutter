# Chapter 5: Functions & Reusability

![Cover - Chapter 5](Images/Covers/chapter-05-functions-reusability.jpg)

## 1. Concept Goal  
**What problem does this solve?**  
Without functions, you’d repeat the same code everywhere—making your app hard to read, test, and update.  
Functions **package logic into named, reusable units** so you write once, use anywhere.

---

## 2. Logical Explanation  
A function is a **mini-program** with a clear job:  
- It takes **inputs** (parameters)  
- It does **one thing well** (e.g., validate email, calculate tax)  
- It gives back an **output** (return value) or causes an **effect** (e.g., show dialog)

Why it matters:  
✅ **Avoid duplication**: Fix a bug in one place, not ten  
✅ **Improve readability**: `isValidEmail(input)` > 10 lines of regex inline  
✅ **Enable testing**: Test the function in isolation

Good functions are:  
- **Short** (usually <10 lines)  
- **Predictable** (same input → same output)  
- **Clearly named** (verbs: `calculateTotal`, `showError`)

---

## 3. Visual Representation  

```
[ Input ] ────→ [ Function: doSomething() ] ────→ [ Output / Side Effect ]
   (params)                                         (return value or action)
```

**Function as a Black Box**:
```
           +---------------------+
Email ────▶| validateEmail()     |────▶ true/false
           +---------------------+

Items ────▶| calculateTotal()    |────▶ 149.99
           +---------------------+
```

> You don’t need to know *how* it works—just what goes in and what comes out.

---

## 4. Dart Syntax  

```dart
// Full function
int add(int a, int b) {
  return a + b;
}

// Arrow syntax (for single-expression functions)
int add(int a, int b) => a + b;

// No return (void)
void log(String message) {
  print('[LOG] $message');
}

// Optional parameters
String greet(String name, {String title = 'Guest'}) {
  return 'Hello, $title $name!';
}

// Usage
print(add(2, 3)); // 5
log('User logged in');
print(greet('Ali', title: 'Dr.')); // Hello, Dr. Ali!
```

> Use arrow syntax **only** for simple, one-line logic. Clarity > brevity.

---

## 5. Practical Examples  

### Example 1: Form Validation Helper  
```dart
bool isValidEmail(String email) {
  return email.contains('@') && email.contains('.');
}
```

### Example 2: Flutter Callback  
```dart
// Passed to a widget
void onLoginPressed() {
  if (isValidEmail(emailController.text)) {
    login();
  } else {
    showError('Invalid email');
  }
}
```

> Functions let you **decouple logic from UI**—a core Flutter best practice.

---

## 6. Problem-Solving Exercises  

**Easy**  
1. Write a function `isEven(int number)` that returns `true` if the number is even.

**Medium**  
2. Create a function `formatPrice(double price)` that returns a string like `"$19.99"`.

**Advanced**  
3. Write a function `retry<T>(Future<T> Function() task, int maxRetries)`  
   that runs an async task and retries up to `maxRetries` times if it fails.  
   Return the result or throw last error.

---

## 7. Clean Solution & Explanation  

**Exercise 1**  
```dart
bool isEven(int number) => number % 2 == 0;
```
> Uses modulo operator `%`—clean and efficient.

**Exercise 2**  
```dart
String formatPrice(double price) => '\$${price.toStringAsFixed(2)}';
```
> `toStringAsFixed(2)` ensures exactly 2 decimal places—avoids `19.9` → `19.90`.

**Exercise 3**  
```dart
Future<T> retry<T>(Future<T> Function() task, int maxRetries) async {
  var lastError;
  for (int i = 0; i <= maxRetries; i++) {
    try {
      return await task();
    } catch (e) {
      lastError = e;
      if (i == maxRetries) break;
      await Future.delayed(Duration(seconds: 1));
    }
  }
  throw lastError!;
}
```
> Generic (`<T>`) → works with any return type.  
> Retries include initial attempt → total tries = `maxRetries + 1`.  
> Waits 1 second between retries (polite API usage).

---

## 8. Key Takeaways  
- Functions **eliminate repetition** and **clarify intent**.  
- Prefer **small, single-purpose** functions.  
- Use **arrow syntax** only for simple expressions.  
- Name functions with **verbs** (`calculate`, `validate`, `show`).  
- In Flutter, functions are often **callbacks**—keep them focused and testable.
