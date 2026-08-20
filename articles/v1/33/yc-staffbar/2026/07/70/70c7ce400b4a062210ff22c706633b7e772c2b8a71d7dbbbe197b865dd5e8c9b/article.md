---
schema_version: "1.0.0"
document_id: "70c7ce400b4a062210ff22c706633b7e772c2b8a71d7dbbbe197b865dd5e8c9b"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/customizing-interfaces-based-on-paywall-presentations-with-superwall"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:395a160f1999520c7c12bf35ddc423ddbcd8882a9b68b588e554f715baea82a9"
---

# Customizing Interfaces Based on Paywall Presentations with Superwall

Most apps with a subscription offering typically have a notion of "pro", or paid, features. One way to sell those? By showing some component in the interface that upsells the value of some particular feature:


- A banner in settings that shows off pro features.
- Or, maybe a workout app would have a lock icon over workouts that are part of a premium plan.


That's where` getPresentationResult` shines in our SDK. Using it, you can tell whether or not a paywall would've shown for a particular[placement](https://superwall.com/docs/campaigns-placements) . This API doesn't actually show a paywall. Instead, it simply tells you, "If you tried to show a paywall here, what would happen?"


With that in hand, you can make smart UI decisions — like whether to show that in-app banner, a modal encouraging users to upgrade, or even dim certain components based on campaign logic configured entirely in Superwall.


Let's walk through why this is useful, when to use it, and how to implement it in your app today.


## Wait, why not just use entitlements?


At first glance, it might seem like something you could just handle with a basic entitlement check. But here's the difference:


Entitlement checks tell you what the *current* access level is.


But,` getPresentationResult` centralizes the logic about who should be upsold, what campaign rules apply, and whether a paywall would actually show — all in one place.


In practice, that means:


- You can always change targeting logic without shipping a new app update.
- You don't hardcode paywall access rules in the client (if they are this level, or that level, etc).
- You stay in sync with the actual paywall experience — because it's literally using the same decision engine under the hood.


## Showing and hiding upsell chrome


Let's check out Superwall's resident example app, Caffeine Pal. In it, we've got some "quick log" buttons that are part of the pro plan. If they aren't on a paid plan, we'll add a lock icon over them:


If users aren't on a pro plan, a lock icon shows over the log buttons.


The buttons themselves? They register a placement called` caffeineLogged` , and we don't have to change anything about them for all of this to work:


```text
Button  (  "  Log  "  ) {
Superwall.  shared  .  register  (  placement  :   "  caffeineLogged  "  ) {
store.  log  (  amountToLog  )
}
}
```


That's perfect for showing the paywall, and we can change whether or not that should show a paywall in the Superwall dashboard at any point (all over the air, no app update needed). But, what about that lock icon? How do we know when to show it?


That's a perfect use case for` getPresentationResult` .


```text
private   func   queryPresentationResult  ()   {
Task   {
let   res   =   await   Superwall
.  shared
.  getPresentationResult  (  forPlacement  :   "  caffeineLogged  "  )


switch   res {
case   .  paywall  (  let   exp  )  :
self  .  showLock   =   true
print  (  "  Result: Showing paywall for   \(  exp  )  "  )
case   .  placementNotFound  :
self  .  showLock   =   false
print  (  "  Result: No placement for caffeineLogged  "  )
case   .  noAudienceMatch  :
self  .  showLock   =   false
print  (  "  Result: No matching audience.  "  )
case   .  holdout  (  _  )  :
self  .  showLock   =   false
print  (  "  Result: User is in a holdout.  "  )
case   .  paywallNotAvailable  :
self  .  showLock   =   false
print  (  "  Result: Paywall is unavailable.  "  )
}
}
}
```


Now, you can easily base the lock icon's appearance based on the same logic Superwall uses to determine whether or not a paywall would show. This is our SDK working together perfectly — the button itself will show a paywall if it needs to, log caffeine if they are pro, the lock icon follows the same logic and *all of that* can be controlled over the air when you edit the campaign that` caffeineLogged` belongs to.


## Digging into` PresentationResult`


If you need to get more fine-grained information about the presentation result, look no further than its return type,` PresentationResult` . As you saw in the example above, it reports back the reason a paywall would show or not show. To distill it down for scenarios like ours, you'd basically want to show upsell chrome when you hit a` paywall` result:


Case Description


` paywall(Experiment)` A matching audience was found and this user will be shown a paywall.


` holdout(Experiment)` A matching audience was found, but this user was assigned to a holdout group, so no paywall will be shown.


` noAudienceMatch` No matching audience was found for this placement, so no paywall will be shown.


` paywallNotAvailable` The paywall is unavailable. This could be due to no internet, no view controller to present from, or a paywall already being presented.


` placementNotFound` The placement string wasn't found on the dashboard. Check your campaign setup or spelling.


Of note, you'll get` noAudienceMatch` when they are a paying customer.


It's a flexible API, and there are several compelling use-cases for it. Whether you need an inline banner for premium-only sections of an app, maybe a conditional tooltip that might guide users to try locked features, or even during free trials, soft gates, and educational moments — the API works for all of them. Hiding and showing premium upsells like the one below becomes trivial:


An upsell in settings.


## Wrapping up


Knowing the result of a paywall presentation attempt is one of the most powerful (and in my opinion, underrated) tools in the Superwall SDK. It lets you write smarter, more adaptable UI by deferring all the logic of "should this user see a paywall?" over to us. After all, you don't ever want to duplicate campaign logic on the client. You can simply just ask: what would happen if I tried to show a paywall right now?


I hope this tip helps you out, and as always — go sign up for a[free Superwall account](https://superwall.com/sign-up) if you haven't yet. Then, check our[installation guide](https://superwall.com/blog/getting-started-with-superwall-in-your-indie-ios-app) to be up and running in minutes.


## FAQ


What does getPresentationResult do? getPresentationResult tells you whether a paywall would show for a given placement without actually presenting it. It runs the same campaign decision engine your live paywalls use, so you can drive conditional UI like lock icons or upsell banners from the exact same logic.


How is getPresentationResult different from an entitlement check? An entitlement check only tells you the user's current access level. getPresentationResult centralizes who should be upsold, which campaign rules apply, and whether a paywall would actually show — all controllable over the air without shipping an app update, so you never hardcode paywall access rules on the client.


What cases can PresentationResult return? PresentationResult can return paywall(Experiment) when a matching audience is found and a paywall will show; holdout(Experiment) when the user is in a holdout group; noAudienceMatch when no audience matches (including paying customers); paywallNotAvailable when the paywall can't be shown (e.g. no internet or no view controller); and placementNotFound when the placement string isn't found on the dashboard.


Why does getPresentationResult return noAudienceMatch for paying customers? Paying customers typically don't match the audience rules that trigger a paywall, so you'll get noAudienceMatch for them. That makes it a reliable signal for hiding upsell chrome from users who already have access.
