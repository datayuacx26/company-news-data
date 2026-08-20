---
schema_version: "1.0.0"
document_id: "329fb27f5dce996cdd65c309d87c68c7ef56f812029f0a783c00431667395ed5"
company_key: "riskified-ltd-class-a-ordinary-shares"
company: "Riskified Ltd."
source_id: "riskified-ltd-class-a-ordinary-shares-rss-dd7d0cc56e2d"
canonical_url: "https://medium.com/riskified-technology/api-integration-sizing-gone-wrong-how-observability-tools-rescued-our-decisions-7b9451affa34"
published_at: "2026-01-05T07:25:27+00:00"
first_seen_at: "2026-07-20T23:18:31.853064+00:00"
fetched_at: "2026-08-20T02:06:38.087890+00:00"
content_hash: "sha256:ae15abd4f0751e45d26b45757375751c3d2be47a0d5fd07bf223fce8242531ea"
---

# API Integration Sizing Gone Wrong: How Observability Tools Rescued Our Decisions

#### From slow responses to clear visibility: a real-world story of API performance debugging


ֿWe all have been there - an external vendor gives you an estimated API response time - say, 1 second.
Your engineering team nods, integrates the new data source, and pushes it to production, eager to see the enhanced data in action. Everything looks green… until the dashboards start telling a different story. Suddenly, those promised sub-100ms response times are spiking to over 4 seconds, grinding critical decision-making processes to a halt. This isn’t just a technical glitch, it’s a **business risk** . Integrating third-party data is supposed to enrich our information, not cripple our performance.


This post isn’t about blaming the vendor, but about the invaluable process we used to get to the truth. In this post, I’ll walk you through the systematic, tool-based approach my team took to debug a critical high-latency issue with a new third-party data API.
The main takeaway - You can’t solve what you can’t measure.


**Let’s dive into the practical tools and techniques that allowed us to isolate the problem and present irrefutable evidence.**


### Tool #1: Raw Request Auditing Logging with Key Identification


Our first, and most critical step, was to move beyond simple success/fail logging.
When integrating external APIs, a standard log entry often looks like:\[INFO\] API call to /data/enrichment succeeded in 120 ms


This kind of log is perfectly fine for basic observability, but it quickly becomes useless when a performance outlier occurs. When response times deviate from expectations, especially when working with external partners, we need much more granular insight.


The key to debugging partner-related performance issues, and the first tool in our toolbox, was auditing what actually matters: **response time per unique identifier** . We needed to understand not just whether a request succeeded, but how long each specific request took, and for whom.


To achieve this, we instrumented our code to capture two crucial pieces of data for every single API call:


- The **unique API key** used for the request
- The **exact time** elapsed from sending the request to receiving the full response body, measured in milliseconds


By isolating the response time for every individual key, we created the foundation for a meaningful performance investigation rather than relying on aggregated averages.


In practice, this meant explicitly measuring the duration of every enrichment call and attaching it directly to the audit event itself. By capturing the start time before calling the external data source and calculating the elapsed time once the response was received, we ensured that every request could later be analyzed and aggregated by key.


```text
def callAPI(enrichmentRequest: EnrichmentRequest): Future[EnrichmentResponse] =     val enrichStartTime = System.currentTimeMillis()      logger.info(s"enrich is handling")     val enrichmentResponse = callDataSource(enrichmentRequest)     val duration = System.currentTimeMillis() - enrichStartTime      auditEventPublisher.publishAudit(       enrichmentRequest,       enrichmentResponse,       duration,     )      enrichmentResponse   }
```


This small change transformed our logs from simple records into an investigative audit dataset. It allowed us to later aggregate and visualize response times per key, identify performance outliers, and directly correlate latency issues with specific partner traffic patterns.


### Tool #2: Dashboard for Visualization and Isolation


Having the raw audit data is only half the battle. We needed to see the forest *and* the trees - to understand the full scope of the problem and pinpoint the specific requests causing the most pain.


We created a performance dashboard which included a p99 HTTP client duration graph, allowing us to track the latency of our requests to the external data source.


Beyond simply visualizing the data, the dashboard helped us understand how the system behaved under real production conditions, identify latency spikes, and validate whether the integration met our original performance expectations.


The dashboard is owned by the development team. We believe that monitoring is an integral part of our responsibility as developers - not something to be handed off elsewhere. This means defining meaningful alerts and actively keeping the dashboard in front of us, rather than treating it as a passive artifact.


Having a dashboard is important, but without clear ownership, alerts, and regular attention, it’s easy for critical signals to go unnoticed.


### Tool #3: Cross-Verification with Vendor Logs


Visualization confirmed the impact but didn’t prove the cause.
The high latency could technically have been an issue on our side (network, client-side processing, etc.).
This is where the third tool - the vendor’s own logs - came into play.


We opened a professional, data-backed case with the external company, including:


- The P99 latency data from our dashboard.
- The exact logged API Keys and the timestamps for a set of high-latency calls.


Because we provided specific keys and timestamps, they were able to cross-reference their internal logs.
They granted us access to a subset of their logging system.


The result was that the latency recorded in their internal logs, perfectly matched the response times we recorded on our end.
This confirmed, with 100% certainty: The performance bottleneck was within the third-party provider’s infrastructure, not ours.


### Conclusion: The Power of Data-Driven Debugging


In our case, the investigation was slightly more complex than a typical third-party integration. The customers consuming our product were also direct customers of the external API provider, which added an extra layer of sensitivity and coordination. Once we completed the three-step process-instrumenting the code, visualizing the data, and validating it against the partner’s perspective - the picture became clear. We were able to pinpoint the source of the latency, understand why the response times deviated from our expectations, and address the issue effectively. The discussion shifted from assumptions to facts, and from friction to collaboration.


More broadly, this experience reinforced the power of data-driven debugging. In every complex external integration, unexpected challenges will arise - whether due to network transit, inefficient API design, rate limiting, or backend bottlenecks on the vendor’s side. Your most valuable skill in these situations is the ability to isolate the problem with confidence.
Next time an integration underperforms, remember this three-step process:


1. **Instrument:** Log the necessary identifiers and granular timing data in your client code.
2. **Visualize:** Use a dashboard to expose the full scope of the problem and identify outliers (P99).
3. **Validate:** Cross-verify your data with the vendor’s logs to establish the true bottleneck.


This approach not only accelerates resolution but also builds trust and credibility as a reliable engineering partner. Regardless of the specific external issue, the ability to identify the true bottleneck and build resilient mitigation layers is what ultimately defines a successful, professional integration.


I’d love to hear your thoughts - reach out on LinkedIn:[Tony Dayan](https://www.linkedin.com/in/tony-dayan/)


---


[API Integration Sizing Gone Wrong: How Observability Tools Rescued Our Decisions](https://medium.com/riskified-technology/api-integration-sizing-gone-wrong-how-observability-tools-rescued-our-decisions-7b9451affa34) was originally published in[Riskified Tech](https://medium.com/riskified-technology) on Medium, where people are continuing the conversation by highlighting and responding to this story.
