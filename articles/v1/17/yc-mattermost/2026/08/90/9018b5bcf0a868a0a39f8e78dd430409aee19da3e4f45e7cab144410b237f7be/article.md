---
schema_version: "1.0.0"
document_id: "9018b5bcf0a868a0a39f8e78dd430409aee19da3e4f45e7cab144410b237f7be"
company_key: "yc-mattermost"
company: "Mattermost"
source_id: "yc-mattermost-rss-3d807a20d23e"
canonical_url: "https://mattermost.com/blog/your-teams-are-already-translating-sensitive-data-the-question-is-where-it-goes/"
published_at: "2026-08-07T13:00:00+00:00"
first_seen_at: "2026-08-07T16:15:39.785658+00:00"
fetched_at: "2026-08-07T16:15:42.353562+00:00"
content_hash: "sha256:663067941701a56efe6e84d770cd0840ea97c5760d244d507e03e0fe303d7c87"
---

# Your Teams Are Already Translating Sensitive Data. The Question Is Where It Goes.

Picture a familiar scenario. An analyst on a joint task force receives a document from a partner organization. It’s written in Korean. She needs to understand it quickly. A decision is pending, a briefing starts in twenty minutes. She doesn’t have a bilingual colleague available. So she does what millions of professionals do every day: copies and pastes it into a third-party translation service, gets the output, and moves on.


The document contained internal personnel details and operational timelines.


She wasn’t reckless. She was busy. And she had no idea if the text she pasted was retained, indexed, or processed by a server outside her organization’s control.


This isn’t hypothetical. It’s a pattern playing out across organizations right now and most security teams still aren’t treating it as the threat it is.


## **The shadow AI problem hiding in plain sight**


Translation is just one corner of a much bigger issue. Every day, employees reach for external AI services to work faster, drafting, summarizing, aggregating content that may be sensitive, regulated, or export-controlled. It’s not malicious. It’s just how people get things done when the approved tools don’t keep up.


