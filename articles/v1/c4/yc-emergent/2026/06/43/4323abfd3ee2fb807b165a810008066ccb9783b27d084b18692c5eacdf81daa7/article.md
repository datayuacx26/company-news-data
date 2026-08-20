---
schema_version: "1.0.0"
document_id: "4323abfd3ee2fb807b165a810008066ccb9783b27d084b18692c5eacdf81daa7"
company_key: "yc-emergent"
company: "Emergent"
source_id: "yc-emergent-news-import-16a7bf482038"
canonical_url: "https://emergent.sh/news/claude-fable-5-banned"
published_at: "2026-06-17T02:00:00+00:00"
first_seen_at: "2026-07-21T18:00:01.007987+00:00"
fetched_at: "2026-07-28T21:23:18.688239+00:00"
content_hash: "sha256:cf4fa97b494adadde7b8b42ba2306a0667e99afde3a5c2c1f0d13d1abd44450b"
---

# Claude Fable 5 Banned: Why the US Shut It Down

#### Update, June 27, 2026


On June 27, Commerce Secretary Howard Lutnick[cleared Mythos 5 for redeployment](https://www.cnbc.com/2026/06/26/us-government-anthropic-claude-mythos5-ai.html) to roughly 100 U.S. organizations that defend critical infrastructure, including government agencies and Project Glasswing partners. Anthropic[confirmed on X](https://x.com/AnthropicAI/status/2070665903440871779) that it is working with the government to expand access further and restore Fable 5 for general use.


Fable 5 remains offline for all users as of this writing.[Axios reports](https://www.axios.com/2026/06/27/anthropic-fable-5-return-soon) that the restrictions could be lifted as soon as this coming week, but Pentagon and NSA sign-off is still pending and no date has been confirmed.


Three days. That is how long Claude Fable 5 lasted as a publicly available product.


On June 9, Anthropic[launched](https://www.anthropic.com/news/claude-fable-5-mythos-5) what it called its most capable AI model ever. On June 12, the US Commerce Department[ordered it taken offline](https://www.anthropic.com/news/fable-mythos-access) . The directive barred all foreign nationals, inside or outside the United States, from accessing Fable 5 and its underlying model, Mythos 5. Because Anthropic could not filter access by nationality across its customer contracts, cloud delivery paths, and its own workforce in real time, it[disabled both models](https://www.cnbc.com/2026/06/12/anthropic-disables-access-to-fable-5-and-mythos-5-to-comply-with-government-directive.html) for every user on the planet.


We now have the most dramatic regulatory intervention in commercial AI to date, a precedent for how governments can shut down frontier models with almost no notice, and a growing list of questions for anyone who builds on third-party AI models.


Here is what we know.


## What Claude Fable 5 was


[Claude Fable 5](https://emergent.sh/learn/what-is-claude-fable-5) was not a routine model update. It was the first time Anthropic released a Mythos-class model to the general public. The Mythos tier, previously restricted to vetted cybersecurity partners and select researchers under Anthropic's[Project Glasswing](https://www.anthropic.com/claude/mythos) initiative, represents the company's most powerful AI capability.


The benchmarks reflected that. Fable 5 scored 80.3% on SWE-Bench Pro, roughly 11 points ahead of any competing frontier model, according to results[Anthropic published](https://www.anthropic.com/news/claude-fable-5-mythos-5) alongside the launch.[Artificial Analysis](https://artificialanalysis.ai/articles/claude-fable-5-mythos-intelligence-index) , an independent AI evaluation firm, ranked it first on its Intelligence Index at 64.9, putting Anthropic nearly five points ahead of any other lab's best model.


Cursor CEO Michael Truell said Fable 5 had opened up a class of long-horizon problems that were previously out of reach.


Stripe's early testing showed the model completing a codebase-wide migration across 50 million lines of Ruby in a day, a task the company said would have taken a full team over two months by hand.


The key design decision was a safety fallback mechanism. When Fable 5's automated classifiers detected requests touching cybersecurity, biology, chemistry, or model distillation, it would silently route those queries to Claude Opus 4.8, a less capable model, and notify the user.


Anthropic described this as a "defense in depth" approach, one that acknowledged perfect jailbreak resistance was not currently possible for any provider. On cybersecurity benchmarks, the unrestricted Mythos 5 scored 78.0% on ExploitBench versus Opus 4.8's 40.0%, according to[Vellum's analysis](https://www.vellum.ai/blog/claude-fable-5-and-mythos-5-benchmarks-explained) of Anthropic's launch data. That gap was exactly what the safeguards were meant to contain.


Pricing was set at $10 per million input tokens and $50 per million output tokens. Pro, Max, Team, and Enterprise subscribers received access at no additional cost through June 22.


## How it was taken down


The sequence of events, reconstructed from[Anthropic's official statement](https://www.anthropic.com/news/fable-mythos-access) ,[Fortune's reporting](https://fortune.com/2026/06/14/how-a-warning-from-amazon-led-the-white-house-to-shut-down-anthropics-mythos-model/) , and a public post by White House science adviser David Sacks, goes roughly like this.


### Amazon raises the alarm


On Thursday, June 11, Amazon CEO Andy Jassy raised concerns with senior administration officials. According to multiple media reports[cited by Fortune](https://fortune.com/2026/06/14/how-a-warning-from-amazon-led-the-white-house-to-shut-down-anthropics-mythos-model/) and confirmed by[Axios](https://www.axios.com/2026/06/13/anthropic-amazon-white-house) , Amazon researchers had used a series of prompts to get the Mythos-class model to provide cybersecurity information that was supposed to be restricted. It is unclear whether Amazon conducted these tests at the government's request or independently.


### The standoff between Amodei and the White House


What followed were several calls between Anthropic CEO Dario Amodei and senior administration officials,[according to Axios](https://www.axios.com/2026/06/13/anthropic-amazon-white-house) . Amodei argued the security bypass was narrow rather than a full jailbreak of the model's safeguards. David Sacks offered a sharper version of events in a[post on X](https://x.com/DavidSacks/status/2065853007619588171?s=20) on Saturday. He wrote that a trusted partner had come forward with a working jailbreak, that the administration asked Amodei to fix the vulnerability or pull the model, and that Amodei refused. The export control followed.


### 90 minutes to comply


Anthropic's account differs on key points. In its[official statement](https://www.anthropic.com/news/fable-mythos-access) , the company said it received the directive at 5:21pm ET on June 12, that the letter provided no specific details of the national security concern, and that it was given roughly 90 minutes to comply. A source familiar with Anthropic[told Fortune](https://fortune.com/2026/06/14/how-a-warning-from-amazon-led-the-white-house-to-shut-down-anthropics-mythos-model/) the company received no prior communication of a national security threat. Anthropic wrote that it had reviewed a demonstration of the specific jailbreak technique and validated that the capabilities it unlocked were already available from other publicly available models, including OpenAI's GPT-5.5, sometimes without any bypass at all.


The Commerce Department's directive arrived, and both models went dark globally.


## The two sides of the dispute


This is not a clean story of a company cutting corners or a government acting proportionately. Both sides have made public claims that directly contradict the other.


**Anthropic's position** , laid out in its[official statement](https://www.anthropic.com/news/fable-mythos-access) : the jailbreak disclosed to the company was narrow and non-universal. It uncovered previously known, minor vulnerabilities. The company stood by its defense-in-depth strategy, which combined strong-but-imperfect guardrails with monitoring and a 30-day data retention policy designed to detect and shut down successful attacks quickly. Anthropic argued that applying this standard across the industry would halt all new model deployments for all frontier providers. The company wrote that it believes the government should have the power to block unsafe deployments but through a statutory process that is transparent, fair, and grounded in technical facts, and that this action met none of those criteria.


**The administration's position** , as articulated by David Sacks[on X](https://x.com/DavidSacks) : Anthropic itself had spent years promoting the idea that Mythos was a cyberweapon that needed to be regulated. If there was a vulnerability, big or small, it was the company's responsibility to patch it. He pointed to what he characterized as Anthropic's refusal to cooperate with a reasonable safety request, calling it at odds with the company's own branding as the AI safety company. Sacks wrote that the export control was issued reluctantly and that the administration wants it lifted as soon as Anthropic remediates the issue.


A senior administration official[told Fox Business](https://www.foxbusiness.com/politics/trump-admin-says-anthropics-recklessness-triggered-export-controls-latest-ai-models) that Anthropic could have avoided the drastic step of export controls by engaging more deeply. According to the same report, administration officials said Amodei was at a wellness retreat and could not be reached when concerns were first raised.[Fortune reported](https://fortune.com/2026/06/14/how-a-warning-from-amazon-led-the-white-house-to-shut-down-anthropics-mythos-model/) that Treasury Secretary Scott Bessent told Amodei directly during one call that he was making a bad decision.


AI policy expert Dean Ball, who briefly served in the Trump administration,[described](https://x.com/deanwball/status/2065595735320453298) the action as "simply cartoonish" on X. Ben Murphy, a non-resident fellow at the Institute for Progress, warned it could discourage AI labs from being transparent with the government about their models in the future.


## What this means for builders


Strip away the politics and the personalities, and the Fable 5 ban leaves one operational fact that applies to every startup, freelancer, and small team building on AI: a frontier model you rely on can be turned off overnight, by forces beyond your control or your provider's control.


This is the same lesson the chip industry absorbed in 2020 and semiconductor buyers learned again in 2022. Concentration of supply in critical technology creates hidden fragility. As[Snyk's security team](https://snyk.io/blog/fable-mythos-suspension-security-takeaways/) wrote in their analysis of the suspension, model redundancy is now a resilience requirement, not just a cost or performance consideration.


Treating a single hosted model as a hard dependency is a single point of failure, and single points of failure are a security problem whether they result from an outage, a billing event, a policy change, or a government letter.


For non-technical builders specifically, the takeaway is practical. If you have built workflows, products, or customer-facing tools on a single AI model, now is the time to think about abstraction. Platforms that let you swap models without rewriting your product, that give you fallback options when one provider goes down, are not just convenient. They are infrastructure resilience. Model access is no longer a given. It is a dependency you need to manage.


All other Claude models, including Opus 4.8 and Sonnet 4.6, remain fully operational. Anthropic says it wants to restore Fable 5 access as soon as possible, but no timeline has been given. Sacks wrote that the ball is in Anthropic's court.


For ongoing coverage as this develops, keep an eye on Emergent's[news](https://emergent.sh/news) section.
