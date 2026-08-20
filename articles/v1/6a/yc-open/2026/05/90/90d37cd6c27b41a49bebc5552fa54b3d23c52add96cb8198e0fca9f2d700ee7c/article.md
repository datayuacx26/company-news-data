---
schema_version: "1.0.0"
document_id: "90d37cd6c27b41a49bebc5552fa54b3d23c52add96cb8198e0fca9f2d700ee7c"
company_key: "yc-open"
company: "Open"
source_id: "yc-open-news-import-9b3d55ca046e"
canonical_url: "https://www.open.cx/blog/ai-hallucination-examples"
published_at: "2026-05-13T00:00:00+00:00"
first_seen_at: "2026-07-22T07:27:48.081524+00:00"
fetched_at: "2026-07-28T21:44:24.455372+00:00"
content_hash: "sha256:b3d35f78cd70e3c47b3d00b0abbfbc746a7c3b1efdecf38e40fe39db171d95a8"
---

# AI hallucination examples: 12 real cases and what they teach

AI hallucinations are when a language model generates content that sounds right but is factually wrong, invented, or contradicts its source material. Most of them happen quietly. The ones below didn't. They ended up in court filings, customer complaints, news cycles, and one viral poem.


This piece collects 12 real, well-documented hallucination cases from 2024 to 2026, with sources, and pulls out the patterns that matter for anyone deploying AI in production.


## TL;DR


- AI hallucinations are statistically inevitable. Even the best-performing legal AI tools hallucinate 17% to 34% of queries, per Stanford research.
- The most expensive failures aren't the technical ones; they're the deployments that didn't have guardrails or observability to catch hallucinations before users did.
- The 12 cases below span customer service, law, search, news, and creative tools. The failure patterns are consistent across them.
- Defenses that work: source citation, action-only architecture, sampling and review, scope constraints, red-teaming.
- Defenses that don't work: hoping the model gets better, manual review at the end, generic accuracy benchmarks.


## What an AI hallucination actually is


A hallucination is output that's confidently wrong. The model generates text that looks plausible, sounds authoritative, and turns out to be invented or incorrect. The system doesn't know it's wrong; from the model's perspective, the output is just another reasonable next token sequence.


This is different from being unable to answer ("I don't know"). The model that hallucinates is the model that gives you a precise, false answer.


Common triggers:


- The question is outside the model's training data
- The retrieved context is incomplete or contradictory
- The prompt is structured to elicit a specific answer regardless of truth
- The model is trying to fill a structural pattern (e.g., generating five examples when it only knows three)


## The 12 cases


### 1. Air Canada: chatbot invents a bereavement fare refund (2024)


