---
schema_version: "1.0.0"
document_id: "185bc10b1f1f4fc330203c5e5eee25e1afbf2457ce849e38fe06d5b51b1abf19"
company_key: "aci-worldwide-inc-common-stock"
company: "ACI Worldwide Inc."
source_id: "aci-worldwide-inc-common-stock-rss-caa113f963a6"
canonical_url: "https://www.aciworldwide.com/blog/the-sales-that-never-post-christian-schonhuth-on-recovering-margin-at-checkout"
published_at: "2026-06-25T23:52:41+00:00"
first_seen_at: "2026-07-20T03:32:54.704728+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:a0381b8c99c4210c81b130474c3f9258690f64ef241d67f79ca666acf6568bd5"
---

# The sales that never post: Christian Schönhuth on recovering margin at checkout

*Enterprise merchants lose margin at checkout to declined cards, false declines, and cart abandonment, and the loss is rarely attributed back to payments. ACI Worldwide’s Christian Schönhuth on where authorization rates and conversion erode, why boards measure payments from the wrong end, and the four measures that surface the gap.*


By the time a shopper reaches the payment step, the expensive work is already done. The ad has been bought, the click paid for, the email sent, the product page has done its job. A marketing team has spent to acquire that customer and spent again to hold their attention long enough to fill a basket. All of it is committed cost the moment the customer is ready to pay, whether they reach for a card, a wallet, or a stored credential.


And then, on a meaningful share of those baskets, the sale does not complete. A card declines on an aging credential. An authentication step arrives one screen too late. The payment method the customer actually uses is not on offer. The order is abandoned, and because an order that never happens does not post to a profit and loss statement, the loss is easy to miss and easier still to attribute to something other than payments.


Christian Schönhuth has spent his career trying to make that kind of loss legible. *“Payments is the end of the tail,”* he says. *“By the time a customer reaches checkout, significant marketing, acquisition, and engagement cost has already been spent to get them there. So every point of friction or failure at the payment step is not just a payments loss. It’s a write-off of everything upstream. But it’s the opening act of the conversion that actually matters. In that moment, months of product, pricing, fraud, and experience decisions either compound into a completed sale or collapse into an abandoned basket.”*


He came at the problem from an unusual direction, not from finance but from the screen. His early career in Munich ran through web development and user experience engineering, then through PAY.ON, the payment orchestration platform ACI acquired, with a year at Klarna in between, a company that made its name by removing steps from the act of paying. By the time he took over product for ACI Worldwide’s merchant and eCommerce business, he had spent more than fifteen years on the seconds between a customer deciding to buy and the order being approved. The line under his name on LinkedIn reads less like a slogan than a description of the job. We orchestrate payments.


## The losses that never reach the ledger


Ask a finance team where its payments costs sit and the answer comes back fast: processing fees, interchange, fraud, chargebacks. Those are real, and they are also only the visible part of the picture. Alongside them sits a second category that is harder to see, the value that never converts, and it tends to escape attribution because failed and abandoned orders do not arrive as a line item to be questioned.


---


*“A 2% decline rate isn’t a payments problem,” Schönhuth says. “It’s a full-funnel margin problem that most organizations are dramatically undervaluing, because they’re not doing the attribution math across the entire consumer journey.”*


---


The figure is illustrative. Not every decline is recoverable, and some were never good business; the prize is the legitimate share that was turned away. The reframing is the point: a number the organization files under payments is really a number about the whole funnel.


Some of the leakage is technical and fixable. Authorization rates decay as stored card credentials age and expire, a problem network tokens were built to address. Network tokens, which update automatically when a card’s underlying details change, can lift authorization rates by as much as 2.1%, a figure Visa reports and ACI cites in its guidance on network tokens. The gain varies by market, scheme, merchant, and transaction mix. Some of the leakage is strategic. Fraud rules written to be cautious can decline good customers, and in Schönhuth’s experience the cost of those false declines can, in some portfolios, run ahead of the fraud the rules prevent. The loss is simply spread across functions, and never lands on one desk as a number anyone owns.


## The wrong end of the telescope


Part of the reason the loss stays hidden is that the boardroom is pointed the wrong way. The conversation at the top of most enterprises is still about cost: fees, fraud, chargeback ratios. All legitimate, all measurable, and all, in Schönhuth’s phrase, the wrong end of the telescope. A board that sees payments only as a cost center will keep underinvesting in optimization, because the upside never enters its field of view.


Ask him which number a board should actually watch, and the answer is immediate. “The metric that should be driving that conversation is conversion,” he says, “specifically, how much revenue is being lost at and before the payment step across the entire consumer funnel.” Payment-method availability, authentication, checkout design, and authorization rates all feed it. Approval rate is the part of conversion that is easiest to put a number on, and the arithmetic, expressed by value, is simple enough for a board to check. On one billion dollars of legitimate, unique purchase attempts, recovering one additional percentage point of that attempted value approves a further ten million dollars in payment value. That is an illustration rather than a forecast, and it is approved payment value, not revenue and not profit; the final figure still has to survive refunds, chargebacks, failed captures, fulfillment, and the cost of acquiring the customer in the first place. But it reframes a payments metric as a commercial one, in the board’s own language.


What makes that translation work is the denominator. Measured against every attempt, an approval rate is distorted by retries, since a failed attempt followed by a successful one is two attempts and one approval, which makes the raw rate hard to compare across merchants and easy to misread. First-attempt approval, eventual approval by unique order, retry recovery, and approval by value each describe a different part of the picture. The number worth taking to a board is approval against unique, legitimate purchase intent, because it maps to the demand the business wanted to keep.


