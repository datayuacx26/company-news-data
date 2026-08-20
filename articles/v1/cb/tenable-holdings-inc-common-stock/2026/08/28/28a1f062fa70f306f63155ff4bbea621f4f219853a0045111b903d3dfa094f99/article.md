---
schema_version: "1.0.0"
document_id: "28a1f062fa70f306f63155ff4bbea621f4f219853a0045111b903d3dfa094f99"
company_key: "tenable-holdings-inc-common-stock"
company: "Tenable Holdings Inc."
source_id: "tenable-holdings-inc-common-stock-news-import-a6b69c49a265"
canonical_url: "https://www.tenable.com/blog/tenable-hexa-ai-automating-exposure-remediation-with-agentic-routines"
published_at: "2026-08-05T12:45:00+00:00"
first_seen_at: "2026-08-05T16:02:51.784766+00:00"
fetched_at: "2026-08-05T16:02:53.378895+00:00"
content_hash: "sha256:0b081b977dfb34c0d625e109f9f43a8bd7be70acb4ac09b62ed2097fce56b971"
---

# Watch Tenable Hexa AI automate remediation with agentic routines

Discover how Tenable Hexa AI closes the gap between exposure management and endpoint patching using intent-driven routines, smart guardrails, and human approval.


## **Key takeaways**


1. **The problem:** A slow handoff between security workflows creates a days-long remediation gap.


2. **The solution:** Tenable Hexa AI bridges this gap using intent-driven Routines that automate scoping, deployment, and verification across integrated platforms like Jamf.


3. **Safety and control:** Autonomy is governed by the harness built into Tenable One, ensuring the AI operates strictly within defined user permissions and guardrails.


Find the exposure. Fix it. Confirm it is gone.


Those three steps are rarely executed in a single place, by a single team. Exposure management knows which assets are at risk, while endpoint management actually changes the machine and applies the fix.


Between those two domains of exposure identification and remediation lies a slow, manual handoff and multi-step routine that costs security teams days or weeks while vulnerabilities remain exposed:


1. Scope the asset group
2. Aim the remediation policy
3. Execute patch deployment
4. Check status
5. Re-scan the environment to confirm the finding was closed.


