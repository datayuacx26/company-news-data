---
schema_version: "1.0.0"
document_id: "d96160e029eeb5a9118d6f9b7a82bc99028c88ea36849d174714479afca99ed6"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/apple-allows-external-payment-links-in-the-app-store-everything-you-need-to"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:1b45a69ff8a552249fb77b5778a823026c81d0ada3ac37de69f96ed5806b59b4"
---

# Apple allows external payment links in the App Store – everything you need to know

In response to the Supreme Court's ruling in the sprawling *Epic Games v Apple* case, developers can now link outside of their apps to external payment methods. So, do developers finally have a viable avenue to bypass Apple's 30% (or 15%, for Apple's App Store Small Business Program) commission fee?


Well yes, but also no. In fact, it's a little complicated to understand the net effect from the ruling from a developer's standpoint.


So, we're here to break it down for you. First, we've got to understand what the ruling actually means. Then, we can see how Apple went about complying with it. If you were hoping for a way to place a link to your app's website to use something like Stripe Checkout, well - it's not quite that simple.


## What the Ruling States


While Apple largely won the case, there was one critical area where the courts ruled in favor of Epic Games. Particularly, Apple's anti-steering rules. Put simply, this means apps should have the ability to link to outside payment processors, as opposed to Apple steering them to only use their in-app purchase flows. Today, to offer any subscription, you are probably keenly aware of the options here - use Apple's StoreKit APIs, and that's it. Anything else will lead to a rejection of your app's submission.


But after this ruling? Well, you *should* be able to do things like direct a user to your website to sign up for a subscription. Again, by the definition of the ruling, this is a flow that will be supported going forward.


Does that mean as of today, you can submit an app with a link to your website to offer more competitive prices on your paywall? The simple is no, you can't. How this works in practice is so complicated that I doubt few developers will actually link to external providers. Plus, as we'll see - if Apple's rules are followed to the letter, it'll likely end up being *more* expensive to do it this way.


## How Linking to External Payments Works


Apple updated App Store Review Guideline 3.1.1 today, which introduced the StoreKit Purchase Link Entitlement. So, to start, a brand new entitlement must be granted to even begin linking out to other payment providers. In fact, there are several steps that are required to use external payment links. To understand how it works, it's easiest to list out all of the requirements to explain how you'd even do it.


To start, you have to meet some qualifications to apply for the entitlement. Those are, your app must:


- Be available on the iOS or iPadOS App Store in the United States storefront;
- Offer in-app purchases when distributed through the iOS or iPadOS App Store in the United States storefront; and
- Not participate in the Video Partner Program or News Partner Program.


After you receive confirmation from Apple that the entitlement has been granted, then you'll need to:


- Configure your App ID in Certificates, Identifiers & Profiles to support the entitlement;
- Update your project with the entitlements plist file, and also your` info.plist` file to list the entitlement and metadata.


The` info.plist` file is where you'll actually enter in the URL to your external purchasing page. Since this is a static URL, you won't be able to use any sort of parameters for it. That's quite a barrier to entry, since many of the advantages of using web-based checkouts is that you can offer discounts, personalized offers and other similar offerings based on certain parameters. Here, it's the same URL for everybody.


If you settle on a link, then you have to take care to style it according to Apple. Among those requirements? It has to use a plain button style. That means there can't be a fill color, background or have any shape around it. Finally, you'll need to use the "link out icon" symbol to the right of the URL.


The next requirement deals with the actual copy. It appears the guidelines state that developers can't simply use whatever copy might make the most sense, but rather - you use a template provided by Apple. Here's what you've got to choose from (keep in mind each of these would need the link-out icon at the end):


**Purchase template:**


- Purchase from the website at www.example.com


**Special offer template:**


- For special offers, go to www.example.com
- For a special offer, go to www.example.com


**Lower price template:**


- Lower prices offered on www.example.com
- Lower price offered on www.example.com


**Percent off template:**


- To get XX% off, go to www.example.com


**Specific price template:**


- Buy for $X.XX at www.example.com


Here's an example of what the styling and copy might look like:


External payment links can only be shown in one screen, and it can't be your paywall


Now that we've:


- Applied for an entitlement
- Setup Xcode with it
- Settled on a static URL
- Styled the button for the URL appropriately
- And, used any of the above template copy


…we're still not done.


Developers can only show this link in particular places. You'd think that your paywall would be the perfect fit for this, but it seems they aren't in compliance. Specifically, Apple states that " *one app page the end user navigates to (not an interstitial, modal, or pop-up), in a single, dedicated location on such page, and may not persist beyond that page.* " can be used. That means it has to be placed outside of your paywall, on a single page and only on that single page.


Finally, before that link is shown - there's API you have to call to present this modal warning first:


This page must be presented before navigating to your external paywall


After all of that, the user will end up at your external payment destination. You might think that would be worth the trouble to save on commission, but I haven't arrived at the trickiest part of the guidelines. That's the commission you'll have to still pay Apple.


## The Real Cost


Apple says that you'll still need to pay a 27% commission on any resulting transactions from external links. If you're in the Small Business Program, that drops to 12%. Now, when you take into consideration any processing fees from your payment provider (and, of course, taxes) you're likely over the results 30% you'd pay from using in-app purchases.


What's more, Apple requires you to submit your transactions over to them within 15 calendar days. That holds true even if you had no transactions. This isn't a simple list of transactions either, these are in-depth reports with dates, amounts, app identifier and more. Here's an example from Apple on what they expect:


Example Self Reported Income


Apple notes that there may API to report these in the future. And, if that ever happens, you've got 30 days to implement it within your app — or you'll face rejection. If you miss payments, then you'll owe interest at the rate of one percent (1%) per month or the highest rate permitted by law, whichever is less. After all of that, you're also subject to Apple auditing your transactions. If they find any discrepancy, they can either terminate you from the Apple Developer Program or withhold the amount from your App Store payments.


As we said, it's not as simple as providing a link to your external website or payment portal. Apple has the whole process locked down end-to-end.


## Wrapping Up


If you're wondering if Apple wants you to this, it's fairly clear the answer is no. While the court of law can shift some of these requirements, time will tell if that will actually happen. You're likely best off using traditional in-app purchase methods unless anything here changes. If it does, we'll be sure to update this post.


At the end of the day, developers have to carefully consider one fundamental question: What is the best way to make money on the App Store?


I don't think a lot of us will arrive at Apple's solution outlined here. The days of using something *other* than in-app purchase APIs isn't soon on the horizon. Either way, we're here to help. Whether your paywall uses an external link in the future, or StoreKit APIs — you can test and optimize all of those things with[Superwall](https://superwall.com/) . You can get started with a free account today by visiting[this link](https://superwall.com/) .
