---
schema_version: "1.0.0"
document_id: "bc0e9616f1a928e23b64ca6ade3a85fe9318485a2cbe793cdf3d682f4b037ea9"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/entity-analytics-watchlists"
published_at: "2026-05-05T00:00:00+00:00"
first_seen_at: "2026-07-20T03:30:09.053610+00:00"
fetched_at: "2026-07-28T22:12:48.521638+00:00"
content_hash: "sha256:25cd5298c51f297b7d485b93f1bb1baa0a32df5a0432ccecba7d2dc0287f68ef"
---

# Know who to watch before the incident finds you

5 May 2026 •


[Erik Huang](https://www.elastic.co/security-labs/author/erik-huang) •


[Jared Burgett](https://www.elastic.co/security-labs/author/jared-burgett)


# Know who to watch before the incident finds you


Elastic Security v9.4 introduces Entity Analytics Watchlists, a way to codify what your team already knows about high-risk entities and feed that context directly into risk scoring, without custom pipelines or detection engineering overhead


5 min read


[Product Updates](https://www.elastic.co/security-labs/category/product-updates)


Elastic Security v9.4 introduces Entity Analytics Watchlists, a new capability in the Entity Analytics suite that lets security teams create named, weighted lists of users, hosts, and services and feed that context directly into the platform's risk scoring pipeline. The gap this closes isn't awareness, as most security teams already know which entities deserve elevated scrutiny. The gap is that SIEMs have had no way to express that organizational knowledge as a risk signal. Watchlists do that without ES|QL, without pipeline configuration, and without a ticket to the detection engineering team.


##


Your riskiest entities are already known; your SIEM just doesn't know that


Security teams aren't starting from a blank slate. You already know certain people, hosts, and services deserve elevated scrutiny: the privileged admin whose access was never revoked after a role change, the engineer on a performance improvement plan, the acquired company's infrastructure not yet fully onboarded, the contractors brought in for a sensitive new business initiative.


The gap has never been awareness. The gap is that your security information and event management (SIEM) has no way to express that organizational knowledge as a first-class risk signal. Behavioral detection fires on anomalies. Threat intel fires on known bad indicators. But neither captures the **context your security and HR teams carry in their heads,** and that context is exactly what insider threat programs are built on.


83% of organizations experienced at least one insider attack in the past year $17.4M average annual cost of insider threat incidents in 2025 246 days average time to identify and contain a credentials-based breach


Mature user and entity behavior analytics (UEBA) platforms allow some form of static lists or risk multipliers, but they're often rigid, buried in configuration, and disconnected from the analyst workflow. Security teams deserve something better: a purpose-built, first-class feature that lets them codify what they already know about their environment and to have that knowledge dynamically influence every risk calculation in the platform.


##


Introducing Entity Analytics Watchlists


Watchlists are a new capability in Elastic Security's Entity analytics suite, arriving in the v9.4 release. They let security teams create named, described, rule-driven, or manually curated lists of users, hosts, services, or other entities and to attach configurable risk score weightings to every entity on each list.


> Think of Entity Analytics Watchlists as the bridge between your organization's institutional knowledge and your security information and event management’s (SIEM's) risk engine. You already know who deserves a second look. Now your platform knows too.


The term watchlist is a familiar concept in the industry, but Entity Analytics Watchlists go further. This isn't just a way to bookmark entities; it's a structured mechanism for injecting **custom correlation factors** directly into the risk scoring pipeline. Every entity that appears on a watchlist carries its membership as a weighted signal, compounded with alert activity, asset criticality, and behavioral anomalies to produce a single, prioritized risk score.


Take a concrete example: John Doe is a departing employee. He’s added to a "Departing Employees" Watchlist configured with an elevated risk weighting. He also owns a server on the "Critical Infrastructure" Watchlist. When John triggers an alert, for example, an unusual volume of file downloads, his risk score now compounds all three signals: the alert, the asset criticality, and both list memberships. The platform surfaces him far higher in the risk queue than it would for the same alert on an average employee. The analyst sees exactly why.


##


The lists your security program already maintains


Entity Analytics Watchlists are most powerful when they reflect the real-world risk categories your security, HR, and operations teams already track informally. Here are the most common starting points:


🚪 Departing employees On notice, performance improvement plans (PIPs), or offboarding: elevated exfiltration risk, regardless of whether an alert has fired. 🔑 Privileged access users Admins and service account holders whose actions carry outsized blast radius. 👑 Crown jewel hosts Critical infrastructure, IP repositories, and financial systems demanding tighter scrutiny.


**🤝 Mergers and acquisitions / acquisition cohorts** Newly onboarded entities where trust has not yet been fully established. **🚀 High-risk business initiatives** Teams in sensitive new ventures, requiring extra monitoring during critical phases. **🛡️ Known-safe allow lists** Dampen scores for verified low-risk entities, keeping analyst focus where it matters.


##


Custom correlation, finally, without the engineering overhead


Historically, bringing organizational context into a SIEM risk model has required significant custom engineering: lookup tables, enrichment pipelines, detection rule overrides, and constant maintenance as personnel and asset inventories change. For most security teams, that overhead means it simply doesn't get done. We remove that barrier entirely.
An insider threat analyst can create a "Departing Employees" list in minutes, add a risk weighting, and immediately see that context reflected in the entity risk queue. No Elasticsearch Query Language (ES|QL) required. No pipeline configuration. No ticket to the detection engineering team. The organizational knowledge that was previously locked in spreadsheets, HR systems, or informal team awareness is now a first-class signal in the platform.


This is the ability to build risk correlations factors that simply haven't been possible anywhere else in the market and to do it without requiring detection engineering expertise.


For more mature teams, our custom watchlists also integrate cleanly with automated population, meaning that lists can be kept automatically current as conditions change or through APIs. An HR integration that marks an employee as departing can trigger list membership automatically; when they're fully offboarded, they're removed. The signal stays fresh without manual upkeep.


##


Coming in Elastic Security v9.4


Entity Analytics Watchlists ship as a major roadmap item in the upcoming Elastic Security v9.4 release. They’re available to customers running Elastic Security with Entity analytics enabled.


If you're already using entity risk scoring and asset criticality, Entity Analytics Watchlists are the natural next step, layering your organization's operational context on top of the platform's behavioral and alert-based signals to produce the most accurate, prioritized risk picture possible.


We've heard from security teams across industries that this capability is one of the most anticipated additions to the UEBA toolkit. We can't wait to see what lists you build.


##


Frequently Asked Questions


**Q: How do I add organizational context to Elastic Security's risk scoring?** A: In Elastic Security v9.4, you can inject organizational context into your risk scoring by utilizing Entity Analytics Watchlists, which allow you to ingest custom lists of high-value entities such as users, hosts, or services and assign them specific risk weightings. These watchlists function as dynamic correlation factors; when an entity on a watchlist appears in an alert, the system automatically compounds its risk score based on your pre-configured weights. This ensures that threats involving your most critical assets are prioritized instantly, transforming raw security data into an outcome-driven investigation queue that reflects your company's unique threat landscape.


**Q: How do I monitor high-risk employees in a SIEM without custom detection rules?** A: Elastic Security Watchlists let you add entities like departing employees or privileged admins to a named list with an elevated risk weighting, with no ES|QL or pipeline configuration required. Their list membership is factored into risk scoring automatically alongside any alert activity.


**Q: How do insider threat programs integrate with SIEM risk scoring?** A: Elastic Security's Entity Analytics Watchlists let insider threat and security operations teams codify existing risk knowledge — departing employees, privileged access holders, acquisition cohorts — and have that context automatically influence entity risk scores without requiring detection engineering involvement.


---


*Entity Analytics is available in Elastic Security.[Learn more about Entity Analytics Watchlists and how the entity store governs user entities.](https://www.elastic.co/docs/solutions/security/advanced-entity-analytics/watchlists)*


#### Jump to section


- [Your riskiest entities are already known; your SIEM just doesn't know that](https://www.elastic.co/security-labs/entity-analytics-watchlists#your-riskiest-entities-are-already-known-your-siem-just-doesnt-know-that)
- [Introducing Entity Analytics Watchlists](https://www.elastic.co/security-labs/entity-analytics-watchlists#introducing-entity-analytics-watchlists)
- [The lists your security program already maintains](https://www.elastic.co/security-labs/entity-analytics-watchlists#the-lists-your-security-program-already-maintains)
- [Custom correlation, finally, without the engineering overhead](https://www.elastic.co/security-labs/entity-analytics-watchlists#custom-correlation-finally-without-the-engineering-overhead)
- [Coming in Elastic Security v9.4](https://www.elastic.co/security-labs/entity-analytics-watchlists#coming-in-elastic-security-v94)
- [Frequently Asked Questions](https://www.elastic.co/security-labs/entity-analytics-watchlists#frequently-asked-questions)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=Know%20who%20to%20watch%20before%20the%20incident%20finds%20you&url=https://www.elastic.co/security-labs/entity-analytics-watchlists)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/entity-analytics-watchlists)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/entity-analytics-watchlists&title=Know%20who%20to%20watch%20before%20the%20incident%20finds%20you)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/entity-analytics-watchlists&title=Know%20who%20to%20watch%20before%20the%20incident%20finds%20you)
