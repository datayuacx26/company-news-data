---
schema_version: "1.0.0"
document_id: "35d051f1cf668ea93d55173269050aa37d2dc3fe21ebfeacf40acc54b77a6b42"
company_key: "mntn-inc-class-a-common-stock"
company: "MNTN Inc."
source_id: "mntn-inc-class-a-common-stock-rss-94a2303e058d"
canonical_url: "https://mountain.com/tech/seeing-is-believing-inside-mntns-executive-ads/"
published_at: "2026-07-22T18:33:15+00:00"
first_seen_at: "2026-07-22T18:40:27.870103+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:913bf6565aa4a888a612790b601d20be22d16f4f066dc293ec4ad8e82226caff"
---

# Seeing Is Believing: Inside MNTN’s Executive Ads

# Seeing Is Believing: Inside MNTN’s Executive Ads


Jul. 2026


You are spending real money on Connected TV. Your ad is airing in living rooms across the country, against shows people actually watch on premium networks like Peacock, ESPN, and HBO Max. But unless you’re intentional about it, you may never see your own ad in the wild.


On social media, it’s relatively easy to see your own ads, but CTV has no feed you can scroll. While MNTN’s Impression Map will notify you when your spot airs, there is no immediate way to experience what your most precisely targeted customers are seeing in their living rooms. For the person signing off on your CTV budget, that makes it one of the harder channels to validate in the wild.


MNTN’s Executive Ads close that validation gap. It lets you and your teammates see your own TV commercials while streaming at home, on the same premium networks where your prospecting campaigns run, at no additional cost. It is the simplest way to confirm your ads are delivering the experience you paid for, and to experience firsthand what your hyper-targeted audience sees when your brand shows up on the big screen. This post walks through what it does and how it works, from the clicks that opt users into the impressions that land in your living room.


## What Executive Ads does


The promise is simple: when you opt in a teammate, they become eligible to see your campaign’s ads on MNTN’s premium networks while streaming their favorite shows at home.


Once a user is enrolled and eligible, every one of your MNTN Prospecting campaigns will be set up to serve them your ads automatically. There is no separate campaign to build, and no extra line item to manage. The feature is included at no additional cost, and that matters because Prospecting campaigns exist to find new customers. You and your team are not prospects, so without Executive Ads your own household sits outside the very audience your ad is chasing.


It is also built to leave your numbers alone. MNTN does not count the site visits or conversions from Executive Ad in your performance reports. More on why that matters below.


## The one signal we already have


Here’s the thing: we cannot reach into your home and force your ad onto your screen. What our engineering can do is make sure your campaign knows that your household is one worth serving, so that when you are streaming, your own ad is in the running.


To do that, we need to know which households belong to your team. CTV gives us no click to work with, but every week your team interacts with MNTN in other ways. They log in to the platform. They click a link in a reporting email to download a CSV. They’re invited to opt into Executive Ads directly. These touchpoints happen across the open internet, and each reveals a household’s IP address.


That IP address is the thread we pull. An ordinary click on a MNTN link becomes the signal that tells your campaign which households belong to your team, so when they are streaming, your own ad is in the running.


The click you already make becomes the targeting signal we needed.


## From a click to your couch


There are three ways a household gets enrolled to receive Executive Ads: a team member logs into their MNTN account, a user clicks a link sent from MNTN (such as a download link in a reporting email or a creative preview email), or accepts a direct invitation from their Portfolio Admin. All three paths rely on the same underlying mechanism, and understanding it explains why the opt-in moment matters so much.


Like we mentioned, each of these touches reveals the household’s IP address. Rather than point that link straight at its destination, we route it through a small, standalone redirector service. The service is public-facing and does one job well.


When a touch comes in, the service runs the same short sequence every time:


