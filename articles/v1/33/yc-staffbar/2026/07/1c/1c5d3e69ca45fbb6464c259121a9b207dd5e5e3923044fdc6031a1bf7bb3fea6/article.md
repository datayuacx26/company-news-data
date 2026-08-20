---
schema_version: "1.0.0"
document_id: "1c5d3e69ca45fbb6464c259121a9b207dd5e5e3923044fdc6031a1bf7bb3fea6"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/integrating-superwall-in-your-flutter-app"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:afa2b6885adae43fb9981e9abbd333f80cfc16de9b861e140291b56374d5c0de"
---

# Integrating Superwall in your Flutter App

Integrating Superwall into your Flutter app is a straightforward, three-step process — and it mirrors the experience of adding Superwall to an[iOS](https://superwall.com/blog/getting-started-with-superwall-in-your-indie-ios-app) or React Native project. In this guide, we'll walk through everything you need to get configurable paywalls presenting in just a few minutes.


The three steps are:


1. Installing Superwall's SDK
2. Configuring the Superwall client inside a Flutter project
3. Presenting a paywall by registering a placement


This guide assumes you've already[created a Superwall account](https://superwall.com/sign-up) and added your products from App Store Connect or Google Play. The minimum requirements are iOS 14 and Android SDK version 26.


Simple Flutter Example


## Install the SDK


There are two ways to install the SDK. You can add` superwallkit_flutter` as a dependency in your project's` pubspec.yaml` file:


pubspec.yaml


```text
dependencies  :
flutter  :
sdk  :   flutter
# Add this:
superwallkit_flutter  :   ^2.0.0
```


Then fetch the package via the terminal:


```text
$   cd   documents/myFlutterProjects
$   dart   pub   get
```


Alternatively, you can install it directly from the command line:


```text
$   cd   documents/myFlutterProjects
$   flutter   pub   add   superwallkit_flutter
```


### iOS Deployment Target


Superwall requires iOS 14 or above. Update your project's` podfile` (located at` MyFlutterProject -> ios -> podfile` ) by adding or updating the platform specification:


```text
platform   :  ios  ,   '  14.0  '
```


If your iOS dependencies haven't been installed yet, run:


```text
$   cd   documents/myFlutterProjects/ios
$   pod   install
```


The Xcode project's minimum deployment version should match the` podfile` setting.


Setting Xcode's Deployment Target


### Android Configuration


Android setup requires:


- **Minimum SDK Version:** 26 or higher
- **Compile SDK Target:** 34


Update the` android/app/build.gradle` file:


android/app/build.gradle


```text
android {
...
compileSdkVersion 34
...
defaultConfig {
...
minSdkVersion 26
...
}
}
```


For SDK target 34, the following versions are required:


- **Gradle Version:** 8.6 or higher
- **Android Gradle Plugin Version:** 8.4 or higher


Update` gradle/wrapper/gradle-wrapper.properties` :


gradle/wrapper/gradle-wrapper.properties


```text
distributionUrl  =https\://services.gradle.org/distributions/gradle-8.6-bin.zip
```


Update` android/build.gradle` :


android/build.gradle


```text
plugins {
id 'com.android.application' version '8.4.1' apply false
}
```


### Verify Installation


Build and run the project to ensure everything installed properly:


```text
$   cd   documents/myFlutterProjects
$   flutter   run   ios
```


## Configure Superwall


Open` main.dart` (typically located at` lib -> main.dart` ) and import Superwall:


```text
import   'package:superwallkit_flutter/superwallkit_flutter.dart'  ;
```


Call` configure` on Superwall with platform-specific API keys:


```text
String   apiKey   =   Platform  .isIOS   ?   "MY_IOS_API_KEY"   :   "MY_ANDROID_API_KEY"  ;
Superwall  .  configure  (apiKey);
```


Here's a complete code sample:


```text
import   'dart:io'  ;
import   'package:superwallkit_flutter/superwallkit_flutter.dart'  ;


class   _MyAppState   extends   StatelessWidget   {
@override
void   initState  () {
Superwall  .  configure  (apiKey);
}


// Rest of app code
}
```


Your API keys are available in the Superwall dashboard. Select your iOS or Android app, then navigate to` Settings -> Keys -> Public API Key` .


## Present a Paywall


Register a placement with Superwall to trigger a paywall. By default, campaigns include a built-in placement called` campaign_trigger` :


```text
void   registerDemoPlacement  () {
Superwall  .shared.  registerPlacement  (  'campaign_trigger'  , feature  :   () {
// access pro feature
});
}
```


When the button is tapped, a default paywall will display.


Showing a Paywall


## Complete Example


Here is the entire source code for the project's` main.dart` file:


main.dart


```text
import   'package:flutter/material.dart'  ;
import   'package:superwallkit_flutter/superwallkit_flutter.dart'  ;
import   'dart:io'  ;


void   main  () {
runApp  (  const   MyApp  ());
}


class   MyApp   extends   StatefulWidget   {
const   MyApp  ({  super  .key});


@override
_MyAppState   createState  ()   =>   _MyAppState  ();
}


class   _MyAppState   extends   State  <  MyApp  > {
@override
void   initState  () {
super  .  initState  ();
Superwall  .  configure  (apiKey);
}


void   registerDemoPlacement  () {
// Access pro
});
}


@override
Widget   build  (  BuildContext   context) {
return   MaterialApp  (
home  :   Scaffold  (
body  :   Center  (
child  :   Column  (
mainAxisAlignment  :   MainAxisAlignment  .center,
children  :   [
ElevatedButton  (
onPressed  :   registerDemoPlacement,
child  :   const   Text  (  'Present Paywall'  ),
),
],
),
),
),
);
}
}
```


## Understanding Placements


Superwall paywalls are triggered through specific actions called **placements** . These markers indicate features that require a subscription or "pro" entitlement to access.


Paywalls are displayed based on placements tied to campaigns, and each campaign can have multiple paywalls linked to it. In a caffeine-tracking app, for example, placements might include:


1. When caffeine is logged:` caffeineLogged`
2. When selecting a custom icon:` customIconSelected`


This approach gives you the flexibility to remotely control feature access and paywalls without requiring app updates. You can pause placements, make paywalls "non-gated," and set up complex filters and rules to determine when placements should trigger.


When you create an app in Superwall, an example campaign and placement are provided as a foundation. Additional details are available in the[official documentation](https://superwall.com/docs) and on our YouTube channel.


## Conclusion


That's all it takes — minimal configuration to integrate Superwall into your Flutter project. If you have any questions while implementing it, support is available through our official channels and on social media.


## FAQ


How do I add the Superwall SDK to a Flutter app? Add superwallkit_flutter as a dependency in your pubspec.yaml and run \`dart pub get\`, or install it directly with \`flutter pub add superwallkit_flutter\`. Then set your iOS deployment target to 14.0 in the podfile and configure Android with a minimum SDK version of 26 and a compile SDK target of 34.


What are the minimum platform requirements for Superwall on Flutter? Superwall on Flutter requires iOS 14 or above and Android SDK version 26 or higher, with a compile SDK target of 34. For SDK target 34 you also need Gradle 8.6+ and the Android Gradle Plugin 8.4+.


How do I present a paywall in Flutter? Register a placement with Superwall.shared.registerPlacement, passing a placement name (campaigns include a built-in campaign_trigger placement by default) and a feature closure. When the placement is registered, Superwall decides whether to show a paywall based on your campaign rules.


What is a placement in Superwall? A placement is a named action in your app that can trigger a paywall, such as caffeineLogged or customIconSelected. Placements are tied to campaigns, letting you control which features are gated and when paywalls appear — all remotely, without shipping an app update.
