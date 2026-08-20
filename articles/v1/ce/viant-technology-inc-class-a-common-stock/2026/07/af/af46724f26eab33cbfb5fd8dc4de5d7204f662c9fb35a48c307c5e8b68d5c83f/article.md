---
schema_version: "1.0.0"
document_id: "af46724f26eab33cbfb5fd8dc4de5d7204f662c9fb35a48c307c5e8b68d5c83f"
company_key: "viant-technology-inc-class-a-common-stock"
company: "Viant Technology Inc."
source_id: "viant-technology-inc-class-a-common-stock-rss-49e83a43fd49"
canonical_url: "https://www.viantinc.com/insights/blog/deterministic-householding/"
published_at: "2026-07-10T16:04:00+00:00"
first_seen_at: "2026-07-24T10:22:44.473327+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:5379ce095facf6da3a56467acadc3cd64919f1fafb92ff2a6823766a5511f502"
---

# Deterministic Householding: Precise Targeting

# Deterministic Householding: Precise Targeting


July 10, 2026 in[Blog](https://www.viantinc.com/insights/blog/)


When you track 55 billion ad requests in a month, you see a comprehensive and clear picture of the latest trends in digital advertising.


Traditional identifiers like third-party cookies and MAIDs enabled 1:1 targeting and measurement, though often probabilistically. Yet, as consumers have become more aware and protective of how their data is used, privacy changes have made 1:1 targeting and attribution measurement much more difficult.


Said simply: with third-party cookies and MAIDs continuing to decline at a precipitous rate, marketers still need to adapt.


## Deterministic Householding vs. Probabilistic Identity: What’s the Difference?


As the industry shifts away from third-party cookies, two primary approaches currently underpin most cookieless identifiers: probabilistic identity and deterministic householding.


Probabilistic identity infers a match by aggregating anonymous signals like IP addresses, device types, browser behavior, and time-of-day patterns. While this approach scales easily across the web, every match is ultimately an educated estimate. As a result, its accuracy degrades as these signals continue to fragment.


Conversely, deterministic householding confirms a match by tying devices directly to a known, registered physical address rather than generating a probability score. This highly accurate method requires a robust, real people-based data foundation to function effectively.


For modern ad buyers, the practical question often becomes a difficult choice: scale versus certainty. Can a provider actually deliver both without forcing consumers behind restrictive login walls? Understanding how to balance these needs starts with recognizing the inherent shortcomings of probability-based models.


### The Problem With Probabilistic Identifiers


The growing cookieless environment, what we call the[New Open Web](https://www.viantinc.com/insights/blog/the-new-open-web-its-time-to-re-imagine-whats-possible-for-marketers-consumers/) (known also as Web 3.0), is in fact, leaving marketers little choice but to move on from third-party cookies and MAIDs.


For example, marketers running media through partners that still rely on third-party cookies now have a much smaller pool of available inventory to utilize third-party cookies against for targeting and measurement. This narrowing of the inventory pool artificially inflates demand, raising costs for advertisers. Additionally, advertisers are simply not able to reach their total addressable audience, as nearly 75% of all ad requests come from cookieless environments.


Driving further inefficiency for advertisers is the inability to accurately manage reach and frequency. Why? As cookies are entirely probabilistic identifiers, and as consumers diversify their portfolio of available devices to consume content on, no single third-party identifier can track them from device to device. Reach and frequency management is an incredibly important lever for marketers to utilize, whether your goal is to maximize audience reach to drive unique awareness or you are aiming to retarget individuals across devices to drive mid to lower-funnel conversions.


## The Next Evolution in Programmatic Technology: People-Based Advertising and the Household ID


For its part, the ad tech industry has sought new, innovative ways to enable granular-level targeting, robust measurement solutions and accurate reach and frequency management in the New Open Web.


We at Viant saw the vulnerability of cookies, and since 2010 we’ve been heavily involved in people-based data solutions. In 2012, we patented the methodology that our proprietary identifier, the[Viant Household ID](https://www.viantinc.com/solutions/planning-and-buying/household-id/) , is built around.


Today, we see the household as the best identifier to solve the challenges being created by the evolving data landscape. The average household size in the U.S. is 2.6, so using a household identifier still enables personalized, targeted advertising opportunities.


And, for marketers, this makes sense: most buying decisions affect the entire household, whether you’re buying a box of cereal, a new family car or deciding which movie to see this weekend.


## How the Household ID Works


Viant focuses on establishing a deterministic household ID, and you can’t cobble that together strictly with first-party data. We pull household IDs from our people-based data infrastructure, which includes identifiers such as name, email, phone and, critically, a physical home address.


Viant’s patented householding methodology uses IP addresses that meet specific criteria such as residential internet service provider, number of devices, plus the times of day these devices are associated with the IP. Then we extrapolate latitude and longitude coordinates from geo-enabled devices to deterministically match them back to our registered physical address.


The result of this admittedly complicated process is a highly accurate, deterministic, household ID.


By anchoring the identifier directly to a physical address rather than a fleeting browser or single device, the household ID remains persistent across every screen under the roof. This durability is critical for omnichannel advertising, unlocking precise cross-device reach, strict frequency management, and closed-loop measurement. For example, it allows advertisers to seamlessly tie a connected TV (CTV) impression to a subsequent mobile conversion within the same household, all without relying on a single third-party cookie.


This householding process is possible because we have physical addresses as part of our registered user database. Other “household” solutions use IP address targeting or another probabilistic methodology to group users into a “household.” Our method applies registered user data to determine real households.


## Viant’s Householding vs Other Deterministic Solutions


Another “deterministic” approach we’ve seen requires that users provide emails for the identifier to be effective. The challenge with this approach is scale.


For example, in order for email to be available across the open web, publishers would need to require that users provide their email and, essentially, log in to their sites. As a consumer, you can imagine the headache this would cause, and not to mention the reluctance by consumers to opt into a “logged-in web”.


From a publisher’s point of view, requiring users to provide their emails creates a barrier to entry that inhibits a publisher’s ability to monetize their sites with advertising, as consumers will be less likely to visit sites that require logins. If you’re a publisher, you want to make it as easy as possible for consumers to visit your site and consume content, so you can serve them ads.


## Measuring Performance Across the Household


A durable household identifier is only as valuable as the performance measurement it enables. Deterministic householding functions as a foundational cross-device attribution layer, bridging the gap between ad exposure and ultimate consumer action. By establishing a persistent link across household screens, it accurately connects a CTV ad view to a subsequent purchase or sign-up on a laptop or mobile device under the same roof.


This persistent framework directly powers the modern measurement stack. It provides the clean, accurate baseline required for rigorous incrementality testing to measure true campaign lift. It also improves Marketing Mix Modeling (MMM) by providing better inputs to evaluate channel contribution, and fuels secure data clean rooms—enabling advertisers and publishers to match ad exposures with conversion outcomes without ever exposing or sharing raw user data.


## Driving Toward a Deterministic Future


While other cookieless solutions are in the market, scale and deterministic matching remain the most significant question mark about these different approaches. Different companies have been intentionally vague around the scale their identifier provides, likely because they are not scaling at critical mass for advertisers to fully utilize. Comparatively, Viant has been transparent about our Viant HHID, which provides advertisers reach across 115M US households.


So as the dominance of third-party cookies ends and you’re looking for better alternatives in a post-cookie world, check out Viant’s householding solution.


*To learn more about Viant’s deterministic householding solution, go[here](https://www.viantinc.com/contact/) .*


AUTHOR: Viant


[More About Viant](https://www.viantinc.com/authors/viant/)


Viant Technology (NASDAQ: DSP) is an exclusively buy-side, AI-powered advertising platform purpose-built for CTV. Viant uniquely combines proprietary content intelligence, household-level identity resolution, and person-level attention signals to connect advertisers with real customers and drive measurable outcomes across the open internet. Through its award-winning AI solutions, Viant is building the future of autonomous advertising, where AI doesn't just assist the campaign, it delivers real results.


Share this with your friends:


## Related Insights


[Programmatic Ad Targeting: Key Types and How They Improve Audience Reach Read More](https://www.viantinc.com/insights/blog/programmatic-ad-targeting-types-effective-methods-for-audience-targeting/)[Deterministic Householding: Precise Targeting Read More](https://www.viantinc.com/insights/blog/deterministic-householding/)[Brand Safety vs Brand Suitability: What Advertisers Need to Know Read More](https://www.viantinc.com/insights/blog/brand-safety-vs-brand-suitability/)[Attention Is Earned: Here’s How Advertisers Can Win It. Read More](https://www.viantinc.com/insights/blog/attention-is-earned-heres-how-advertisers-can-win-it/)[Viant Integrates with Publica to Expand Premium Connected TV Access Read More](https://www.viantinc.com/insights/blog/viant-integrates-with-publica-to-expand-premium-connected-tv-access/)[Havas & Viant Bring the Attention Marketplace to Life for Brands That Demand More Read More](https://www.viantinc.com/insights/blog/havas-viant-bring-the-attention-marketplace-to-life/)[A Direct Path to Bad Signal Is Still a Bad Path Read More](https://www.viantinc.com/insights/blog/a-direct-path-to-bad-signal-is-still-a-bad-path/)


## Frequently Asked Questions


### What is deterministic householding?


Expand


Deterministic householding is a method of grouping devices into a real household by matching them to a known, registered physical address rather than inferring the link from probabilistic signals. Because the identifier is anchored to verified address data instead of a browser cookie, it stays accurate as people move between phones, tablets, and connected TVs in the same home. This gives advertisers a durable, privacy-safe foundation for targeting and measurement across the household.


### How does deterministic householding differ from probabilistic targeting?


Expand


Deterministic householding confirms that devices belong to the same home using registered, address-based data, while probabilistic targeting estimates the connection using signals like IP address and device behavior. The deterministic approach trades some raw scale for far higher accuracy and stability. For advertisers, that means more reliable cross-device reach, frequency management, and attribution.


### What is a household ID?


Expand


A household ID is a persistent identifier that links all the devices within a single home to one privacy-safe profile. Rather than tracking an individual across the web with cookies, it recognizes the household as the unit of targeting and measurement. This reflects how most purchase decisions, from groceries to a family car, are made at the household level.


### What is identity resolution and how does it work?


Expand


Identity resolution is the process of connecting fragmented data signals into a single, unified view of a person or household. Deterministic identity resolution confirms matches using known identifiers like a registered address, while probabilistic resolution infers matches from behavioral and device signals. Working without cookies, it lets advertisers recognize and reach the same household consistently across every device.


### What is cross-device targeting?


Expand


Cross-device targeting is the practice of reaching the same audience across all the screens they use, such as connected TV, mobile, and desktop. A durable household identifier makes this possible by tying those devices back to one home rather than relying on device-specific cookies. This supports accurate reach, frequency capping, and attribution across the full customer journey.


### How is deterministic householding measured?


Expand


Deterministic householding is measured by using the household ID as a cross-device attribution layer that links ad exposures to outcomes within the same home. It pairs with modern measurement methods such as incrementality testing, Marketing Mix Modeling, and secure data clean rooms. Together these let advertisers prove ROI without depending on fragmented, cookie-based signals.


## STAY IN THE LOOP WITH OUR NEWSLETTER


Sign up to get Viant news and announcements delivered straight to your inbox.


Sign up to get Viant news and announcements delivered straight to your inbox.
