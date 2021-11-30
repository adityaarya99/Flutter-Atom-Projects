# INAPP PURCHASE (IOS) 


Getting Started 
Include the package:

```
  in_app_purchase: <latest_version>
 ```
 ```
  intl: <latest_version>
 ```
 
 ```
 STEP 1 : Select Manange & Add In App Purchase
 ```
 <img width="1438" alt="appstore_manage" src="https://user-images.githubusercontent.com/30201421/144039083-4b80a09e-6533-4b37-a895-7e8fc859a073.png">


```
 STEP 2 : Select the in-app purchase you want to create:
 ```
<img width="709" alt="subscription_type" src="https://user-images.githubusercontent.com/30201421/144039140-aeee2e51-3d26-4f43-b42c-e2571753352b.png">
 
```
STEP 3 : Create Subscription
 ```
<img width="594" alt="product_id" src="https://user-images.githubusercontent.com/30201421/144039200-123b8f81-5c63-4695-b517-b6283d942dda.png">

```
Step 4 : Add to Subscription Group
```
<img width="597" alt="subscription_group" src="https://user-images.githubusercontent.com/30201421/144039271-cf5e0417-fc66-47fe-9415-a075ffe2852f.png">

```
Final Result : After adding the main code to project
```
<img width="316" alt="Device_view" src="https://user-images.githubusercontent.com/30201421/144039321-f3a51fce-8f67-4330-bd89-b8b59c29b852.png">

```
- Do Remember to add Email in Sandbox Testing in User & Access section in App Store connect
- Upload new version of build to app store & then Run the application.
```

```
import 'dart:async';

import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';
import 'package:in_app_purchase/in_app_purchase.dart';
import 'package:intl/intl.dart';

class DemoPage extends StatefulWidget {
  @override
  _DemoPageState createState() => _DemoPageState();
}

class _DemoPageState extends State<DemoPage> {
  final InAppPurchase _inAppPurchase = InAppPurchase.instance;
  ///Todo : Add your product id here
  final String _productID = 'GOLD_PLAN';
  final String _productID1 = 'SILVER_PLAN';
  final String _productID2 = 'BRONZE_PLAN';

  bool _available = true;
  List<ProductDetails> _products = [];
  List<PurchaseDetails> _purchases = [];
  StreamSubscription<List<PurchaseDetails>>? _subscription;

  @override
  void initState() {
    final Stream<List<PurchaseDetails>> purchaseUpdated =
        _inAppPurchase.purchaseStream;

    _subscription = purchaseUpdated.listen((purchaseDetailsList) {
      setState(() {
        _purchases.addAll(purchaseDetailsList);
        _listenToPurchaseUpdated(purchaseDetailsList);
      });
    }, onDone: () {
      _subscription!.cancel();
    }, onError: (error) {
      _subscription!.cancel();
    });

    _initialize();

    super.initState();
  }

  @override
  void dispose() {
    _subscription!.cancel();
    super.dispose();
  }

  void _initialize() async {
    _available = await _inAppPurchase.isAvailable();

    List<ProductDetails> products = await _getProducts(
      productIds: Set<String>.from(
        [_productID,_productID1,_productID2],
      ),
    );

    setState(() {
      _products = products;
    });
  }

  void _listenToPurchaseUpdated(List<PurchaseDetails> purchaseDetailsList) {
    purchaseDetailsList.forEach((PurchaseDetails purchaseDetails) async {
      switch (purchaseDetails.status) {
        case PurchaseStatus.pending:
        //  _showPendingUI();
          break;
        case PurchaseStatus.purchased:
        case PurchaseStatus.restored:
        // bool valid = await _verifyPurchase(purchaseDetails);
        // if (!valid) {
        //   _handleInvalidPurchase(purchaseDetails);
        // }
          break;
        case PurchaseStatus.error:
          print(purchaseDetails.error!);
          // _handleError(purchaseDetails.error!);
          break;
        default:
          break;
      }

      if (purchaseDetails.pendingCompletePurchase) {
        await _inAppPurchase.completePurchase(purchaseDetails);
      }
    });
  }

  Future<List<ProductDetails>> _getProducts(
      {required Set<String> productIds}) async {
    ProductDetailsResponse response =
    await _inAppPurchase.queryProductDetails(productIds);

    return response.productDetails;
  }

  ListTile _buildProduct({required ProductDetails product}) {
    return ListTile(
      leading: Icon(Icons.attach_money),
      title: Text('${product.title} - ${product.price}'),
      subtitle: Text(product.description),
      trailing: ElevatedButton(
        onPressed: () {
          _subscribe(product: product);
        },
        child: Text(
          'Subscribe',
        ),
      ),
    );
  }

  ListTile _buildPurchase({required PurchaseDetails purchase}) {
    if (purchase.error != null) {
      return ListTile(
        title: Text('${purchase.error}'),
        subtitle: Text(purchase.status.toString()),
      );
    }

    String? transactionDate;
    if (purchase.status == PurchaseStatus.purchased) {
      DateTime date = DateTime.fromMillisecondsSinceEpoch(
        int.parse(purchase.transactionDate!),
      );
      transactionDate = ' @ ' + DateFormat('yyyy-MM-dd HH:mm:ss').format(date);
    }

    return ListTile(
      title: Text('${purchase.productID} ${transactionDate ?? ''}'),
      subtitle: Text(purchase.status.toString()),
    );
  }

  void _subscribe({required ProductDetails product}) {
    final PurchaseParam purchaseParam = PurchaseParam(productDetails: product);
    _inAppPurchase.buyNonConsumable(
      purchaseParam: purchaseParam,
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('IOS In App Purchase 1.0.9'),
      ),
      body: _available
          ? Column(
        children: [
          Expanded(
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                Text('Current Products ${_products.length}'),
                ListView.builder(
                  shrinkWrap: true,
                  itemCount: _products.length,
                  itemBuilder: (context, index) {
                    return _buildProduct(
                      product: _products[index],
                    );
                  },
                ),
              ],
            ),
          ),
          Expanded(
            child: Column(
              // mainAxisAlignment: MainAxisAlignment.center,
              children: [
                Text('Past Purchases: ${_purchases.length}'),
                Expanded(
                  child: ListView.builder(
                    shrinkWrap: true,
                    itemCount: _purchases.length,
                    itemBuilder: (context, index) {
                      return _buildPurchase(
                        purchase: _purchases[index],
                      );
                    },
                  ),
                ),
              ],
            ),
          ),
        ],
      )
          : Center(
        child: Text('The Store Is Not Available'),
      ),
    );
  }
}

```
