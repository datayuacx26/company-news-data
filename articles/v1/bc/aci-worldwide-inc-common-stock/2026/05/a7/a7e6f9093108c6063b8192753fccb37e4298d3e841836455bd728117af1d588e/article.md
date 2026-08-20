---
schema_version: "1.0.0"
document_id: "a7e6f9093108c6063b8192753fccb37e4298d3e841836455bd728117af1d588e"
company_key: "aci-worldwide-inc-common-stock"
company: "ACI Worldwide Inc."
source_id: "aci-worldwide-inc-common-stock-rss-ca67c52d46dd"
canonical_url: "https://www.aciworldwide.com/blog/agentic-commerce-trust-fraud-liability-checkout"
published_at: "2026-05-28T23:52:41+00:00"
first_seen_at: "2026-07-25T01:10:51.435202+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:1e38834ece800ddba2f449b8223d4149dcb8f117d0b88360fdc1eec81be79a2c"
---

# Agentic commerce and the trust problem at checkout: ACI’s Adriana Iordan on what breaks when software starts buying

*****ACI’s Adriana Iordan on why agentic commerce breaks the trust layer rather than the rails, how fraud turns into a three-way problem, what real consent looks like once you hand buying to software, and why liability is the unbuilt half.*****


Online payment was built on one quiet assumption: a person was there, making the purchase. Fraud checks, authentication, disputes, and the rules on who is liable were all built around that single idea. Take the person out, put a piece of software in their place, and the payment can still clear cleanly. What changes is everything wrapped around it.


Adriana Iordan has spent more than twenty years building payments and ecommerce products, at PayU, at 2Checkout and Verifone, and now as senior vice president for merchant and payments intelligence at ACI Worldwide. She is also, by temperament, allergic to jargon, with a working rule that the quality of a question matters more than the technical vocabulary wrapped around it. Ahead of her panel at Payments Unleashed EMEA, she answered the big question by first moving it. When software starts buying, she says, the rails are the last thing to worry about.


The first pieces are already in place. Since 2025, the industry has been building toward this version of checkout. Google introduced an open Agent Payments Protocol with more than sixty payments and technology partners, organized around the questions of authorization, authenticity, and accountability, and recording what a user actually approved through cryptographically signed mandates that form an auditable trail. Visa and Mastercard have both moved to support agent payments, Visa with user-set spending controls, Mastercard with tokenized credentials and verified agents. That pace mismatch is what Iordan keeps coming back to: the infrastructure is moving faster than the trust and operating model around it.


## What breaks first is not the rails


If an agent reaches checkout with a real customer credential, Iordan says, the payment clears. What breaks first is the trust layer around it, because almost every control was designed for a moment when a person was present and making the decision. That creates a double problem at once. Fraud models lose the human signals they were trained to read, and legitimate agents start to look like malicious bots, so a business can miss bad activity and block good activity in the same motion. Then the pressure moves into operations, because agents act at machine speed, retry instantly, and throw off exceptions faster than many systems were built to absorb.


The strain lands on specific controls. Approval systems still tuned to human-cardholder patterns can read a legitimate agent payment as unusual and decline it. Step-up security checks assume a person is on hand to complete them. And financial-crime controls, built to identify a person or a company as the customer, struggle to name the true actor when software is transacting on someone’s behalf.


What the industry needs, she argues, is easy to state and hard to build: proof of who initiated a transaction, what authority they had, and who stands behind it. “The real gap is not payment execution,” she says. “It is trust attribution.”


## Fraud becomes a three-way problem


Today’s fraud models are good at reading people, the device, the rhythm of a session, the small signs that a real customer is present. A software agent offers none of that. It does not type, pause, or hesitate like a person. So the model has to answer a different question: who is this agent, what is it allowed to do, and is this action inside those limits.


What an agent does leave behind looks nothing like a human session. Iordan describes the tells: almost no clicks, scrolling, or dwell time, then a sudden burst of complex catalog queries; carts assembled in seconds with exact product picks; jumps across unrelated categories to satisfy one broad instruction; and spikes in calls for pricing and availability with no page views behind them. Read correctly, she says, those are the marks of an agent to be understood, not a bot to be blocked.


Iordan frames it as a shift in the basic category problem. “It used to be human or bot, where the bot simply gets blocked,” she says. Now there is a third category, a legitimate agent acting with permission, and the system has to tell it apart from both a malicious bot and a person. Once you can no longer lean on human behavior, she argues, the strongest signal is the one that was never about the human at all: what can be observed about an agent across the wider ecosystem, not just inside one checkout.


ACI’s own fraud data gives her a concrete example of cross-ecosystem signal at work. In its World Cup fraud analysis, drawn from 24.5 million transactions across 61 live-event merchants, the warning signs of a fraud surge showed up across the network weeks before they were visible to any single merchant or bank, and alternative payment methods showed a far lower attempted-fraud rate than cards in that dataset, 0.57 percent against 3.97 percent. The same wide view that catches event fraud early, she says, is what a business will need when the buyer is an agent.


