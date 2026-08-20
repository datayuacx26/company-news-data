---
schema_version: "1.0.0"
document_id: "9e9d48b53eb13faadafaa552901e1a7b4d4dfd8e425243778bf07643115c23e9"
company_key: "yc-mezmo"
company: "Mezmo"
source_id: "yc-mezmo-news-import-4bb0a7224006"
canonical_url: "https://www.mezmo.com/newsroom/how-mezmo-cuts-ai-observability-costs-by-90-with-context-engineering-tucker-callaway"
published_at: null
first_seen_at: "2026-07-22T04:13:37.000074+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:39055ebcfbf4e426b3f1eb16163905037aec735d111d3fc6eff5d42174bb5978"
---

# How Mezmo Cuts AI Observability Costs by 90% With Context Engineering | Tucker Callaway

*Tucker Callaway, CEO of Mezmo, explains how context engineering achieves 90% cost savings in AI observability without training models. Learn about headless observability.*


AI promises to transform[observability](https://tfir.io/tag/observability/) and free[SREs](https://tfir.io/tag/sres/) from endless incident firefighting. But most approaches demand lengthy model training, massive data volumes, and costs that spiral as quickly as the problems they’re meant to solve.[Tucker Callaway](https://www.linkedin.com/in/tucker-callaway-9310171) , CEO of[Mezmo,](https://www.mezmo.com/) has a different answer: stop training models and start engineering context.


At[KubeCon and CloudNativeCon in Atlanta](https://events.linuxfoundation.org/kubecon-cloudnativecon-north-america/) , Callaway outlined how Mezmo is challenging the conventional wisdom around AI-driven observability. Rather than following the industry trend of training custom models on customer data, Mezmo takes what Callaway calls a “context engineering” approach. The company processes, embeds, and vectorizes telemetry data offline before feeding it to foundation models. This preprocessing dramatically reduces the volume of data sent to the LLM—by 90%—while simultaneously improving accuracy and speed.


“We looked at the ‘train the model’ approach, and it just took too long, and the accuracy wasn’t there, and the cost was too high,” Callaway explained. “We scrapped that approach about six months ago and started looking at what are the bottlenecks inside of the LLM that are causing accuracy problems.”


The result is an AI SRE solution that customers can spin up in five minutes without training, without sharing their data for model fine-tuning, and at a fraction of the cost. Mezmo is so confident in the efficiency gains that the company plans to roll out its AI SRE agent to all 3,500 weekly active users at no additional charge.


## **Detection, Diagnosis, Remediation**


Callaway is particular about terminology. He doesn’t love the term “AI SRE” because it suggests replacing a multifaceted job role with software. Instead, he frames Mezmo’s solution around three core functions: detection, diagnosis, and remediation of incidents.


“Our focus is really on the diagnosis, because we believe that you really have to deeply trust the diagnosis if we’re ever going to get to automation of the remediation and closing that full loop,” Callaway said. The goal isn’t to eliminate SREs but to free them from incident management minutiae so they can focus on designing, scaling, and building systems.


By offloading the majority of inference work—what Callaway describes as 80% deterministic processing—Mezmo drastically reduces the probabilistic nature of LLMs. This minimizes hallucinations, context confusion, and context poisoning. The model spends its attention on output and accuracy rather than parsing massive volumes of raw telemetry data.


## **The Path to Headless Observability**


When asked about the future of observability, Callaway painted a provocative picture. He predicts the industry will move toward “headless observability” where dashboards and investigatory UIs become largely irrelevant.


“Today, the process is we identify an incident, and then we go through an investigative process in a UI trying to find the root cause,” he said. “All that’s going to go away in the years to come. There’s really no purpose in having this visualization layer in observability.”


Instead, analysis capabilities will happen agentically, and remediation will occur automatically. This shift is accelerated by parallel disruption in how applications are built. As AI-driven coding and agentic development transform the application layer, the combination of smarter apps and autonomous observability could arrive faster than most people expect.


## **Cost, Complexity, and the Data Problem**


Kubernetes and cloud environments are notorious for complexity and cost overruns. Callaway acknowledged that left unchecked, AI observability could make both worse. The answer, he argues, is sophisticated data management.


Mezmo’s real-time data pipelining foundation allows it to structure, normalize, and focus on what Callaway calls “active telemetry”—the data needed to drive outcomes. Everything else gets archived in cold storage like an S3 bucket. This agentic data engineering happens behind the scenes, keeping the overall system simple and cost-effective.


“We can make this very complex, or we can make this very simple,” Callaway said. “The answer to that is really a data management problem.”


## **Customers and Feedback**


Mezmo has been working with design partners and beta testers ahead of the KubeCon announcement. Callaway said the feedback has centered on one thing: speed to value. Customers struggled with the long onboarding cycles and training requirements of traditional AI observability tools. Mezmo’s approach, which requires no training and delivers value within an hour, has been transformative.


“People can spin it up in a morning and within an hour be getting value out of it and finding new outcomes very easy to get to,” Callaway said.


The solution is available through Mezmo’s UI, via MCP server integration, and within native customer environments. The company is not targeting any specific industry or workload, focusing instead on the SRE role and the universal challenge of managing complex systems efficiently.


Tucker Callaway’s vision for observability isn’t just incremental improvement. It’s a fundamental rethinking of how telemetry data is processed, how intelligence is applied, and what role humans play in keeping modern infrastructure running. If context engineering delivers on its promise, the dashboards SREs spend hours staring at today may soon become relics of a different era.


‍
