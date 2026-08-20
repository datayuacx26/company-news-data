---
schema_version: "1.0.0"
document_id: "2a13e399ccadbbfa841f4cc33cc68dd73fa64cf9f3829a87d393388401d07820"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/getting-started-with-superwall-in-your-indie-ios-app"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:9ff432e3ff0ed4e6b7cadd5097d2e3c10d4f66e60b815763178ae9f640f2cb53"
---

# Integrating Superwall in your iOS app

Getting up and running with Superwall is a quick, three step process. As someone who has used countless SDKs, I know that sometimes you simply want a to-the-point walk through of how to get started. If that's you, you're in the right place.


Today, I'm going to take you through those three steps — beginning to end. In a fictional caffeine tracking app, we'll present a paywall when someone taps the blue button to log caffeine. Here's where we'll end up:


Paywall Presentation in Indie iOS App


To get there, we'll need to do the following:


1. Install the Superwall SDK.
2. Configure the SDK in our app.
3. Present a paywall by registering a placement.


Before we get started, I want to point out that this quick start guide does assume two things:


1. That you've already setup your subscriptions in App Store Connect and Superwall.
2. You have a Superwall account.


If you aren't there yet, no worries! Sign up for a free account (you can do that at the bottom of this post), and then follow our guide to setup products. Also, if you'd like to see a sample app with our SDK fully integrated, check out our sample projects.


First up, let's get Superwall into your app!


## Step One: Install the SDK


You can install our SDK using two different methods. We support Swift Package Manager and Cocoapods. I'll show you how to do both below (we also have a video over it you can watch here).


**Using Swift Package Manager:**


1. In Xcode, choose` File -> Add Package Dependencies...` .
2. In the new window, paste this into the search bar you'll see at the top right:` https://github.com/superwall-me/Superwall-iOS` .
3. Select` Superwall-iOS` , and set the dependency rule as` Up To Next Major` Version, and the minimum version to` 4.0.0` .
4. Click` Add Package` .
5. Make sure your app is selected on the left hand side under` Add to Target` . Click` Add Package` again.


**Using Cocoapods:**


1. Ensure your project is setup with Cocoapods first (i.e. you have a podfile already). If you don't — check out their guide here.
2. In your podfile, add Superwall:` pod 'SuperwallKit', '< 5.0.0'` .
3. Then, open Terminal and navigate to the location of your podfile (` $cd documents/my-awesome-project` ).
4. Run` pod repo update` to make sure your local spec repo is up to date.
5. Run` pod install` .
6. Finally, ensure your target's **Build Settings -> User Script Sandboxing** is set to No.


At this point, no matter if you used Swift Package Manager or Cocoapods, Superwall's SDK should be in your project. Take a minute to make sure everything is building just fine. If you've hit a snag, don't hesitate to ask for help.


## Step Two: Configure Superwall


Next, we're going to setup Superwall in our app. We want to initialize Superwall early on in your app's life cycle. Here are some recommended approaches:


- In UIKit, use` application(_:didFinishLaunchingWithOptions:)` inside your application delegate.
- For SwiftUI, use the` init` of your Struct conforming to the` App` protocol works.


Getting Superwall ready is as easy as calling one function and passing in your API key. If you don't know your API key, you can find yours in the Superwall dashboard under` Settings->Keys->Public API Key` .


From there, calling` configure(apiKey:)` is all that we have to do:


```text
import   SwiftUI
import   SuperwallKit


@  main
struct   MyApp:   App   {


init  ()   {
// Use your own API key here
Superwall.  configure  (  apiKey  :   "  YOUR_API_KEY  "  )
}


var   body:   some   Scene {
WindowGroup   {
ContentView  ()
}
}
}
```


Now, we can interact with Superwall to show paywalls and more. Out of the box, we'll handle all of the basic subscription-based logic for you. However, if you need fine grain control or want to use an existing service such as Revenue Cat, we support that too. Check out how to use our` PurchaseController` here.


## Step Three: Present a Paywall


Now comes the fun part — presenting a paywall!


With Superwall, we can certainly show a paywall arbitrarily, from a button tap or anything else. But, I want to quickly explain the methodologies behind our SDK.


With Superwall, paywalls are typically shown based off of placements you create for a given campaign. Each campaign corresponds to one (or several!) paywalls. At its core, a placement is anything in your app that might merit being a paid feature, otherwise known as being "paywalled". For example, in a caffeine tracking app, some placements might be:


1. When a user logs caffeine,` caffeineLogged` .
2. Or, possibly setting a custom icon,` customIconSelected` .


This approach is incredibly flexible, because now we can do things dynamically — such as paywalling a particular feature. Continuing our example, if entering in caffeine should be a pro feature — Superwall can ensure a paywall is shown for non-pro users when they attempt to do some logging. If they are pro, we'll simply log the caffeine:


```text
Button  (  "  Log  "  ) {
Superwall.  shared  .  register  (  placement  :   "  caffeineLogged  "  ) {
store.  log  (  amountToLog  )
}
}
```


If you've followed our Caffeine Pal project from our StoreKit 2 post, you might notice how the logic has become even simpler to paywall features now. Here's what we did using traditional methods, without Superwall:


```text
Button  (  "  Log  "  ) {
if   storefront.hasCaffeinePalPro {
store.  log  (  amountToLog  )
}   else   {
showPaywall.  toggle  ()
}
}
```


Out of the box, we provide a campaign with a corresponding placement (` campaign_trigger` ) to test things out. You can use that for demonstration purposes. However, it's best practice to get your campaigns up and running early — so I would advise setting up a placement that you'll actually use (even when in the testing phase).


## Finishing Up


In just a few steps, we've installed Superwall and presented a remotely configurable paywall. But, cliché as it sounds, that's just the start of what you can do. Want to dig in further? Check these links out:


1. A demo app using Superwall alongside RevenueCat.
2. Learn how to preview paywalls before going live.
3. Performing in-app actions from button taps in your paywalls.
4. Or, visit our documentation or YouTube channel to learn more!


If you got stuck along the way or have any other questions or feedback, reach out on Twitter! I'm more than happy to help, and all of us at Superwall are excited to see what you can do with our SDK.
