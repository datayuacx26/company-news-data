---
schema_version: "1.0.0"
document_id: "08dab121ec4ea20935b0e9e6d3f9f988c16eda5866eb6d8c564857e29895ff96"
company_key: "yc-zapier"
company: "Zapier"
source_id: "yc-zapier-rss-44f77ec59be6"
canonical_url: "https://zapier.com/blog/autotorino-lead-intake-inbound-call-automation"
published_at: "2026-08-08T00:00:00+00:00"
first_seen_at: "2026-08-12T20:37:25.219063+00:00"
fetched_at: "2026-08-12T20:37:26.560403+00:00"
content_hash: "sha256:83692aa5bcf1a7b900706edceafd69709d24ccb69aba6fea3aa7b97c2392abf7"
---

# How Autotorino automated lead intake and inbound call capture with Zapier and Salesforce

[Autotorino](https://www.autotorino.it/) manages sales and after-sales across 74 locations and 23 brands, with customer requests arriving through web forms, lead-gen tools, email, and a high volume of phone calls. The challenge was routing: getting every interaction into the right system, in the right shape, fast enough to matter.


That meant solving two problems at once. The first was[digital lead flow](https://zapier.com/solutions/lead-management) : routing leads from many sources into[Salesforce](https://zapier.com/apps/salesforce/integrations) without asking people to spend their days copying, pasting, and checking records. The second was inbound call volume. As the business grew, Autotorino was handling roughly 100,000 inbound calls a month — more than any team could realistically cover with people alone.


Autotorino built the fix in two layers.[Riccardo Berta](https://www.linkedin.com/in/riccardo-berta-bb965963/) , in CRM and digital, used Zapier to connect lead sources and make Salesforce the system of record.[Gabriele Luciani](https://www.linkedin.com/in/gabrieleluciani/) and[Michele Spalla](https://www.linkedin.com/in/michele-spalla-6777bbb6/) , on the innovation team, extended that foundation into voice workflows, pairing Zapier with AI to catch the calls human teams couldn't reach.


## Riccardo's build returned 2,394 hours to the CRM team


Before the build, lead handling was manual. An inquiry might arrive through a brand landing page, a form, a lead ad, or email. Someone then keyed it into Salesforce, matched it to the right process, and moved to the next one. Necessary work, but the kind that drains a team: slow, error-prone, and a poor use of skilled people who should be talking to customers.


Riccardo rebuilt that layer as production workflows in Zapier. Instead of forcing very different lead types through one oversized process, the team built separate workflows for separate triggers. Typeform submissions,[Facebook Lead Ads](https://zapier.com/apps/facebook-lead-ads/integrations) , Outlook-based flows, and brand-specific landing pages each got their own path into Salesforce. On the heavier flows, AI enriched or classified the incoming data before the final record was written.


Each lead source got the handling it needed, and Salesforce stayed clean on the other side. 109 Salesforce-writing Zaps now run across the business, each built around a specific trigger or process. In 2025, Autotorino calculated the automation returned 2,394 hours to the CRM team and removed a whole class of copy-and-paste errors from the system of record. That kind of data integrity is what makes Salesforce a reliable foundation as teams move toward AI-powered workflows: the models are only as good as the records underneath them.


> "With the introduction of Zapier, we completely eliminated this workload, allowing our Customer Service team to focus on higher-value activities. Automation also removed the risk of human error, like simple copy/paste mistakes." — Riccardo Berta, CRM & Digital, Autotorino


## Gabriele's team captured showroom call demand it couldn't afford to miss


Digital lead flow was only half the problem. The bigger operational load sat on the phone line.


Autotorino was managing around 100,000 inbound calls a month across its dealership network. Those calls came into numbers assigned to individual showrooms, so the problem started at the showroom before it ever became a network-wide one. The business already ran a multi-step handling model across dealerships, a business development center, and external call centers. Even so, it still couldn't answer every call the way it wanted to, and hiring more people into the same process wasn't the answer.


That's where the innovation team stepped in. Gabriele and Michele started with a smaller, controlled version of the problem: an early pilot that used Zapier to turn recorded messages into Salesforce leads across a group of dealerships. The pilot now handles roughly 1,500 client requests a month — calls that used to go unanswered. Gabriele estimates the recovered after-sales demand at roughly €150,000 in net profit influenced.


From there, the team went further, building a more interactive workflow that combined Zapier with ElevenLabs voice technology. The stack varied by use case, but the pattern held: capture the customer interaction, process it through AI steps, filter and format the output through Zapier, then write the result into Salesforce so the business could act on it. In one production workflow, the architecture ran through AssemblyAI, OpenAI, Zapier, and Salesforce: recorded audio was transcribed, requests were classified against Autotorino's internal framework, and the structured output was routed into the CRM. When the team hit accuracy limits, they refined prompts, split the AI step into smaller parts, and tuned until the workflow produced data the business could trust.


> "The pilot showed us something simple: calls we thought were lost could actually be captured, structured, and put in front of the sales team automatically. Once that was proven, expanding with Zapier and AI wasn't a bet. It was the obvious next step to protect demand we couldn't afford to keep missing." — Gabriele Luciani, Innovation Project Manager, Autotorino


## Automation gave Autotorino safer scale and new visibility


Plenty of companies use AI. What's telling about Autotorino is how deliberately the team chose where to use Zapier, where to add AI, and where to keep human judgment in the loop.


Leadership was enthusiastic and department leaders were pushing for progress, and the team treated that as a reason to build responsibly rather than a license to roll tools out carelessly. That showed up two ways.


First, Zapier ran as a controlled orchestration layer inside specific business workflows. The team focused on known operational problems where automation could be measured, so the system scaled without becoming a free-for-all. Salesforce provided the trusted customer intelligence; Zapier kept it clean, connected, and flowing everywhere it needed to go. Together they gave Autotorino the data discipline that prepares their Salesforce environment for what's coming next in agentic AI.


Second, the work created visibility the business didn't have before. Once call handling and lead capture moved through structured workflows, Autotorino could see why people were calling, which showrooms were generating missed demand, and where follow-up was getting lost. Under the old manual process, most of that signal disappeared. Under the new one, it became something the business could analyze.


The goal was to move people off repetitive intake and toward higher-value work — less manual entry for the CRM team, and automation catching the calls humans couldn't reasonably cover, so employees could focus where judgment matters most.


## Zapier connects Autotorino's systems, teams, and AI workflows


Autotorino didn't treat automation as an isolated tool for one department. It used Zapier to connect customer demand, AI processing, and Salesforce execution into one system. For Riccardo's team, that meant turning fragmented lead sources into clean CRM records. For Gabriele and Michele, it meant giving voice workflows a reliable path into the business. For leadership, it meant pursuing AI use cases without losing operational discipline.


Autotorino took processes that were too manual, too error-prone, or too large to handle consistently, and rebuilt them into systems that keep up with the business.


## What's next


Autotorino is now building an outbound-calling workflow designed to follow up with customers after brand communications and help book the next interaction automatically. If the inbound workflow proved Zapier could help Autotorino capture showroom demand more reliably, the outbound project tests whether the same approach can make follow-up proactive instead of reactive.
