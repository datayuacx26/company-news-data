---
schema_version: "1.0.0"
document_id: "55d48bf5cc20ccf9abc703ede3682b1ebdda25684234b9c4fd645b87b06dd90a"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/llm-benchmarking-agentic-soc"
published_at: "2026-08-04T23:59:59+00:00"
first_seen_at: "2026-08-03T17:38:59.274611+00:00"
fetched_at: "2026-08-03T18:22:19.433930+00:00"
content_hash: "sha256:d774876d868bcb28502d30678220da52cc24c6e2cb8776888aa25ca4358cf390"
---

# Benchmarking the Agentic SOC: How we evaluate LLMs for security workflows

3 August 2026 •


[Dhrumil Patel](https://www.elastic.co/security-labs/author/dhrumil-patel)


# Benchmarking the Agentic SOC: How we evaluate LLMs for security workflows


Public leaderboards can't tell you which LLM to trust in your SOC, so Elastic built an evaluation framework that grades models on the work (tool calls, execution traces, blind judging) across Agent Builder, Attack Discovery, and automatic migration.


15 min read


[Generative AI](https://www.elastic.co/security-labs/category/generative-ai) ,


[Internals](https://www.elastic.co/security-labs/category/internals)


An agentic SOC is only as good as the model driving it. The moment you let an LLM triage an alert, hunt across your telemetry, or author a detection rule, the question stops being "is this a smart model?" and becomes something much more specific: will it pick the right skill, call the right tool in the right order, and reach the right disposition without inventing a result it never actually checked?


That is not a question a general-purpose leaderboard can answer. A model can top every public benchmark and still confidently tell you a malicious loader is "clean" because it narrated a VirusTotal verdict instead of calling VirusTotal. In a SOC, that is not a rounding error. That is a missed intrusion.


So we built an evaluation framework to answer the question directly. It seeds a realistic intrusion into a live Elastic deployment, drives every available model through the same set of security tasks against the same agent, captures not just what each model said but every tool it called and every parameter it passed, and then judges the results blind. This post explains how it works and why we built it the way we did. The results themselves are published and continuously updated in the[Large language model performance matrix for Elastic Security](https://www.elastic.co/docs/solutions/security/ai/large-language-model-performance-matrix) .


##


Why generic LLM benchmarks fail for agents for security


Public leaderboards measure knowledge and chat quality. They ask a model to recall facts, solve a puzzle, or write a tidy paragraph. Those are real capabilities, but they are the wrong proxy for agentic security work.


Inside an agent, the model is not writing prose. It is making decisions:


- Which skill does this task need? Alert triage and entity risk-scoring are different jobs with different tools.
- Which tool, with which parameters, in which order? A hash goes to VirusTotal; an on-call question goes to the schedule; a case gets opened once, with the right fields.
- Is the output grounded? Did the model actually run the query and read the result, or did it produce a plausible answer with an empty trace?


The most dangerous failure mode in an agentic SOC is the confident, fluent, wrong answer that was never grounded in a tool call. Generic benchmarks reward exactly that, because they only see the final text. To evaluate a SOC agent honestly, you have to grade the work, not the writing.


##


What we actually need to measure


We anchored the evaluation on the concrete capabilities a security analyst relies on, most of them built-in Agent Builder skills we shipped in Elastic Security. (For the product side of that story, see our companion post on the[five Agent Builder skills in Elastic Security](https://www.elastic.co/security-labs/skills-elastic-security-9-4) .) That gives us seven capability categories:


Category What it tests


Alert analysis Triage an alert, reach the correct disposition, pull related alerts, enrich with threat intel


Entity analytics Investigate hosts and users with purpose-built entity lookups and risk context


Threat hunting Generate and run queries against process, file, and network telemetry to find specific artifacts


Detection rules Author a working detection rule, grounded in research when asked


Workflow authoring Produce a valid, executable automation workflow (verified by actually running it)


Triggering workflows Call the correct backed action for the task (hash lookup, on-call, case creation)


Multi-step Chain several steps in the right order, carrying findings forward without skipping or fabricating


These categories deliberately straddle two levels of the agent. Alert analysis, entity analytics, threat hunting, and detection rules are built-in Agent Builder skills; workflow authoring exercises the platform workflow capability; and triggering workflows and multi-step response exercise the custom tools and the model's ability to chain them. So the suite measures both things that matter: whether the model routes to the right skill, and whether it then selects and orders the right tools.


Alongside the agent suite, we run two parallel evaluations that apply the same philosophy to different tasks. One is Attack Discovery, which uses an LLM to correlate raw alerts into coherent attack narratives. The other is Automatic Migration, which uses an LLM to translate detection content from another SIEM (Splunk, QRadar, or Microsoft Sentinel) into Elastic rules. Each has its own answer key, so we grade them separately.


##


The framework, end to end


The whole system is one automated flow. Seed the right data, point every model at the same agent and the same prompts, capture the full trace of what each model did, then anonymize and judge.


The design principle throughout: hold everything constant except the model. Same agent, same skills and tools, same data, same prompts. The only variable that changes between runs is which model is answering. That is what makes the comparison fair.


##


Seeding a realistic range


You cannot benchmark an investigation without something to investigate. Real product telemetry is noisy, sensitive, and non-reproducible, so we generate a synthetic intrusion instead.


The scenario is the Chrysalis backdoor campaign we detailed in our[earlier Attack Discovery](https://www.elastic.co/security-labs/speeding-apt-attack-discovery-confirmation-with-attack-discovery-workflows-and-agent-builder) post: a DLL side-loading attack on a Windows host,` srv-win-defend-01` , where a legitimate-looking` BluetoothService.exe` loads a malicious` log.dll` . A simulator seeds the alerts, endpoint telemetry, threat-intel documents, and an on-call schedule that make the scenario coherent enough for an agent to actually work through.


One methodology detail matters here. The "malicious loader" in the scenario carries the[EICAR test file](https://en.wikipedia.org/wiki/EICAR_test_file) hash. EICAR is the industry-standard harmless test string that every antivirus engine flags as malicious by design. Using it as the stand-in loader means VirusTotal returns a stable, unambiguous "malicious" verdict on every run, we never touch real malware, and the ground truth never drifts. A correct model reports "malicious, flagged by many engines." A model that says "clean" or never calls VirusTotal is wrong, deterministically.


There is a second dataset in play, a set of eight independent detection episodes producing roughly 95 alerts, which we use as the ground truth for Attack Discovery. And this is where the most important operational rule lives:


*The two suites need different, mutually exclusive alert data. Attack Discovery runs against the clean episodes-only index, because its answer key assumes exactly those threats are present. Agent Builder runs with the Chrysalis intrusion seeded on top, because its prompts reference Chrysalis entities. Mix them, and both answer keys become meaningless.*


Getting this wrong does not throw an error. It silently corrupts the results. So the harness treats the index state as a first-class step: prep the correct data, print the alert counts, and pause for an explicit go-ahead before a single model request is sent.


##


The agent under test


Every model drives the same agent: the full Elastic AI Agent, with all of Elastic Security's built-in skills enabled (alert analysis, threat hunting, entity analytics, detection-rule editing, and more), extended with five custom, workflow-backed tools from the Chrysalis scenario. Skills and tools sit side by side on the agent. The built-in skills carry the investigative capabilities, and each one, when the model reaches for it, unlocks its own underlying tools. The five custom tools below bolt on the response actions the scenario needs:


Tool What it does


vt.hash.lookup Analyze a file hash with VirusTotal


check.on.call.schedule Query the on-call schedule for the current responder


create.case Open a case in Elastic Security


create.channel Create a Slack channel for incident coordination


get.time Get the current time for naming and timestamps


These are live tools hitting live systems, including a real Slack connector. When a model decides to open a case or spin up an incident channel, it actually happens. That is deliberate: we want to measure what the model does when the tools are real and the side effects are real, not whether it can describe calling them.


Because those side effects accumulate, the harness cleans up after every call. Authored validation workflows are deleted, and any Slack channels the agent created are archived through the same connector the agent used. Cases are intentionally kept for later inspection.


The agent is given a two-phase playbook in its instructions. First a **confirmation phase** : verify the hash on VirusTotal, hunt the logs for corroborating activity, ground the finding in research, and reach a verdict. Then a **response phase** , entered only when the threat is confirmed: get the time, find the on-call responder, open a case, and spin up an incident channel. It is explicitly told not to open cases or channels for benign or low-confidence findings unless asked. That structure gives every model the same operating procedure, so what we are measuring is how well each one executes it, not whether it happened to guess the workflow.


The model routing is the elegant part. Every model on the deployment is exposed as an inference connector, and the converse API takes a` connector_id` per call. So swapping models is a one-field change. The agent, its tools, its instructions, and the data underneath it never move.


##


The prompt matrix


The suite is 21 prompts: three variants for each of the seven capabilities, all revolving around the same Chrysalis incident so the run reads like one coherent investigation rather than disconnected trivia. Prompts are written the way an analyst actually talks, and some carry attachments, the latest alert document or a detection rule to edit, so the model has real context to work from.


The three variants per capability are not just repetition. They escalate the difficulty in a deliberate way:


- **Variant A is the clear single-skill ask.** The task names what it wants, and there is one obviously right tool for the job.
- **Variant B is realistic.** It reads like a real analyst request, with enough context to be actionable but no explicit instruction on which tool to reach for.
- **Variant C is ambiguous, routing under pressure.** It is underspecified or slightly misleading, so the model has to infer intent and still pick the right capability. This is where weaker models overreach or grab the wrong tool.


To make the escalation concrete, here is one capability, alert analysis, across all three variants:


- **A (clear):** "Take a look at this alert and tell me what's going on. Is it something I should worry about, and what's your recommended disposition?" (latest alert attached)
- **B (realistic):** "We've got alerts firing on srv-win-defend-01. Triage what's in the alert queue for that host, are these related to the same incident, and which entities do they share?"
- **C (ambiguous):** "I'm seeing noise on srv-win-defend-01. Is this a real threat or a false positive? Pull in any threat intel that would help me decide."


Same capability, escalating difficulty: A names the task and the right tool is obvious, B forces the model to infer intent from context, and C is underspecified and slightly misleading, so the model has to resist the "false positive" framing and still investigate.


A few representative prompts across the other capabilities:


- **Threat hunting:** "Hunt for signs of the Chrysalis backdoor on srv-win-defend-01. Look for BluetoothService.exe execution, suspicious DLL loads, and any C2 network or DNS activity. Walk me through what you discover."
- **Triggering workflows:** "The Chrysalis loader hash is <loader-hash>. Check this hash against VirusTotal and tell me the verdict, how many engines flagged it and what threat name."
- **Multi-step:** "There's a confirmed Chrysalis incident on srv-win-defend-01. Verify the loader hash on VirusTotal, check the on-call schedule, open a critical Security case, then spin up a Slack incident channel with the on-call responder and post the case summary and top IOCs."


The multi-step prompts are where models separate. They require ordering, carrying a finding from one step into the next, and knowing when not to act. One variant explicitly says "don't escalate if it's benign," which tests judgment: since the scenario is a true positive, the correct behavior is to escalate, and a model that bails out has misread the evidence.


##


What a single LLM evaluation run looks like


Before we scale to every model, it helps to watch a single prompt run end to end. The harness dispatches the prompt to the converse API, the agent routes to the right skill, and from there the work is real. It verifies the loader hash on VirusTotal, generates and runs an ES|QL hunt that turns up the corroborating events, checks the on-call schedule and resolves the responder in Slack, opens a critical case, and spins up an incident channel with the verdict and the top IOCs. Then the harness verifies the side effects and archives the channel it created. Every step, every tool call, every parameter and result, is captured as it happens.


That is one prompt. A full sweep is 21 prompts against every model in the catalog, more than 500 evaluated conversations, each one recorded in full.


##


Capturing tool traces, tokens and latency in agent workflows


This is the part that makes the framework more than a chatbot bake-off. For every single call, we capture the final answer and the full execution trace: the ordered steps the agent took, each reasoning turn, each tool it invoked with the exact parameters, the tool's result, plus token usage, latency, and the conversation and round identifiers. When a task routes through a built-in skill, the trace shows the model loading that skill first and then calling the tools the skill unlocks; when it uses a custom tool like` vt.hash.lookup` , the call shows up directly. Either way, the tool calls are the evidence.


Here is a real captured step from a triggering-workflows prompt, trimmed for length. The model was asked to verify the loader hash on VirusTotal:


```text
{
"steps": [
{
"type": "reasoning",
"reasoning": "The user provided a file hash for the 'Chrysalis loader' and requested a VirusTotal verdict, including the number of flagging engines and the threat name. I will use the vt_hash_lookup tool to retrieve this."
},
{
"type": "tool_call",
"tool_id": "vt.hash.lookup",
"params": { "hash": "275a021b...fd0f" },
"results": [
{ "last_analysis_stats": { "malicious": 65, "undetected": 3 } }
]
}
],
"response": {
"message": "The hash was flagged by 65 engines on VirusTotal..."
}
}
```


That trace is the evidence. It lets us answer the questions a final answer alone cannot:


- Did the model actually call` vt.hash.lookup` , or did it assert a verdict with no tool call? (If the latter, the answer is unsubstantiated, no matter how confident.)
- Did it use the purpose-built entity tools, or fall back to a generic search and invent a risk score?
- For threat hunting, is there an executed query in the trace, or just a claim of findings?


For workflow authoring, we go one step further and capture objective ground truth. When a model writes a workflow, the harness actually creates it, enables it, runs it, and polls the execution to a terminal status. The result is not a judgment call about whether the YAML "looks right." It either validated and executed against the live system or it did not.


##


Blind LLM evaluation: how we remove model bias from scoring


Once the matrix has run, every model has produced 21 traces. Scoring them by hand, knowing which vendor produced which output, is a recipe for bias. So we anonymize first.


A deterministic obfuscation step strips the model names and relabels everything Model A, Model B, Model C, and so on, consistently across every report and across both suites, so the same model wears the same label everywhere. It also scrubs identity giveaways out of the traces themselves, the connector ids and slugified vendor names that leak into things like workflow tags, while leaving legitimate vendor mentions in tool output intact. With more than two dozen models in a run, the judge, whether a human or an LLM, sees only the labels and the traces.


The judging rubric grades every capability on a 1 to 10 scale and weighs two things at once:


- **Answer quality:** correctness against the ground-truth answer key, soundness of the disposition, depth, and actionability.
- **Tool and skill selection:** did the model call and use the right tool for the job, in a reasonable order, without a pile of irrelevant calls or fabricated results?


And the rule that keeps the whole thing honest:


*No tool call, no credit. A plausible answer produced without the supporting tool calls the task required is capped at 6 out of 10, because the result is unsubstantiated. A confident wrong answer scores lower than a hedged correct one. Evidence over eloquence.*


Reliability is tracked separately from quality. Some attempts error out or time out; those are counted as reliability failures and excluded from the quality average rather than being silently folded in. A model that is brilliant when it works but fails a third of the time is a different proposition from one that is merely good but never falls over, and the report shows both.


##


What this surfaces that leaderboards miss


Because we grade the trace and not the prose, the framework consistently surfaces failure modes that a text-only benchmark would score as passing:


- **Fabricated verdicts.** A model asserts a hash is malicious, or benign, without ever calling VirusTotal. Fluent, confident, and unsubstantiated.
- **Skipping purpose-built tools.** For entity questions, a model leans on a generic search and invents a risk score instead of calling the entity lookups that would return the real one.
- **Workflows that do not run.** Authored YAML that reads perfectly but fails validation or execution against the live system. The prose was fine; the automation was broken.
- **Reasoning strong, tool use weak.** Some models narrate an excellent investigation plan and then do not execute it. Others quietly do exactly the right thing with minimal commentary. Only the trace tells them apart.


The headline finding is not "model X wins." It is that ranking flips depending on the capability. A model that is excellent at open-ended threat-hunting reasoning can be middling at the disciplined tool selection that multi-step response demands. The right model for an agentic SOC is a per-capability decision, and this framework is what lets you make it with evidence instead of vibes.


##


Evaluating Attack Discovery: how LLMs correlate alerts into attack narratives


The Attack Discovery suite runs the same philosophy on a different task. Attack Discovery has no tools; the model works purely from the alert context it is handed. Every model gets the identical clean corpus of roughly 95 alerts, which we constructed to contain exactly eight independent intrusions, one per host, spanning macOS, Windows, and Linux and built from real, publicly documented malware families. That number, eight, is the single most important figure in the evaluation. A top-tier model produces eight clean discoveries. Weaker ones surface a fraction of them, merge distinct intrusions into one blurry narrative, or over-split a single attack into several.


We grade the output on four axes, each on the same 1 to 10 scale:


- **Coverage:** how completely the eight intrusions are accounted for, penalizing both misses and threats hidden by merging.
- **Accuracy:** correct host, malware family, kill chain, and MITRE mapping, with fabrication penalized hard.
- **Correlation quality:** the right alerts grouped together, without merging separate intrusions or over-splitting one.
- **Actionability:** usable titles and summaries, a correct entity picture, and sensible risk prioritization.


The guiding principle is that a clean, accurate eight beats a noisy twelve. Surfacing extra "discoveries" that are really fragments of the same attack is not thoroughness, it is noise an analyst has to untangle. As with the agent suite, latency is reported as a secondary signal and never folded into quality, because correlation speed matters operationally when Attack Discovery runs on a schedule, but a fast wrong answer is still wrong.


##


Evaluating Automatic Migration: Using LLMs to Port Detections from Other SIEMs to Elastic


The third suite leaves the agent behind entirely and evaluates a different LLM-powered capability:[Automatic Migration](https://www.elastic.co/docs/solutions/security/get-started/automatic-migration) , the feature that ports detection content from other SIEMs into Elastic such as[Splunk](https://www.elastic.co/blog/automatic-migration-ai-rule-translation) ,[QRadar](https://www.elastic.co/security-labs/from-qradar-to-elastic) , and, as of 9.5 release,[Microsoft Sentinel](https://www.elastic.co/security-labs/sentinel-detection-rules-migration) . Here the model is not calling tools in a loop, it is doing a bulk translation job, and we grade it on how faithfully it lands. We evaluated it on a Splunk package for this run; the same method applies to any supported source.


The input used are real Splunk detection rules: 52 saved searches built on Sysmon and Windows event telemetry, spanning credential access, defense evasion, discovery, and persistence. Every model migrates the identical package, so the comparison is apples to apples. The wrinkle that makes this hard is macros. Splunk searches lean on reusable macros, and 33 of the 52 rules are macro-only, meaning their entire detection logic lives behind a single macro reference like` sysmon_detect_dcsync_execution` rather than in the search text itself. So we hand the feature both files it needs, the 52 searches and the 200 macro definitions they depend on, and a faithful migration has to resolve that nesting instead of translating the surface text.


Scoring is deliberately simple and objective. Each of the 52 rules earns a score based on translation fidelity:


Score Result What it means


1.0 Full translation The Elastic rule reproduces the Splunk detection end to end, with macros expanded, filters preserved, and fields correctly mapped


0.5 Partial translation The core intent is captured but with gaps, such as an unresolved macro, a dropped filter or eval, or an incomplete field mapping


0 Miss Not translated or translated into something that does not reflect the original detection


Sum the per-rule points, divide by 52, and multiply by 10 to land on the same 1 to 10 scale as the other suites. The result is a single, legible number for a genuinely messy task: what fraction of a real customer's detection library would survive an automated migration, and how much of it would land clean versus need an analyst's rework.


##


Key takeaways


- **Grade the work, not the writing.** In an agentic SOC, the tool trace is the evidence. Judging final text alone rewards the most dangerous failure mode there is: the confident, ungrounded answer.
- **Realistic data plus ground truth is non-negotiable.** A reproducible synthetic intrusion with a known answer key, and strict discipline about which dataset backs which suite, is what makes the results mean anything.
- **Hold everything constant except the model.** Same agent, same skills and tools, same prompts, one-field model routing. Fairness comes from the setup, not the scoring.
- **Capture the whole trace.** Steps, tool calls, parameters, results, tokens, latency, and objective workflow validation. You cannot diagnose a failure you did not record.
- **Judge blind, and separate reliability from quality.** Anonymize before scoring, cap unsubstantiated answers, and never average a model's crashes into its quality score.
- **The best model is per-capability.** Rankings flip across alert triage, hunting, rule authoring, and multi-step response. Evaluate for the job you are actually asking the model to do.


##


Conclusion: Why evidence-based LLM evaluation matters for security use-cases


Three suites, three very different jobs, one standard of evidence. The agent suite measures whether a model can route to the right skill and drive the right tools in the right order. Attack Discovery measures whether it can turn a wall of alerts into the true set of intrusions, no more and no fewer. Automatic Migration measures whether it can faithfully port a detection library it did not write. Because all three land on the same blind-judged, 1 to 10 scale, every new model that ships gets a complete security profile in one view: where it is strong, where it fails, and whether those failures are quality or reliability.


That evidence is not academic. It is how we decide which models to put in front of security teams, and how we keep re-checking that decision as the model landscape shifts. The agentic SOC is only as trustworthy as the evidence behind the model you put in it. This framework is how we generate that evidence, so that when we say a model is ready to triage your alerts, we can show our work, and we keep the current results live in the[Large language model performance matrix for Elastic Security](https://www.elastic.co/docs/solutions/security/ai/large-language-model-performance-matrix) .


#### Jump to section


- [Why generic LLM benchmarks fail for agents for security](https://www.elastic.co/security-labs/llm-benchmarking-agentic-soc#why-generic-llm-benchmarks-fail-for-agents-for-security)
- [What we actually need to measure](https://www.elastic.co/security-labs/llm-benchmarking-agentic-soc#what-we-actually-need-to-measure)
- [The framework, end to end](https://www.elastic.co/security-labs/llm-benchmarking-agentic-soc#the-framework-end-to-end)
- [Seeding a realistic range](https://www.elastic.co/security-labs/llm-benchmarking-agentic-soc#seeding-a-realistic-range)
- [The agent under test](https://www.elastic.co/security-labs/llm-benchmarking-agentic-soc#the-agent-under-test)
- [The prompt matrix](https://www.elastic.co/security-labs/llm-benchmarking-agentic-soc#the-prompt-matrix)
- [What a single LLM evaluation run looks like](https://www.elastic.co/security-labs/llm-benchmarking-agentic-soc#what-a-single-llm-evaluation-run-looks-like)
- [Capturing tool traces, tokens and latency in agent workflows](https://www.elastic.co/security-labs/llm-benchmarking-agentic-soc#capturing-tool-traces-tokens-and-latency-in-agent-workflows)
- [Blind LLM evaluation: how we remove model bias from scoring](https://www.elastic.co/security-labs/llm-benchmarking-agentic-soc#blind-llm-evaluation-how-we-remove-model-bias-from-scoring)
- [What this surfaces that leaderboards miss](https://www.elastic.co/security-labs/llm-benchmarking-agentic-soc#what-this-surfaces-that-leaderboards-miss)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=Benchmarking%20the%20Agentic%20SOC:%20How%20we%20evaluate%20LLMs%20for%20security%20workflows&url=https://www.elastic.co/security-labs/llm-benchmarking-agentic-soc)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/llm-benchmarking-agentic-soc)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/llm-benchmarking-agentic-soc&title=Benchmarking%20the%20Agentic%20SOC:%20How%20we%20evaluate%20LLMs%20for%20security%20workflows)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/llm-benchmarking-agentic-soc&title=Benchmarking%20the%20Agentic%20SOC:%20How%20we%20evaluate%20LLMs%20for%20security%20workflows)
