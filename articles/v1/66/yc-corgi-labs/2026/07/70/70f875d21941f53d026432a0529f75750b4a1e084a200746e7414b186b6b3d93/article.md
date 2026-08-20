---
schema_version: "1.0.0"
document_id: "70f875d21941f53d026432a0529f75750b4a1e084a200746e7414b186b6b3d93"
company_key: "yc-corgi-labs"
company: "Corgi Labs"
source_id: "yc-corgi-labs-atom-a99da208b4cc"
canonical_url: "https://www.corgilabs.ai/insights/fraud-detection-agentic-commerce"
published_at: "2026-07-27T15:00:00+00:00"
first_seen_at: "2026-07-27T15:41:10.735014+00:00"
fetched_at: "2026-07-28T20:32:04.512542+00:00"
content_hash: "sha256:679719e035bfd4c47952199e3ad3363939c097f8c46782da641e97cc20499e5d"
---

# Your Fraud Model Wasn't Built for Buyers That Aren't Human

## One in Six Purchases Won't Come From a Human


By 2028, Gartner projects that 15% of all e-commerce transactions will involve some form of AI agent assistance. That means one in roughly six purchases on your platform could be initiated by software acting on a consumer's behalf.


Your fraud model was trained on humans. It watches for human browsing speeds, human typing patterns, and human purchase cadences. When the buyer isn't human, those signals stop working.


This isn't hype. In the last week of April 2025, both major card networks made their moves. Mastercard unveiled Agent Pay on April 29, followed a day later by Visa launching its Intelligent Commerce platform with partners including Anthropic, Microsoft, OpenAI, Samsung, and Stripe. Meanwhile, Stripe itself is building delegated authorization infrastructure. The message is clear: **agentic commerce is arriving, and it's arriving fast.**


## What Is Agentic Commerce, Exactly?


Agentic commerce is when an AI agent makes purchases on behalf of a consumer. The consumer delegates authority ("buy me running shoes under $150 from a reputable brand") and the agent handles research, selection, and checkout autonomously.


Think of it as the difference between a consumer clicking "buy" and a consumer telling their AI assistant to handle it. The payment still uses the consumer's credentials. But the entity initiating the transaction is software, not a person.


That distinction changes everything about how fraud detection works.


## Five Fraud Vectors Your Current Stack Doesn't See


Agentic commerce introduces attack surfaces that traditional fraud rules weren't designed to catch.


**1. Credential misuse that looks human.** When a fraudster uses stolen credentials through an AI agent, the agent mimics natural browsing behavior. It browses catalogs at realistic speeds and adds items to carts over hours. Traditional bot detection flags these interactions as low risk because they don't match typical scraping or bulk-purchase patterns.


**2. Agent spoofing and session hijacking.** Fraudsters can create fake agent identities or hijack legitimate agent sessions to make unauthorized purchases. Because agents operate through APIs and cloud infrastructure, verifying whether an agent is "real" requires entirely new authentication frameworks.


**3. Synthetic agent identities.** Just as synthetic human identities have plagued 'Know Your Customer' processes, synthetic agent profiles with fabricated transaction histories are emerging. These agents build credibility over time before conducting fraud.


**4. Proxy purchases through mule networks.** Agents can be configured to make purchases on behalf of mule accounts, distributing fraudulent transactions across hundreds of seemingly unrelated buyers. Each individual transaction looks clean.


**5. Agent-to-agent collusion at scale.** Coordinated rings of compromised agents can conduct distributed fraud across multiple merchants simultaneously. The volume and coordination possible with automated agents dwarfs what human fraud rings can achieve.


Based on fraud patterns reported at industry events in early 2025, a representative scenario illustrates the risk: a fraud ring deployed AI agents on fashion e-commerce sites that created accounts with AI-generated profile photos, browsed at human speeds, and used stolen cards at checkout. **Traditional bot detection scored them as low risk.** The operation was only discovered when a merchant noticed identical writing patterns in customer service inquiries across "different" accounts.


## Why Traditional Fraud Detection Breaks Down


Rule-based fraud systems assume the buyer is human. That assumption is embedded in nearly every signal they use.


**Velocity rules assume human purchase patterns.** A rule that flags three purchases in five minutes makes sense for a person. For an AI agent authorized to comparison-shop and buy across multiple merchants, three purchases in five minutes is normal behavior.


**Device fingerprinting fails on cloud infrastructure.** Legitimate agents run from cloud environments, often sharing IP ranges and device characteristics with thousands of other services. An agent running on AWS looks identical to a fraud bot running on AWS.


**Behavioral biometrics models break.** These models are trained on mouse movements, scroll patterns, and typing cadence. An agent interacting through APIs produces none of those signals. The model doesn't flag it as suspicious; it simply can't see it. (Agents using browser automation may produce some mouse and scroll signals, but most current agent architectures interact at the API level.)


**3D Secure assumes a human will respond.** Step-up authentication prompts ("enter your code") require a person on the other end. When an agent initiates the purchase, there may be no human available to respond, forcing merchants to choose between blocking legitimate agent purchases or bypassing their strongest authentication layer.


According to Ethoca/Verifi network data, the false decline rate for transactions from cloud-hosted environments (where many agents operate) is already 3.2x higher than residential IP transactions. As agentic volume grows, this problem compounds.


