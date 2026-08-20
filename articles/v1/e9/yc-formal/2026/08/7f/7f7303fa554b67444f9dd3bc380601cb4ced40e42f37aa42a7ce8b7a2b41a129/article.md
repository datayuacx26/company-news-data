---
schema_version: "1.0.0"
document_id: "7f7303fa554b67444f9dd3bc380601cb4ced40e42f37aa42a7ce8b7a2b41a129"
company_key: "yc-formal"
company: "Formal"
source_id: "yc-formal-news-import-a5b088b50d89"
canonical_url: "https://www.formal.ai/blog/metabase-zero-day-data-access/"
published_at: "2026-08-11T00:00:00+00:00"
first_seen_at: "2026-08-12T04:22:53.162080+00:00"
fetched_at: "2026-08-12T04:22:55.029920+00:00"
content_hash: "sha256:af519051b3ce7eb4142c5b30c033c9cfaff68d1253e339d6134237e102ebb497"
---

# The Metabase Zero Day and the Case for Enhanced Data-Layer Security

At Formal, when we meet with security engineers and CISOs to listen to their use cases and introduce our product, they *always* challenge us to frame our conversation in a way that makes their lives better and/or easier.


Yes, security absolutely cares about keeping up with fast-moving vendors and the rapidly improving tech stack. They love the opportunity to learn how they could be tackling the same problems more efficiently. But, they’re also grounded by the real-world risks they carry, ones they must knock out as quickly as possible before the clock runs out.


News like the latest Metabase security incident, rated the maximum severity (CVSS 10.0), interjects itself into our conversations frequently, so we thought we’d take a moment to acknowledge it, as it’s currently center stage in some of our conversations.


## What’s the Metabase security news?


A[zero-day vulnerability](https://www.wiz.io/blog/inside-the-metabase-sqli-exploited-in-the-wild) in Metabase allowed unauthenticated remote attackers to inject arbitrary SQL into the Metabase application database, and Metabase has confirmed active exploitation of this vulnerability. Furthermore, another aspect of this SQLi is that the unauthenticated attacker can become *admin* on the Metabase instance. Unfortunately, a number of companies who use Metabase have reported that their customer data was accessed as a part of this incident, which often means we can expect additional downstream impact later.


If you’re using Metabase and haven’t upgraded to the latest major version,[do so right away](https://github.com/metabase/metabase/security/advisories/GHSA-vwf4-m7j8-wcjf) ; if upgrading isn’t possible, make sure you implement the workaround immediately. We are first and foremost security practitioners at Formal, so drop what you’re doing and fix this. We’re just back from Hacker Summer Camp this week, so we are a little behind on editorials on the latest news. This is the kind of supply-chain vulnerability we all have nightmares about.


### Too many tools: what is Metabase?


Metabase is a powerful and easy-to-use BI tool that a lot of small- and mid-sized companies rely on as a lightweight alternative to more expensive, hosted solutions like[Tableau](https://www.tableau.com/business-intelligence) or[Looker](https://cloud.google.com/looker-bi) (Google).


Metabase’s power lies in enabling your non-technical users to build their own queries, visualize data, etc. as well as your power users to create custom/templated queries, embed live dashboards into their apps, etc. It’s a very user-friendly tool and adoption is wide.


On its own, Metabase doesn’t hold any data – instead, it accesses customers’ existing data sources as a BI layer.


## Have companies been identified as “affected?”


Yes. After a quick search, we saw that a number of well-known companies were affected and posted information about the breach online.


The data exposed by this attack depends on the company affected, and ranges from: cryptographic hashes of API keys, custom values (headers, cookies, query-string parameters, etc.), names, email addresses, and passwords stored as cryptographic hashes, Slackbot access tokens, physical shipping addresses, and network-login IP addresses.


## Could this have been prevented by using Formal?


We know what you’re probably thinking: here comes the vendor pitch, but guess what? *We get these questions* *every time something like this is in the news* , mainly because Formal has always provided access control for any actor on the network. We’ve also found that security teams need to justify any new spend, and have a “never let a good incident go to waste” means of justifying the new spend.


Formal probably couldn’t have been a silver bullet for preventing this incident wholesale; however, *it can easily limit the blast radius for similar incidents* , and it is extremely helpful on the response/corrective side of an incident.


Regarding the prevention side of the incident, Formal customers create policies that limit how and what kinds of data can be accessed. They can choose to *mask all PII* before it hits Metabase, they can apply *limits on the volume of data* that can be queried (throttling), they can specify *how often and when* data is accessed (rate limiting), *who* is accessing the data, *from where* the data is accessed and *to where* it’s destined, whether *step-up auth* is needed for certain situations, etc. In fact, every aspect you can think of for applying preventative security controls to the data, is available for use.


On the corrective side, because neither Metabase nor the upstream sources natively have an easy way to audit the nature of the queries made by Metabase users and abusers, our customers can use Formal as a proxy between Metabase and its upstream data sources. A quick look at the Observability → Logs page shows exactly what queries were run, by whom, when, what was accessed, etc.


When it comes to incident response, quickly and thoroughly collecting these details is critical in your customer comms, because you and your customer teams will be repeatedly asked for the scope of impact. It’s most helpful to everyone when you can provide customized assurances on each customer’s specific timeline, specific resources affected, and specific cleanup instructions so they can move past this incident as quickly as possible, because despite everyone’s best efforts, incidents will continue to happen.