The[LayerX Enterprise AI & SaaS Data Security Report 2025](https://thehackernews.com/2025/10/new-research-ai-is-already-1-data.html) found that about 77% of enterprise employees paste data into GenAI tools, and more than half of those paste events include corporate information, with 82% of that activity happening through personal accounts that IT never sees. A separate[ManageEngine survey](https://www.kiteworks.com/cybersecurity-risk-management/employees-sharing-confidential-data-unauthorized-ai-tools/) put the share of employees using unapproved AI tools at 93%, including nearly a third who’ve entered confidential client data.


Call it shadow AI, the same problem as shadow IT, just faster-moving and harder to spot. And according to a[2025 survey of nearly 500 security and compliance professionals](https://www.kiteworks.com/cybersecurity-risk-management/ai-security-gap-2025-organizations-flying-blind/) , 83% of organizations have no automated controls to stop it, and 86% have no visibility into where their data is going when employees use these tools. Most organizations are, in a very literal sense, flying blind.


## **Translation has a breach history worth knowing**


There’s a reason security practitioners keep coming back to one particular incident. Years ago, employees at Statoil, the Norwegian state energy company, used the free web service Translate.com to translate internal documents. Routine stuff. Except the service retained what was submitted, and[sensitive content including passwords, internal memos, and confidential HR plans ended up indexed in public search results](https://www.csoonline.com/article/563519/data-breached-in-translation.html) . Nobody hacked anything. Employees just used a free tool to get their work done.


That was nearly a decade ago. The tools have changed; they’re more capable, more embedded in daily workflows, and far easier to access. The underlying dynamic hasn’t changed at all.


A[2024 Forrester Total Economic Impact™ study commissioned by DeepL](https://tei.forrester.com/go/deepl/AItranslation/?lang=en-us) found that the organizations interviewed had all watched employees rely on free translation services before moving to a governed alternative, and had the compliance incidents to show for it. This wasn’t theoretical risk modeling. It was a lesson already learned the hard way.


Most free translation services are built on a simple premise: you provide the data, they improve the product. That text may train a model, sit on a foreign server, or get transmitted in ways the person submitting it never thought about. For teams working with export-controlled content, regulated data, or anything operationally sensitive, that trade-off isn’t acceptable, whether or not anyone told them to think about it.


## **Banning tools isn’t a strategy**


The instinct when something like this surfaces is to block it. When Samsung discovered engineers had been uploading source code to ChatGPT,[they banned generative AI tools company-wide](https://www.forbes.com/sites/siladityaray/2023/05/02/samsung-bans-chatgpt-and-other-chatbots-for-employees-after-sensitive-code-leak/) . It’s become the canonical cautionary tale. But banning the visible tool doesn’t make the underlying need disappear, it just pushes it somewhere less visible.


The[U.S. Department of Veterans Affairs](https://department.va.gov/ai/guidance-for-generative-ai-use-at-va/) has issued guidance barring staff from submitting sensitive data to tools without an Authority to Operate. Canada’s Translation Bureau flags that[public machine translation tools may store content on servers outside the country](https://bluente.com/blog/google-translate-official-documents) , a meaningful concern for anyone with data residency obligations. These are reasonable positions. They’re just not complete solutions.


When you ban the tool without solving the problem, people find another way. They always do.


## **The operational reality is harder to ignore than it looks**


This isn’t just a compliance headache for enterprise IT. The need to communicate across languages in real time is a front-line operational challenge.


The U.S. Army’s[work on multinational interoperability during exercises like Saber Strike 24](https://www.army.mil/article/283999/mission_partner_kit_advancing_multinational_interoperability_with_nato_allies) , coordinating live operations across American, German, Italian, Spanish and other allied units in Poland, makes the stakes plain. When communication systems don’t work across partner forces, the Army’s own reporting is clear: operational cohesion suffers, movements slow down, and the coordination splinters, unnecessarily risking lives and mission success.


That dynamic isn’t unique to the military. It plays out in every environment where multilingual teams need to move fast: intelligence sharing, crisis response, cross-border financial operations. The answer isn’t to accept the friction or to hand the problem back to individual users. It’s to build translation into the governed environment where the work is already happening.


## **What “governed” actually looks like**


The gap here isn’t technical, it’s architectural. Consumer translation tools are built for convenience. They weren’t designed to respect access controls, maintain audit trails, or keep data inside an organization’s security perimeter. Asking employees to use them responsibly is asking them to understand an architecture they have no insight into or control over while in the middle of getting something done. That’s not realistic.


The alternative is translation that works the same way as the rest of the security infrastructure: running on systems the organization controls, with the organization choosing the engine. That might be a self-hosted open-source model for maximum data sovereignty, a commercial AI service operating under a proper data processing agreement, or a custom model fine-tuned on mission-specific terminology, the kind of domain knowledge a general-purpose LLM simply won’t have.


When translation is built into the collaboration environment rather than bolted on from outside, the dynamic changes entirely. Engineers in one country write in English; their counterparts write in Korean; everyone reads in their own language in real time. Nobody leaves the governed platform. Nobody has to remember a policy. The shadow channel disappears because the need for it does.


## **The real question for security teams**


The organizations most exposed to translation-driven data loss usually aren’t the ones with careless employees. They’re the ones that locked down the obvious tools without addressing their teams’ legitimate needs and their teams quietly found other ways to get their mission-critical work done.


Security teams who get ahead of this become enablers, not blockers. They don’t just say no to ChatGPT. They ask what the workflow actually requires and make sure there’s a governed way to meet it. That’s the posture that reduces risk without grinding operations to a halt.


Your people need to work across languages. That’s not changing. The only question is whether it happens inside a system you control or in a browser tab you’ll never know about.


We recently shipped exactly this. Mattermost Enterprise Advanced now supports automatic channel translation natively inside the platform messages are automatically translated into each user’s preferred language, and nothing leaves your environment. You choose the translation engine: LibreTranslate for a fully self-hosted, open-source option, or the Mattermost Agents plugin backed by the LLM of your choice. We built it because we kept hearing the same thing from customers operating across languages: the approved options were too slow, too expensive, or didn’t exist, so people were improvising. This closes that gap.


If that’s a problem you’re trying to solve,[it’s worth a look](https://mattermost.com/demo) .


***Your data. Your translation engine. Your destiny.***


**See how Mattermost enables secure multilingual collaboration** →[Request a Demo · mattermost.com/demo](https://mattermost.com/demo)


## Read More Collaboration Articles


## Open source news, right in your inbox


## Thanks for subscribing!
