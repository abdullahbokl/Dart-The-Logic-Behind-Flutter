# Chapter 3: Control Flow

![Cover - Chapter 3](resources/images/Covers/chapter-03-control-flow.jpg)

## 1. Concept Goal  
**What problem does this solve?**  
Programs rarely run the same way every time. Users make choices, data changes, and conditions vary.  
Control flow lets your code **make decisions**—so it can react intelligently instead of blindly following one path.

---

## 2. Logical Explanation  
Control flow is how your program **chooses what to do next** based on conditions (true/false questions).  

- **`if`/`else`**: “If it’s raining, take an umbrella; else, wear sunglasses.”  
- **`switch`**: “Based on the user’s role (admin, editor, viewer), show the right menu.”  
- **Logical operators** (`&&`, `||`, `!`): Combine simple conditions into smart rules (“Is logged in **AND** email verified?”).

The goal isn’t just to branch—it’s to **keep logic readable and predictable**. Complex nesting leads to bugs. Flatten when possible.

---

## 3. Visual Representation  

```
Start
  │
  ▼
[ Condition? ] ──Yes──→ Do A ──┐
  │                           │
  No                          ▼
  │                        End
  ▼
Do B ────────────────────────┘
```

**Switch Flow**:
```
Value = "admin"
   │
   ├── "admin" → Show full menu
   ├── "editor" → Show edit menu
   └── default → Show read-only menu
```

> Each path handles one scenario—no guesswork.

---

## 4. Dart Syntax  

```dart
// if / else
if (isLoggedIn) {
  showDashboard();
} else if (isGuest) {
  showWelcome();
} else {
  redirectToLogin();
}

// Logical operators
if (isLoggedIn && emailVerified) {
  enableFeatures();
}

if (isOffline || isTrialExpired) {
  disableSave();
}

// switch (exhaustive when using enums)
switch (userRole) {
  case UserRole.admin:
    showAdminPanel();
    break;
  case UserRole.viewer:
    showReports();
    break;
  default:
    showError('Unknown role');
}
```

> Dart requires `break` (or `return`) in `switch` cases to avoid fall-through bugs.

---

## 5. Practical Examples  

### Example 1: Form Validation  
```dart
if (email.isEmpty) {
  showError('Email is required');
} else if (!email.contains('@')) {
  showError('Invalid email format');
} else {
  submitForm();
}
```

### Example 2: Flutter Permission Check  
```dart
switch (permissionStatus) {
  case PermissionStatus.granted:
    loadLocation();
    break;
  case PermissionStatus.denied:
    showPermissionRationale();
    break;
  case PermissionStatus.permanentlyDenied:
    openAppSettings();
    break;
}
```

> Real-world logic is rarely binary—it’s a chain of clear, testable conditions.

---

## 6. Problem-Solving Exercises  

**Easy**  
1. Write an `if` check: Show “Adult” if age ≥ 18, else “Minor”.

**Medium**  
2. A discount applies only if:  
   - User is a member **AND**  
   - Cart total > $50 **OR** it’s a holiday.  
   Write the condition using logical operators.

**Advanced**  
3. You have a `ConnectionState` enum: `waiting`, `active`, `inactive`, `error`.  
   Use a `switch` to return the correct message for each state.  
   Ensure all cases are handled.

---

## 7. Clean Solution & Explanation  

**Exercise 1**  
```dart
if (age >= 18) {
  print('Adult');
} else {
  print('Minor');
}
```
> Direct translation of the rule. No extra logic.

**Exercise 2**  
```dart
if (isMember && (cartTotal > 50 || isHoliday)) {
  applyDiscount();
}
```
> Parentheses clarify that “cart total OR holiday” is one unit.  
> Without them, precedence could mislead.

**Exercise 3**  
```dart
String getMessage(ConnectionState state) {
  switch (state) {
    case ConnectionState.waiting:
      return 'Connecting...';
    case ConnectionState.active:
      return 'Online';
    case ConnectionState.inactive:
      return 'Offline';
    case ConnectionState.error:
      return 'Connection failed';
  }
}
```
> Exhaustive switch = no surprises. Dart will warn if you miss a case (when using enums).

---

## 8. Key Takeaways  
- Use `if`/`else` for simple or dynamic conditions.  
- Use `switch` for known, discrete values (especially enums).  
- Group complex logic with **parentheses** for clarity.  
- Avoid deeply nested `if`s—extract to functions or flatten logic.  
- Control flow should **mirror real-world rules**, not obscure them.
