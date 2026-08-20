---
schema_version: "1.0.0"
document_id: "49bef43bccab690baaa5ea4063b0928e132b21889b1712d5448e8fd8cd3080bb"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/consumable-purchases-with-the-superwall-sdk"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:abde57b4110d274d8d24c409a932d46f9dfb9b92a9eb5512792de3ea9d76982b"
---

# Consumable Purchases with the Superwall SDK

## Overview


Superwall's SDK extends beyond subscription models to support consumable products such as game coins and app credits. By default, the SDK treats consumable purchases identically to subscriptions when purchased:


```text
// This will be true when a consumable, or non-consumable, is purchased
if   Superwall.shared.subscriptionStatus   ==   .active {
showSubscribedAccountView  ()
}
```


If this behavior suits your needs, no additional implementation is required.


## Customizing Paywall Presentation


There are two approaches when you need different behavior:


**Option 1: Always Display Paywalls**


Configure your paywall to always display regardless of purchase history. Navigate to **Sidebar -> Settings** and set the "Present Paywall" option to "Always".


Setting the presentation option for a paywall


**Option 2: Manual Subscription Status Management**


For more complex logic, implement a purchase controller to manage subscription states manually. This approach requires creating a` PurchaseController` that handles purchasing, restoration, and subscription status updates.


## Creating a Purchase Controller


Adopt the` PurchaseController` protocol and implement required functions:


```text
class   PurchaseManager:   PurchaseController   {
func   purchase  (  product  : SKProduct  )   async   ->   SuperwallKit.PurchaseResult {
// Purchase a product
}


func   restorePurchases  ()   async   ->   SuperwallKit.RestorationResult {
// Restore one
}
}
```


### Sample Implementation


```text
class   PurchasesManager:   PurchaseController   {
let   result   =   await   SomeProductPurchaseUtil.  purchase  (  product  )


switch   result {
case   .  success  :
return   .  purchased
case   .  userCancelled  :
return   .  cancelled
case   .  pendingPurchase  :
return   .  pending
case   .  alreadyPurchased  :
return   .  restored
case   .  failed  (  let   error  )  :
return   .  failed  (  error  )
}
}


func   restorePurchases  ()   async   ->   SuperwallKit.RestorationResult {
do   {
try   await   AppStore.  sync  ()
try   await   updateUserPurchases  ()
return   .  restored
}   catch   {
return   .  failed  (  error  )
}
}
}
```


## Handling Subscription States


Monitor purchase and subscription changes from your source of truth, then update Superwall accordingly:


```text
class   PurchasesManager:   PurchaseController   {
let   purchaseListener   =   PurchaseListener  ()


init  ()   {
purchaseListener.  subscriptionStatusChanged   { status   in
switch   status {
case   .  onlyConsumables  :
Superwall.  shared  .  subscriptionStatus   =   .  inactive
case   .  purchaseSubscription  , purchasedBoth  :
Superwall.  shared  .  subscriptionStatus   =   .  active
}
}
}


let   result   =   await   ProductPurchase.  purchase  (  product  )


switch   result {
case   .  success  (  let   result  )  :
return   .  purchased
case   .  userCancelled  :
return   .  cancelled
case   .  pending  :
return   .  pending
@  unknown   default  :
fatalError  ()
}
}


func   restorePurchases  ()   async   ->   SuperwallKit.RestorationResult {
do   {
try   await   AppStore.  sync  ()
try   await   updateUserPurchases  ()
return   .  restored
}   catch   {
return   .  failed  (  error  )
}
}
}
```


Maintaining accurate subscription status is critical, as it affects paywall presentation and other SDK functionality.


## SDK Initialization


Pass your purchase controller during Superwall initialization:


```text
@  main
struct   MyApp:   App   {
let   purchaseUtil: PurchasesManager   =   .  init  ()


init  ()   {
Superwall.  configure  (  apiKey  :   "  YourAPIKey  "  ,
purchaseController  : purchaseUtil  )
}


var   body:   some   Scene {
WindowGroup   {
ContentView  ()
}
}
}
```


## Summary


Superwall supports flexible monetization strategies beyond subscriptions, including consumable and non-consumable products. By implementing a purchase controller, you maintain complete control over purchasing and subscription state management, enabling diverse revenue models tailored to your app's needs.
