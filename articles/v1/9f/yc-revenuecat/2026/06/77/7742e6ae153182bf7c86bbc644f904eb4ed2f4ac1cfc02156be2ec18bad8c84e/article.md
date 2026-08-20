---
schema_version: "1.0.0"
document_id: "7742e6ae153182bf7c86bbc644f904eb4ed2f4ac1cfc02156be2ec18bad8c84e"
company_key: "yc-revenuecat"
company: "RevenueCat"
source_id: "yc-revenuecat-rss-41fa37a4cb36"
canonical_url: "https://www.revenuecat.com/blog/engineering/how-to-build-a-name-your-price-paywall"
published_at: "2026-06-08T13:18:21+00:00"
first_seen_at: "2026-07-20T23:24:09.080214+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:0b9b86b10e422b10a98a2fd671197f11aac88a37688b9eac9d9d3f6b572979a9"
---

# How to build a “name your price” paywall

Paywalls don’t always have to follow the same model, in which customers choose between 2-3 subscription options (e.g., annual or yearly). Sometimes you might want something different. One option could be, for example, “name your price paywall”, where users can choose between differently priced packages, that all unlock the same entitlement:


This is a custom paywall that I built for my app Trippity, which I will be launching (hopefully soon). The reason I built it this way was to offer a single lifetime unlock for features while allowing customers to choose how much they want to support the app. The levels also appear in the app, so later on you can purchase a higher level if you feel like it.


