---
schema_version: "1.0.0"
document_id: "74198c193759f93c1469f0a632c350be46626f1912a8111b1241bf0f0ec8d515"
company_key: "yc-corgi-labs"
company: "Corgi Labs"
source_id: "yc-corgi-labs-atom-a99da208b4cc"
canonical_url: "https://www.corgilabs.ai/insights/agent-payments-intelligence-stripe-visibility"
published_at: "2026-04-16T21:45:00+00:00"
first_seen_at: "2026-07-20T23:20:24.663691+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:6a465bdb8b99d003121eb5ea70aef4f3e47705c3397ac39503bce6439de6684c"
---

# You Can't See Your Agent Channel Yet. Here's How to Fix That.

Every agent-initiated purchase on Stripe looks identical to a human-initiated one after settlement. That's becoming an expensive blind spot.


Stripe has shipped the primitives you need to sell through AI agents: Shared Payment Tokens (SPTs), the Agentic Commerce Protocol, agentic network tokens from Visa and Mastercard, BNPL over SPT, and the Agentic Commerce Suite. The first merchants to go live (Coach, URBN, Ashley Furniture, and Kate Spade) are processing agent traffic from confirmed integrations like ChatGPT Instant Checkout and Microsoft Copilot Checkout, with additional partners like Perplexity in earlier stages.


Ask a payments lead at one of those merchants how their agent channel is performing. They can't tell you. The data isn't there.


## The Persistence Problem


Here's the technical reality most teams haven't caught up to yet.


When an agent initiates a purchase, Stripe creates a Shared Payment Token: an` spt_` -prefixed object scoped to a single transaction, time-limited, and carrying Radar risk signals from the agent side. The merchant confirms a PaymentIntent with the SPT.


But at confirmation time, Stripe clones the underlying payment method and sets the PI's` payment_method` field to the clone. The SPT is consumed. The resulting charge object that lands in your Sigma tables, your BigQuery sync, or your data warehouse looks structurally identical to a normal card-on-file transaction.


As of April 2026, there is no` is_agentic` field on the charge. No` channel = "agent"` enum on the PaymentIntent. No agent platform attribution. **The distinction between a checkout inside ChatGPT and one on your own website disappears** in most standard reporting surfaces the moment the PI settles.


This is a defensible engineering choice. It keeps the downstream reporting surface consistent, refunds behave the same way, and legacy integrations don't break. But it leaves every merchant on the Agentic Commerce Suite unable to answer a basic question: *how is my agent channel performing?*


## Why "Just Tag the Metadata" Isn't Enough


The obvious workaround is to tag the PaymentIntent metadata yourself, setting` metadata.channel = "agentic"` at the moment of PI creation. Your backend knows this is an SPT flow because it's running a different code path. One extra line.


In practice, this fails for three reasons:


1.


**It requires engineering effort.** Agent traffic is still a small enough share of revenue that it rarely makes the sprint.


2.


**Tagging conventions drift.** Some teams use` agentic` , some use` agent` , some use` ai_channel` . Reconciling across them becomes its own cleanup project.


3.


**It only covers new integrations.** Every merchant already live on the Suite has a back catalog of agent transactions that were never tagged. You can't retroactively fix that.


The better answer is to detect agent transactions passively, without asking the merchant to change anything.


## What We Built


Corgi's agent payments intelligence layer sits on top of the webhook subscriptions we already maintain on merchant Stripe accounts.


When a merchant confirms a PaymentIntent with an SPT, Stripe fires webhook events related to the shared payment token lifecycle. Our pipeline correlates these events with the resulting PaymentIntent to identify SPT-originated transactions. We write the association to a dedicated table (` agentic_transactions` ) with the SPT ID, PaymentIntent ID, merchant account ID, and the event timestamp.


From there, every downstream metric in our pipeline gets an` is_agentic` flag through a join on PaymentIntent ID.


**No merchant code changes. No metadata discipline. No retroactive cleanup.** If we have webhook access to your Stripe account, we detect agentic transactions from the moment we connect. We can't recover pre-integration history, but from that point forward, no agent transaction goes untagged.


The same pipeline captures SPT revocation and expiry events, which matter for a specific class of agent fraud covered below.


## What the Analytics Show You


Once the detection layer is running, the real question becomes: what do agent transactions actually look like compared to human ones?


### Authorization Rates Diverge


Early agent traffic on SPTs tends to authorize higher than card-on-file for two reasons. Many SPT transactions use network tokens, which issuers trust more than raw PANs. Mastercard reports a 2.1% auth rate lift for tokenized transactions, and Visa cites 4.6%.


The risk signals Stripe forwards with the SPT (card testing likelihood, stolen card indicators, chargeback history) are also stronger than what issuers typically see on a standard ecommerce transaction. If your auth rate on agent traffic is *lower* than your card-on-file baseline, something is likely wrong with your integration, not with the channel.


### Decline Distributions Are Expected to Shift


Issuers are rolling out new authorization logic for Visa Intelligent Commerce and Mastercard Agent Pay. Based on early patterns, we expect the mix of` do_not_honor` ,` insufficient_funds` ,` invalid_account` , and network-declined reasons to look different for agent traffic.


If your retry logic is tuned to human-channel declines, it will likely make the wrong call a meaningful fraction of the time.


### Dispute Rates Are the Headline Metric


Stripe has publicly reported near-zero fraud on the first wave of Suite merchants. That's accurate for the early cohort: enterprise brands with mature fraud programs and a filtered customer base (buyers logged into Copilot or ChatGPT with saved payment methods).


As agent traffic scales past early adopters, expect the chargeback rate to move in ways that differ from human patterns. Two scenarios illustrate why:


-


