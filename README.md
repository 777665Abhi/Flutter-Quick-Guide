
# 🟢 1. Flutter Basics
# ➤ Flutter Architecture
Flutter uses a declarative UI approach. The UI is built as a tree of widgets. At runtime, Flutter renders this tree efficiently using its own rendering engine (Skia).

# ➤ Dart Language Essentials
Dart is an object-oriented, strongly typed language developed by Google — used exclusively in Flutter for both frontend and backend logic.

🔹 1. Variables and Data Types

var name = 'Abhishek';    // Type inferred as String

String city = 'Delhi';     // Explicit type

final age = 25;            // Set once, not reassignable

const PI = 3.14;           // Compile-time constant

late String country;       // Will assign later, non-null

Common Types:

String, int, double, bool


List, Set, Map


dynamic (can hold any type)


Object? (safe nullable object)




🔹 2. Null Safety

Dart has null safety to avoid null-pointer errors.

String name = 'John';       // Non-nullable

String? nickname = null;    // Nullable

Operators:

? → Nullable

! → Force non-null

?? → Default value if null

?. → Null-aware access

print(nickname?.length ?? 0);



🔹 3. Functions
int add(int a, int b) {
  return a + b;
}

// Arrow syntax
int square(int x) => x * x;

// Optional & Named Params
String greet({String name = 'Guest'}) => 'Hello, $name!';


🔹 4. Control Flow

if (age > 18) {
  print("Adult");
} else {
  print("Minor");
}

for (int i = 0; i < 5; i++) {
  print(i);
}

while (true) {
  break;
}


🔹 5. Collections (List, Set, Map)

var items = [1, 2, 3];           // List

var uniqueItems = {1, 2, 3};     // Set

var user = {'name': 'John'};     // Map

Spread Operator: ... and null-aware ...?
Collection if / for:


var list = [
  'Home',
  if (isLoggedIn) 'Profile',
  for (var i in [1, 2]) 'Item $i',
];


🔹 6. Classes and Objects

class User {
  String name;
  int age;
  User(this.name, this.age);
  void greet() => print('Hi $name');
}

var user = User('Abhi', 28);

🔹 7. Constructors & Named Constructors

class Product {
  final String id;
  final String name;
  Product(this.id, this.name);
  Product.fromJson(Map<String, dynamic> json)
      : id = json['id'],
        name = json['name'];
}


🔹 8. Getters, Setters

class Circle {
  double radius;
  Circle(this.radius);
  double get area => 3.14 * radius * radius;
  set diameter(double d) {
    radius = d / 2;
  }}

🔹 9. Enums & Switch

enum Status { loading, success, error }

void handle(Status s) {
  switch (s) {
    case Status.loading:
      print('Loading...');
      break;
    case Status.success:
      print('Done!');
      break;
    default:
      print('Error');
  }  }

🔹 10. Asynchronous Programming

Dart supports Future, async/await, and Stream.
Future<String> fetchUser() async {
  await Future.delayed(Duration(seconds: 1));
  return 'Abhi';
}

Stream<int> counterStream() async* {
  for (int i = 0; i < 5; i++) {
    await Future.delayed(Duration(seconds: 1));
    yield i;
  }
}


🔹 11. Mixins & Abstract Classes

mixin Logger {
  void log(String msg) => print('Log: $msg');
}

abstract class Animal {
  void sound();
}

class Dog extends Animal with Logger {
  @override
  void sound() => log('Woof');
}


🔹 12. Extension Methods

Add new methods to existing types:

extension StringUtils on String {
  String capitalize() => this[0].toUpperCase() + substring(1);
}

print('flutter'.capitalize()); // Flutter


✅ Best Practices

Prefer final over var when variables are immutable.


Avoid dynamic unless absolutely necessary.


Use late for non-null values initialized later.


Use const for compile-time constants and UI optimization.




# ➤ Stateless vs Stateful Widgets

StatelessWidget: UI that doesn’t change over time (e.g., a logo).


StatefulWidget: UI that reacts to user interaction or async data (e.g., a counter).
Here’s a clear comparison of StatelessWidget and StatefulWidget in Flutter, along with examples:

🔹 StatelessWidget

Definition: A widget that does not require mutable state.


When to use: When the UI doesn’t change once it’s built.


Examples:


Static text, Logo or icons, Read-only UI


✅ Example:

class MyLogo extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Center(
      child: FlutterLogo(size: 100),
    );           }            }

🔸 StatefulWidget

Definition: A widget that maintains state that might change during the widget's lifetime.


When to use: When UI needs to update in response to:


User interaction, API calls, Animations


Examples:


Counter, Form with validation, Fetching and displaying async data


✅ Example:

class CounterWidget extends StatefulWidget {
  @override
  _CounterWidgetState createState() => _CounterWidgetState();
}

class _CounterWidgetState extends State<CounterWidget> {
  int _counter = 0;
  void _increment() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text('Count: $_counter'),
        ElevatedButton(
          onPressed: _increment,
          child: Text('Increment'),
        ),
      ],    );    }  }


## 🔹 StatelessWidget Lifecycle

Stateless widgets are simple and have only one lifecycle method:
🔁 build(BuildContext context)
Called once when the widget is inserted into the widget tree.


It does not get called again unless the widget is recreated (e.g., due to parent widget rebuild).


✅ Example:

```
class MyStateless extends StatelessWidget {

  @override
  Widget build(BuildContext context) {

    print("Stateless build called");

    return Text("Hello");
  }   }
```
✅ Stateless = Build Only Once (unless parent rebuilds)

