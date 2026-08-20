---
schema_version: "1.0.0"
document_id: "b2b4628d30c955031c14387bd1ad4a26d036e9e1720b1ef667b1e4ef9f78dff7"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/increase-revenue-with-superwalls-expo-sdk"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:d86ad6bd6f8710648b0e61a8370e3e0bfafd02270edf24685684c3c04e73fb8a"
---

# Increase Revenue with Superwall's Expo SDK

Expo has become the go-to choice for many React Native developers. At Superwall, we wanted to make sure we had an experience that fits right into its ecosystem, and that's what we've delivered with our[Superwall Expo SDK](https://superwall.com/docs/expo) . If you're building with Expo SDK 53 or later, it all just works.


The result? You can go from zero to paywall in about two minutes.


- 🎥 Prefer a video? Check out our[YouTube tutorial here](https://youtu.be/YY1TDn7sgYI) .
- 📖 Check out our[documentation here](https://superwall.com/docs/expo) .


## Get started in three steps


Getting Superwall running in your Expo app takes three quick steps. In these steps, I'm assuming you've already got an Expo project up and running. If you don't, the Expo team has an incredible tutorial that'll walk you through that[right here](https://docs.expo.dev/tutorial/create-your-first-app/) .


### Step 1: Install the SDK


```text
//   bun
bunx   expo   install   expo-superwall


//   pnpm
pnpm   dlx   expo   install   expo-superwall


//   npm
npx   expo   install   expo-superwall


//   yarn
yarn   dlx   expo   install   expo-superwall
```


### Step 2: Configure your build settings


The Superwall Expo SDK works with iOS 14.0 or higher, and Android SDK 26 or higher. So next, grab` expo-build-properties` if you don't have it:


```text
npx   expo   install   expo-build-properties
```


Then, update your` app.json` or` app.config.js` :


```text
{
"expo"  : {
"plugins"  : [
[
"  expo-build-properties  "  ,
{
"android"  : {
"minSdkVersion"  :   26
},
"ios"  : {
"deploymentTarget"  :   "  14.0  "
}
}
]
]
}
}
```


### Step 3: Initialize Superwall


Wrap your app (or the relevant parts of it) with the` <SuperwallProvider />` and drop in your API key:


```text
import   { SuperwallProvider }   from   "  expo-superwall  "  ;


export   default   function   App  ()   {
return   (
<  SuperwallProvider   apiKeys  =  {  { ios:   "  YOUR_API_KEY  "  ,   android:   "  YOUR_API_KEY  "   }  }  >
{  /* Your app goes here */  }
</  SuperwallProvider  >
);
}
```


That's it! Note that if you hit any issues around missing a module, you may need to run` npx expo prebuild` first.


Now, you're ready to start triggering paywalls and processing purchases. Here's what that could look like:


```text
function   PaywallScreen  ()   {
const   {   registerPlacement  ,   state  :   placementState   }   =   usePlacement  (  {
onError  :   (  err  )   =>   console  .  error  (  "  Placement Error:  "  ,   err  )  ,
onPresent  :   (  info  )   =>   console  .  log  (  "  Paywall Presented:  "  ,   info  )  ,
onDismiss  :   (  info  ,   result  )   =>
console  .  log  (  "  Paywall Dismissed:  "  ,   info  ,   "  Result:  "  ,   result  )  ,
}  );
const   handleTriggerPlacement   =   async   ()   =>   {
await   registerPlacement  (  {
placement  :   "  campaign_trigger  "
}  )  ;
}  ;
return   (
<  View   style  =  {  { padding:   20   }  }  >
<  Button   title  =  "  Show Paywall  "   onPress  =  {  handleTriggerPlacement  }   />
{  placementState   &&   (
<  Text  >  Last Paywall Result:   {  JSON  .  stringify  (  placementState  )  }  </  Text  >
)  }
</  View  >
);
}
```


It's that easy to get up and running with Superwall in your Expo app. Now, you're ready to experiment and leverage the full Superwall platform in your React Native app. Kick off some price tests, try running some A/B tests with paywalls, and more. If you need some inspiration on what to start testing, check out our[in-depth blog post covering testing methodologies](https://superwall.com/blog/how-we-test-paywalls-at-superwall-and-how-you-can-too) .


## Wrapping up


The Expo SDK is live and ready to use today. Check out the[full documentation](https://superwall.com/docs/expo) to see everything you can do, or dive into the[example app](https://github.com/superwall/expo-superwall) to see it in action.


If you're new to Superwall, grab a[free account](https://superwall.com/sign-up) first. And if you hit any snags,[open an issue on GitHub](https://github.com/superwall/expo-superwall/issues) ; we're always improving the SDK and we're committed to making it the best way to add paywalls to your Expo app.