## How ML-Based Fraud Models Adapt


The same machine learning capabilities that make agents possible also make detecting agent-based fraud possible. The key is **training models on agent-specific behavioral data** rather than retrofitting human-behavior models.


Here's what that looks like in practice:


1.


**Agent behavioral signatures.** Instead of mouse movements, track API call patterns, decision latency, and prompt structures. A legitimate agent has consistent behavioral fingerprints. A compromised or spoofed one doesn't.


2.


**Cryptographic identity verification.** Visa's Intelligent Commerce platform assigns each agent a scoped token specifying merchant, amount limits, and product categories. Visa's own pilot data shows a 60% reduction in agent-related fraud compared to traditional card-not-present transactions when these tokens are enforced.


3.


**Graph-based collusion detection.** Graph neural networks can map relationships between agents, transactions, and merchants to spot coordinated fraud rings. One suspicious transaction in isolation looks clean. Hundreds of related transactions across a network reveal the pattern.


4.


**Preference-model anomaly detection.** A legitimate agent develops consistent purchasing preferences based on its consumer's behavior. A sudden shift (an agent that always buys running gear suddenly purchasing luxury electronics) signals compromise or hijacking.


5.


**Merchant-specific training data.** Network-level fraud rules apply the same thresholds across all merchants. Models trained on your specific transaction patterns learn what "normal" looks like for your business, including what normal agent behavior looks like on your platform.


## What Payment Ops Teams Should Do Now


Agentic commerce volumes are still small, but the infrastructure is being built today. Waiting until agent transactions represent a meaningful percentage of your volume means retrofitting your fraud stack under pressure.


Start with these steps:


-


**Audit your fraud rules for agent-specific blind spots.** Test your current velocity rules, device fingerprinting, and behavioral models against simulated agent transactions. Identify which rules would fire on legitimate agent activity and which would miss fraudulent agents.


-


**Request documentation from your PSP.** Ask your payment processor about their agentic commerce roadmap. Specifically, ask about agent identity verification, tokenization support, and what new data fields will be available for fraud decisioning.


-


**Prepare for new data points.** Agent attestations, permission scopes, and delegated authority tokens are becoming standard fields in transaction payloads. Your fraud models need to ingest and weight these signals.


-


**Evaluate your model architecture.** Rule-based systems will struggle with agentic commerce. ML models trained on merchant-specific data can adapt to new transaction patterns as they emerge, learning what legitimate agent behavior looks like on your platform rather than applying network-wide averages.


## The Architecture Is Changing


Agentic commerce is not a future problem. Visa and Mastercard are live with agent frameworks. Visa projects tokenized payment credentials for agent use will reach 500 million or more by end of 2026. Stripe, Adyen, and Checkout.com are building agent-aware capabilities into their infrastructure.


The fundamental shift is clear: fraud detection is moving from "verify the human" to "verify the algorithm." Your fraud model needs to understand not just whether a transaction is legitimate, but whether the entity initiating it has proper authorization, consistent behavioral patterns, and valid cryptographic credentials.


Corgi Model trains on your merchant-specific transaction data, learning what normal looks like for your business. As agentic transactions enter your payment flow, that same approach applies: custom ML that adapts to new buyer types without waiting for network-level rule updates. No development work required.


**The buyers are changing. Your fraud model should too.**


[Book a Demo →](https://www.corgilabs.io/demo)


---


## Sources


1.


"Visa and Mastercard unveil AI-powered shopping,"[TechCrunch, April 30, 2025](https://techcrunch.com/2025/04/30/visa-and-mastercard-unveil-ai-powered-shopping/) .


2.


Visa Intelligent Commerce announcement materials,[Visa Press Center, 2025](https://usa.visa.com/about-visa/newsroom/press-releases.releaseId.21361.html) .


3.


Mastercard Agent Pay announcement,[Mastercard Newsroom, April 2025](https://www.mastercard.com/global/en/news-and-trends/press/2025/april/mastercard-unveils-agent-pay-pioneering-agentic-payments-technology-to-power-commerce-in-the-age-of-ai.html) .


4.


[Stripe Sessions 2025 presentations](https://stripe.com/blog/top-product-updates-sessions-2025) and API documentation on agentic payment infrastructure.


5.


[Gartner research on AI agent e-commerce projections, 2025](https://www.gartner.com/en/newsroom/press-releases/2025-10-21-gartner-unveils-top-predictions-for-it-organizations-and-users-in-2026-and-beyond) .


6.


[Akamai](https://www.akamai.com/blog/security-research/online-fraud-abuse-2025-ai-drivers-seat) and[Imperva](https://www.imperva.com/blog/2025-imperva-bad-bot-report-how-ai-is-supercharging-the-bot-threat/) reports on bot traffic and automated login attempt statistics, 2024-2025.


7.


[Ethoca/Verifi network data on false decline rates by IP type](https://www.ethoca.com/blog/how-the-right-data-can-help-you-prevent-fraud-and-reduce-false-declines-and-chargebacks) .


8.


[Visa pilot program materials on tokenized agent architecture results, 2025](https://usa.visa.com/about-visa/newsroom/press-releases.releaseId.21961.html) .