🔸 StatefulWidget Lifecycle
Stateful widgets are more complex and have multiple lifecycle methods in two parts:
StatefulWidget class (immutable)


_State class (mutable, holds state and lifecycle logic)


🔁 Lifecycle Methods in _State:
Method
Description
initState()
Called once when the widget is first inserted into the tree. Ideal for initialization like data fetching or setting up controllers.
didChangeDependencies()
Called immediately after initState() and whenever an inherited widget changes.
build()
Called after initState() and every time setState() is called.
setState()
Triggers a rebuild of the widget with new data/state.
didUpdateWidget()
Called when the widget configuration changes but the state is reused.
deactivate()
Called when the widget is removed from the tree but might be reused later.
dispose()
Called when the widget is permanently removed. Clean up controllers, listeners, etc.


🔄 Full Lifecycle Flow:
createState() → initState() → didChangeDependencies() → build()
        ↓
    (on setState) → build()
        ↓
   didUpdateWidget()
        ↓
   deactivate()
        ↓
     dispose()


✅ Example:
class MyStateful extends StatefulWidget {
  @override
  _MyStatefulState createState() => _MyStatefulState();
}


class _MyStatefulState extends State<MyStateful> {
  @override
  void initState() {
    super.initState();
    print("initState");
  }


  @override
  void didChangeDependencies() {
    super.didChangeDependencies();
    print("didChangeDependencies");
  }


  @override
  void didUpdateWidget(covariant MyStateful oldWidget) {
    super.didUpdateWidget(oldWidget);
    print("didUpdateWidget");
  }


  @override
  void dispose() {
    print("dispose");
    super.dispose();
  }


  @override
  Widget build(BuildContext context) {
    print("build");
    return Text("Hello Stateful");
  }
}


🧠 Summary Table:
Lifecycle Method
StatelessWidget
StatefulWidget
build()
✅
✅
initState()
❌
✅
didChangeDependencies()
❌
✅
didUpdateWidget()
❌
✅
setState()
❌
✅
deactivate()
❌
✅
dispose()
❌
✅


🟡 2. UI & Layout
➤ Layout Widgets
Row & Column: Arrange widgets horizontally or vertically.


Expanded, Flexible: Control how widgets share space.


Stack: Overlay widgets on top of each other.


➤ Responsive UI
Use MediaQuery to get screen size.


Use LayoutBuilder or packages like flutter_screenutil for adaptive layout.


➤ Animations
Implicit: Widgets like AnimatedContainer.


Explicit: Using AnimationController, Tween, AnimatedBuilder.


Hero Animation: For smooth transition between screens.


Lottie: Render JSON-based animations.



🟠 3. State Management
State management is how your app updates its UI when data changes.
➤ Simple Options:
setState: Good for small widgets or pages.


➤ Popular Packages:
Provider: Officially recommended. Uses ChangeNotifier.


Riverpod: Safer and more scalable version of Provider.


BLoC: Separates business logic from UI using Streams.


GetX: Lightweight and reactive. Handles routing, state, and dependencies.



🔵 4. Navigation & Routing
➤ Navigator 1.0
Push and pop routes manually:

 Navigator.push(context, MaterialPageRoute(builder: (_) => NextScreen()));


➤ Navigator 2.0
More control (especially for web and deep links).


➤ GoRouter / AutoRoute
Simplify routing, support deep linking and route guards.



🔴 5. Forms & Input
➤ Form Widget
Groups form fields together. Use TextFormField with validation logic.
➤ Focus & Keyboard
Manage focus using FocusNode and FocusScope, useful when validating and auto-switching fields.

🟣 6. Backend & API
➤ HTTP Requests
Use http or dio to call REST APIs. Decode JSON into Dart models.
➤ Parsing JSON
Use json_serializable and build_runner for automatic JSON mapping.
➤ Real-time
Use WebSockets, Firebase, or MQTT for live updates.

🟤 7. Database & Storage
➤ Local Storage
Hive: Fast, lightweight NoSQL.


SharedPreferences: Store key-value pairs.


sqflite: SQL-based local DB.


➤ Firebase
Firestore (NoSQL cloud DB), Auth, Cloud Storage.



⚫ 8. Testing
➤ Types:
Unit Tests: Test business logic.


Widget Tests: Render and test widgets in isolation.


Integration Tests: Simulate user actions across the app.


Use flutter_test, mockito, and integration_test package.

⚪ 9. Advanced Topics
➤ Performance Optimization
Use const widgets


Avoid unnecessary rebuilds


Use RepaintBoundary, ListView.builder


➤ Platform Channels
Talk to native Android (Java/Kotlin) or iOS (Swift/Obj-C) code using MethodChannels.
➤ Code Generation
Use freezed, json_serializable, injectable for writing less boilerplate code.

🧠 10. Architecture & Patterns
➤ Clean Architecture
Entity: Core business objects.


UseCase: Specific actions (e.g., GetUserProfile).


Repository: Interface to data sources.


Data Layer: Actual API/DB implementations.


Helps in testing, scaling, and maintenance.

🧩 11. Tooling & DevOps
➤ Linting & CI/CD
Use flutter_lints, very_good_analysis for code quality.


Automate builds and testing using GitHub Actions, Codemagic, or Bitrise.


➤ Flavors
Separate environments (dev/stage/prod) using custom main.dart files and configuration.

Would you like a study plan, PDF version, or interview prep questions based on this list?