Returns and refunds are where the problem turns practical, and the surface is already vast. The National Retail Federation and Happy Returns put US returns at $849.9 billion in 2025, about 15.8 percent of sales, with roughly 9 percent of them fraudulent and 85 percent of surveyed merchants already using AI or machine learning to identify and combat fraud. Drop in an agent that can trigger refunds on its own, and serial returns become an industrial process. The job, in her view, has changed: tell three actors apart, a human, an authorized agent, and a malicious bot, which makes fraud a classification problem rather than a gate.


## Consent becomes delegated authority


When a customer hands buying power to software, consent stops being a single click at checkout and becomes a standing permission with boundaries. Good consent, Iordan says, has four parts: who the agent is, what it can buy, where it can act, and how the customer can revoke it. In plain terms, this is not payment authorization any more. It is delegated authority, and the distinction matters, because people may accept delegation but they do not want to hand over a blank check.


That is why she keeps separating two things the industry tends to blur. Recording that permission was given is getting more mature. Deciding who is accountable when an agent goes beyond it is not. Many systems can already show that consent existed. Far fewer can say who is responsible when the agent oversteps.


## Liability is the unbuilt half


Accountability, for Iordan, is where the market is least ready. The industry has made real progress on consent, with signed, scoped, revocable mandates beginning to appear, but the other half is still unbuilt. When an agent acts beyond its mandate, there is no clean, shared answer on who carries the loss, and the question lands on a disputes system that is already under strain. That, she notes, is why early European agentic payments have been deliberately bounded and human approved, with the issuing bank in the authorizing seat and the agent blocked from moving money on its own.


Her advice is to stop waiting for the rule and start capturing the evidence. Whoever can show who authorized a given purchase, what limits applied, and whether the agent stayed inside them, across the whole chain of issuer, acquirer, merchant, and network, is the party that can actually settle a dispute and, in time, stand behind autonomy. The mandate, the identity, and the signal history are a product in their own right, she says, not an afterthought, because the answer rarely sits inside any one party’s records. Her summary of the right posture is blunt. “Build as if it is imminent,” she says. “Underwrite as if it is unproven.”


## What merchants should actually do


For all the momentum, Iordan is precise about the gap between interest and trust. In ACI’s own research, a YouGov survey of 2,080 UK adults in June 2026, just 17 percent of people said they would trust an agent with their payment data, even as many are happy to let one help them shop.


---


*“Interest has crossed over,” she says. “Trust has not.”*


---


The gap runs the other way for merchants. In a Payments Association report Iordan contributed to, 58 percent of UK merchants believed an AI agent had already transacted on their sites, and 72 percent said they were actively preparing, even as 39 percent named uncertainty over standards, rather than budget or business case, as their biggest barrier. Preparedness, in other words, is running ahead of confidence.


So her counsel to merchants is to act now, but to be specific about where. Do not lead with agent checkout. She points to one large US retailer that, by her account, found checkout inside a chat converting roughly three times worse than sending the shopper to its own site, after which the platform pulled back to discovery, and she puts agent traffic at still under one percent of traffic today. The work that pays off is less glamorous: clean, structured product data so an agent represents you accurately, and fraud and returns controls that hold when the buyer is software. Treat returns abuse as a fraud surface rather than a service cost, get the growth team and the risk team into the same room before anything ships, and build those controls across every channel, because the behavior will not stay inside the agent lane. For the payment providers serving those merchants, she frames the same work as a product to package. That means agent-ready acceptance, identity and mandate checks, a dispute-evidence trail, and fraud scoring that holds up across networks rather than collapsing when the human signals disappear.


Near term, she says, merchant-hosted models have the edge, because people trust a retailer they know more than a third-party agent. The longer-term risk sits with the open agent surfaces, which can step between a brand and its customer, and the most exposed brands are the ones whose catalogue an agent cannot read, because they simply will not be surfaced. So show up on those surfaces deliberately, she says, and hold on to the relationship you own rather than surrender it to win a channel.


## Hear Adriana Iordan in London


Adriana Iordan joins Merchant Breakout 3, Preparing for the agentic buyer: consent, fraud, and trust at checkout, at[Payments Unleashed EMEA](http://events.aciworldwide.com/paymentsunleashedemea) . The event opens with an evening reception on 29 June at 12th Knot, Sea Containers, and continues with a full day of content on 30 June at the Hilton London Bankside, bringing senior payments leaders from across EMEA together for sessions on real-time payments, fraud and scam liability, sovereignty in European payments, and the shift to agentic commerce.


Agentic commerce does not replace the rails. It adds a new layer to how a transaction begins, and that layer runs on trust as much as tokens. The payment, as Iordan puts it, will clear for everyone. The trust will not.


**Register for Payments Unleashed EMEA – London 2026**


Registration is complimentary for a curated audience of senior payments leaders.
[Register today to secure your place](http://events.aciworldwide.com/paymentsunleashedemea) .