This quick tutorial will look at how to organize your subscriptions to support a “name your price paywall” with five different subscription levels. Code examples are in React Native, but you could easily implement this in any development stack. The main setup takes place in the RevenueCat dashboard or,[if you are using our MCP](https://www.revenuecat.com/docs/tools/mcp) , in your coding agent.


### Use the RevenueCat AI Toolkit to shorten development time


If you’re using an AI coding assistant, you can skip most of the manual setup.[The new RevenueCat AI Toolkit](https://www.revenuecat.com/blog/company/ai-toolkit/) lets agents like Claude Code, Codex, Gemini CLI, and VS Code create products, entitlements, offerings, and SDK integrations directly from prompts.


Install it with:


```text
claude   plugins   marketplace   add   RevenueCat/ai-toolkit


claude   plugins   install   RevenueCat
```


Or, for Codex:


```text
codex   plugin   marketplace   add   RevenueCat/ai-toolkit
```


After authenticating with RevenueCat, you can simply ask your agent to create the products, entitlements, and offering needed for this tutorial. We’ll show both the AI-assisted and manual approaches below.


## How “name your price” slider paywall works


The paywall slider lets users choose how much they feel the premium features are worth while still guiding them toward reasonable price anchors (e.g., $3.99 → $9.99). Because all price points unlock the same entitlement, the technical setup stays simple.


There are three parts to the system:


1. Multiple IAP products in App Store Connect / Google Play Console
Example: support_399, support_599, support_999
2. A slider UI
Each slider position corresponds to a specific product ID
3. A single entitlement in RevenueCat


No matter which price the user selects, they unlock the same features


This model works with both non-consumable in-app purchases as well as subscriptions (“Pay what feels right” kind of a thing). You could even combine these by making the lowest price a subscription. Just remember to keep it obvious what the total price and monthly price are so your app doesn’t get rejected by Apple or Google for deliberately confusing customers.


## Step 1: Create your price-tiered products and entitlement


Our first step is to create the products we will be showing in our custom paywall. In this example, there are five products, with prices ranging from $1.99 to $19.99. I’ll show two ways of doing this: first, through RevenueCat MCP, and then through App Store Connect and importing those products to RevenueCat. The same approach works with the Google Play Store Console.


###
Create products using RevenueCat MCP


Creating 5 different products using RevenueCat MCP is simple. First, see our setup guide for RevenueCat MCP here, and once that is done, run the following prompt in your MCP-connected agent of choice:


## Agent instructions


Using RevenueCat MCP, create 5 non-consumable lifetime in-app purchases in both RevenueCat and App Store Connect. Create a single entitlement called Pro and connect all products to it.


Products:


* Pro Bronze Lifetime — $1.99
* Pro Silver Lifetime — $4.99
* Pro Gold Lifetime — $9.99
* Pro Platinum Lifetime — $14.99
* Pro Diamond Lifetime — $19.99


Requirements:


* All products unlock the same Pro entitlement permanently.
* Users can purchase any tier and receive lifetime Pro access.
* Users may later purchase a higher tier even if they already own a lower tier.
* Product IDs should follow the pattern: pro_bronze_lifetime, pro_silver_lifetime, etc.
* Ensure RevenueCat offerings and App Store Connect products are configured correctly and linked to the Pro entitlement.


Once you run this prompt, you will be potentially asked for access to the MCP. As long as you’ve correctly set up the App Store Connect connection in RevenueCat, products should get created in both places. Alternatively, you can create these products in the RevenueCat Test store if you have not connected App Store Connect to RevenueCat.


### Create products using RevenueCat dashboard


If you’d rather set everything up manually, start by creating your products in App Store Connect (or Google Play Console). Create one product for each price point and give them clear identifiers, such as:


- pro_bronze_lifetime — $1.99
- pro_silver_lifetime — $4.99
- pro_gold_lifetime — $9.99
- pro_platinum_lifetime — $14.99
- pro_diamond_lifetime — $19.99


Since every purchase unlocks the same features, keep the product names and descriptions consistent. The only thing changing is the price. Once the products have been created in your store, import them into RevenueCat from **Product Catalog → Products** .[RevenueCat can automatically import products from App Store Connect and Google Play after you’ve connected your stores.](https://www.revenuecat.com/docs/offerings/products-overview)


Next, create a single entitlement that represents the premium access users receive. Navigate to **Product Catalog → Entitlements** , click **New Entitlement** , and create an entitlement called pro (or any identifier you prefer).


After creating the entitlement, open it and click **Attach** in the Products section. Select all five lifetime products and save. This tells RevenueCat that purchasing any price tier should unlock the same premium access.


Finally, create an Offering (for example, **default** ) and add all five products to it. Your React Native app can then fetch the offering and display the products in the slider UI we’ll build next.


## Step 2: Connect RevenueCat to React Native


Next, we need to add RevenueCat to our app. Once again, I’m going to give you both an agentic way to do this and instructions on how to do it by hand.


### Setup RevenueCat using an agent


If you installed the RevenueCat AI Toolkit earlier, open your React Native project in your coding agent and ask it to wire up the SDK:


## Agent instructions


Using the RevenueCat AI Toolkit, add RevenueCat to this React Native app.


Requirements:


- Install react-native-purchases


- Configure the SDK on app startup


- Use the correct public SDK key for iOS and Android


- Add any required native setup for iOS and Android


- Create a small purchases helper file for fetching offerings, checking the Pro entitlement, restoring purchases, and making purchases


The agent should install the SDK, add the initialization code, and make sure the native project is configured correctly. You may be asked to authenticate with RevenueCat through OAuth before the agent can access your project.


### Set up RevenueCat manually


Install the React Native SDK:


```text
npm   install   --save   react-native-purchases
```


Then configure RevenueCat when your app starts:


```text
import   { useEffect }   from   'react'  ;
import   { Platform }   from   'react-native'  ;
import   Purchases, { LOG_LEVEL }   from   'react-native-purchases'  ;


export   default   function   App  () {
useEffect  (()   =>   {
Purchases.  setLogLevel  (  LOG_LEVEL  .  DEBUG  );   // remove for release


const   apiKey   =   Platform.  OS   ===   'ios'
?   'appl_YOUR_IOS_PUBLIC_SDK_KEY'
:   'goog_YOUR_ANDROID_PUBLIC_SDK_KEY'  ;


Purchases.  configure  ({ apiKey });
}, []);


return   /* … your UI … */  ;
}
```


You can find your public SDK keys in the RevenueCat dashboard under Project Settings → API Keys.


To check whether the user has unlocked your pro entitlement, fetch their CustomerInfo and use it in a custom hook:


```text
import   Purchases   from   'react-native-purchases'  ;


async   function   hasProAccess  () {
const   customerInfo   =   await   Purchases.  getCustomerInfo  ();
return   customerInfo.entitlements.active.pro   !==   undefined  ;
}
```


You can use this anywhere in your app to determine whether premium features should be unlocked. Since all five of our “name your price” products are attached to the same pro entitlement, it doesn’t matter which price tier the user purchased—RevenueCat will grant access in exactly the same way.


With RevenueCat connected and the entitlement configured, we’re ready to fetch our products and build the slider UI.


## Step 3: Fetch the available price tiers


Now that RevenueCat is connected, we can fetch the products that will power the slider. Once again, you can either ask your coding agent to build this for you or implement it manually.


### Build the slider using an agent


If you’re using the RevenueCat AI Toolkit, you can ask your agent:


## Agent instructions


Build a React Native "name your price" paywall using RevenueCat.


Requirements:


- Fetch the current offering from RevenueCat


- Display all available packages as price tiers


- Sort the tiers from lowest to highest price


- Show a slider where each step maps to one package


- Show the selected price above the slider


- Purchase the selected package when the user taps Unlock


- After purchase, check whether the pro entitlement is active


- Include restore purchases


That you should get to the stage where you have a slider. Prompt it to your liking to get the UI you want.


### Code for name your price paywall


Here’s a pretty lengthy code sample that aims to replicate the slider flow you saw in the beginning:


```text
import   { useEffect, useMemo, useState }   from   'react'  ;
import   {
ActivityIndicator,
Alert,
Pressable,
ScrollView,
StyleSheet,
Text,
View,
}   from   'react-native'  ;
import   { LinearGradient }   from   'expo-linear-gradient'  ;
import   Slider   from   '@react-native-community/slider'  ;
import   Purchases, { PurchasesPackage }   from   'react-native-purchases'  ;


const   ENTITLEMENT_ID   =   'pro'  ;


// Each slider position is a gem tier. All tiers map to the SAME entitlement —
// only the color, name, and product ID change.
type   GemTier   =   {
id  :   string  ;
name  :   string  ;
productIdentifier  :   string  ;
color  :   string  ;
gradient  :   [  string  ,   string  ];   // [light, dark] for the card + button
tagline  :   string  ;
level  :   number  ;   // 1...5, drives the dot row
};


const   SHARED_FEATURES   =   [
'See data from all years'  ,
'Remove watermark from sharing'  ,
'Join the leaderboard'  ,
];


const   TIERS  :   GemTier  []   =   [
{ id:   'topaz'  ,    name:   'Topaz'  ,    productIdentifier:   'lifetime_topaz'  ,    color:   '#FFB233'  , gradient: [  '#FFCC4D'  ,   '#FF991A'  ], tagline:   'Essential Explorer'  , level:   1   },
{ id:   'sapphire'  , name:   'Sapphire'  , productIdentifier:   'lifetime_sapphire'  , color:   '#3366FF'  , gradient: [  '#4D80FF'  ,   '#1A4DCC'  ], tagline:   'Sky Master'  ,         level:   2   },
{ id:   'emerald'  ,  name:   'Emerald'  ,  productIdentifier:   'lifetime_emerald'  ,  color:   '#33CC66'  , gradient: [  '#4DE680'  ,   '#1AB34D'  ], tagline:   'Eco Traveler'  ,       level:   3   },
{ id:   'ruby'  ,     name:   'Ruby'  ,     productIdentifier:   'lifetime_ruby'  ,     color:   '#E63350'  , gradient: [  '#FF4D66'  ,   '#CC1A33'  ], tagline:   'Premium Voyager'  ,    level:   4   },
{ id:   'diamond'  ,  name:   'Diamond'  ,  productIdentifier:   'lifetime_diamond'  ,  color:   '#B3CCFF'  , gradient: [  '#FFFFFF'  ,   '#99B3E6'  ], tagline:   'Ultimate Edition'  ,   level:   5   },
];


export   function   NameYourPricePaywall  () {
const   [  selectedIndex  ,   setSelectedIndex  ]   =   useState  (  2  );   // default to middle tier
const   [  prices  ,   setPrices  ]   =   useState  <  Record  <  string  ,   string  >>({});
const   [  packages  ,   setPackages  ]   =   useState  <  PurchasesPackage  []>([]);
const   [  isPurchasing  ,   setIsPurchasing  ]   =   useState  (  false  );


const   selectedTier   =   TIERS  [selectedIndex];
const   isLoaded   =   packages.  length   >   0  ;


useEffect  (()   =>   {
(  async   ()   =>   {
try   {
const   offerings   =   await   Purchases.  getOfferings  ();
const   available   =   offerings.current?.availablePackages   ??   [];
setPackages  (available);


const   priceMap  :   Record  <  string  ,   string  >   =   {};
for   (  const   pkg   of   available) {
priceMap[pkg.product.identifier]   =   pkg.product.priceString;
}
setPrices  (priceMap);
}   catch   (e) {
console.  error  (  'Failed to load offerings'  , e);
}
})();
}, []);


async   function   handlePurchase  () {
const   pkg   =   packages.  find  (
(  p  )   =>   p.product.identifier   ===   selectedTier.productIdentifier
);
if   (  !  pkg) {
Alert.  alert  (  'Unavailable'  ,   'That tier is not available right now.'  );
return  ;
}


setIsPurchasing  (  true  );
try   {
const   {   customerInfo   }   =   await   Purchases.  purchasePackage  (pkg);
if   (customerInfo.entitlements.active[  ENTITLEMENT_ID  ]) {
Alert.  alert  (  'Unlocked'  ,   `You're now a ${  selectedTier  .  name  } member.`  );
}
}   catch   (  e  :   any  ) {
if   (  !  e.userCancelled) {
Alert.  alert  (  'Purchase failed'  ,   'Please try again.'  );
}
}   finally   {
setIsPurchasing  (  false  );
}
}


