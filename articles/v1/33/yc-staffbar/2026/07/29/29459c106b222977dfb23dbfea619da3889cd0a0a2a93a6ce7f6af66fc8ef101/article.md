---
schema_version: "1.0.0"
document_id: "29459c106b222977dfb23dbfea619da3889cd0a0a2a93a6ce7f6af66fc8ef101"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/show-paywalls-at-the-perfect-moment-with-contextsdk-and-superwall"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:c0fe326dc884ba310984b8250657caf3c4ca12feb32c3fe7f8dbf6fd8833a7bd"
---

# Show Paywalls at the Perfect Moment with ContextSDK and Superwall

With the recent introduction of[Superwall's Pop-up Style Presentations](https://superwall.com/blog/introducing-pop-up-style-presentations-and-requesting-reviews) , you probably asked yourself when to show the prompt to achieve the highest amount of sales, while minimizing any interruption to your user's workflow.


With that, you have two dimensions on how to think about this:


1. **Where** in the app and in which flow to show the paywall pop-up.
2. **When** to show the paywall pop-up during a user-session (if at all).


## Where in your app to show the paywall


You likely already have a deep understanding of your own app, as well as the flows within your app where it makes the most sense for a user to see an upgrade prompt.


One obvious situation where you likely show a paywall is when the user tries to access a premium feature. In those cases, we recommend showing the full paywall, and not a pop-up style presentation of the Superwall paywall.


When it comes to "unprompted prompts", meaning situations where you want to ask the user if they're interested in purchasing an upgrade, there are many different situations within your app where it nicely fits into the user journey. For example:


1. During or after onboarding.
2. After adding a new entry or saving their work.
3. After reaching a usage milestone or limit.


To show a paywall during any of these scenarios using Superwall, all you need to do is to register a placement:


```text
// Register Superwall placement to trigger paywall
Superwall.  shared  .  register  (  placement  :   "  onboarding_complete  "  )
```


If you look at the above cases of where you may want to show a paywall, you will notice that they are all optional: You could show the prompt each time any of those cases happen, or you could deploy additional logic using Superwall's built-in features such as custom filters and segmentation:


- Users already having used the app at least \[x\] times.
- The user's region, device attributes, or other similar metadata.
- Or, other user attributes.


You could also limit the number of times the paywall is shown (cooldown logic):


Adding a cooldown limit for a paywall


## When to show the paywall within the user's session


Let's assume you want to show an "unprompted prompt" a maximum of 2 times per 24 hours as the example above: how do you now decide **when** to show those prompts?


For example, your app has a total of 10 placements, spread across onboarding steps, completing a level, adding an entry or sharing the app with a friend.


By default, the 2 prompts would already be used within the first two times a placement was reached by the user, therefore having wasted all opportunities to upsell your app immediately in the very first session.


Now picture this scenario: the user installed & onboarded your app while waiting for the train on their way home. During that 3 minute session, your new user dismissed your paywall twice. Later that day, they arrive back home, settle into bed and start your app again to continue playing around with it. At this point, your cooldown logic would not allow another prompt, even though the likelihood of converting that user in that context may be significantly higher than earlier that day.


Context makes all the difference.


Right now, most of your tools involve coming up with hand-made rules, like trying to segment the user based on various attributes, and introducing a cooldown period using the Superwall limit feature. The best approach is to combine this with A/B tests, then you can learn what actually converts best.


These approaches work well, but often miss an important layer: the user's real-world context. By analyzing the phone's motion sensor data,[ContextSDK](https://contextsdk.com/?utm_source=superwall&utm_medium=blog&utm_campaign=superwall_collab) can detect what a user is doing right now and whether they're receptive to certain in-app offers or messages. In other words, this technology identifies the perfect moment when a user is most likely to engage with an app - we call this **Moment Monetization** .


Without requiring any additional app permissions, and only adding 500kb to your app's binary size, you can easily use ContextSDK closely together with Superwall to get access to a "Context Score" between 0 and 1 indicating the likelihood of the user converting in this specific moment.


Using a context score for a filter.


You can set a custom threshold, so depending on **where** in the app this placement is, you may want to use **different thresholds** . For example, you can use a more aggressive ContextSDK Score during the onboarding process, and instead focus on less interruptions during the day to day usage of the app.


The technical integration looks like this, only adding an extra line of code right before your existing` Superwall.register` code to set the ContextSDK-related info as user attributes.


```text
Superwall.  shared  .  setUserAttributes  (  ContextManager.  shared  .  superwallAttributes  )
Superwall.  shared  .  register  (  placement  :   "  onboarding_step  "  )
```


## Conclusion


Combining the two dimensions of **where** in your app to show paywall prompts and **when** to show them allows you to maximize your sales, while reducing user annoyances of showing prompts at a bad moment.


We all know situations where you just wanted to quickly pull up some information on your phone, or quickly complete a certain action, only to be interrupted by popups, info sheets or paywall prompts.


Save those for the best moments, with the highest likelihood of the user actually reading what you're showing. Prompt-blindness is real, and we all know certain apps or websites where we're already well trained to dismiss unwanted prompts.


If you're a visual learner, check out this[Loom](https://www.loom.com/share/6db3a0ac41014083941b969e366129c2) we've recorded which shows all of this in action.


## Next steps


### How to join the Beta


The Superwall <>[ContextSDK](https://contextsdk.com/?utm_source=superwall&utm_medium=blog&utm_campaign=superwall_collab) integration is currently in private beta. We're looking for a small group of app publishers who want to experiment with context-aware paywall timing and help shape how this integration evolves.


To join, just send a short email to:


- Dieter:[\[email protected\]](https://superwall.com/cdn-cgi/l/email-protection#c3a7aaa6b7a6b183a0acadb7a6bbb7b0a7a8eda0acae)
- Jake:[\[email protected\]](https://superwall.com/cdn-cgi/l/email-protection#bbd1dad0defbc8cecbdec9ccdad7d795d8d4d6)


### Case study collaboration


We're also selecting a few partners to co-publish a joint case study showcasing early results with ContextSDK × Superwall. If you'd like to participate, reach out to[\[email protected\]](https://superwall.com/cdn-cgi/l/email-protection#e3878a86978691a3808c8d97869b97908788cd808c8e) or[\[email protected\]](https://superwall.com/cdn-cgi/l/email-protection#c5afa4aea085b6b0b5a0b7b2a4a9a9eba6aaa8) and we'll guide you through the next steps.
