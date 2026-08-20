---
schema_version: "1.0.0"
document_id: "aebe1699a1909e95383f5de6e4f9024afe1ab4e5274526adc7733a21fd0f5bce"
company_key: "dynatrace-inc-common-stock"
company: "Dynatrace Inc."
source_id: "dynatrace-inc-common-stock-rss-2f172b160f47"
canonical_url: "https://www.dynatrace.com/news/blog/dynatrace-intends-to-acquire-arize/"
published_at: "2026-08-13T10:00:21+00:00"
first_seen_at: "2026-08-13T10:35:03.277668+00:00"
fetched_at: "2026-08-13T10:35:04.334959+00:00"
content_hash: "sha256:a79ea5781ce7c41b60e270695f7443041f76ad7196703efc6cfcd21f8cc30455"
---

# Dynatrace and Arize bring full-lifecycle observability to AI applications

The combination of Dynatrace and Arize will bring together AI-native evaluation, open AI instrumentation, and unified observability to help organizations evaluate, operate, and continuously improve AI applications from development through production at enterprise scale.


Organizations are deploying AI-powered applications and agentic systems across customer experiences, employee workflows, and critical business processes. Getting them into production is only part of the challenge. Organizations need to know whether they are accurate, reliable, cost-efficient, and delivering the outcomes they were built to achieve.


Enterprise AI applications require observability across the full lifecycle, connecting how AI systems are built and evaluated with how they perform in production and the outcomes they deliver.


AI engineering teams use AI-native evaluation tools to trace AI applications, inspect model and agent behavior, run evaluations, compare experiments, evaluate model and agent behavior, investigate failures and improve quality. Platform Engineering, SRE, and operations teams use a different set of tools to monitor the applications, infrastructure, user experiences, and business processes that support those AI systems.


