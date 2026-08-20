---
schema_version: "1.0.0"
document_id: "54ecdba687281c1957add087a8fb4763704b25bc378117332d4b057e17293f8f"
company_key: "wayfair-inc-class-a-common-stock"
company: "Wayfair Inc."
source_id: "wayfair-inc-class-a-common-stock-news-import-42cff1750757"
canonical_url: "https://www.aboutwayfair.com/careers/tech-blog/vision-language-models-in-the-wild-damage-diagnostics-from-customer-photos"
published_at: "2026-01-30T16:16:55.818+00:00"
first_seen_at: "2026-07-22T19:30:59.923738+00:00"
fetched_at: "2026-07-28T21:26:59.511520+00:00"
content_hash: "sha256:f136e9f2f1a7bcffe8ad2351e0e4725c5e81e80fd7df1fc4ea756b01572febae"
---

# Vision-Language Models in the Wild: Damage Diagnostics from Customer Photos

Providing a fast and fair resolution when an item arrives damaged is critical for maintaining customer trust at Wayfair. When customers report a problem, whether online or with an agent, we often ask for photos because they’re the quickest way to understand what happened and choose the right resolution.


Historically, interpreting those photos meant manual review and follow-up questions, which is slow, inconsistent and hard to scale, exactly when the customer wants a quick answer. To save time and improve reliability at scale, we built **Saffron** , Wayfair’s post-order AI vision-language assessment service, which turns customer photos into structured signals to support real-time resolution flows.


# The Wild World of Customer Photos


Figure 1: A sample of the wide range of photos submitted by customers as evidence of an issue with their order.


Customer-submitted images are challenging to analyze due to:


- **High variance** : lighting, angles, occlusions, packaging, background clutter
- **Low standardization** : no fixed distance, no scale reference, inconsistent framing
- **Real stakes** : the output impacts customer experience, operational cost and trust


Traditional computer vision approaches can work well for specific tasks, but they typically require extensive labeled data and careful retraining as products, scenarios and business requirements evolve.
We took a different approach: use **vision-language models (VLMs)** to interpret images *in context* , then deliver outputs that downstream decision systems can safely consume.


# How AI Fits Into the Customer’s Journey


Figure 2: Key steps in the customer journey for reporting a problem with their order, including where AI is powering decision-making.


Saffron supports the customer journey at multiple points in the Report a Problem workflow. For example, it evaluates the customer’s photos and incident context to determine whether a **replacement part** is the best fix for the customer. If not, it streamlines the process to a more appropriate resolution, such as a **replacement unit** or **refund,** without unnecessary delays.


Additionally, when a replacement unit or refund is issued, Saffron evaluates the photos for potential recovery value. We plan to use this to inform downstream reverse logistics processes – including whether we send the product to liquidation or to open box resale. Depending on the result of the assessment, the return might get routed to a different part of our supply chain.


Saffron is designed with modularity, allowing it to support a growing number of use cases. These currently include verifying photo relevance and detecting AI-generated images, with more capabilities continuously being developed.


# Under the Hood: Building a Scalable Vision Engine


## From traditional ML to multimodal LLM


Saffron uses multimodal vision-language models (VLMs), such as Gemini-class models, to reason over various conceptual, context-driven and evidence-based questions related to the product and incident. This lets us answer questions like: *“Will a replacement part actually fix this?”* or *“Is this item salvageable enough to justify a return?”.* These are judgments that depend on context, not just pixel patterns.


The key shift is that we’re not only predicting a label. We’re producing **structured assessments** that downstream systems can safely consume.


## Architecture overview


At a high level, Saffron is a service that supports two modes:


1. **Preprocessing** (async): start model work early, as soon as photos are uploaded.
2. **Retrieval** (fast): return cached, structured results when the workflow needs them.


The architecture is critical because, while powerful, modern VLMs have substantial latency. A single model API call can take **seconds at the tail** , while parts of the Report a Problem experience require responses on the order of **hundreds of milliseconds** .


Figure 3: Architecture overview of the Saffron service.


## Process early, retrieve fast


In Report a Problem flows, both self-service and live agent-assisted, there’s typically a natural delay between when a customer uploads photos and when the workflow reaches a resolution decision. As soon as photos are uploaded, Saffron starts processing asynchronously and caches the results. When the workflow reaches a decision point (e.g., scam check or resolution determination), Saffron returns cached results quickly. This turns “seconds-long model work” into a “milliseconds-level read” at decision time without blocking the customer.


Figure 4: Customers upload photos on one of the initial screens of the workflow. Our service starts processing the images as soon as they are uploaded.


# Modular prompting: treat LLM like an API


Saffron is modular: a set of small, typed components that each answer one question from customer photos + incident context (product details, customer comment, metadata, etc).


To make those components reliable, we treat model I/O like an interface: required fields are clearly defined, context is standardized, and outputs are constrained to clear types. This keeps downstream integration safe and lets us improve one capability without breaking another.


# Lessons Learned (Shipping GenAI Safely at Scale)


## Tuning precision/recall is harder when there’s no threshold


Even with generative AI (GenAI), deployment is still about managing tradeoffs: how often you’re willing to miss true cases (recall) versus how often you’re willing to be wrong (precision).


In traditional ML, we often tune the precision/recall tradeoff by moving **a decision threshold.** With GenAI, there typically isn’t a single clean scalar score to use. In practice, we tune the tradeoff through a combination of:


- **Prompt wording and instructions** (narrowing definitions to reduce false positives)
- **Schemas and validators** (forcing the model to commit to a constrained set of outcomes, with explicit “undetermined” states)
- **Guardrails and fallback paths** (when uncertain, avoid intervention and defer to safer/default flows)


For customer-impacting workflows, we typically start with **high precision** (lower false positives) and expand coverage over time as we build confidence with offline evaluation, monitoring, and careful rollouts.


## One big model call is tempting, but modular calls scale better


As task complexity grows, bundling many questions into a single large language model (LLM) call can look cheaper on paper, but it couples unrelated decisions and often degrades quality. We had better results treating each use case (damage severity, relevance, resolution viability, monetization potential, etc.) as a **separate, typed module** with its own prompt, schema and evaluation loop.


That modular approach made it easier to iterate safely: we could tighten one prompt to reduce false positives without unintentionally changing downstream behavior, and we could add new use cases without rewriting the entire system.


# Future Roadmap: Real-Time, Video-First Evidence Assessment


Today, Saffron primarily evaluates **static images** , and the system is designed around LLM latency limitations. The next frontier is improving the customer experience by pushing in two directions at once:


- **Faster evaluation** : as models and infrastructure improve, we can drive latency down so feedback is delivered while the customer is still in the upload experience.
- **More flexible inputs (video)** : moving beyond single frames to short clips provides more signal: multiple angles, motion and context. This can improve both accuracy and the kinds of questions we can answer.


Together, faster inference and video unlock **evidence assessment** : real-time guidance on the quality and completeness of what the customer is providing, like:


- “Zoom in on the damaged corner.”
- “Move closer, the damage isn’t visible.”
- “Try another angle to reduce glare.”


The vision: collect higher-quality evidence in the moment, reduce back-and-forth, and streamline the path to the best resolution.


Figure 5: Mockup of interface for AI-based photo feedback.


# Join the Team


This work sits at the intersection of the digital and physical world: real products, real photos and real customer outcomes, as well as real constraints from operations.


If solving real-world challenges with **vision AI** , **LLM platforms** and **customer-facing decision systems** excites you, we’d love to hear from you. Wayfair’s Science and Engineering teams are building the future of customer-first decisioning with AI.