1. **Parse the request** . It reads the query parameters: the advertiser’s API key, the destination URL (Base64-encoded), and optional values like the referrer and a hashed email.
2. **Validate** . Are the required parameters present? Does the destination decode to a valid URL with a protocol? Is that destination on the allowlist of MNTN domains? Anything that fails returns a 400.
3. **Resolve the advertiser** . The API key maps to the advertiser’s ID, so the visit lands against the right campaigns.
4. **Check the guardrails** . Is this IP blocked for this advertiser? Is the user agent blocked? Either check can stop the request before it goes further.
5. **Emit a pageview** . The service publishes a pageview event to Kafka. This is the exact same event our tracking pixel emits for a real site visit, carrying the household’s IP, the advertiser’s ID, and a flag marking it as an Executive Ads touch. Because there is no browser cookie and no separate identity round trip, the service derives a stable, deterministic ID from the IP itself.
6. **Redirect** . It returns an HTTP 302 with the Location header set to the real destination. The user lands exactly where they meant to go.


That pageview flows into the same audience pipeline as any other signal, so the household joins your Prospecting campaign’s audience automatically. The next time that household is streaming and an eligible auction opens, the bidder can serve your own ad.


The service itself is deliberately lightweight. It is a single standalone deployment running on Kubernetes, shipped through our standard build-and-deploy pipeline and watched with the same monitoring as the rest of our platform. It holds no state and stores no cookies. It exists to do one fast thing on the path of a click and then get out of the way.


## Why a redirector, and not a pixel


Across all three opt-in paths, the redirector is doing the heavy lifting. The obvious alternative for the link-click path is to drop a tracking pixel into the reporting email and skip the redirect entirely. But that does not work. Email clients only allow an image pixel, and an image pixel cannot reliably reveal the real household IP or follow up with anything else. The signal we need is not there.


A second alternative is to bounce the click through a web page that quietly records the visit and then forwards the user on. We rejected that too. It assumes every click comes from a browser, and many of these links are hit by automated reporting engines pulling a CSV, not by a person in a browser at all. A heavy, browser-shaped detour would break those.


The redirector avoids both traps. It is protocol-agnostic, adds one hop instead of two, and captures the signal at exactly the moment the touch happens. The simplest design that does the job is the one we shipped.


## Keeping your reporting honest


Your teammates are not your customers, and counting them as if they were would quietly corrupt the numbers you use to make decisions. A spike in visits from the executive who just clicked through reporting is not demand. It is noise.


So every Executive Ads touch is flagged, and MNTN excludes the site visits and conversions that follow from your performance reports. You get the benefit of seeing your own ad without paying for it in distorted metrics. The point is confidence, not credit, and the reporting reflects that.


## Why you might not see your ad


Executive Ads improve the odds that your ad reaches your household. They do not override how your campaigns are set up, and a few real-world limits still apply:


- **Prospecting only** . Executive Ads only serve through your Prospecting campaigns.
- **Household availability** . Delivery still depends on an eligible auction for that household opening up while they stream.
- **Geotargeting still rules** . If you are opted in but fall outside a campaign’s geotargeting, you will not receive its ads.
- **Blocked networks stay blocked** . If you are streaming on a network your campaign excludes, such as ESPN, you will not see your ad on that network.


One practical note: a teammate should accept their invitation while they are at home, on the same internet connection they use to stream TV. That is the household we want in the audience.


## Executive Ads by the numbers


Year to date:


- **About 1 in 5 advertisers use it.** Of the advertisers who served an ad on MNTN this year, approximately 20% have served Executive Ads.
- **It barely registers in delivery.** Those touches account for just 0.00066% of the impressions served on MNTN year to date and remain a rounding error in cost.


## Putting it all together


Connected TV gave marketers the reach of television and the measurement of digital, but it took away the simple reassurance of seeing your own ad in the wild. Executive Ads gives that back without having to invent a new tracking scheme. It reuses signals your team already generates, turns them into the same pageview event the rest of the platform already understands, and lets your existing campaigns do the rest.


The engineering is intentionally small: one redirector on the path of every touch, one familiar event on the way to the audience, one fast 302 back to the user. The payoff is not small at all. The ad you are paying for finally shows up on the screen you are sitting in front of, and for the people betting budget on Connected TV, seeing is believing.
