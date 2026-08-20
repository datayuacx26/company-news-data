---
schema_version: "1.0.0"
document_id: "702dcbd0f94bbb745a357cdcc98d0cc6fe3844439cc2f3971ec94217b78ba3b2"
company_key: "yc-hightouch"
company: "Hightouch"
source_id: "yc-hightouch-news-import-8089bfc075a8"
canonical_url: "https://hightouch.com/blog/what-does-a-composable-cdp-require"
published_at: null
first_seen_at: "2026-07-25T08:06:20.021758+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:209193a6ab483446869233f6c143b8472ca4ae84410271044f0c738d02f748da"
---

# What does a Composable CDP require?

This post is intended to provide clarity in a world where everyone calls themselves a CDP, and many CDPs call themselves a Composable CDP.


Five years ago, we made the case that the[traditional CDP was fundamentally the wrong architecture](https://hightouch.com/blog/cdps-are-dead) . Customer data shouldn't be copied into another vendor-controlled database. It should stay in the warehouse, where the rest of the business already operates.


That idea became the[Composable CDP](https://hightouch.com/blog/composable-cdp) . Today, thousands of companies run on this architecture, and nearly every CDP vendor has adopted some version of it.


Over those five years, nearly every company in our space has started calling themselves a Composable CDP – so we wanted to define what actually makes a complete Composable CDP offering.


## Why the Composable CDP became popular


Companies want to own their data. Many are investing in cloud data warehouses and data lakes, and building a Customer 360 within those platforms. Marketing teams and product teams need a layer on top that makes the data accessible and actionable.


Connecting to the data and having the data is just step one of the journey.


Some tools still copy entire tables into their own systems. Others are truly zero copy, but the differences appear in the details: how marketers use the product, how it scales, how it handles complexity, and how it performs under real-world conditions.


After working with more than 1,000 customers, these are the capabilities we believe every Composable CDP should provide, and what separates a capability that exists from one that's built for enterprise marketing.


## What to look for in a Composable CDP


A complete composable CDP is more than an architecture. It's a set of capabilities that have to work together, at enterprise scale, in production.


Here are the key dimensions, covered in depth below.


## Marketing execution


**Self-serve audience builder for marketers.**


Almost every marketing tool has some form of audience builder. What separates a great audience builder from the rest is flexibility, depth, usability, and the governance to run it safely at enterprise scale. It needs to handle audiences in both batch and real time, support approval workflows and consent rules, and give data teams the controls to ensure the right data reaches the right places – without slowing marketers down.


It should also allow marketers to work with all of their customer data, not just users and events. Modern customer relationships often involve products, subscriptions, accounts, households, orders, and other entities that don't fit neatly into a traditional event-based model.


A good self-serve audience builder ensures the data is legible: plain-language field names, clear definitions, value dropdowns that auto-complete and show what's actually in the data, and a schema translated for marketers rather than inherited from engineers.


From there, marketers should be able to create complex behavioral audiences with a visual interface, define calculated traits such as lifetime value or last product viewed, and understand what they've built through audience breakdowns, overlap analysis, and previews.


The next (and optional) step is a chat interface. Marketers should be able to describe the audience they want in plain English, ask questions about their data, and have agents build, explain, and refine audiences for them.


Finally, as organizations scale, reusable audience and activation templates become increasingly important. They give marketers a faster starting point while helping organizations enforce consistent logic, best practices, and governance.


**Journey orchestration.**


Next is journey orchestration. Knowing who to reach is half the problem. A complete CDP handles what happens next: multi-step flows, behavioral branching, time delays, channel selection, re-entry logic, coupon assignment, testing simulation, experimentation, measurement, and prioritization – all built on the same customer profiles, not bolted on from a separate tool.


Just as agents can help marketers build audiences, they should also help them build and optimize journeys through natural language conversation.


A basic audience builder can be stood up in months. A journey engine that works in production at a large scale takes years to build, test, and provide reliably, especially with the complexity of channels that may exist at the edges of the journeys.


**Match boosting.** For paid media, identity resolution based on first-party data often isn't enough. Ad platforms can only match customer records they have identifiers for.


A complete CDP should help marketers maximize addressability by enhancing audiences in-flight as they sync to ad platforms. Ideally, this process is done by appending additional identifiers to audiences and conversion feeds without dropping records that fail to match, and without replacing your original first-party data. That distinction matters: some data onboarding solutions discard records that can't be matched to an intermediary graph, which can actually hurt your match rates rather than improve them.


The result is increased reach, stronger lookalike seed lists, more comprehensive suppression, and more accurate measurement. Match boosting can also extend to anonymous visitors by matching unknown visits to known people, devices, and households, so your paid media reach isn't limited to customers you've already identified.


For teams investing in paid media at any meaningful scale, this is one of the highest-leverage capabilities a CDP can offer, and one of the clearest signals of whether a vendor has thought seriously about the full activation journey, not just the handoff from the warehouse to the ad platforms.


**Real-time and same-session personalization.** The most impactful marketing happens in the moment, when a customer is actively engaged with your product or website. Real-time personalization makes that possible, and the best real-time personalization requires two things working together: what's happening in the session right now and everything you already know about the customer from their history.


Most solutions can only give you one or the other. A warehouse stores historical information, but can't respond to session behavior in real time. Other CDPs can react to session behavior, but don't have access to the full customer history sitting in your warehouse.


Consider a travel site: a user searches for flights to Los Angeles. The real-time signal is the search. But what you show them next should depend on what you already know about them. If they live in LA, surface local excursions. If they don't, surface car rental options. That combination requires purpose-built infrastructure that sits alongside the warehouse and can evaluate both live behavioral signals and historical profile data simultaneously in under a second.


**Measurement.** A complete CDP should close the loop between activation and outcomes – connecting campaign performance back to your warehouse, where it can be measured against any business outcome that matters, not just the metrics the CDP itself tracks.


It should support randomized audience splits and holdout groups for multivariate testing across channels, with the flexibility to run 50/50 splits, 80/20 splits, or more sophisticated stratification based on attributes from your own data science models.


Results – including lift and statistical significance – should be visible directly in the platform, so marketers can understand what's working and share findings with stakeholders without exporting data or waiting on a data team. And it should write performance logs back to your warehouse for auditing, analysis, and unlimited retention, so nothing is trapped in a vendor-controlled system.


An often overlooked piece of measurement is ensuring conversion signals actually make it back to your ad platforms. Historically, ad platforms tracked conversions through browser cookies – when someone clicked an ad and later made a purchase, the browser connected the dots. As cookie deprecation and iOS privacy changes have eroded that tracking, those connections are increasingly missed, making it harder to know which campaigns are actually driving results. Server-side conversion APIs solve this by sending conversion data directly from your servers to ad platforms, bypassing the browser entirely. A complete CDP should support these natively.


**Proactive insights agents.** Traditionally, marketers arrive at their CDP with a plan already in place. They build the audience they had in mind, launch the campaign they already planned, and move on. The CDP executes. Historically, CDPs didn’t generate ideas of their own.


That's changing. A complete Composable CDP should be able to[proactively surface opportunities,](https://hightouch.com/blog/the-agentic-cdp) investigate what's working and what isn't, and present plans that are already sized and ready to act on – not wait to be asked. A competitor drops prices, and an agent has drafted two response strategies before your team has met about it. Light-colored yoga pants start trending in a region, and an agent checks inventory and customer fit before recommending a localized campaign – already built and ready to run. The CDP stops being a place you go with ideas and starts being a place that brings them to you.


That only works with complete context. An agent that can see your customer data but not your campaign history, brand guidelines, or business goals will still miss the point – proposing discounts on products that never go on sale, or campaigns that conflict with what's already in flight. A complete, Composable CDP brings all of that context together: customer data from your warehouse, brand assets from the systems that own them, and the strategy and goals that exist in no data table. Wherever your context lives, it should be accessible – because agents are only as smart as what they can see.


## Data foundation


**Schema layer.** Traditional CDPs force customer data into a predefined schema, typically centered around users and events. This is often lossy and doesn’t fit the complexities of most organizations. Complex organizations have accounts, households, subscriptions, products, reservations, applications, policies, stores, and countless other entities that don't fit neatly into a generic CDP schema.


A complete Composable CDP takes the opposite approach. Rather than forcing teams to remodel their data, it provides a semantic layer on top of the existing schemas that exist in the data warehouse. Via this semantic layer, the Composable CDP will write valid and governed queries directly on the underlying database, and then it will adapt as schemas change over time. It will also obfuscate some of the data and results, especially in environments where data privacy is paramount.


Marketing teams gain access to building audiences, journeys, and personalization using the same business objects that exist elsewhere in the company, surfaced in plain language and without custom engineering work.


This has practical benefits from day one. Implementation is faster because teams don't have to remodel customer data to fit a predefined structure, and maintenance is simpler because the CDP evolves alongside the warehouse. More importantly, marketers gain access to all of their customer data, not just the subset that a CDP was designed to support.


**Event collection and forwarding.** Events are how you capture what customers do on your website and in your apps – every page view, click, form submission, and in-app action. They're the behavioral layer of your customer data, and for many use cases, the most time-sensitive.


A key requirement is that events can power real-time use cases. If a CDP routes events through the data warehouse, then those events would have to be ingested and processed before anything downstream can act on them. That takes hours. A customer abandons a cart, and your triggered email arrives the next morning. A user hits a milestone in your app, and the personalized experience surfaces the next day.


A complete Composable CDP needs purpose-built event infrastructure that collects events at the source and forwards them in seconds to wherever they need to go, while storing them permanently in your warehouse as the long-term source of truth.


**Identity resolution on the warehouse.** A CDP should unify all records you have about a customer across devices, channels, and data sources into a single, trusted profile, creating the foundation for better personalization, targeting, and measurement.


Not all[identity resolution](https://hightouch.com/blog/announcing-adaptive-identity-resolution) is created equal. Look for these qualities.


-


**Does it support both deterministic and probabilistic matching?** Most CDPs only support deterministic identity resolution, which links records on exact identifiers, such as email, phone, and customer ID. It's precise, which is exactly what you want for lifecycle and transactional marketing.


But if you're running acquisition campaigns or suppressing existing customers from ad campaigns, deterministic-only matching limits your reach. Probabilistic matching closes that gap. It makes looser connections based on device signals and inferred attributes – recognizing, for example, thatemily.lee@gmail.com andemilylee@hightouch.com sharing the same physical address are likely the same person. That's the kind of reach deterministic matching alone can't provide.


Hightouch supports both approaches, allowing teams to maintain different identity graphs for different use cases and optimize for precision or reach as needed.


-


**Does it produce a golden record?** Once records are matched, the platform should let you define survivorship rules that determine which values win when there are conflicts – which email address is canonical, which phone number to use for outreach, which name to display. The output should be a clean, trusted golden record your whole business can rely on.


-


**Can it support entity resolution?** Companies often need to target beyond the individual level. A cable company markets to households, not just the person who pays the bill. A bank needs to understand all the accounts a single customer holds. A company selling to businesses needs to connect individual contacts to the account or organization they belong to. Entity resolution lets you model these relationships explicitly, stitching together records across sources into a unified view of each entity that matters for your business.


-


**Is there an audit trail?** Identity resolution makes thousands of decisions about your customer data automatically. If you can't see why records were merged, which rules fired, and which sources contributed, you can't verify that a profile is accurate. Every merge decision should be visible, explainable, and traceable back to the source record.


-


**Can you correct mistakes?** A misconfigured rule can merge thousands of records into a single profile. A mature system lets you find that profile, unmerge it, and rerun only that part of the graph without rebuilding everything from scratch. Without that, fixing one bad rule means taking down the entire graph and re-running it.


-


**Does the identity graph live in your warehouse?** A complete composable CDP writes the identity graph and unified records back to your warehouse. That means you own the graph – and can use it for analytics, internal applications, data science, and activation, not just what the CDP supports.


## Enterprise readiness


**Integration breadth and depth.** The audiences you build are only as valuable as the places you can send them, and integrations are what determine how widely you can deploy them. Hightouch supports 300+ destinations across marketing, advertising, CRM, and business tools.


It's not only the number that matters, but also the depth of each one.


There's a real difference between a connector that exists and one that's enterprise-ready. A basic connector is a pipe, moving data from A to B. An enterprise-grade connector is a control plane. It handles incremental syncs so destinations only receive what changed, not a full refresh every time. It manages row-level and field-level diffing for destinations like Braze and Salesforce to reduce API credit consumption. It handles rate limits, batching, parallelization, and retry logic gracefully – backing off when a destination API is overloaded rather than failing silently. And when destination APIs change, it adapts without breaking your syncs.


Observability matters too. A complete CDP should give you row-level visibility into every record and every sync, an in-app debugger that shows exactly what was sent and what came back, anomaly detection, and custom alerts. When a marketer asks why a specific customer didn't enter a campaign, you should be able to answer that question without filing a support ticket.


These are just a few examples. Integration breadth gives marketers flexibility. Integration depth gives them confidence that those destinations will work reliably at scale. A complete CDP requires both.


As AI becomes a bigger part of the marketing stack, integrations also extend beyond destinations. Support for open standards like MCP – which lets AI agents connect to your data and tools directly – ensures the intelligence you build isn't locked into a single vendor's ecosystem.


**Governance.** Customer data is among the most sensitive data a company handles. Regulations like GDPR in Europe, state laws like CCPA, HIPAA in healthcare, and KYC requirements in financial services set strict requirements for how it can be stored, accessed, and used. Multi-brand and global organizations have to manage data access across business units and regions. And customers themselves have expectations around consent and opt-out that have to be honored across every channel and destination.


A CDP sits at the center of all of this. A complete CDP should support:


-


**Multi-region data residency.** The ability to ensure customer data remains within a specific geography and never crosses borders, in compliance with regulations such as GDPR. This includes support for multiple workspaces for total isolation across business units or regions, and bring-your-own-bucket storage so data never rests in a vendor-controlled system.


-


**Data access by brand or business unit.** The ability to restrict which teams can access which data – so a specific brand or regional team sees only their customers, not the entire customer base.


-


**Role-based access controls (RBAC).** Granular permissions that define exactly what each user or team can see and do – down to specific data sources, destinations, and syncs.


-


**SSO and SCIM.** Support for SAML SSO and SCIM for identity provider integration and automated user lifecycle management, so access is managed centrally and stays in sync as teams change.


-


**Field-level controls.** The ability to mark specific attributes as sensitive or off-limits for marketing, so fields containing PII or PHI don't appear when marketers build audiences.


-


**Consent and opt-out enforcement.** The ability to ensure customers who have opted out or withdrawn consent are automatically excluded from the right channels and campaigns, without relying on manual processes to enforce it.


-


**Audit logs.** A complete record of how customer data is being accessed and used across the platform, for compliance and troubleshooting.


-


**Change management.** Review and approval processes that ensure only vetted changes reach production, reducing the risk of errors involving sensitive data.


-


**Lineage.** Lineage surfaces where resources are used across a workspace so users can understand their configuration, foresee the impact of a change, and be stopped from making destructive ones.


A complete CDP makes all of this possible without slowing marketing down, while giving data and compliance teams the confidence that governance remains intact.


## Taking it further with agentic marketing


Hightouch's Agentic Marketing Platform complements the composable CDP with two capabilities that extend what marketing teams can do. AI decisioning scales the decisions no team could make manually – which customers to target, which message to send, which channel to use, and when. Agentic content generates creative assets such as emails and ads to remove the content bottleneck that typically inhibits hyper-personalization.


**AI Decisioning.** Marketing at scale requires thousands of decisions that no team can make manually – which customers to target, which message to send, which channel to use, when to send it, and how to respond to what happens next.[AI decisioning](https://hightouch.com/blog/introducing-ai-decisioning-platform) automates this, drawing on your complete customer data, campaign history, and business goals to power next-best-action recommendations, predictive scoring, and real-time personalization. The decisions are only as good as the context behind them – which is why decisioning built on a composable foundation, with access to your full warehouse, produces fundamentally better outcomes than decisioning built on a subset of ingested data. This is powered by reinforcement learning that is constantly optimizing marketing towards a goal.


**Content generation.** Once you've decided what to do, you need the content to bring it to life. Hightouch's Lifecycle Studio and[Ad Studio](https://hightouch.com/blog/introducing-ad-studio) help create emails, ad creative, and messaging that is on brand. We specifically focus on photos and videos because AI struggles to follow brand guidelines in multi-modal settings. We teach AI to build content using existing assets and to fit your brand guidelines from the start.


## Beyond capabilities: What to look for in a CDP partner


A CDP is not a set-and-forget purchase. The category is evolving quickly, and the vendor you choose will shape how your marketing organization operates for years to come. Beyond the capabilities outlined above, a few things set the best CDP partners apart from the rest.


**A clear AI roadmap.** AI is only as good as the context it can access – so the first question to ask any CDP vendor is whether their platform can feed agents your complete customer data, campaign history, brand guidelines, and business goals.


The second is what they're actually building on top of it. AI is changing what's possible in marketing faster than any previous technology shift, and the vendors that will matter in five years are the ones investing seriously in that transition today – not bolting on a chat interface and calling it agentic. Look for a partner that can show you where they're taking the product, not just what it does today.


**A path to best in breed, without vendor lock-in.** The average marketing team manages dozens of tools – CDPs, email automation platforms, data onboarders, personalization engines, A/B testing tools, and more. Best of breed allows each function to choose the best tool for the job, but it can also get complex. Completeness of vision makes it possible to add more and more modules within the same stack that work with each other natively. The best CDP partners help reduce that sprawl rather than add to it. Use what you need, add more as you're ready, and never pay for what you don't.


**Expertise, not just software.** The best CDP vendors provide more than software – they embed practitioners directly with your team who understand marketing, know your industry, and can help you translate customer data into real business outcomes. That kind of expertise is hard to replicate. It's built from working across hundreds of deployments, seeing what works and what doesn't, and bringing that knowledge to every new engagement.


## Final thoughts


The Composable CDP has become the dominant model. But as vendors recast their offerings as Composable CDPs, distinguishing between true platforms and marketing messaging has become increasingly difficult.


A CDP is ultimately judged by outcomes. Can marketers move faster? Can they personalize more effectively? Can they measure impact more accurately? Can they adapt as channels, customer expectations, and technology continue to evolve?


The architecture question has largely been settled. Customer data belongs in the warehouse. The question now is what you can do with it. A complete Composable CDP turns that foundation into a system that helps marketers understand customers, orchestrate experiences, measure results, and increasingly, discover opportunities they wouldn't have found on their own.


If you want to learn more about the Composable CDP,[book a demo](https://hightouch.com/demo) today.
