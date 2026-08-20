---
schema_version: "1.0.0"
document_id: "9de385d9017d380c65695b35bd8aa89a21f495c65203636e2b0f67f69343198b"
company_key: "yc-didit"
company: "Didit"
source_id: "yc-didit-news-import-d47711ec305c"
canonical_url: "https://didit.me/blog/llm-distillation-defense-identity/"
published_at: "2026-08-04T14:51:59.780+00:00"
first_seen_at: "2026-08-04T18:58:29.118711+00:00"
fetched_at: "2026-08-04T20:28:10.240209+00:00"
content_hash: "sha256:35c2394043232c7fab0dcf0e7a601205f49eb743c078810134be30bd65ceb683"
---

# The Hydra Account Problem: Why Distillation Defense Starts With Identity Resolution

[Back to blog](https://didit.me/blog/) Blog · August 4, 2026


# The Hydra Account Problem: Why Distillation Defense Starts With Identity Resolution


Anthropic reported 16 million exchanges through roughly 24,000 fraudulent accounts, and one proxy network running more than 20,000 accounts at once. Distillation is not a prompt problem. It is an access problem.


By Didit


·


August 4, 2026 ·


Updated Aug 4, 2026


On 23 February 2026, Anthropic published measured numbers on something the AI industry had until then discussed mostly in the abstract. In identified distillation campaigns, the company reported that labs "generated over 16 million exchanges with Claude through approximately 24,000 fraudulent accounts." One detail in that report reframes the entire problem: "a single proxy network managed more than 20,000 fraudulent accounts simultaneously."


Twenty thousand accounts. One actor.


That is the shape of the threat. Adversarial distillation — using a frontier model's outputs to train a cheaper copy of it — is not executed by one suspicious account making one suspicious request. It is executed by an organisation that spreads a single extraction campaign across thousands of accounts, payment instruments, devices, and network paths, so that every individual account looks unremarkable. Reviewed one at a time, none of them trips anything. Reviewed as a graph, they are one machine.


This is why the defense cannot live only in the model or only in the traffic. It also has to answer a question those layers cannot: **who is actually behind these accounts?**


## **Key takeaways**


- Anthropic reported **over 16 million exchanges** generated through **approximately 24,000 fraudulent accounts** in identified distillation campaigns, with a single proxy network running **more than 20,000 accounts simultaneously** .
- Distillation is a **coordinated access problem** , not a single-request problem. Per-account review is structurally blind to it.
- The Frontier Model Forum's February 2026 issue brief defines adversarial distillation and its methods. By design it is scoped to the threat rather than to controls, so it makes **no recommendation on account verification, identity, rate limiting, or access control** .
- Anthropic's own listed mitigations include **"strengthened verification for educational and startup accounts"** alongside detection classifiers and behavioral fingerprinting.
- A verified-access layer does three things model controls cannot: it **reduces anonymity** , it **links accounts that share a person, device, or network** , and it **makes a regenerated account die on arrival** .
- **Identity verification alone does not prevent model extraction.** It is one layer of three, and it does not replace the other two.


## **What actually happened**


Anthropic's report is worth reading in full, but three findings matter most for anyone designing access controls.


**The volume was concentrated in a few actors.** Anthropic attributed "over 13 million" exchanges to MiniMax, "over 3.4 million" to Moonshot AI, and "over 150,000" to DeepSeek. These are not opportunistic scrapes. They are sustained industrial programmes.


**The accounts were coordinated, and the coordination was visible in metadata.** Anthropic described "identical patterns, shared payment methods, and coordinated timing" that "suggested 'load balancing'" across accounts. Attribution came from "request metadata and infrastructure indicators" — in one case, request metadata that "matched the public profiles of senior Moonshot staff."


**The prompts repeated at absurd scale.** Anthropic noted prompt variations arriving "tens of thousands of times across hundreds of coordinated accounts."


Read those three together and a pattern emerges. Almost every signal that exposed these campaigns was a *relational* signal — something shared across accounts. Shared payment methods. Shared timing. Shared infrastructure. Shared prompt templates. Not one of them is visible if your unit of analysis is a single account.


## **Why per-account review fails**


Most abuse tooling is built around a per-account verdict. An account signs up, gets scored, gets a risk band, and gets a decision. That model works well for the abuse it was designed for — a fraudster using a stolen card, a spammer blasting a channel.


It fails against distillation for a specific structural reason: **the attacker controls how much of the campaign any single account carries.**


If your review threshold is "an account making 50,000 unusual requests," the attacker uses 20,000 accounts making 800 requests each. If your threshold drops, they add accounts. The economics favour them because accounts are the cheapest input in the system. A frontier model API call has real marginal cost; an email address does not.


So the attacker's optimisation problem is simple: keep per-account behavior under whatever the per-account threshold is, and scale horizontally. And it works, because the defense is measuring the wrong object. It is measuring accounts when the adversary is an actor.


This is the hydra property. Cut off one account and two more appear, because the thing that generates accounts was never touched. A proxy network running more than 20,000 accounts simultaneously, as Anthropic reported, is the pure expression of it — at that scale, removing accounts one at a time is not a defense, it is a maintenance task.


To beat a hydra you have to stop cutting heads and start finding the body.


## **The three layers of a distillation defense**


A serious defense has three layers, and they are owned differently.


Layer Owned by Role


**Model controls** The model provider Limit sensitive reasoning traces, shape outputs, protect model behavior


**Traffic detection** The model provider, or a research/vendor stack Detect repetitive, semantically concentrated, or coordinated extraction patterns


**Verified access** Identity infrastructure Resolve who is behind an account, link shared identifiers, enforce policy across everything connected to a confirmed abuser


The first two layers are well understood and actively researched. On the traffic side, recent academic work is genuinely encouraging: Liu, Guo and Dong ([arXiv 2606.05725](https://arxiv.org/abs/2606.05725) , June 2026) frame extraction monitoring as benign-calibrated traffic-window distribution testing, using maximum mean discrepancy in semantic space calibrated only on benign traffic. Across fourteen attacker-normal query pairs from four extraction scenarios, they report a 100% detection rate for pure-attacker cases at a 0.3% false-positive rate on benign queries.


That is a strong result, and it is worth being clear about what it means: **traffic detection works, and it is not the layer Didit is talking about.** Semantic analysis of prompt distributions is the model provider's job, and it should stay there.


The third layer is the one with the least published design guidance — including in the industry's own canonical brief on the threat.


## **What the canonical brief covers**


The Frontier Model Forum published its[adversarial distillation issue brief](https://www.frontiermodelforum.org/issue-briefs/issue-brief-adversarial-distillation/) on the same day as Anthropic's report. It is a good document. It defines adversarial distillation as covertly accessing a model's outputs — typically in violation of terms of service — to train a secondary model that replicates the teacher's capabilities *while bypassing its safety training* . It catalogues the methods: chain-of-thought exfiltration, chain-of-thought critiquing, chain-of-thought autograding, prompt generation for reinforcement learning, and synthetic data generation. It names the target capabilities: mathematical and scientific reasoning, agentic coding, multimodal processing, general reasoning.


It also acknowledges the real tension in this space — "addressing these security concerns without impeding legitimate research."


What it does not contain is any recommendation on account verification, identity resolution, rate limiting, or access control.


That is a scope choice, and a sensible one: the brief sets out to define the threat and its methods, not to prescribe controls. Access-layer design is simply a different document.


It is a document the industry has not really written yet, though — while the lab that measured the threat listed "strengthened verification for educational and startup accounts" among the mitigations it invests in, right alongside detection classifiers and behavioral fingerprinting. The access layer is doing real work in practice and has very little published design guidance behind it. That is what the rest of this post is about.


## **What the verified-access layer actually does**


Be precise about the claim here, because overclaiming is how this whole category loses credibility.


Identity verification does not detect distillation. It cannot see prompt semantics, it does not know which capability is being targeted, and it will never tell you that a request stream looks like chain-of-thought exfiltration. That is the traffic layer's job.


What identity does is change the attacker's cost structure in four specific ways.


**It reduces anonymity.** An account bound to a verified person or a registered company is an account with an attributable owner. That does not stop abuse, but it changes what abuse costs and what it risks.


**It links accounts that share something physical.** Two accounts registered by the same face, the same device, or the same network are related whether or not their behavior looks related. This is the signal per-account review cannot produce, and it is exactly the class of signal — shared payment method, shared timing, shared infrastructure — that surfaced the campaigns Anthropic described.


**It makes regeneration expensive.** The hydra property depends on new accounts being cheap and disconnected from old ones. When a confirmed abuse case propagates to every identifier it touched — face, device, IP range, email, phone — the next account created off the same infrastructure fails at the door instead of being caught 800 requests later.


**It keeps friction off legitimate developers.** This is the constraint that makes the whole thing viable. Verification applied to everyone is a growth tax. Verification applied adaptively — by access tier, quota, credit volume, geography, or a behavioral alert from the traffic layer — puts the friction on the risk and leaves the trusted developer path fast.


Four things. None of them is "prevents extraction." All four are real.


## **Where Didit fits**


Didit is infrastructure for identity and fraud, and the primitives that compose into a verified-access layer are ones we already ship and publish prices for:


- **Identity verification** — ID document, passive liveness, face match and IP analysis, bundled at **$0.33** per verification, with 500 free KYC verifications per month.
- **Face Search 1:N** — biometric search across the verified users your own application has enrolled, **free** with verification. This is the primitive that links two accounts to one person.
- **IP & Device Analysis** — $0.03, and included in the $0.33 bundle. Emits` DUPLICATED_IP_ADDRESS` ,` DUPLICATED_DEVICE_FINGERPRINT` ,` DEVICE_RECOVERED_HIGH_CONFIDENCE` ,` AUTOMATION_FRAMEWORK_DETECTED` and related codes.
- **Lists API** — blocklists across 12 entry types, where blocklisting from one confirmed session auto-extracts the identifiers that session captured — face, document, phone, email, IP and device.
- **Biometric Authentication** — $0.10 passwordless re-verification, for binding a privileged action to the human who onboarded rather than to a bearer token.
- **Business Verification** — from **$2.00** per company: registry lookup, ultimate beneficial owners and officers, with entity screening at $0.20 and any linked identity check for a beneficial owner billed at standard User Verification rates. For organisation-tier and research-tier access.


You compose those into whatever policy your platform needs. The composition is your architecture; the primitives are the parts. Every one is delivered through the unified` /v3/` API with a published price and no minimum — some as their own endpoint, like` POST /v3/face-search/` , and some through a session workflow, like biometric authentication, which has no standalone endpoint of its own.


## **Use cases**


**Frontier model providers.** Bind high-quota, high-credit and research-tier access to a verified person or company, while leaving the free and low-volume tiers frictionless.


**AI API platforms and inference providers.** Resellers and aggregators inherit the abuse without inheriting the detection stack. An access layer is often the only control they realistically own.


**AI coding and agent products.** Trial abuse and credit farming use the same account-multiplication mechanics as distillation. The same linking primitives address both.


**Cloud AI platforms and marketplaces.** Organisation-level verification answers "is this a real company with real beneficial owners" before an enterprise quota is granted.


## **Frequently asked questions**


**Does identity verification prevent model distillation?**


No. Extraction is prevented — to the extent it can be — by model-level output controls and by semantic traffic detection. Identity reduces anonymity, links accounts across a campaign, and makes regenerated accounts fail early. It is one layer of three.


**Won't verification drive away legitimate developers?**


Only if you apply it to everyone. The design that works is adaptive: verify on risk, tier, quota, credit volume, geography, or a behavioral alert. Most developers should never see a verification step. Didit's Reusable KYC is free, so a developer already verified elsewhere on the network can clear a check without redoing it.


**Can Didit tell me a request stream looks like distillation?**


No. Didit does not see your prompts and does not analyse request semantics. That signal comes from your own traffic layer. What Didit provides is the identity resolution to attach that alert to an actor, and the enforcement primitives to act across every account connected to them.


**What does this cost at the scale of an AI platform?**


Verification is priced per successful check with no minimums: $0.33 for the full identity bundle, $0.03 for IP and device analysis on its own, free for Face Search 1:N, $0.10 for biometric re-authentication, and from $2.00 for business verification. Because the policy is adaptive, you pay on the fraction of access that actually warrants a check. The first 500 KYC verifications each month are free.


**We already have a fraud vendor. Why is this different?**


Most fraud tooling returns a per-account verdict. The distillation problem is a per-actor problem, and the primitives that link accounts to one actor — 1:N biometric search across your own users, device and IP correlation, and blocklist propagation across every identifier from a single confirmed case — are the specific things per-account scoring does not do.


## **Ready to get started?**


Start with the primitive that produces the linking signal you are missing today.


- **Read the docs** —[Face Search 1:N](https://docs.didit.me/core-technology/face-search/overview) ,[IP & Device Analysis warnings](https://docs.didit.me/core-technology/ip-analysis/warnings-ip-analysis) , and the[Lists API](https://docs.didit.me/management-api/lists/overview) .
- **See the product** —[User Verification](https://didit.me/products/user-verification) and[Business Verification](https://didit.me/products/business-verification) .
- **Check the pricing** — every module is[publicly priced](https://didit.me/pricing) , pay-per-success, no minimums.
- **Start free** — create an account at[business.didit.me](https://business.didit.me/) and run your first 500 KYC verifications a month at no cost.


Keep reading


## Related articles


- [Business Verification for AI API Access: Who Actually Controls This Account?](https://didit.me/blog/kyb-ai-api-enterprise-access/)
- [Verified API Access for AI Model Providers: A Risk-Tiered Architecture](https://didit.me/blog/verified-api-access-ai-model-providers/)
- [Face Search 1:N: Finding Every Account One Person Controls](https://didit.me/blog/face-search-duplicate-account-detection/)
- [Biometric Step-Up for AI API Access: Binding Privilege to a Person](https://didit.me/blog/biometric-authentication-ai-api-access/)
- [Hydra Account Networks: How 20,000 Accounts Become One Actor](https://didit.me/blog/hydra-account-networks-ai-api-abuse/)
- [Blocklist Propagation: Making One Confirmed Abuse Case Kill the Whole Network](https://didit.me/blog/ai-api-abuse-blocklist-propagation/)
