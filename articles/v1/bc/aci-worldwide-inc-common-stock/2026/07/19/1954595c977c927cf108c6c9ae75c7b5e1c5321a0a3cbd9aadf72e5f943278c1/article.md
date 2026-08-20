---
schema_version: "1.0.0"
document_id: "1954595c977c927cf108c6c9ae75c7b5e1c5321a0a3cbd9aadf72e5f943278c1"
company_key: "aci-worldwide-inc-common-stock"
company: "ACI Worldwide Inc."
source_id: "aci-worldwide-inc-common-stock-rss-ca67c52d46dd"
canonical_url: "https://www.aciworldwide.com/blog/standing-still-is-the-real-risk-in-cards"
published_at: "2026-07-08T19:52:43+00:00"
first_seen_at: "2026-08-14T15:48:53.874446+00:00"
fetched_at: "2026-08-14T15:48:55.411012+00:00"
content_hash: "sha256:71824bd3f1046e3a9abd63cf385cf00d67b95d2f7f27ae1a53c49e3f24ec4c1b"
---

# Standing still is the real risk in cards. Q&A with cards & payments expert, Dean Wallace

Cards have spent decades absorbing every shock the payments world has thrown at them. So why has modernization transitioned to a board-level conversation rather than an IT one? We asked[Dean Wallace](https://www.linkedin.com/in/deanwallace/) , Director, Consumer Payments Modernization at ACI Worldwide, where the real competitive pressure is coming from, where legacy platforms hide their costs, and why the riskiest part of migration is rarely the technology.


## Cards and real-time payments are usually framed as competitors. Where do you think the more overlooked competitive pressure on cards is actually coming from?


It’s a decent question, but before you answer whether cards and real-time payments are competitors, you have to look at both the issuing space and the acquiring space, or to put it another way, the consumer and the merchant; because that’s what cards actually covers.


Cards came out decades ago to make transactions between merchants and consumers easier; a way to move away from cash and cheques. In the 1990s, adoption went global fast: credit cards in big markets like the US, debit mainly elsewhere. Amex, Visa, and Mastercard did a great job of taking that worldwide, alongside the likes of China UnionPay, JCB in Japan, RuPay in India, Diners Club, and a whole set of domestic schemes, Switch in the UK, and Mister Cash in Belgium, most of which have since consolidated.


Real-time payments came about for a different reason: to speed up creaking ACH. You had clearing cycles of three days or so, and that tied up a lot of liquidity. Once you can move money between bank accounts instantly, and pay your bills instantly, the next obvious use case is person-to-person (P2P); Zelle in the US, for example. And once you’ve done P2P, the next step is eCommerce. It’s online, it’s digital, and there’s no physical card involved.


That’s really where the “Are they competitors?” question comes from. They serve different purposes, though in some markets, real-time payments are stepping up further.


But the more interesting and overlooked pressure is economic. On the issuing side, whether it’s a retail bank, a corporate card provider, a credit-card monoline, or a prepaid provider, you get paid on every transaction for managing the risk. These economics have long been baked in and provide revenue for the issuer, as well as funding consumer protection: chargebacks, dispute management, and refunds when there’s fraud. For a card issuer, interchange is usually a big revenue line. And in a lot of markets that’s under real pressure. Europe capped interchange, so it’s been flattened substantially. The US hasn’t, and you can see the difference: A US credit card gets you airport lounges; in Europe, you might be lucky to put a few points towards a five-pound grocery shop.


Acquirers feel a different kind of pressure. It’s not really competitive, it’s, “How do I support all of these capabilities, and how do I keep my cost margin low enough to provide them and keep them compliant?” Which is the same challenge the issuer has: How do I lower my costs and increase my margin because the revenue streams are under increasing pressure?


---


*The sharper question isn’t “cards vs. real-time payments.” It’s margin. Capped interchange and ever-growing capability demands are squeezing issuers and acquirers alike, and this economic pressure, not any single rival rail, is what’s really reshaping cards.*


---


**Takeaways**


The real pressure on cards isn’t just real-time payments, it’s economic and structural:


- Issuers are seeing revenue pressure from capped interchange, especially in Europe
- Acquirers are being forced to support a growing range of payment types
- Both sides are under pressure to lower costs while maintaining margins


## What about fraud and consumer protection on real-time payments?


There’s a fundamental difference between cards and real-time payments that gives an almost native fraud-protection angle. Think about a card: There’s the 16-digit PAN on the front and the CVV on the back. If someone got the PAN, the CVV, and the expiration date, they could transact with your card online because the merchant or their processor takes that data, goes through their acquirer and the card rails, and pulls the funds from your account. This is where the risk comes in, and as an industry, we’ve had decades to manage it: 3D Secure, two-factor authentication, and the one-time passcode you type into your app to improve security and add consumer protection


Real-time payments are newer, and the model is different. To pay someone, I can’t hand over my bank account and have them pull from it; the systems aren’t built that way. They’re built as a push. In the UK, if I give you my account number and sort code, the worst thing you can do is send me money. You can’t take it from me. That push model is, at a structural level, more secure.


But it does create its own challenges; the fraud people would call them “new vectors.” Now, the merchant has to send me a request to pay, I receive it as a notification, I pick the account, and authorize it. There’s scope for someone to send a fake request to pay, but typically, you’re in front of the merchant, digitally or physically, so you’re expecting it. If one such notification turns up out of nowhere, you probably know it isn’t legitimate.


Real-time payments aren’t automatically safer, but the push model does remove some of these vulnerabilities by design. Where cards really pull ahead is maturity. Cards are backed by scheme rules that are legal in essence: They dictate who carries which liability, what happens when something goes wrong, the chargeback rights, and all the mechanisms that make disputes and refunds work. And cards have had decades to harden this and get this right.


Real-time payments are still relatively nascent globally when compared to cards. India is a great example of a market that’s done a lot to replicate what cards do well. Most markets don’t have that protection framework in place yet. Cards has it in spades.


The push-vs-pull distinction gives real-time payments a native security advantage, but cards win on maturity: decades of hardened fraud tooling, plus scheme rules that clearly assign liability. This governance gap is what real-time payments still has to close.


---


*Real-time payments has a native security edge from the push model, but cards haven’t stood still: decades of fraud tooling that keeps evolving, plus scheme rules that clearly assign liability. Closing this governance gap is the real challenge of real-time payments.*


---


**Takeaways**


Built-in protection—who carries the risk?


- Push model of real-time payments = safer by design
- New scams still possible (fake requests)
- Cards are stronger on protections, rules, experience, and anti-fraud consortiums


## Network tokens, push provisioning, and credential-on-file have changed what an issuer platform has to do. What’s the capability gap that catches banks by surprise?


I don’t think banks get caught by surprise very often. The real gap is speed. Take provisioning mobile wallets, for example. When Apple Pay came out more than ten years ago, issuers had to set up tokenization services, and it was a competitive, market-by-market move. If you didn’t have a card on Apple Pay in the market where it launched, that was a negative impact straight away: concerns around share price going down or reputational damaged were real at the time, so you had to embrace it.


Some capabilities still haven’t reached mass market; wearables, for instance. I’ve got a Garmin watch, and Garmin Pay is proprietary, so only a select number of banks support it. This has actually pushed me towards specific banks: I started with Starling, moved to Monzo because they’re also on Garmin, and ended up adding Revolut for a separate pot of ‘beer money.’ Three new-age banks. I didn’t mention a single traditional one. That tells you something.


The bigger banks with tens of millions of accounts in mature Western markets have a different problem, and it’s mostly regulatory and architectural. They maintain legacy systems where the cost of change is massive, and the pace of change is very slow compared to “challenger” banks. This speed gap has been one of the biggest concerns on the issuing side.


Regulation adds to it. Brazil mandated that any issuer with more than 250,000 accounts— a small number in a market of 200 million-plus people—had to open API access so PIX could connect directly. This is similar to what PSD2 did in Europe and Open Banking did in the UK but compressed into a much shorter window and created a payment rail at the same time.


Amazon recently launched Pay by Bank, offering a streamlined purchasing experience through Monzo, although it may not be as simple as a tap of your card. To use it, you check out, click to pay with Monzo, it takes you to your bank app, you authorize it, and done; it takes the funds straight from the account. Other banks haven’t accomplished this as smoothly because the risk aversion is much stronger at a larger bank.


As a trend, we see this: In markets without clear governance of what the experience should be, consumers are left unsure which method to stick with after the initial use or if they want to share their experience with their friends, while other banks implement completely differently, perhaps curating an even worse experience. Smaller providers are delivering stronger user experiences, creating a new dynamic.


---


*The gap isn’t a missing feature banks don’t see coming, it’s velocity. Legacy platforms can’t match the pace at which challengers ship wallet support or regulatory connectivity, and this speed gap is now showing up in customer experience.*


---


**Takeaways**


The real gap comes down to:


- Legacy architecture slowing down change
- Regulatory complexity increasing the cost of innovation
- Inconsistent customer experiences across banks


## When you look at what a legacy card platform actually costs a bank, where is the cost hiding? It usually isn’t the line item in the IT budget.


Compliance presents a significant challenge. Scheme compliance for cards is hard to measure; the schemes issue mandatory updates twice a year, plus optional updates that a bank might choose to take. This can present hidden costs: Most banks assume a level of compliance work and build a cost base around it.


There are also IT costs associated with ensuring systems meet regulatory requirements. This is an area where ACI supports its clients by providing solutions designed to maintain compliance. Clients receive mandate updates and can efficiently implement them within their environments to meet requirements. Given the complexity of this space, ACI has consistently helped reduce the associated burden for its customers over many years.


Another significant factor that is not hidden is infrastructure cost. Many heritage and legacy platforms depend on expensive infrastructure, including the box it runs on, network cabling, storage area networks, and security layers. It adds up rapidly.


And there’s a capacity trap. If activity dips, your revenue dips with it, but your cost base doesn’t because you’ve already paid for the infrastructure. If activity spikes, you might not have provisioned for it. In reality, that second scenario rarely materializes because organizations typically scale systems with substantial redundant capability. This underutilized infrastructure incurs significant costs while being used infrequently, often just once or twice a year.


Cloud technology addresses this challenge by enabling dynamic scalability, so the cost base tracks your revenue-generating activity rather than sitting idle. It’s a big-ticket item for any bank or acquirer.


---


*The real cost of legacy is the hard-to-measure compliance burden and the redundant infrastructure banks over-provision “just in case.” Cloud’s appeal is matching cost to actual activity rather than peak-day fear.*


---


**Takeaways**


- The second major cost besides compliance sits in infrastructure
- With legacy systems, costs remain fixed; unused capacity sits idle most of the year
- Cloud becomes strategic; it allows costs to scale dynamically with demand


## What specifically changed in the last 18-24 months that’s pushed card modernization onto the CEO’s agenda as well as the CIO’s?


Much of it comes back to those cost synergies, as well as valuations. There are many mergers and acquisitions in the acquiring space, so it’s about the value of your capabilities and your ability to compete.


Issuers are asking how they keep up with the “challenger” banks; acquirers are asking whether their assets are valuable enough to make them an attractive target or attractive enough as a buyer to bring someone else onto their platform for economies of scale. Both sides have a similar need in that regard, and the technology today enables that scale on a level we’ve never seen before.


The term “challenger” is becoming less applicable, as these institutions are increasingly leading in certain areas of banking. While traditional banks tend to offer broader portfolios, including loans and mortgages, the “challengers” are hitting them where it hurts: the customer intimacy on the current account.


This is also what’s driving the investment in modernization, and the numbers are big. Some banks will spend billions of dollars over the next two years on modernization, just in cards alone. Some of these systems are decades old. “If it isn’t broken, don’t fix it” has held for a long time, but not anymore. The problem now is that the cost of doing nothing is starting to hit revenue.


---


*Modernization became a CEO issue because it stopped being about uptime and started being about valuation, survival, and the ability to grow and compete.*


---


**Takeaways**


Three things have converged:


- Economics and valuation pressure: In both issuing and acquiring, platform capability now directly impacts company valuation, especially in a market with active consolidation, mergers, and acquisitions.
- Competitive reality: “Challenger” banks are no longer challengers; they’re leading in key areas such as customer experience and card issuance scale.
- Cost of inaction: Legacy systems are no longer just inefficient, they’re constraining growth, speed, and relevance.


## What’s the single riskiest moment in migrating a live card platform, and how do teams that get it right manage it?


The biggest issue with any conversion from one system to another is keeping the lights on. You can’t afford for a single customer to be impacted simply because you’re changing the plumbing, so you have to keep things transacting. In the consumer world, this sometimes means issuing a new card because you’ve got a new PAN. A lot of this is automated now and easier than it used to be. Migrating the data itself sounds daunting, but in the grand scheme, it’s one of the easier things.


What really drives the cost is your risk appetite. If you’re willing to go in one move overnight, the cost is lower, but the risk is much higher because your teams aren’t used to the new solution yet. If operability, observability, or scalability do not meet expectations after a full-scale migration, the impact can be significant. As a result, banks typically take a measured approach to migration.


Recently, I’ve seen two primary approaches emerge: one involves migrating system capabilities piece by piece over time, while the other focuses on establishing a new site, preparing it fully, and then transferring actual account transactions across. These contrasting strategies—incremental migration versus greenfield deployment—highlight the range of approaches organizations are taking.


But the costly factor is how risk-averse you are. These are big programs, and the longer one runs, the more expensive it becomes. That’s the biggest cost.


After that, it’s the reissue of cards. If you’re an acquirer reconnecting a merchant point of sale, it can be extremely costly. You might have to roll updates out across an entire estate, some of which might not even be relevant to the new world and may need replacing. Ultimately, the biggest driver of cost is risk appetite itself: Faster migrations reduce cost but increase operational risk. Slower, phased approaches reduce risk but significantly increase program cost. The organizations that get it right strike a clear balance between the two.


**Takeaways**


Key risk factors include:


- Reissuing cards (new PANs, physical replacement costs)
- Migration approach (big bang vs. phased)
- Operational readiness of teams using the new platform


## What does the certification and risk workload actually look like during a modernization program? Where do banks most often underestimate it?


Certification is often significantly underestimated. Many banks think: “I already transact with this card scheme today, so surely I can do it again easily tomorrow,” and you can, but you still have to go through the certification program with the scheme. They have to provide a project manager and be ready to certify the new platform. That’s a project within the program, and it’s underestimated quite a lot. Knowing the steps you need to go through to certify with a scheme matters, because there’s a lot to do, and it’s not just the technology, it’s the operational side too.


Even organizations that are changing a system but staying with the same scheme sometimes think, “It’s just a system that’s changing.” But it’s the recertification that gets you as it’s a significant event. It’s one of the reasons we built our ACI Connetic solution the way we did: We took the same BASE24-eps capability that already runs in many markets globally and modernized it into cloud-native compliant containers, as part of our unified[cards and payments hub, ACI Connetic](https://www.aciworldwide.com/solutions/payments-hub) . The code running for cards in ACI Connetic is the capability we have deployed globally running at enterprise scale today. When we work with a bank to recertify, we’ve effectively already done it and kept it compliant. This was a key factor in selecting this approach, enabling us to accelerate the certification piece.


---


*Recertification is a full project, not just a formality, and it spans technology and operations. Banks most often underestimate it precisely when they’re staying with the same scheme and assume nothing has really changed.*


---


**Takeaways**


Certification is not just about technology:


- Operational processes must be validated
- Scheme rules must be reimplemented
- Responsibility and liability models must be retested


## Some banks defer modernization deliberately. When is that the right call, and when is it a slow-motion mistake?


Deferment has been going on for quite some time. Many industries outside financial services have been implementing modern capabilities for 10 years or more. We’re typically a bit late in financial services. There are more challenges here because we’re moving funds, and people don’t want to lose money. So, I understand the caution.


But this delay is letting other parties steal your breakfast. The challenger banks didn’t start with legacy tech; they started with modern tech so they are able to innovate faster, they’re more flexible, can flex the cost base, and add new capabilities, fast-fail. These are words the big players would love to use, but they’re not there yet because they’ve got creaking infrastructures. They are on it for sure, but it’s not easy, and some key decisions at the heart of transformation, such as “How do I get my cards auth platform to elastically scale, self-heal, and automatically patch itself,” are still causing delays industry-wide.


Is there ever a good time to defer? The truth is, not really. There may be short-term reasons to defer, but the market is moving. Industry data suggests that a significant majority of banks are actively making modernization decisions over the next couple of years, meaning doing nothing is no longer a neutral position. Those who wait too long will find themselves trying to catch up in a race already underway.


And it’s worth adding that you don’t have to do it all at once. You can absolutely modernize selectively; it’s very much how we’ve designed our solution. Some customers want to move as fast as possible, e.g. in two years. Others say, “We think we’ve got five to seven years.” With ACI’s solution, you can go as fast or as slow as you want, incurring business benefits along the way. You might not get maximum flexibility on day one, but you get the knowledge and the learning, building competence, with the ability to move to the next step later or speed up as confidence builds.


---


*Modernization doesn’t have to be all –at once. Many banks are now taking staged approaches, building capability incrementally while reducing risk.*


---


**Takeaways**


There are always tradeoffs, but increasingly, deferring is becoming risky. Banks that delay are effectively giving ground to:


- More agile competitors
- Lower-cost operating models
- Faster innovation cycles


Meanwhile, newer entrants, built on modern technology from day one, are able to:


- Launch features faster
- Scale more efficiently
- Adapt to market changes with far less friction


## What’s the most common mistake banks make in the first 90 days of a modernization program?


A common mistake we’ve often seen is not having an end goal in sight with strategic stepping stones to get there. There’s a tendency to take a “just do it” mentality; “We’ll make it happen.” But the decisions you make at the start are the ones that affect you most at the end.


At ACI, we believe a well-designed **conversion strategy** is essential. High-level, but clear enough to give you the mission-critical stepping stones to get where you’re going and prove value or grow knowledge incrementally. It must be high-level enough that the direction stays visible so teams planning phase by phase can always bring it back to the North Star, but with enough flex to pivot when you need to as well as clear criteria for when to pivot and when to keep moving forward. Some organizations get straight to the work and miss this critical step. And that’s where the costly mistakes often arise.


---


*The first 90-days failure is starting work without a clear end state. A good conversion strategy stays high-level enough to keep direction clear, yet flexible enough to pivot on defined criteria.*


---


**Takeaways**


Starting without a clear end state is the biggest mistake a bank can make. Too often, organizations jump into execution without defining:


- A clear target architecture
- Strategic milestones
- Decision criteria for course correction


The most effective programs anchor everything to a clear “North Star” while maintaining enough flexibility to adapt along the way.


### Explore card platform modernization in depth


Index finger of young man touching smartphone


Read the ACI paper: ***Card modernization as a driver of profit-and growth-strategy*** to explore how modern card platforms turn scale and change into economic leverage.


[Download Now](https://www.aciworldwide.com/card-modernization-profit-growth-strategy)
