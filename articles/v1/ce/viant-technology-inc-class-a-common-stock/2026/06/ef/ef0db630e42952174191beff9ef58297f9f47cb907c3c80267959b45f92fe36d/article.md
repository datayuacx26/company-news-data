---
schema_version: "1.0.0"
document_id: "ef0db630e42952174191beff9ef58297f9f47cb907c3c80267959b45f92fe36d"
company_key: "viant-technology-inc-class-a-common-stock"
company: "Viant Technology Inc."
source_id: "viant-technology-inc-class-a-common-stock-rss-49e83a43fd49"
canonical_url: "https://www.viantinc.com/insights/blog/a-direct-path-to-bad-signal-is-still-a-bad-path/"
published_at: "2026-06-17T21:27:52+00:00"
first_seen_at: "2026-07-20T23:18:49.205623+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:7800429c6bcf62c63d1f45c4f5c9187b08b02e57ed03f8945beb4bb32d6c1fab"
---

# A Direct Path to Bad Signal Is Still a Bad Path

# A Direct Path to Bad Signal Is Still a Bad Path


June 17, 2026 in[Blog](https://www.viantinc.com/insights/blog/)


*Last week, Viant launched its enhanced Publisher Solutions. The headline most people will reach for is that it’s free to publishers. That’s true, but in my opinion, that’s the least interesting thing about it.*


For the last three years, the industry has been having an argument about supply paths, but it has mostly been the wrong argument.


The argument has been about length: how many hops between the publisher and the bidder, whether your path is “direct”, and whether you’ve cut out the middle. Length became the proxy for quality the same way a deal ID became the proxy for premium— a label that’s easy to market and hard to verify.


But a direct path to bad signal is still a bad path. What matters is not how short the route is, but whether the bidder receives the complete, unaltered information needed to make an intelligent decision at the end of that route.


When advertisers receive cleaner signal, they make better decisions. When they make better decisions, publishers capture more value from their inventory. And when both sides perform better, the platform benefits through growth rather than through additional fees.


That distinction is the entire reason[Viant Publisher Solutions](https://www.viantinc.com/solutions/publisher/) exists.


## Why supply, and why now


The open web is getting smaller.


AI search and zero-click answers are diverting traffic that used to land on publisher pages. Mid- and long-tail publishers are hit hardest, because they were the ones most dependent on search referral to sustain an ad-supported model. As that inventory thins, the programmatic supply that rides on it thins with it. The impressions that remain are worth more, and the cost of being wasteful with them goes up.


At the same time, the structure of the chain hasn’t gotten cheaper. The[ANA](https://www.ana.net/content/show/id/media-programmatic-transparency?) has put open-web waste at roughly $20 billion a year, with only about 36 cents of each dollar reaching the publisher once fees, hops, and infrastructure are accounted for. Some of that is legitimate infrastructure cost, but some of it is margin taken by platforms that are supposed to be neutral. The two are not always clearly labeled.


You can read that as a fee problem. I read it as a signal problem, with a fee problem attached.


When a bid request passes through an exchange hop, things get stripped or rewritten along the way: the real IP address, the supply-chain validation, the content metadata, the identity match. By the time the bidder sees the request, it may be evaluating a fundamentally different impression than the one the publisher actually sold. The fee is what you pay, and the degraded signal is what you’re left holding.


So the question we started with in late 2025 wasn’t “how do we charge less.” It was “how do we make sure the bidder sees what the publisher actually sent?” Everything else followed from that.


## What we actually built


Viant Publisher Solutions has four components that do one job: get clean, complete signal to the bidder and tell the publisher exactly what the bidder sees.


**Direct Access** is the path. It’s an exchange-less route between a publisher’s inventory and the Viant DSP, interoperable with any ad server and with oRTB and server-side Prebid. Removing the exchange hop isn’t the point in itself. The point is that when the hop is gone, the bidder receives the publisher’s full, unaltered request: real IP, complete supply-chain data, identity, content metadata. Think of directness as the delivery vehicle and the signal as the payload; the less interference along the journey, the more value arrives at the other end.


**SupplyIQ** is the feedback layer, and it’s the piece I care most about. It shows a publisher exactly how their inventory appears to our bidder: where signal coverage is strong, where it’s degraded, and what to fix. For years the relationship between a DSP and a publisher has been a black box in both directions. SupplyIQ turns it into a measurable loop to improve the signal so that the bidder can value the inventory more accurately. That’s not only a benefit to publishers, but the mechanism that makes the whole thing work.


**Household ID** and **IRIS_ID** are the signal itself. HHID is deterministic, household-level identity that survives without cookies, so first-party data stays addressable and attributable across devices. IRIS_ID maps CTV content to standardized identifiers, so video inventory becomes something you can target and measure at the level of the content, not just the app bundle.


I’ve made the argument elsewhere that[the segment is not the signal](https://keithpetri.com/2026/04/07/the-segment-is-not-the-signal/) , that handing a model a pre-computed conclusion is structurally different from giving it the evidence that produced one. This is the constructive version of that argument: preserve the raw inputs deeper into the decisioning surface instead of collapsing them upstream. That’s what these four pieces do together.


## The decision we left out


Now let’s discuss the fee.


We didn’t build one. This is not a promotion or a limited-time offer to win share. We looked at the architecture and decided a publisher fee was a line we wouldn’t cross, because of what it would have done to our own incentives.


If a platform charges a publisher a percentage of advertiser spend to access demand, that platform now has a financial interest in routing as much spend as possible through the fee-bearing path because that’s where its margin lives. That creates structural pressure to consolidate demand through itself, and puts the publisher in a dependent position relative to a platform that profits from how their inventory gets routed. It’s the same conflict you see when[an SSP builds its own demand path](https://keithpetri.com/2026/05/21/who-owns-the-pipes-matters/) and then competes with the buyers it’s supposed to serve. The take rate and the conflict are the same object viewed from two sides.


We removed that object from the design.


The fair question is what we get in return. When Direct Access removes the exchange hop, our bidder gets a smarter, cleaner request. Smarter bidding produces better campaign performance, and better performance keeps advertisers on the platform and grows the business. That is the return. It’s a different model than taking margin on both sides of a transaction, but it is a model, and it’s arguably a more durable one because it aligns our incentive with the advertiser’s outcome instead of with the volume of spend we can tax on the way through.


It’s important to be clear about who Viant serves. We are a buy-side platform, and our customers are advertisers. Publishers are not our clients.We built publisher-facing tools because publishers control many of the signals that help advertisers achieve better outcomes. Giving publishers a direct way to share those signals improves performance for our customers while creating more value for the publisher.


That doesn’t limit publisher choice. A publisher using Viant Publisher Solutions can still work with any DSP, SSP, or private marketplace. The integration creates an additional path to demand; it doesn’t replace or restrict existing ones.


## What the numbers say


Viant’s enhanced Publisher Solutions model is already running at scale. As of this launch, 85% of CTV spend on the Viant platform is transacting through Direct Access, making it the dominant path for the majority of our CTV business. Direct Access charges publishers nothing and returns the exchange-fee savings to the advertiser’s campaign as working media. Overall, Viant revenue grew 25% year over year in Q1, and ad spend linked to Household ID grew 33% over the same period. The adoption of the signal and the growth of the business are moving together, which is exactly what you’d expect when the alignment argument is real and exactly what you wouldn’t see if it were purely marketing dust.


Partners are saying it more plainly than I can. Tubi’s Vijay Rao framed Direct Access as a way to collaborate more closely, improve signal quality, and unlock incremental revenue without additional platform fees. Brad Feinberg at Molson Coors put the identity side of it in terms of future-proofing marketing effectiveness as the cookie goes away. Those are the two ends of the same pipe: a publisher and an advertiser describing the same clean connection from opposite sides.


## The part that’s bigger than us


What Viant Publisher Solutions is really trying to prove is a claim about the open internet, not just about Viant.


The claim is that there’s a viable model for the open web that doesn’t depend on intermediaries taking margin on both sides of a transaction they’re supposed to be neutral on. Advertisers get better signal and more working media, publishers get transparent access to demand without a fee, and the platform gets a performance advantage that compounds as it grows. If that model holds at scale — and the share of our CTV business now running through it says it does — then the burden of proof shifts. The question stops being “why would you do this for free” and becomes “what is everyone else’s fee actually buying.”


I don’t think the path was ever the point. The path is plumbing, but the signal is the intelligence that makes the plumbing worth building. We spent the last nine months building the cleanest version of that we could, deciding what to leave out as carefully as what to put in, and the thing we left out turned out to be the easiest decision of the whole project.


A direct path to bad signal is still a bad path. We built the other thing.


AUTHOR: Keith Petri


[More About Keith](https://www.viantinc.com/authors/kpetri/)


As SVP of Data, Identity + Supply at Viant (NASDAQ: DSP) and CEO of Lockr by Viant, Keith Petri drives innovation in identity and privacy-first data collaboration, shaping the future of addressability and measurement across digital media. Keith helps advertisers unlock the value of first-party data while advancing Viant’s broader vision around the company’s core pillars of addressability, AI, and CTV. His work strengthens supply quality and transparency across the ecosystem, driving better outcomes for advertisers and publishers alike. A data-driven executive with more than 15 years of experience at the intersection of emerging technology, digital privacy, and advertising, Keith is passionate about building businesses, challenging the status quo, and driving operational impact—both as a founder and post-acquisition executive. Keith founded Lockr to help enterprises ethically unlock the value of their first-party data, prioritizing long-term business growth over short-term gains for adtech intermediaries. Prior to founding Lockr, he served as Chief Strategy Officer at Screen6, where he helped establish the industry standard for cross-device identity resolution. Following Screen6’s acquisition by Samba TV, he led platform strategy, integrating identity management with advanced TV analytics to improve audience measurement across a fragmented media landscape. Keith has built and scaled multiple ventures, including Human Demand (acquired by IgnitionOne), Screen6 (acquired by Samba TV), and Lockr (acquired by Viant).


Share this with your friends:


## Related Insights


[Programmatic Ad Targeting: Key Types and How They Improve Audience Reach Read More](https://www.viantinc.com/insights/blog/programmatic-ad-targeting-types-effective-methods-for-audience-targeting/)[Deterministic Householding: Precise Targeting Read More](https://www.viantinc.com/insights/blog/deterministic-householding/)[Brand Safety vs Brand Suitability: What Advertisers Need to Know Read More](https://www.viantinc.com/insights/blog/brand-safety-vs-brand-suitability/)[Attention Is Earned: Here’s How Advertisers Can Win It. Read More](https://www.viantinc.com/insights/blog/attention-is-earned-heres-how-advertisers-can-win-it/)[Viant Integrates with Publica to Expand Premium Connected TV Access Read More](https://www.viantinc.com/insights/blog/viant-integrates-with-publica-to-expand-premium-connected-tv-access/)[Havas & Viant Bring the Attention Marketplace to Life for Brands That Demand More Read More](https://www.viantinc.com/insights/blog/havas-viant-bring-the-attention-marketplace-to-life/)[A Direct Path to Bad Signal Is Still a Bad Path Read More](https://www.viantinc.com/insights/blog/a-direct-path-to-bad-signal-is-still-a-bad-path/)


## Frequently Asked Questions


What is a cookieless DSP?Expand


A cookieless DSP is a programmatic advertising platform that identifies and reaches audiences using durable, privacy-first identifiers rather than third-party cookies.


Instead of relying on fragile browser tracking, these platforms leverage advanced solutions like Household ID™ to maintain scale and precision. This approach ensures that digital campaigns remain effective and measurable regardless of evolving browser restrictions or privacy regulations.


How does programmatic advertising work without cookies?Expand


Programmatic advertising functions without cookies by activating first-party data through identity resolution frameworks like Unified ID 2.0 and the Household ID. Marketers also utilize contextual targeting to align ads with real-time content and prioritize natively cookieless channels such as CTV and DOOH. These methods allow for high-impact reach and frequency management without the need for invasive user-level tracking.


What is the difference between a DSP and a cookieless DSP?Expand


Traditional DSPs rely on syncing third-party cookies across exchanges to build audience profiles, a process that is increasingly fragmented and unreliable. A cookieless DSP uses people-based identity, first-party data, and privacy-safe signals to reach consumers across all devices.


Viant DSP was purpose-built to be cookieless from the ground up, ensuring it never relied on the legacy infrastructure of third-party cookies.


What targeting options are available in a cookieless environment?Expand


Marketers in a cookieless environment can utilize a robust suite of options including contextual targeting, first-party data onboarding, and household-level identity resolution.


Furthermore, interoperability with alternative ID frameworks like Unified ID 2.0 allows brands to maintain significant scale across the open web. These diverse tactics provide a durable, privacy-forward alternative to outdated cookie-based audience segments.


How is campaign performance measured in a cookieless world?Expand


In a cookieless world, campaign measurement is achieved through modern attribution models such as incrementality testing, Marketing Mix Modeling (MMM), and secure data clean rooms.


The Household ID serves as a critical cross-device attribution layer, linking high-impact exposures on CTV to conversions on other digital devices within the home. This transition allows advertisers to prove ROI and optimize spend without relying on fragmented, cookie-based signals.


## STAY IN THE LOOP WITH OUR NEWSLETTER


Sign up to get Viant news and announcements delivered straight to your inbox.


Sign up to get Viant news and announcements delivered straight to your inbox.
