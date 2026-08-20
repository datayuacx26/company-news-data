---
schema_version: "1.0.0"
document_id: "b25c8963c0afb014226471f205707946eb12d02d5f7d42157c6c2a4bff128a26"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/how-to-create-feature-based-paywalls-for-higher-conversions"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:47542e018fa1dd639e2332ac3dbd707f322792520852bc6d0ade9510c59234ea"
---

# How to Create Feature-Based Paywalls for Higher Conversions

Want to watch instead of read?[Check this post out on YouTube.](https://youtu.be/7KT9FfKLByA)


We've often said that showing the *right* paywall at the *right* time is paramount to maximizing conversions. That's why Superwall makes it easy to show a[paywall tailored towards a specific action](https://superwall.com/docs/tips-paywalls-based-on-placement) .


Another similar (and effective) strategy is to highlight the specific feature someone tried to access when they triggered a paywall. Since they already expressed interest in trying to use it, now is the time to sell it. To do that, here's a technique you can try:


- Using our[custom placement parameters](https://superwall.com/docs/feature-gating#placement-parameters) , you can specify which feature they tried to use.
- Then, in your paywall, you can access it as a paywall variable.
- Finally, you can dynamically change the content on your paywall based on it.


## Example


In my soccer app, the team size toggle is a pro feature. When a free user taps on it, I show my basic paywall:


Elite Soccer Club's basic paywall


We can do better, though. Using a custom placement, I'll instead show the same paywall but I'll put more of an emphasis on the team size toggle. When I register this[placement](https://superwall.com/docs/campaigns) , I'll pass in a placement parameter now, too:


```text
// I'll go from this...
Superwall.  shared  .  register  (  event  :   "  teamSizeToggle  "  ) {
court.  teamCount   =   selectedSize
}


// To this...
Superwall.  shared  .  register  (  event  :   "  teamSizeToggle  "  ,
params  :[  "  selectedFeature  "  :  "  teamSize  "  ]  ) {
court.  teamCount   =   selectedSize
}
```


Next, I'll make sure I can check that variable in the paywall editor. To do that, I'll open the **Variables** tab on the sidebar, and add it:


Adding our placement parameter as a variable in the paywall editor


Then, I'll check for that value in the paywall editor by using a[dynamic value](https://superwall.com/docs/paywall-editor-dynamic-values) . A good place to start would be text, so I'll change the copy here based on if I passed in something for` selectedFeature` . I'll select it, click the gear and choose "Dynamic Value":


Updating text with a dynamic value


That brings up the dynamic value editor, where I can use the placement parameter. If it's empty, I'll just keep the same copy I already was using. This is perfect, because now this paywall works as a "basic"/post-onboarding paywall, and for more focused situations like this where we emphasize a particular feature:


Using the placement parameter to change text


Now, my paywall can switch out the copy (or anything else using the same technique) by looking at the value of` selectedFeature` . This helps me create a more natural sales pitch, since the user will be prompted to buy exactly what they were trying to use.


I went ahead and made a few more tweaks to the images and header text for the team size toggle feature. Now, check out the difference. When the user taps on the team size buttons, they'll see my paywall tailored exactly for that feature:


Our updated paywall, customized by using a parameter placement


## Try it today


As you can see, with Superwall you can do this easily — all within a single paywall. You don't need to create a separate paywall for each feature (unless you want to!). Instead, you can leverage placement parameters. Just pass one in your` register` calls, and then check for it in Superwall's paywall editor. As they say, it's that easy.


As always, if you have the Superwall SDK installed — you can try this out today. If you need help getting started, check out our integration guide[here](https://superwall.com/blog/getting-started-with-superwall-in-your-indie-ios-app) . Remember, Superwall is free to get started with, so why not give it a spin today?
