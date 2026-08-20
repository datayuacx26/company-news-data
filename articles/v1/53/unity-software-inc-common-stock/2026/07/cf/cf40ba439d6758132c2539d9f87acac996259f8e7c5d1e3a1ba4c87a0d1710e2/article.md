---
schema_version: "1.0.0"
document_id: "cf40ba439d6758132c2539d9f87acac996259f8e7c5d1e3a1ba4c87a0d1710e2"
company_key: "unity-software-inc-common-stock"
company: "Unity Software Inc."
source_id: "unity-software-inc-common-stock-rss-726793b11211"
canonical_url: "https://unity.com/blog/mobile-game-webshop-business-case"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-07-29T08:59:29.142450+00:00"
fetched_at: "2026-07-29T08:59:30.880208+00:00"
content_hash: "sha256:6f1aa340a4087a5313c019366ef9363488e4e14e57e2054c7a22d66175db0c51"
---

# Why go D2C now: The mobile game webshop business case

Direct-to-consumer (D2C) webshops are no longer an experiment. They have become an established and growing revenue channel across mobile games. In 2025, top 100 titles grew D2C earnings by an estimated 38%*. Most top mobile social casino games now operate a webshop, 80% of top Strategy games do too, as do 75% of top Action games**.


D2C has moved from experiment to standard operating practice.


