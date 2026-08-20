---
schema_version: "1.0.0"
document_id: "b6307081391061d4e3038fca5fc9ed09750b77c4b2fcb2d49c12aace95183da0"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/integrating-superwall-in-your-indie-react-native-app"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:7975f3a6085ee0280866113df133d596bdc5278124d4151a7cd520af5cadc379"
---

# Integrating Superwall in your React Native app

**Setting up Superwall in React Native takes no time at all. Get started with paywall experiments in a few minutes.**


Superwall's React Native SDK is ready to go! In this post, I'll show how to integrate Superwall in your Expo project. Essentially, this is a three-step process. If you've installed Superwall in iOS for example, a lot of this will be familiar.


Those three steps are:


1. Installing Superwall's SDK.
2. Configuring the Superwall client inside your Expo project.
3. And finally, presenting a paywall by registering a placement.


Before we dive in, this post assumes that you've already signed up with Superwall and added your respective products from App Store Connect or Google Play. If you need help with either of those,[sign up for a free account](https://superwall.com/sign-up) or learn how to[add products into Superwall](https://superwall.com/docs/products) .


Finally, our React Native's minimum SDK support as of this writing is iOS 14, and SDK version 26 for Android.


## Install the SDK


To install Superwall, use your preferred package manager. First, navigate to your Expo project (i.e.` cd documents/amazingapp` ) and then use either:


1. bun:` bunx expo install expo-superwall`
2. pnpm:` pnpn dlx expo install expo-superwall`
3. npm:` npmx expo install expo-superwall`
4. yarn:` yarn dlx expo install expo-superwall`


If your Expo project doesn't have` expo-build-properties` , install that next:` npx expo install expo-build-properties`


In your` app.json` or` app.config.js` , add the following:


```text
{
"  expo  "  : {
"  plugins  "  : [
...
[
"  expo-build-properties  "  ,
{
"  android  "  : {
"  minSdkVersion  "  :   26
},
"  ios  "  : {
"  deploymentTarget  "  :   "  14.0  "   // or higher
}
}
]
]
}
}
```


If you only are targeting iOS, then you're ready for the next step. If you're also developing for Android, then be sure to also add Superwall's Maven repository:


```text
// In your app's android/build.gradle...
allprojects {
repositories {
maven { url   '  https://mvn.superwall.com/release  '   }
}
}
```


Now is a good time to check that everything is working. Make sure there are no errors after installing the SDK and that your app still runs just fine.


## Configure Superwall


Now we can initialize the Superwall client. To use the Superwall SDK, you need to wrap your application (or the relevant part of it) with the` SuperwallProvider` . This provider initializes the SDK with your API key:


```text
import   { SuperwallProvider }   from   "  expo-superwall  "  ;


// Replace with your actual Superwall API key
export   default   function   App  ()   {
return   (
<  SuperwallProvider   apiKeys  =  {  { ios:   "  YOUR_SUPERWALL_API_KEY  "   /* android: API_KEY */   }  }  >
{  /* Your app content goes here */  }
</  SuperwallProvider  >
);
}
```


If you aren't sure of what your API keys are, just go to Superwall's dashboard and choose either your iOS or Android app that was created for your React Native project. Then go to` Settings -> Keys -> Public API Key` to copy your app's key.


## Present a paywall


Okay, now we're all setup to present paywalls!


In my demo app, I want a paywall to show when someone tries to log caffeine and they aren't on a pro plan:


```text
import   { usePlacement, useUser }   from   "  expo-superwall  "  ;
import   { Alert, Button, Text, View }   from   "  react-native  "  ;


function   PaywallScreen  ()   {
const   {   registerPlacement  ,   state  :   placementState   }   =   usePlacement  (  {
onError  :   (  err  )   =>   console  .  error  (  "  Placement Error:  "  ,   err  )  ,
onPresent  :   (  info  )   =>   console  .  log  (  "  Paywall Presented:  "  ,   info  )  ,
onDismiss  :   (  info  ,   result  )   =>
console  .  log  (  "  Paywall Dismissed:  "  ,   info  ,   "  Result:  "  ,   result  )  ,
}  );


const   handleTriggerPlacement   =   async   ()   =>   {
await   registerPlacement  (  {
placement  :   "  log_caffeine  "
}  )  ;
}  ;


return   (
<  View   style  =  {  { padding:   20   }  }  >
<  Button   title  =  "  Show Paywall  "   onPress  =  {  handleTriggerPlacement  }   />
{  placementState   &&   (
<  Text  >  Last Paywall Result:   {  JSON  .  stringify  (placementState)  }  </  Text  >
)  }
</  View  >
);
}
```


Now, when I run my app and tap on the blue button to log some caffeine — a paywall is shown! Otherwise, the caffeine logging screen will be presented.


React Native Paywall presenting on iOS


If you're new to Superwall, you may be wondering how this is working. Read on for a quick explanation, or if you're a Superwall veteran — then you're all set!


## Understanding Placements


With Superwall, we can present paywalls from button taps or any other user action. That said, it helps to understand how paywalls should be handled using our SDK.


Paywalls are typically shown based off of *placements* you create for a *campaign* . Each campaign has one (or more!) paywalls. Placements can represent anything in an app that should be a paid feature or interaction, otherwise known as being "paywalled". For example, in a caffeine tracking app, some placements might be:


1. When a user logs caffeine,` caffeineLogged` .
2. Or, possibly setting a custom icon,` customIconSelected` .
3. And so on...


This approach is flexible, because it'll allow you to dynamically feature gate things remotely — all without requiring an app update. You can create advanced filters and rules for when these placements should be evaluated, assign different paywalls for them and more.


Superwall will create an example campaign and a corresponding placement for you when you create an app. So, feel free to use those to get started. And remember, this is just the start, be sure to visit our[documentation](https://superwall.com/docs) or[YouTube channel](https://www.youtube.com/@SuperwallSDK) to learn more!


## Finishing up


And just like, we've installed Superwall and presented a remotely configurable paywall in React Native. If you had any issues with your setup, feel free to reach out to us via Superwall's support channels or ping me on Twitter. We're all happy to help, and we can't wait to see what developers can do with our React Native SDK!
