---
schema_version: "1.0.0"
document_id: "ff2331ed77e2942a85580aa95633e919e112d56790be325b03692d532f05d8d1"
company_key: "qualys-inc-common-stock"
company: "Qualys Inc."
source_id: "qualys-inc-common-stock-rss-d1030f25037f"
canonical_url: "https://blog.qualys.com/product-tech/2026/07/07/qualys-cisco-cloud-control-studio-launch-partner"
published_at: "2026-07-07T15:00:00+00:00"
first_seen_at: "2026-07-20T03:32:58.378885+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:576dfed4becb556496032d8eea6e00cfddfe027bfbd1aba08651c9451c3e1fc1"
---

# Qualys Joins Cisco Cloud Control Studio as a Launch Partner to Bring Risk Intelligence to Agentic Operations

#### Table of Contents


- The Agentic Imperative: Why This Partnership Matters Now
- What Is Cisco Cloud Control Studio
- What Qualys Brings to the Cisco Cloud Control Ecosystem
- How the Integration Works
- Real-World Example: From SOC Alerts to Risk-Minded Response
- Why This Alliance Matters for Joint Customers
- Get Started
- Frequently Asked Questions (FAQs)


#### Key Takeaways


- Qualys is a launch partner in Cisco Cloud Control Studio, Cisco’s new unified platform for agentic IT operations.
- Joint customers can access Qualys intelligence Unified Asset Inventory, TruRisk Prioritized Findings, and orchestrate remediation workflows directly within Cisco’s AI Canvas environment.
- The integration is powered by Model Context Protocol (MCP) via Cisco Cloud Control Studio.
- Qualys brings three core capabilities to the ecosystem: AI-speed detection, hyper-prioritization and risk validation, and autonomous remediation and zero-day mitigation.
- Together, Qualys and Cisco Cloud Control connect asset and risk intelligence to operational workflows across cloud, network, identity, datacenter, and security domains, eliminating the need to manually switch between tools.


## The Agentic Imperative: Why This Partnership Matters Now


Qualys is proud to be a launch partner in Cisco Cloud Control, Cisco’s new unified platform for agentic IT and security operations, unveiled at Cisco Live Las Vegas 2026. Here’s why this matters.


Frontier AI models are shrinking the gap between vulnerability discovery and active exploitation to minutes. Attack surfaces are expanding across every environment, and the volume of alerts facing security teams has long outpaced what any human team can manage. Security teams are not struggling because they lack data. They are struggling because they have too much disconnected data and not enough context to determine what requires immediate action.


The answer lies in agentic AI: not AI as a search tool or report generator, but intelligent agents that observe, reason, and act at machine speed while keeping humans in the loop where they matter most. Cisco Cloud Control is built for exactly that, with Qualys intelligence at its core.


## What Is Cisco Cloud Control Studio


Cisco Cloud Control Studio is what Cisco describes as the secure harness for the agentic era. It brings together networking, security, AI infrastructure, observability, and collaboration domains into one platform with a unified operating model. Rather than stitching together outputs from a dozen different tools, security and IT teams can investigate, orchestrate, and resolve issues in a single environment, with AI agents working alongside them.


The platform’s integration hub, Cloud Control Studio, connects third-party tools via the Model Context Protocol (MCP). This open standard enables AI agents to securely access data and actions from external systems. Qualys is a launch partner integrated into Cloud Control Studio at general availability.


The workspace where this all comes to life is Cisco AI Canvas, a multiplayer environment where operators and AI agents collaborate on live investigations, correlate signals across domains, and drive resolution together in real time.


Cisco is building this platform on the recognition that IT teams can no longer manage critical infrastructure at human scale. Supporting AI-speed operations requires an agentic foundation. Qualys shares that view, and our participation in this ecosystem reflects where we believe enterprise security operations are heading.


## What Qualys Brings to the Cisco Cloud Control Ecosystem


Qualys has spent more than two decades building the industry’s broadest and deepest asset- and vulnerability-intelligence platform. The platform consolidates asset discovery, vulnerability detection, risk quantification, and automated/autonomous remediation into a unified platform. It is what our customers rely on to understand what they have, what is exposed, what is at risk, and what to do about it across every environment, at scale.


These three capabilities sit at the core of what Qualys delivers and what we bring into the Cisco Cloud Control ecosystem:


- **Unified asset visibility and AI-speed detection:** Qualys continuously discovers and inventories assets across on-premises, cloud, applications, and hybrid environments, while detecting vulnerabilities at the speed AI-powered threats require. Security teams get a real-time picture of their attack surface without gaps or blind spots.
- **Hyper-prioritization and validation of risk:** Not every vulnerability is equal, and not every finding demands immediate attention.[TruRisk](https://www.qualys.com/enterprise-trurisk-platform) scores assets and vulnerabilities based on real-world threat intelligence, active exploitation signals, asset criticality, misconfigurations, attack path, and business context. This gives security teams the confidence to focus on what truly matters rather than wasting cycles on noise. This is what meaningful prioritization looks like at enterprise scale. Leveraging agentic AI workflows with Agent Val and TruConfirm, vulnerabilities are validated with safe payloads.
- **Autonomous zero-day remediation with Qualys Eliminate.** Once a critical vulnerability is confirmed and prioritized, the next challenge is to close it quickly enough. Qualys Eliminate enables autonomous remediation workflows that allow organizations to patch and remediate at machine speed, reducing mean time to remediate and shrinking the window of exposure before attackers can act. **** When immediate patching is not feasible, Qualys can guide compensating mitigation actions to reduce exposure while engineering teams complete permanent remediation.


Together, these three capabilities create a continuous loop: see everything, prioritize intelligently, and remediate autonomously.


## How the Integration Works


Qualys exposes its intelligence to Cisco Cloud Control Studio through a managed MCP server. Cisco’s AI agents discover a set of typed tools at runtime and call them directly to query the Unified Asset Inventory, pull vulnerability detections for a host or workload, retrieve TruRisk scores and exploit context, and trigger remediation or mitigation workflows. Each tool returns structured findings the agent can reason over, delivering context the agent can act on rather than reports that require manual/human interpretation.


Inside a Cisco AI Canvas session, this creates a continuous workflow. When an operator opens an investigation, the agents in that session read from Qualys to answer the questions that determine risk: Is this asset known and inventoried? Is it vulnerable? Is it exposed? How critical is it, and what is the blast radius? They evaluate those answers alongside Cisco signals, including networking telemetry, identity data, and observability. When the risk is confirmed, the same integration allows agents to trigger a Qualys remediation or mitigation workflow, rather than adding another task for the operator to manage across multiple tools.


Because this is a security integration, access is controlled the way a security company would expect. Qualys credentials never enter the agent’s context. Access is scoped per tenant and bounded by read or write permissions, allowing investigation workflows to read-only, while remediation actions require explicit authorization. Every call is logged, providing security teams with a complete audit trail of queries and changes.


Qualys brings the depth of asset visibility, risk quantification, and intelligence. Cisco Cloud Control brings the operational harness to act on it.


## Real-World Example: From SOC Alerts to Risk-Minded Response


When a cloud security alert fires, knowing *something* is suspicious isn’t enough. SOC analysts need answers: Is this workload vulnerable? Is it exposed? What’s the blast radius if an attacker moves fast?


That’s exactly what risk-minded cloud detection and response (CDR) is built for. By combining Cisco’s live security telemetry, network activity, firewall events, and identity context with Qualys’s deep risk intelligence, TruRisk scores, exploit data, asset criticality, and attack paths, teams get a complete, unified picture within a single AI-assisted workflow.


The SOC investigates faster. The risk team remediates smarter. And when a real threat is confirmed, the response goes beyond triage: patching vulnerable packages, replacing affected container images, isolating workloads, or updating firewall policies, all coordinated and prioritized by actual business risk.


The result is fewer alerts sitting in queues, and the most urgent risks are resolved quickly.


## Why This Alliance Matters for Joint Customers


Enterprise security teams have always had a data problem: not a shortage of data, but a surplus of disconnected data. Vulnerability scanners produce findings. Network tools produce alerts. Cloud platforms produce logs. Each system tells part of the story. Correlating that story into a coherent picture of risk and then turning it into action has required enormous manual effort.


Agentic operations platforms like Cisco Cloud Control are designed to close that gap. By connecting specialized tools through a common integration layer and allowing AI agents to reason across multiple data sources simultaneously, they can compress the investigation-to-remediation cycle in ways that were not practical before.


For joint Qualys and Cisco customers, this means the asset visibility and risk-prioritization data Qualys already provides can become an active input into broader operational workflows. Investigations that span network, identity, and security domains without requiring manual export, pivot, or translation between tools.


This is the direction enterprise security operations is heading. Qualys and Cisco are building toward that future together.


## **Get Started**


Qualys is available today in the Cisco Cloud Control Studio Marketplace. If you are a joint customer or want to learn more about how Qualys integrates with Cisco Cloud Control, visit our listing in the[Cisco Cloud Control Studio Marketplace](https://www.cisco.com/site/us/en/solutions/artificial-intelligence/agentic-ops/cloud-control-studio/index.html) to get started.


## Frequently Asked Questions (FAQs)


**What is Cisco Cloud Control Studio?**


Cisco Cloud Control Studio is Cisco’s unified platform for agentic IT and security operations. It brings networking, security, AI infrastructure, observability, and collaboration into one environment where operators and AI agents work together on live investigations and drive resolution in real time.


**How does the Qualys Integration Work?**


Qualys exposes its intelligence through a managed MCP server. Cisco AI agents can query the Unified Asset Inventory, pull TruRisk scores, leverage context, and trigger remediation or mitigation workflows directly within Cisco AI Canvas, all without manual pivoting between tools.


**What Specific Qualys Capabilities are Available in Cisco Cloud Control Studio?**


Joint customers get access to Unified Asset Inventory, TruRisk prioritized findings, safe exploit validation via TruConfirm and Agent Val, and autonomous remediation via Qualys Eliminate, all delivered as structured, actionable context for AI agents.


**What Does This Mean for SOC and Risk Teams?**


It transforms investigations from alert triage to risk-reduction workflows. Teams can validate exposure, map blast radius, identify business impact, and trigger coordinated remediation or mitigation all inside a single AI-assisted environment.
