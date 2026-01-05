# Chapter 20: Asynchronous Programming

![Cover - Chapter 20](Images/Covers/chapter-20-asynchronous-programming.jpg)

## 1. Concept Goal  
**What problem does this solve?**  
Apps need to wait for slow operations (network, file I/O, animations) without freezing the UI.  
Asynchronous programming lets your app **stay responsive while waiting**—by running tasks in the background and continuing when they complete.

---

## 2. Logical Explanation  
Dart is **single-threaded** but **non-blocking**.  
Instead of halting everything for a network call, Dart:  
1. Starts the task  
2. Returns immediately to handle UI/input  
3. **Resumes your code later** when the result is ready  

The core idea:  
> “Do this **eventually**—don’t wait for it now.”

Key concepts:  
- **`Future`**: A placeholder for a value that will arrive later  
- **`async`/`await`**: Write async code that looks synchronous  
- **Error handling**: Use `try`/`catch`—just like sync code

> Async isn’t about speed—it’s about **responsiveness**.

---

## 3. Visual Representation  

```
Main Thread Timeline:
│
├─ Start network request
│
├─ Continue: update UI, handle taps
│
├─ ... (user interacts freely) ...
│
└─ ← Future completes → Resume async function
```

**Future States**:  
```
Future<T>
├── Uncompleted (pending)
├── Completed with value (T)
└── Completed with error (Exception)
```

> Your code only runs after the Future resolves.

---

## 4. Dart Syntax  

```dart
// Declare async function
Future<String> fetchUserName() async {
  // Simulate network delay
  await Future.delayed(Duration(seconds: 2));
  return 'Ali';
}

// Use with await (inside async function)
void loadProfile() async {
  try {
    final name = await fetchUserName();
    print('Hello, $name!');
  } catch (e) {
    print('Failed: $e');
  }
}

// Or use .then()/.catchError()
fetchUserName().then((name) {
  print('Hello, $name!');
}).catchError((e) {
  print('Error: $e');
});
```

> - Mark function with `async` to use `await`  
> - `await` pauses **only that function**, not the whole app  
> - Always handle errors (networks fail!)

---

## 5. Practical Examples  

### Example 1: Flutter API Call  
```dart
Future<User> fetchUser(int id) async {
  final response = await http.get(Uri.parse('/users/$id'));
  if (response.statusCode == 200) {
    return User.fromJson(json.decode(response.body));
  } else {
    throw Exception('Failed to load user');
  }
}

// In widget
void _loadUser() async {
  setState(() => isLoading = true);
  try {
    final user = await fetchUser(1);
    setState(() => _user = user);
  } finally {
    setState(() => isLoading = false);
  }
}
```

### Example 2: Sequential vs Parallel  
```dart
// Sequential (slow)
final user = await fetchUser(1);
final posts = await fetchPosts(1);

// Parallel (fast)
final results = await Future.wait([
  fetchUser(1),
  fetchPosts(1),
]);
```

> Use `Future.wait` to run async tasks concurrently.

---

## 6. Problem-Solving Exercises  

**Easy**  
1. Write an async function `delayedGreeting(String name)` that waits 1 second, then returns `"Hello, $name!"`.

**Medium**  
2. You have `Future<int> fetchA()` and `Future<int> fetchB()`.  
   Write a function that returns the **sum** of both results, running them in parallel.

**Advanced**  
3. Implement a `retry` function that:  
   - Takes an async function `Future<T> task()`  
   - Runs it up to 3 times if it throws  
   - Waits 500ms between retries  
   - Throws last error if all fail

---

## 7. Clean Solution & Explanation  

**Exercise 1**  
```dart
Future<String> delayedGreeting(String name) async {
  await Future.delayed(Duration(seconds: 1));
  return 'Hello, $name!';
}
```
> Simple async delay with return.

**Exercise 2**  
```dart
Future<int> fetchSum() async {
  final values = await Future.wait([fetchA(), fetchB()]);
  return values[0] + values[1];
}
```
> `Future.wait` runs tasks in parallel → faster total time.

**Exercise 3**  
```dart
Future<T> retry<T>(Future<T> Function() task) async {
  Exception? lastError;
  for (int i = 0; i < 3; i++) {
    try {
      return await task();
    } catch (e) {
      lastError = e;
      if (i < 2) await Future.delayed(Duration(milliseconds: 500));
    }
  }
  throw lastError!;
}
```
> Generic retry logic—reusable across the app.  
> Waits only between retries (not after last failure).

---

## 8. Key Takeaways  
- Use `async`/`await` to write clean async code  
- **Never block the main thread**—use `Future` for I/O  
- Handle errors with `try`/`catch` (not just `.catchError`)  
- Run independent tasks in **parallel** with `Future.wait`  
- Async code should **never** assume order—always handle timing safely
