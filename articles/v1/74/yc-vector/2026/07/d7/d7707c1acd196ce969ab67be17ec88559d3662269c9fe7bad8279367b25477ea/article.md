---
schema_version: "1.0.0"
document_id: "d7707c1acd196ce969ab67be17ec88559d3662269c9fe7bad8279367b25477ea"
company_key: "yc-vector"
company: "Vector"
source_id: "yc-vector-news-import-aaa3c7a46c08"
canonical_url: "https://www.vector.co/blog/what-is-intent-data"
published_at: "2026-07-23T00:00:00+00:00"
first_seen_at: "2026-07-24T06:31:07.213979+00:00"
fetched_at: "2026-07-28T21:40:00.658555+00:00"
content_hash: "sha256:6b044142394682c84bf7e262ea91c4a10284f18bc684dcb9d152623da1cf6b7c"
---

# Intent data, honestly: what it tells you and what it doesn't

Intent data is supposed to tell you who's ready to buy. More often it tells you something much weaker: that *someone* at a company you care about looked at a topic you sell into, sometime in the last week or two. That's useful for prioritizing. It doesn't tell you a real buyer is in-market, and it definitely doesn't tell you which person to reach.


Most teams buy the third-party, account-level version: a vendor infers that "Acme is surging on your category" from research happening on other people's websites. The signal that actually holds up is first-party and contact-level, what a named person did on *your* site and ads. Same words, very different signal. This piece walks through both and gets honest about where each one actually helps.


## What is intent data, exactly?


Intent data is behavioral data used to infer that a person or company is researching a product or category, and might be moving toward a purchase. The key word is *infer* . You're not reading anyone's mind. You're watching digital behavior, page views, content downloads, search activity, review-site visits, ad clicks, and estimating interest from it. Two distinctions decide how much that estimate is worth: where the data comes from, and how precisely it identifies the person.


### First-party vs third-party intent data


First-party intent data is behavior on your own properties: your site, your pricing page, your ads, your webinars. You control it, you can see it in near real time, and it points at people who already found you. Third-party (often sold as "purchase intent data") is aggregated from other websites, publisher networks, and review sites, then modeled into a topic score for an account. It gives you reach into companies that never touched your site, which is its whole appeal. As Foundry puts it in its[breakdown of intent data types](https://foundryco.com/blog/blog-first-party-second-party-third-party-intent-data-whats-the-difference/) , third-party intent "reveals account interests and topics across the internet but it does not necessarily indicate buying intent." Hold onto that sentence.


### Account-level vs contact-level


The second distinction is the one the category keeps skipping. Most B2B intent data is account-level: it resolves to a logo, not a human. "A company in your ICP is researching data security" is a very different asset than "the VP of Security at that company read your comparison page twice this week." Account-level intent tells you something is happening at the company. A named contact is someone your rep can call this afternoon. Before you trust any intent feed, know which two boxes it sits in: first-party or third-party, account-level or contact-level. That combination tells you more about the feed's value than any vendor pitch will.


## Where third-party intent data misleads you


Third-party, account-level intent is the noisiest, latest signal in your stack, and it's usually the one sold hardest. This is where it burns teams.


- **It's late.** By the time a topic surge shows up in a modeled feed, the buyer may already have a shortlist. You're reacting to research that happened days or weeks ago, on someone else's timeline.
- **It doesn't tell you who.** "Acme is hot" is not a person. Acme might have two thousand employees. Your reps can't call a logo, and handing them a hot account with no name just moves the guesswork downstream.
- **Your competitors bought the same list.** Third-party intent is[sold to anyone who pays for it](https://solutions.trustradius.com/vendor/intent-data/activating-3-different-types-of-intent-data/) , and only a slice of those signals map to accounts genuinely moving toward a purchase. If everyone selling into that account sees the same surge, it's not an edge.
- **Interest is not intent.** Someone constantly reading about a category can have deep curiosity and zero budget, like a person who browses luxury-car sites every night but drives a 10-year-old hatchback. Topic consumption is interest. Treating it as a purchase decision erodes sales' trust in the whole program fast.


None of this means third-party intent is worthless. It means it's a prioritization layer. Use account-level intent to decide where to point attention, then corroborate with something closer to the buyer before you build a play around it.


## The signal that actually holds up


The durable signal is first-party and contact-level: a named person from a company you care about, doing something meaningful on your own properties. A director at a target account who visited your pricing page, clicked an ad, and came back three days later is worth more than a whole account "surging" on a topic somewhere off in a data broker's model. You saw the behavior yourself. It's fresh, and you can tie it to a human.


This is the layer Vector is built on. We de-anonymize the real contacts landing on your site and ads, filter them to your ICP, and let you act on the actual person. Be honest about the boundary, though. First-party behavior only covers people who actually reach you (roughly a third of US B2B site traffic resolves to a named contact), so it won't show you accounts that never visited. That's exactly why the two work together: third-party for reach and prioritization, first-party for the signal you build real plays on. For the mechanics of the people-level version, see[contact-level targeting](https://www.vector.co/blog/contact-level-targeting) . Weight your scoring and your spend toward behavior you can see on your own properties and attach to a name.


## How to actually use intent data


A signal sitting in a dashboard changes nothing. The teams that get value from intent data score it, prioritize with it, and get to the right person fast. The common failure is raw signals sprayed into a dozen Slack channels with no scoring and no play, so nobody works them and the program stalls.


### Score and prioritize


Blend fit and behavior into one score. Fit is how closely the company and the person match your ICP. Behavior is what they actually did, and how recently. A pricing-page visit from an in-ICP director outranks a topic surge from an account nobody can name. Your ICP is a precision dial. Tighten it and volume drops but false positives fall too. Loosen it and you pay to reach non-buyers. Set that expectation before you cut the audience down. For a longer walkthrough of turning scores into moves, read[what to do with intent data](https://www.vector.co/blog/what-to-do-with-intent-data) .


### Reach the right person with the right message, fast


Once a signal fires, three things have to line up. Start with the person, the actual buyer or committee member at the account, which isn't always whoever happened to click the most. Then match the message to where that person sits in the buying group, because one ad aimed at everyone from the CIO to a project manager lands with nobody. Then get it to them on a channel that hits, usually contact-level paid on LinkedIn, Google, or Meta, sometimes a scoped alert that routes the named contact to the owning rep for a same-day touch. Speed matters here. Warm behavior goes cold quickly. For the activation side specifically, see[how to act on buyer interest](https://www.vector.co/blog/how-to-use-intent-signals-act-on-buyer-interest) , and if you're building the business case,[the ROI of ABM intent data](https://www.vector.co/blog/abm-intent-data-whats-the-roi) lays out the math.


## Treat intent data as an input


Intent data earns its place when you treat it honestly. Third-party, account-level intent points you at where to look. It won't tell you a deal is real or name a buyer, and your competitors are reading the same feeds. First-party, contact-level behavior is the signal you can actually build on, because you watched it happen and you can attach it to a person. Score both together, move fast to the right human, and weight your budget toward the signal you can verify. If you want to see what that looks like when identity resolution, scoring, and activation run in one system,[that's what Vector is built for](https://www.vector.co/product/reveal) .
