# async_button


Getting Started 
Include the package:

```
  async_button_builder: <latest_version>
 ```


```
AsyncButtonBuilder(
  child: Text('Click Me'),
  onPressed: () async {
    await Future.delayed(Duration(seconds: 1));
  },
  builder: (context, child, callback, _) {
    return TextButton(
      child: child,
      onPressed: callback,
    );
  },
),
```


  ![alt text](https://github.com/Nolence/async_button_builder/blob/main/screenshots/ezgif-7-61c436edaec2.gif?raw=true)
  
  ```
  AsyncButtonBuilder(
  child: Text('Click Me'),
  loadingWidget: Text('Loading...'),
  onPressed: () async {
    await Future.delayed(Duration(seconds: 1));

    // See the examples file for a way to handle timeouts
    throw 'shucks';
  },
  builder: (context, child, callback, buttonState) {
    final buttonColor = buttonState.when(
      idle: () => Colors.yellow[200],
      loading: () => Colors.grey,
      success: () => Colors.orangeAccent,
      error: () => Colors.orange,
    );

    return OutlinedButton(
      child: child,
      onPressed: callback,
      style: OutlinedButton.styleFrom(
        primary: Colors.black,
        backgroundColor: buttonColor,
      ),
    );
  },
),
```
  
  
  ![alt text](https://github.com/Nolence/async_button_builder/blob/main/screenshots/ezgif-7-a971c6afaabf.gif?raw=true)
    
  ```  
  AsyncButtonBuilder(
  child: Padding(
    padding: const EdgeInsets.symmetric(
      horizontal: 16.0,
      vertical: 8.0,
    ),
    child: Text(
      'Click Me',
      style: TextStyle(color: Colors.white),
    ),
  ),
  loadingWidget: Padding(
    padding: const EdgeInsets.all(8.0),
    child: SizedBox(
      height: 16.0,
      width: 16.0,
      child: CircularProgressIndicator(
        valueColor: AlwaysStoppedAnimation<Color>(Colors.white),
      ),
    ),
  ),
  successWidget: Padding(
    padding: const EdgeInsets.all(4.0),
    child: Icon(
      Icons.check,
      color: Colors.purpleAccent,
    ),
  ),
  onPressed: () async {
    await Future.delayed(Duration(seconds: 2));
  },
  loadingSwitchInCurve: Curves.bounceInOut,
  loadingTransitionBuilder: (child, animation) {
    return SlideTransition(
      position: Tween<Offset>(
        begin: Offset(0, 1.0),
        end: Offset(0, 0),
      ).animate(animation),
      child: child,
    );
  },
  builder: (context, child, callback, state) {
    return Material(
      color: state.maybeWhen(
        success: () => Colors.purple[100],
        orElse: () => Colors.blue,
      ),
      // This prevents the loading indicator showing below the
      // button
      clipBehavior: Clip.hardEdge,
      shape: StadiumBorder(),
      child: InkWell(
        child: child,
        onTap: callback,
      ),
    );
  },
),
```
   
  ![alt text](https://github.com/Nolence/async_button_builder/blob/main/screenshots/ezgif-7-61c436edaec2.gif?raw=true)
