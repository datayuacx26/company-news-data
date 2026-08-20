---
schema_version: "1.0.0"
document_id: "817c5dcefce89201eafa6a97b87c56b7251a32ec72de6385cc0d90d48758354e"
company_key: "varonis-systems-inc-common-stock"
company: "Varonis Systems Inc."
source_id: "varonis-systems-inc-common-stock-rss-915499d71e96"
canonical_url: "https://www.varonis.com/blog/rogue-agent-dialogflow-attack"
published_at: "2026-07-07T13:00:02+00:00"
first_seen_at: "2026-07-24T05:47:29.345543+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:bebaad077be9b184c58ba70dcff3e0ff9db6880498d88ee3c2590853c3720b7e"
---

# Rogue Agent: How a Single Code Block Could Hijack Your AI Conversations in Google’s DialogFlow

Varonis Threat Labs discovered a critical vulnerability in Google Cloud Platform’s (GCP) Dialogflow CX service, Google’s flagship conversational AI platform for building interactive experiences across voice and text chatbots. We’ve named this latest discovery Rogue Agent.


The vulnerability allowed attackers to exploit the Code Blocks feature to inject persistent malicious code into the Dialogflow agents’ pipeline, silently exfiltrating conversations and conducting large-scale phishing campaigns. To initiate, the exploit requires a single edit permission known as dialogflow.playbooks.update on one agent.


Rogue Agent highlights the growing risk posed by the integration of AI into cloud platforms. Like a turncoat spy who exposes colleagues to the enemy, our Rogue Agent could compromise other agents on the same project by overriding their shared execution environment.


