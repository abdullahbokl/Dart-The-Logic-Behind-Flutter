# Chapter 24: Clean Code for Flutter

## 1. Concept Goal  
**What problem does this solve?**  
As Flutter apps grow, code becomes messy: giant widgets, unclear state logic, and tangled dependencies.  
Clean code principles **keep your app maintainable, testable, and scalable**—without over-engineering.

---

## 2. Logical Explanation  
Clean Flutter code isn’t about fancy architecture—it’s about **clarity and intent**.  
Key principles:  
- **Small widgets**: Each widget does one thing  
- **Descriptive names**: `UserAvatar` > `Container12`  
- **Separation of concerns**: UI ≠ business logic ≠ data  
- **Immutable state**: Changes create new objects, not mutations  

Flutter’s reactive model rewards simplicity:  
✅ Small widgets = easier to reuse and test  
✅ Clear names = faster onboarding  
✅ Separated logic = no “where is this state coming from?”  

> Clean code = **code that reads like a story**.

---

## 3. Visual Representation  

**Before (Messy)**  
```
MyPage
├── 200-line build()
├── Mixed UI + API calls + validation
└── Hardcoded strings and colors
```

**After (Clean)**  
```
MyPage
├── UserList (widget)
│   └── UserTile (widget)
├── UserViewModel (state + logic)
└── Constants (colors, strings)
```

> Each piece has **one reason to change**.

---

## 4. Dart Syntax  

```dart
// ✅ Good: Small, focused widgets
class UserProfile extends StatelessWidget {
  final User user;
  const UserProfile({required this.user, super.key});

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        UserAvatar(user: user),
        UserInfo(user: user),
      ],
    );
  }
}

// ✅ Good: Extract complex logic
class UserInfo extends StatelessWidget {
  final User user;
  const UserInfo({required this.user, super.key});

  String _getStatusLabel() {
    return user.isActive ? 'Active' : 'Inactive';
  }

  @override
  Widget build(BuildContext context) {
    return Text('${user.name} • ${_getStatusLabel()}');
  }
}
```

> - Widgets < 50 lines  
> - Helper methods for complex expressions  
> - No business logic in `build`

---

## 5. Practical Examples  

### Example 1: Constants File  
```dart
// constants.dart
class AppColors {
  static const primary = Color(0xFF1A73E8);
  static const background = Colors.white;
}

class AppStrings {
  static const loginTitle = 'Welcome back';
  static const save = 'Save';
}
```

Usage: `Text(AppStrings.loginTitle)`, `color: AppColors.primary`

### Example 2: State Separation  
```dart
// ❌ Bad: Logic in widget
void _onSubmit() {
  if (email.contains('@') && password.length >= 8) {
    api.login(email, password);
  }
}

// ✅ Good: Delegate to service
final authService = AuthService();

void _onSubmit() {
  if (authService.isValid(email, password)) {
    authService.login(email, password);
  }
}
```

> Widgets delegate—never own logic.

---

## 6. Problem-Solving Exercises  

**Easy**  
1. Refactor this widget name: `ContainerWidget` → ?

**Medium**  
2. A `build()` method has 120 lines and handles form validation, API calls, and UI.  
   List 3 ways to clean it up.

**Advanced**  
3. You have a widget that shows a product and allows adding to cart.  
   How do you separate UI, state, and business logic without using a full BLoC?

---

## 7. Clean Solution & Explanation  

**Exercise 1**  
> → `ProductCard`, `LoginForm`, `SettingsHeader`  
> **Rule**: Name should reflect **what it displays**, not that it’s a widget.

**Exercise 2**  
1. **Extract child widgets** (e.g., `FormSection`, `ActionButton`)  
2. **Move validation logic** to a dedicated service/class  
3. **Move API calls** to a repository or service  

> Result: `build()` becomes a **composition of clear parts**.

**Exercise 3**  
```dart
// UI only
class ProductTile extends StatelessWidget {
  final Product product;
  final VoidCallback onAddToCart;
  const ProductTile({required this.product, required this.onAddToCart, super.key});

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text(product.name),
        IconButton(
          icon: Icon(Icons.add),
          onPressed: onAddToCart,
        ),
      ],
    );
  }
}

// State + logic in parent or hook
class ProductScreen extends StatefulWidget {
  // ...
}

class _ProductScreenState extends State<ProductScreen> {
  final cartService = CartService();

  void _addToCart(Product product) {
    if (cartService.canAdd(product)) {
      cartService.add(product);
    }
  }

  @override
  Widget build(BuildContext context) {
    return ProductTile(
      product: product,
      onAddToCart: () => _addToCart(product),
    );
  }
}
```
> UI is dumb. Logic lives in services. State is managed locally or via lightweight state (not over-architected).

---

## 8. Key Takeaways  
- **Widgets should be small and focused**  
- **Never put business logic in `build`**  
- **Use constants** for colors, strings, dimensions  
- **Name things by purpose**, not type (`UserList`, not `Widget12`)  
- **Delegate complexity** to services, view models, or helpers  
- Clean code in Flutter = **simplicity, clarity, and separation**