The most-cited customer service hallucination case.[Air Canada's chatbot told a customer he could claim bereavement fares retroactively after purchasing full-price tickets](https://www.envive.ai/post/case-study-of-air-canadas-chatbot) . The policy didn't exist. When the airline refused the refund, a tribunal held Air Canada liable for what its chatbot said.


The legal precedent matters. Companies cannot disclaim responsibility for AI-generated communications. The chatbot represents the company.


### 2. Cursor: AI invents a login policy that doesn't exist (2025)


[Cursor's customer support AI, named "Sam," told users that simultaneous logins were no longer allowed under a new policy](https://www.helpscout.com/blog/ai-curse-of-cursor/) . There was no new policy. There was no Sam. The response was a hallucination, and customers began cancelling subscriptions.


The damage spread fast in the developer community before Cursor could correct the record. The lesson: AI in technical communities gets fact-checked publicly, and brand damage from a hallucination can outpace the response.


### 3. DPD: chatbot writes a poem about how bad the company is (2024)


[DPD's customer service chatbot, after being prompted to "disregard the rules," wrote profane responses and a poem describing the company as "useless"](https://www.cxtoday.com/speech-analytics/dpds-genai-chatbot-swears-and-writes-a-poem-about-how-awful-it-is/) . The customer's screenshot went viral with 1.3 million views before DPD suspended the chatbot.


This isn't a typical hallucination so much as a jailbreak; the user got the model to abandon its instructions. The lesson is similar: AI deployed without robust guardrails will eventually produce content the company can't defend.


### 4. MyPillow lawyers fined $3,000 each for AI-fabricated citations (2025)


[Two attorneys representing MyPillow CEO Mike Lindell in a Colorado defamation case were ordered to pay $3,000 each](https://www.npr.org/2025/07/10/nx-s1-5463512/ai-courts-lawyers-mypillow-fines) after submitting a court filing filled with citations to cases that didn't exist. The AI generated case names and citations that looked legitimate; none could be located in any legal database.


This was one of many similar cases. The[AI Hallucination Cases Database](https://www.damiencharlotin.com/hallucinations/) tracks 486 incidents worldwide, 324 in U.S. courts alone.


### 5. James Martin Paul: hallucinated citations across 8 matters, leading to dismissal (2025)


[Florida attorney James Martin Paul used hallucinated citations across eight different legal matters](https://www.sternekessler.com/news-insights/insights/ai-ip-year-in-reviewai-hallucinations-in-court-filings-and-orders-a-2025-review-of-sanctions-across-the-courts-and-rule-proposals/) , resulting in substantial sanctions including the dismissal without prejudice of four federal matters. The lawyer hadn't verified the AI's outputs before filing.


The pattern: AI tools made it easy to generate large volumes of legal content; the verification work didn't scale at the same pace. The first wave of sanctions in 2024 didn't stop the second wave in 2025.


### 6. Handa & Mallick (Australia): lawyer disciplined for hallucinated family law authorities (2025)


In the Australian case[Handa & Mallick, a solicitor submitted hallucinated authorities generated by AI in a family law matter](https://www.sternekessler.com/news-insights/insights/ai-ip-year-in-reviewai-hallucinations-in-court-filings-and-orders-a-2025-review-of-sanctions-across-the-courts-and-rule-proposals/) . The Victorian Legal Services Board disciplined him, prohibiting him from handling trust money or practicing unsupervised for two years.


The international scope of the problem matters. AI hallucination sanctions aren't a U.S. phenomenon; courts worldwide are seeing the same pattern.


### 7. Stanford study: even purpose-built legal AI hallucinates frequently (2024)


A[Stanford Human-Centered AI study tested purpose-built legal AI tools](https://hai.stanford.edu/news/ai-trial-legal-models-hallucinate-1-out-6-or-more-benchmarking-queries) and found Lexis+ AI and Ask Practical Law AI produced incorrect information more than 17% of the time. Westlaw's AI-Assisted Research hallucinated more than 34% of the time.


These are tools specifically marketed for legal accuracy. The hallucination rate didn't go to zero just because the vendor's pitch said it should.


### 8. Mata v. Avianca: the original ChatGPT legal hallucination case (2023, foundational)


The case that put AI hallucinations in the legal news cycle.[An attorney used ChatGPT to research case law and ended up citing six cases that didn't exist](https://www.reuters.com/legal/transactional/lawyer-who-cited-cases-concocted-by-ai-asks-judge-spare-sanctions-2023-06-08/) . The judge sanctioned the attorney and his firm. Every subsequent legal AI hallucination case has been measured against this one.


Even though this case is from 2023 and not within the 2024-2026 scope of this piece, it's worth including because it set the precedent and is referenced in nearly every subsequent ruling.


### 9. Google Bard: invented a discovery about the James Webb Telescope (2023, foundational)


In Google's launch demo for Bard, the AI[confidently stated that the James Webb Space Telescope took "the very first pictures of a planet outside of our own solar system"](https://www.reuters.com/technology/google-ai-chatbot-bard-offers-inaccurate-information-company-ad-2023-02-08/) . It hadn't. Google's market cap dropped $100 billion the next day.


The lesson for product teams: hallucinations in demos can move markets. The lesson for AI buyers: vendors' demo confidence isn't the same as production reliability.


### 10. Social Security appeal: 12 fabricated cases in one brief (2025)


A lawyer drafting an appeal of a denied Social Security claim[cited 12 cases that were "fabricated, misleading, or unsupported"](https://cronkitenews.azpbs.org/2025/10/28/lawyers-ai-hallucinations-chatgpt/) . The judge described the filing as "replete with citation-related deficiencies, including those consistent with artificial intelligence generated hallucinations."


The pattern in 2025 sanctions cases: the volume of fabricated citations per brief is growing. Early cases had 1 to 3 hallucinated citations; some 2025 cases have a dozen or more.


### 11. Customer service hallucination: ChatGPT-powered support inventing pricing (multiple cases, 2024-2025)


Multiple[reports of AI customer service tools quoting prices or policies that don't exist](https://www.cmswire.com/customer-experience/ai-in-customer-service-billion-dollar-mistake-when-deployed-wrong/) . One common pattern: customer asks about a discount, AI invents a discount code that doesn't work, customer is frustrated. Another: customer asks about a refund timeline, AI states a number it made up, customer waits the wrong amount of time.


These cases mostly don't make the news, but they accumulate as CSAT drops and recontact rates climb. The aggregate cost across deployments is larger than the high-profile cases.


### 12. Microsoft Copilot: election misinformation in 2024 testing


[Researchers found Microsoft's Copilot AI made factual errors in about 30% of responses to election questions tested in 2024](https://www.businessinsider.com/microsoft-copilot-election-questions-misinformation-2024-2) . Wrong dates, wrong candidate information, invented quotes. Microsoft has since tightened guardrails on election-related queries.


The lesson: high-stakes domains (elections, medical, legal, financial) require explicit constraints. Default LLM behavior isn't safe for these contexts.


## Patterns across the cases


Reading through these, the failure patterns are consistent.


### Pattern 1: Confident wrong answers


Hallucinations don't sound uncertain. They sound exactly like correct answers. The model isn't lying; it's pattern-matching, and sometimes the pattern produces invented content.


The mitigation: source citation. If the AI cites its source, the user can verify. If it can't cite a source, the answer is harder to trust.


### Pattern 2: Pressure to produce specific output shapes


Models hallucinate more when prompted to produce a specific shape (5 examples, 12 case citations, a numbered list). They'll fill the shape with invented content rather than admit they only know 3 or 7.


The mitigation: prompt design that allows for variable-length output. "List relevant cases" instead of "list five relevant cases."


### Pattern 3: Out-of-distribution queries


Questions about niche legal cases, obscure customer issues, or recent events the model didn't see during training are higher hallucination risk.


The mitigation: retrieval-augmented generation (RAG) with strong source attribution, and escalation when retrieval confidence is low.


### Pattern 4: Lack of observability


The high-profile cases all share a feature: nobody caught the hallucination internally before it hurt someone. The lawyer filed without checking. The customer service team didn't sample outputs. The product team didn't run red teams before launch.


The mitigation: sampling during deployment (100% for the first weeks, bottom decile by confidence ongoing), red-teaming, and structured QA.


## Defenses that work


What the deployments that don't end up in news cycles do differently.


**Source citation.** Every AI answer cites the source it pulled from. Users can verify. Internal teams can audit. Bad sources are findable.


**Action-only architecture for high-stakes work.** Instead of free-form generation for refunds or policy decisions, the AI takes structured actions through APIs. The action either succeeds or fails; there's no room for invented policy.


**Sampling and review.** AI conversations are sampled (especially low-confidence ones) and reviewed by humans. Patterns of failure get caught fast.


**Scope constraints.** The AI is explicitly told what it can and can't speak to. Out-of-scope queries escalate rather than getting an invented answer.


**Red-teaming.** Before launch, the team deliberately tries to break the AI: jailbreaks, edge cases, adversarial prompts. What breaks gets patched.


## Defenses that don't work


What the failures share.


**Hoping the model gets better.** Hallucination rates have improved with newer models but haven't dropped to zero. They probably won't.


**Manual review at the end.** Reviewing every output before sending is slow and unreliable; humans miss what they didn't expect to look for. Systematic review of samples works better than ad-hoc full review.


**Generic accuracy benchmarks.** A vendor citing "95% accuracy" doesn't tell you what happens on the 5%. The shape of failures matters more than the rate.


**Removing the human from the loop entirely.** Klarna's 2025 reversal is the public example. AI handles routine cases well; complete autonomy without human backstop produces edge-case failures that compound.


## Related guides


If you are deploying AI in production and want to avoid the failures above, these go deeper.


- [What is conversational AI](https://www.open.cx/blog/what-is-conversational-ai) : the capability stack behind modern AI assistants and where hallucinations creep in.
- [AI agents for customer service](https://www.open.cx/blog/ai-agent-customer-service-guide) : how action-taking AI agents resolve tickets, and the guardrails that keep them safe.
- [How to automate customer support with AI](https://www.open.cx/blog/how-to-automate-customer-support-with-ai) : a practical playbook for rolling out AI support with sampling and observability.
- [Generative AI vs. traditional chatbots](https://www.open.cx/blog/generative-ai-vs-traditional-chatbots) : why generative models hallucinate where rule-based bots simply fail closed.


## A final note


AI hallucinations are real, statistically common, and getting documented at increasing rates. They aren't a bug to be fixed by a future model release; they're a property of how language models generate text. The teams deploying AI responsibly assume hallucinations will happen and build the operational discipline to catch them. The teams that don't build that discipline end up in lists like this one.


The 12 cases above are the documented ones. The hallucinations happening quietly in production right now, in support chats and internal tools, are larger in aggregate. The work of deploying AI well is mostly about making them visible and contained.
