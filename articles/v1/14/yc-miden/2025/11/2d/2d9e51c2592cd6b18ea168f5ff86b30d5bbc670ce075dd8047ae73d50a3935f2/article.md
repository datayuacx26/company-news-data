---
schema_version: "1.0.0"
document_id: "2d9e51c2592cd6b18ea168f5ff86b30d5bbc670ce075dd8047ae73d50a3935f2"
company_key: "yc-miden"
company: "Miden"
source_id: "yc-miden-atom-c1e119f02e59"
canonical_url: "https://blog.miden.co/building-trust-in-payments"
published_at: "2025-11-03T12:34:00+00:00"
first_seen_at: "2026-07-25T14:13:42.218758+00:00"
fetched_at: "2026-07-28T21:59:41.762292+00:00"
content_hash: "sha256:ef90e5fe07e2baeda64f06e400bdaa320d0ec0c39c967aae8a17606e926a1d70"
---

# Building Trust in Payments: Lessons From (3) Fintech Experts

Behind every transfer or card swipe are ops leaders, settlement experts, ensuring every naira or dollar reaches its destination.


For this edition, we brought together experts who manage payments in the African fintech ecosystem. They open up about the innovation, pressure, and hard lessons behind every payment transaction.


A special thank you to the experts who shared their time and perspectives.


1. **Ekenechukwu Okpalauwaekwe** - Manager, Payment Operations at Chipper Cash
2. **Oyedola Yemi-Olatunji** - Manager, Scheme Mandates & Partnerships (Global) at Checkout.com
3. **Blessing Obioha** - Senior Settlement Officer at Moniepoint Group


### Q: Settlement is often described as the “invisible layer” of fintech. How would you explain its importance to someone outside the industry?


**Blessing:**


Think of it like the engine in a car.


You don't see it when you're enjoying the smooth ride, the cool air conditioning, or even the great music.


But if that engine knocks even once, the entire journey is over. You're going nowhere.


**Settlement is that engine.**


###


### Q: What does sustainable growth look like for you, beyond transaction volume, especially in a fast-growing fintech company?


In fintech, growth can be subjective. It can be the number of transactions performed in a day, the amount of these transactions, or more.


As such, it’s easy to get hung up on non-substantial factors.


But we asked an expert, and here’s what she had to say:


**Ekenechukwu** :


Transaction volume is like counting how many cars pass through a toll booth, when what really matters is whether those drivers become regular commuters who trust your road enough to build their daily routine around it.


*She went further to explain;*


We stopped celebrating volume milestones and started tracking what I call customer stickiness metrics.


Are people using us once and disappearing, or are they building us into their financial habits?


We measure things like how many users send money in both directions across a corridor, because bidirectional usage means we're solving real economic relationships, not just one-time transfers.


The companies that really get this right focus on corridor maturity instead of corridor expansion.


### Q: How do you handle thousands of transactions daily, especially in a fast-growing fintech company, while reducing payment errors?


**Yemi** :


The right question isn’t just how do we handle errors today?


But what happens if these errors multiply 100x or 1,000,000x?


If the outcome would be a disaster for customers, internal teams, or growth, then you need to design systems that prevent or flag those errors before they ever happen at scale.


I learned this firsthand, leading payments at a billion-dollar fintech while we were still just a promising startup with trickles of transactions, preparing to launch instant payments.


At first, we used Excel sheets and formulas to flag errors, but we quickly realized that wouldn’t work once volumes surged.


So we built automation.


Our system could flag when a transaction showed “pending” or “failed” internally, but returned a “00” success response from the payment processor.


It worked by running real-time queries against the processor’s API, updating statuses immediately rather than waiting hours or days for the reconciliation team to catch it.


**The key is collaboration** . Payments teams must work hand-in-hand with engineers to anticipate error scenarios and build real-time resolutions.


That way, when the payments team reviews data, they’re focused on catching the 1% of new or edge-case errors that automation didn’t pick up, not firefighting the 99% that could have been prevented.


### Q: How do you handle operations and compliance when partner systems (banks, switches, card schemes) fail or delay?


When your system relies on a partner’s infrastructure, their functionality also affects or influences your value.


Your end users power their businesses or individual purposes with this infrastructure. As such, it can feel overwhelming to handle such situations, especially in high-risk circumstances.


However, different situations call for different actions. According to:


**Ekenechukwu:**


There are **scheduled maintenance windows** , and there are **abrupt failures** . These are fundamentally different operational challenges.


- **Scheduled maintenance is Manageable:**


You can plan around it, pre-alert customers, route traffic to alternative providers during that window, and staff support accordingly. It's predictable, so you control the narrative.


- **Abrupt Outages are Where Things Get Messy:**


No warning, no clear resolution timeline, no idea what's actually broken. And when they keep happening repeatedly, it becomes exponentially harder to manage.


*The first time, customers are understanding. The second time, they're frustrated. By the fourth or fifth time, they're questioning whether they can trust the platform at all.*


