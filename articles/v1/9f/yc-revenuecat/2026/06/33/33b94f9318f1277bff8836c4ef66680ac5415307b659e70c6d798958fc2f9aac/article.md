---
schema_version: "1.0.0"
document_id: "33b94f9318f1277bff8836c4ef66680ac5415307b659e70c6d798958fc2f9aac"
company_key: "yc-revenuecat"
company: "RevenueCat"
source_id: "yc-revenuecat-rss-41fa37a4cb36"
canonical_url: "https://www.revenuecat.com/blog/engineering/play-billing-choice"
published_at: "2026-06-23T02:28:37+00:00"
first_seen_at: "2026-07-20T23:24:09.080214+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:479632e4b73d3d261a51c4c276d969b50656684bc15e9ef873370e7b69eb076a"
---

# Understanding Google Play’s Billing Choice: a complete guide to offering alternative billing

For most of Google Play’s history, charging for digital goods inside an Android app meant one path. You integrated Google Play Billing, Google processed the payment, and Google collected its service fee.[Billing Choice](https://developer.android.com/google/play/billing/billingchoice/integration) changes the shape of that flow. It lets your app present a screen where the user picks between Google Play Billing and your own billing system, or a link out to your website, and then completes the purchase through whichever option they chose. The idea is simple to state, but the integration carries real weight: you decide who draws the choice screen and where the money is collected, and you take on an obligation to report every alternative sale back to Google.


In this article, you’ll explore what Billing Choice changes about the purchase flow, the four integration scenarios that fall out of two independent decisions, the` BillingProgram` API surface for enabling the program and checking its availability, how each scenario launches its purchase, the external transaction token that ties every alternative sale to a server side report, how subscription upgrades and downgrades behave, and the UX rules that keep the choice screen fair.


## **What Billing Choice actually changes**


Google describes the program plainly: the billing choice program lets you integrate your own billing system or guide users to your website for purchases using external web links. Whichever option you implement, the user must still be offered a choice between Google Play Billing and your alternative.


That last clause is the heart of it. Billing Choice is not “alternative billing instead of Google Play.” It is “alternative billing alongside Google Play, with the user deciding.” Google Play Billing never disappears from the screen. Your job is to present a fair choice and then honor whichever side the user taps.


The program is exposed through a` BillingProgram` enum, and the value you work with is` BillingProgram.BILLING_CHOICE` . You pass that value into nearly every Billing Choice call, which signals the shape of the API: alternative billing is configured as a named program you opt into, not a single global switch.


Two independent decisions define how your integration looks:


- **Who renders the choice screen** : Google can draw it for you (` ChoiceScreenType.GOOGLE_RENDERED` ), or you can draw your own (` ChoiceScreenType.DEVELOPER_RENDERED` ) as long as you follow the UX guidelines.
- **Where the payment happens** : the user pays inside your app through your own billing system (` DeveloperBillingType.IN_APP` ), or you send them to your website through an external web link (` DeveloperBillingType.EXTERNAL_LINK` ).


The` DeveloperBillingType` matters beyond the user experience. It is recorded inside the transaction token you generate, so Google knows whether a given alternative sale was an in app transaction or an external link transaction when it assesses the service fee.


## **The four integration scenarios**


Those two decisions combine into the four scenarios that organize the entire integration. Everything else in this guide is a variation on one of these four rows.


Scenario


Choice screen


Payment location


How the purchase launches


**1A**


Google renders it (` GOOGLE_RENDERED` )


In app (` IN_APP` )


` launchBillingFlow()` with a developer billing option enabled


**1B**


You render it (` DEVELOPER_RENDERED` )


In app (` IN_APP` )


You handle the tap yourself, then process payment and report


**2A**


Google renders it (` GOOGLE_RENDERED` )


External link (` EXTERNAL_LINK` )


` launchBillingFlow()` with a link URI and token attached


**2B**


You render it (` DEVELOPER_RENDERED` )


External link (` EXTERNAL_LINK` )


` launchExternalLink()` with a link URI and token attached


Notice the pattern. The “1” versus “2” axis is about where money is collected. The “A” versus “B” axis is about who renders the screen. When Google renders the choice screen (the A scenarios), Google needs a way to tell you the user picked your option, so you register a listener up front. When you render the choice screen (the B scenarios), you already know what the user tapped, so instead of a listener you fetch the assets you need to draw a fair screen and generate the reporting token yourself.


## **Prerequisites: library version, enrollment, and Play Console**


Before any code compiles against this API, three things need to be in place.


First, the library. Billing Choice requires Play Billing Library version 9.1 or higher. This is separate from the broader migration deadline you may already be tracking: by August 31, 2026, every new app and every update to an existing app must build against Billing Library 8 or later, with an extension available on request until November 1, 2026. That deadline is about version 8 as a floor for the whole ecosystem. Billing Choice sits well above it at 9.1, so adopting the program puts you well past that floor.


Second, enrollment. You must enroll in the program and review its requirements before calling the APIs. If you intend to offer external web links, you also declare that preference in Play Console before you ship the integration.


Third, Play Console configuration. You set your choice screen preference (whether Google renders it or you do) and your external web links preference in the Console. You can also upload an image asset that represents the payment methods your own billing option accepts, to be shown on the choice screen. That asset follows strict specifications:


Asset specification


Value


Overall image size


192dp x 20dp


Single card size


32dp x 20dp


Spacing between cards


8dp


Inner padding


3dp


Card outline


1dp inner stroke, 2dp radius, color` #E0E0E0`


Card background


Solid color, preferably white


File format


PNG with transparent background


Maximum payment methods


5


## **Enabling the billing program**


You turn on Billing Choice when you build the` BillingClient` . The new piece is` enableBillingProgram()` , which takes an` EnableBillingProgramParams` configured with` BillingProgram.BILLING_CHOICE` .


The following setup is for a scenario where Google renders the choice screen:


```text
val   params   =   EnableBillingProgramParams.  newBuilder  ()
.  setBillingProgram  (BillingProgram.BILLING_CHOICE)
.  setDeveloperProvidedBillingListener  (developerProvidedBillingListener)
.  build  ()


val   billingClient   =   BillingClient.  newBuilder  (context)
.  setListener  (purchasesUpdatedListener)
.  enablePendingPurchases  (pendingPurchasesParams)
.  enableBillingProgram  (params)
.  build  ()
```


The important detail is` setDeveloperProvidedBillingListener()` . You register this listener only when Google renders the choice screen, because Google needs a callback to reach when the user picks your billing option rather than Play’s. The` purchasesUpdatedListener` you already know that standard Billing handles the case where the user picks Google Play. The two listeners sit in different places, the familiar` PurchasesUpdatedListener` on the client and the` DeveloperProvidedBillingListener` inside` EnableBillingProgramParams` , but together they cover the two outcomes of the choice screen.


When you render your own choice screen, you omit the` DeveloperProvidedBillingListener` entirely. You build the same` EnableBillingProgramParams` with` BillingProgram.BILLING_CHOICE` , but without` setDeveloperProvidedBillingListener()` , because you handle the user’s tap yourself and never need Google to call back into your app.


## **Checking availability and reading the choice configuration**


Billing Choice is not guaranteed to be available for a given user, device, or region, and the configuration you set in Play Console is delivered to your app at runtime. You check both with` isBillingProgramAvailable()` . The Kotlin form is a suspend function that returns the result alongside an availability details object:


```text
val   (billingResult, availabilityDetails)   =
billingClient.  isBillingProgramAvailable  (BillingProgram.BILLING_CHOICE)


val   choiceDetails   =   availabilityDetails.billingChoiceAvailabilityDetails
if   (billingResult.responseCode   ==   BillingResponseCode.OK   &&   choiceDetails   !=   null  ) {
val   screenType   =   choiceDetails.choiceScreenType
val   externalLinkSupported   =   choiceDetails.isExternalLinkAvailable
\  /  \  /   Branch your integration on these two values.
}
```


The` BillingChoiceAvailabilityDetails` object answers the two questions that decide which scenario you are in. The` choiceScreenType` property is either` ChoiceScreenType.GOOGLE_RENDERED` or` ChoiceScreenType.DEVELOPER_RENDERED` , telling you who is responsible for rendering the screen. The` isExternalLinkAvailable` property tells you whether external web links are enabled for this user. A robust integration reads both values and falls back to standard Google Play Billing whenever Billing Choice is unavailable, rather than assuming the program is always on.


For callback-style code, there is also` isBillingProgramAvailableAsync()` , which delivers the same` BillingResult` and availability details to a listener instead of suspending.


## **Scenario 1A: Google renders the choice, payment happens in app**


This is the lightest scenario to integrate, because Google does most of the work. You enable the program with a` DeveloperProvidedBillingListener` , confirm availability shows` GOOGLE_RENDERED` , and then launch the flow with a developer billing option attached:


```text
val   developerBillingOptionParams   =   DeveloperBillingOptionParams.  newBuilder  ()
.  setBillingProgram  (BillingProgram.BILLING_CHOICE)
.  build  ()


val   billingFlowParams   =   BillingFlowParams.  newBuilder  ()
.  setProductDetailsParamsList  (productDetailsParamsList)
.  enableDeveloperBillingOption  (developerBillingOptionParams)
.  build  ()


billingClient.  launchBillingFlow  (activity, billingFlowParams)
```


The call to` enableDeveloperBillingOption()` is what tells Google to show the choice screen instead of going straight to Play’s payment sheet. From here the flow splits down two paths depending on what the user taps:


- **The user picks Google Play Billing.** The purchase completes through Google, and your existing` PurchasesUpdatedListener` receives the result exactly as it would without Billing Choice.
- **The user picks your billing option.** Google invokes the` DeveloperProvidedBillingListener` you registered and hands you a` DeveloperProvidedBillingDetails` object that carries an` externalTransactionToken` . That token is your receipt to take payment through your own system and later report the sale.


The split is the whole point. One code path keeps the familiar Google Play purchase, and the other hands control to you with a token already minted. You did not have to draw a screen or generate the token yourself, because in this scenario Google did both.


## **Scenario 1B: You render the choice, payment happens in app**


When you render the choice screen, you trade convenience for control. You no longer register a` DeveloperProvidedBillingListener` , because there is no Google screen to call you back from. Instead, you gather the pieces you need to draw a screen that treats Google Play fairly, then mint your own token when the user chooses you.


The first piece is the Google Play side of the screen. You ask Billing for the image and loyalty message to display for the Play option with` getBillingChoiceInfo()` :


```text
val   params   =   GetBillingChoiceInfoParams.  newBuilder  ()
.  setBillingProgram  (BillingProgram.BILLING_CHOICE)
.  setPlayBillingChoiceImageLayout  (GetBillingChoiceInfoParams.ImageLayout.RECTANGULAR_FOUR_BY_ONE)
.  build  ()


val   (billingResult, info)   =   billingClient.  getBillingChoiceInfo  (params)
if   (billingResult.responseCode   ==   BillingResponseCode.OK   &&   info   !=   null  ) {
val   imageUrl   =   info.playBillingChoiceImageUrl
val   loyaltyMessage   =   info.playBillingLoyaltyInfo
\  /  \  /   Render these into your own choice screen.
}
```


The` playBillingChoiceImageUrl` is the payment method artwork for Google Play, and` playBillingLoyaltyInfo` is any loyalty message Google wants shown beside its option, such as a Play Points rewards message. You must load these every time you draw the screen rather than caching them, a requirement covered in the UX section below. For callback style code there is also` getBillingChoiceInfoAsync()` , which delivers the same` PlayBillingChoiceInfo` to a listener, and that is the form the UX guidelines reference.


Once the user taps your option, you generate a reporting token with` createBillingProgramReportingDetails()` , declaring the billing type as in-app:


```text
val   params   =   BillingProgramReportingDetailsParams.  newBuilder  ()
.  setBillingProgram  (BillingProgram.BILLING_CHOICE)
.  setDeveloperBillingType  (DeveloperBillingType.IN_APP)
.  build  ()


val   (billingResult, details)   =
billingClient.  createBillingProgramReportingDetails  (params)
if   (billingResult.responseCode   !=   BillingResponseCode.OK)   return
val   transactionToken   =   details?.externalTransactionToken
```


Before you charge the user, you also call` showBillingProgramInformationDialog()` , which presents the legally required disclosure for transacting outside Google Play Billing. The dialog request carries the` BillingProgram` and the transaction token you just generated. After the disclosure, you run your own payment flow and keep the` externalTransactionToken` for the server-side report that follows every alternative sale.


## **Scenarios 2A and 2B: Sending users to an external web link**


The external link scenarios replace your in app payment with a redirect to your website. In both of them you generate the token yourself with` createBillingProgramReportingDetails()` , declaring` DeveloperBillingType.EXTERNAL_LINK` instead of` IN_APP` . This is the one place the A versus B shorthand bends: even in Scenario 2A, where Google renders the choice screen, you mint the token before launching rather than receiving it through the listener, because the token has to travel with the redirect. You should only attempt these scenarios when availability reported` isExternalLinkAvailable` as true.


In **Scenario 2A** , Google renders the choice screen, so you still launch through` launchBillingFlow()` . The difference is that your developer billing option now carries the destination URI, the token you just generated, and a launch mode:


```text
.  setBillingProgram  (BillingProgram.BILLING_CHOICE)
.  setLinkUri  (Uri.  parse  (  "https:  \/\/  www.example.com  \/  external  \/  purchase"  ))
.  setExternalTransactionToken  (transactionToken)
.  setLaunchMode  (DeveloperBillingOptionParams.LaunchMode.LAUNCH_IN_EXTERNAL_BROWSER_OR_APP)
.  build  ()
```


You attach this to` BillingFlowParams` with` enableDeveloperBillingOption()` and call` launchBillingFlow()` as before. If the user picks Google Play, the normal purchase happens. If they pick your option, Google opens your link and the token travels with the redirect.


In **Scenario 2B** , you render the choice screen yourself, so there is no billing flow to launch. You send the user out with a dedicated call,` launchExternalLink()` :


```text
val   params   =   LaunchExternalLinkParams.  newBuilder  ()
.  setBillingProgram  (BillingProgram.BILLING_CHOICE)
.  setLinkUri  (yourLinkUri)
.  setLinkType  (LaunchExternalLinkParams.LinkType.LINK_TO_DIGITAL_CONTENT_OFFER)
.  setLaunchMode  (LaunchExternalLinkParams.LaunchMode.LAUNCH_IN_EXTERNAL_BROWSER_OR_APP)
.  setExternalTransactionToken  (transactionToken)
.  build  ()


billingClient.  launchExternalLink  (activity, params) { billingResult   ->
if   (billingResult.responseCode   ==   BillingResponseCode.OK) {
\  /  \  /   The user   is   on the way to your site. Report the sale   when   it completes.
}
}
```


The` LinkType.LINK_TO_DIGITAL_CONTENT_OFFER` describes what waits on the other side of the link, and the launch mode opens it in an external browser or app. The result callback only confirms that the link launched successfully. It does not tell you the purchase succeeded, because that happens on your website, outside the app. Completing and recording the sale is your responsibility from this point on.


## **The external transaction token: reporting every alternative sale**


Across all four scenarios, the same object keeps appearing: the` externalTransactionToken` . It is the thread that connects a tap on the choice screen to a line item in your Play revenue report, and understanding it is what separates a working integration from a compliant one.


Every alternative billing transaction, whether it happened in app or through an external link, must be reported back to Google Play. The token is how Google links your report to the original choice. It also encodes the` DeveloperBillingType` , so Google knows whether to treat the sale as an in app transaction or an external link transaction when it calculates the service fee. Where the token comes from depends on the scenario: in Scenario 1A it arrives through the` DeveloperProvidedBillingListener` , inside the` DeveloperProvidedBillingDetails` object, and in the other scenarios you generate it yourself with` createBillingProgramReportingDetails()` .


The report itself is a server side call to the Google Play Developer API, on the` externaltransactions` resource. After the user pays through your system, your backend calls` createexternaltransaction` with the token in the request body. The body differs by product type. A one time product uses a` oneTimeTransaction` wrapper, while a subscription uses a` recurringTransaction` wrapper that also declares the subscription type:


```text
{
"recurringTransaction"  : {
"externalTransactionToken"  :   "the_token_from_the_client"  ,
"externalSubscription"  : {   "subscriptionType"  :   "RECURRING"   }
}
}
```


There is a distinction worth holding onto for subscriptions. The token is required for the first transaction in a recurring purchase, but renewals do not get a fresh token. Instead, you report each renewal against the original sale using` initialExternalTransactionId` , the identifier of that first transaction. The API also exposes` getexternaltransaction` to read a transaction back and` refundexternaltransaction` to report a refund, so the full lifecycle of an alternative sale is mirrored on Google’s side.


The service fee that results from these reports varies by transaction type and by geography, with the European Economic Area called out specifically, and the published rates live in Google’s external offers program documentation rather than in the Billing Library reference. The takeaway for your architecture is that choosing alternative billing does not remove the service fee. It moves the responsibility for declaring each sale onto your own backend.


## **Subscription upgrades and downgrades**


Subscriptions add one more case: what happens when a user who already bought through your billing system upgrades or downgrades their plan. You do not want to show the choice screen again, because the user already made their choice on the original purchase, and Billing Choice preserves it.


You signal this by carrying the original transaction’s identifier into the replacement flow with` setOriginalExternalTransactionId()` on` SubscriptionUpdateParams` :


```text
.  setBillingProgram  (BillingProgram.BILLING_CHOICE)
.  build  ()


val   billingFlowParams   =   BillingFlowParams.  newBuilder  ()
.  setProductDetailsParamsList  (newPlanProductDetailsList)
.  setSubscriptionUpdateParams  (
BillingFlowParams.SubscriptionUpdateParams.  newBuilder  ()
.  setOriginalExternalTransactionId  (externalTransactionId)
.  build  ()
)
.  enableDeveloperBillingOption  (developerBillingOptionParams)
.  build  ()


billingClient.  launchBillingFlow  (activity, billingFlowParams)
```


Because the original choice is preserved, no choice screen appears for the replacement. The user moves directly into the new plan through the billing system they already selected. Two identifiers are in play here, and they are not the same thing.` setOriginalExternalTransactionId()` runs on the client and names the prior purchase this upgrade replaces, which is what lets Google skip the choice screen.` initialExternalTransactionId` is a server side reporting field that chains a subscription’s renewals back to its own first transaction. So the new plan is still a new purchase: once the upgrade or downgrade completes, you report it as a new transaction with its own` externalTransactionToken` , and that transaction then becomes the` initialExternalTransactionId` its later renewals reference.


## **Designing a fair choice screen**


When you render your own choice screen (the B scenarios), the UX guidelines stop being advice and become requirements you agreed to by declaring` DEVELOPER_RENDERED` in Play Console. The single principle behind every rule is that the user must be able to compare the two options fairly, with no thumb on the scale toward your billing system.


The concrete requirements:


- **Label who is responsible.** Each option must clearly identify the authorized entity, app name, or developer name, so the user understands who fulfills the purchase and handles support.
- **Show the right icons.** Display the Google Play icon in full color against a neutral light or dark background per Google’s brand guidelines, and display only your own app icon or brand logo for your option. You may not dress your option in Google’s branding.
- **Fetch payment method logos live.** Use the assets from` getBillingChoiceInfoAsync()` to show Google Play’s payment methods, and treat your own payment methods the same way. You must fetch and display these each time and never cache or alter them. If you cannot determine your own payment methods, you may omit them, but you must still show Google Play’s.
- **Keep the buttons equal and close.** Button sizes, text size, font style, contrast, tap targets, and in-button logo sizing should be equivalent, and the two buttons must sit in close proximity so the user can compare them side by side.
- **Use equivalent calls to action.** The primary button labels must be equivalent and consistent, for example “Pay with Google Play” next to “Pay with \[your service\].” You may present differentiated offers, but if you do, Google Play’s loyalty benefits must be shown in an equivalent manner.
- **Treat loyalty equally.** If you display loyalty information, show it for both options with equal treatment, and pull Google Play’s loyalty message from` getBillingChoiceInfoAsync()` each time without caching it.
- **Disclose the redirect.** If your option sends the user to a website, you must clearly and prominently inform them they are leaving the app, with explicit wording such as “You’ll be redirected to a web page.”


A nuance to note: the guidelines phrase the broad visual equality clause with “should,” while proximity, equivalent calls to action, in button logo sizing, and the no caching rules use “must.” When you are uncertain whether something is mandatory, the safe reading is to treat the equality requirements as binding, because the program’s entire purpose is to avoid nudging users away from Google Play Billing.


## **Supervised users and parental controls**


One behavior is handled for you. For supervised users, Google Play automatically displays the appropriate parental control screen during the Billing Choice flow. The integration notes call this out for each of the four scenarios, so expect it whether Google renders the choice screen or you do. You do not build or trigger it yourself, so it is worth knowing the extra screen exists before it surprises you during testing with a supervised account.


## **What this means for your monetization**


Stepping back from the API, Billing Choice is a trade. Google Play Billing handed you a great deal of infrastructure for its fee: payment processing, tax handling, receipts, refunds, renewals, fraud protection, and the entire reporting pipeline. Billing Choice lets you take some of that back, but only by taking on the work that came with it.


When a user picks your option, you own the payment processing, the fulfillment, the customer support, the refunds, and the server side reporting that keeps you compliant. The service fee does not vanish, it is assessed from the transactions you report through the` externaltransactions` API. And Google Play Billing remains on the screen as a first-class choice for every user, every time, which means your alternative has to earn its selection rather than being the only door.


That framing helps decide which scenario fits. If you mainly want to reduce friction without standing up a payment stack, the Google rendered scenarios (1A and 2A) carry the least integration burden, because Google draws the screen and mints the token. If you already run billing infrastructure on the web and want to route users there, an external link scenario (2A or 2B) reuses what you have. The fully developer rendered, in app scenario (1B) gives you the most control over the experience and demands the most from you in return: a compliant choice screen, your own payment flow, the disclosure dialog, and the reporting backend, all built and maintained by your team.


## **Conclusion**


In this article, you’ve explored how Billing Choice reshapes the Android purchase flow into a fair choice between Google Play Billing and your own alternative. You saw the two decisions that produce the four integration scenarios, the` BillingProgram` API for enabling the program and reading its availability, how each scenario launches its purchase, the external transaction token that ties every alternative sale to a` createexternaltransaction` report, how subscription replacements preserve the user’s original choice, and the UX rules that govern a screen you render yourself.


Understanding these internals helps you make the decision Billing Choice actually asks of you, which is not “should I use alternative billing” but “which of the four scenarios matches what my team can build and operate.” The token flow, the reporting obligation, and the fairness requirements are the real cost of admission, and seeing them clearly up front is what prevents an integration that works in a demo but falls out of compliance in production.


Whether you are adding a single external link to an existing web checkout, building a full in app billing system behind your own choice screen, or simply letting Google render the choice while you collect payment, this foundation lets you reason about the program as a set of deliberate trade offs rather than a single switch to flip. Choose the scenario that fits, report every sale, and keep the choice honest.