## The cost of measuring in pieces


Press him on where the money actually goes, and he does not start with technology. Legacy architecture is expensive. Weak orchestration is expensive. Both are at least addressable once there is urgency and a mandate, even if regulation, integration, and operational risk keep the work slow. The thing that survives every reorganization is structural.


---


**“Silos are the root cause, and they’re the hardest to fix because they’re not a technology problem,”* he says. *“They’re a structural and incentive problem.”**


---


The journey from acquisition to a completed order crosses marketing, product, fraud, finance, and payments operations. Each team owns a slice and is measured on its own targets: clicks, engagement, chargeback ratios, processing cost. None of those targets is wrong. The difficulty is that no single view reconciles them, so the trade-offs between them stay out of sight. A fraud control that looks prudent on a chargeback dashboard can be writing off acquisition cost that surfaces only on a different team’s report. A checkout step that reads as minor in a usability review can be material once it is set against the value moving through it. None of this is one team failing at its job. It is that no shared number exists to show any of them the whole.


*“Silos survive,”* Schönhuth says, *“because no single team feels the full pain of the status quo. Breaking them is a leadership decision as much as a technology one.”*


## A moment, not a process


There is a pattern Schönhuth sees often. Checkout flows are frequently shaped by the merchant’s own constraints, by legacy architecture, by security and regulatory requirements, by the order in which systems were assembled over a decade, rather than by how a customer wants to pay. The result is friction the business can explain and the customer only experiences.


The shape of that friction is well documented. In its cart-abandonment research, the[Baymard Institute](https://baymard.com/lists/cart-abandonment-rate) finds that 18% of US online shoppers have abandoned an order because the checkout was too long or complicated, and 8% because a card was declined. A separate Baymard benchmark, from 2024, puts the[average checkout at 11.3 form fields](https://baymard.com/blog/checkout-flow-average-form-fields) when most sites need only about eight. The damage is rarely one catastrophic failure. It is accumulation.


Not all of that friction should be removed. Some of it protects the customer, satisfies a regulatory obligation, or sustains trust, and in Europe in particular a checkout cannot simply strip out authentication in the name of speed. The aim, as ACI frames it, is to satisfy strong customer authentication while using the available exemptions well. Two distinct things help: sending richer data with each individual payment, and keeping fraud performance strong enough to stay eligible for exemptions in the first place. The issuer still makes the final call on whether an exemption is honored and whether to challenge; the merchant shapes the odds rather than controls them. Optimization is not the absence of friction; it is friction applied with judgment.


What the strongest merchants have changed, in Schönhuth’s account, is the frame. *“They’ve stopped thinking about checkout as a process and started thinking about it as a moment,”* he says, one that should be as close to effortless as the risk allows. Stored credentials. Wallet integration that works. Authentication that challenges when there is a reason to. The technology mostly exists; what is often missing is ownership, because checkout tends to live in engineering as a maintenance task rather than in the business as a priority.


## What optimization frees up


The merchants who treat payments as a source of advantage tend to share one trait. They are not spending their attention on problems they have already solved. With orchestration, conversion, fraud strategy, and clear accountability for the payments P&L in place, the same people are free to work on what a customer will want next rather than on the last thing that broke.


A good deal of that work is the returning customer. The shopper worth designing for first, in Schönhuth’s view, is the one who already chose the merchant once, the customer whose lifetime value is often the highest and whose next purchase is usually the cheapest to win. The experiences that recognize that loyalty are where repeat value is kept or lost: stored credentials, a faster path for a known customer, the sense of being remembered rather than re-processed. As Schönhuth puts it, the payment becomes part of the relationship rather than a transaction stapled to the end of it.


The payoff he keeps coming back to is room. *“The best payments organizations don’t just run efficiently,”* he says. *“They create space for the business to move.”* The advantage is as much about having the freedom to build what does not yet exist as it is about optimizing what does.


## What the board should see


A more useful payments dashboard is short. Four measures carry most of the story:


- First-attempt and eventual approval rates, measured against unique, legitimate purchase attempts rather than raw attempts
- Payment-step conversion, broken out by market and payment method
- The value of false positives and recoverable declines, not only the fraud caught
- A payments-contribution figure built on captured, settled value, net of fraud losses, processing fees, refunds, and chargebacks; fold in acquisition and fulfillment cost and it becomes a fuller, funnel-level contribution number rather than a payments one


Producing these does not necessarily mean replacing a platform. For some merchants it means analytics or integration work to resolve identity across retries and join fraud, refund, and order data; for others the data already exists and simply sits unjoined. What it takes in every case is the decision to look at payments as one picture rather than several, and someone clearly accountable for doing so.


## Hear Christian Schönhuth in London


Christian Schönhuth moderates the merchant session on payments as a profit engine and end-to-end optimization at enterprise scale at[Payments Unleashed EMEA](https://events.aciworldwide.com/paymentsunleashedemea) . The event opens with an evening reception on June 29 at 12th Knot, Sea Containers, and continues with a full day of content on June 30 at the Hilton London Bankside, bringing senior payments leaders from across EMEA together for keynotes, panels, and working sessions on real-time payments, fraud and scam liability, sovereignty in European payments, and the operating-model changes shaping the next decade of commerce.


If your organization is losing value at the checkout that rarely gets attributed back to payments, this is a room worth being in.


**Register for Payments Unleashed EMEA – London 2026**


Registration is complimentary, and the audience is curated for senior leaders across the payments ecosystem.[Register today to secure your place](http://events.aciworldwide.com/paymentsunleashedemea) .