async   function   restorePurchases  () {
try   {
const   customerInfo   =   await   Purchases.  restorePurchases  ();
Alert.  alert  (
customerInfo.entitlements.active[  ENTITLEMENT_ID  ]   ?   'Restored'   :   'No purchases found'  ,
customerInfo.entitlements.active[  ENTITLEMENT_ID  ]
?   'Your access has been restored.'
:   'We could not find an active purchase.'
);
}   catch   (e) {
console.  error  (  'Restore failed'  , e);
}
}


return   (
// Background gradient tints toward the selected tier's color
<  LinearGradient
colors  =  {[  '#FFFFFF'  ,   `${  selectedTier  .  color  }26`  ,   `${  selectedTier  .  color  }4D`  ]}
style  =  {styles.container}
>
<  ScrollView   contentContainerStyle  =  {styles.scroll}>
{  /* Hero: frequent-flyer style membership card */  }
<  MembershipCard   tier  =  {selectedTier} />


{  /* Gem indicators on a connecting line */  }
<  GemRail   selectedIndex  =  {selectedIndex}   onSelect  =  {setSelectedIndex} />


{  /* Slider — each step is one tier */  }
<  Slider
minimumValue  =  {  0  }
maximumValue  =  {  TIERS  .  length   -   1  }
step  =  {  1  }
value  =  {selectedIndex}
onValueChange  =  {(  v  )   =>   setSelectedIndex  (Math.  round  (v))}
minimumTrackTintColor  =  {selectedTier.color}
thumbTintColor  =  {selectedTier.color}
style  =  {styles.slider}
/>


{  /* Price */  }
<  View   style  =  {styles.priceBlock}>
{prices[selectedTier.productIdentifier]   ?   (
<>
<  View   style  =  {styles.priceRow}>
<  Text   style  =  {[styles.price, { color: selectedTier.color }]}>
{prices[selectedTier.productIdentifier]}
</  Text  >
<  Text   style  =  {[styles.priceTier, { color: selectedTier.color }]}>
{selectedTier.name}
</  Text  >
</  View  >
<  Text   style  =  {styles.priceCaption}>
One-time payment • Lifetime access
</  Text  >
</>
)   :   (
<  ActivityIndicator   />
)}
</  View  >


{  /* Unlock button — gradient matches the tier */  }
<  Pressable
onPress  =  {handlePurchase}
disabled  =  {isPurchasing   ||   !  isLoaded}
style  =  {({   pressed   })   =>   [{ opacity: pressed   ?   0.9   :   1   }]}
>
<  LinearGradient
colors  =  {selectedTier.gradient}
start  =  {{ x:   0  , y:   0   }}
end  =  {{ x:   1  , y:   1   }}
style  =  {styles.unlockButton}
>
{isPurchasing   ?   (
<  ActivityIndicator   color  =  "#fff"   />
)   :   (
<  Text   style  =  {styles.unlockText}>◆  Unlock {selectedTier.name}</  Text  >
)}
</  LinearGradient  >
</  Pressable  >


<  Pressable   onPress  =  {restorePurchases}   style  =  {styles.restore}>
<  Text   style  =  {styles.restoreText}>Restore Purchases</  Text  >
</  Pressable  >


<  Text   style  =  {styles.terms}>
By purchasing, you agree to our Terms of Service and Privacy Policy
</  Text  >
</  ScrollView  >
</  LinearGradient  >
);
}


