# Chapter 16: Enums & Extensions

## 1. Concept Goal  
**What problem does this solve?**  
When you use strings or integers to represent fixed sets of values (e.g., `"pending"`, `"approved"`), your code becomes error-prone and hard to maintain.  
**Enums** provide type-safe, self-documenting constants.  
**Extensions** let you add functionality to existing types—without modifying their source.

---

## 2. Logical Explanation  
### Enums: Represent a fixed set of named values  
Instead of `status = "active"`, use `status = UserStatus.active`.  
Benefits:  
✅ Compiler ensures only valid values are used  
✅ Auto-complete in IDE  
✅ Exhaustive `switch` coverage (no missed cases)

### Extensions: Add methods to any type  
You can’t modify `String` or `DateTime`—but with extensions, you can make them **behave like you own them**.  
Example: `"hello".toTitleCase()` instead of a loose helper function.

Together, they make code **cleaner, safer, and more expressive**—without inheritance or wrapper classes.

---

## 3. Visual Representation  

**Enum as a Closed Set**  
```
UserStatus: { active, inactive, suspended }
                ↑
        Only these 3 values allowed
```

**Extension as Glue Logic**  
```
Original Type (String)
       │
       ▼
+------------------+
| toTitleCase()    | ← Added via extension
| isEmail()        |
+------------------+
```

> Extensions **attach behavior** without changing the original type.

---

## 4. Dart Syntax  

```dart
// Enum
enum UserStatus { active, inactive, suspended }

// Enhanced enum (with fields & methods)
enum HttpStatus {
  ok(200),
  notFound(404),
  serverError(500);

  const HttpStatus(this.code);
  final int code;
}

// Extension
extension StringX on String {
  String toTitleCase() {
    if (isEmpty) return this;
    return '${this[0].toUpperCase()}${substring(1).toLowerCase()}';
  }

  bool get isEmail => contains('@') && contains('.');
}

// Usage
final status = UserStatus.active;
print('Status: $status'); // "active"

final message = 'HELLO WORLD'.toTitleCase(); // "Hello world"
```

> - Use **simple enums** for flags/states  
> - Use **enhanced enums** when values carry data  
> - Extensions work on **any type** (even `int`, `List`, or your own classes)

---

## 5. Practical Examples  

### Example 1: Form Validation States  
```dart
enum ValidationState { initial, valid, invalid }

// In UI
Color getStateColor(ValidationState state) {
  switch (state) {
    case ValidationState.valid: return Colors.green;
    case ValidationState.invalid: return Colors.red;
    case ValidationState.initial: return Colors.grey;
  }
}
```
> Compiler ensures you handle all states.

### Example 2: Date Utilities  
```dart
extension DateUtils on DateTime {
  bool get isToday {
    final now = DateTime.now();
    return year == now.year && month == now.month && day == now.day;
  }

  DateTime get tomorrow => add(const Duration(days: 1));
}

// Usage
if (selectedDate.isToday) { ... }
final nextDay = DateTime.now().tomorrow;
```
> Reads like built-in functionality.

---

## 6. Problem-Solving Exercises  

**Easy**  
1. Create an enum `ThemeMode` with values: `light`, `dark`, `system`.  
   Write a `switch` that returns the corresponding string for each.

**Medium**  
2. Add an extension on `num` called `formattedCurrency` that returns a string like `"$1,234.56"`.

**Advanced**  
3. Design an enhanced enum `IconType` that holds an icon data value (e.g., `IconData`).  
   Use it to build a generic `AppIcon` widget that takes an `IconType` and renders it.

---

## 7. Clean Solution & Explanation  

**Exercise 1**  
```dart
enum ThemeMode { light, dark, system }

String toSettingString(ThemeMode mode) {
  switch (mode) {
    case ThemeMode.light: return 'light';
    case ThemeMode.dark: return 'dark';
    case ThemeMode.system: return 'system';
  }
}
```
> Exhaustive switch = no runtime surprises.

**Exercise 2**  
```dart
extension NumX on num {
  String get formattedCurrency {
    return '\$${toStringAsFixed(2)}';
  }
}

// Usage: 1234.5.formattedCurrency → "$1234.50"
```
> Simple, reusable, and attached directly to the type.

**Exercise 3**  
```dart
import 'package:flutter/material.dart';

enum IconType {
  home(Icons.home),
  settings(Icons.settings),
  profile(Icons.person);

  const IconType(this.icon);
  final IconData icon;
}

class AppIcon extends StatelessWidget {
  final IconType type;
  const AppIcon({super.key, required this.type});

  @override
  Widget build(BuildContext context) {
    return Icon(type.icon);
  }
}

// Usage: AppIcon(type: IconType.home)
```
> Type-safe icons. No magic strings or raw `IconData`.

---

## 8. Key Takeaways  
- Use **enums** for fixed sets of states/values—never strings or ints  
- **Enhanced enums** can carry data and behavior  
- **Extensions** add methods to existing types cleanly and safely  
- Combine both to create **expressive, self-documenting APIs**  
- Flutter developers use these daily for clean, maintainable code
