---
schema_version: "1.0.0"
document_id: "ec7e00b152a1d1815651a6a77f2c67fe19e66d5b8f6ff570753e533723d8bf55"
company_key: "yc-revenuecat"
company: "RevenueCat"
source_id: "yc-revenuecat-rss-41fa37a4cb36"
canonical_url: "https://www.revenuecat.com/blog/engineering/license-mac-app-merchant-of-record"
published_at: "2026-07-20T14:00:00+00:00"
first_seen_at: "2026-07-20T23:24:09.080214+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:1d581ce89e00ac20bd68b2f6b8f9c2c100d07b3b6718c781b4f276c4083476b2"
---

# A license to bill: Selling a Mac app without a merchant of record

I hang out in a couple of online communities focused on app development. One of my favorites is a Slack community called "AppKit Abusers." It's a great place for developers writing Mac apps to hang out, help each other work through problems, and share ideas. There's one question that gets asked regularly:


> How do I make money?


This question comes up because many Mac developers choose to distribute their apps outside the Mac App Store. Sandboxing imposes restrictions on iOS apps, but it's even more extreme for Mac apps. Many of the things we've come to expect from "[Mac-assed Mac apps](https://daringfireball.net/linked/2020/03/20/mac-assed-mac-apps) " over the years are simply not possible with sandboxing. However, disabling the sandbox on a Mac app comes at a cost: you cannot ship your app on Apple's App Store. This means that all of the work of collecting money from customers, handling licensing, etc. falls squarely on the developer's shoulders. And so every couple of months, like clockwork, another developer is in the Slack group asking for recommendations on how to implement licensing.


This is a problem I've faced several times myself. I love writing Mac apps; I've got three I'm building for myself right now. On iOS, I've used RevenueCat to simplify the logic around dealing with in-app purchases, and it's been wonderful to offload that mental burden onto a reliable framework, and allow me to focus on the task of *building my app* . I've wished something as easy existed for Mac apps, too.


