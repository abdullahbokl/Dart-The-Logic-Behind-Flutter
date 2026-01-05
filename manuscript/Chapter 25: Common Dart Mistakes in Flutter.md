# Chapter 25: Common Dart Mistakes in Flutter

![Cover - Chapter 25](resources/images/Covers/chapter-25-common-dart-mistakes-in-flutter.jpg)

## 1. Concept Goal  
**What problem does this solve?**  
Even experienced developers fall into Dart/Flutter traps that cause performance issues, bugs, or memory leaks.  
This chapter highlights **real-world mistakes**—and how to avoid them—so you build robust, efficient apps.

---

## 2. Logical Explanation  
Flutter’s reactive model is powerful—but misusing Dart features leads to:  
- Unnecessary widget rebuilds  
- Memory leaks  
- Null safety errors  
- Performance bottlenecks  

The root cause is usually one of three:  
1. **Misunderstanding immutability** (mutating state directly)  
2. **Ignoring widget lifecycle** (not disposing streams/timers)  
3. **Over-engineering** (complex patterns where simplicity works)  

> Fixing these isn’t about “advanced Dart”—it’s about **applying fundamentals correctly**.

---

## 3. Visual Representation  

**Mistake vs Fix**  
```
Mistake:                            Fix:
StatefulWidget                     StatefulWidget
├── List items = [...]             ├── final List items;
├── _addItem() {                   ├── _addItem() {
│     items.add(...);              │     items = [...items, newItem];
│     setState(() {});             │     setState(() {});
│   }                              │   }
└── Rebuilds entire list           └── Efficient, immutable update
```

> Small changes, big impact.

---

## 4. Dart Syntax  

```dart
// ❌ Mistake 1: Creating objects in build()
@override
Widget build(BuildContext context) {
  return FutureBuilder(
    future: fetchData(), // ← New Future on every rebuild!
    builder: ...,
  );
}

// ✅ Fix: Create once in initState or as final field
late final Future _future = fetchData();

// ❌ Mistake 2: Not disposing StreamSubscription
void initState() {
  _sub = myStream.listen((_) { ... });
}

// ✅ Fix: Cancel in dispose()
@override
void dispose() {
  _sub.cancel();
  super.dispose();
}

// ❌ Mistake 3: Using == with mutable objects in state
if (oldState.items == newState.items) // ← false even if content same!

// ✅ Fix: Use const, immutable objects, or DeepCollectionEquality
```

---

## 5. Practical Examples  

### Example 1: Rebuild Hell from Inline Callbacks  
```dart
// ❌ BAD: New function instance every build → unnecessary rebuilds
ElevatedButton(
  onPressed: () => _handleTap(context),
  child: Text('Submit'),
);

// ✅ GOOD: Stable callback
final _onPressed = () => _handleTap(context); // or method reference

ElevatedButton(
  onPressed: _onPressed,
  child: Text('Submit'),
);
```

> Use `static`, `final`, or top-level functions for stable callbacks.

### Example 2: Null Safety Misuse  
```dart
// ❌ Dangerous: Bypassing null safety
final name = user.name!; // CRASH if name is null

// ✅ Safe: Handle null gracefully
final name = user.name ?? 'Guest';
```

> Never use `!` unless you’ve already checked.

---

## 6. Problem-Solving Exercises  

**Easy**  
1. Why is this code problematic?  
```dart
TextButton(onPressed: () { print('Hello'); }, child: Text('Click'))
```

**Medium**  
2. A `StatefulWidget` uses a `Timer` to update every second.  
   What’s missing, and what could happen?

**Advanced**  
3. You pass a mutable `List<String>` to a widget as `items`.  
   The widget calls `items.add('new')`. What’s the risk, and how do you fix it?

---

## 7. Clean Solution & Explanation  

**Exercise 1**  
> Creates a **new function instance** on every `build()`.  
> If this button is inside a `ListView.builder`, it causes **unnecessary rebuilds** of other items.  
> **Fix**: Use a stable method: `onPressed: _printHello`.

**Exercise 2**  
> Missing `Timer.cancel()` in `dispose()`.  
> **Result**: Timer keeps firing after widget is removed → **memory leak** and potential `setState() called after dispose` errors.  
> **Fix**:  
```dart
late Timer _timer;
void initState() {
  _timer = Timer.periodic(Duration(seconds: 1), (_) => _update());
}
@override
void dispose() {
  _timer.cancel();
  super.dispose();
}
```

**Exercise 3**  
> Risk: **Mutating shared state**—other widgets relying on the list see unexpected changes.  
> **Fix**:  
```dart
// In parent: pass unmodifiable view
child: MyWidget(items: List.unmodifiable(originalList));

// In widget: never mutate
// items.add(...) → compile error
```
> Or use immutable lists from the start.

---

## 8. Key Takeaways  
- **Never create** `Future`, `Stream`, or **callbacks** inside `build()`  
- **Always dispose** `StreamSubscription`, `Timer`, `AnimationController`  
- **Avoid `!`**—handle nulls with `??`, `?.`, or conditionals  
- **Pass immutable data** to widgets (`final`, `List.unmodifiable`)  
- **Stable widget properties** prevent unnecessary rebuilds  
- Most “Flutter performance issues” are **Dart misuse issues**
