# Chapter 22: Isolates (Conceptual)

## 1. Concept Goal  
**What problem does this solve?**  
Dart runs on a single thread—so heavy computations (e.g., image processing, large JSON parsing) can **freeze the UI**.  
Isolates let you **run code in parallel** by creating separate Dart VM threads that don’t block the main UI thread.

---

## 2. Logical Explanation  
An **isolate** is an independent Dart execution unit with its own memory.  
Key facts:  
- **No shared memory**: isolates communicate only via **message passing**  
- **True parallelism**: on multi-core devices, isolates run simultaneously  
- **Heavy use only**: creating isolates has overhead—don’t use for trivial tasks

When to use isolates:  
✅ Image/video processing  
✅ Cryptography  
✅ Large data sorting/parsing  
✅ Complex math (e.g., physics simulations)

> Isolates = **offload work** so your UI stays buttery smooth.

---

## 3. Visual Representation  

```
Main Isolate (UI Thread)
│
├── Handles widgets, taps, animations
│
└── Sends message: "Process this image"
         │
         ▼
Worker Isolate (Background)
│
├── Runs CPU-heavy task
│
└── Sends back: "Done! Here's result"
         │
         ▼
Main Isolate resumes with result
```

> **No shared state**—only messages flow between isolates.

---

## 4. Dart Syntax  

```dart
// 1. Define top-level or static function (isolates can't access closures)
Future<String> parseLargeJson(String json) {
  // Simulate heavy work
  final data = jsonDecode(json);
  return 'Parsed ${data.length} items';
}

// 2. Run in isolate
void startProcessing() async {
  final json = '{"items": [...]}';
  final result = await compute(parseLargeJson, json);
  print(result);
}
```

> - Use `compute()` from `dart:isolate` for simple cases  
> - The function **must be top-level or static**  
> - Data is **copied** (not shared) between isolates

---

## 5. Practical Examples  

### Example 1: Image Resizing (Conceptual)  
```dart
// In isolate
Uint8List resizeImage(Uint8List original) {
  // heavy image processing
  return resizedBytes;
}

// In UI
final resized = await compute(resizeImage, originalBytes);
setImage(resized);
```

### Example 2: Prime Number Checker  
```dart
bool isPrime(int n) {
  if (n < 2) return false;
  for (int i = 2; i * i <= n; i++) {
    if (n % i == 0) return false;
  }
  return true;
}

// Check large number without freezing UI
final result = await compute(isPrime, 982451653);
showResult(result ? 'Prime!' : 'Not prime');
```

> Keeps UI responsive during computation.

---

## 6. Problem-Solving Exercises  

**Easy**  
1. Why can’t you pass a local function (defined inside another function) to `compute()`?

**Medium**  
2. You need to sort a list of 1 million integers.  
   Write a top-level function `sortList(List<int> numbers)` that returns the sorted list.  
   How would you call it from the UI thread?

**Advanced**  
3. What are two limitations of `compute()`? When would you need to use `Isolate.spawn()` instead?

---

## 7. Clean Solution & Explanation  

**Exercise 1**  
> Because isolates **cannot share closures or context**.  
> Only top-level or static functions have no hidden dependencies.

**Exercise 2**  
```dart
// Top-level function
List<int> sortList(List<int> numbers) {
  return numbers..sort();
}

// In UI
final sorted = await compute(sortList, hugeList);
```
> Offloads sorting—UI remains interactive.

**Exercise 3**  
**Limitations of `compute()`:**  
1. Only supports **one input and one output**  
2. Cannot maintain **long-lived** worker (starts/stops each time)

> Use `Isolate.spawn()` when you need:  
> - Continuous communication (e.g., real-time data pipeline)  
> - Multiple messages over time  
> - Custom message handling

---

## 8. Key Takeaways  
- Isolates = **parallel execution** for CPU-heavy tasks  
- **No shared memory**—communicate via messages only  
- Use `compute()` for **simple, one-off** tasks  
- Functions must be **top-level or static**  
- **Don’t overuse**: isolates have startup overhead  
- Critical for **smooth UI** in data-intensive apps