[Tenable Hexa AI](https://www.tenable.com/products/tenable-one/capabilities/hexa-ai) , the agentic engine of the[Tenable One Exposure Management Platform](https://www.tenable.com/products/tenable-one) , now spans that handoff, so you don’t have to manually toggle among tools or continually restart the conversation.


## How does Tenable Hexa AI autonomously close the remediation loop


Say there’s an actively exploited Chrome vulnerability, and a fleet of your Macs is still running the vulnerable version. Rather than navigating multiple tools, you simply tell Tenable Hexa AI to patch it. Tenable Hexa AI executes the workflow in three unified stages:


1. **Enumeration and mapping** - Tenable Hexa AI enumerates the affected assets across your exposure sources and resolves them to the devices your endpoint team already manages in Jamf.
2. **Policy identification** - Tenable Hexa AI maps the CVE to the version that fixes it, then finds the Jamf patch definition that delivers that version.
3. **Proposal presentation** - Before writing any changes, Tenable Hexa AI stops and shows you a proposal detailing the number of devices, which devices, what policy, and what the deployment window looks like. It highlights the blast radius and it tells you plainly that the action does not roll itself back.


If you need to narrow enumeration to one business unit, just tell Hexa and it will re-scope and return a new plan before executing any changes.


Once you approve, Tenable Hexa AI creates a static group in Jamf holding exactly the devices you approved, then triggers the policy against it. Ask Hexa for status at any point, and it returns the rollout device by device, without you leaving the chat window. Tenable Hexa AI then schedules a re-scan for after the patch deployment window.


What previously took days across multiple tools now takes minutes within a single conversation, and all you need to do is make one decision rather than coordinate the manual execution of multiple, complex steps.


## Hand off recurring security work to intent-driven routines


The ultimate goal of agentic AI for security is to help security teams efficiently and effectively scale cyber defense by taking on complex manual routines. To carry out vulnerability remediation and validation routines with Tenable Hexa AI, you define three core elements in plain, natural language:


1. **An objective** - State what you are trying to achieve, in the words you would use with a colleague, not a sequence of steps (e.g., “Triage and patch critical vulnerabilities across MacOS endpoints”).
2. **Guardrails** - Set explicit operational limits (e.g., “Never launch a credentialed scan against anything tagged OT,” or “Open no more than twenty-five tickets in a run”).
3. **A cadence** - Choose whether to run the routine on demand or tell Hexa to run it on a specific schedule.


Tenable Hexa AI drafts the plan using capabilities discovered from the tools you have already connected to Tenable One. If the objective is ambiguous, Tenable Hexa AI asks for clarification instead of guessing, and if the objective and the guardrails are at odds, it tells you rather than quietly dropping one.


Because routines in Tenable Hexa AI are intent-driven and not step- or script-driven, they adapt seamlessly to real-world infrastructure changes, such as new asset classes, renamed tags, or failed scans that require a restart. Scripts break when conditions change; intent survives contact with reality.


## Controlling AI autonomy in Tenable Hexa AI


Trust is not something you hand off blindly to an agentic security product. You extend it one job at a time, and it grows through demonstrated reliability.


Routines allow security teams to scale autonomy at their own pace through the following attributes:


- **Defined scope** - The routine’s objective sets what Tenable Hexa AI owns, while guardrails enforce strict boundaries. Write a narrow routine and Hexa works narrowly; widen it and Hexa widens with you.
- **Isolated decisions** - Guardrails are scoped per routine so you never have to make an all-or-nothing decision for the entire platform.
- **Human-in-the-loop gates** - You retain total control to approve, narrow, widen, or cancel proposals as many times as you want, before any action is written to your production environment.


So the pace is yours. Start by handing off a routine weekly report, then move to automated overnight triage once you have watched the report run successfully a few times. Expand autonomy one routine at a time as you build confidence in the agentic engine. Tenable Hexa AI moves as fast as you let it.


## Agent Center, the home page for agentic activity in Tenable One


Autonomy you cannot see is not autonomy you can inherently trust. Agent Center serves as the primary dashboard for agentic activity within Tenable One. It answers four critical operational questions the moment you log in:


1. What needs my attention?
2. What is currently running?
3. What did Hexa already handle?
4. What routines are scheduled?


Agent Center transforms your daily security routine. While you were offline, Tenable Hexa AI ran overnight sweeps, triaged findings, and prepared scoped proposals. Instead of starting your day with work, you begin with an auditable, and evidence-backed decision — with a ledger behind it recording what happened, who approved it, and when.


*The Agent Center dashboard in Tenable One*


## The agentic AI harness under every routine


None of what makes the routines safe is a property of the model. It comes from the harness Tenable One puts around any model or agent working in a customer environment.


The harness starts from a blunt assumption: models and agents are untrusted participants. Permissions, context boundaries, approval gates, validation, and auditability all sit outside the model, and a routine never reaches past the permissions of the person who created it.


You can see this today in Tenable Hexa AI. The objective resolves against Tenable’s Exposure Data Fabric — your real environment, priorities, and prior context — not the internet. Tenable Hexa AI picks capabilities so you never wire steps; the approval gate defines what runs; and a separate check validates every state change before it leaves the queue. The audit log makes it verifiable after the fact, and failures, fallbacks, model changes, and latency are the harness’s problem rather than your own.


This is why a proposal to write into your endpoint management tool is something you can reasonably approve at 8 AM. Access to a frontier model is not what makes agentic security work; everything around the model does. The Exposure Data Fabric makes the answers relevant to your environment, and the harness makes the actions taken against it trustworthy.


The model reasons. Tenable Hexa AI coordinates. Agents do the specialized work. The harness holds the boundary.


## How to get started with Tenable Hexa AI


To begin automating exposure management with Tenable Hexa AI, follow these three simple steps:


1. **Identify a repetitive task** - Pick the job you are tired of starting.
2. **Define your objective and guardrails** - Write down what you want Tenable Hexa AI to do and set the limits you would want if you were handing the routine to a new analyst.
3. **Set the cadence** - Observe the first few runs of the routine to make sure it’s executing properly. When you’re ready, put the routine on a schedule.


Delegating your first routine does more than save you a couple of hours. It gives you back nights and weekends while closing exposures in minutes.


## **Frequently asked questions (FAQs)**


### **What is Tenable Hexa AI?**


Tenable Hexa AI is an agentic security automation capability within Tenable One that automates vulnerability triage, asset scoping, endpoint patch deployment, and post-remediation verification across integrated tools like Jamf.


### **What are routines in Tenable Hexa AI?**


Routines are intent-driven automations defined in natural language. They consist of an objective (what to accomplish), guardrails (operational constraints), and a schedule. Unlike traditional scripts, routines adapt dynamically to changes in your infrastructure.


### **How does Tenable Hexa AI prevent unauthorized changes to endpoints?**


Tenable Hexa AI enforces human-in-the-loop approval gates before any changes are committed to endpoint tools. Furthermore, the **Tenable One harness** guarantees that a routine can never exceed the strict access permissions of the security professional who created it.


### **What is the Tenable One Exposure Data Fabric?**


The Exposure Data Fabric is the underlying data architecture in Tenable One that provides real-time asset context, priorities, and environment data to Hexa AI. This ensures AI reasoning is anchored in actual enterprise context rather than external data.


## Learn more


- [Meet Tenable Hexa AI: Agentic AI for exposure management](https://www.tenable.com/blog/hexa-ai-agentic-ai-for-exposure-management)
- [Implement agentic AI in cybersecurity with Tenable Hexa AI](https://www.tenable.com/blog/implement-agentic-ai-in-cybersecurity-to-reduce-risk-tenable-hexa-ai)
- [Beating the Mythos clock: Automate patching with Tenable Hexa AI](https://www.tenable.com/blog/beating-the-mythos-clock-using-tenable-hexa-ai-custom-agents-for-automated-patching)
