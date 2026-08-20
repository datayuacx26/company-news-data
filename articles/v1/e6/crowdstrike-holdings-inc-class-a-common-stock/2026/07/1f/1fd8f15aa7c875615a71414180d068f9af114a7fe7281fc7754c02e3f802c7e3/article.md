---
schema_version: "1.0.0"
document_id: "1fd8f15aa7c875615a71414180d068f9af114a7fe7281fc7754c02e3f802c7e3"
company_key: "crowdstrike-holdings-inc-class-a-common-stock"
company: "CrowdStrike Holdings Inc."
source_id: "crowdstrike-holdings-inc-class-a-common-stock-rss-29758b507457"
canonical_url: "https://www.crowdstrike.com/en-us/blog/aidr-how-crowdstrike-is-defining-next-era-of-cybersecurity/"
published_at: null
first_seen_at: "2026-07-20T23:21:19.193454+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:f6013c2debafed830de9a7a7e55431020981ea6a17fd0a308f9dab10f7e06b8d"
---

# AIDR: How CrowdStrike Is Defining the Next Era of Cybersecurity

Every foundational shift in computing has created a new security category. The internet created network security, the rise of workstations created the need for endpoint detection and response (EDR), and cloud computing created the need for cloud security. Each technology transition has moved faster than the one before it. None has moved faster than AI.


