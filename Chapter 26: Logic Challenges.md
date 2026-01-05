# Chapter 26: Logic Challenges

![Cover - Chapter 26](Images/Covers/chapter-26-logic-challenges.jpg)

## 1. Concept Goal  
**What problem does this solve?**  
Knowing syntax isn’t enough—you must **think algorithmically** to solve real problems.  
This chapter sharpens your problem-solving muscle with focused challenges on conditions, loops, and data manipulation—using only core Dart.

---

## 2. Logical Explanation  
These exercises mimic **real coding tasks** you’ll face in Flutter:  
- Validating user input  
- Transforming API responses  
- Filtering and grouping data  

Approach each problem in 3 steps:  
1. **Understand**: What’s the input? What’s the expected output?  
2. **Plan**: Break into small steps (IPO model)  
3. **Implement**: Use the simplest Dart features that work  

> No frameworks. No magic. Just logic + Dart.

---

## 3. Visual Representation  

**Problem-Solving Loop**  
```
Read Problem → Clarify Input/Output → Sketch Steps → Code → Test → Refine
```

**Example: Find duplicates in a list**  
```
Input: [1, 2, 2, 3]
Step 1: Track seen items (Set)
Step 2: For each item:
        - if in seen → duplicate
        - else → add to seen
Output: [2]
```

> Visualize the data flow first.

---

## 4. Dart Syntax (Patterns You’ll Use)  

```dart
// Common tools
List.where(), List.map(), Set, Map,
for-in loops, if/else, ??, ??

// Example: Filter and transform
final validEmails = inputs
  .where((s) => s.contains('@'))
  .map((s) => s.trim())
  .toList();
```

> Leverage Dart’s collection methods—they’re concise and safe.

---

## 5. Practical Examples  

### Example 1: FizzBuzz  
```dart
// Return "Fizz" for multiples of 3, "Buzz" for 5, "FizzBuzz" for both
List<String> fizzBuzz(int n) {
  return [
    for (int i = 1; i <= n; i++)
      if (i % 15 == 0) 'FizzBuzz'
      else if (i % 3 == 0) 'Fizz'
      else if (i % 5 == 0) 'Buzz'
      else '$i'
  ];
}
```

### Example 2: Group By  
```dart
Map<String, List<User>> groupByRole(List<User> users) {
  final map = <String, List<User>>{};
  for (final user in users) {
    map.putIfAbsent(user.role, () => []).add(user);
  }
  return map;
}
```

> Real-world data transformation.

---

## 6. Problem-Solving Exercises  

**Easy**  
1. Write a function `isPalindrome(String s)` that returns `true` if the string reads the same backward (ignore case).

**Medium**  
2. Given a `List<int> numbers`, return a new list with only **positive even** numbers, sorted in descending order.

**Advanced**  
3. You have `List<Map<String, dynamic>>` representing user sessions:  
   ```dart
   [
     {'userId': '1', 'duration': 120},
     {'userId': '2', 'duration': 80},
     {'userId': '1', 'duration': 90},
   ]
   ```
   Return a `Map<String, int>` with **total duration per user**.

---

## 7. Clean Solution & Explanation  

**Exercise 1**  
```dart
bool isPalindrome(String s) {
  final clean = s.toLowerCase();
  return clean == clean.split('').reversed.join();
}
```
> Simple, readable. Uses built-in `reversed`.

**Exercise 2**  
```dart
List<int> positiveEvenDesc(List<int> numbers) {
  return numbers
      .where((n) => n > 0 && n % 2 == 0)
      .toList()
    ..sort((a, b) => b.compareTo(a));
}
```
> `where` → filter, `sort` with custom comparator → descending.

**Exercise 3**  
```dart
Map<String, int> totalDurationPerUser(List<Map<String, dynamic>> sessions) {
  final totals = <String, int>{};
  for (final session in sessions) {
    final userId = session['userId'] as String;
    final duration = session['duration'] as int;
    totals.update(userId, (value) => value + duration, ifAbsent: () => duration);
  }
  return totals;
}
```
> `update(ifAbsent:)` handles first occurrence cleanly.

---

## 8. Key Takeaways  
- **Break problems into input → process → output**  
- Use **collection methods** (`where`, `map`, `fold`) over manual loops when possible  
- **Test edge cases**: empty lists, nulls, zeros  
- **Readability > cleverness**—simple code is debuggable code  
- These patterns appear daily in Flutter data processing