// MARK: – Membership card


function   MembershipCard  ({   tier   }  :   {   tier  :   GemTier   }) {
return   (
<  LinearGradient
colors  =  {[  `${  tier  .  color  }E6`  ,   `${  tier  .  color  }B3`  ,   `${  tier  .  color  }80`  ]}
start  =  {{ x:   0  , y:   0   }}
end  =  {{ x:   1  , y:   1   }}
style  =  {[styles.card, { shadowColor: tier.color }]}
>
{  /* Metallic sheen overlay */  }
<  LinearGradient
colors  =  {[  'rgba(255,255,255,0.12)'  ,   'transparent'  ,   'rgba(255,255,255,0.05)'  ]}
start  =  {{ x:   0  , y:   0   }}
end  =  {{ x:   1  , y:   1   }}
style  =  {StyleSheet.absoluteFill}
/>


{  /* Header: brand + tier badge */  }
<  View   style  =  {styles.cardHeader}>
<  Text   style  =  {styles.brand}>TRIPPITY</  Text  >
<  View   style  =  {styles.badge}>
<  Text   style  =  {styles.badgeText}>◆ {tier.name.  toUpperCase  ()}</  Text  >
</  View  >
</  View  >


<  Text   style  =  {styles.userName}>Traveler</  Text  >


{  /* Shared features */  }
<  View   style  =  {styles.features}>
{  SHARED_FEATURES  .  map  ((  f  )   =>   (
<  Text   key  =  {f}   style  =  {styles.feature}>✓  {f}</  Text  >
))}
</  View  >


{  /* Footer: level dots + lifetime label */  }
<  View   style  =  {styles.cardFooter}>
<  View   style  =  {styles.dots}>
{[  0  ,   1  ,   2  ,   3  ,   4  ].  map  ((  i  )   =>   (
<  View
key  =  {i}
style  =  {[
styles.dot,
{ backgroundColor: i   <   tier.level   ?   '#fff'   :   'rgba(255,255,255,0.3)'   },
]}
/>
))}
</  View  >
<  Text   style  =  {styles.lifetime}>LIFETIME MEMBER</  Text  >
</  View  >
</  LinearGradient  >
);
}


