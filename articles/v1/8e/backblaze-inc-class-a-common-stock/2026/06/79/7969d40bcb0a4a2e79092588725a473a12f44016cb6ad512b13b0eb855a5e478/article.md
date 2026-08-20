---
schema_version: "1.0.0"
document_id: "7969d40bcb0a4a2e79092588725a473a12f44016cb6ad512b13b0eb855a5e478"
company_key: "backblaze-inc-class-a-common-stock"
company: "Backblaze Inc."
source_id: "backblaze-inc-class-a-common-stock-rss-a06767c1ff83"
canonical_url: "https://www.backblaze.com/blog/why-ai-projects-stall-data-silos/"
published_at: "2026-06-15T19:16:52+00:00"
first_seen_at: "2026-07-20T23:18:53.525659+00:00"
fetched_at: "2026-07-28T21:13:18.116875+00:00"
content_hash: "sha256:7c6bf1889dcb690f5217bf878cb80f87b814ced41aa51caad4021283322a28f2"
---

# Why AI Projects Stall: Data Silos

Customer records live in the database. Payment activity is safely stored in your payment processor. Call recordings and transcripts live in Zoom, Teams, Webex, or another video conferencing application–or are shared to Gong for customer insights. Telemetry resides in an observability tool like Grafana or DataDog. Your own day-to-day work is in Google Drive or OneDrive. It takes hundreds of human hours to figure out what customer behavior and business continuity patterns can be extracted from all of this data.


Extracting insights from your data starts with knowing what you have. The first step is centralizing it — pulling multimodal data from across your systems into a single storage repository where your engineering team and AI agents can actually access it. From there, you can assess what’s useful, what’s usable, and what still needs to be labeled or anonymized before it’s ready to work with.


Assessing your data is like cleaning out the garage: first, you have to do a full inventory to know what you actually have before deciding on new data destinations and purposes.


## **The hidden data silos most organizations overlook**


One of the less-discussed barriers to[AI readiness](https://www.backblaze.com/blog/your-ai-strategy-is-only-as-strong-as-your-data-foundation/) is that many organizations lack a complete picture of their own data assets.


Financial assets are documented. Physical assets are tracked. But images, audio recordings, video files, email archives, documents, logs, and customer interaction histories often sit across systems with inconsistent labeling, unclear ownership, disparate tooling, and no centralized catalog.


Customer calls, support chat transcripts, QA screen captures, surveillance footage, and product images all contain operational insight that can inform[AI applications](https://www.backblaze.com/cloud-storage/industries/ai-ml) , assuming they’re stored in a way that makes them accessible and usable. Most organizations haven’t done that inventory and don’t know what data they’re sitting on.


In our experience, organizations that broaden their definition of data — and build infrastructure to collect and manage it centrally — consistently find that their AI potential is larger than they initially estimated. The inverse is also true. Organizations that skip this step tend to hit the data silo problem mid-project, when data they assumed was available turns out to be fragmented, unlabeled, or simply missing.


The term “[multimodal](https://www.backblaze.com/blog/building-multimodal-ai-data-infrastructure-with-pixeltable/) ” describes this in practice: datasets that span formats—images, audio, video, text, and structured records—within the same pipeline. Managing multimodal data at a meaningful scale requires infrastructure decisions made well before an AI project kicks off.


## **Where the infrastructure question meets the strategy question**


Here’s what aligning AI strategy with data strategy actually requires:


Inventory what you have. Before sourcing anything new, take stock of what exists. Support call recordings, usage footage, survey data, transaction histories—these are continuously generated across most organizations and rarely treated as AI assets. A governance committee (described below) is the natural owner of this inventory.


Establish governance before you deploy. Who can use which data, under what conditions, and for what purposes. When data governance is established early, teams get answers in days rather than weeks. When it’s deferred, it becomes a bottleneck mid-project.


[Plan storage infrastructure for what you will have, not just what you have](https://www.backblaze.com/blog/gpus-are-only-half-the-equation/) . A storage decision made today carries a different cost profile 18 months from now. Hyperscaler egress fees that look manageable on a pilot-scale workload become structural constraints at training scale. Archive tiers that appear to reduce costs carry retrieval latencies incompatible with active AI pipelines. Modeling these costs before committing to a provider architecture prevents the predictable trade-offs: smaller datasets, shorter retention windows, fewer training cycles.


Make the C-suite part of the conversation.[IBM’s 2025 CEO Study](https://www.ibm.com/thought-leadership/institute-business-value/c-suite-study/ceo) found that 68% of AI-first organizations have mature, well-established data and governance frameworks. When the CEO is involved in AI governance decisions, the conversation stays connected to business strategy instead of fragmenting into siloed technical decisions.


## **The competitive advantage lives in the data (silos)**


Foundation models are increasingly commoditized. The leading model today will be superseded within months, and capable alternatives are widely available from multiple providers. The latest generation from any major provider is capable, widely available, and will be superseded by something better within months. What cannot be licensed, replicated, or accessed by a competitor is the proprietary data your organization has built up over years of operation: customer patterns, process histories, institutional knowledge.


Getting that data foundation right is what separates AI programs that scale from those that stall.


Organizations that align their AI strategy with their data strategy from the start make fundamentally different infrastructure decisions. They choose storage providers that support active data movement without penalizing it. They build governance structures that give the right people access without creating bottlenecks. And they treat data growth as a business opportunity, not a cost to manage.


For most organizations, that shift in thinking starts with a simple question: who owns AI strategy? If the answer is “it’s fragmented across different teams,” then the second question is: what would it take to bring those conversations into one room?


Everything that follows—the data readiness, the governance, the infrastructure that actually works at scale—flows from that first alignment.


Read the Backblaze ebook,[Navigating Multimodal Dataset Economics](https://www.backblaze.com/contact-sales/navigating-multimodal-dataset-economics-ebook) , to make decisions about the AI datasets at your organization.
