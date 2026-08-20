---
schema_version: "1.0.0"
document_id: "0e5506d0a3dbfa1fe8da60b2232269e80037a05ab7fdd19fe614182152c41fd3"
company_key: "grid-dynamics-holdings-inc-class-a-common-stock"
company: "Grid Dynamics Holdings Inc."
source_id: "grid-dynamics-holdings-inc-class-a-common-stock-news-import-14c47dcfb441"
canonical_url: "https://www.griddynamics.com/blog/why-ai-projects-fail-trust-architecture-in-agentic-commerce"
published_at: null
first_seen_at: "2026-07-23T10:48:25.089553+00:00"
fetched_at: "2026-07-28T21:40:00.658555+00:00"
content_hash: "sha256:682d7b29c7eef7875f94d4bc0e817ec32f7dd72ac727bf2668a427bbcea6a80d"
---

# The trust architecture: Why most agentic commerce pilots fail, and what separates the ones that don’t

[Home](https://www.griddynamics.com/)


[Insights](https://www.griddynamics.com/blog)


[Articles](https://www.griddynamics.com/blog/articles)


The trust architecture: Why most agentic commerce pilots fail, and what separates the ones that don't


# The trust architecture: Why most agentic commerce pilots fail, and what separates the ones that don’t


[Brennan Gleason](https://www.griddynamics.com/author/brennan-gleason)


Jun 05, 2026 • **9 min read**


Share


Follow


Subscribe


Follow


Table of Contents


**


- Why customers resist autonomous commerce experiences
- Where agentic AI pilots actually fail
- The four moments that decide the agentic commerce pilot
- What successful agentic AI deployments get right
- Why agentic commerce is really a trust architecture decision
- What next?
- References


The gap between a working demo and a system that survives real customers is the most expensive distance in the enterprise right now. It’s also widening.


Boards are writing checks for agentic commerce based on demos that won’t last a week against actual shoppers. The receipts are already in. Air Canada’s chatbot told a grieving customer he could claim the airline’s bereavement fare retroactively. He couldn’t 1. A tribunal made the airline pay out anyway. Volkswagen’s Cariad project, the attempt to unify software across Audi, Porsche, and VW, collapsed under integration debt and internal politics, costing roughly 1,600 jobs along the way 2. Taco Bell’s drive-thru AI buckled the first time customers behaved like customers 3. Pak’nSave’s Savey Meal-bot in New Zealand cheerfully recommended a chlorine gas recipe because nobody had told it which ingredient pairs could kill a person 4.


Most teams blame hallucination. Hallucination is the symptom. Agentic AI pilots fail at the seams, the moments of trust (consent, verification), and the moments of operation (handoff, repair) that demos conveniently skip. The model answers what. The architecture decides how, when, and whether at all. Right now, most organizations are pouring money into the floor and ignoring how low their ceiling is. **That asymmetry is going to sort the next three years of winners from everyone else.**


These failures point to a broader set of agentic commerce challenges: weak system integration, unclear decision boundaries, and a lack of trust design. Solving them requires more than better models. It requires building trust into every layer of the experience.


## Why customers resist autonomous commerce experiences


*Grid Dynamics survey of 201 conversational and agentic search users. The signal across questions points in the same direction: customers want competence, not personality.*


Grid Dynamics’ research team recently surveyed over 200 participants on conversational and agentic search. The sample is small. The signal is consistent enough to take seriously.


Only 2.49% of respondents wanted a witty assistant. In survey research, near-unanimity that strong is rare enough to stop on, and this question wasn’t badly worded. The rest of the survey points in the same direction.


- 44% are actively uncomfortable with AI making purchases for them.
- 70%+ prefer a professional, neutral tone, the register of a competent colleague rather than a quirky assistant.
- 68% want a hybrid experience that pairs conversational answers with traditional links they can verify.
- 66% prefer the agent to know where they are and what they just searched for, not what they bought last year.


Read that as a strategic signal, and it says something uncomfortable: customers aren’t asking for what most agentic roadmaps are building. They aren’t asking for more autonomy. They aren’t asking for more personality. They’re asking for competence, verification, and restraint. That’s a more conservative product than most teams are scoping.


Which raises a tension worth sitting with. If users want to save time but don’t trust the agent to act independently, what is the agent actually for?


I sat with this for a while. The answer the demos imply—autonomous agent, hands-off shopping—is exactly what most customers are rejecting. So either the demos are wrong about what people want, or the demos are right, and the survey is catching a transitional moment where customers haven’t yet caught up to what they’ll eventually accept. I don’t think it’s the second one. The discomfort numbers are too consistent across age and category, and they’re not moving the way you’d expect if this were just unfamiliarity wearing off.


Which leaves the first reading. The agent’s job, in the product that actually survives, is to compress the work without eliminating the oversight. Less time finding options, comparing them, checking claims. The same amount of time, or sometimes more, spent confirming the call.


That product looks different from the demo. It costs less to build wrong and more to build right. And once those expectations collide with real operational systems, the failure patterns become surprisingly predictable.


## Where agentic AI pilots actually fail


*The five seams where agentic pilots fail. The model usually performs as advertised. The connections around it are where trust gets lost.*


Study the failures and they cluster.


### **1. The grounding gap: When the agent loses the source of truth**


The agent isn’t tightly bound to the system of record, so it confabulates. Air Canada is the textbook case. From an engineering seat it’s a retrieval failure: the bot got the policy almost right, then invented a process for applying it that the airline didn’t actually offer. From a strategic seat it’s a governance failure, where nobody designed the line at which the agent’s authority ended and a customer would have known they’d crossed it. Both readings are true. Fixing it takes both.


### **2. Edge-case fragility: Exposing weak agent design**


The Taco Bell pattern. An agent runs cleanly on ninety percent of orders and folds when a customer orders eighteen thousand water cups, or speaks with an accent the training data missed, or simply asks for something weird. The cost isn’t the failed order. It’s the human staff cleaning up the wreckage while the line backs up behind them.


### **3. Decision design that nobody specified: Where AI governance breaks down**


Pilots in supply chain, customer service, and procurement keep collapsing back into spreadsheets because the team built an agent without designing the escalation path. Who owns the call when something goes sideways? At what confidence threshold does the agent stop and ask? These are governance questions dressed in engineering clothes, and most pilots leave them unanswered until something has already broken in public.


### **4. The integration chasm: When organizational chaos breaks the system**


Cariad is the cautionary tale at scale, though the honest version of that story is a tangle of org politics, shifting hardware specs, and corporate complexity that no AI project was going to survive. The lesson isn’t *integration kills agents* . It’s that you can’t bolt agentic capability onto organizational chaos and expect it to hold. The chaos wins.


### **5. The confident lie: The answers that destroy customer trust**


This is the failure mode that keeps legal teams up at night. Wrong answers delivered in the same fluent cadence as right ones. Wrong delivery dates at checkout. Promotions applied inconsistently. Policies that *sound* real because the model has learned the rhythm of policy language without learning what’s actually true.


What links these patterns is geometry. Every failure happens at a seam, most often the seam between what the agent claims to know and what the company can actually deliver.


Seam design is the unglamorous work that pays off, and it’s exactly the work most teams skip because the demo doesn’t ask for it. Those seams show up repeatedly, but they become most visible during a handful of high-stakes moments in the customer journey.


## The four moments that decide the agentic commerce pilot


Want to know whether an[agentic commerce](https://www.griddynamics.com/blog/agentic-commerce-disruption) experience will survive contact with reality? Watch four moments.


### 1. Discovery to decision-making


*The two jobs (discovery and decision-making) shopping actually does. Most agents try to handle both with one prompt. The ones that work classify intent first.*


Online shopping is at least two jobs, and the agents that work tell them apart. Discovery is exploratory and subjective: a shopper asking for “a meaningful gift for someone who hates clutter” wants a thoughtful narrative, not a spec sheet. Decision is comparative and evidence-hungry: a shopper choosing between three mattresses wants citations, real-time inventory, and total landed cost.


The agents that work design for both modes on purpose.


Spotify’s published engineering writing describes using an LLM to classify whether the user is in discovery or navigation mode before responding. Amazon’s Rufus does well in discovery and has historically struggled with decision-stage rigor.


The technical prerequisite is customer intent classification at the entry point, not a single model trying to do both jobs from the same prompt and hoping the user adapts.


### 2. The autonomous transactional boundary


The moment money moves is the moment trust is most fragile. Two competing approaches to[agentic payments](https://www.griddynamics.com/blog/agentic-payments) are emerging, and they’re not just engineering choices.


OpenAI and Stripe are pushing one path: the purchase happens inside the chat, with the agent brokering tokenized payment so it never sees the raw card. A coalition that includes Google, Shopify, and Walmart is pushing the other path, where the merchant keeps control of checkout and the protocol requires verifiable proof of consent for every authorization.


Frame this as payments plumbing, and you’ll miss the actual question. It’s about platform sovereignty. If your purchase happens inside someone else’s agent, you’re a fulfiller. If your customers authorize transactions through architecture you control, you keep the relationship. That distinction will define enterprise value over the next decade. The technical prerequisite is a real-time state layer that can broker consent, not a prettier chat interface.


Executives making infrastructure decisions this quarter are choosing sides. Most don’t realize it’s a customer-ownership decision until later.


### 3. The customer trust repair moment


Klarna is worth studying carefully. The company’s AI assistant launched in 2024 to numbers that looked spectacular: 2.3 million chats in the first month, average resolution time falling from eleven minutes to two, with around forty million dollars in projected annual savings. By 2025, Klarna had restructured. The AI was hitting its operational targets and missing the moments that defined customer relationships: the de-escalation work, the read-between-the-lines empathy that frustrated customers when something has gone wrong.


Klarna shifted to what they’re calling an Uber-style model 5, with AI on routine traffic and humans on the moments that matter. That wasn’t a retreat. It was an architectural decision. They had optimized for resolution time without designing the emotional handoff, and the savings were costing them something more valuable than the time they were saving.


How an agent fails defines its brand more reliably than how it succeeds. Conversation analysis researchers have a clean hierarchy of recovery moves: defer to a human, offer specific options, explain what went wrong, ask the user to repeat. Most teams ship the cheapest recovery move, “Sorry, can you say that again?”, and call it done. The technical prerequisite is sentiment detection wired to escalation logic.


### 4. The invasive personalization line


This one matters most for retail, because retailers have the most customer data and the strongest temptation to use all of it.


The early data points one direction. Users welcome situational context: your general location, what you searched for ten minutes ago, and the device you’re on. They recoil from agents that surface deep profile data without warning. Sixty-six percent want the light touch.


A fair counterpoint: Amazon and Netflix have built dominant businesses on deep profile integration. The difference isn’t the data. It’s the interface.


Recommendation rows on a homepage feel like helpful curation. The same data deployed conversationally, surfaced by an agent that brings up details the user didn’t realize you knew, feels invasive. The line isn’t about how much data you use. It’s about how the data is surfaced. A row of suggestions on a homepage feels like the store layout: curated, ambient, and ignorable. The same data spoken aloud by a chatbot (“I noticed you’ve been buying running shoes.”) feels like someone read your file.


Drawing that line carefully isn’t a privacy compliance task. It’s the durable advantage.


The organizations scaling agentic commerce successfully aren’t avoiding these moments. They’re designing for them deliberately.


## What successful agentic AI deployments get right


Watch the organizations whose pilots are quietly scaling, and a few habits show up.


**They invest in machine-readable brand identity before they invest in agent fluency.** That’s more than clean data. It’s structuring product information, inventory, pricing, and policy as a legible source of truth that external agents can’t help but prefer when they’re choosing what to recommend. In an agentic ecosystem, brands that are technically illegible to AI become invisible to the customers who shop through it. The ones whose data architecture treats the agent as a reader get surfaced.


**They design for the handoff, not the autonomy.** Explicit escalation paths. Calibrated uncertainty signals. Human review at the moments where the stakes are highest. The pilots that survive aren’t the most autonomous. They’re the ones that know when to step back.


**They treat repair as a first-class design problem.** Failure paths get the same investment as success paths. Recovery is tested with the rigor reserved for happy paths in less serious shops.


**They calibrate personalization to context, not capability.** Just because you can surface the deep profile data in conversation doesn’t mean you should. Restraint is the trust-builder.


None of this demos well. None of it shows up in the highlight reel a vendor brings to your CFO. All of it shows up in whether the pilot is still running a year later. Which is why the real competitive advantage isn’t the AI model itself. It’s the operational and experience architecture surrounding it.


## Why agentic commerce is really a trust architecture decision


Agentic commerce isn’t an AI buying decision. It’s a trust architecture decision, and trust architecture lives at the intersection of model performance, data quality, and experience design. The model is the cheap part. The architecture is what you’re actually buying. Most organizations are spending on the first and assuming the second will sort itself out.


The companies treating UX as a service team, the people who make the chat feel friendly after the engineers have built the real thing, are the ones whose pilots are failing in public. The companies treating it as a strategic discipline, the ones who design the seams where trust is won or lost, are the ones whose pilots are quietly making it into production.


Three years from now, the league table won’t be sorted by who shipped the most ambitious agent. It’ll be sorted by who decided, early and against the demo culture, that the model was the cheap part of the buy.


The problem is still early, and most organizations are learning in production faster than the agentic AI research is catching up.


## What next?


*A note on what’s coming. Most published research on agentic commerce right now is vendor case studies and analyst predictions. Neither tells you what actual customers do when they’re handed a conversational shopping experience and left alone with it. The Grid Dynamics UX Design and Research practice is running primary studies through 2026 on the moments where consent breaks down, how customers respond to different repair patterns, where the personalization line actually sits in practice, and what separates the agentic shopping experience people return to from the ones they try once and forget. The survey shared above is one slice.*


## References


1. [Air Canada ordered to pay customer who was misled by airline’s chatbot | Canada | The Guardian](https://www.theguardian.com/world/2024/feb/16/air-canada-chatbot-lawsuit?utm_source=chatgpt.com)
2. [Volkswagen to lay off 1,600 staff at Cariad software unit, Handelsblatt reports | Reuters](https://www.reuters.com/business/autos-transportation/volkswagen-lay-off-1600-staff-cariad-software-unit-handelsblatt-reports-2025-03-11/?utm_source=chatgpt.com)
3. [Taco Bell Rethinks Future of Voice AI at the Drive-Through – WSJ](https://www.wsj.com/articles/taco-bell-rethinks-future-of-voice-ai-at-the-drive-through-72990b5a?utm_source=chatgpt.com)
4. [Supermarket AI meal planner app suggests recipe that would create chlorine gas | New Zealand | The Guardian](https://www.theguardian.com/world/2023/aug/10/pak-n-save-savey-meal-bot-ai-app-malfunction-recipes?utm_source=chatgpt.com)
5. [As Klarna flips from AI-first to hiring people again, a new landmark survey reveals most AI projects fail to deliver | Fortune](https://fortune.com/2025/05/09/klarna-ai-humans-return-on-investment/?utm_source=chatgpt.com)


## Tags


[Agentic AI](https://www.griddynamics.com/blog/agentic-ai)


[Agentic commerce](https://www.griddynamics.com/blog/agentic-ai-commerce)


[AI-driven search and experiences](https://www.griddynamics.com/blog/ai-driven-search-and-experiences)


[Artificial intelligence](https://www.griddynamics.com/blog/ai)


[Customer experience](https://www.griddynamics.com/blog/customer-experience)


[Digital engagement](https://www.griddynamics.com/blog/digital-engagement)


[Retail](https://www.griddynamics.com/blog/retail)


Share


Follow


Subscribe


Follow


## You might **also like**


Article


Shift auto parts search into high gear with Google Cloud and Grid Dynamics


[Automotive](https://www.griddynamics.com/blog/automotive)


Article


[Shift auto parts search into high gear with Google Cloud and Grid Dynamics](https://www.griddynamics.com/blog/auto-parts-search)


Auto parts e-commerce is booming, but complexity risks revenue. Think fitment accuracy, interchange precision, catalog and PDP content standardization, and omnichannel expectations. One misfit leads to a lost sale, and can even jeopardize customer safety.​ Auto parts search is in a dif...


[Automotive](https://www.griddynamics.com/blog/automotive)


Article


Six reasons your product catalog needs a makeover in 2026—and how to get it right


[Retail](https://www.griddynamics.com/blog/retail)


Article


[Six reasons your product catalog needs a makeover in 2026—and how to get it right](https://www.griddynamics.com/blog/enterprise-product-catalog-optimization)


Once upon a time, your enterprise product catalog was a backend concern. A necessary system of record. Something teams updated quietly while the real “experience” work happened elsewhere. Today, that separation no longer exists. Research shows that 87% of shoppers rate product data as “extremely...


[Retail](https://www.griddynamics.com/blog/retail)


Article


Hybrid deep learning with Azure Databricks and on-prem NVIDIA DGX


[Financial services](https://www.griddynamics.com/blog/financial-services)


Article


[Hybrid deep learning with Azure Databricks and on-prem NVIDIA DGX](https://www.griddynamics.com/blog/hybrid-deep-learning-azure-databricks-nvidia-dgx)


Modern enterprises increasingly rely on deep learning to power mission-critical workflows such as global demand forecasting, inventory optimization, supply chain prediction, video-based defect detection, and financial risk modeling. These workloads demonstrate rapidly increasing GPU requirements, g...


[Financial services](https://www.griddynamics.com/blog/financial-services)


Article


Time-series foundation models: AI demand forecasting comparison


[Manufacturing](https://www.griddynamics.com/blog/manufacturing)


Article


[Time-series foundation models: AI demand forecasting comparison](https://www.griddynamics.com/blog/ai-models-demand-forecasting-tsfm-comparison)


Predictive analytics is undergoing a major transformation. This AI demand forecasting model comparison reveals significant performance gaps between traditional and modern approaches. Demand forecasting has long guided decisions in retail and manufacturing, but today’s data volumes and volatility ar...


[Manufacturing](https://www.griddynamics.com/blog/manufacturing)


Article


What the ACP vs AP2 agentic payments comparison means for you


[Retail](https://www.griddynamics.com/blog/retail)


Article


[What the ACP vs AP2 agentic payments comparison means for you](https://www.griddynamics.com/blog/agentic-payments)


Agentic commerce is in the midst of a defining moment. Instead of a customer navigating a checkout flow, AI shopping agents can now autonomously purchase goods, renew subscriptions, or restock supplies, executing payments entirely on the customer’s behalf through agentic payments protocols. It’s...


[Retail](https://www.griddynamics.com/blog/retail)


Article


Beyond multichannel: The competitive edge of omnichannel order management


[Retail](https://www.griddynamics.com/blog/retail)


Article


[Beyond multichannel: The competitive edge of omnichannel order management](https://www.griddynamics.com/blog/omnichannel-order-management-system)


You know the feeling: you walk into a store only to find out that the product you saw online is out of stock! This is one of the most common and problematic experiences for customers who shop multichannel retail. The problem for you? Disconnected sales channels, lost income, frustrated custom...


[Retail](https://www.griddynamics.com/blog/retail)


Article


Composable commerce for B2B: Overkill or delivers big?


[High-tech](https://www.griddynamics.com/blog/high-tech)


Article


[Composable commerce for B2B: Overkill or delivers big?](https://www.griddynamics.com/blog/b2b-composable-commerce)


The buzzword “composable commerce” has dominated digital strategy conversations since Gartner popularized the term in 2020. But behind the marketing hype lies a longstanding, proven practice of integrating specialized, best-of-breed technology components into a flexible and scalable ecosystem....


[High-tech](https://www.griddynamics.com/blog/high-tech)


### Subscribe to Grid Dynamics
insights now
