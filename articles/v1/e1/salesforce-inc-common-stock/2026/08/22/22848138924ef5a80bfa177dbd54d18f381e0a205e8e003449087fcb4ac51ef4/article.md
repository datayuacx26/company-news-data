---
schema_version: "1.0.0"
document_id: "22848138924ef5a80bfa177dbd54d18f381e0a205e8e003449087fcb4ac51ef4"
company_key: "salesforce-inc-common-stock"
company: "Salesforce Inc."
source_id: "salesforce-inc-common-stock-rss-5706a1a21901"
canonical_url: "https://engineering.salesforce.com/how-agentforce-powered-ai-security-workflows-accelerate-incident-response/"
published_at: "2026-08-12T19:28:41+00:00"
first_seen_at: "2026-08-12T20:14:20.928560+00:00"
fetched_at: "2026-08-12T20:14:21.979763+00:00"
content_hash: "sha256:045f160adfa043e943dcdf2e7ef4a610e36ed89dbd909402c217f4b25dbf71aa"
---

# How Agentforce-Powered AI Security Workflows Accelerate Incident Response

In our Engineering Energizers Q&A series, we highlight the engineering minds driving innovation across Salesforce. Today, we spotlight Zac Taylor, a software engineering architect on the Trusted Services team. Zac and his team are building Security Center, an[Agentforce](https://engineering.salesforce.com/how-agentforce-runs-secure-ai-agents-at-11-million-calls-per-day/) -powered visibility tool that helps accelerate AI-powered security investigations and remediation workflows. The latest release accelerated engineering velocity by 2–3X while enabling security teams to burrow deeper into telemetry, rapidly chase anomalies across distributed systems, and scale AI-driven security playbooks.


Explore how the team expanded Security Center’s AI capabilities from a standalone conversational interface into a stateful investigations platform capable of managing long-running security incidents across multiple telemetry systems, along with how the team addressed the challenge of maintaining accurate, trustworthy AI-driven security responses across non-deterministic LLM workflows.


##### **What is your team’s mission in building an AI-powered agent, and how does it extend Security Center’s capabilities for managing security operations?**


The mission is to enhance Security Center with AI-powered capabilities to help organizations investigate threats, respond to incidents, and strengthen Salesforce-specific security operations. Many organizations already run sophisticated infrastructure security programs, but Salesforce environments introduce unique operational and[telemetry](https://engineering.salesforce.com/how-standardizing-product-telemetry-reduced-time-to-insight-by-97/) challenges that traditional security tooling does not address effectively.


With[Agentforce](https://engineering.salesforce.com/how-agentforce-converts-llm-responses-into-structured-ui-for-ai-agents-across-4m-sessions/) acting as a Salesforce-native security analyst, customers can investigate suspicious activity, review relevant telemetry, and receive remediation guidance through natural language instead of manually navigating distributed security tooling and operational dashboards.


AI makes it possible to summarize large volumes of telemetry and investigation context into concise, actionable responses, giving organizations greater visibility, stronger layered protection, and more adaptive incident response playbooks across increasingly complex telemetry environments.


##### **After Security Center became powered by Agentforce, how did the agent expand its capabilities to support end-to-end security investigations?**


We first focused on delivering a conversational AI experience where customers could ask questions about security findings and telemetry. Very quickly, it became clear that real incident-response systems require significantly more than a standalone chat interface.


[Security](https://engineering.salesforce.com/scaling-trust-how-salesforces-security-team-uses-agentforce-to-triage-security-reports-at-speed/) investigations behave more like long-running operational systems than transient conversations. A single incident can span triage, investigation, escalation, communication, and resolution that span rapidly changing operational game scripts. That pushed the team toward building a broader investigations platform with investigation lifecycle management, remediation tracking, auditability, and visualization tools.


The result was investigation-focused workflow hubs and state-management capabilities that allow customers to manage incidents end to end across evolving remediation flows. Conversational AI alone is not sufficient for enterprise-grade incident response across complex telemetry jungle environments with today’s rapidly evolving threat landscape.


##### **What made non-deterministic LLM behavior difficult to validate consistently at scale for AI-powered security investigations?**


LLM-driven systems break traditional deterministic testing because identical prompts can generate different responses across executions. That becomes especially pertinent in security workflows, especially under the high-pressure conditions that often arise during active incidents.


To solve this, the team built AI-driven evaluation pipelines where AI systems validate the behavior of other AI systems. Large collections of simulated customer interactions are fed into the agent, and LLM evaluators assess whether responses align with the intended investigative outcome and operational expectations. Critically, those evaluators do not validate exact wording. Instead, they assess whether the response satisfies the intended investigative behavior while remaining accurate and contextually grounded through more adaptive read-and-react evaluation strategies.


The impact was significant. AI-generated testing increased throughput by roughly 10–20X while enabling continuous evaluation of new investigation scenarios, prompt variations, and customer interaction patterns that would be impractical to validate manually across traditional telemetry samples, retrieval pipelines, and broader investigation coverage models.


##### **What made balancing investigative depth, response accuracy, and trustworthy AI reasoning difficult in real-world security investigations?**


One of the hardest engineering problems was balancing conversational flexibility with highly precise security reasoning. Security investigations quickly evolve from broad questions into deep operational analysis, where customers may start by asking about suspicious activity, then progressively burrow deeper into specific APIs, telemetry patterns, user actions, or remediation history tied to the investigation.


Maintaining coherent reasoning across those workflows is difficult because LLM context windows are finite and investigations often span multiple telemetry systems with large volumes of data. Large investigations can trigger reasoning pocket collapse scenarios where critical operational context falls outside the active context window.


Hallucination mitigation became equally important. Inaccurate security investigations can overstate blast radius, generate false positives, or erode operational trust.


The team addressed this through careful prompt engineering, structured action routing, controlled data-source selection, and rigorous automated evaluation strategies. Continuous refinement was also necessary because customers frequently interact with[Agentforce](https://engineering.salesforce.com/how-agentforce-enabled-conversational-recommendations-with-ai-driven-intent-on-data-360/) , using ambiguous language, unexpected terminology, or unforeseen questions.


##### **What architectural challenges emerged when combining security telemetry, remediation workflows, and multi-system investigation data?**


One of the largest architectural challenges was handling the wide array of security signals and telemetry systems across Salesforce. Security investigations can originate from many different internal systems, each with different data models, telemetry formats, remediation workflows, and operational implications. Session hijacking incidents, API anomalies, suspicious user behavior, and other threat categories all carry different remediation requirements and security context. To mitigate this, the team designed extensible investigation data models capable of handling disparate telemetry formats and multiple investigation categories without tightly coupling the architecture to specific threat types.


Aggregating telemetry from heterogeneous security systems while staying within LLM context window constraints introduced a separate challenge. Investigation telemetry grows extremely large very quickly, making it impractical to pass raw data directly into the model. The team addressed this by partitioning telemetry into smaller contextual segments and using AI-driven summarization pipelines to compress investigation state into more manageable representations before feeding it into the agent.


The broader architecture relied on orchestration layers and retrieval pipelines coordinating workflow state across distributed remediation systems, with strong layered protection schemes capable of handling operational edge pressure while maintaining consistent telemetry coverage. Coordinated workflow handoffs, intelligent fallback paths, adaptive remediation route trees, and increasingly dynamic operational cadence were all required to support active investigations at scale.


##### **What challenges still remain in grounding AI security responses with Salesforce-specific remediation and investigation knowledge?**


We are continuing to improve Salesforce-specific grounding. Current agentic workflows rely heavily on publicly available foundation models. Those models carry some Salesforce security awareness derived from public internet content. However, they do not inherently understand Salesforce-specific remediation logic, security operations workflows, or platform-specific threat models. That gap becomes increasingly important as the agent expands into more advanced remediation planning and autonomous investigation workflows where customers expect highly accurate, Salesforce-specific guidance.


To address this, the team is working closely with the Setup Agent organization to build stronger grounding systems capable of injecting curated, Salesforce-owned security knowledge directly into the agent context.


The long-term vision is that Security Center helps organizations manage investigations, remediation, and operational decision-making directly inside Salesforce — with battle-tested orchestration patterns, faster incident containment, adaptive defensive postures, and more intelligent investigation reasoning across evolving security environments.


##### **Learn more**


- Stay connected — join our[Talent Community](https://flows.beamery.com/salesforce/eng-social-2023) !
- Check out our[Technology and Product](https://www.salesforce.com/company/careers/teams/tech-and-product/?d=cta-tms-tp-2) teams to learn how you can get involved.