Earlier this year, I joined RevenueCat as a Senior SDK Engineer and learned more about all the ways they help developers make money. Of course, I immediately thought "Well, what about Mac developers?" One feature in particular caught my eye:[web-based billing support](https://www.revenuecat.com/feature/web) . This is a way for web developers (and mobile developers selling outside their platform stores) to offer subscriptions to their customers. Since all the information is associated with RevenueCat's service, purchases made on the web get tied to the customer’s profile, which can then be accessed via the RevenueCat SDK in an app. And since the RevenueCat SDK works on macOS… I quickly started connecting the dots and forming a plan.


After thinking about this approach, I put it on my to-do list to see if it would actually work. As luck would have it, a couple of days later my friend[Jared Sorge](https://jsorge.net/) mentioned he was looking to solve this exact problem, and he was game to try my idea. After some careful reading of the documentation, a couple of Zoom chats, and a bit of trial and error, Jared got it to work. In the end, it was extremely straightforward:


- Configure the product offerings and entitlements in the RevenueCat dashboard, like you would for any other app
- Connect Stripe as a[web provider](https://www.revenuecat.com/docs/projects/connect-a-store) and hook your offering up to a[Web Purchase Link](https://www.revenuecat.com/docs/web/web-billing/web-purchase-links)
- Send identified users from your Mac app to the web to purchase
- After purchase, ask the SDK to refresh, and see that they now have the purchased entitlement


That's the short version. The longer version is Jared's story—how he got there for[Arborist](https://taphouse.io/arborist) , his new Mac app launching today.


## Why not Paddle, FastSpring, or Lemon Squeezy?


Arborist is a native macOS command center for git repositories and worktrees. It works by running user-entered commands at arbitrary locations, which means sandboxing—and therefore the Mac App Store—was never an option. Jared settled on a one-time version 1 purchase of $39, "just like the Software Days of Yore," and then hit the question every direct-sales Mac developer hits: how do I do licensing on my own?


He knew the established options like[Paddle](https://www.paddle.com/) ,[FastSpring](https://fastspring.com/) , and[Lemon Squeezy](https://www.lemonsqueezy.com/) have worked for many apps like his over the years. But:


> I also wanted something simple for my users. Not to mention Apple Pay was a must-have. I've experienced friction more times than I can count when an app doesn't support Apple Pay, and every time it happens I am ever so slightly less happy with that app because of it. Customers are also more familiar with the smooth in-app purchase experiences provided by the App Store, and if I could achieve something that easy I wanted to do just that.


## No license server, no license keys


What Jared landed on contains three pieces:


- RevenueCat serves as the source of truth for whether or not a customer is licensed.
- Customer IDs come from local iCloud identifiers—specifically, calling` userRecordID()` on the app's CKContainer in CloudKit.
- When the user makes a purchase, they earn an[entitlement](https://www.revenuecat.com/docs/getting-started/entitlements) that Arborist looks for to grant them a license.


The second bullet is the clever one:


> I didn't want to have to spin up a licensing server to send out codes (much less have to store them), and I didn't want to have an email-based system. I wished for something simple like StoreKit but without using the App Store as a backend.


Because the customer ID is the user's iCloud record ID, a license bought on one Mac automatically follows the customer to every Mac signed in to the same iCloud account. There are no license keys to email, no "Restore Purchases" button, and no personally identifying information collected by the app. As Jared puts it: "Stripe handles the payment process, and the iCloud's identifiers give nothing away."


The trade-off is that this strategy requires the customer to be signed in to iCloud—a reasonable assumption for a power-user app, and Jared is working on a web-based iCloud sign-in for everyone else, coming after 1.0.


## The merchant of record question


Jared initially set up[RevenueCat Billing](https://www.revenuecat.com/billing) , RevenueCat's own billing engine for web purchases, but then he ran into a term he'd never encountered:


> I had never heard the term[merchant of record (MoR)](https://stripe.com/resources/more/merchant-of-record) before and when I first did I didn't know it was something to care about (spoiler alert: it very much is).


The merchant of record is the entity legally responsible for the sale: collecting and remitting sales tax, VAT, and GST, and handling refunds. RevenueCat Billing does not act as your merchant of record: you are. For solo developers, this can add a significant amount of complexity. For Jared and his first direct-sale app, it was a dealbreaker.


The good news is that Stripe offers[Managed Payments](https://stripe.com/managed-payments) , a merchant-of-record service where Stripe takes on tax collection and remittance per transaction, and RevenueCat[supports it out of the box](https://www.revenuecat.com/docs/web/integrations/stripe/stripe-managed-payments) . From an app developer’s perspective, it’s the same entitlements, same SDK, and same Web Purchase Links; Stripe just carries the compliance burden. It adds a cost, but that was worth it to Jared:


> Thankfully I found Managed Payments and from everything I can tell, making Stripe my MoR puts all the burden of these collections and distributions on Stripe. So I'm happy to give them a few extra percent of each sale to handle that for me.


## Setting up the backend


Jared has documented his full setup, including the places he tripped up,[on his blog](https://jsorge.net/2026/07/15/revenuecat-mac-licensing) . The condensed version is short:


1. Create the entitlement in RevenueCat that the app checks for a license.
2. Add the product in the Stripe dashboard, making sure it meets Managed Payments' eligibility criteria.
3. Connect Stripe to RevenueCat as a[web provider](https://www.revenuecat.com/docs/projects/connect-a-store) , checking the["Use Managed Payments when available" box](https://www.revenuecat.com/docs/web/integrations/stripe/stripe-managed-payments) .
4. Create the RevenueCat product by importing it from Stripe.
5. Add an offering containing the product with a **lifetime** duration—effectively a one-time purchase.
6. Generate a[Web Purchase Link](https://www.revenuecat.com/docs/web/web-billing/web-purchase-links) for the offering, including the callback URL the web checkout uses to deep-link back into the app.


## Hooking up the app


The full purchase flow in Arborist looks very familiar to app users: open the License screen in the Settings, click “Buy,” complete the purchase in your browser, and automatically get returned to Arborist. That’s it.


Jared had initially hoped to keep everything in-app using a WKWebView, but a[known WebKit bug](https://bugs.webkit.org/show_bug.cgi?id=282078) prevents Apple Pay from working in that context. Instead, he accepted the small friction of jumping out to the user’s browser so customers can use Apple Pay instead of typing in card details by hand.


Another caveat he found is that the RevenueCat SDK is geared toward apps that use StoreKit and doesn’t supply the corresponding web purchase link directly. Instead, he hard-codes the checkout URLs (production and sandbox) instead. When the user clicks buy, Jared's licensing view model assembles the checkout URL (shaped like` https://pay.rev.cat/{web_link_id}/{user_id}` ) and hands it to the system browser:


```text
func   startCheckout  ()   async   {
guard   isStartingCheckout   ==   false   else   {   return   }


isStartingCheckout   =   true
actionMessage   =   nil
defer   { isStartingCheckout   =   false   }


do   {
let   checkoutURL   =   try   await   licenseManager.  checkoutURL  ()
guard   NSWorkspace.shared.  open  (checkoutURL)   else   {
throw   LicenseCheckoutOpenError.failedToOpenBrowser
}
actionMessage   =   "Checkout opened in your browser."
}   catch   {
displayState   =   LicenseDisplayState.  resolved  (
from  :   nil  ,
previous  : displayState,
error  : error
)
}
}
```


After a successful purchase, RevenueCat's backend associates the passed-in user ID with the entitlement and calls back into the app via the registered deep link. The handler re-fetches the customer and validates the entitlement:


```text
func   handleDeepLinkPurchase  ()   async   throws   {
let   customerInfo   =   try   await   Purchases.shared.  customerInfo  (  fetchPolicy  : .fetchCurrent)


guard
let   entitlement   =   customerInfo.entitlements[entitlementID],
entitlement.isActive
else   {
// Handle a missing entitlement; this means the
// transaction did not succeed
return
}


// Once we get here we have a validated customer who has
// made a purchase and the app can be unlocked.
}
```


That first line with the fetchPolicy is important:


> The fetch policy here is critical because without specifying .fetchCurrent the SDK will return cached data, and if a customer has made a purchase that won't be reflected instantly like they'll expect. Instructing the SDK to bust out of its cached data is the thing that will make this process feel seamless to my customers (and yours!).


Similar code runs at app launch to validate the user against their iCloud ID, which is how a license bought on one Mac unlocks Arborist automatically on the next.


I was thrilled to discover that not only is using RevenueCat for desktop app licensing possible, but it's actually a straightforward setup process with a relatively small amount of in-app code. There are really only about three main areas where Jared needed to make changes: the UI for offering the purchase option to users and sending them to the web, the code to handle returning from the web, and the code that runs at startup to configure everything and check for existing purchases.


Having such a simple way to offer Mac app licensing to customers is *very* exciting to me. There are some ways I can see for the SDK to make this even easier, and I hope to start addressing them. If you're writing a Mac app and looking for a simple way to offer licensing to your customers, RevenueCat can help you make money.


[Arborist launches today](https://taphouse.io/arborist) —and Jared's full write-up, including everything he learned, is[on his blog](https://jsorge.net/2026/07/15/revenuecat-mac-licensing) .