// MARK: – Gem rail (indicators + connecting line)


function   GemRail  ({
selectedIndex  ,
onSelect  ,
}  :   {
selectedIndex  :   number  ;
onSelect  :   (  i  :   number  )   =>   void  ;
}) {
return   (
<  View   style  =  {styles.rail}>
{  TIERS  .  map  ((  tier  ,   i  )   =>   {
const   isActive   =   i   <=   selectedIndex;
const   isSelected   =   i   ===   selectedIndex;
return   (
<  View   key  =  {tier.id}   style  =  {styles.railSegment}>
<  Pressable   onPress  =  {()   =>   onSelect  (i)}   hitSlop  =  {  12  }>
<  Text
style  =  {[
styles.gem,
{
fontSize: isSelected   ?   26   :   16  ,
color: isActive   ?   tier.color   :   'rgba(150,150,150,0.5)'  ,
textShadowColor: isActive   ?   tier.color   :   'transparent'  ,
textShadowRadius: isSelected   ?   10   :   0  ,
},
]}
>
◆
</  Text  >
</  Pressable  >
{i   <   TIERS  .  length   -   1   &&   (
<  View
style  =  {[
styles.connector,
{
backgroundColor:
i   <   selectedIndex   ?   TIERS  [i   +   1  ].color   :   'rgba(150,150,150,0.3)'  ,
},
]}
/>
)}
</  View  >
);
})}
</  View  >
);
}


