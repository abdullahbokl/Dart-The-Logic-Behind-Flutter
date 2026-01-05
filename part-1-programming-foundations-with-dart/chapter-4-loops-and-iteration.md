# Chapter 4: Loops & Iteration

![Cover - Chapter 4](../.gitbook/assets/chapter-04-loops-iteration.jpg)

## 1. Concept Goal

**What problem does this solve?**\
Repeating the same action for multiple items (e.g., showing a list of products, validating 10 fields) is tedious and error-prone if done manually.\
Loops automate repetition—so you write the logic **once** and apply it **many times**.

***

## 2. Logical Explanation

A loop says: _“Do this thing again and again—until a condition is met or all items are processed.”_

* **`for`**: Best when you know **how many times** to repeat (e.g., 10 items, numbers 1–100).
* **`while`**: Best when you repeat **until something changes** (e.g., “keep polling until data arrives”).
* **`do-while`**: Like `while`, but runs **at least once** (useful for menus or initial setup).

The key to safe loops:\
✅ Always move toward the end condition\
❌ Never create infinite loops (e.g., forgetting to increment a counter)

Common patterns:

* **Iterating a list**: Process each item
* **Counting down**: Timer, retries
* **Searching**: Stop when you find what you need

***

## 3. Visual Representation

```
Start
  │
  ▼
[ Loop Condition True? ] ──No──→ End
          │ Yes
          ▼
    Execute Body
          │
          ▼
   Update Counter/State
          │
          └───────────────┐
                          ▼
```

**For Loop Anatomy**:

```
for (start; condition; update) {
  // body
}

Example: for (int i = 0; i < 5; i++)
Step 1: i = 0
Step 2: Is i < 5? → Yes → run body
Step 3: i++ → i = 1
... repeat until i = 5 → stop
```

> The loop **controls its own lifecycle**—setup, check, update.

***

## 4. Dart Syntax

```dart
// for loop
for (int i = 0; i < items.length; i++) {
  print(items[i]);
}

// for-in (cleaner for collections)
for (final item in items) {
  print(item);
}

// while
int attempts = 0;
while (attempts < 3 && !success) {
  tryLogin();
  attempts++;
}

// do-while (runs at least once)
do {
  showMenu();
  choice = getUserChoice();
} while (choice != 'exit');
```

> Prefer `for-in` over indexed `for` when you don’t need the index—it’s safer and cleaner.

***

## 5. Practical Examples

### Example 1: Render List in Flutter (Conceptual)

```dart
// Often seen in ListView.builder
for (final product in products) {
  buildProductCard(product);
}
```

### Example 2: Retry Logic

```dart
int retries = 0;
bool loaded = false;

while (retries < 3 && !loaded) {
  loaded = await fetchData();
  retries++;
}

if (!loaded) showError('Failed after 3 tries');
```

> Loop stops when success **or** max retries—no waste.

***

## 6. Problem-Solving Exercises

**Easy**

1. Print numbers 1 to 10 using a `for` loop.

**Medium**\
2\. You have a list of emails. Use a loop to find the first one containing `"@company.com"`. Stop when found.

**Advanced**\
3\. Simulate a countdown timer from 10 to 0. After reaching 0, print “Blast off!”.\
Use a `while` loop. What happens if you forget to decrement the counter?

***

## 7. Clean Solution & Explanation

**Exercise 1**

```dart
for (int i = 1; i <= 10; i++) {
  print(i);
}
```

> Starts at 1, ends at 10 (inclusive). Simple and clear.

**Exercise 2**

```dart
String? found;
for (final email in emails) {
  if (email.contains('@company.com')) {
    found = email;
    break; // stop early!
  }
}
```

> `break` avoids unnecessary checks—efficiency matters.

**Exercise 3**

```dart
int seconds = 10;
while (seconds >= 0) {
  print(seconds);
  seconds--; // critical!
}
print('Blast off!');
```

> Without `seconds--`, the condition never changes → **infinite loop** → app freezes.\
> Always verify your loop moves toward termination.

***

## 8. Key Takeaways

* Use `for-in` to iterate collections—clean and idiomatic.
* Use `while` when the number of iterations is unknown.
* Always ensure your loop **progresses toward an end**.
* Use `break` to exit early when a goal is met (e.g., found item).
* Avoid manual index management unless you truly need it.
