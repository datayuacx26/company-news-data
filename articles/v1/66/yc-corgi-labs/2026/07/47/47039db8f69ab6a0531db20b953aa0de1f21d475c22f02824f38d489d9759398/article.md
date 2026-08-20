---
schema_version: "1.0.0"
document_id: "47039db8f69ab6a0531db20b953aa0de1f21d475c22f02824f38d489d9759398"
company_key: "yc-corgi-labs"
company: "Corgi Labs"
source_id: "yc-corgi-labs-atom-a99da208b4cc"
canonical_url: "https://www.corgilabs.ai/insights/agentic-commerce-fraud"
published_at: "2026-07-28T15:02:55.276+00:00"
first_seen_at: "2026-07-28T15:46:28.953740+00:00"
fetched_at: "2026-07-28T20:31:35.420648+00:00"
content_hash: "sha256:a018d374fe58a46c0ebaf50802706bdc810d18fa75a4a7890ebd1b969c8d9e06"
---

# AI Agents Are Learning to Buy. What About Fraud?

## The customer who never gets emotional


Last holiday season, AI agents placed $262 billion in online orders by Salesforce's count. Somewhere in that number, an agent compared prices across dozens of stores, picked a merchant, and checked out at 4am in under a second. No hesitation, no browsing, none of the messy human signals your fraud stack was trained to read.


That customer is not going away. The question I keep hearing from founders is how agents will pay. The question almost nobody asks is the one I care about: what does fraud look like when they do?


## The rails got built in seven months


The payments industry moved on this faster than on anything in recent memory. Four separate agent-payment frameworks shipped between April and October 2025:


-


**Mastercard Agent Pay** (April): Agentic Tokens that bind a card credential to a specific agent, merchant scope, and consent policy.


-


**Agentic Commerce Protocol** (September): the open standard from Stripe and OpenAI behind Instant Checkout in ChatGPT.


-


**Google AP2** (September): an open protocol both card networks joined.


-


**Visa Trusted Agent Protocol** (October): signed agent credentials and intent passed with the transaction.


Each answers "how does an agent pay?" at a different layer of the stack. Notice what's missing from that sentence: none of this, by itself, answers what happens to fraud detection when the buyer stops being human.


And the traffic is already here. Adobe's analytics show AI-driven visits to US retail sites up 393% year over year in early 2026, converting better than traditional channels. This stopped being a thought experiment about a year ago.


## Your fraud model has never met an agent


Fraud models learn what "normal" looks like from years of human behavior: session patterns, time on page, typing rhythms, the 1am impulse buy. An agent breaks nearly all of those signals at once. It moves too fast, too cleanly, and too consistently, which is exactly what fraud models were taught to flag.


Machine learning people call this distribution drift: the traffic the model scores stops resembling the traffic it trained on. In practice it means a model that was excellent against human fraudsters starts making confident mistakes in both directions.


The uncomfortable part is how thin the line is. DataDome logged nearly 8 billion AI agent requests in the first two months of 2026, and separate analysis puts the behavioral gap between legitimate automation and fraud at about half a percentage point. Blocking everything that looks automated used to be a safe default. Now it means declining real buyers who happen to shop through an agent, and if you read our last post, you know what false declines cost.


## Machine versus machine might actually be easier


Here's the counterintuitive thing I keep coming back to: fraud detection against machines could end up easier than against humans.


Any agent transacting at scale has an underlying probability distribution. It behaves consistently, because it's software. Humans don't. A guy gets into a fight with his girlfriend in the morning and buys something expensive by noon, and no model saw it coming.


Consistency is something you can model tightly, and deviation from consistency becomes a clean fraud signal. It's early, and this thesis still has to be borne out at scale, but the shape of it is promising. The chaos was always the hard part.


## The new fraud patterns are already here


The attackers didn't wait for the standards to settle. Stripe has framed its new Radar defenses around a threat it says is already real: a few thousand humans operating millions of agents to steal payment tokens from AI products. Card testing was always automated; now the automation reasons, adapts, and imitates legitimate agents.


Impersonation is measurable today. DataDome found about 2.4% of traffic claiming to be PerplexityBot was spoofed, with millions of forged requests wearing other agents' names.


And a new dispute category is emerging: "the AI bought it without my permission." That's friendly fraud in a new costume, and sometimes it's even true, which makes consent records suddenly load-bearing. Subscription merchants already know how this movie goes.


