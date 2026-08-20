---
schema_version: "1.0.0"
document_id: "878f411edb3fcda280f4cb7ce2154c88522ae6b993fae2ef7017812ec79c6a1b"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/implementing-ppp-in-the-boring-javascript-stack/"
published_at: "2026-01-05T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T20:54:56.230774+00:00"
content_hash: "sha256:f5015445d3c4bd2b530389d56bd0eec848c28d340c26dcb78d9a6f05c09b8b4e"
---

# Implementing Purchasing Power Parity (PPP) in The Boring JavaScript Stack

Purchasing Power Parity (PPP) is an economic principle that accounts for the relative cost of living and inflation rates between countries. In simpler terms, $100 in the United States doesn’t have the same buying power as $100 converted to Nigerian Naira or Indian Rupees, even after exchange rates.


For digital product creators, this creates a dilemma. A course priced at $99 might be reasonable for someone in San Francisco, but that same price could represent a week’s salary for a developer in Lagos or Nairobi. By implementing PPP-adjusted pricing, you make your products accessible globally while still capturing revenue you’d otherwise lose entirely.


The business case is compelling too. PPP pricing reduces piracy (people pay what they can afford rather than pirating), cuts down on credit card fraud, and ultimately increases sales. You’d rather make $15 from a legitimate customer than $0 from piracy or lose money to chargebacks.


