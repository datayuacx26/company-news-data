---
schema_version: "1.0.0"
document_id: "e28c1c9e3347b531985450cb0a0506c07342926e7b3c30cb9344a5f1e0f6a3e4"
company_key: "everpure-inc"
company: "Everpure Inc."
source_id: "everpure-inc-rss-a7fca946ec64"
canonical_url: "https://blog.everpuredata.com/perspectives/teclo-ai-forum/"
published_at: "2026-07-10T13:00:00+00:00"
first_seen_at: "2026-07-25T03:30:11.662383+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:b258dab3d49102010b604c1087f20336e3bf49e26692c59aa82e3926dad1ee5c"
---

# Telco AI Forum 2026: How Telecom Operators Can Build Scalable AI

### Summary


At Telco AI Forum 2026, a panel discussion explored how telecom operators can build scalable AI by prioritizing data readiness, governance, and a hybrid edge-to-core deployment strategy.


Recently, I had the chance to participate in a panel discussion for[RCR Wireless’s Telco AI Forum](https://www.telcoaiforum.com/) , alongside Alex Guilbault of TELUS, Rob Hughes of 1Finity, Jake Katz of Cisco, and moderator Sean Kinney. Coming out of the conversation, my biggest takeaway was that successful, scalable telco AI is not really about hardware and who has the fastest GPUs. Instead, it’s about data readiness, governance, solid operating models, and the discipline to build around real use cases instead of hype.


## **Takeaway #1: AI is an evolution.**


The discussion started with a question about whether AI exposes a new infrastructure paradigm or is an evolution of work already in progress around cloud-native operations and mobile edge computing (MEC). The panel pretty consistently agreed that AI is an evolution of the work that has come before, though it brings some new challenges. While AI will not necessarily be needed at every remote office/managed switching office (MSO) site, it will bring different stressors on privacy and security and really raise the bar for data readiness and data context to provide meaningful answers. Operational simplicity will be critical in these lower-staffed and harder to reach sites.


## **Takeaway #2:** **Telco AI requires a hybrid deployment strategy.**


Not everything will be installed at the near edge sites, of course. One of the strongest themes from the discussion was that this is not a binary choice between centralized AI and edge AI, but rather a decision of where to place the workload for the desired outcome. The panel highlighted the efficiency benefits of centralization, especially around power and utilization, while cautioning that transport costs still matter. For example, large reasoning models should be centralized because they require much more compute and users are already accustomed to significant latency in the response time. However, physical AI, real-time voice chat, and computer vision models will benefit significantly from lower-latency deployment at the edge.


## **Takeaway #3: User experience and economics should drive AI placement.**


That hybrid point is not just theoretical. It connects directly to user experience and economics. In the discussion, I gave the example of real-time voice chat with an agent, where a centralized large model might deliver roughly two-second round-trip latency, while a smaller model served from a managed switching office could bring that closer to 400 milliseconds, which feels much more natural in conversation. On top of that, since smaller, focused models (small language models, models specifically built for a single domain of computer vision, etc.) can be leveraged at the edge, the compute and token costs are much cheaper and there’s a significant reduction in backhaul traffic (up to 83X).


## **Takeaway #4: The biggest challenges may not be what you expect.**


Another lesson I came away with is that the hardest AI problems are usually not where people expect them to be. The dynamic of AI FOMO (fear of missing out) means that many organizations start by procuring hardware, then choosing the model, and only later discover that the deeper issues are operational and data-related. This is the reverse of how we would normally build solutions. It not only leads to wasting the useful life of expensive hardware assets, but also often requires a redesign of the infrastructure for production. We talked about emphasizing people and process over the technology, especially:


- **Data readiness:** Data may be siloed and hard to extract, or not governed to ensure only the right data is used.
- **Data context:** The model may see data without enough business meaning or policy context to produce trustworthy outputs.
- **Operationalization:** The pilot may work, but the infrastructure is too complex to adjust and operate for scale in production across multiple customers or multiple modalities (video, text, images, etc.).


## **Takeaway #5: Start small and build the right foundation.**


The panel also discussed how up to 95% of AI projects still fail to make it to production and argued that organizations should focus more on shrinking the time to fail so they can reduce sunk cost and learn faster instead of focusing on improving the success rate. The panel agreed that operators should start small and consider the business cases first, especially adjacent to areas where mobile network operators have existing expertise or relationships.


The clearest shared lesson from the panel is that to deploy practical, sustainable, and scalable AI, operators should start with the business case, build the context-aware data pipeline, then deploy the infrastructure to support it. And none of this can be done without focusing on the people and processes necessary to build and support the solution.


## **Practical lessons for building scalable AI**


If I had to condense the panel into a few practical lessons for operators, they would be these:


- Start where the business case is clear, learn quickly, and scale from proven demand rather than from broad assumptions.
- Fail fast—test the weakest parts of the solution, learn, and iterate.
- Fix the data foundation early because governance, context, and operationalization are usually the real blockers to production AI.
- Invest in people, process, and corporate governance as seriously as you invest in compute.
- Build around differentiated telco strengths, such as distributed sites, network control, and proximity, rather than trying to replicate hyperscaler economics everywhere.


[Watch the panel discussion replay](https://attendees.bizzabo.com/804288/agenda/activity/1884038) .


## Get the Telco AI Forum recording


Register now to receive the recording and hear expert perspectives on scalable AI, automation, and analytics for telecom.


[Register Now](https://www.telcoaiforum.com/page/5523063/register-now)