These disciplines have evolved separately. This separation creates a visibility gap as enterprises move more AI applications into production. In a recent[Dynatrace study](https://www.dynatrace.com/news/blog/agentic-ai-report-reliable-autonomous-operations/) of 919 agentic AI leaders, 51% cited technical challenges in managing and monitoring agents at scale as a top barrier to production, and 45% said they lack clear rules for when agents can act autonomously versus when humans need to intervene.


That’s why Dynatrace has signed a definitive agreement to acquire[Arize](https://www.dynatrace.com/news/press-release/dynatrace-intends-to-acquire-arize/) .


## AI applications do not operate in isolation


AI systems fail differently from traditional software.


A misconfigured application often produces an error that can be seen, traced, and reproduced. An AI application can be available without being accurate, respond quickly without being useful, or generate a confident, plausible-sounding output that is biased, inaccurate, irrelevant, unsafe, or misaligned with the task. An agent can complete a workflow while taking the wrong path, and a model can perform well during development but behave differently as data, prompts, dependencies, or user behavior change in production – often without triggering a corresponding infrastructure alert or application error.


The underlying issue may not always sit within the model itself.


An agent that passed pre-production evaluation could fail in production because of latency in an upstream service, a change in a data source, a degraded retrieval result, a tool failure, an infrastructure constraint, or an application dependency that was not present during development.


Dynatrace research found that 42% of organizations have limited real-time visibility to trace and troubleshoot agent behavior, and 44% still rely on manual methods to review communication flows among agents.


In another case, an AI application may function as designed from a technical perspective but produce an unacceptable customer experience, excessive cost, or a poor business outcome.


Today, the signals needed to understand these situations often live in separate systems and are owned by different teams. AI engineers can see model and agent behavior through traces, evaluations, experiments, and other AI-native signals. Platform Engineering, SRE, and operations teams monitor application, services, infrastructure, and processes that support those AI systems. Business teams can see the eventual outcome.


What’s often missing is the shared context between them across the lifecycle.


## Bringing together complementary strengths


Arize gives AI engineering teams a way to understand, evaluate, and improve AI applications, agents, and agentic workflows.


[Phoenix](https://github.com/Arize-ai/phoenix) , Arize’s open-source AI observability and evaluation platform, helps teams trace AI applications, run evaluations, investigate failures, compare experiments, and improve quality during development and model iteration. It gives developers visibility into model calls, retrieval, tool use, agent decision paths, custom logic, and output quality as they build and refine AI applications and agentic systems.


[Arize AX](https://arize.com/docs/ax) extends that foundation into a managed enterprise platform for AI observability, evaluation, and continuous improvement. It gives teams advanced agent observability, online evaluations, monitoring, and workflows to detect hallucinations, measure output quality, compare changes, and continuously validate AI systems from experimentation through production. AX also helps teams turn production signals into better AI systems through datasets, experiments, prompt workflows, human feedback, and AI-assisted investigation.


Dynatrace brings the enterprise observability, production resilience, and data foundation that AI workloads depend on.


Our platform provides unified observability across applications, infrastructure, AI infrastructure, model performance, user experience, and business outcomes. It gives Platform Engineering, SRE and operations teams the context to determine whether issues stem from the model, application, infrastructure, or dependencies, understand downstream impact, and act.


Dynatrace also provides the data foundation to manage AI telemetry at enterprise scale across distributed cloud environments and operate AI-powered services with reliability and efficiency.


After the acquisition closes, Dynatrace customers are expected to gain continuous coverage across the AI delivery lifecycle. Dynatrace expects the combined capabilities to help teams connect evaluation and experimentation in development with deployment validation, runtime evaluation, production observability, and automated feedback loop. Agent evaluation and tracing will sit alongside the application, infrastructure, user experience, and business observability Dynatrace already provides.


The combined capabilities are expected to help teams:


- **Experiments with production behavior:** Teams can compare how a model, prompt, retrieval strategy, or agent workflow performs in development with how it behaves under real production conditions.
- **Traces with full-stack context:** When an AI output degrades, teams can investigate whether the cause sits in the model, prompt, retrieval layer, agent workflow, tool call, application dependency, or infrastructure layer.
- **Evaluations with operational signals:** Quality scores, hallucination checks, human feedback, and regression tests can be understood alongside latency, errors, throughput, resource utilization, and cost.
- **AI workloads with the infrastructure supporting them:** Latency in an upstream service, GPU saturation, a changed data source, or an infrastructure constraint becomes visible as a contributing factor, not a blind spot.
- **Agent workflows with customer and business outcomes:** When an agent completes or fails a workflow, that event connects to a measurable business result, not just a telemetry trace.
- **Production signals with engineering improvement cycles:** When live behavior reveals a problem, that signal feeds directly back into the evaluation workflow, closing the loop between operations and the teams responsible for improving the AI system.


Teams will be able to understand what happened, why it happened, what it affected, what it cost, and how to improve it.


## Built on community and open standards


Arize’s value extends beyond its technology. Through Phoenix, AX, and[OpenInference](https://github.com/Arize-ai/openinference) , it has earned the trust of thousands of AI engineering teams and helped shape how AI applications are evaluated, instrumented, and observed.


That matters because AI technology decisions increasingly begin with the practitioners building these systems. Developers often adopt and evaluate tools before those technologies become broader enterprise platform decisions.


OpenInference is central to that work. Created and maintained by Arize, OpenInference is an open specification for AI tracing built on[OpenTelemetry](https://opentelemetry.io/) . It gives organizations an open, standards-based way to instrument AI applications without relying on proprietary instrumentation while helping advance a common foundation for AI observability.


Dynatrace intends to support Phoenix and contribute to the continued stewardship of OpenInference. That commitment is consistent with Dynatrace’s long-standing engagement with open standards and open ecosystems, including its contributions to OpenTelemetry and[OpenFeature](https://openfeature.dev/) . Open instrumentation is part of how modern observability advances, and we expect it to remain central to how AI observability evolves.


AI engineers can start with Phoenix today without vendor lock-in and scale to the full Dynatrace platform as their needs grow. The same OpenInference instrumentation works across both.


## To Arize customers, AX users, and the open-source community


To Arize customers and AX users, we recognize the trust you have placed in the platform, its capabilities, and the team behind it.


You rely on Arize to understand what your AI applications are doing, whether they are behaving as expected, and improve quality over time. By bringing Arize’s AI-native evaluation capabilities together with the Dynatrace enterprise observability platform, Dynatrace expects to extend that visibility across the complete AI lifecycle and the full stack beneath it.


To the Phoenix and OpenInference communities, we recognize that openness is fundamental to what Arize has built.


Our intention is to support that open path, back Phoenix, and contribute to the continued stewardship of OpenInference.


After the acquisition of Arize closes, both will continue to operate independently. Nothing changes immediately for customers, partners, developers, or community members. Existing products, relationships, and commitments remain in place.


## Defining the future of AI observability


For traditional software, observability connects code, applications, infrastructure, users, and business outcomes. AI introduces another dimension: systems that reason, retrieve, call tools, and make decisions across multi-step workflows.


Understanding those systems requires visibility into what the model or agent did, why it did it, how the systems around it behaved, what it cost, and what happened as a result.


Bringing Arize’s AI-native evaluation capabilities together with Dynatrace observability is intended to provide that visibility across the AI lifecycle, from development through production and continuous improvement.


That is the next frontier for observability: connecting AI behavior, evaluation, production context, business outcomes, and continuous improvement.


#### Cautionary Language Concerning Forward-Looking Statements


This blog includes certain “forward-looking statements” within the meaning of the Private Securities Litigation Reform Act of 1995, including statements regarding the expected benefits of the proposed acquisition, capabilities expected to be available to organizations from using Dynatrace and Arize following the closing of the proposed acquisition, expected future growth in the AI Observability market segment and its expected size in 2030, the expected timing for closing of the proposed acquisition, Dynatrace’s plans to fund the proposed acquisition, and the expected financial impact of the proposed acquisition. These forward-looking statements include all statements that are not historical facts and statements identified by words such as “will,” “expects,” “anticipates,” “intends,” “plans,” “believes,” “seeks,” “estimates,” and words of similar meaning. These forward-looking statements reflect our current views about our plans, intentions, expectations, strategies, and prospects, which are based on the information currently available to us and on assumptions we have made. Although we believe that our plans, intentions, expectations, strategies, and prospects as reflected in or suggested by those forward-looking statements are reasonable, we can give no assurance that the plans, intentions, expectations, or strategies will be attained or achieved. Actual results may differ materially from those described in the forward-looking statements and will be affected by a variety of risks and factors that are beyond our control, including our ability to successfully complete the Arize acquisition and integrate the newly acquired business and offerings, the risks set forth under the caption “Risk Factors” in our most recent Annual Report on Form 10-K, subsequent Quarterly Reports on Form 10-Q, and our other SEC filings. We assume no obligation to update any forward-looking statements contained in this document because of new information, future events, or otherwise.
