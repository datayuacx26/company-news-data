---
schema_version: "1.0.0"
document_id: "e9b013f66aa1a2b5b431daf15dd23f457171e64c4c01db9793f7b2ca16fec544"
company_key: "yc-escape"
company: "Escape"
source_id: "yc-escape-rss-d1370620bfa7"
canonical_url: "https://escape.tech/blog/claude-in-security-engineering-from-proof-of-concept-to-production/"
published_at: "2026-06-08T12:26:12+00:00"
first_seen_at: "2026-07-20T23:23:55.281376+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:44901947b8d2330ec015c6cc7295852aa00590e75c50afcfb8af938f2a30b18b"
---

# Claude in Security Engineering: From Proof-of-Concept to Production

> “This is not a knowledge problem or a work ethic problem. The mathematics just doesn’t work when it’s one or two people dealing with the expectation of a whole team.” - **Geet Pradhan, Senior Security Engineer**


Being outnumbered and underpowered is the all-too-familiar reality for most security engineering teams, and everyone is particularly talking about harnessing Claude to overcome just that.


But how do you effectively integrate it into security engineering while facing token limits, choosing from various models, and is it ultimately just hype?


That’s exactly what Geet Pradhan, a Senior Security Engineer, and Tristan Kalos, CEO and Co-Founder of Escape, tackled in this webinar. They run through three key use cases for Claude:


- Proactive security and threat modeling
- Detection engineering
- Incident response


replete with practical examples, strategies for supporting offensive security workflows at scale, and outlining exactly which models to use when.


Tristan then runs through a live demo of Escape’s Claude integration, including how to discover crown-jewel assets, run human-grade pentests, triage findings and hand developers remediation code, all within the assistant.