For every partner we work with, we have a backup plan. We've built what I call a **"backup plan for your backup plan"** approach.


The key is making these switches invisible to users while keeping our internal teams informed about what's happening behind the scenes.


The technical solution is only half the battle. The other half is communication, and that's where it gets really complex.


Stressed users don't read communications about outages.


Someone stuck at a fuel station isn't checking our Instagram feed. They're panicking about getting home and having their payment completed.


That's why support is critical; they're not just answering tickets, they're managing anxiety.


*While handling the customer’s perspective is very important, especially for a growing company, it’s important to also address other affected groups -* ***the Business & Reconciliation Team*** *.*


**Yemi:**


1. **Streamlined Contract for Extended Downtimes:**


In some climes, contracts are enforced strictly: if delays or mismatches go beyond agreed SLAs, **there should be financial credits or rebates back to the fintech** . Every mismatch has a cost, so contracts must protect the ops process and the bottom line.


**2. Push partners for more automation:**


What AI or RPA tools can speed up bulk refunds, resolve anomalies faster, and reduce the time from days to seconds?


The goal is not just to fix errors, but to recover fast enough that reconciliation doesn’t become another week-long project.


Reconciliation isn’t just fixing mismatches; it’s about protecting customers, safeguarding the business, and building systems that bounce back fast.


### Q: How do you communicate operational reliability to non-ops stakeholders (like CEOs, regulators, or investors)?


One of the toughest challenges most fast-paced fintech companies face is collaboration, especially with support teams, stakeholders, and more.


Here are the best practices for navigating these situations;


**Ekenechukwu** :


1. **For Senior Leadership - Translate everything to business impact:**


If I were to translate a business impact, I wouldn't tell them about service uptime or transaction success rates/failures; I would tell them what it means for the business.


Instead of saying "we achieved 99.5% transaction success rate," I say "we successfully processed 99.5% of all transactions, which means we protected revenue from failed payments and prevented customer churn. The 0.5% that failed? We've identified the root causes, mostly partner system issues, and built redundancy that's already improving that number.


**2. For Engineers - Technical depth and learning opportunities:**


Engineers want to understand what actually happened and what we learned. When systems fail, I share detailed post-mortems with the engineering team, not to assign blame, but to map dependencies and improve architecture.


After any outage, we document the failure cascade: what triggered it, how long detection took, which services degraded, how failover performed, and where we found gaps. Engineers respect transparency about what broke and why.


**3. For Other Stakeholders - Context for their specific concerns:**


Different stakeholders need different contexts. Customer support teams need to know what to tell frustrated users.


Finance needs to understand the impact on settlement and reconciliation. Product teams need to know if reliability issues affect roadmap priorities.


I use a **"Why-What-How"** framework: Why does this matter to your role? What's happening in terms you understand? How are we addressing it, and what do we need from you?


Across all stakeholders, I've learned that proactive communication builds trust while reactive communication destroys it. Communication isn't just crisis management; it's relationship insurance.


### Q: What practices from your playbook would you say are non-negotiable for someone in your position?


These can be different from one individual to another. According to Ekene, she said:


**Ekenechukwu:**


1. The support team's involvement in every product launch
2. Having a backup plan for your backup plan.
3. Never compromise on transparency with customers during problems. Operate on a simple principle: tell customers what you know, when you know it, even if the news is bad.


### Q: If you could give one piece of advice to budding individuals in your field, what would that be?


**Yemi:**


I’d say start your career in a bank or a highly structured fintech. Payments are like medicine; it has so many moving parts, and every detail you know or don’t know can make or break the organization you work for. But if you build the right foundation early, you’ll set yourself up for long-term success and keep learning as the industry evolves.


**Blessing** :


Treat your knowledge like you would a high-performance car. You don't wait for it to break down on the highway before you check the engine. The fintech landscape shifts constantly. New regulations, new technologies, new fraud patterns. You have to be proactive about maintenance, continuous learning, system updates, and process reviews. If you get complacent, your skills become obsolete, and no matter how good your past reputation was, you risk being left behind.


*Keep “Panadol in your bag”, a good sense of humor, and patience, because some days you'll need it.”*


##


## Conclusion


Every payment tells a story.


The insights shared in this roundtable reveal what makes those stories possible. From trust, precision, to a strong partnership, everybody has a major role to play.


**Miden is the financial operating system for modern businesses** , powering the entire payment and finance lifecycle in real time.


From **card issuing to reconciliations, global payments, pay-ins & pay-outs** , plus an AI-driven core banking engine, our composable, API-first infrastructure gives companies the tools to move money, streamline operations, and scale with confidence.


**Built in Africa. Designed for the world.**


Share


[Twitter](https://twitter.com/intent/tweet?text=Building%20Trust%20in%20Payments:%20Lessons%20From%20(3)%20Fintech%20Experts%20https://blog.miden.co/building-trust-in-payments)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://blog.miden.co/building-trust-in-payments)[Linkedin](https://www.linkedin.com/sharing/share-offsite/?url=https://blog.miden.co/building-trust-in-payments)
