---
schema_version: "1.0.0"
document_id: "510549b665973bc40d66b244adb63039841c919572d3899347c5b02b42197f8b"
company_key: "palo-alto-networks-inc-common-stock"
company: "Palo Alto Networks Inc."
source_id: "palo-alto-networks-inc-common-stock-rss-e942a00c1738"
canonical_url: "https://www.paloaltonetworks.com/blog/2026/06/securing-the-agentic-ai-frontier-with-palo-alto-networks-and-databricks/"
published_at: "2026-06-16T13:00:12+00:00"
first_seen_at: "2026-07-25T01:08:05.069426+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:9e9b2d26ed9a9a858c8dde34db09fd3fbd1b2f213b515285af4884588139706a"
---

# Securing the Agentic AI Frontier: Palo Alto Networks and Databricks Deliver a New Standard for AI Security

[AI Security](https://www.paloaltonetworks.com/blog/category/ai-security/)


[Announcement](https://www.paloaltonetworks.com/blog/category/announcement/)


[Databricks](https://www.paloaltonetworks.com/blog/tag/databricks/)


The rise of Agentic AI is rapidly reshaping the enterprise, yet its deployment opens a complex new frontier for cyber threats. As organizations race to harness the power of enterprise agents, the "Data Estate" has become the new perimeter. CISOs today face a high-stakes trade-off: enabling developers to build at the speed of AI while keeping proprietary data visible, governed, and secure across the entire AI lifecycle. This requires meticulously checking user inputs, agent outputs, and tool calls for threats like prompt injections, sensitive data loss, and malicious code, while simultaneously preventing autonomous agents from performing destructive actions.


Securing the AI-driven enterprise requires a fundamental shift from reactive measures to proactive runtime protection. Palo Alto Networks and Databricks are delivering on that vision. Our partnership will integrate the


**Prisma AIRS API** with


**Databricks Unity AI Gateway** , embedding seamless security at runtime. This collaboration will enable organizations to innovate with AI agents, applications, models and MCP Servers at scale while maintaining a robust, policy-driven security posture. By combining the centralized AI governance and control capabilities of the Databricks platform with the runtime security protections of Palo Alto Networks, organizations can scale AI innovation without sacrificing visibility, compliance, or security.


## The Context: Why AI Security is Different


AI security represents a fundamental departure from traditional defense. Legacy tools are designed for structured threats, leaving them incapable of parsing the intent behind complex, conversational attacks.


Furthermore, the integration of Retrieval-Augmented Generation (RAG) and autonomous workflows creates a dynamic attack surface that goes far beyond traditional data loss. Without AI-native oversight, organizations can face severe risks from prompt injections, custom topics, and toxic content manipulating model logic, to tool misuse, malware execution, and malicious URLs hijacking agent actions.


Modern AI development requires more than just a perimeter; it requires contextual intelligence. By integrating Prisma AIRS directly into Databricks Unity AI Gateway, we will evolve security from a reactive layer into a


**native pillar of the AI architecture.**


## The Joint Solution: Centralized Security at the Gateway


The most effective way to secure an entire AI environment is at the governance layer. Our integration focuses on Databricks Unity AI Gateway, which serves as the centralized interface for all AI activity within the Databricks environment. Unity AI Gateway is designed for managing, governing, and monitoring access to all models, agents and MCP Servers—whether they are open-source models deployed within Databricks or external proprietary models. As organizations deploy more agents, applications, and models, centralized governance becomes critical. Unity AI Gateway provides a single control plane for AI usage, enabling teams to apply consistent policies, monitor activity, and manage access across AI workloads.


Through this integration, Unity AI Gateway will make real-time calls to the Prisma AIRS Runtime Security API for security inspection. Instead of managing fragmented security policies across dozens of individual applications, SecOps teams will be able to enforce consistent guardrails across the entire Agentic AI estate from one location, providing a single, unified enforcement point for all AI workloads.


###### Figure 1: Centralized AIRS guardrail configuration delivers instant protection across all applications, agents and MCP Servers without requiring client-side code refactoring


## Mechanism: API Intercept for AI Runtime Security


Prisma AIRS operates as an advanced inspection layer, leveraging its API Intercept capability to provide real-time security embedded directly into the application flow. By embedding Prisma AIRS directly into the workflow, we offer a seamless 'Security-as-Code' experience that unifies development and defense. Prisma AIRS intercepts AI prompts, responses, and MCP calls—inspecting them in real time to enforce security policies with an immediate Go/No-Go verdict or by sanitizing the data in transit. Prisma AIRS uses deep learning classifiers to detect data exfiltration risks, such as the presence of PII (Personally Identifiable Information), PHI, or PCI data. If sensitive data is found, it can be dynamically redacted or blocked based on corporate policy.


## Key Benefits for the Enterprise


This integration isn't just about blocking threats—it’s about accelerating your AI roadmap. By removing the "security friction" that often slows down production deployments, we enable teams to move faster with confidence. Key benefits include:


- **Zero-Friction Governance:** Developers continue working within their familiar Databricks environment. Security is enforced via the Unity AI Gateway API, meaning there are no bulky agents to install and no complex architectural re-wiring required.


- **Prevention of Data Leakage:** Leverage Prisma AIRS’s data classifiers to automatically protect sensitive intellectual property, preventing data leaks to public models and unauthorized users.


- **Resilience Against AI-Specific Attacks:** Protect your Unity AI Gateway deployments from emerging threats that standard network security tools cannot see, including prompt injection, toxic content, custom topics, malware detection and malicious URL detection.


## Key Takeaway


- **Ease of use and unified Policy Management:** Enable runtime security through the Unity AI Gateway to gain centralized control over security enforcement.


- **Audit-Ready Compliance:** Every transaction mediated by the Unity AI Gateway is logged with detailed security metadata, delivering enriched insights in Strata Cloud Manager. This provides the forensic trail required for regulatory compliance in highly governed industries like finance and healthcare.


- **Protection for Agentic Workflows:** Future-proof your multi-step AI agents against sophisticated Agentic Threats by inspecting function and tool calls within the runtime.


## Looking Ahead


As agentic workflows and multi-step model interactions become the standard, a 'fail-closed' runtime security posture is no longer optional; it is foundational. The integration of Prisma AIRS API and Databricks Unity AI Gateway marks a definitive shift toward a future where enterprise AI is secure by default. By integrating Prisma AIRS API with the Databricks platform through Unity AI Gateway, organizations can centrally govern AI across models, agents, applications, and MCP servers while enforcing consistent runtime security policies. Together, Databricks and Palo Alto Networks are helping customers scale AI innovation with the control, visibility, and protection required for the agentic era.


Are you ready to secure your AI workloads and agentic applications?
[check out the latest Databricks blog](https://www.databricks.com/blog/ai-governance-data-ai-summit-2026-whats-new-unity-ai-gateway) and stay tuned for technical deep-dive sessions coming soon.


***Forward-Looking Statements***


*This blog contains forward-looking statements that involve risks, uncertainties and assumptions, including, without limitation, statements regarding the benefits, impact, or performance or potential benefits, impact or performance of our products and technologies or future products and technologies. These forward-looking statements are not guarantees of future performance, and there are a significant number of factors that could cause actual results to differ materially from statements made in this blog. We identify certain important risks and uncertainties that could affect our results and performance in our most recent Annual Report on Form 10-K, our most recent Quarterly Report on Form 10-Q, and our other filings with the U.S. Securities and Exchange Commission from time-to-time, each of which are available on our website at investors.paloaltonetworks.com and on the SEC's website at www.sec.gov. All forward-looking statements in this blog are based on information available to us as of the date hereof, and we do not assume any obligation to update the forward-looking statements provided to reflect events that occur or circumstances that exist after the date on which they were made.*