## Scoped tokens are the early answer


The common thread across every framework is replacing inference with attestation. Instead of your fraud model guessing who's behind a checkout, the payment itself carries proof.


Stripe's shared payment token is a good example of the mechanism: the agent never holds raw card credentials, and the token is scoped to one merchant and one cart total. Stolen, it's nearly worthless. Mastercard's Agentic Tokens and Visa's signed intent payloads make the same move at the network layer: identity and consent travel with the transaction instead of being reconstructed after the fact.


For merchants, that turns "is this bot good or bad?" from a behavioral guess into a credential check, with fraud models focusing on what credentials can't prove.


## What to do before your first agent customer


You likely already had one. Four things worth doing this quarter:


1.


Measure what share of your checkout traffic is agent-driven, even roughly. You can't reason about a segment you haven't sized.


2.


Stop hard-blocking known agents by default, and classify them instead. A blanket bot rule now declines real revenue.


3.


Watch your dispute reasons for "I didn't authorize my agent." Keep consent and intent records where you can get them.


4.


Ask your payment provider what agent identification they support today, such as Stripe's shared payment tokens, and what signals they pass you.


This is the frontier we're building on: we're already working with Stripe's shared payment tokens in beta to understand how agent traffic differs platform to platform, and Corgi Model is trained on your transactions, so as your traffic shifts toward agents, the model shifts with it instead of defending a world that no longer exists. Corgi Intelligence shows you the funnel either way, humans and agents alike.


The buyers are changing faster than the defenses. Being early here is cheap. Being late won't be.


[Book a Demo →](https://www.corgilabs.ai/#book-demo)


## Sources


-


Salesforce holiday shopping data via commercetools, "Agentic Commerce Stats 2026: Enterprise Guide"[https://commercetools.com/blog/agentic-commerce-stats-enterprise-guide](https://commercetools.com/blog/agentic-commerce-stats-enterprise-guide)


-


Adobe Analytics AI traffic data via Elogic, "ChatGPT Commerce & Agentic Shopping Statistics 2026"[https://elogic.co/blog/chatgpt-commerce-statistics/](https://elogic.co/blog/chatgpt-commerce-statistics/)


-


Stripe, "Stripe powers Instant Checkout in ChatGPT and releases Agentic Commerce Protocol codeveloped with OpenAI"[https://stripe.com/newsroom/news/stripe-openai-instant-checkout](https://stripe.com/newsroom/news/stripe-openai-instant-checkout)


-


Stripe, "Introducing our agentic commerce solutions"[https://stripe.com/blog/introducing-our-agentic-commerce-solutions](https://stripe.com/blog/introducing-our-agentic-commerce-solutions)


-


Stripe Documentation, "Shared payment tokens"[https://docs.stripe.com/agentic-commerce/concepts/shared-payment-tokens](https://docs.stripe.com/agentic-commerce/concepts/shared-payment-tokens)


-


Visa, "Visa Unveils Trusted Agent Protocol for AI Commerce"[https://corporate.visa.com/en/sites/visa-perspectives/newsroom/visa-unveils-trusted-agent-protocol-for-ai-commerce.html](https://corporate.visa.com/en/sites/visa-perspectives/newsroom/visa-unveils-trusted-agent-protocol-for-ai-commerce.html)


-


Stellagent, "Mastercard Agent Pay Explained: Agentic Tokens and Verifiable Intent for AI Agent Payments"[https://stellagent.ai/insights/mastercard-agent-pay-agentic-tokens](https://stellagent.ai/insights/mastercard-agent-pay-agentic-tokens)


-


DataDome, "The Agentic Threats & Industry Trends Defining 2026 (So Far)"[https://datadome.co/threat-research/agentic-threats-and-industry-trends-defining-2026-so-far/](https://datadome.co/threat-research/agentic-threats-and-industry-trends-defining-2026-so-far/)


-


PPC Land, "AI agents are now buying things - and fraud looks identical"[https://ppc.land/ai-agents-are-now-buying-things-and-fraud-looks-identical/](https://ppc.land/ai-agents-are-now-buying-things-and-fraud-looks-identical/)


-


Saif's talk at the Founder Meetup, San Francisco, May 2026 (internal recording)