**Scope disputes.** "I didn't authorize this purchase" claims get murkier when a buyer delegated authority to an agent but says the agent exceeded scope.


-


**Signal loss.** Traditional fraud signals vanish: no browser fingerprint, no mouse movement, no device telemetry. Fraud models tuned on human traffic will produce false declines on legitimate agents and miss adversarial ones at the same time.


### Platform Attribution Matters for GTM


Consider a merchant processing $50 million annually through the Suite. If 80% of their agent volume comes from ChatGPT and 5% from Copilot, that's a very different partnerships conversation than a 40/40 split. Without channel-level data, these decisions get made on gut feel.


## The Fraud Model Gap


Every fraud model in production today was trained on human payment traffic. Those models rely on signals that don't exist in agentic flows:


-


How the user navigated to checkout


-


What device they used


-


How they typed their card number


-


What else they did on the page


In an agentic world, the "user" is an API call. There is no device, no session, no typing cadence.


This creates two failure modes at once. Legitimate agent traffic gets scored as suspicious because it doesn't look human, leading to block rates that cost real revenue. Adversarial agents that *do* look human enough (scripted to mimic human behavior) slip past models never designed to catch them.


The answer isn't to throw out your fraud model. It's to know which transactions need to be scored by which model. That requires three things: a reliable channel flag, a drift monitor that alerts you to distribution shifts on agent traffic, and a retraining trigger indexed on agent volume crossing a threshold. Corgi Intelligence provides all three.


## Why We Built This First


Our merchants started asking. Not in the abstract, but with specific questions.


*"Can you tell me what my chargeback rate looks like on ChatGPT orders versus direct checkout?"*


That question had no answer in Stripe's dashboard. As far as we've been able to determine, it had no answer in other third-party tooling either. We shipped the first version in a week because the lift was small and the need was obvious.


We're among the first fraud intelligence platforms with a production detection layer for agentic commerce transactions on Stripe. Not because the idea was hidden, but because the infrastructure we already had (live webhook ingestion across merchant Stripe accounts) turned out to be the right foundation. We didn't build new plumbing. We recognized that one additional event type closed the analytics gap.


## Where This Goes Next


Agent payments intelligence is the starting point. The longer-term product is a fraud model explicitly trained on agentic traffic, one that uses SPT risk signals, agent platform attribution, and behavior patterns absent from any human-channel dataset.


The detection layer makes that training set possible. Every merchant we onboard adds labeled agent transactions to a growing corpus of agentic commerce data, the foundation for models purpose-built for a channel that barely existed 18 months ago.


If you're running a Stripe integration and you're live on the Agentic Commerce Suite (or about to be), the question worth asking your team this week is simple. *When the board asks how our agent channel is performing, what exactly will we show them?*


If the answer is "we'll figure it out later," we should talk.


[Be part of our CLOSED BETA](https://www.corgilabs.ai/beta/agentic-intelligence)


---


*Corgi Labs is a Y Combinator-backed payments intelligence platform built natively on Stripe. We help merchants detect fraud, recover false declines, and see their agent channel clearly.*


---


## Sources


1.


Stripe, "Introducing the Agentic Commerce Suite" —[https://stripe.com/blog/agentic-commerce-suite](https://stripe.com/blog/agentic-commerce-suite)


2.


Stripe, "Agentic Commerce Suite" (newsroom announcement) —[https://stripe.com/newsroom/news/agentic-commerce-suite](https://stripe.com/newsroom/news/agentic-commerce-suite)


3.


Stripe, "Shared Payment Tokens" (documentation) —[https://docs.stripe.com/agentic-commerce/concepts/shared-payment-tokens](https://docs.stripe.com/agentic-commerce/concepts/shared-payment-tokens)


4.


Stripe, "Developing an Open Standard for Agentic Commerce" —[https://stripe.com/blog/developing-an-open-standard-for-agentic-commerce](https://stripe.com/blog/developing-an-open-standard-for-agentic-commerce)


5.


Stripe, "10 Things We Learned Building for the First Generation of Agentic Commerce" —[https://stripe.com/blog/10-lessons](https://stripe.com/blog/10-lessons)


6.


Stripe, "Stripe Powers Instant Checkout in ChatGPT" —[https://stripe.com/newsroom/news/stripe-openai-instant-checkout](https://stripe.com/newsroom/news/stripe-openai-instant-checkout)


7.


Stripe, "Microsoft Copilot and Stripe" —[https://stripe.com/newsroom/news/microsoft-copilot-and-stripe](https://stripe.com/newsroom/news/microsoft-copilot-and-stripe)


8.


Stripe, "Supporting Additional Payment Methods for Agentic Commerce" —[https://stripe.com/blog/supporting-additional-payment-methods-for-agentic-commerce](https://stripe.com/blog/supporting-additional-payment-methods-for-agentic-commerce)


9.


Solidgate, "Network Tokenization and Authorization Rates" —[https://solidgate.com/blog/network-tokenization-authorization-rates/](https://solidgate.com/blog/network-tokenization-authorization-rates/)


10.


Mastercard, "Agent Pay: Pioneering Agentic Payments Technology" —[https://www.mastercard.com/global/en/news-and-trends/press/2025/april/mastercard-unveils-agent-pay-pioneering-agentic-payments-technology-to-power-commerce-in-the-age-of-ai.html](https://www.mastercard.com/global/en/news-and-trends/press/2025/april/mastercard-unveils-agent-pay-pioneering-agentic-payments-technology-to-power-commerce-in-the-age-of-ai.html)


11.


Y Combinator, "Corgi Labs" —[https://www.ycombinator.com/companies/corgi-labs](https://www.ycombinator.com/companies/corgi-labs)
