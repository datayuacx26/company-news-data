---
schema_version: "1.0.0"
document_id: "1195c478a7deef09def338ff8bb71290d775c7925b47b823303e52e61cdc66f0"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/alert-triage-agentic-soc-elastic-workflows"
published_at: "2026-07-02T00:00:00+00:00"
first_seen_at: "2026-07-20T03:30:09.053610+00:00"
fetched_at: "2026-07-28T20:47:34.280666+00:00"
content_hash: "sha256:808537f6361d677f87b3dd51b3ab5458e4931befe0bf3100b2d84954589916b9"
---

# Inside Elastic InfoSec's agentic SOC: cutting alert triage from 30 minutes to under 3

2 July 2026 •


[Aaron Jewitt](https://www.elastic.co/security-labs/author/aaron-jewitt)


# Inside Elastic InfoSec's agentic SOC: cutting alert triage from 30 minutes to under 3


Elastic's InfoSec team built AI agents on Elastic Workflows that investigate every alert and assemble the case before an analyst ever opens it.


14 min read


[Generative AI](https://www.elastic.co/security-labs/category/generative-ai) ,


[Detection Engineering](https://www.elastic.co/security-labs/category/detection-engineering)


This is Part 1 of the Inside Elastic InfoSec's Agentic SOC series.[Part 2: choosing the right agent architecture for a 5× cost reduction](https://www.elastic.co/security-labs/agentic-soc-token-budget-architecture) .[Part 3: how we cut AI agent LLM calls by 60%](https://www.elastic.co/security-labs/ai-agent-optimization-production-scale)


Elastic's InfoSec team built an agentic SOC that triages every alert before an analyst opens it. A 30-minute manual investigation now finishes in under 3 minutes: deterministic ES|QL queries close obvious false positives at zero token cost, specialized AI agents investigate the rest across endpoint, cloud, and SaaS domains, and a Final Review agent writes the verdict to a Kibana case. The whole pipeline runs on Elastic's native stack ([Workflows](https://www.elastic.co/docs/explore-analyze/workflows) ,[Agent Builder](https://www.elastic.co/docs/explore-analyze/ai-features/elastic-agent-builder) , the[Elastic Inference Service](https://www.elastic.co/docs/explore-analyze/elastic-inference/eis) , and[Kibana Cases](https://www.elastic.co/guide/en/security/current/cases-overview.html) ) with no third-party orchestrator, and inference routed only to providers documented with zero data retention.


AI-assisted attacks have compressed the timeline from initial access to exfiltration from days to hours, and traditional manual alert triage cannot keep pace. Hiring more analysts does not scale with alert volume. The[Agentic SOC](https://www.elastic.co/what-is/agentic-security-ops) pattern fixes this gap: automate the investigation work that does not require human judgment so analysts can focus on the alerts that do.


Note that we use a workflow as our Agentic SOC orchestration layer instead of an Agent. We chose to use a workflow for orchestration instead of an Agent because of the scale we are operating at. A workflow is deterministic, fast, and does not consume tokens. When you are triaging tens of thousands of alerts per month, this can make a huge difference in costs and performance.


For a security team processing sensitive alert data, the inference layer's data handling matters. The[Elastic Inference Service](https://www.elastic.co/docs/explore-analyze/elastic-inference/eis-supported-models) routes requests to trusted third-party model providers that operate with zero data retention and do not use inputs to train models. Per-model data retention and training-data status are documented on the[EIS supported-models page](https://www.elastic.co/docs/explore-analyze/elastic-inference/eis-supported-models) so customers can verify the status of the specific model their pipeline uses. For airgapped or highly sensitive environments, the same pipeline can run against a model hosted on your own infrastructure.


At Elastic, our InfoSec team operates as "Customer Zero." We run the newest versions of Elastic Security in our production environment, often before they are released publicly. Our fleet spans thousands of laptops, servers, and cloud workloads across a globally distributed workforce. We are the first and most demanding user of every feature we ship, including the Workflows and Agent Builder platforms.


Our Agentic SOC journey started with a single[Agent Builder](https://www.elastic.co/docs/explore-analyze/ai-features/elastic-agent-builder) triage agent in Elastic Security 9.2. It handled workstation alerts well, where the investigation pattern is consistent, but we found that SaaS provider logs and[Higher-Order](https://www.elastic.co/security-labs/higher-order-detection-rules) threshold alerts required a more specialized methodology. That gap drove our move to domain-specific agents.


##


Alert triage with Workflows and ES|QL: closing alerts without AI


The principle behind this first step is simple: any check that can be resolved by a query should be a query, not an LLM call. ES|QL queries are deterministic, auditable, fast, and cost nothing in tokens. An LLM call is non-deterministic, slower, more expensive, and introduces failure modes (hallucinated facts, prompt injection, inconsistent reasoning across runs) that a query does not have. Most false-positive patterns in a mature SOC are well understood and can be expressed in code, so spending tokens to reason about them is a wasted cost. The LLM is the right tool for the alerts where the data is genuinely ambiguous, not for the ones a query can close cleanly.


This builds on the approach we described in our earlier[automated SIEM investigation post](https://www.elastic.co/blog/false-positives-automated-siem-investigations-elastic-tines) using Tines, where many of these same triage checks ran as Tines stories. Bringing them into Elastic Workflows keeps the full pipeline inside Kibana.


Detection rules in Kibana support a new[workflow action](https://www.elastic.co/docs/solutions/security/detect-and-alert/common-rule-settings#rule-notifications) . When you configure this on a rule, every alert the rule generates is automatically sent to the designated workflow with no manual intervention. Our orchestration workflow is the entry point for the entire pipeline. Each workflow has a trigger configuration that tells it how it is expected to be called. To use workflows with alerts, the trigger configuration is straightforward:


```text
triggers:
- type: alert
```


Our detection engineers tag rules with triage categories (` Triage: Workstation` ,` Triage: PMFA` ,` Triage: Asset` ,` Triage: All` ) that control which checks run. A workstation rule runs device and user identity checks. An infrastructure rule runs broader asset and CI/CD checks. This tagging is how you express "what does a false positive look like for this rule" at authoring time, and the workflow enforces it automatically. The rule's tags appear on every alert it generates in the` kibana.alert.rule.tags` field.


Our workflow groups triage checks by alert type. For alerts from IP-based sources (Okta, AWS, Azure, GCP, GitHub, and similar), the workflow runs up to 16 ES|QL queries across our asset inventory, fleet data, and SaaS audit logs to determine whether the source IP belongs to known corporate infrastructure. Here is one example, checking whether the source IP has an active low-risk Okta session that indicates phishing-resistant MFA was used from this IP:


```text
- name: ip_okta_consolidated
type: elasticsearch.esql.query
with:
query: |
FROM logs-okta*
| WHERE source.ip == "{{ event.alerts[0].source.ip }}"
AND @timestamp > NOW() - 24h
AND event.action == "policy.evaluate_sign_on"
AND okta.debug_context.debug_data.risk_level == "LOW"
| KEEP @timestamp, source.ip, user.email, event.action
| LIMIT 1
```


If any query returns a result (for example, the source IP matches a successful low-risk Okta login), the workflow closes the alert immediately and adds the workflow tag` Closed: Okta PMFA IP` :


```text
- name: close_alert_okta
type: kibana.request
with:
method: POST
path: "/s/{{ consts.space_id }}/api/detection_engine/signals/status"
body:
signal_ids:
- "{{ event.alerts[0].kibana.alert.uuid }}"
status: closed
```


No tokens used. No case created. The alert is closed.


##


ES|QL enrichment: building the shared alert context every agent reads


Alerts that survive the triage step go on to the enrichment portion of the workflow. This step gathers all the supporting information needed to provide context about the activity in order to accurately triage an alert. Any query that an analyst would run to investigate an alert should be added to the workflow. Our workflow queries more than 20 data sources using the values from the alert's ECS fields:


- User and host names checked against Entity Risk scoring.
- User Okta login locations and devices from the last 7 days.
- Asset Inventory information for a complete profile of the users involved.


- User asset inventory: work role, geographic location, assigned workstations.
- For workstation alerts, the asset inventory finds the owner, then pulls that user's profile.


- Cloud account ownership.


- All entity information for any service account or cloud asset in the alert.


- Source IP activity across AWS, Azure, GCP, Google Workspace, Office 365, Salesforce, and GitHub.
- List of all alerts for the same user, workstation, and` source.ip` in the last 72 hours.
- Specialized enrichment tailored to the alerts datasource to assist the specialized triage agents.


- Any context we can provide to the specialized agents via ESQL helps reduce the number of LLM calls made by the agents, which can dramatically reduce overall costs.


- Recent cases containing the same observables as the alert


- Case outcome, alert names, and summary; flag if the case was marked false positive with the same alert.


The workflow assembles the results into a note for the Initial Triage agent's prompt; if a case is later opened, the same note is added as one of the first comments. Every downstream agent reads this note rather than re-running the same queries.


##


The Initial Triage agent: automated alert triage in under a minute


The Initial Triage agent is the first agent in the pipeline, and its output determines the workflow path. The primary additional source we provide this agent is the[Elastic Security Labs](https://www.elastic.co/security-labs) knowledge base, which lets it compare the alert against every published Elastic article on threat actor techniques and malware behavior. The agent’s job is to do a structured assessment of the alert. The first line of its response must follow a specific format, and the workflow uses a substring check to parse it. The Verdict can only be` True Positive` or` False Positive` , the Assessment can only be` malicious` ,` suspicious` ,` unknown` or` benign` , and the Confidence can only be` high` or` low` .


```text
## Verdict: True Positive | Assessment: suspicious | Confidence: high
**Reason:** One-line explanation.
**Summary:**
Short report about the alert with a max size of 3000 characters.
```


If the verdict is` False Positive` and the confidence is` high` , the workflow adds a[timeline note](https://www.elastic.co/guide/en/security/current/timeline-api-update.html) to the alert and closes it. The whole path, from alert trigger through enrichment to the Initial Triage close, typically completes within a minute at a token cost of around 50k tokens. For an alert that would have taken an analyst 15 minutes or more to investigate manually, that is a significant reduction in both cost and response time.


If the verdict is anything other than a high-confidence false positive, the workflow moves to the case path.


The Initial Triage agent is intentionally narrow in scope to increase speed and reduce token usage. The initial triage agent only uses an average of 50k tokens per use; a general-purpose agent can consume 500k or more tokens per use. If your Agentic SOC is triaging 10,000 alerts per month, this is a huge cost savings when your initial triage agent can close even 5,000 of those alerts. This limited scope also keeps the agent fast, predictable, and affordable.


##


Opening a Kibana case and dispatching the Specialized agents


When the workflow does not close the alert, it opens a new case in[Kibana Cases](https://www.elastic.co/guide/en/security/current/cases-overview.html) , our SOC's case management system, and attaches the alert and enrichment context to the case. Every alert that needs deeper investigation gets its own case, which becomes the shared workspace for everything that happens next. The workflow attaches the alert as the first artifact, then adds the full enrichment as a comment. The workflow also adds the detection rule investigation guide to the case as a separate comment to help guide the following agents. Every downstream Specialized agent writes its findings to the same case as a comment, and our analysts manage, comment on, link, and resolve those cases in the same view they already use for the rest of our incident response work. The enrichment is already there when the Specialized agents run; they do not have to re-derive it.


Routing to the Specialized agents uses ECS fields from the alert:` agent.type` and` host.os.type` for endpoint alerts, and` event.dataset` for cloud and SaaS alerts. Only the relevant agents are run. A macOS endpoint alert triggers the macOS Forensics agent, not the GCP or Azure agents. An AWS CloudTrail alert triggers the AWS agent and the Cloud Forensics agent, not the endpoint agents. This reduces unnecessary token usage.


Specialized agent Domain Data sources


Threshold Enrichment Contributing alerts for threshold rules Alerts index, entity resolution


macOS Forensics macOS endpoint[logs-endpoint.events.*](https://www.elastic.co/docs/reference/integrations/elastic-defend) , process entity IDs


Windows Forensics Windows endpoint[logs-endpoint.events.*](https://www.elastic.co/docs/reference/integrations/elastic-defend) ,[logs-winlog.*](https://www.elastic.co/docs/reference/integrations/windows)


Linux Forensics Linux endpoint[auditbeat-*](https://www.elastic.co/docs/reference/beats/auditbeat)


AWS CloudTrail AWS API activity[logs-aws.cloudtrail*](https://www.elastic.co/docs/reference/integrations/aws/cloudtrail)


Okta Authentication and sessions[logs-okta*](https://www.elastic.co/docs/reference/integrations/okta)


Azure Azure AD and activity[logs-azure.*](https://www.elastic.co/docs/reference/integrations/azure)


GCP GCP audit logs[logs-gcp*](https://www.elastic.co/docs/reference/integrations/gcp)


Cross Cloud Forensics Examining entity behavior through multi-cloud environments AWS, Azure, GCP indices


Same-Rule Recent Cases Prior cases for this rule Kibana Cases API


SaaS Activity Investigate user or IP activity in SaaS logs such as Slack, Office 365, Google Workspace Multiple Elastic integrations


Each Specialized agent has a specific investigation methodology written directly into its system prompt. This is different from using a broad agent with many skills. A broad agent, which is excellent for analyst-led chat sessions where a human can steer it, can load the needed skills to investigate alerts depending on what it thinks it needs at that time. For automation, that runtime decision-making and skill loading adds costs from LLM calls and produces less consistent results.


We tested this trade-off in detail on the companion post[Part 2: choosing the right agent architecture for a 5× cost reduction](https://www.elastic.co/security-labs/agentic-soc-token-budget-architecture) . The short version: when an agent runs in automation, the dominant cost driver is the number of LLM calls it makes, because each call carries the full conversation history with it. It is a little counterintuitive, but sometimes using a longer system prompt that tells the agent exactly what to do reduces total cost by eliminating the LLM calls the agent would otherwise spend deciding what to do next. That is why every agent in our pipeline has a precise, methodology-rich prompt rather than a thin one with skill delegation.


###


MacOS Forensics agent: an example investigation


The agent prompt frames the agent's role precisely: it is a macOS forensic examiner whose job is to document what happened, not to decide whether the activity is malicious. The instructions are explicit and repeated: the agent must not include any verdict, assessment, or judgment (benign, malicious, suspicious, true positive, false positive). That call belongs to the Final Review agent later in the pipeline. To support its investigation, the agent has a tight tool set: ES|QL queries against endpoint events, a dedicated` endpoint.process.entity_id` tool for pulling all related network and file events for a given process, an alerts lookup for cases where` process.entity_id` is missing, and` security.security_labs_search` , which gives it access to the[Elastic Security Labs](https://www.elastic.co/security-labs) knowledge base. The Security Labs tool lets the agent check command lines, hashes, or file paths against every published Elastic article on threat actor techniques and malware behavior, so it can flag known malicious indicators directly rather than reasoning about them from scratch.


Here is a condensed view of the macOS forensics investigation workflow from the agent's instructions. The full prompt includes example ES|QL queries and lists of fields for the agent to keep.


```text
You are a forensic examiner specializing in **MacOS** endpoint forensics. Your job is to document **what happened**, not to judge whether it is malicious or benign. You receive an alert plus pre-enriched context (including host and owner when available). The workflow has already run ESQL queries to pre-gather MacOS endpoint context (recent process and file events on this host). This pre-gathered data is included in your message. Perform a focused deep-dive using process tree analysis and return factual findings.


Constraints:
- Never pull "full documents" when a tiny field set is enough. Always **KEEP** only required fields and use a small **LIMIT**.
- You have **120 seconds** total. Optimize for speed and reliability.
- Do NOT include any verdict, assessment, or judgment (benign/malicious/suspicious/true positive/false positive). Your report is purely factual.
- the process.entity_id field from the alert is unique to the process that triggered the alert, use this field for finding related events.
- All MacOS endpoint data is located in the logs-endpoint.* index and the SIEM alerts are in the .alerts-security.alerts-* index. Do not use any other index


Investigation Steps:


1. Process tree: query endpoint.process_entity_id with the alerting process's entity_id, then extract process.Ext.ancestry to find parent and grandparent processes.
2. Ancestry trace: query each non-system parent's entity_id, up to 2 hops. Stop tracing at well-known high-event processes (launchd, WindowServer, kernel_task, loginwindow, node, Cursor, Code Helper, Electron, python, Terminal, iTerm2, zed). They add no forensic value and waste the query budget.
3. Command line analysis: look for script abuse (bash, zsh, python, osascript), execution from /tmp or /var/folders, persistence via LaunchAgents/LaunchDaemons.
4. File and network: note file.path under /Applications, ~/Library, or /usr/local; unusual outbound connections.


Output: process tree ASCII art, 2-3 key observations, chronological timeline. Note any network connections or files created. Include process and user names, the entity_id fields are unique strings and not descriptive for users.
```


The "no verdict" constraint is intentional. The Specialized agents are fact-finders. Their output is purely what happened. The assessment of whether those findings are malicious, suspicious, or benign belongs to the Final Review agent. Keeping facts and verdict in separate agents prevents the interpretation in one domain's findings from biasing the final call.


Every Specialized agent writes its findings to the case as a separate comment. The case accumulates a structured audit trail: enrichment, the Initial Triage assessment, and one comment per Specialized agent that ran.


##


The Final Review agent: the final alert triage checkpoint


The Final Review agent is the synthesis agent. It has only two built-in tools:` platform.core.cases` and` security.security_labs_search` . It reads the case, including all comments and the attached alerts, compares that information to the Elastic Security Labs knowledge base, and writes the final analyst-facing report using all of the available information.


The constraints are tight by design. The Final Review agent does not query for additional data; it cannot look up anything that is not already in the case. This forces the workflow to ensure all relevant data is in the case before the Final Review agent runs, and it ensures its output is grounded entirely in the evidence already assembled.


The report begins with a required header that the workflow parses the same as the Initial Triage agent:


```text
## Verdict: True Positive | Assessment: malicious | Confidence: high
**Summary:** Unauthorized IAM role creation from external IP with no
matching Okta session or corporate asset context.
```


After the verdict header, the Final Review agent produces a one-paragraph summary of the findings followed by the detailed report. The detailed report includes:


- A list of all entities involved and a Cross Entity Behavior Analytics (CEBA) report that maps relationships between them (user, endpoint, source IP, cloud account).
- All recent alerts from those entities.
- A numbered list of recommended actions for the analyst.
- A chronological timeline of events from the alert and the Specialized agents' findings.


If the Final Review verdict is` False Positive` with` high` confidence, the workflow closes the case and the alert. If the Final Review verdict is` True Positive` with` high` confidence, we can have the workflow increase the case severity and send a message in Slack or PagerDuty to the analysts depending on the criticality of the alert. The workflow then updates the case summary with the verdict and summary so the analyst sees the main findings and recommended actions at the top of the case without having to scroll through the full comment thread first.


##


What the analyst sees after automated alert triage


Instead of starting from scratch with an alert, the analyst finds a fully investigated case already assembled. Most of the queries they would have done during the investigation are already complete. The Kibana case contains:


- The alert that triggered the case.
- The full enrichment note: source IP activity across all relevant data sources, user profile and Okta stats, workstation or cloud account context, and correlated alerts from the last 72 hours.
- One comment per Specialized agent that ran, each with a focused forensic report from the relevant domain specialist.
- The Final Review report in the case description, with a True Positive / False Positive assessment, recommended next actions, CEBA relationship analysis, and event timeline.


This typically completes within a minute of the alert being created. An analyst reviewing the case can quickly decide whether to act on it, close it, or escalate.


##


How to build an alert triage pipeline in your environment


The architecture is a workflow and a collection of agents, but the underlying pattern is straightforward. Here is the recipe at a high level:


1.


**Tag your detection rules.** Define what a false positive looks like for each rule type.` Triage: Workstation` means "close if Fleet or Jamf confirms this is a managed corporate device."` Triage: Asset` means "run the full infrastructure inventory check." Detection engineers own the tags; the workflow enforces them. See our[earlier post on automated SIEM investigations](https://www.elastic.co/blog/false-positives-automated-siem-investigations-elastic-tines) for additional information.


2.


**Build the orchestration workflow.** The workflow is the backbone of the pipeline:


- Receives every alert via the workflow action.
- Runs deterministic triage checks to close what it can.
- Enriches the rest with ES|QL across your relevant data sources.
- Routes to the right agents and opens cases.
- Handles closes when the Initial Triage or Final Review agent returns a high-confidence false positive.


For each alert type, decide which data sources contain useful context and build ES|QL steps for each. All ES|QL queries in the workflow should use` KEEP` statements to keep only the needed fields in the output to prevent overwhelming the agents.


The workflow can be large and complex; we recommend using an AI Coding assistant such as Claude or Codex to help create and edit the workflow.


3.


**Build a narrow Initial Triage agent.** It should receive the enrichment and make a single structured verdict. Give it a small tool set for gap-filling and a strict output format the workflow can parse. The narrower the scope, the more predictable the token cost. One important detail: do not pass the full alert document to the agent. Raw alert documents contain many fields that are not useful for triage and will inflate your token count. Instead, use an ES|QL` KEEP` statement in the workflow to extract the fields that matter (rule name, event action, process command line, source IP, user, host, and similar) along with the alert ID. If the agent needs additional fields, it can retrieve the full document using the alert ID.


4.


**Build Specialized agents for your highest-volume domains.** Write the investigation methodology directly into the system prompt rather than relying on skill delegation. A step-by-step methodology produces consistent, reproducible output. Start with the domains that generate the most alerts in your environment.


5.


**Build a Final Review agent that reads the case.** Its only job is to interpret what the Specialized agents found and render a final assessment and report. Giving it access to the case and no other tools keeps it grounded in evidence and prevents it from hallucinating or going off on its own investigation.


##


Alert triage in under 3 minutes: the bottom line


The agentic SOC pipeline turns 30-minute manual alert triage into under 3 minutes of automated investigation. Every alert that reaches an analyst already comes with a full investigation and a recommended action, so the analyst's time goes toward deciding what to do, not toward gathering the context to decide.


Deterministic ES|QL triage closes the false positives that have clear, queryable patterns at zero token cost. The Initial Triage agent closes the next layer at around 50k tokens. Anything that survives gets a full investigation from the Specialized agents and a synthesis report from the Final Review agent before an analyst ever opens the alert.


We built the entire pipeline on Elastic's native stack:[Elastic Workflows](https://www.elastic.co/docs/explore-analyze/workflows) for orchestration,[Agent Builder](https://www.elastic.co/docs/explore-analyze/ai-features/elastic-agent-builder) for the agents, the[Elastic Inference Service](https://www.elastic.co/docs/explore-analyze/elastic-inference/eis) for inference, and[Kibana Cases](https://www.elastic.co/guide/en/security/current/cases-overview.html) as the shared investigation workspace. No third-party automation platforms, no separate orchestrators, and inference routed through providers documented with zero data retention. If you want to build something similar, the[Agent Builder](https://www.elastic.co/docs/explore-analyze/ai-features/elastic-agent-builder) and[Workflows](https://www.elastic.co/docs/explore-analyze/workflows) documentation are the right starting points. If you are not already running Elastic Security, you can[start a free trial](https://www.elastic.co/cloud/elasticsearch-service/signup) to explore both.


We would like to hear what you build. The[Elastic Security community forum](https://discuss.elastic.co/c/security) is a good place to share what you have tried and ask questions.


#### Jump to section


- [Alert triage with Workflows and ES|QL: closing alerts without AI](https://www.elastic.co/security-labs/alert-triage-agentic-soc-elastic-workflows#alert-triage-with-workflows-and-esql-closing-alerts-without-ai)
- [ES|QL enrichment: building the shared alert context every agent reads](https://www.elastic.co/security-labs/alert-triage-agentic-soc-elastic-workflows#esql-enrichment-building-the-shared-alert-context-every-agent-reads)
- [The Initial Triage agent: automated alert triage in under a minute](https://www.elastic.co/security-labs/alert-triage-agentic-soc-elastic-workflows#the-initial-triage-agent-automated-alert-triage-in-under-a-minute)
- [Verdict: True Positive | Assessment: suspicious | Confidence: high](https://www.elastic.co/security-labs/alert-triage-agentic-soc-elastic-workflows#verdict-true-positive--assessment-suspicious--confidence-high)
- [Opening a Kibana case and dispatching the Specialized agents](https://www.elastic.co/security-labs/alert-triage-agentic-soc-elastic-workflows#opening-a-kibana-case-and-dispatching-the-specialized-agents)
- [MacOS Forensics agent: an example investigation](https://www.elastic.co/security-labs/alert-triage-agentic-soc-elastic-workflows#macos-forensics-agent-an-example-investigation)
- [The Final Review agent: the final alert triage checkpoint](https://www.elastic.co/security-labs/alert-triage-agentic-soc-elastic-workflows#the-final-review-agent-the-final-alert-triage-checkpoint)
- [Verdict: True Positive | Assessment: malicious | Confidence: high](https://www.elastic.co/security-labs/alert-triage-agentic-soc-elastic-workflows#verdict-true-positive--assessment-malicious--confidence-high)
- [What the analyst sees after automated alert triage](https://www.elastic.co/security-labs/alert-triage-agentic-soc-elastic-workflows#what-the-analyst-sees-after-automated-alert-triage)
- [How to build an alert triage pipeline in your environment](https://www.elastic.co/security-labs/alert-triage-agentic-soc-elastic-workflows#how-to-build-an-alert-triage-pipeline-in-your-environment)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=Inside%20Elastic%20InfoSec%27s%20agentic%20SOC:%20cutting%20alert%20triage%20from%2030%20minutes%20to%20under%203&url=https://www.elastic.co/security-labs/alert-triage-agentic-soc-elastic-workflows)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/alert-triage-agentic-soc-elastic-workflows)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/alert-triage-agentic-soc-elastic-workflows&title=Inside%20Elastic%20InfoSec%27s%20agentic%20SOC:%20cutting%20alert%20triage%20from%2030%20minutes%20to%20under%203)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/alert-triage-agentic-soc-elastic-workflows&title=Inside%20Elastic%20InfoSec%27s%20agentic%20SOC:%20cutting%20alert%20triage%20from%2030%20minutes%20to%20under%203)
