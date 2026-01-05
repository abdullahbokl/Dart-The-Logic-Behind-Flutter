# Chapter 21: Streams

![Cover - Chapter 21](resources/images/Covers/chapter-21-streams.jpg)

## 1. Concept Goal  
**What problem does this solve?**  
Some data arrives **repeatedly over time**: user input, sensor readings, chat messages, or real-time updates.  
`Future` handles **one-time** async results.  
`Stream` handles **multiple values over time**—so you can react to every new event as it arrives.

---

## 2. Logical Explanation  
A **Stream** is a sequence of data events that may arrive now, later, or never.  
Think of it as a **pipeline**:  
- Events flow in (from user taps, network, sensors)  
- You listen to the stream  
- Your code runs **each time** a new event arrives

Key patterns:  
- **Single-subscription streams**: One listener (e.g., file reading)  
- **Broadcast streams**: Many listeners (e.g., button clicks, Firebase updates)

> Use `Future` for “get one result.”  
> Use `Stream` for “keep getting results.”

---

## 3. Visual Representation  

```
Stream<Event>
│
├── Event 1 → listener runs
├── Event 2 → listener runs
├── Event 3 → listener runs
└── Done (or Error)
```

**Broadcast Stream (Multiple Listeners)**:  
```
          Stream
             │
     ┌───────┴───────┐
     ▼               ▼
Listener A      Listener B
```

> Broadcast streams = **pub/sub** pattern.

---

## 4. Dart Syntax  

```dart
// Create a broadcast stream (e.g., for UI events)
final controller = StreamController<String>.broadcast();

// Add data
controller.sink.add('Hello');
controller.sink.add('World');

// Listen
controller.stream.listen(
  (message) => print(message), // called for each event
  onError: (error) => print('Error: $error'),
  onDone: () => print('Stream closed'),
);

// Close when done
controller.close();
```

> - Use `StreamController` to create and manage streams  
> - `sink.add()` pushes data into the stream  
> - Always call `close()` to free resources

---

## 5. Practical Examples  

### Example 1: Real-Time Search (Debounced)  
```dart
final _searchController = TextEditingController();
late StreamSubscription _subscription;

void initState() {
  _subscription = _searchController.textChanges
      .debounceTime(Duration(milliseconds: 300))
      .listen((text) {
    searchApi(text);
  });
}

@override
void dispose() {
  _subscription.cancel();
  super.dispose();
}
```

### Example 2: Firebase-Like Realtime Updates  
```dart
Stream<User> userUpdates(String userId) {
  return FirebaseFirestore.instance
      .collection('users')
      .doc(userId)
      .snapshots()
      .map((snapshot) => User.fromJson(snapshot.data()!));
}

// In widget
userUpdates('123').listen((user) {
  setState(() => _currentUser = user);
});
```

> Streams power real-time features in Flutter.

---

## 6. Problem-Solving Exercises  

**Easy**  
1. Create a broadcast stream that emits numbers 1, 2, 3 with 1-second delays between them.

**Medium**  
2. You have a `Stream<String>` of user input.  
   Transform it into a stream that only emits **non-empty, trimmed** strings.

**Advanced**  
3. Build a `Stream<int>` that emits the current second (0–59) every second.  
   Use `Stream.periodic`. Cancel after 10 seconds.

---

## 7. Clean Solution & Explanation  

**Exercise 1**  
```dart
Stream<int> countToThree() async* {
  for (int i = 1; i <= 3; i++) {
    await Future.delayed(Duration(seconds: 1));
    yield i;
  }
}

// Usage
countToThree().listen(print);
```
> `async*` + `yield` = easy stream generation.

**Exercise 2**  
```dart
Stream<String> cleanInput(Stream<String> raw) {
  return raw
      .map((text) => text.trim()) // trim
      .where((text) => text.isNotEmpty); // filter empty
}
```
> Use `map` to transform, `where` to filter.

**Exercise 3**  
```dart
void runTimer() {
  final sub = Stream.periodic(Duration(seconds: 1), (i) => DateTime.now().second)
      .take(10) // stop after 10 events
      .listen(print);

  // No need to cancel—`take(10)` closes the stream automatically
}
```
> `Stream.periodic` = built-in timer stream  
> `take(n)` = auto-cancel after n events

---

## 8. Key Takeaways  
- Use **Streams** for **multiple, time-based events**  
- **Broadcast streams** for UI/events (multiple listeners)  
- **Single-subscription** for files/network (one listener)  
- Always **cancel subscriptions** or use `take()`/`first` to auto-close  
- Transform streams with `map`, `where`, `debounce`, etc.  
- Streams are the backbone of **real-time** Flutter apps
