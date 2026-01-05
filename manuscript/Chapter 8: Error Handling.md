# Chapter 8: Error Handling

![Cover - Chapter 8](resources/images/Covers/chapter-08-error-handling.jpg)

## 1. Concept Goal  
**What problem does this solve?**  
Things go wrong: network fails, files disappear, users enter invalid data.  
Without structured error handling, your app crashes or behaves unpredictably.  
Dart’s `try`/`catch` lets you **anticipate, contain, and respond to failures gracefully**.

---

## 2. Logical Explanation  
Errors fall into two categories:  
- **Expected errors**: Network timeout, invalid input—things you can plan for.  
- **Unexpected errors**: Bugs, logic mistakes—should be fixed, not caught.

Dart encourages you to **handle expected errors**, not hide all exceptions.  
Good error handling:  
✅ Prevents crashes  
✅ Gives users clear feedback  
✅ Keeps the app in a known state  

The mindset:  
> “If this step can fail, decide **how** it should fail—and what to do next.”

---

## 3. Visual Representation  

```
Start
  │
  ▼
[ try { riskyOperation() } ]
          │
  ┌───────┴────────┐
  │ Success        │ Exception thrown
  ▼                ▼
Continue       [ catch (e) { handle(e) } ]
  │                │
  └───────┬────────┘
          ▼
        Resume safely
```

**Error Flow vs Crash**:  
- **Without handling**: App stops → bad user experience  
- **With handling**: Show message, retry, or fallback → app stays alive

---

## 4. Dart Syntax  

```dart
try {
  final data = await fetchUserData();
  updateUserUI(data);
} on NetworkError catch (e) {
  showNetworkError(e.message);
} on FormatException {
  log('Data format invalid');
} catch (e) {
  // Fallback for any other error
  showError('Unexpected issue');
} finally {
  hideLoadingIndicator(); // always runs
}
```

> - Use `on SpecificError` to handle known cases  
> - Use `catch (e)` for unknowns (log them!)  
> - `finally` runs regardless—ideal for cleanup

---

## 5. Practical Examples  

### Example 1: Safe File Reading  
```dart
try {
  final content = await File('config.txt').readAsString();
  parseConfig(content);
} on FileSystemException {
  useDefaultConfig();
}
```

### Example 2: API Call in Flutter  
```dart
Future<void> loadProfile() async {
  setState(() => isLoading = true);
  try {
    final user = await api.getUser();
    setState(() => profile = user);
  } catch (e) {
    ScaffoldMessenger.of(context)
      .showSnackBar(SnackBar(content: Text('Failed to load profile')));
  } finally {
    setState(() => isLoading = false);
  }
}
```

> Errors become **user-facing messages**, not crashes.

---

## 6. Problem-Solving Exercises  

**Easy**  
1. Wrap `int.parse('abc')` in a `try`/`catch` that prints “Invalid number”.

**Medium**  
2. Create a `CustomException` called `ValidationError` with a `message` field.  
   Throw it if a username is shorter than 3 characters.

**Advanced**  
3. Write a function `safeDivide(double a, double b)` that:  
   - Returns `a / b` if `b != 0`  
   - Throws a `DivisionByZeroError` (custom) if `b == 0`  
   - Is called inside a `try`/`catch` that prints the error message

---

## 7. Clean Solution & Explanation  

**Exercise 1**  
```dart
try {
  int.parse('abc');
} catch (e) {
  print('Invalid number');
}
```
> `int.parse` throws `FormatException` on bad input—caught safely.

**Exercise 2**  
```dart
class ValidationError implements Exception {
  final String message;
  ValidationError(this.message);
}

void validateUsername(String name) {
  if (name.length < 3) {
    throw ValidationError('Username must be at least 3 characters');
  }
}
```
> Custom exceptions carry **meaningful context**.

**Exercise 3**  
```dart
class DivisionByZeroError implements Exception {
  @override
  String toString() => 'Cannot divide by zero';
}

double safeDivide(double a, double b) {
  if (b == 0) throw DivisionByZeroError();
  return a / b;
}

// Usage
try {
  print(safeDivide(10, 0));
} catch (e) {
  print(e.toString()); // "Cannot divide by zero"
}
```
> Separation of concern:  
> - `safeDivide` enforces rule  
> - Caller decides how to respond

---

## 8. Key Takeaways  
- Use `try`/`catch` for **expected, recoverable errors** (network, input, files).  
- Create **custom exceptions** for domain-specific failures.  
- Always include a `finally` block for cleanup (hide loader, close stream).  
- Never ignore errors—**log or show them**.  
- Let **unexpected errors crash** during development—they reveal bugs to fix.