You can watch the full webinar here and access[Geet's](https://drive.google.com/file/d/1GiYxnvIpOp5owBODOYu23_KoY5WGuyIJ/view?usp=sharing&utm_source=hs_email&utm_medium=email&_hsenc=p2ANqtz--Fj7wgjt0jd--fP75HI7j-iB6H-FTk6vb5nkSfbl0AXjXspJR9EdZciHRZSCQAy0UlmXTr) and[Tristan's](https://drive.google.com/file/d/1-foGP51jc5hsDVSKYWZCCunFYQmsekfZ/view?_hsenc=p2ANqtz--Fj7wgjt0jd--fP75HI7j-iB6H-FTk6vb5nkSfbl0AXjXspJR9EdZciHRZSCQAy0UlmXTr) slides:


## Meet the speakers


**Geet Pradhan** has built security programs from the ground up in high growth organisations, particularly building significant security automation. With deep expertise in SecOps, Product Security, and Security Engineering, he’s architected security functions that actually align with business goals and not just compliance checkboxes.


**Tristan Kalos** is the co-founder and CEO at Escape, drawing from a background as a software engineer and machine learning researcher at UC Berkeley. Escape is an offensive security engineering platform that provides tooling that spans the entire security cycle, from automated discovery to remediation.


## AI's role in security engineering


One clear pattern emerges when considering where AI stands in security: it works best as a force multiplier for teams, not as a replacement.


> "Don't think about it as AI doing security. Think about it as having an always on, always present advisor whom you can ask questions to." - **Geet Pradhan** , **Senior Security Engineer**


AI is essentially an engineer that covers all of the gaps that you don't have expertise in, whether that's the LLM Top 10, OWASP Top 10 vulnerabilities, SOC2 Controls, and so forth. But it can also reason and find insights based on the specific problem you're defining instead of purely pattern matching.


Finally, AI is really good at producing and reviewing standard security outputs, like threat models, rules, detection engineering, and incident response, which is what this article will dive further into.


## Which model to use when


Geet's overview of the tradeoffs between Claude models


As a general overview, Haiku is traditionally cheaper and suits needs best when you want to be cognizant of costs.


Sonnet would be the daily go-to which does everything slightly better than Haiku but is still cheaper than Opus.


Opus is overall the best at reasoning, judgement, and exceeds Sonnet in most things but it is also significantly more expensive so this tradeoff depends on your budget and needs.


## Threat modeling with a multi-agent architecture


💡


You can find all the prompts, agents, and examples Geet runs through at his[GitHub repo link](https://github.com/Gprad-Work/claude-security-agents?utm_source=hs_email&utm_medium=email&_hsenc=p2ANqtz--Fj7wgjt0jd--fP75HI7j-iB6H-FTk6vb5nkSfbl0AXjXspJR9EdZciHRZSCQAy0UlmXTr)


While security workflows vary by company, there's generally a manager who takes in work, delegates across the security team, then the findings are consolidated and reviewed.


Geet's proactive security agential architecture


Geet built an agentic architecture that replicates this, creating an agentic persona for different types of domain knowledge. He created a local skill - /security-review - which reads all of the artifacts you give it, from code to documents to whiteboard sketches, and from that analyzes the information to break it up into different lenses.


Instead of going one domain at a time and rewriting code with fixes, Claude agents can simultaneously review the same artifact from different perspectives (AI Security, DevSecOps, GRC) and findings are spec edits not code rewrites.


Here's how it works:


- **Top-level triage agent (Opus):** This takes all of the artifacts you feed it and reasons about what kind of review is needed and which specialist agents to dispatch.
- **Parallel specialist sub-agents:** These dispatched agents then run simultaneously, each through their own lens whether that's GRC (GDPR, SOC2 or other regulatory requirements), network security, AI security, etc. Each agent produces its findings in a structured format with a description, severity rating, and mitigations.
- **Security lead synthesis (Opus):** This agent collects all of the findings from above, deduplicates, recalibrates severity, and then delivers a single consolidated report. The same findings can become surfaced by multiple agents, so synthesis is vital to avoid noise.


### WhatsApp bot example


Geet ran his /security-review architecture on a WhatsApp bot he created called Pesti to streamline communications with his partner. Running 5 agents on 9 documents, it only took 3 minutes to run and discovered important gaps spanning prompt injections, SecOps, and DevSecOps.


Interestingly, the agents surfaced 96 findings but after deduplication, it came down to 44. This is where the security architecture is crucial to creating a final list of actionable findings.


### Which model to use when


As an agent that excels in reasoning and open-ended tasks, Geet found **Opus** is best-placed as the top-level triage and the synthesis agent. These are the two agents that most require the ability to deliberate on what is required from artifacts, what findings are acceptable, which are duplicates, and how to work in a space where there are lots of unknowns.


He then recommended **Sonnet** for the specialist sub-agents as a model that is really good when working with a lot of known patterns, such as known vulnerability lists, and a relatively fixed answer space.


While there remain unknowns in this case, running Opus on all agents was found to **cost twice as much** ** as using Sonnet for the sub-agents. Interestingly, Sonnet also found more criticals because it classifies more aggressively than Opus.


It's important to note that Opus is the better model for all three, and Geet saw that using Opus as the sub-agents found 7 to 8 more findings though they were all medium to low in severity. The tradeoff is Opus then uses more tokens, runs slower, and requires more guidance to maximise its output because it reasons more and so can be prone to head in a different direction than you want it to.


Overall, Opus for triage and synthesis, and Sonnet for specialist sub-agents is the smartest assignment but if you have the tokens, time, and infrastructure to guide Opus, using it across the architecture could be beneficial for uncovering more findings.


## Detection Engineering


> "What I think Claude has been a game changer for me to scale my own detection engineering platforms and my own pipelines is it's really good at doing a lot of the stuff I am really bad at." - **Geet Pradhan, Senior Security Engineer**


Detection engineering is one of the highest-leverage use cases of AI in security because of how tedious it can be to nail the exact format, logic, and coverage gaps unique to your environment. This usually relies on synthesising different resources, detection libraries and nailing validators, all of which are dependent on code still written by someone. Fortunately, AI is really good at doing just that, writing rules, validators, and coverage reports that remove the subjective judgement of individuals.


Geet proposed a three-agent architecture to use in sequence for an optimal detection engineering workflow:


- **Detection rule writer:** This drafts the rule in whatever format your SIEM uses (Sigma, KQL, Splunk queries). It looks through the structure of your detection rules folders, determines the category, create a UID and then write the rule based on the fields you're giving it. You naturally have to fine-tune it slightly but Claude is overall quite skilled at writing detection rules.
- **Efficacy reviewer:** Here you essentially get Claude to rate its own work. Importantly, just writing a prompt telling Claude to review its work doesn't stick so you need to create another agent to apply a 'peer review' framework that tests for correctness, threshold, time frame, false positives and accuracy for what kind of rule it is and whether it is effectively calibrated to your environment.
- **Coverage scanner:** This maps your entire rule library against the MITRE, ATT & CK framework to surface any coverage gaps.


Alongside the efficacy reviewer it is also important to add in **non-AI schema validators** - Geet uses a Python script - as an extra layer of verification to see if your rule is missing a field, tag, author, or any other requirements.


Demo of Geet's detection rule agents


### Which model to use when


**Sonnet** works well as the **detection rule writer** because in this task it is already given a lot of context and YAML and MITR ATT&CK tags are fairly standard for it to apply.


**Opus** as a reasoning tool is more optimal for **efficacy reviews** to make the operational judgements of whether a rule will be effective or too loud in your specific environment.


**Both Opus and Sonnet** can be effective for **coverage scans** so it can depend on your time and token limits.


## Incident Response: from 60 minutes to 45 seconds


The pressure is naturally highest in reactive security and this is where AI can create some of the most vital time saving. Typically incident triage without AI looks like opening the SIEM, pivoting across log sources, running queries, correlating what you're seeing, writing a ticket and forming a hypothesis. Depending on the complexity of the issue that's 30 to 90 minutes of contemplation to even establish a direction.


Geet's /triage skill setup


Claude and MCP access to your SIEM turns that process into one that takes less than a minute. Geet's triage workflow for incident response uses three agents in the following sequence:


- **Alert parser:** This agent structures the incoming alert, identifies the relevant fields, and makes sense of the available raw data.
- **SIEM investigator:** This queries your SIEM for historical context, looking back 10-30 days to see whether this pattern has happened before or if there are related events that change the context of the alert. f your SIEM exposes an MCP server, a CLI, or an API token, this agent can query it directly. Geet highlighted RunReveal specifically as a SIEM worth trying for this workflow — it has native MCP support built in, which removes the integration overhead.
- **Incident response analyst:** This agent synthesises the structured alert and the historical evidence to deliver a triage verdict of whether it is a false positive, a true positive with recommended next steps, or a confirmed incident that requires escalation.


This trigger for these agents can be manual (a command you run when an alert comes in), a webhook from your alerting platform, or a fully automated pipeline. The agents run in parallel, not in sequence, which is most of where the time saving comes from.


### Example:


In the webinar, Geet ran through an alert which, in isolation, looked like suspicious repository activity. Running the three-agent workflow had 4 tool calls, 89 evidence items reviewed across the alert and historical log data, all within 45 seconds. Ultimately, it was a false positive. The SIEM investigator went back into the last 10-20 days of activity and found the pattern was consistent with standard repo initialisation behaviour. The IR analyst agent then took the evidence from the alert, applied the context from the SIEM investigator, and closed the alert.


While this would have taken a trained analyst up to an hour of manual investigation, it was achieved in under a minute. Crucially, the final agent delivers a report explaining the reasoning which analysts can then override if they disagree with the call.


> "Whatever you can do to save time while actually running the incident or running a response task — that is great in my books." - **Geet Pradhan, Senior Security Engineer**


### Which model to use when


**Sonnet** works best for the alert parser and SIEM investigator since both agents are working with structured, well-defined inputs (the alter fields and SIEM queries) and so can conduct these pattern-recognition tasks at a lower cost.


**Opus** best serves the requirements of the IR analyst as the agent making the judgement call, accounting for environment-specific context and distinguishing real incidents from false positives.


While you could run all three agents on Opus there would not be a significant quality difference in the first two in doing so. Opus is most crucial as the IR analyst making the decision.


## What AI gets right in security and what it doesn't yet


> This is a double-edged sword paradox of AI and cybersecurity because the tools at our disposal today are more powerful than ever, yet security teams have never felt as outnumbered by the amount of code that is produced, the pace of evolution of the technical stack and the new type of attacks that are being created" - **Tristan Kalos, CEO and Co-Founder of Escape**


As we've seen so far, AI is extremely good at research, analysing existing data and context, reasoning on existing findings or finding new vulnerabilities. However, where it struggles is with scale, repeatability, false positive discipline, and its process.


When conducting continuous analysis of thousands of data points then token usage explodes and limits kick in, restricting the scale and repeatability of AI-powered security testing.


The same prompts can also return different results, so automation at scale can create a lot of noise. Similarly, models can also exaggerate the severity of noise, meaning there is still a manual review needed in every loop.


Finally, AI tends to favour ad hoc processes over standardized guides, which can prove particularly problematic with distributed engineering teams and centralized security teams.


So how do you leverage AI in security to multiply outcomes without scaling headcounts or burning through tokens?


## Escape x Claude Integration


Escape's engineering principles


The answer to the above question is implementing offensive security as an engineering discipline rather than a point-in-time assessment. The prevalence of AI in development and security processes means any tooling for security engineering needs to give AI agents and custom scripts the same access and feature set that a human has access to.


> Everyone has access to the same models, including the attackers. It's using the models 1000 times better than attackers. It's using the models better to stay ahead and have the right level of security. - **Tristan Kalos, CEO and Co-Founder of Escape**


The Escape platform exposes an MCP server, a public API, and a CLI, which means Claude Code and Claude agents can trigger pen tests, pull findings, apply remediations, and query attack surface data directly, all without touching the UI.


💡


Find in-depth documentation of Escape's Claude integration[here](https://docs.escape.tech/documentation/remediate/escape-for-claude/?_hsenc=p2ANqtz--Fj7wgjt0jd--fP75HI7j-iB6H-FTk6vb5nkSfbl0AXjXspJR9EdZciHRZSCQAy0UlmXTr) .


There are three particular use cases for what this looks like in practice:


### Automated penetration testing


All you need to do is pass Claude Code a URL, username, and a password and tell Claude to setup a scan and a penetration test. Claude then passes this information to Escape and the platform then handles the rest, which visits the application, identifies the login flow, authenticates, and launches a fleet of AI agents to perform a full security assessment spanning[business logic flaws](https://escape.tech/blog/what-is-business-logic-and-why-its-important/) ,[authentication issues](https://escape.tech/blog/complex-authentication-in-dast-scans/) ,[injection vulnerabilities](https://escape.tech/blog/sql-injection-in-graphql/) , and more.


💡


We just launched our new AI pentesting engine[Project Cascade](https://escape.tech/blog/introducing-cascade-the-multi-agent-penetration-testing/) , which becomes an expert in your business.


For AppSec teams running assessments across multiple applications, this collapses a process that previously required significant manual setup into a single prompt.


### Automated remediation


Once pen test findings are populated in the Escape platform, Claude Code can pull the findings directly from Escape - complete with reproduction steps, severity ratings and remediation guidance - and then locate the vulnerable code in the repository, apply the fix, and generate a report of what was changed.


Demo of Claude pulling remediation code from Escape and implementing it autonomously


You can also then tell Claude to trigger a retest in Escape to confirm the fixes held and no regressions were introduced. The entire loop, from finding the fix to remediation, can be conducted autonomously.


### Attack surface analysis at scale


Analyze your entire attack surface with a single prompt


Even after conducting penetration tests and implementing remediations, you still need to analyze the entire attack surface both internal and external. Escape continuously discovers and evaluates assets across both internal and external networks -[web applications](https://escape.tech/blog/escape-dast-web-app-algorithm/) ,[REST APIs](https://escape.tech/blog/rest-security-testing/) , anything exposed on the internet - and tracks status, accessibility, and vulnerability data for each one. But making sense of it all manually would take hours.


A single Claude prompt changes that. Pulling Escape's full attack surface data into Claude you can analyse the weekly evolution of your attack surface: dates assets are created, what's going on per domain, which endpoints are in production versus development, which are protected by a WAF, and so forth.


### Custom rules and the Escape tenant review


The real leverage comes from combining automated pen testing, remediation, and attack surface analysis and Escape's custom rules feature brings these three together.


[Custom rules](https://escape.tech/blog/writing-custom-security-tests/) let you create retests for specific vulnerabilities that have been found before or you can create your own. These can be written yourself, have Escape's agents generate them automatically, or import them from external sources such as bug bounty reports, third-party pen test findings, or any other data you have.


Tristan built a custom Claude Code command - the /escape-tenant-review - which performs a full analysis of all of the data in Escape, looking at existing issues, triaging them, removing irrelevant ones, and then performing a more in-depth analysis of all of the web applications and API services that Escape dynamically discovers on your attack surface. If anything is missed, the tenant command automatically creates the custom rules for Escape, so you could run it weekly, for instance, and have a full triage and full analysis of your entire attack surface, with custom rules automatically added for detection at scale.


💡


If you want to get a personalized walkthrough of the Escape platform,[book a demo](https://escape.tech/book-a-demo) now!


## Q&A


**How does AI-powered pen testing compare to traditional SAST and IaC scanning?**


They solve different problems: where SAST and IaC scanning analyse source code, that approach is static and misses everything that becomes visible only when applications are running, including business logic flaws, authentication edge cases and how the system behaves with real data flows through it.


As Tristan highlighted, Escape's approach is a replacement for offensive security, not for source code scanning. The two approaches complement each other: SAST catches what's in the code and dynamic pen testing catches what the code does when it's actively facing the real world. The most thorough security programs are increasingly using AI models for this in combination with traditional SAST tools.


**Should you feed threat models to your security agents?**


The short answer is yes, and Geet made the case for going further than a one-time input. His agential architecture produces threat models itself, which means you can feed prior runs back into future ones so threat models are updated continuously as your product changes.


Simply, the more context an agent has, the more accurate and relevant its findings will be. For example, S3 buckets may be intentionally public for legitimate business reasons but this would be continually unnecessarily flagged without that context.


However, don't feed in PII or anything that violates your AI usage policy. Within the guardrails of that policy, more context is always better across threat modeling, detection engineering, and incident response alike.


**I ran an AI audit across hundreds of repos and got thousands of findings. How do I actually manage that?**


AI models can exaggerate severity and flag issues that aren't actually exploitable or operationally relevant. Tristan advised beginning from the attacker's point of view. What is genuinely exploitable from the outside? Reducing your lens to this narrows the findings to what actually matters and builds credibility with engineering teams when you're only flagging real issues.


Geet also advised running a second pass before sending anything to engineering. Asking Claude to review its own findings can help filter through the noise before you erode trust.
