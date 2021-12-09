# liquid_swipe_example

Demonstrates how to use the liquid_swipe plugin.

## Usage

1. Add this to your pubspec.yaml

```
dependencies:
liquid_swipe: ^1.1.0
```

2. Get package from Pub:

```
flutter packages get
```

3. Import it in your file

```
import 'package:liquid_swipe/liquid_swipe.dart';

```


## Example

![simulator_screenshot_1AEB5310-DA4B-4C79-A873-7855C95D7018](https://user-images.githubusercontent.com/30201421/145389915-b105c8df-4ffd-477e-9264-1672c682ad7f.png)

* First, create a list of Containers.

```
final pages = [
   Container(
     color: Colors.pink,
     child: Column(
       crossAxisAlignment: CrossAxisAlignment.center,
       mainAxisSize: MainAxisSize.max,
       mainAxisAlignment: MainAxisAlignment.spaceEvenly,
       children: <Widget>[
         Image.asset(
           'assets/1.png',
           fit: BoxFit.cover,
         ),
         Padding(padding: const EdgeInsets.all(20.0)),
         Column(
           children: <Widget>[
             new Text(
               "Hi",
               style: TextStyle(
                   fontSize: 30,
                   fontFamily: "Billy",
                   fontWeight: FontWeight.w600),
             ),
             new Text(
               "It's Me",
               style: TextStyle(
                   fontSize: 30,
                   fontFamily: "Billy",
                   fontWeight: FontWeight.w600),
             ),
             new Text(
               "Sahdeep",
               style: TextStyle(
                   fontSize: 30,
                   fontFamily: "Billy",
                   fontWeight: FontWeight.w600),
             ),
           ],
         )
       ],
     ),
   ),
   Container(
     color: Colors.deepPurpleAccent,
     child: Column(
       crossAxisAlignment: CrossAxisAlignment.center,
       mainAxisSize: MainAxisSize.max,
       mainAxisAlignment: MainAxisAlignment.spaceEvenly,
       children: <Widget>[
         Image.asset(
           'assets/1.png',
           fit: BoxFit.cover,
         ),
         Padding(padding: const EdgeInsets.all(20.0)),
         Column(
           children: <Widget>[
             new Text(
               "Take a",
               style: TextStyle(
                   fontSize: 30,
                   fontFamily: "Billy",
                   fontWeight: FontWeight.w600),
             ),
             new Text(
               "look at",
               style: TextStyle(
                   fontSize: 30,
                   fontFamily: "Billy",
                   fontWeight: FontWeight.w600),
             ),
             new Text(
               "Liquid Swipe",
               style: TextStyle(
                   fontSize: 30,
                   fontFamily: "Billy",
                   fontWeight: FontWeight.w600),
             ),
           ],
         )
       ],
     ),
   ),
   Container(
     color: Colors.greenAccent,
     child: Column(
       crossAxisAlignment: CrossAxisAlignment.center,
       mainAxisSize: MainAxisSize.max,
       mainAxisAlignment: MainAxisAlignment.spaceEvenly,
       children: <Widget>[
         Image.asset(
           'assets/1.png',
           fit: BoxFit.cover,
         ),
         Padding(padding: const EdgeInsets.all(20.0)),
         Column(
           children: <Widget>[
             new Text(
               "Liked?",
               style: TextStyle(
                   fontSize: 30,
                   fontFamily: "Billy",
                   fontWeight: FontWeight.w600),
             ),
             new Text(
               "Fork!",
               style: TextStyle(
                   fontSize: 30,
                   fontFamily: "Billy",
                   fontWeight: FontWeight.w600),
             ),
             new Text(
               "Give Star!",
               style: TextStyle(
                   fontSize: 30,
                   fontFamily: "Billy",
                   fontWeight: FontWeight.w600),
             ),
           ],
         )
       ],
     ),
   ),
 ];

```


* Second, just pass it to Liquid Swipe Widget.


```
 @override
 Widget build(BuildContext context) {
   return new MaterialApp(
       home: new Scaffold(
           body: LiquidSwipe(
             pages: pages,
             fullTransitionValue: 500,
             enableSlideIcon: true,
           )));
 }

```

*Remember pages can only be containers.
