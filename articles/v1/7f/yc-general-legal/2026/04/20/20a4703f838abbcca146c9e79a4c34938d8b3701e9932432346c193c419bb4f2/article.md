---
schema_version: "1.0.0"
document_id: "20a4703f838abbcca146c9e79a4c34938d8b3701e9932432346c193c419bb4f2"
company_key: "yc-general-legal"
company: "General Legal"
source_id: "yc-general-legal-news-import-cc42147fa719"
canonical_url: "https://general.legal/blog/which-privacy-enhancing-technology-to-use"
published_at: "2026-04-28T00:00:00+00:00"
first_seen_at: "2026-07-29T04:44:54.246082+00:00"
fetched_at: "2026-07-29T04:44:55.619052+00:00"
content_hash: "sha256:7dca4550d49b26712dd510b5fc0e9d02fb9f83e9d49b2a62c2e26b430a75b80e"
---

# So Which Privacy-Enhancing Technology Should You Use?

**Part 6 (final) of our series on Privacy Enhancing Technologies**


Five posts in, here’s the honest summary: there is no single best privacy enhancing technology. Each one solves a different problem, costs a different amount, and needs a different level of engineering sophistication. That’s not a cop-out. It’s the actual, useful answer, and this post is about making it concrete.


If you remember nothing else from this series, remember this: the right question is never “which PET is best?” It’s “what am I actually trying to protect, and what can my team realistically build?”


# Quick Recap: What Each One Actually Needs


**Technology** **What it needs from you** **Who it needs to trust**


Anonymization / pseudonymization (Part 1) Basic process discipline, no special engineering Nothing exotic; this is table stakes


Differential privacy (Part 2) A real engineering pipeline, ongoing Math, but someone has to set the parameters correctly


Synthetic data (Part 3) Generation plus leakage testing (the testing is not optional) Whoever built and tested the generator


Federated learning (Part 4) Multiple cooperating institutions, each with their own infrastructure Every participating institution


MPC (Part 5) Multiple cooperating institutions agreeing on one exact question Math, split across all parties


FHE (Part 5) Specialist cryptography talent Math, all in one place


TEE (Part 5) Off-the-shelf cloud product, no custom cryptography The chip manufacturer’s hardware


## If You’re Not a Tech Giant: Start Here


### Start with anonymization and pseudonymization


Step one, always, no exceptions:[Part 1’s](https://general.legal/blog/gdpr-anonymization) anonymization basics. If you don’t know whether your “anonymized” data would actually hold up under GDPR versus HIPAA versus CCPA, nothing else in this series matters yet. This costs no special engineering. It’s a process and a legal-standards question, and it’s where every single client conversation should start.


### Consider synthetic data as an achievable advanced option


The most achievable “advanced” option today: synthetic data ([Part 3](https://general.legal/blog/synthetic-data) ), specifically because of something that’s changed recently. Most companies now have an enterprise LLM sitting on their desk, which makes generation itself genuinely easy. The catch, which we said plainly in that post, is that generation is only half the job. Skipping the leakage-testing half is how “privacy-preserving” quietly becomes privacy theater with better marketing.


### Use TEEs when you need compute isolation


If you just need compute isolation, not new cryptography: TEEs ([Part 5](https://general.legal/blog/computing-on-data-without-seeing-it) ). This is the cheapest and fastest of the three frontier technologies, and it’s already a standard product from every major cloud provider. You don’t need to invent anything; you’re renting a security property that already exists.


## When You May Need a Specialist or Partner Institution


If none of the above fits, you probably need a specialist, or a partner institution:


### When should you use differential privacy?


Need a mathematical guarantee, not just good practice? Differential privacy ([Part 2](https://general.legal/blog/differential-privacy) ), but budget for real engineering, not a checkbox.


### When should you use MPC?


Need to collaborate with another organization that doesn’t trust you with their raw data? MPC (Part 5), but you need that other organization on board too, doing real work on their end.


### When should you use FHE?


Need the absolute strongest guarantee, no matter the cost? FHE (Part 5), but this needs cryptography specialists most companies don’t have in-house.


### When should you use federated learning?


Are you actually a hospital network, research consortium, or similarly resourced multi-institution group? Federated learning ([Part 4](https://general.legal/blog/federated-learning) ) might be worth the infrastructure investment. If you’re a single company, it almost certainly isn’t yet.


## The One Idea Worth Keeping


Every technology in this series trades off privacy strength against cost, speed, and complexity. None of them is free, and none of them is magic. The companies that get burned aren’t the ones that pick the “wrong” PET. They’re the ones that assume a PET solves the problem completely, stop asking questions, and skip the testing and legal review that turns a promising technology into an actual, defensible privacy practice.


That’s the whole series in one sentence: know what you’re actually protecting, know what your team can actually build, and don’t mistake the name of a technology for a guarantee.


## Get Help Choosing a Privacy-Enhancing Technology


If you want help figuring out where your company actually falls on this list, that’s what we’re here for.[General Legal](https://general.legal/) has attorneys who work on data privacy and AI governance day-to-day, and we’re glad to talk through your specific situation.


*This post is for general informational purposes and does not constitute legal advice. Which of these technologies, if any, satisfies your specific regulatory obligations depends entirely on your data, your architecture, and your jurisdiction. Talk to us before relying on any of them as a compliance answer.*