When we added PPP to[The African Engineer](https://africanengineer.com/) , we got our first annual subscription almost immediately. The price was discounted at 85% based on the user’s location, and suddenly what seemed expensive became comfortable for them.


In this post, I’ll show you exactly how to implement PPP pricing in The Boring JavaScript Stack.


## The Architecture


Our PPP system has four parts:


1. **Region Configuration** : JSON files with PPP data for each country
2. **PPP Helpers** : Sails helpers to detect location and fetch PPP data
3. **Pricing Controller** : Calculate and display adjusted prices
4. **Payment Integration** : Support multiple providers per currency


Let’s build each piece.


## Step 1: Region Configuration


Create a` config/regions` directory with JSON files for each supported country:


```text
config/
regions/
us.json
ng.json
ke.json
gh.json
za.json
```


Each file contains PPP data:


```text
// config/regions/ng.json
{
"countryCode"  :   "NG"  ,
"currency"  :   "USD"  ,
"currencySymbol"  :   "$"  ,
"localCurrency"  :   "NGN"  ,
"localCurrencySymbol"  :   "₦"  ,
"pppConversionFactor"  :   0.12  ,
"exchangeRate"  :   1438.97  ,
"discount"  :   0.88
}
```


The` pppConversionFactor` is key. It represents what fraction of the USD price someone in that country should pay. A factor of` 0.12` means 12% of the original price, or an 88% discount.


The baseline (US) has a factor of 1:


```text
// config/regions/us.json
{
"countryCode"  :   "US"  ,
"currency"  :   "USD"  ,
"currencySymbol"  :   "$"  ,
"localCurrency"  :   "USD"  ,
"localCurrencySymbol"  :   "$"  ,
"pppConversionFactor"  :   1  ,
"exchangeRate"  :   1  ,
"discount"  :   0
}
```


### How do you calculate the discount?


We use the[World Bank PPP API](https://data.worldbank.org/indicator/PA.NUS.PPP) to calculate our conversion factors. The World Bank provides PPP conversion factors that represent the number of units of a country’s currency required to buy the same amount of goods and services in the domestic market as a U.S. dollar would buy in the United States.


Here’s the formula we use:


```text
pppConversionFactor = Country's PPP conversion factor / US PPP conversion factor
discount = 1 - pppConversionFactor
```


For example, if Nigeria’s PPP conversion factor is 148.55 and the exchange rate is 1438.97 NGN/USD:


```text
pppConversionFactor = 148.55 / 1438.97 ≈ 0.12
discount = 1 - 0.12 = 0.88 (88% discount)
```


This means a $99 product would cost $12 for someone in Nigeria, a price that reflects actual purchasing power rather than just currency conversion.


Other resources you can use:


- [Big Mac Index](https://www.economist.com/big-mac-index) : The Economist’s fun-but-useful index based on burger prices globally
- [Spotify Pricing](https://mts.io/projects/spotify-pricing/) : See how Spotify adjusts prices across countries for reference


## Step 2: PPP Configuration


Add PPP settings to your config:


```text
// config/ppp.js
module  .  exports  .ppp   =   {
enabled:   true  ,


// Data sources for updating region files
dataSources: {
worldBankApi:   'https://api.worldbank.org/v2/country/{country}/indicator/PA.NUS.PPP'  ,
exchangeRateApi:   'https://api.exchangerate.fun/latest'
},


// Supported currencies
currencies: [  'USD'  ,   'NGN'  ],


// Payment providers by currency
providersFor: {
USD: [  'lemonsqueezy'  ],
NGN: [  'paystack'  ]
}
}
```


## Step 3: Location Detection Helper


To apply PPP pricing, we need to know where the user is located. Our approach uses a layered detection strategy:


1. First, check for CDN-injected headers (Cloudflare, Vercel, or custom)
2. If no headers, fall back to IP geolocation using` geoip-lite`
3. Default to a sensible fallback for local development


Install the required packages:


```text
npm   install   request-ip   geoip-lite
```


Here’s the helper:


```text
// api/helpers/ppp/get-country-from-request.js
const   requestIp   =   require  (  'request-ip'  )
const   geoip   =   require  (  'geoip-lite'  )


module  .  exports   =   {
sync:   true  ,
friendlyName:   'Get country from request'  ,
description:   'Detect the country code from the request IP address'  ,


inputs: {
req: {
type:   'ref'  ,
description:   'The current incoming request (req)'  ,
required:   true
}
},


exits: {
success: {
outputFriendlyName:   'Country code'  ,
outputDescription:   'The ISO Alpha-2 country code (e.g., NG, US, KE)'  ,
outputType:   'string'
}
},


fn  :   function   ({   req   }) {
// Check CDN headers first (fastest, most reliable)
const   countryCode   =
req.headers[  'cf-ipcountry'  ]   ||
req.headers[  'x-vercel-ip-country'  ]   ||
req.headers[  'x-country-code'  ]


if   (countryCode   &&   countryCode   !==   'XX'  ) {
return   countryCode
}


// Fall back to IP geolocation
const   clientIp   =   requestIp.  getClientIp  (req)


// Local development fallback
if   (  !  clientIp   ||   clientIp   ===   '127.0.0.1'   ||   clientIp   ===   '::1'  ) {
return   'NG'
}


const   geo   =   geoip.  lookup  (clientIp)


return   geo?.country   ||   'US'
}
}
```


The` geoip-lite` package uses MaxMind’s free GeoLite database, which is bundled with the package. No API calls needed.


## Step 4: PPP Data Helper


Create a helper to fetch PPP data for a country with a fallback:


```text
// api/helpers/ppp/get-data.js
const   path   =   require  (  'path'  )


module  .  exports   =   {
sync:   true  ,
friendlyName:   'Get PPP data'  ,
description:   'Get PPP data for a country from region files'  ,


inputs: {
countryCode: {
type:   'string'  ,
required:   true  ,
description:   'ISO Alpha-2 country code (e.g., NG, US, KE)'
}
},


exits: {
success: {
outputFriendlyName:   'PPP data'
}
},


fn  :   function   ({   countryCode   }) {
const   regionPath   =   path.  join  (
sails.config.appPath,
`config/regions/${  countryCode  .  toLowerCase  ()  }.json`
)


try   {
return   require  (regionPath)
}   catch   {
// Fallback to US if region file doesn't exist
sails.log.  warn  (  `PPP: Region file not found for ${  countryCode  }, falling back to US`  )
return   require  (path.  join  (sails.config.appPath,   'config/regions/us.json'  ))
}
}
}
```


## Step 5: Pricing Page Controller


Now wire it all together in your pricing controller:


```text
// api/controllers/billing/view-pricing.js
module  .  exports   =   {
friendlyName:   'View pricing'  ,
description:   'Display pricing page with PPP-adjusted prices'  ,


exits: {
success: {
responseType:   'inertia'
}
},


fn  :   async   function   () {
const   pppConfig   =   sails.config.ppp
const   countryCode   =   sails.helpers.ppp.  getCountryFromRequest  (  this  .req)
const   pppData   =   sails.helpers.ppp.  getData  (countryCode)


// Get providers for local currency
const   localProviders   =   pppConfig.providersFor[pppData.localCurrency]   ||   []


// Calculate PPP-adjusted price
const   basePrice   =   99   // Your base USD price
const   pppPrice   =   Math.  round  (basePrice   *   pppData.pppConversionFactor)


return   {
page:   'billing/pricing'  ,
props: {
plans: {
annual: {
name:   'Annual'  ,
basePrice,
pppPrice,
discount: pppData.discount
}
},
currency:   'USD'  ,
currencySymbol:   '$'  ,
localCurrency: pppData.localCurrency,
localCurrencySymbol: pppData.localCurrencySymbol,
localProviders,
pppDiscount: Math.  round  (pppData.discount   *   100  ),
countryCode
}
}
}
}
```


## Step 6: The PayLink Component


Create a Vue component(Or React if that’s your UI language) that handles both USD and local currency payments. The split button shows a local currency option when available:


```text
<!-- assets/js/components/PayLink.vue -->
<  script   setup  >
import   { ref, computed }   from   'vue'


const   props   =   defineProps  ({
plan: { type: String, required:   true   },
amount: { type: Number, required:   true   },
currency: { type: String, required:   true   },
providers: { type: Array, required:   true   },
localProviders: { type: Array,   default  : ()   =>   [] },
localCurrency: { type: String, default:   null   },
localCurrencySymbol: { type: String, default:   null   },
label: { type: String, default:   'Get started'   }
})


const   hasLocalProviders   =   computed  (()   =>   props.localProviders.  length   >   0  )


const   checkoutUrl   =   computed  (()   =>   {
return   `/checkout?plan=${  props  .  plan  }&provider=${  props  .  providers  [  0  ]  }&currency=${  props  .  currency  }`
})


const   localCheckoutUrl   =   computed  (()   =>   {
if   (props.localProviders.  length   ===   1  ) {
return   `/checkout?plan=${  props  .  plan  }&provider=${  props  .  localProviders  [  0  ]  }&currency=${  props  .  localCurrency  }`
}
return   null
})
</  script  >


<  template  >
<  div   class  =  "relative inline-flex w-full"  >
<!-- Main USD button -->
<  a
:href  =  "checkoutUrl"
:class  =  "[
'flex-1 flex items-center justify-center rounded-xl px-6 py-4 text-lg font-bold',
'bg-gradient-to-r from-brand-600 to-accent-600 text-white',
hasLocalProviders ? 'rounded-r-none' : ''
]"
>
{{ label }}
</  a  >


<!-- Local currency button -->
<  a
v-if  =  "hasLocalProviders && localCheckoutUrl"
:href  =  "localCheckoutUrl"
:title  =  "`Pay in ${localCurrency}`"
class  =  "flex items-center justify-center rounded-r-xl bg-accent-600 px-4 py-4 text-lg font-bold text-white"
>
{{ localCurrencySymbol }}
</  a  >
</  div  >
</  template  >
```


## Step 7: Display the PPP Discount Banner


Rather than automatically applying the discount, we make it opt-in. This gives users the choice and feels more respectful:


```text
<!-- In your pricing page -->
<  div   v-if  =  "showPppBanner"   class  =  "bg-linear-to-r from-brand-600 to-accent-600 px-4 py-4"  >
<  div   class  =  "mx-auto max-w-4xl"  >
<  div   class  =  "flex flex-col items-center justify-between gap-4 text-center md:flex-row md:text-left"  >
<  div   class  =  "flex items-start gap-3"  >
<  svg
class  =  "h-6 w-6 shrink-0 text-white"
fill  =  "none"
stroke  =  "currentColor"
viewBox  =  "0 0 24 24"
>
<  path
stroke-linecap  =  "round"
stroke-linejoin  =  "round"
stroke-width  =  "2"
d  =  "M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
/>
</  svg  >
<  div  >
<  p   class  =  "text-sm font-bold text-white md:text-base"  >
🎉 {{ discountPercentage }}% Regional Pricing Available!
</  p  >
<  p   class  =  "mt-1 text-xs text-white/90 md:text-sm"  >
We want every African engineer to have access. Get {{ discountPercentage }}% off and invest in your engineering career.
</  p  >
</  div  >
</  div  >
<  div   class  =  "flex shrink-0 gap-2"  >
<  button
type  =  "button"
class  =  "rounded-lg cursor-pointer bg-white px-4 py-2 text-sm font-semibold text-brand-700 transition-all hover:bg-white/90"
@click  =  "acceptPppDiscount"
>
Yes, apply discount
</  button  >
<  button
type  =  "button"
class  =  "rounded-lg cursor-pointer border-2 border-white/30 px-4 py-2 text-sm font-semibold text-white transition-all hover:bg-white/10"
@click  =  "dismissPppDiscount"
>
No thanks
</  button  >
</  div  >
</  div  >
</  div  >
</  div  >
```


The` acceptPppDiscount` method sets` pppAccepted` to` true` in your component state, which then adjusts the displayed prices and checkout URLs to use the discounted amount. The` dismissPppDiscount` method hides the banner for the current session.


We intentionally don’t persist this choice to localStorage. A user’s PPP eligibility can change if they travel or use a VPN, so we check their location fresh on each visit and let them decide again whether to accept regional pricing.


## Step 8: Automated PPP Updates (Optional)


Create a[scheduled script](https://docs.sailscasts.com/sails-quest) to update PPP data monthly using[Sails Quest](https://docs.sailscasts.com/sails-quest) :


```text
// scripts/update-ppp-regions.js
module  .  exports   =   {
friendlyName:   'Update PPP regions'  ,
description:   'Fetch latest PPP and exchange rate data'  ,


quest: {
schedule:   '0 3 1 * *'   // 1st of every month at 3 AM
},


fn  :   async   function   () {
const   fs   =   require  (  'fs'  ).promises
const   path   =   require  (  'path'  )


const   REGIONS_DIR   =   path.  join  (sails.config.appPath,   'config'  ,   'regions'  )
const   {   dataSources   }   =   sails.config.ppp


// Fetch exchange rates
const   ratesResponse   =   await   fetch  (  `${  dataSources  .  exchangeRateApi  }?base=USD`  )
const   ratesData   =   await   ratesResponse.  json  ()


// Update each region file
const   regionFiles   =   await   fs.  readdir  (  REGIONS_DIR  )


for   (  const   file   of   regionFiles) {
if   (  !  file.  endsWith  (  '.json'  )   ||   file   ===   'us.json'  )   continue


const   filePath   =   path.  join  (  REGIONS_DIR  , file)
const   regionData   =   JSON  .  parse  (  await   fs.  readFile  (filePath,   'utf8'  ))


// Update exchange rate
if   (ratesData.rates[regionData.localCurrency]) {
regionData.exchangeRate   =   ratesData.rates[regionData.localCurrency]
}


await   fs.  writeFile  (filePath,   JSON  .  stringify  (regionData,   null  ,   2  ))
}


sails.log.  info  (  'PPP regions updated successfully'  )
}
}
```


Run it manually with:


```text
npx   sails   run   update-ppp-regions
```


P.S: Quest will run it automatically on schedule when you deploy.


## The Result


With this system in place:


1. **Users see fair pricing** : Automatically adjusted based on their location
2. **Multiple payment options** : USD via Stripe or Lemon Squeezy, local currencies via Paga, Paystack, or,Flutterwave
3. **Transparent discounts** : Users know why they’re getting a discount
4. **Automated updates** : PPP data stays current with monthly updates


## Conclusion


PPP pricing isn’t complicated to implement, but it makes a massive difference in accessibility. When we added this to The African Engineer, we saw immediate results.


For the philosophical take on why PPP matters, especially for African builders, check out my post[Build Globally, Price Locally](https://dominuskelvin.dev/blog/build-globally-price-locally) .