But for years, the idea of selling in-game items outside the app store was more theoretical than practical. Platforms held all the cards. If studios wanted players to make an in-app purchase ([IAP](https://unity.com/resources/in-app-purchases-guide) ), they spent through Apple or Google, and 20-30% of every transaction was the cost of doing business.


That model is no longer the only option. Platform changes, maturing tooling, and many studios proving the concept at scale have converged to make now the most compelling moment yet to launch a D2C webshop.


## What’s changed with platform policies across markets


The shift began not with the players, but with the platforms. Under legal and regulatory pressure across major markets, Apple and Google have adjusted the rules that barred developers from steering players to their own payment channels***.


As a result, in the United States, European Union, Japan and South Korea, Apple and Google can no longer require developers to sell exclusively through their in-app purchase systems. Studios in these markets can now sell in-game items through a webshop operated outside of the platforms and direct players to it from within the app.


This represents a structural change, and the takeaway for studios operating in these markets is clear. Studios now have a choice about where purchases happen, and consequently what fees they pay and how much control they have over the purchase flow.


## The business case for D2C webshops for mobile games


With these platform changes, D2C webshops have unlocked significant revenue for mobile game studios and better value for players through discounts and new items. As a result, they’ve fast become an established revenue channel for many studios.


But fees have not disappeared entirely, and where they apply depends on three things: how the transaction is routed, which platform it's on, and where the player is located:


- **In-game link-outs:** when a player taps a link inside the game to reach your webshop, the purchase is attributed to that in-app click, and Apple or Google may apply a commission, up to ~20% depending on platform and region**** in addition to payment processing and merchant of record fees.
- **Direct web traffic:** when a player reaches your webshop without an in-app link - by typing the URL, using a bookmark, or following a link shared outside the game - that transaction carries no in-app attribution, so no platform commission applies. Your only cost is payment processing (or merchant-of-record fees, where you use one).


What the platform fee structure might look on a $10 purchase as of July 2026 in the US/EU*****


Note: webshop requires developers to use their own billing system and a payment provider or merchant of record that charges a fee not applied to the net revenue column (~3–5%).


**Channel**


**Google Play Store platform fee (US/EU)**


**Apple App Store platform fee (US/EU)**


**Net revenue to developer (Play Store)**


**Net revenue to developer (App Store)**


In-app purchase


~30%


~30%


$7.00


$7.00


Webshop (embedded webview, Android only)


Standard fee minus 3–4%: ~26–27%


N/A


$7.00–7.10


N/A


Webshop (via in-game link-out)


US: 0%6 EEA: 3% acquisition + 10% Tier 1 service fee + optional Tier 2 service fee ≈ 13% to mid-20s%


US: 0%6 EU: 2% acquisition + 5–13% store services + €0.50/ Core Technology Fee over 1M installs ≈ 7–15%


US: $9.50–9.60 EEA: ~$7.40–8.40


US: $9.50–9.60 EU: ~$8.10–9.00


Webshop (direct traffic)


0%


0%


$10


$10


**Channel**


**Channel**


**Google Play Store platform fee (US/EU)**


**Apple App Store platform fee (US/EU)**


**Net revenue to developer (Play Store)**


**Net revenue to developer (App Store)**


In-app purchase


**Google Play Store platform fee (US/EU)**


~30%


**Apple App Store platform fee (US/EU)**


~30%


**Net revenue to developer (Play Store)**


$7.00


**Net revenue to developer (App Store)**


$7.00


**Channel**


**Google Play Store platform fee (US/EU)**


**Apple App Store platform fee (US/EU)**


**Net revenue to developer (Play Store)**


**Net revenue to developer (App Store)**


Webshop (embedded webview, Android only)


**Google Play Store platform fee (US/EU)**


Standard fee minus 3–4%: ~26–27%


**Apple App Store platform fee (US/EU)**


N/A


**Net revenue to developer (Play Store)** $7.00–7.10


**Net revenue to developer (App Store)**


N/A


**Channel**


**Google Play Store platform fee (US/EU)**


**Apple App Store platform fee (US/EU)**


**Net revenue to developer (Play Store)**


**Net revenue to developer (App Store)**


Webshop (via in-game link-out)


**Google Play Store platform fee (US/EU)**


**Apple App Store platform fee (US/EU)**


**Net revenue to developer (Play Store)**


US: $9.50–9.60 EEA: ~$7.40–8.40


**Net revenue to developer (App Store)**


US: $9.50–9.60 EU: ~$8.10–9.00


**Channel**


**Google Play Store platform fee (US/EU)**


**Apple App Store platform fee (US/EU)**


**Net revenue to developer (Play Store)**


**Net revenue to developer (App Store)**


Webshop (direct traffic)


**Google Play Store platform fee (US/EU)**


0%


**Apple App Store platform fee (US/EU)**


0%


**Net revenue to developer (Play Store)** $10


**Net revenue to developer (App Store)**


$10


Even if you incentivize players with a 10–15% discount to bring them to the webshop through in-game link-outs, you still will likely net more per transaction than the equivalent in-app purchase. The incentive becomes a marketing cost that also improves the player experience - offering access to premium content at a discount. For high-spending players who buy frequently, that compound effect is substantial.


But the margin improvement is only part of the story. Operating a webshop unlocks structural advantages that the platforms don't allow.


With D2C webshops you gain pricing flexibility. Platforms typically enforce minimum price tiers, but on the web, studios set the price that makes sense for their players and economy. This opens up price points that aren't viable in-app, enables more granular discount structures, and gives back control over bundle and limited-time offer pricing.


## How D2C solutions and infrastructure are empowering studios


Another element that sets the stage for successfully implementing a D2C channel is practical: the infrastructure to build and run a webshop has never been more accessible.


Solutions with D2C commerce capabilities, like[Unity IAP 5.4](https://unity.com/products/iap) , empower studios to take control of their entire commerce stack without the overhead that once made D2C feel out of reach for all but the largest teams. Managing on and off-platform commerce has historically meant juggling multiple SDKs, rebuilding integrations every time a platform requirement changed, and piecing together revenue data from siloed systems.


That picture has changed considerably. The tooling available to studios today is meaningfully more mature, with solutions that abstract away much of the underlying complexity. No-code and low-code webshop builders have reduced the technical lift of standing up a storefront.[Payment provider ecosystems](https://www.youtube.com/watch?v=_szt-lgU_64) have expanded, with more options for handling compliance, fraud detection, and regional configuration without custom engineering work. And reporting has improved, with more solutions offering aggregated views across native stores and web channels rather than forcing studios to reconcile data manually.


What used to require a dedicated commerce engineering team is now achievable for studios of almost any size. D2C is no longer a channel reserved for publishers with the resources to build and maintain it from scratch.[The ecosystem of solutions has caught up to the opportunity](https://youtu.be/XAGc_D2SGG0?si=OfoabrmwPFbnCOQc) .


[Explore Unity IAP →](https://unity.com/products/iap)


*[https://appmagic.rocks/research/mobile-landscape-report-2026](https://appmagic.rocks/research/mobile-landscape-report-2026)


**[https://www.appcharge.com/blog/mobile-game-web-store-report](https://www.appcharge.com/blog/mobile-game-web-store-report)


***EU: Digital Markets Act, Regulation (EU) 2022/1925. Japan: Mobile Software Competition Act (effective Dec. 2025). South Korea: Telecommunications Business Act, as amended (2021). US: *Epic Games, Inc. v. Apple Inc.* , 147 F.4th 917 (9th Cir. 2025), cert. granted (U.S. June 30, 2026); *In re Google Play Store Antitrust Litigation* , No. 3:21-md-02981 (N.D. Cal.), joint motion to modify withdrawn July 15, 2026.


****As of July 2026, no platform commission is currently being collected on US link-out purchases. Following the April 2025 federal injunction in *Epic v. Apple* , Apple cannot charge a commission on US external-link purchases; the Ninth Circuit held the total ban overbroad in December 2025 and remanded for a "reasonable" fee to be set, which has not yet been determined. Under Google's October 29, 2025 injunction-compliance policy, Google is likewise not currently collecting fees on US external-link or alternative-billing transactions; Google has announced an intended fee (up to ~20% on external links) that is not yet in effect. Sources: *Epic Games, Inc. v. Apple Inc.* (N.D. Cal. 2025; 9th Cir. Dec. 2025); Google Play Console policy update (Oct. 29, 2025 / Dec. 2025)


*****[https://www.revenuecat.com/blog/engineering/app-to-web-purchase-guidelines/](https://www.revenuecat.com/blog/engineering/app-to-web-purchase-guidelines/)