Rogue Agent demonstrates how AI expands the attack surface. Using various techniques, attackers can work around AI guardrails, insert code, and seed malicious instructions. With[80% of the Fortune 500](https://www.microsoft.com/en-us/security/blog/2026/02/10/80-of-fortune-500-use-active-ai-agents-observability-governance-and-security-shape-the-new-frontier/?msockid=18d50060648b653f1c611739651f64a1) actively using AI agents, the risk is real. Rogue Agent is the latest in a series of AI threats ─ like[Reprompt](https://www.varonis.com/blog/reprompt?hsLang=en) in Microsoft Copilot Personal and[SearchLeak](https://www.varonis.com/blog/searchleak?hsLang=en) in Microsoft Copilot Enterprise ─ that we’ve responsibly disclosed by working closely with leading cloud providers.


Varonis initially discovered and reported this vulnerability in November 2025. Google issued an initial security update in April 2026 and fully resolved the issue in June 2026. All affected components have since been remediated. Before the patch, any GCP organization using Dialogflow CX agents with Playbook Code Blocks was potentially at risk.


Additionally, Varonis and Google recommend that customers audit their Dialogflow CX configurations for suspicious Playbook updates and analyze past playbook update actions. We are not aware of any exploitation in the wild before Google’s patch release.


Continue reading our report to see how[Varonis Threat Labs](https://www.varonis.com/varonis-threat-labs?hsLang=en) made the Rogue Agent discovery in detail and why the attack was virtually undetectable.


## **Architecture overview of Dialogflow**


Dialogflow agents power customer support systems, financial services bots, healthcare assistants, and enterprise workflows that handle sensitive data, including personally identifiable information (PII), payment details, and confidential business information. Since the agents integrate with backend systems, their security posture is critical.


Architecture overview of Dialogflow


Architecture overview of Dialogflow


## **What are Code Blocks?**


Dialogflow CX uses Playbooks to provide a structured workflow during conversations with users. As part of Playbooks, Dialogflow offers Code Blocks, a feature that allows developers to embed custom Python logic directly into conversation flows. This means agents can dynamically process user input, call external APIs, and manipulate data — all within the execution environment provided by Google.


Here’s an example of a code block that checks whether a given number is prime:


Code Blocks execute inside a Google-managed Cloud Run service, a fully managed serverless platform for running containerized applications. Cloud Run abstracts infrastructure management, scales automatically, and provides isolation between workloads. Cloud Run instances also have public network egress by default, meaning they can initiate outbound connections to the internet and effectively communicate across data perimeters and break Zero Trust architectures.


Here lies the critical design detail — all Dialogflow agents that use Code Blocks in the same GCP project effectively share the same Cloud Run execution environment, which is managed by Google and is outside the victim’s scope. This simplifies operations but introduces a critical trust gap: customers have no direct visibility or control over that environment.


## **How are data perimeters enforced in GCP?**


GCP enforces data perimeters using VPC Service Controls (VPC-SC), which prevents data exfiltration by enforcing strict access boundaries around resources. Organizations rely on VPC-SC to keep sensitive data inside trusted networks and comply with regulations such as GDPR and HIPAA.


## **Diving into Dialogflow CX**


The vulnerability we discovered exploited a fundamental weakness in Dialogflow CX’s Playbook Code Blocks architecture and was restricted to our own GCP environment. The shared Cloud Run Service that runs the Code Blocks code had public network access, a write-enabled file system, and ran under a user with sufficient privileges to modify system files. These conditions created the perfect attack surface for threats, where a single foothold could lead to systemic compromise.


The only permission required to configure Code Blocks was *dialogflow.playbooks.update* , which can be granted at the project level and scoped down to a specific agent, allowing Playbooks updates for that agent. However, because Playbooks could include Code Blocks, they also enabled the execution of arbitrary Python code by design.


With the ability to run arbitrary Python code by feature, we started by checking if code constraints were available to run. To our surprise, we found no restrictions at all. Enumeration of the Python files in Cloud Run’s filesystem revealed a key file named *code_execution_env.py,* with content suggesting it was responsible for executing the configured Playbook Code Blocks by using Python’s *exec()* function.


Since *code_execution_env.py* was writeable, overriding it allowed the attacker to implement their own malicious code with access to session parameters and user history conversations, directly interfering with the Code Blocks pipeline and manipulating workflows.


During the exploitation, we discovered that the configured Code Block was simply appended to internal system code before being passed to the *exec* () function. This internal code defined critical variables such as:


- *history* – containing the full conversation history, including past user utterances and agent responses.
- *state* – exposing session-level parameters such as the current session ID.


Here’s an example — notice the appended Code Block at the end of the internal system code:


Because the injected Code Block is executed in the same scope inside *exec()* , attackers could reference these variables directly. This meant full visibility into ongoing conversations and the ability to hijack sessions or impersonate legitimate flows.


To add to the danger, attackers could call internal functions such as *respond()* and force the agent to return a specified string, making it appear as if the LLM generated the response. This opened the door for threats that enable phishing attacks, social engineering, and complete manipulation of the conversation.


The exploit chain was straightforward:


- T


he attacker created a modified version of


*code_execution_env.py* which:


-


Intercepted every execution before calling *exec()*
- Exfiltrated conversation data to an attacker-controlled server via access to internal parameters
- Injected phishing prompts disguised as legitimate reauthentication requests from the agent by utilizing *respond()* , prompting the users to submit their credentials.


-


In the following exfiltrated conversations, the attacker would catch the submitted credentials


-


Using Code Blocks, configure a Code Block that downloads a modified version of the *code_execution_env.py* file from an attacker-controlled public GCS bucket and overwrites the original file inside the Cloud Run container


-


Persist malicious logic that runs the modified version of *code_execution_env.py* for every user utterance


The Rogue Agent attack flow.


The Rogue Agent attack flow.


Below is the actual PoC Code Block used to overwrite the execution environment:


Once executed, the attacker could restore the original Code Block configuration to make the activity in the Dialogflow Console UI appear normal. Meanwhile, the malicious code persisted in the Cloud Run environment, completely invisible to the victim. Cloud Logging did not record the overwrite or the injected logic. This made detection nearly impossible.


The result? Attackers could silently take control of every agent in the same GCP project, manipulate conversations, and exfiltrate sensitive data without detection. For organizations relying on Dialogflow CX for customer interactions, this flaw represented a catastrophic breach of trust, all from a single, overlooked permission on a single agent.


Large-scale social engineering, regulatory violations, and reputational damage are among the consequences organizations could face when threats weaponize the infrastructure enterprises rely on to power AI agents.


## **Bonus vulnerabilities**


While our research on the Code Injection flaw was the most severe, we uncovered two additional weaknesses that amplified the overall risk of rogue agents.


### **VPC-SC bypass vulnerability**


Dialogflow CX agents often operate in environments protected by VPC Service Controls (VPC-SC), which enforce perimeter security to prevent data exfiltration. Code Blocks, however, are executed inside a Google-managed Cloud Run service with unrestricted outbound internet access, effectively placing the execution environment outside the project’s VPC-SC perimeter and turning the Cloud Run Service into a covert proxy for data exfiltration. Combined with the code injection vulnerability above, attackers could exfiltrate sensitive data even if VPC-SC was applied to the agent.


Using preinstalled libraries such as urllib, we established a bidirectional communication channel from the execution environment to an external server and bypassed VPC-SC entirely. In addition to exfiltrating sensitive data, this channel could also receive commands to enable attackers to create a command-and-control (C2) channel for persistent remote control. Attackers could potentially inject instructions, manipulate workflows, and maintain stealthy access without detection.


The PoC was as simple as the following code block, which signals an HTTP request to an attacker-controlled server even though the Dialogflow agent is protected by a VPC-SC perimeter:


### **Credential leakage via IMDS**


Another vulnerability involved the Instance Metadata Service (IMDS) being exposed within the Cloud Run Service environment. By querying IMDS, we retrieved access tokens belonging to a Google-managed service account.


While these credentials belong to a low-privileged service account, their presence represented a serious architectural flaw: code execution environments should never have access to IMDS. This violates isolation principles and creates systemic risks. Attackers could have leveraged this same flaw to escalate privileges inside Google’s own project if they were to grant this service account additional privileges.


This POC snippet extracted all the data from the IMDS, base64-encoded it, and then printed it out in the Dialogflow Console UI by raising an exception:


Here’s the structure of the redacted extracted token, which contains Google-owned IDs:


## **Detecting rogue agents in Google logic**


Detecting these vulnerabilities was challenging because the overwrite occurred in a Google-managed Cloud Run environment outside the victim’s visibility. Cloud Logging does not capture the exact configuration changes.


Although these vulnerabilities have been patched, we recommend taking the following actions to ensure your organization wasn’t impacted.


### **Review logs for playbook updates**


If you have DATA_WRITE Audit Logs enabled for the Dialogflow API in your projects, look for successful past events with:


Correlate these events with additional indicators of compromise, such as:


- Rare API access by a user
- Unusual IP addresses
- Atypical access times


### **Run a query for failed requests**


Run the following Cloud Logging query to identify failed user requests:


Under the field protoPayload.status.message, review the reason for the failure. In some cases, this message may include exceptions thrown by Dialogflow Code Blocks that were potentially triggered by malicious logic.


### **Manually review Code Blocks**


Although attackers could remove malicious blocks after exploitation, you should ensure that no unauthorized code was configured by a sloppy attacker.


In the Dialogflow CX console for each agent in your organization, navigate to **Playbooks** .


Review each Playbook’s current Code Block configuration:


Confirm that all configured Code Blocks are whitelisted and approved.


Confirm that all configured Code Blocks are whitelisted and approved.


## **The bottom line**


As AI agents become central to enterprise workflows, risks to your data — fueled by misconfigurations or overlooked permissions — grow exponentially.


The vulnerabilities revealed in Dialogflow CX serve as a powerful reminder that layered defense is essential for cloud-native AI platforms. When event data and logging are not enough, organizations must incorporate UEBA and posture management solutions to ensure Dialogflow configurations adhere to best practices.


This research also underscores that cloud services like Dialogflow are deeply integrated with other GCP components, and that security features are not always properly implemented. Defenders should deeply understand their cloud architecture and recognize that true data security requires vigilance across every layer, not just the perimeter.
