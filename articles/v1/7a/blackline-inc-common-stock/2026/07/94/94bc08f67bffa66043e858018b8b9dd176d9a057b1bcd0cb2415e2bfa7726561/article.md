---
schema_version: "1.0.0"
document_id: "94bc08f67bffa66043e858018b8b9dd176d9a057b1bcd0cb2415e2bfa7726561"
company_key: "blackline-inc-common-stock"
company: "BlackLine Inc."
source_id: "blackline-inc-common-stock-news-import-2d1fe8859cfb"
canonical_url: "https://www.blackline.com/blog/model-complexity-and-uncertainty-is-opportunity/"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-07-31T18:32:00.544365+00:00"
fetched_at: "2026-07-31T18:32:01.019641+00:00"
content_hash: "sha256:f187ceadaafc0c733d3d9df1437f918179b0c7d520f0ce11864a3530e8df37cd"
---

# Model Complexity and Uncertainty is Opportunity

#### BlackLine Blog


[Blog Home](https://www.blackline.com/blog/)


July 30, 2026


# Model Complexity and Uncertainty is Opportunity


Industry Priorities & Trends


Finance & Accounting Technology


4 Minute Read


JU


Jeremy Ung


Chief Technology Officer


Share Article


A year ago, most companies used one or two AI providers. Today they juggle many, and that shift is changing how businesses buy and trust AI.


More than a third of enterprises now run five or more AI models at once, and they're doing it on purpose. Depending on a single AI vendor has become a real risk: most executives worry about lock-in, and nearly half say losing their main provider would break something they rely on. For them, keeping options open is now simply part of how you buy AI.


## Why the AI Market Got So Unpredictable


Three things happened in the first half of 2026.


-


**Cheap challengers arrived:** A wave of Chinese-developed AI models (many of them "open-weight," meaning any company can download and run them on its own machines) caught up fast while costing 60 to 90 percent less than the leading American models. Businesses noticed. On OpenRouter, a service that lets developers tap dozens of different AI models through a single connection, the share of usage going to Chinese-developed models climbed from about 4.5 percent in early 2025 to a weekly peak of 46 percent a year later, a tenfold jump.


-


**Their quality caught up:** This isn't only about price. On independent quality tests, a recently released, freely available Chinese-developed model (Kimi K3) now ranks among the best in the world, and comes first on a widely-used public coding leaderboard (LMArena's Frontend Code Arena). A handful of others sit just behind the American leaders. Europe's main entry trails on quality and competes instead on trust, licensing, and price.


-


**Prices split in two:** The striking part is at the top of the range: near-top-tier quality now costs a tiny fraction of what the leaders charge. Some capable models cost less than a dollar to do the same amount of work the leaders bill $30 to $50 for. And the leaders went the other way: rather than cut prices, they raised them on their best models and moved their advantage into other services. On top of that, in June the US government ordered a top American model taken offline for 18 days — the first time a government has switched off a live AI system.


Figure 1. Quality versus price, July 2026. Up means more capable, left means cheaper, so the best value sits top-left. Open-weight models (most of them Chinese-developed) now reach near-top quality far below the price of the closed leaders. Some open-weight prices are estimated. Sources: Artificial Analysis (quality); published vendor prices.


Put those together and three things a CFO cares about are all moving at once: cost, quality, and even whether a model will still be available next month.


## How Fast the Newcomers Caught Up


The gap didn't shrink gradually; it closed in a single quarter. As recently as April, US government testing put the best freely available model about eight months behind the leaders. By mid-July, that gap had narrowed to a matter of weeks. In the chart below, each dot is a model release; the paid-closed and free-open trends have nearly converged.


Figure 2. Every model release over time, colored by closed (paid) versus open-weight (free); the dotted lines are each group's average trend. Both climb, and the open-weight trend is closing fast on the closed one. Recent points are measured; earlier ones are estimated to show the trend.


For a business, the month's winner barely matters. What matters is that the best choice keeps changing, so the smart move is to stay flexible: run several models and switch as price and quality shift. But that flexibility comes with a catch.


## The Accountability Problem


Companies are beginning to hand routine finance work to AI agents. These don't just answer questions; they execute tasks: matching payments, closing the monthly books, flagging unusual entries.


This is the shift to


[agentic AI](https://www.blackline.com/agentic-financial-operations/)


, systems that act on their own within the limits you set. As those agents multiply, each running on a different underlying model, basic accountability gets harder: knowing which model executed a given reconciliation, what the close actually cost, and whether the audit trail survives when a cheaper model is swapped in next quarter.


That last one carries real weight for a public company. The


[Sarbanes-Oxley Act (SOX)](https://www.blackline.com/resources/glossaries/sox-compliance/)


requires firms to prove their financial reporting is properly controlled, which makes an unbroken, traceable record a legal requirement.


Running many models is the right architecture. Without something built to govern it, it is unmanageable.


## We Have Seen This Before


When companies first spread their work across several cloud providers such as Amazon, Microsoft, and Google, no single tool could tell them whether the whole setup was safe and above board.


The winners turned out to be independent cloud security vendors that could monitor every provider at once, precisely because they weren't owned by any of them. One such independent company, Wiz, was bought for $32 billion on exactly that logic.


Finance is at the same point now. There are many AI agents, and many different AI models beneath them, and the vendors can't referee themselves. An AI provider can tell you what its own model did, after the fact. Your accounting-software vendor can watch its own agents. Neither can watch all of them, or answer the live question: is this AI following our rules right now?


## What We're Building


The answer is an independent oversight layer that doesn't care which vendor built the agent or which AI model it runs on. That is what we are building with Control Console. It comes down to three things.


-


**Stop problems before they happen, not after:** The rules are enforced before an agent acts. If an agent tries to post an entry above an approved limit, it pauses and hands off to a person, rather than doing it and getting flagged later. Because the rules live outside the AI, swapping one model for another doesn't change them.


-


**Know the cost and quality as models change:** One place tracks every agent, the model it runs on, and what each monthly close actually costs, measured by finished task, not by raw usage. When a provider changes its prices, the finance chief sees the impact the same day, and a better model can be swapped in without losing the trail.


-


**Keep a record that outlives any vendor:** A complete, tamper-proof history of what every agent did, one an auditor can read directly. It survives even if a model disappears overnight — as one just did.


## The Stakes


The scale is arriving fast. Analysts expect large companies to run an average of 150,000 AI agents by 2028, up from almost none in 2025, yet only about one in eight say they have the controls to manage that.


Finance will be among the heaviest users, because closing the books and checking the numbers are exactly the kind of repetitive, high-stakes work that these agents are built for.


Businesses have already decided they want choice in AI. The real question is who makes it safe: choice without oversight is a risk; choice with oversight is an advantage.


Ready to see how BlackLine Control Console could help you?


[Request a demo today.](https://www.blackline.com/request-a-demo/)


In This Post


- Why the AI Market Got So Unpredictable
- How Fast the Newcomers Caught Up
- The Accountability Problem
- We Have Seen This Before
- What We're Building
- The Stakes


Share Article


Just For You


Verity AI: BlackLine's Trusted AI for Finance & Accounting


[Read More](https://www.blackline.com/products/verity-ai/)


About the Author


JU


### Jeremy Ung


###### Chief Technology Officer


Jeremy oversees BlackLine’s global technology direction with an emphasis on enhancing BlackLine’s solutions for the Office of the CFO through connected data and AI-powered platforms that will accelerate the company’s ability to scale and continuously deliver customer value.
