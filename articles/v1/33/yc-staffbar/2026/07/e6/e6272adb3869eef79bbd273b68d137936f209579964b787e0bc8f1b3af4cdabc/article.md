---
schema_version: "1.0.0"
document_id: "e6272adb3869eef79bbd273b68d137936f209579964b787e0bc8f1b3af4cdabc"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/show-a-paywall-during-these-three-high-converting-app-experiences"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:0f9bc961165dcb2e181641c5d46058b37784aa89f524ea8d3c55916400abc73e"
---

# Show a paywall during these three high-converting app experiences

One of the keys to successfully monetizing is showing the right paywall at the right time. However, doing this isn't exactly easy:


- Developers and designers need to maintain (and build) multiple paywalls. Or, at the very least, vend different variants of the same one.
- And, maintaining the logic of several different events in your app becomes cumbersome and easy to get wrong.


Of course, Superwall fixes these types of problems. Today, I'll show you three novel use-cases of presenting a unique paywall using standard placements to help you better monetize:


1. **Touches Began:** Want to show a paywall the first time a user interacts with your app without sprinkling presentation code everywhere? That's exactly what this placement can help you with.
2. **An abandoned transaction:** Did the user begin to purchase something, but change their mind? This is a perfect time to offer a high-intent user a discount.
3. **A declined paywall:** If the user rejects the offer on our first paywall, we can still try to win them over by presenting the same subscription — but a little cheaper — in a subsequent paywall.


Here are the paywalls we'll be using for each placement:


Paywall Examples


The left-most paywall displays when the user first touches anywhere in the app. We can think of this as the most "basic" paywall, a generalized one which simply asks the user if they'd like to subscribe.


The middle paywall will be shown if the user starts to subscribe (i.e. the payment sheet presents) but then taps the X button and cancels the transaction. In that case, we show a paywall which offers the same subscription but with a discount. We call this an abandoned transaction paywall.


Finally, the right-most paywall is what we'll show after the user closes the first paywall we've presented. In that case, we'll show a paywall offering a cheaper rate to try and change their mind.


Setting up something like this is trivial with Superwall. Three different campaigns have been created for each placement:


Campaigns for placements


Even better — there's no customization or additional audience filters required. Add these placements, associate a paywall and that's it:


- ` touches_began`
- ` transaction_abandon`
- ` paywall_decline`


For example, here's what our campaign for the first touch paywall looks like:


Campaign for Touches Began


The best part? This doesn't require any client side changes. If you've got the Superwall SDK installed, you can try any of these techniques in production right now.


## First touch


The idea here is that we want to show our most general, one-size-fits-all paywall. We don't have a particular opinion of what should trigger it, but we do want to aim for it to be presented for each user.


So, the first touch (per session) will trigger this paywall. Here, I tap on "Log" which presents it:


Your browser does not support the video tag. Showing a paywall on first touch


## Transaction abandoned


Now, let's act like we're about to purchase our subscription — but then change our mind. In that case, we'll present a paywall that shows the same subscription at a discount:


Your browser does not support the video tag. Showing a paywall during an abandoned transaction


## Paywall declined


Finally, we'll offer up the same product with a discount (a similar strategy to our abandoned transaction campaign) when the user closes our initial paywall. Keep in mind that you can also limit the times the user can match against this paywall, which makes it a great candidate to show a limited-time offer too:


Your browser does not support the video tag. Showing a paywall after initially declining another


## Bonus: survey responses


Here's another useful one you can try — reacting to survey responses from users who didn't convert. Using the placement` survey_response` , we can present a paywall after a survey response has been recorded.


Here, if a user closes our general paywall and the one-time survey is shown — we'll show a paywall which offers a 30 day free trial once they respond:


Your browser does not support the video tag. Showing a paywall from a survey response


One important note about this one — in this example, we're showing the same paywall based on any response to the survey. However, it's also simple to show a unique paywall based off of the particular response they chose. For example, if we wanted to show a different paywall (which offers a longer trial) if they selected "I need to try it first", we could add a campaign filter matching` survey_selected_option_title` to that response:


Campaign based on a particular survey response


This approach is flexible, since if the user doesn't choose "I need to try it first", the "catch-all" audience below it would then show the paywall we've used in our example.


## Wrapping up


Of course, here's the question you need to ask yourself — which of these techniques is right for your app? Is it just one of them, maybe two? All three? Perhaps none?


To answer that, you need intuition and the ability to quickly test these types of things out. That's where we come in. Superwall is the easiest way to get these things up and running, and the best way to scale them afterwards. The techniques here are just the start.[Give us a try for free today!](https://superwall.com/sign-up)