The AI threat surface is fundamentally different from anything security teams have defended before, and the defining challenge is the agent: the autonomous system that executes code, calls tools, invokes APIs, accesses credentials, and takes consequential actions at machine speed with the inherited privileges of the human who deployed it. Powerful AI agents like Claude Code, OpenAI Codex, and OpenClaw now[execute on endpoints](https://www.crowdstrike.com/en-us/resources/white-papers/securing-ai-where-it-executes/) .


Figure 1. SaaS, endpoint, and cloud environments form the three coverage planes where AI operates.


With great autonomy comes great risk. Agents can be compromised by adversaries; they can also create unintended security risks and accidentally cause breaches via misalignment. For example, in early 2026, adversaries[targeted ClawHub](https://www.crowdstrike.com/en-us/blog/what-security-teams-need-to-know-about-openclaw-ai-super-agent/) , the community skill registry for OpenClaw, in a supply chain attack that deployed silent data exfiltration payloads to affected agents who used the skill.1


The CrowdStrike Falcon® Adversary OverWatch™ threat hunting team is closely monitoring agent-triggered detection leads, which are now tracking at 2.5 times the rate of human-triggered leads on monitored endpoints. In one instance, the team observed an agent attempt to complete a data-sharing task by sharing sensitive company files via a public file-sharing repository.


Agents are ruthlessly goal-seeking, and we are rapidly entering a world where agents, not humans, will become the majority users of software. These examples of agentic threats and risks are the opening chapter of an AI threat landscape that will define enterprise security for decades to come. The traditional tools that security teams have at their disposal were not built for this moment and do not address what matters most: agent runtime. A prompt injection attack does not appear in a governance report. A compromised agent exploiting inherited credentials may not trigger a data loss prevention rule. Detection without runtime interception is observation, not protection. AI detection and response (AIDR) as a cybersecurity solution category must stop what is going wrong at the moment of execution, before the threat propagates.


### Defining the AIDR Category


AIDR is a unified, runtime security category that involves detecting, investigating, and responding to threats targeting and originating from AI systems across the coverage planes where AI operates: endpoints, SaaS applications, and cloud environments. AIDR represents the new runtime security architecture that the agentic era requires, one that is unified and agent-action-oriented.


Three principles separate a true AIDR platform from adjacent solution categories:


1.


**Runtime security, not posture management:** Posture management operates before and after the action. AIDR operates *during* the execution, inspecting every prompt, tool call, and agent action at the millisecond cadence that live, autonomous threats require.


2.


**Unified platform, not point solutions:** AI agents cross boundaries between endpoint, cloud, and SaaS within a single session. A stack of single-layer point tools generates fragmented signals with no correlation layer to stitch the attack chain together in time.


3.


**Action-oriented control, not observation:** AIDR closes the loop, blocking, redacting, isolating, and revoking at the point of execution.


AIDR must also deliver protection across the seven-layer AI estate: data, models, prompts, agents, identities, infrastructure, and interactions, each of which creates new attack surfaces and security challenges.


Figure 2. Seven layers form the AI estate: data, models, prompts, agents, identities, infrastructure, and interactions.


An AI control plane, the operational core of AIDR, enforces four runtime actions: identity enforcement via continuous authorization, data protection that blocks, redacts, and encrypts, execution controls that stop threats at runtime, and remediation that isolates, contains, and removes. This control plane must cover all seven layers of the AI estate and run across the endpoint, SaaS, and cloud coverage planes.


### What Falcon AIDR Delivers Today


CrowdStrike is pioneering the AIDR category with[CrowdStrike Falcon® AI Detection and Response](https://www.crowdstrike.com/en-us/platform/falcon-aidr-ai-detection-and-response/) , which provides unified visibility, real-time threat detection, data protection, access controls, and automated response capabilities across endpoints, SaaS, and cloud environments to secure both workforce AI adoption and enterprise-developed agents and AI workloads at runtime. Its capabilities include:


- **Securing workforce AI use:** Falcon AIDR illuminates shadow AI by discovering employee agent and tool usage across endpoints, AI gateways, MCP servers, cloud environments, and Copilot ecosystems. Organizations can use Falcon AIDR to enforce granular attribute-based access controls to align with AI governance policy, such as policy based on user and model version. Falcon AIDR also automatically detects and blocks confidential information, personally identifiable information (PII), secrets, keys, and regulated data before exposure and also offers multiple redaction methods including format-preserving encryption to protect data while preserving AI productivity.
- **Securing AI development at runtime:** Falcon AIDR stops prompt injection and jailbreak attacks across more than 200+ attack techniques that CrowdStrike tracks in the industry’s most comprehensive[adversarial prompt taxonomy](https://www.crowdstrike.com/en-us/resources/infographics/taxonomy-of-prompt-injection-methods/) . It validates MCP server communications to prevent unauthorized tool execution, detects malicious URLs, domains, IP addresses, and entities within AI responses in real time, and generates comprehensive audit trails of AI runtime behavior for compliance, forensics, and continuous improvement. Findings stream directly to CrowdStrike Falcon® Next-Gen SIEM for unified security operations and cross-domain correlation.


Figure 3. A Falcon AIDR visibility screen shows users, AI applications, and underlying models being used in an environment as well as token consumption statistics.


### What’s Coming Next


The most consequential frontier in AI security is the endpoint, where powerful agents execute. Process-level actions, tool calls, file operations, and MCP invocations occur on the host, largely below the visibility threshold of network and application-layer controls. There is only one vantage point from which to observe, govern, and act on this activity in real time: the endpoint.


CrowdStrike will extend Falcon AIDR via deep integration with the Falcon sensor, the same kernel-level sensor that has protected endpoints from adversaries for over a decade, to detect, identify, and monitor AI agents at the OS level. This will not require third-party vendor relationships, custom SDK integrations, or new deployment motion.


Figure 4. Falcon AIDR view of an enterprise’s agentic deployment with high-risk agents highlighted in red


This fusion will unlock powerful new capabilities to discover and secure AI agents at runtime, including:


- **A live inventory of the entire agent fleet:** A real-time map of every AI agent running across the enterprise — Claude Code, Cursor, Copilot, Kiro, Codex, and unclassified shadow agents — will show exactly where each is deployed, who is operating it, and its active security risk status. At enterprise scale, this sea of legitimate activity becomes the perfect camouflage for an adversary.
- **Agent risk scoring and behavioral detection:** Correlation of multiple indicators across the agent execution chain to surface behavioral anomalies and assign risk scores that will give analysts immediate clarity on where to focus. When a high-risk agent is identified, analysts will be able to view the full attack path by connecting the AI agent execution chain and the OS process tree into a single causal graph, linking every prompt to every downstream OS effect such as file reads or credential access. An analyst can pivot on problematic elements of the graph, such as a compromised skill, to understand the agentic blast radius and take remedial action.
- **From investigation to enforcement at machine speed:** From the same console, the analyst can move directly from discovery to response, deploying a global block rule, for example, that immediately prevents the malicious skill from executing anywhere in the environment, containing current exposure and blocking future risk in a single action.


*This capability is currently pre-beta and will go to GA next quarter (Q3).*


### Why CrowdStrike


The AIDR market is forming rapidly. CrowdStrike’s competitive advantage is structural, built over a decade of endpoint investment and extended through deliberate expansion into cloud and SaaS coverage planes.


The Falcon sensor is deployed on hundreds of millions of endpoints worldwide. When AI agents became a security challenge, CrowdStrike’s existing telemetry infrastructure already observed the relevant process-level behaviors. No competitor can currently replicate this endpoint footprint in a meaningful competitive time frame, and this asymmetry compounds with every additional AIDR capability added to the CrowdStrike Falcon platform.


CrowdStrike’s prompt injection taxonomy will continuously grow as Falcon Adversary OverWatch tracks adversary tradecraft in AI environments. One customer’s agent anomaly becomes every organization’s defense. By the time competitors build the sensor footprint to collect comparable telemetry, CrowdStrike will have built years of behavioral baselines that others cannot retroactively replicate.


With CrowdStrike, AIDR runs on one platform, one sensor, and one console. For organizations already running the Falcon platform, it is not a new vendor relationship or a new deployment motion. It is the next capability of a platform already in production, extending the same sensor that stops adversaries today to secure AI agents at runtime.


For more information:


- *Download the white paper:[AIDR: Defining the Next Era of Cybersecurity](https://www.crowdstrike.com/en-us/resources/white-papers/aidr-defining-the-next-era-of-cybersecurity/)*
- *Watch the[AIDR CrowdCast](https://www.crowdstrike.com/en-us/resources/crowdcasts/aidr-defining-the-next-era-of-cybersecurity/)*
- *Schedule a[Falcon AIDR demo](https://www.crowdstrike.com/en-us/platform/falcon-aidr-ai-detection-and-response/demo/)*
- *Register for[Fal.Con 2026](https://www.crowdstrike.com/en-us/events/fal-con/las-vegas/?utm_medium=evt&utm_source=blog&utm_campaign=fal-con-26&utm_term=crwdblogs&utm_language=en-us)*


#### Forward-Looking Statements


This blog may include discussion of unreleased services or features. Any unreleased services or features referenced here are still in development and subject to change. Customers should make their purchase decisions based upon features that are currently available.


1[https://www.crowdstrike.com/en-us/blog/what-security-teams-need-to-know-about-openclaw-ai-super-agent/](https://www.crowdstrike.com/en-us/blog/what-security-teams-need-to-know-about-openclaw-ai-super-agent/)
