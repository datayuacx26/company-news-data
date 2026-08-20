---
schema_version: "1.0.0"
document_id: "f626cdf9693fe180dd1352d4ca8d2a5f5faf3f67eb8217c894e917b834d8142b"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/agentic-soc-alert-triage-alertzero"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-07-31T17:52:30.959585+00:00"
fetched_at: "2026-07-31T17:52:31.997851+00:00"
content_hash: "sha256:c477705fd4c723c6cf55d5aa7a8cca3125d1adf1d5e97963c9da566f9370534a"
---

# Alert Zero: AI-driven alert triage and attack investigation for the agentic SOC

31 July 2026 •


[David Elgut](https://www.elastic.co/security-labs/author/david-elgut)


# Alert Zero: AI-driven alert triage and attack investigation for the agentic SOC


Elastic Security 9.5 gives SOC teams AI that handles first-pass alert triage and investigation, so analysts can get back to threat hunting and detection engineering instead of working through queue noise.


10 min read


[Product Updates](https://www.elastic.co/security-labs/category/product-updates) ,


[Generative AI](https://www.elastic.co/security-labs/category/generative-ai)


It's 9 a.m. Monday, the start of your shift.


You begin the day like any other Monday: You open the queue, and the wall of alerts is already waiting. New alerts land between 9:05 and 9:10 a.m., while you close yesterday’s. You're already drowning, and you haven’t even had a chance to refill your coffee. You know that you won’t be able to get to things that really need prioritization. Threat hunting stays deferred, and detection engineering waits. Incident response practice never quite starts. This is the default security operations center (SOC) day for a lot of teams, and that probably includes yours.


The concept of Alert Zero addresses this problem. It doesn’t mean that the analysts' queue will always remain at zero; new alerts will always come in, and some will still need a human in the loop to review and do deeper investigations. The goal is to keep that queue from dictating the analyst’s day. Instead of walking into a wall of alerts, the team starts with a smaller set of work that genuinely needs attention, giving analysts more time to hunt, tune rules, and investigate the threats that matter most. Alert Zero is about moving toward that kind of shift, and Elastic Security 9.5 gives teams practical tools to do it.


With 9.5, Elastic is bringing together three pieces that can move your SOC closer to Alert Zero without requiring you to build and maintain a complex agent architecture yourself:


1. The Security alert analysis workflow helps separate predictable false-positive noise from alerts that deserve attention.
2. Attack Discovery investigates the alerts worth pursuing and turns them into grounded attack narratives.
3. [Elastic Workflows](https://www.elastic.co/security-labs/security-automation-with-elastic-workflows) provides the automation layer that brings these capabilities into the playbooks your team already trusts.


You choose where to start and how much to automate. You also choose where a human still needs to approve the next step. That’s what an agentic SOC should look like in practice: agents handling more of the repetitive work, while analysts stay in control and the queue keeps moving closer to zero.


##


Suggested flow


##


What is Alert Zero, and how does it reduce SOC alert fatigue?


Alert Zero is a state that your SOC works toward over time, and 9.5 ships the pieces that make real progress toward making this practical. It isn’t a feature that you simply turn on. Most teams already automate some alert handling, whether that’s through playbooks or another automation method, but the hardest and most expensive work is usually what remains: true positives, unclear cases, and groups of related alerts that need human judgment. That’s where queues grow and analysts burn out. It’s also where gaps in your detections are easiest to miss.


Getting closer to Alert Zero changes what a security analyst’s shift can feel like. Senior analysts can spend more time investigating real risk instead of repeatedly gathering host, user, and alert context that the platform can collect for them. The queue gets shorter without turning false positives into incidents, and detection engineers can use lessons from real investigations to improve coverage. That’s the flow that every team wants and the one that the queue usually keeps out of reach.


None of this means handing the SOC over to autopilot. Analysts still make the decisions that matter. This is what we can’t stress enough. We want you to be making decisions on the things that actually matter, not automating your critical thinking away. Agents help with the first pass of triage and investigation, and you decide how much autonomy they receive. Process still matters, but Elastic Security 9.5 gives teams more practical tools for making that journey.


##


How Security alert analysis automates SOC alert triage


One of the first steps toward Alert Zero is reducing noise before it reaches an analyst or an agent. In Elastic Security 9.5, the Security alert analysis workflow can provide a first-pass assessment for alerts generated by rules you select. A security information and event management (SIEM) engineer or SOC manager chooses the agent and model and sets the confidence requirements. They also decide which rules participate.


When an alert fires, the workflow gathers the alert details, related alerts, frequency, and prior resolution history. It adds a note explaining the classification, confidence, and rationale, along with details about the workflow run. If you want to dig deeper, you can open the agent conversation and see what the model reviewed.


Auto-close is optional and disabled until you turn it on. Even then, it only applies to alerts classified as false positives above the confidence threshold you set. A sensible approach is to begin with notes and tags, compare the results with how your analysts would handle the same alerts, and introduce auto-close only once you trust the pattern. You remain the decision-maker; that is, you choose the rules and set the confidence level. You also decide how much autonomy is appropriate. Once those boundaries are in place, the agent operates within them.


The path to Alert Zero becomes more effective when Security alert analysis and Attack Discovery work together. Alert analysis handles the first pass by classifying alerts as true positives, false positives, or inconclusive and can optionally close high-confidence false positives. You can then use those results to focus Attack Discovery on the open alerts that deserve deeper investigation. That gives Attack Discovery a cleaner starting point and reduces noise and unnecessary token usage. That also allows it to spend more of its context finding relationships between the signals that matter. In 9.5, Attack Discovery takes that focused alert set and does more than surface a discovery; it investigates it.


##


How Attack Discovery investigates alerts as attack chains


Once the Security alert analysis workflow has classified and optionally closed false-positive alerts, the goal isn’t to investigate every remaining alert one by one. Attack Discovery takes that smaller set of alerts worth investigating and turns them into attack chains that become the unit of work. Since launching two years ago, its core strength has been correlating related alerts and building the broader story so analysts don’t have to piece it together manually. This frees your team to focus on the decisions and investigations that matter most.


Attack Discovery is also no longer disconnected from where analysts spend their time. In 9.5, its discoveries live on the new Attacks page under Detections, directly alongside Alerts. Analysts can begin with correlated attacks that are ready to investigate, with the underlying alert evidence right next to them when they need it, instead of starting their day with a wall of individual alerts.


In 9.5, it goes further. Attack Discovery still does what it has always done well: correlate related alerts and build the broader attack narrative. Now it investigates the activity behind that narrative, too, so by the time an analyst picks it up, much of the groundwork is already done.


Attack Discovery runs through a skill-backed pipeline powered by Elastic Agent Builder and Elastic Workflows. The skill begins by retrieving relevant alerts and then uses the available security skills to hunt for related activity, analyze individual alerts, inspect entity context, and search raw logs in the Elasticsearch platform before generating the attack narrative. These steps can uncover malicious activity that existing detections missed while gathering additional evidence to determine whether the activity represents a real attack. The result is a richer, more grounded discovery that gives analysts a much stronger place to begin their investigation.


If the investigation uncovers activity that existing detections missed, the Attack Discovery skill presents a detection gap analysis showing what wasn’t covered. From there, it can draft an Elasticsearch Query Language (ES|QL) rule to close the gap. An analyst must review and explicitly approve the draft before the rule is created. No detection is added automatically.


Just as importantly, you can see how the investigation happened. Each run includes an Agent Builder conversation showing which skills were used, which alerts were investigated, and how the model reached its conclusion. If something goes wrong, before you consider opening a support case, the same agent can help explain where the run failed.


By the time an Attack Discovery reaches an analyst, much of the context gathering has already happened. The analyst can begin with a grounded attack narrative and focus on the decision that matters: what to do next, instead of jumping between alerts, entities, and raw logs. It gets analysts there faster, rather than replacing analyst judgment. That’s what moving closer to Alert Zero looks like for the hardest alerts in the queue.


##


Connect Attack Discovery to your existing SOC workflows


Most mature teams already have established playbooks and/or Elastic Workflows for tenant scoping, enrichment, escalation, cases, and more, so building toward Alert Zero doesn’t mean replacing the operating model that your SOC already trusts. The goal is to bring deeper investigation into those workflows, not force analysts to move their operations onto another product page.


This is what makes Attack Discovery an always-on part of the SOC. Schedule it to run as often as your team needs, or trigger it from a workflow, so investigations begin without waiting for an analyst. Your team still controls what happens next.


Elastic Workflows,[generally available (GA) since 9.4](https://www.elastic.co/security-labs/elastic-workflows-ga-9-4) , allows Attack Discovery to fit into that operating model. Many teams can begin with the skill-driven retrieval and default validation included with Attack Discovery, but teams with more complex requirements can connect custom retrieval or validation workflows to control how alerts are gathered, enriched, and approved. This might include translating output, checking indicators against VirusTotal, adding MITRE ATT&CK context, or validating entities against other data in the environment.


You can also run the complete Attack Discovery pipeline as a step inside a workflow. That step retrieves alerts, generates discoveries, and validates the results before continuing through the rest of your playbook. Additional workflow steps let you manage alert and attack status, tags, and assignees from the canvas without writing Kibana requests by hand.


Once created, attacks become operational objects alongside alerts under Detections. Teams can search, filter, assign, tag, update, and add notes through the UI, APIs, or Workflows. If Attack Discovery surfaced an attack that looks suspicious to you as an experienced analyst, you can inspect why the pipeline trusted it. If it’s real, your team can manage the attack chain as the unit of work instead of returning to a wall of individual alerts.


The value goes beyond faster investigations. You can bring AI into your security operations without taking on a separate implementation project. Attack Discovery can live inside the playbooks that already page the right people, open the right cases, and drive the next response. That’s how Alert Zero becomes part of daily operations instead of another dashboard your team has to monitor.


###


Bring your own AI model to the agentic SOC


Moving toward Alert Zero requires trust. Teams need control over the models doing the work and over the decisions that those models are allowed to make.


Elastic lets you choose your model and provider, including local large language models (LLMs) for air-gapped and sovereign deployments. That means you can introduce AI-assisted triage and investigation without rebuilding your existing security, privacy, and governance programs around a vendor-selected model.


You also control the level of autonomy. Workflows and agent conversations remain inspectable, so your team can see what ran and what evidence was considered. The team can also see how the agent reached its conclusion. You decide what happens automatically and what requires approval. Plus, you decide what remains entirely human-driven.


A clear queue isn’t a success if nobody can explain how the work left it. Staged automation, inspectable agents, and human approval for higher-risk actions help teams move toward Alert Zero without replacing alert fatigue with unexplained decisions.


##


How to get started with AI-driven SOC alert triage


You don’t need to adopt every capability at once. Start with a part of the alert queue your team understands, keep the first runs visible, and introduce automation only after the output matches how your analysts make decisions today.


###


Start with alert classification, not auto-close


Choose a small set of well-understood detection rules. Enable Security alert analysis with notes and tags, but leave auto-close disabled. Review the classifications, confidence scores, and reasoning with the analysts who normally handle those alerts.


###


Test Attack Discovery against known alert activity


Run Attack Discovery on demand against an alert window that your team already knows. Have senior analysts compare the attack chains, supporting evidence, and discarded alerts with their own investigation. Adjust the scope until the results consistently provide a useful starting point.


###


Connect alert triage and investigation to existing workflows


Use the classifications and fields written by alert analysis to focus Attack Discovery on alerts that deserve deeper investigation. Add the Attack Discovery step to an existing enrichment or escalation playbook, while leaving cases, paging, and response processes where they already work.


###


Add automation autonomy gradually


Once the results are reliable, enable auto-close for high-confidence false positives one rule at a time. Schedule stable Attack Discovery scopes, and keep analyst approval in place for new detection drafts. Monitor true-positive escapes more closely than queue reduction.


###


Measure what changed for the analyst


Track how much of the queue still requires human review and how long it takes analysts to reach a decision. You should also track whether new detection rules reduce repeat work. The success metric here is the measurable time saved rebuilding context repeatedly on your team, thus allowing analysts to investigate real threats and focus on protecting your customers.


Analysts remain in control throughout the process. The goal is to let agents handle repeatable work and to give experts a better place to begin, not to automate every click.


##


What changes for SOC analysts when the alert queue is clear


The point of Alert Zero is the freedom and agency that your team can regain when the queue no longer controls the entire shift, rather than to reach a number and stay there, finding a new metric to chase.


Security alert analysis handles more of the predictable noise, and Attack Discovery gives the alerts that matter a deeper, evidence-backed investigation. And Workflows put those capabilities into the operating model that your SOC already trusts. Analysts remain responsible for the decisions that carry risk, but they no longer have to begin every investigation from scratch.


The best outcome is an SOC where experts have more time to hunt, improve detections, practice response, and stop real attacks, rather than an empty dashboard. Elastic Security 9.5 gives your team a real place to start and a practical path toward an agentic SOC where the queue no longer runs the day.


See the documentation for[Attack Discovery](https://www.elastic.co/docs/solutions/security/ai/attack-discovery) and[Elastic Workflows](https://www.elastic.co/docs/explore-analyze/workflows) to get started, and try it on your deployment. Or start a[free trial](https://cloud.elastic.co/serverless-registration) . Connect with us on[Elastic's community Slack](https://elasticstack.slack.com/archives/C018PDGK6JU) to share feedback or tell us what you’re building.


#### Jump to section


- [Suggested flow](https://www.elastic.co/security-labs/agentic-soc-alert-triage-alertzero#suggested-flow)
- [What is Alert Zero, and how does it reduce SOC alert fatigue?](https://www.elastic.co/security-labs/agentic-soc-alert-triage-alertzero#what-is-alert-zero-and-how-does-it-reduce-soc-alert-fatigue)
- [How Security alert analysis automates SOC alert triage](https://www.elastic.co/security-labs/agentic-soc-alert-triage-alertzero#how-security-alert-analysis-automates-soc-alert-triage)
- [How Attack Discovery investigates alerts as attack chains](https://www.elastic.co/security-labs/agentic-soc-alert-triage-alertzero#how-attack-discovery-investigates-alerts-as-attack-chains)
- [Connect Attack Discovery to your existing SOC workflows](https://www.elastic.co/security-labs/agentic-soc-alert-triage-alertzero#connect-attack-discovery-to-your-existing-soc-workflows)
- [Bring your own AI model to the agentic SOC](https://www.elastic.co/security-labs/agentic-soc-alert-triage-alertzero#bring-your-own-ai-model-to-the-agentic-soc)
- [How to get started with AI-driven SOC alert triage](https://www.elastic.co/security-labs/agentic-soc-alert-triage-alertzero#how-to-get-started-with-ai-driven-soc-alert-triage)
- [Start with alert classification, not auto-close](https://www.elastic.co/security-labs/agentic-soc-alert-triage-alertzero#start-with-alert-classification-not-auto-close)
- [Test Attack Discovery against known alert activity](https://www.elastic.co/security-labs/agentic-soc-alert-triage-alertzero#test-attack-discovery-against-known-alert-activity)
- [Connect alert triage and investigation to existing workflows](https://www.elastic.co/security-labs/agentic-soc-alert-triage-alertzero#connect-alert-triage-and-investigation-to-existing-workflows)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=Alert%20Zero:%20AI-driven%20alert%20triage%20and%20attack%20investigation%20for%20the%20agentic%20SOC&url=https://www.elastic.co/security-labs/agentic-soc-alert-triage-alertzero)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/agentic-soc-alert-triage-alertzero)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/agentic-soc-alert-triage-alertzero&title=Alert%20Zero:%20AI-driven%20alert%20triage%20and%20attack%20investigation%20for%20the%20agentic%20SOC)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/agentic-soc-alert-triage-alertzero&title=Alert%20Zero:%20AI-driven%20alert%20triage%20and%20attack%20investigation%20for%20the%20agentic%20SOC)
