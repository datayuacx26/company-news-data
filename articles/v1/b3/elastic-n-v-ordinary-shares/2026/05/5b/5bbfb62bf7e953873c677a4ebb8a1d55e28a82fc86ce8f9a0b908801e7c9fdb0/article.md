---
schema_version: "1.0.0"
document_id: "5bbfb62bf7e953873c677a4ebb8a1d55e28a82fc86ce8f9a0b908801e7c9fdb0"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/ai-esql-detection-rule-creation"
published_at: "2026-05-04T00:00:00+00:00"
first_seen_at: "2026-07-20T03:30:09.053610+00:00"
fetched_at: "2026-07-28T22:15:27.754583+00:00"
content_hash: "sha256:0f30a1ab8c04a9bedf7cdaebb7b18b6664d7cb545eed4b2cfe2a9bda9cfeaa3b"
---

# From plain English to production rule: AI-native Elasticsearch ES|QL detection in Elastic Security

4 May 2026 •


[Kseniia Ignatovych](https://www.elastic.co/security-labs/author/kseniia-ignatovych)


# From plain English to production rule: AI-native Elasticsearch ES|QL detection in Elastic Security


Elastic Security now lets analysts describe a threat behavior in plain language and receive a complete, validated Elasticsearch ES|QL detection rule in return, no query expertise required.


10 min read


[Product Updates](https://www.elastic.co/security-labs/category/product-updates)


Elastic Security now includes AI-powered detection rule creation, built into the rule creation workflow. Analysts describe a threat behavior in plain English and receive a complete, validated Elasticsearch Query Language (ES|QL) rule in return, with MITRE ATT&CK mappings, severity recommendations, and a preview against live data, all without leaving the platform or writing a single line of query syntax. This post walks through exactly how that works using an Okta credential stuffing and account takeover scenario as the example.


##


Why detection engineering needs AI-native tooling


The threat landscape has changed. Attackers are increasingly using AI to automate and scale their operations: generating[phishing campaigns at volume](https://hoxhunt.com/guide/phishing-trends-report) , accelerating[vulnerability research and exploitation](https://www.rapid7.com/blog/post/tr-accelerating-attack-cycle-2026-global-threat-landscape-report/) , and launching credential attacks that would have required significant manual effort just a few years ago. The result is a faster, higher-volume threat environment where the window between a new attack pattern emerging and it hitting your environment is narrowing.


Detection engineering teams are on the other side of that equation. The expectation is that coverage keeps pace with the threat, but the tooling available to write, test, and deploy rules hasn’t historically matched the speed at which new attack patterns appear. Writing an effective detection rule from scratch requires deep familiarity with the query language, the field schema, and the aggregation logic needed to express the behavior you are trying to catch, before you even begin thinking about the threat itself. For most security teams, that friction means a growing backlog and gaps in coverage that attackers can exploit.


Arming detection engineers with native, AI-powered tooling isn’t just about convenience; it’s also about keeping pace with an adversary that’s already using AI to move faster. Elastic Security is now adding AI rule creation, powered by the[Elastic Agent Builder](https://www.elastic.co/docs/explore-analyze/ai-features/elastic-agent-builder) . Unlike external AI tooling or stand-alone code generation workflows, this capability is built into the detection engineering experience: The rule is created and validated, with results preview generated entirely within your platform, against your own data, without leaving Elastic Security. Analysts can now describe what they want to detect in natural language and receive a complete, ready-to-review ES|QL rule in return, without leaving the rule creation workflow. This capability is available at the Enterprise license tier.


Support for ES|QL rule creation is available now. Additional rule types are on the roadmap, so keep an eye on upcoming releases as these capabilities expand.


##


Detections without the heavy lifting


[ES|QL](https://www.elastic.co/guide/en/elasticsearch/reference/current/esql-language.html) , Elastic's pipeline query language, is very helpful for behavioral and aggregation-based detections. Its pipe-based syntax makes it natural to express the kind of "filter, count, group by, threshold" logic that underlies most modern detections: How many failed logins came from this IP? Which accounts were targeted? Does this count exceed the expected baseline?


That same expressiveness is also what makes ES|QL harder to write by hand than a simple field-match query. You need to think in terms of pipelines: Filter first with` WHERE` , aggregate with` STATS...BY` , and then filter again on the computed values. It requires knowing the right Elastic Common Schema (ECS) field names, the correct function syntax, and how the pipeline stages interact. This is exactly the kind of structured, pattern-based logic that AI can translate reliably from a plain English description.


With the new detection engineering[skills](https://www.elastic.co/security-labs/skills-elastic-security-9-4) and the knowledge of Elastic documentation, ECS field definitions, and local data access, the[Elastic AI Agent](https://www.elastic.co/docs/explore-analyze/ai-features/agent-builder/builtin-agents-reference) uses detection engineering best practices to come up with the rule, and moreover, the generated rule query is validated before it’s returned: What you see in the editor will run.


##


Walkthrough: Detecting Okta credential stuffing and account takeover


A credential stuffing attack that succeeds in breaching an account doesn’t stop at the login. The full attack chain (multifactor authentication \[MFA\] bypass, session establishment, privilege escalation, and policy modification) leaves a distinct footprint across Okta system logs if you know what to correlate. This is exactly the kind of multistage behavioral pattern that ES|QL handles well: Collect all the relevant event types, classify each one, aggregate by the shared identity attributes, and then apply threshold logic that requires the full sequence to be present before alerting.


Writing that query manually means knowing the Okta-specific[event action names](https://developer.okta.com/docs/reference/api/event-types/) , knowing how to use` EVAL` with` CASE` to create per-event type flags, and how to then aggregate those flags with` SUM` to count each stage independently. It’s a realistic but nontrivial query, exactly the kind that benefits most from AI generation.


Imagine your team has[an Okta integration](https://www.elastic.co/docs/reference/integrations/okta) and logs are coming in. Threat intelligence has flagged an active campaign targeting Okta tenants: automated credential stuffing followed by MFA fatigue and post-compromise privilege changes. You need detection coverage today.


Note: We’re skipping the step of checking whether prebuilt detection rules exist or are already enabled, for the simplicity of the scenario here.


###


Opening the AI Agent rule creation flow


From the Elastic Security sidebar, navigate to **Detection rules** and click **Create a rule -> AI rule creation** .


###


Describing the detection in plain language


No special syntax is required. Describe the full attack chain the way you would explain it to a colleague, including the data source and the specific event sequence you want to match:


Analyst prompt:


` In Okta, detect when the same user and source IP shows: three or more failed logins due to bad credentials, at least one MFA failure, then a successful login, and then either a privilege grant or a policy update. That full sequence together is a credential stuffing attack that succeeded.`


The AI Agent processes this against its knowledge base, including Okta integration field mappings and ECS conventions, and executes multiple steps that we can follow and review:


And then it returns a complete ES|QL rule that covers the full attack sequence described.


###


Reviewing and adjusting the generated rule logic


There’s a lot happening in this rule’s query, and it’s worth understanding each stage, because the structure itself tells the story of the attack chain.


The query is concise by design: It uses ES|QL's inline` WHERE` filtering inside` COUNT()` to compute each stage of the attack chain in a single` STATS` pass, without needing a separate` EVAL` block. Here’s what each part does:


` FROM logs-okta*` scopes the query to all Okta log indices, using a wildcard that picks up` logs-okta.system-*` and any other Okta data streams in the environment.


The` STATS` block is the core of the detection. It aggregates all Okta activity and computes four counters per unique combination of` user.name` and` source.ip` , one for each stage of the attack chain.` failed_logins` counts` user.session.start` events with` outcome: failure` (the password spray attempts).` mfa_failures` counts failed MFA challenges, indicating the attacker encountered a second factor and attempted to push through it.


` successful_logins` counts` user.session.start` events with` outcome: success` ; a value of one or more means the attacker got in.` post_compromise_events` counts any of six actions that indicate the attacker is acting on their objective after login: adding the account to a group, granting application access, escalating privileges, modifying a policy lifecycle, updating a policy rule, or changing the account profile. This is a broad net that covers the full range of post-compromise behavior seen in Okta account takeover incidents.


The` WHERE` clause after the aggregation requires all four conditions to be true simultaneously before a row becomes an alert. This is what makes the rule high-fidelity. A user who forgot their password and eventually logged in won’t match because they’ll have no post-compromise events. An attacker who got through but took no further action won’t match either. All four stages must be present.


The` KEEP` statement trims the output to the six fields that matter for triage (the targeted account, the source IP, and the count for each stage), giving the responding analyst everything they need to start an investigation without querying the raw logs first.


Along with the query, the AI Agent generates the following rule metadata: rule name, description, severity and risk score recommendations, MITRE ATT&CK technique and tactic mapping (T1110.004 Credential Stuffing, T1078 Valid Accounts, ), execution schedule, and tags. Where other rules exist, the AI Agent also reuses relevant tags from those rules, so new custom rules stay consistent with your existing detection library from the start. The data source is selected from indexes available in the system, or data ingestion is suggested. The rule fields are editable with an AI Agent before or after filling the resulting rule information in the rule creation form.


**Tip:** You can also ask the AI Agent to explain an existing rule query, suggest threshold adjustments based on a description of your environment, or help troubleshoot unexpected results.


Let’s keep working on the rule to adjust a few things. We want to ensure the rule detects credential stuffing and not other failure reasons, like expired passwords or locked accounts. We want to ensure the attack sequence is preserved.


Using AI Agent, we’ll ask it to fix these few things. It comes back with the adjusted query and summarizes what it did.


We now apply the changes to the rule form from the AI Agent chat:


###


Previewing and enabling the rule


Before enabling, use the Preview rule results panel to run the query against recent data in your environment. Any existing matches will surface immediately, running against your actual Okta log data in your Elastic deployment (no sample data, no sandbox, no external validation step required), useful both for validating that the query logic is correct and for checking whether an attack may already be in progress in your Okta tenant.


In this example, we’ve added sample logs to get a single alert generated:


Now, satisfied with the results, we’ll enable the rule. It will begin executing on its configured schedule and generate alerts for any user and source IP combination where the full attack sequence is observed within the query window.


If we execute the rule manually for the past week to find any past attacks and check resulting alerts, we see the same alert we’ve gotten in the Rule Preview.


**Note:** AI-generated rules should be reviewed before deployment in production environments. The AI Agent may not have full awareness of your specific data schema, log source quirks, or environment-specific baseline behavior. Use the rule preview to validate against your actual data before enabling.


##


Impact on detection engineering workflows


The walk-through above, from opening the rule creation form to having a validated, multistage, MITRE-mapped ES|QL rule covering the full Okta account takeover chain, takes a few minutes. Writing the same query manually would require knowing the Okta-specific event action names, the correct` okta.outcome.reason` field and its enumerated values, how to structure` EVAL` with` CASE` to produce per-stage flags, how to aggregate those flags with` SUM` rather than` COUNT` , and how to express a compound post-compromise condition using` OR` across two aggregated fields. For an analyst onboarding a new data source under time pressure, that’s a significant amount of context to hold simultaneously.


The AI Agent doesn’t replace detection expertise. The analyst still makes every meaningful decision: which event types constitute the attack chain, what thresholds make sense for their environment, and whether the preview results look correct. What changes is the time it takes to get from having threat knowledge to having a working rule. Engineers who understand the attack and can describe it iterate and get a production-quality query back quicker, rather than spending time on implementation mechanics.


This matters most at the moments when speed is most critical: when a new campaign is active, when a data source has just been onboarded, or when an existing rule needs rapid refinement because the threat has evolved. AI-powered attackers aren’t waiting for your rule backlog to clear. Detection engineering tooling shouldn’t require it either.


##


What's next


AI rule creation for ES|QL is the first step in a broader expansion of AI Agent-driven detection engineering in Elastic Security. ES|QL was the natural starting point given its aggregation-first pipeline structure, which maps cleanly to the behavioral descriptions analysts naturally provide. Support for additional rule types and additional quality of life rule creation steps and beyond is on the roadmap. Keep an eye on the[Elastic Security Labs](https://www.elastic.co/security-labs) blog and release notes for updates as new capabilities become available.


For a broader look at how AI Agents are reshaping the detection engineering role, from threat modeling and telemetry tuning through to rule authoring and maintenance at scale, see[Supercharge Your SOC: Detection Engineering in the Era of AI Agents](https://www.elastic.co/security-labs/supercharge-your-soc) on Elastic Security Labs. For a comprehensive overview of the full detection engineering toolset available in Elastic Security today, including prebuilt rules, alert suppression, MITRE ATT&CK coverage, and Detections as Code, see[Know your tools: The full range of Elastic Security's detection engineering capabilities](https://www.elastic.co/blog/elastic-security-detection-engineering) .


Try the new AI rule creation capability on your deployment, or[start a free trial](https://www.elastic.co/cloud/cloud-trial-overview/security) . Connect with us on[Elastic's community Slack](https://join.slack.com/t/elasticstack/shared_invite/zt-2sgssfr0n-NhTOlSwHbaGH85tYfx6kGg) to share feedback or tell us what detection use cases you’re building and how we can help.


*The release and timing of any features or functionality described in this post remain at Elastic's sole discretion. Any features or functionality not currently available may not be delivered on time or at all.*


*In this blog post, we may have used or referred to third-party generative AI tools, which are owned and operated by their respective owners. Elastic does not have any control over the third-party tools and we have no responsibility or liability for their content, operation or use, nor for any loss or damage that may arise from your use of such tools. Please exercise caution when using AI tools with personal, sensitive or confidential information. Any data you submit may be used for AI training or other purposes. There is no guarantee that information you provide will be kept secure or confidential. You should familiarize yourself with the privacy practices and terms of use of any generative AI tools prior to use.*


*Elastic, Elasticsearch, ESRE, Elasticsearch Relevance Engine and associated marks are trademarks, logos or registered trademarks of Elasticsearch N.V. in the United States and other countries. All other company and product names are trademarks, logos or registered trademarks of their respective owners.*


#### Jump to section


- [Why detection engineering needs AI-native tooling](https://www.elastic.co/security-labs/ai-esql-detection-rule-creation#why-detection-engineering-needs-ai-native-tooling)
- [Detections without the heavy lifting](https://www.elastic.co/security-labs/ai-esql-detection-rule-creation#detections-without-the-heavy-lifting)
- [Walkthrough: Detecting Okta credential stuffing and account takeover](https://www.elastic.co/security-labs/ai-esql-detection-rule-creation#walkthrough-detecting-okta-credential-stuffing-and-account-takeover)
- [Opening the AI Agent rule creation flow](https://www.elastic.co/security-labs/ai-esql-detection-rule-creation#opening-the-ai-agent-rule-creation-flow)
- [Describing the detection in plain language](https://www.elastic.co/security-labs/ai-esql-detection-rule-creation#describing-the-detection-in-plain-language)
- [Reviewing and adjusting the generated rule logic](https://www.elastic.co/security-labs/ai-esql-detection-rule-creation#reviewing-and-adjusting-the-generated-rule-logic)
- [Previewing and enabling the rule](https://www.elastic.co/security-labs/ai-esql-detection-rule-creation#previewing-and-enabling-the-rule)
- [Impact on detection engineering workflows](https://www.elastic.co/security-labs/ai-esql-detection-rule-creation#impact-on-detection-engineering-workflows)
- [What's next](https://www.elastic.co/security-labs/ai-esql-detection-rule-creation#whats-next)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=From%20plain%20English%20to%20production%20rule:%20AI-native%20Elasticsearch%20ES|QL%20detection%20in%20Elastic%20Security&url=https://www.elastic.co/security-labs/ai-esql-detection-rule-creation)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/ai-esql-detection-rule-creation)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/ai-esql-detection-rule-creation&title=From%20plain%20English%20to%20production%20rule:%20AI-native%20Elasticsearch%20ES|QL%20detection%20in%20Elastic%20Security)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/ai-esql-detection-rule-creation&title=From%20plain%20English%20to%20production%20rule:%20AI-native%20Elasticsearch%20ES|QL%20detection%20in%20Elastic%20Security)
