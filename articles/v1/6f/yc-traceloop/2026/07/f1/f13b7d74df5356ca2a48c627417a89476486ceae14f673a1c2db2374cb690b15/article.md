---
schema_version: "1.0.0"
document_id: "f13b7d74df5356ca2a48c627417a89476486ceae14f673a1c2db2374cb690b15"
company_key: "yc-traceloop"
company: "Traceloop"
source_id: "yc-traceloop-news-import-a0183e54c5bf"
canonical_url: "https://www.traceloop.com/blog/from-bills-to-budgets-how-to-track-llm-token-usage-and-cost-per-user"
published_at: null
first_seen_at: "2026-07-24T05:14:44.927731+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:4f6cb793526f8c0bffad315c52cd5f1cd7379ac0e49e286a89d5d44417155309"
---

# From Bills to Budgets: How to Track LLM Token Usage and Cost Per User

The adoption of Large Language Models (LLMs) has unlocked incredible capabilities, but it has also introduced a new, volatile, and often-alarming line item to the budget. Unlike predictable cloud infrastructure costs, LLM spend can spike unexpectedly, driven by a single new feature or even one power user. Simply looking at the total monthly bill from your provider is a reactive measure; it tells you *that* you overspent, but not *why* . To effectively diagnose and control budget overruns, teams must move from high-level bills to granular, per-user cost attribution.


**Key Takeaways:**


- Granular Attribution is Key: The only way to control LLM costs is to track token usage and attribute it to specific dimensions, such as per-user, per-feature, or per-team.
- Metadata Tagging is the Mechanism: The core technical solution involves attaching "tags" or "metadata," like a user_id or feature_name, to every single API request sent to an LLM provider.
- Proxies & Standards Simplify Tracking: Using an LLM proxy or an OpenTelemetry-based solution can centralize all API calls, making it easier to automatically attach this metadata without littering your application code.
- Observability Enables Control: A platform that can ingest and visualize this data allows you to move from diagnosis to proactive control by setting up dashboards and alerts for specific user or feature spending.


### **The FinOps Framework for Controlling LLM Spend**


Managing LLM costs is a classic FinOps (Financial Operations) challenge. As the FinOps Foundation points out, the fundamental principles of cloud cost management are visibility, accountability, and optimization. These apply directly to AI. The primary unit of cost is the token, and the primary challenge is attribution. Without a "robust tagging strategy," it's impossible to hold a specific team accountable for a cost spike or to know if a new feature is profitable.


This is where the technical solution comes in. The most effective way to track costs per user is to pass metadata with every API request. For example, by including a user_id in the metadata of an API call, you are permanently tagging that request (and its associated cost) to a specific user. This method is the foundational "how-to" for granular tracking.


However, managing this manually across dozens of services and models is not scalable. This is why many teams are adopting a proxy layer or a standardized observability framework. An LLM gateway or proxy acts as a single front door for all your LLM calls, providing a perfect central checkpoint to auto-log tokens, models, and user data.


A more integrated and powerful approach is to use a framework built on OpenTelemetry. OpenTelemetry is the industry standard for tracing and observability, allowing you to capture rich, contextual data about every request in your application, not just the LLM call. By leveraging this standard, you can automatically capture the user_id from your application's trace and link it to the token usage and cost of the LLM call. This provides a complete picture, enabling you to[visualize LLM performance](https://www.traceloop.com/blog/visualizing-llm-performance-with-opentelemetry-tools-for-tracing-cost-and-latency) and see exactly which user action triggered which LLM cost.


This level of detailed, per-user visibility is the key to diagnosing and controlling overruns. Instead of a single, baffling bill, you get a detailed dashboard. You can instantly filter and group costs to answer critical questions like: "Which 5 users are costing us the most?" "Is our new 'summarizer' feature responsible for the 30% budget spike?" "How much does an average request from 'user-123' cost?" This visibility is the first and most critical step to building alerts, setting budgets per user, and making data-driven decisions.


For teams building on modern AI stacks, Traceloop provides this capability out-of-the-box. Built on the OpenTelemetry standard, Traceloop automatically instruments your LLM calls to capture essential metrics and dimensions. It allows you to effortlessly add attributes like user_id and feature_name to your traces, providing pre-built dashboards to visualize[granular token usage and cost per user](https://www.traceloop.com/blog/granular-llm-monitoring-for-tracking-token-usage-and-latency-per-user-and-feature) . This transforms your LLM costs from an unpredictable black box into a transparent, controllable, and optimizable part of your budget.


‍


### **FAQ Section**


Q1: What is the most important metric to track for LLM cost control?


A1: While total cost is the ultimate metric, the most actionable metrics are prompt tokens and completion tokens attributed to a specific user, feature, or team. Tracking "total cost per user" is the key to understanding your budget drivers and identifying anomalies.


Q2: What's the difference between a proxy and an OpenTelemetry-based solution?


A2: A proxy centralizes your API calls, which is great for logging and simple metadata. An OpenTelemetry-based solution, like Traceloop, is more powerful because it's part of your application's core observability. It can automatically link LLM costs to a user's entire journey through your application by understanding traces and spans, giving you much richer context for debugging and cost analysis.


Q3: How do I start tracking costs per user if I'm already in production?


A3: The easiest way is to integrate an observability SDK built for LLMs. For instance, Traceloop's SDK, which is built on OpenTelemetry, can be added to your application to start capturing this data. You can then add a few lines of code to attach the user_id (which you likely already have in your application's session) as an attribute to your traces.


Q4: Can I set hard budget limits or alerts per user?


A4: Yes. Once you are tracking cost per user, you can build monitoring on top of that data. You can set up dashboard alerts to notify you when a single user's cumulative cost exceeds a certain threshold (e.g., "$50 in 24 hours"), allowing you to automate alerts for performance degradation and cost, so you can proactively investigate or rate-limit that user.


‍


### **Conclusion**


Moving from reactive panic at monthly bills to proactive control of your LLM spend is not only possible but essential. The solution lies in shifting your perspective from the total bill to the individual request. By implementing a FinOps strategy based on granular attribution, tagging every request with a user ID, you gain the visibility needed to diagnose overruns. Leveraging modern observability platforms built on standards like OpenTelemetry gives you the tools to not only see these costs in real-time but to build the controls that prevent budget-breaking surprises.


For more insights and technical guides on LLM observability, you can also explore the[Traceloop blog](https://www.traceloop.com/blog) .


Check out[Traceloop's pricing](https://www.traceloop.com/pricing) and[get started for free](https://www.traceloop.com/) to gain end-to-end observability into your LLM applications. Stop guessing and start debugging.


‍
