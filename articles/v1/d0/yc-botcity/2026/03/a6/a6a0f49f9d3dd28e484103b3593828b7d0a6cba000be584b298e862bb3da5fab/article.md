---
schema_version: "1.0.0"
document_id: "a6a0f49f9d3dd28e484103b3593828b7d0a6cba000be584b298e862bb3da5fab"
company_key: "yc-botcity"
company: "BotCity"
source_id: "yc-botcity-rss-7ddde15ad998"
canonical_url: "https://blog.botcity.dev/2026/03/19/your-edr-is-blind-to-python-heres-whats-already-running-in-your-endpoints/"
published_at: "2026-03-19T11:00:31+00:00"
first_seen_at: "2026-07-26T23:58:30.888214+00:00"
fetched_at: "2026-07-28T21:56:54.694470+00:00"
content_hash: "sha256:2db2604251d12ddcfadb43ee1de1a0c8dbf6bf94a34130d5a75cbf32fc910934"
---

# Your EDR is blind to Python: Here’s what’s already running in your endpoints

The proliferation of Generative AI (GenAI) and Large Language Models (LLMs) has ushered in a new era of innovation, but it has simultaneously created a formidable, and often invisible, security challenge:[Shadow AI](https://blog.botcity.dev/2026/01/27/shadow-ai-governanca-riscos-forbes/) .


This phenomenon is rapidly transforming the corporate technology landscape, positioning Python (the “lingua franca of AI”) as the **new attack surface** and the modern-day equivalent of the problematic Excel macros of the past.


## Why do traditional controls fail against parallel AI?


Traditional endpoint security and governance solutions were not designed for today’s reality. The speed and ease with which corporate users can now **generate and deploy Python scripts** (often for innovative and beneficial purposes) mean that established controls are fundamentally inadequate.


The data is clear and alarming:


- 20% of organizations have already suffered security breaches through unauthorized AI, yet 63% do not have a formal AI governance policy ([IBM Cost of a Data Breach Report, December 2025](https://www.ibm.com/reports/data-breach) ).
- [Gartner](https://www.gartner.com/en/newsroom/press-releases/2025-11-19-gartner-identifies-critical-genai-blind-spots-that-cios-must-urgently-address0) predicts that 40% of organizations will suffer a security breach through hidden AI by 2030.
- 69% of organizations already suspect their employees are using prohibited GenAI tools.


The core issue is that employees, empowered by tools such as GPTs and AI copilots, are **automating processes and building small applications** (frequently in Python) that directly handle sensitive company data.


For someone who has never programmed before, simply asking a legal professional to “ **track a package from the postal service** ” or “extract data from a spreadsheet” results in immediately executable Python code.


The risks are no longer theoretical; they are manifesting across regulated industries, creating serious operational and security risks:


**Industry** **Scenario** **Risk profile**


Financial services An FP&A analyst uses SAP GUI scripts, now often in LLM-generated Python, to extract 15,000 General Ledger (GL) records every night and store them in a personal OneDrive account for analysis. **Data exfiltration** : Sensitive financial data is transferred to an unmanaged cloud server, outside corporate control.


Manufacturing The Accounts Payable team automates invoice posting in Oracle EBS and SAP using a Python script. This script contains hardcoded Oracle credentials and accesses vendor banking data attached to emails on a shared drive. **Credential exposure and financial risk** : Hardcoded credentials represent a massive single point of failure, risking unauthorized system access and potential fund diversion.


Pharmaceuticals A data manager exported 2.4 million rows of clinical trial patients’ Personally Identifiable Information (PII) to a local hard drive to “clean and analyze in pandas.” The data remained on the local machine for six weeks before being discovered during a routine IT security audit. **Serious compliance violation (PII)** : A large volume of highly sensitive data is stored on an unsecured local endpoint, violating patient privacy regulations.


## The six critical risk vectors your traditional controls miss


When unsupervised Python scripts run on endpoints, they introduce a range of risks that standard Endpoint Detection and Response (EDR), Data Loss Prevention (DLP), and Mobile Device Management (MDM) tools fail to detect:


1. **Embedded credentials** : Scripts frequently contain hardcoded usernames, passwords, or API keys, creating easily exploitable security vulnerabilities.
2. **Anomalous after-hours activity** : Automated scripts often run at unusual times, making it difficult to distinguish them from malicious activity.
3. **Vulnerabilities and malicious packages** : LLMs can recommend outdated or vulnerable Python libraries, and these scripts can initiate network connections that carry malicious payloads.
4. **Database queries:** Scripts can establish direct connections to company databases, bypassing application-level security controls.
5. **File reading, manipulation, and exfiltration** : Scripts are designed to process data. This includes reading sensitive files and exfiltrating them outside the corporate environment (for example, to personal cloud storage or via external APIs).
6. **Script loss and operational risk** : If the employee who created a script leaves the company, valuable and functional automations (the correct scripts) can be lost, leading to critical operational failures.


The fundamental question for the board is: “How many Python scripts are processing customer data on our endpoints right now, and who approved them?”


While EDR monitors process behavior, DLP protects documents, and MDM manages devices, all of them focus on known applications and company-managed data flows. They do not address the core characteristics of the parallel AI use case:


- Corporate users are **building their own applications** (scripts) on their workstations.
- They are **manipulating company data** to which they have legitimate access.
- They are creating automations that **affect other applications** .


The intent is usually good: **employees are innovating and generating** value. However, at the same time, they are introducing significant security and operational risks.


Trying to force business users to adopt a traditional, secure software development workflow (such as GitHub) presents several obstacles:


- **Technical knowledge:** It requires technical proficiency that the average business user does not have.
- **Late-stage analysis** : A secure pipeline analyzes code only after it has been submitted. The most significant risk occurs during development: the business user “playing” with AI-generated code locally, where every run can pose a risk.
- **Scalability** : Requiring a dedicated virtual environment for every employee is a monumental, unscalable IT challenge.


## Sentinel: Python governance without blocking innovation


The solution is not to block Python. Doing so stifles innovation and pushes the problem further into the shadows. The key is to provide visibility and granular control at the point of execution.


[BotCity Sentinel](https://botcity.dev/sentinel) shifts the technical aspects of **governance to the endpoint** , allowing organizations to manage the risk of parallel AI without hindering business process improvement.


Sentinel provides continuous, in-depth monitoring of all Python execution, focusing on critical data points:


- **Libraries:** Tracking of imports.
- **LLM usage** : Monitoring of LLM interactions.
- **Communication** : Inbound and outbound network activity.
- **Data access** : File read/write operations and database connections.
- **System interaction** : Application execution, spreadsheet processing, and logging.
- **Resource utilization** : Monitoring of computing resources (CPU/RAM).


This comprehensive data gives teams a complete view of every Python script running, including its location (machine), associated risks, and the ability to take immediate, precise action. Crucially, the entire solution **runs 100% locally** , ensuring data sovereignty and regulatory compliance.


## Where does your organization stand?


We have observed three distinct responses to the Shadow AI challenge:


1. **Aware and acting fast** : Companies in this category believe: “I have this problem, it’s huge, and I need immediate visibility.” Their proposed action is to implement immediate endpoint monitoring and risk assessment.
2. **Aware and facing the problem** : These companies say: “I know I have this problem, but I don’t know its scale or impact.” They plan to run a proof of concept to map their current Python environment and quantify the risk.
3. **In denial** : Companies here claim: “Everything is locked down here. I’m sure we don’t have Shadow Python installed.” Their recommended action is to start a discovery scan that leaves no trace, to reveal what is *actually* running.


If you recognize your organization in one of these scenarios, now is the time to act. Contact us. We’ll show you exactly **what’s running on your endpoints** and how to govern this new reality. Our experts are ready to provide a deep, practical analysis of your current state.


Don’t wait for the next incident. Let us help you gain the **visibility and control** you need to protect your digital environment and govern your IT ecosystem with confidence.


### Dive deeper into this discussion in the webinar.


If this topic has raised concerns about your operations, it’s worth watching the webinar we developed specifically on this subject.


In this article, we showed how the risk is already present on endpoints, and why **BotCity Sentinel** provides the **visibility and execution evidence that** GRC and SecOps teams need to govern Python usage more securely.


- [Share on X (Opens in new window) X](https://blog.botcity.dev/2026/03/19/your-edr-is-blind-to-python-heres-whats-already-running-in-your-endpoints/?share=twitter)
- [Share on Facebook (Opens in new window) Facebook](https://blog.botcity.dev/2026/03/19/your-edr-is-blind-to-python-heres-whats-already-running-in-your-endpoints/?share=facebook)
-


### Like this:


Like


Loading…