const   styles   =   StyleSheet.  create  ({
container: { flex:   1   },
scroll: { padding:   24  , gap:   20   },


card: {
height:   260  ,
borderRadius:   16  ,
padding:   20  ,
justifyContent:   'space-between'  ,
overflow:   'hidden'  ,
shadowOpacity:   0.4  ,
shadowRadius:   20  ,
shadowOffset: { width:   0  , height:   10   },
elevation:   12  ,
},
cardHeader: { flexDirection:   'row'  , justifyContent:   'space-between'  , alignItems:   'center'   },
brand: { color:   'rgba(255,255,255,0.9)'  , fontSize:   14  , fontWeight:   '700'  , letterSpacing:   2   },
badge: {
flexDirection:   'row'  ,
backgroundColor:   'rgba(255,255,255,0.2)'  ,
borderColor:   'rgba(255,255,255,0.3)'  ,
borderWidth:   1  ,
borderRadius:   100  ,
paddingHorizontal:   10  ,
paddingVertical:   4  ,
},
badgeText: { color:   '#fff'  , fontSize:   11  , fontWeight:   '700'  , letterSpacing:   1   },
userName: { color:   '#fff'  , fontSize:   24  , fontWeight:   '700'  , marginTop:   10   },
features: { gap:   6  , marginTop:   8   },
feature: { color:   'rgba(255,255,255,0.9)'  , fontSize:   12  , fontWeight:   '500'   },
cardFooter: { flexDirection:   'row'  , justifyContent:   'space-between'  , alignItems:   'center'   },
dots: { flexDirection:   'row'  , gap:   3   },
dot: { width:   6  , height:   6  , borderRadius:   3   },
lifetime: { color:   'rgba(255,255,255,0.6)'  , fontSize:   8  , fontWeight:   '500'  , letterSpacing:   1   },


rail: { flexDirection:   'row'  , alignItems:   'center'  , paddingHorizontal:   8   },
railSegment: { flexDirection:   'row'  , alignItems:   'center'  , flex:   1   },
gem: { textAlign:   'center'  , width:   30   },
connector: { flex:   1  , height:   3  , borderRadius:   2   },


slider: { marginHorizontal:   8   },


priceBlock: { alignItems:   'center'  , gap:   4   },
priceRow: { flexDirection:   'row'  , alignItems:   'flex-end'  , gap:   6   },
price: { fontSize:   36  , fontWeight:   '800'   },
priceTier: { fontSize:   18  , fontWeight:   '500'  , marginBottom:   4   },
priceCaption: { fontSize:   12  , color:   '#888'   },


unlockButton: { borderRadius:   14  , paddingVertical:   16  , alignItems:   'center'   },
unlockText: { color:   '#fff'  , fontSize:   16  , fontWeight:   '600'   },


restore: { alignItems:   'center'  , paddingTop:   4   },
restoreText: { color:   '#888'  , fontSize:   14   },
terms: { color:   '#aaa'  , fontSize:   11  , textAlign:   'center'  , paddingHorizontal:   24  , paddingBottom:   20   },
});
```


This fetches your current RevenueCat Offering, sorts the available packages by price, and maps each package to a slider step. When the user taps Unlock Pro, the app purchases the currently selected package and then checks whether the pro entitlement is active.


Because every price tier unlocks the same entitlement, the app does not need separate access logic for Bronze, Silver, Gold, Platinum, or Diamond. The selected product only controls how much the user pays.


## Wrapping up


A “name your price” paywall is surprisingly simple to implement. Under the hood, it’s just multiple products linked to the same entitlement, with a slider that lets users choose the level of support that feels right to them. The approach works especially well for indie apps, creator tools, open-source projects, and products where users genuinely want to support ongoing development. Instead of forcing customers into a single price point, you give them flexibility while keeping your purchase logic and entitlement management simple.


Because everything is powered by RevenueCat Offerings, you can also experiment over time. Add or remove price tiers, change your default recommendation, or test entirely different pricing strategies without rebuilding the underlying purchase flow.


If you build one, I’d love to see it. Tag me on Twitter at[@plahteenlahti](http://x.com/plahteenlahti) and let me know what pricing tiers you decided to offer.


### Sources


- [RevenueCat AI Toolkit](https://github.com/RevenueCat/ai-toolkit)
- [RevenueCat React Native SDK installation](https://www.revenuecat.com/docs/getting-started/installation/reactnative)
- [RevenueCat entitlements](https://www.revenuecat.com/docs/getting-started/entitlements)
- [RevenueCat offerings and product configuration](https://www.revenuecat.com/docs/projects/configuring-products)
