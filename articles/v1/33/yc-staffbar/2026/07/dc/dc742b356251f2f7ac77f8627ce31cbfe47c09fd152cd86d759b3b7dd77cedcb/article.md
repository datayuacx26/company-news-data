---
schema_version: "1.0.0"
document_id: "dc742b356251f2f7ac77f8627ce31cbfe47c09fd152cd86d759b3b7dd77cedcb"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/the-superwall-newsletter-volume-1"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:fb27cfa2cf9eef9ed7b0c96553ce6c66ecaae0864253b0263102e2058e3518a7"
---

# The Superwall Newsletter: Volume 1

## Welcome to the Superwall Newsletter!


Every few weeks or so, we send out a shockingly short, succinct but actionable issue of our newsletter (just sign up for a Superwall account to get it directly). Our aim is to equip you with some industry news, tips to take to your own app business, or a little Superwall product feature gem.


Let's get started!


## 3 Tips to Grow


### 1. Default to Higher Interval Plans


Whenever you have a paywall with multiple plan durations, default to setting the default plan to the longest duration plan. For example:


- If you have a paywall with monthly and yearly plans — choose yearly as the default.
- Or, if you're showing a weekly, monthly and yearly plan, but are hesitant to set the yearly plan as the default, then use the monthly option.


The data we've combed through is compelling. **Most apps find that their plan mix totally inverts.** If you were seeing 37% selecting yearly, switching defaults could increase it to 63%.


So, why does this work?


First off, primacy bias is what creates a tendency for people to select the first option presented to them. Secondly, some segment of users will simply start a trial with whatever plan is set (no matter the duration).


In one use case, 71% of users who started a trial initially closed the paywall. In that segment, 54% selected the monthly option. These are the shrewd shoppers who need to see the product before starting a trial.


However, when looking at those subscribers who didn't close the paywall, an overwhelming **82%** selected the default yearly option. These are the high intent users — so it literally pays to optimize for them.


### 2. So, How Many Products Should Show on a Paywall?


Once you've settled on your products, figuring out how to present them on your paywall can be a bit of a puzzle. At Superwall, our analysis of popular paywalls in the App Store reveals compelling trends to help guide you, including a **notable 60% boost in conversions** based on product display quantity.


Here's what we found in terms of how displaying a different number of products affects conversions.


**Key Insights:**


- **Two Product Placements:** Compared to a single product display, offering two choices can increase conversions by 61%. This significant lift isn't surprising, as it allows for direct price comparison and clearer consumer choice. One subscription is easier to justify when compared to a cheaper (or more expensive) alternative.
- **Three Product Placements:** Moving up to three products, we observed a 44% increase in conversions. This patterns shows that offering more options generally resonates well with consumer purchasing habits. In short, it's comparison shopping.


**What This Means for You:**


While it may be tempting to immediately add three products to your paywall, we recommend testing everything. Use metrics like conversion rates and paywall views to guide your strategy and make data-driven decisions (all things you can do in Superwall 😉). We've seen paywalls offer as much as five to ten options. Try it out yourself and test the results!


### 3. Leverage Apple's Media Services


Social sharing, content marketing — none of these are novel concepts. In fact, they are the baseline. But, did you know about this neat little tool from Apple to make it all easier?


Apple has a lightweight "Media Services" page to generate QR codes, shorten app links and more. It'll help you with social graphics and sharing the word about updates. Check out[this example](https://tools.applemediaservices.com/app/6443711183?country=us) our developer advocate made for his own app:


## 2 Tech Tips


### 1. SwiftUI's[.contentTransition()](https://developer.apple.com/documentation/swiftui/environmentvalues/contenttransition)


You can easily interpolate text changes, numeric or otherwise, in SwiftUI. It just takes one modifier:


```text
Text  (  database.  valueThatChanges  )
.  contentTransition  (  .  numericText  ())
```


### 2.[Software Schema Type](https://schema.org/MobileApplication)


[Schema.org](http://schema.org/) is *the* source of structured data for the open internet. Did you know there is a type specifically for software and apps? For example, here's what Angry Birds might look like:


```text
<script type=  "  application/ld+json  "  >
{
"@context"  :   "  https://schema.org  "  ,
"@type"  :   "  SoftwareApplication  "  ,
"name"  :   "  Angry Birds  "  ,
"operatingSystem"  :   "  iOS  "  ,
"applicationCategory"  :   "  GameApplication  "  ,
"aggregateRating"  : {
"@type"  :   "  AggregateRating  "  ,
"ratingValue"  :   "  4.6  "  ,
"ratingCount"  :   "  8864  "
},
"offers"  : {
"@type"  :   "  Offer  "  ,
"price"  :   "  1.00  "  ,
"priceCurrency"  :   "  USD  "
}
}
</script>
```


You'd add this in the` <head>` tag of your page. This way, it can be crawled by services — chief among them?[Google](https://developers.google.com/search/docs/appearance/structured-data/software-app) .


## 1 Super Superwall Tip


Show an annual price at a monthly rate using our product variables:


For some potential customers, the annual "sticker shock" is a real thing. However, pricing psychology comes into play here, too. If you say…


"This product is $120 a year"


…versus


"This product is $10 a month"


…the difference between them can be a sale versus a pass. If you want to show this type of pricing in your paywalls, it's super easy. Here's how:


1. Choose a text control (or add one) on your paywall.
2. Edit its text to use the` {{ products.primary.monthlyPrice }}` variable.


Here's an example:


## …One More Thing


- One thing from our blog
- One inspiring design tweet
- One well-done paywall


### From the Blog:


Figure out how` CKSyncEngine` works to add iCloud syncing to your iOS app:


### Pretty Design Tweet


From[@OpenPurpose](https://twitter.com/openpurpose) — some nice,[bottom sheets for the web](https://twitter.com/openpurpose/status/1779872330250060170) :


### One Nice Paywall


[Riveo](https://apps.apple.com/us/app/riveo-video-effects-enhancer/id1546053158) , Apple Design Award finalist, utilizing the new popular "Free Trial" toggle:


Thanks for reading, and we'll see you next time!
