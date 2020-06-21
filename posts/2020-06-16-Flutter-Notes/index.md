# Flutter Notes


* [Introduction](#introduction)
* [Declarative UI](#declarative-ui)
  * [Why](#why)
  * [How](#how)
* [Views](#views)
  * [The equivalent of a view in Flutter](#the-equivalent-of-a-view-in-flutter)
  * [How to update widgets](#how-to-update-widgets)
* [Intents](#intents)
  * [The equivalent of an Intent in Flutter](#the-equivalent-of-an-intent-in-flutter)
  * [How to handle incoming intents from external apps](#how-to-handle-incoming-intents-from-external-apps)
  * [The equivalent of startActivityForResult()](#the-equivalent-of-startactivityforresult)
* [Project structure and resources](#project-structure-and-resources)
  * [Image files](#image-files)
  * [Strings](#strings)
* [Layouts](#layouts)
  * [Equivalent of a LinearLayout](#equivalent-of-a-linearlayout)
  * [Equivalent of a RelativeLayout](#equivalent-of-a-relativelayout)
  * [Equivalent of a ScrollView](#equivalent-of-a-scrollview)
* [Gesture detection and touch event handling](#gesture-detection-and-touch-event-handling)
  * [Equivalent of onClick](#equivalent-of-onclick)
  * [Other gestures](#other-gestures)
* [ListView and adapters](#listview-and-adapters)
  * [Equivalent of ListView](#equivalent-of-listview)
  * [Which item is clicked](#which-item-is-clicked)
* [References](#references)

## Introduction

This post is assuming that the reader has Android development background.

## Declarative UI

### Why

1. Flutter lets the developer describe the current UI state and leaves the transitioning to the framework. This lightens the burden on developers from having to program how to transition between various UI states.

### How

1. Example:

    ![Declarative UI](/images/declarativeUIchanges.png)

    ```java
    // Imperative style
    b.setColor(red)
    b.clearChildren()
    ViewC c3 = new ViewC(...)
    b.add(c3)
    ```

    ```dart
    // Declarative style
    return ViewB(
        color: red,
        child: ViewC(...)
    )
    ```

2. In the declarative style, view configurations (such as Flutter's Widgets) are immutable and only lightweight blueprints. To change the UI, a widget triggers a rebuild on itself and constructs a new Widget subtree. The framework manages many of the responsibilities behind the scenes.

## Views

### The equivalent of a view in Flutter

1. In Flutter, the rough equivalent to a `View` is a `Widget`.
2. The difference is that widgets are immutable and lightweight.

### How to update widgets

1. Since widgets are immutable, we have to work with the state.

2. A `StatelessWidget` is a widget with no state info, similar to a `ImageView` with a logo, which doesn't change during runtime. An example is the `Text` widget, which is a subclass of `StatelessWidget`:

    ```dart
    Text(
        'I like Flutter!',
        style: TextStyle(fontWeight: FontWeight.bold),
    );
    ```

3. If we want the text to change dynamically, we wrap the `Text` widget in a `StatefulWidget`:

    ```dart
    import 'package:flutter/material.dart';

    void main() {
      runApp(SampleApp());
    }

    class SampleApp extends StatelessWidget {
      // This widget is the root of your application.
      @override
      Widget build(BuildContext context) {
        return MaterialApp(
          title: 'Sample App',
          theme: ThemeData(
            primarySwatch: Colors.blue,
          ),
          home: SampleAppPage(),
        );
      }
    }

    class SampleAppPage extends StatefulWidget {
      SampleAppPage({Key key}) : super(key: key);

      @override
      _SampleAppPageState createState() => _SampleAppPageState();
    }

    class _SampleAppPageState extends State<SampleAppPage> {
      // Default placeholder text
      String textToShow = "I Like Flutter";

      void _updateText() {
        setState(() {
          // update the text
          textToShow = "Flutter is Awesome!";
        });
      }

      @override
      Widget build(BuildContext context) {
        return Scaffold(
          appBar: AppBar(
            title: Text("Sample App"),
          ),
          body: Center(child: Text(textToShow)),
          floatingActionButton: FloatingActionButton(
            onPressed: _updateText,
            tooltip: 'Update Text',
            child: Icon(Icons.update),
          ),
        );
      }
    }
    ```

## Intents

### The equivalent of an Intent in Flutter

1. Flutter doesn't have intents. And Flutter doesn't have a direct equivalent to activities and fragments; rather, we navigate between screens using a `Navigator` and `Route`s, all within the same `Activity`.

2. A `Route`is an abstraction for a screen or page. And a `Navigator` is a widget that manages routes. A `Route` roughly maps to an `Activity`.

3. A navigator can push and pop routes to move from screen to screen. It works like a stack on which you can `push()` new routes we want to navigate to and `pop()` routes that we want to go back.

4. We specify a `Map` of route names:

    ```dart
    void main() {
     runApp(MaterialApp(
       home: MyAppHome(), // becomes the route named '/'
       routes: <String, WidgetBuilder> {
         '/a': (BuildContext context) => MyPage(title: 'page A'),
         '/b': (BuildContext context) => MyPage(title: 'page B'),
         '/c': (BuildContext context) => MyPage(title: 'page C'),
       },
     ));
    }

    Navigator.of(context).pushNamed('/b');
    ```

5. For calling a Camera or File picker, we need a native platform integration or use plugins.

### How to handle incoming intents from external apps

1. Flutter handles incoming intents from Android by directly talking to the Android layer and requesting the data shared.

2. The basic flow is that we first handle the shared data in `Activity` and wait until Flutter requests with a `MethodChannel`.

### The equivalent of startActivityForResult()

1. It's done by `await`ing on the `Future` returned by `push()`:

    ```dart
    Map coordinates = await Navigator.of(context).pushNamed('/location');
    // Then in the location route
    Navigator.of(context).pop({"lat":43.821757,"long":-79.226392});
    ```

## Project structure and resources

### Image files

1. No predefined folder structure. We declare the assets (with location) in the `pubspec.yaml` file.

2. For example:

    ```code
    images/my_icon.png       // Base: 1.0x image
    images/2.0x/my_icon.png  // 2.0x image
    images/3.0x/my_icon.png  // 3.0x image
    // declare these in pubspec.yaml
    assets:
     - images/my_icon.jpeg
    // Then access using AssetImage
    return AssetImage("images/my_icon.jpeg");
    // Or in an Image widget
    @override
    Widget build(BuildContext context) {
        return Image.asset("images/my_image.png")
    }
    ```

### Strings

1. No dedicated resources-like system. The best practice is:

    ```dart
    class Strings {
        static String welcomeMessage = "Welcome to Flutter";
    }
    // Access
    Text(Strings.welcomeMessage);
    ```

2. We're encouraged to use the intl package for internationalization.

## Layouts

### Equivalent of a LinearLayout

1. The Row or Column widgets are the equivalent:

    ```dart
      @override
      Widget build(BuildContext context) {
        return Row(  // Or Column
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text('Row One'),
            Text('Row Two'),
            Text('Row Three'),
            Text('Row Four'),
          ],
        );
      }
    ```

### Equivalent of a RelativeLayout

1. We can achieve the same result by using a combination of Column, Row, and Stack widgets.

### Equivalent of a ScrollView

1. THe equivalent is a ListView. A ListView in Flutter is both a ScrollView and an Android ListView.

    ```dart
      @override
      Widget build(BuildContext context) {
        return ListView(  // Or Column
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text('Row One'),
            Text('Row Two'),
            Text('Row Three'),
            Text('Row Four'),
          ],
        );
      }
    ```

## Gesture detection and touch event handling

### Equivalent of onClick

1. If the widget supports event detection, pass a function to it and handle it in the function:

    ```dart
    @override
    Widget build(BuildContext context) {
      return RaisedButton(
          onPressed: () {
            print("click");
          },
          child: Text("Button"));
    }
    ```

2. If the widget doesn't support event detection, wrap it in a GestureDetector and pass a function to the `onTap` parameter:

    ```dart
    class SampleApp extends StatelessWidget {
      @override
      Widget build(BuildContext context) {
        return Scaffold(
            body: Center(
          child: GestureDetector(
            child: FlutterLogo(
              size: 200.0,
            ),
            onTap: () {
              print("tap");
            },
          ),
        ));
      }
    }
    ```

### Other gestures

1. Using GestureDetector, we can listen to gestures such as:
    * Tap
    * Double tap
    * Long press
    * Vertical drag
    * Horizontal drag

## ListView and adapters

### Equivalent of ListView

1. The equivalent of ListView is ListView:

    ```dart
    import 'package:flutter/material.dart';

    void main() {
      runApp(SampleApp());
    }

    class SampleApp extends StatelessWidget {
      // This widget is the root of your application.
      @override
      Widget build(BuildContext context) {
        return MaterialApp(
          title: 'Sample App',
          theme: ThemeData(
            primarySwatch: Colors.blue,
          ),
          home: SampleAppPage(),
        );
      }
    }

    class SampleAppPage extends StatefulWidget {
      SampleAppPage({Key key}) : super(key: key);

      @override
      _SampleAppPageState createState() => _SampleAppPageState();
    }

    class _SampleAppPageState extends State<SampleAppPage> {
      @override
      Widget build(BuildContext context) {
        return Scaffold(
          appBar: AppBar(
            title: Text("Sample App"),
          ),
          body: ListView(children: _getListData()),
        );
      }

      List<Widget> _getListData() {
        List<Widget> widgets = [];
        for (int i = 0; i < 100; i++) {
          widgets.add(Padding(
            padding: EdgeInsets.all(10.0),
            child: Text("Row $i"),
          ));
        }
        return widgets;
      }
    }
    ```

### Which item is clicked

1. Use the touch handling provided by the passed-in widgets:

    ```dart
    import 'package:flutter/material.dart';

    void main() {
      runApp(SampleApp());
    }

    class SampleApp extends StatelessWidget {
      // This widget is the root of your application.
      @override
      Widget build(BuildContext context) {
        return MaterialApp(
          title: 'Sample App',
          theme: ThemeData(
            primarySwatch: Colors.blue,
          ),
          home: SampleAppPage(),
        );
      }
    }

    class SampleAppPage extends StatefulWidget {
      SampleAppPage({Key key}) : super(key: key);

      @override
      _SampleAppPageState createState() => _SampleAppPageState();
    }

    class _SampleAppPageState extends State<SampleAppPage> {
      @override
      Widget build(BuildContext context) {
        return Scaffold(
          appBar: AppBar(
            title: Text("Sample App"),
          ),
          body: ListView(children: _getListData()),
        );
      }

      List<Widget> _getListData() {
        List<Widget> widgets = [];
        for (int i = 0; i < 100; i++) {
          widgets.add(GestureDetector(
            child: Padding(
              padding: EdgeInsets.all(10.0),
              child: Text("Row $i"),
            ),
            onTap: () {
              print('row tapped');
            },
          ));
        }
        return widgets;
      }
    }
    ```

## References

* [Flutter for Android Developers](https://flutter.dev/docs/get-started/flutter-for/android-devs)

* [Introduction to Declarative UI](https://flutter.dev/docs/get-started/flutter-for/declarative)

