---
schema_version: "1.0.0"
document_id: "0da3fd36f0e840b8dd59d45983344bce8d1cd377ce3777763b9cc94655cf575d"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/the-superwall-newsletter-volume-4"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:b289c8f56aa5fdee44de99052807e3f57a2d2c05570e0d54b526c38959253e48"
---

# The Superwall Newsletter: Volume 4

Welcome to our 3-2-1 newsletter. In each issue, we give you three growth tips, two tech tips and one Superwall tip. In this issue, we'll tackle:


- 💰 Lessons for you to apply from[Stardust's 4x revenue growth](https://superwall.com/blog/how-stardust-increased-ios-app-revenue-by-4x-in-six-weeks)
- ↗️ How to easily build feature-based paywalls
- ✨ And of course, news from our blogs and expertly crafted paywall examples


## 3 Tips to Grow


Superwall customer[Stardust](https://superwall.com/blog/how-stardust-increased-ios-app-revenue-by-4x-in-six-weeks) is enjoying a **4x** revenue multiplier from our recent white-glove growth service. Here are three critical lessons I've taken from the experience to help you grow your own app (but, definitely read the[post](https://superwall.com/blog/how-stardust-increased-ios-app-revenue-by-4x-in-six-weeks) — it's full of crazy insights!):


### Make Your Pricing Clear and Appealing


Don't just focus on the price itself — think about how you present it. Offering a free trial, showing things like discount percentages, or featuring how much users save can make your pricing more attractive. Stardust saw a huge *131% increase* in conversions when they added a free trial to their yearly plan!


But remember, when testing different pricing options, you need to give free trial periods enough time to finish. That way, you can see how many people actually convert to paying customers and your next steps are informed by good data.


Here's the first round of tests Stardust tried:


Stardust's experiments around copy


And, here's the second round — which had a focus on the annual plan:


Stardust's experiments around layout, pricing and more


### Test Multiple Things at the Same Time


Speed up your growth by running different experiments at the same time. Some companies call this *experimentation layering* . How did Stardust do this? By testing a new price presentation **while** their free trial experiment was still underway. Experimentation layering lets you collect data more quickly and optimize different aspects of your app faster.


But — and this is critical if you try experimentation layering — it's important to choose tests where the results of one won't directly impact the other.


Experiment Layering Example


### Surprise! Testing Design Matters


A poorly designed paywall with cheaper prices will often under-perform against a well-designed paywall with higher prices. Sometimes, the only way to find what's "good" design is by testing variants. Remember, it's not always about the aesthetics or pricing, but how information is conveyed. Experiment with different approaches and see what resonates best with your users.


Here, Stardust focused on testing video versus a "trial timeline" design:


Stardust's experiment around video paywalls


## 2 Tech Tips


With iOS 18 just hitting phones across the globe, I figured now is a great time to call out some new APIs from the release.


### 1.[It's all about App Intents](https://superwall.com/blog/an-app-intents-field-guide-for-ios-developers)


If App Intents were important before, they are *critical* now. Not only do they power common app actions, but the list of what they can do continues to grow. It's the new[NSUserActivity](https://x.com/JordanMorgan10/status/1542591855308771328) ! With their new schema definitions, we will finally reach the "hands free" flows that we've wanted to do for years with Siri. Plus, there are several quality-of-life additions, like easy integration with[Spotlight Search](https://x.com/JordanMorgan10/status/1824144425494384782) .


The framework is easy to get started with, in fact — this is an entire working App Intent:


```text
struct   GetCaffeineIntent:   AppIntent   {
static   var   title   =   LocalizedStringResource  (  "  Get Caffeine Intake  "  )
static   var   description   =   IntentDescription  (  "  Shows how much caffeine you've had today.  "  )


func   perform  ()   async   throws   ->   some   IntentResult {
let   store   =   CaffeineStore.  shared
let   amount   =   store.  amountIngested
return   amount
}
}
```


Check out this[post](https://superwall.com/blog/an-app-intents-field-guide-for-ios-developers) to dive more into them. If you haven't started yet, now is the time.


### 2. How about all that new SwiftUI stuff?


SwiftUI gets better every single year, and iOS 18 is no exception. There's a better way to manage geometry changes, perform color mixing and *so* much more. Wouldn't it be great if there was not only a blog post with code samples, but with working examples too?


Yes, it would. So at Superwall, we made it![Check out this repo which has all sorts of important examples of iOS 18 API additions, SwiftUI included too](https://github.com/superwall/iOS18-SwiftUI-Tour) :


iOS 18 Tour


## 1 Superwall Tip


The right paywall, at the right time. We say that phrase all the time here at Superwall. One easy way to do that?


Creating feature-based paywalls.


We touched briefly on these in the[last issue](https://superwall.com/blog/the-superwall-newsletter-volume-3) , but here I'll show you how you can quickly set them up. Instead of a one-size-fits-all approach, feature-based paywalls allow you to tailor what users see based on their in-app activity. Consider a workout app, where a user taps on a "pro" workout template. On a feature-based paywall, you'd call out those pro workout templates prominently.


Here's how you can use[placement parameters](https://superwall.com/docs/feature-gating#placement-parameters) to create feature-based paywalls using Superwall's editor:


**1.** In your code to register a placement, use the` params` argument, like this:


```text
let   params   =   [  "  selectedFeature  "  :  "  teamSize  "  ]
Superwall.  shared  .  register  (  event  :   "  teamSizeToggle  "  ,   params  : params  ) {
toggleTeamSize  ()
}
```


Now, that data will be shipped along with your placement — meaning you can use it all throughout Superwall.


**2.** Head on over to the paywall editor and create a new variable. Make sure it's the same name as whatever you used in step one (i.e.` selectedFeature` in our example):


Adding our placement parameter as a variable in the paywall editor


**3.** Use dynamic values to update any component on your paywall that should react to the feature the user tried to access:


Using the placement parameter to change text


And that's it! Now, your paywall can try to sell the exact feature the user wanted to access. This approach can save you from making a bunch of paywalls, with a similar design, when the only changes are copy, images or other elements pertaining to a particular feature. Again, the right paywall, at the right time.


Placement parameters can be used for a bunch of other useful things, too. Here are some ideas:


1. Create a new audience filter for a campaign based off of one.
2. Use our Liquid templating engine to use them in your copy (i.e. {{ params.somePlacementParam }}).
3. Interface with your own in-house analytics.


If you're more of the visual type, I made a video over how to do this recently —[check that out here](https://youtu.be/7KT9FfKLByA) .


## ...One More Thing


### From the blog


[How to build multi-tiered paywalls](https://superwall.com/blog/how-to-build-multi-tiered-paywalls)


### Pretty Design Tweet


How about this fun, minimalist[weather widget](https://x.com/sovpal/status/1826012593477353645) by[@sovpal](https://www.x.com/sovpal) ?


Weather Widget


### One Nice Paywall


Check out[Tripsy](https://tripsy.app/) — using several best practices for their paywall:


Tripsy's plans, what's new view and paywall


Thanks for reading, and we'll see you next time!
