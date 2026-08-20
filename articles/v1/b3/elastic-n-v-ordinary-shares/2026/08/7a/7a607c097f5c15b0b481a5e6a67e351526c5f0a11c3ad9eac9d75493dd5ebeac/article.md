---
schema_version: "1.0.0"
document_id: "7a607c097f5c15b0b481a5e6a67e351526c5f0a11c3ad9eac9d75493dd5ebeac"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/ai-vulnerability-triage-bug-bounty-hackerone"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T16:53:44.245370+00:00"
fetched_at: "2026-08-04T17:31:37.256081+00:00"
content_hash: "sha256:1e777576b0f416361b1783caa37cbaa4d78e4259ed3e5599eafd372d9f236990"
---

# Agents vs. agents: how we triage HackerOne reports for $2 each, 85% as well as a human

4 August 2026 •


[Ioannis Kakavas](https://www.elastic.co/security-labs/author/ioannis-kakavas)


# Agents vs. agents: how we triage HackerOne reports for $2 each, 85% as well as a human


LLMs made it cheap to flood bug bounty programs with submissions. Here's how Elastic built an AI triage agent that matches human decisions 85% of the time, including the architecture, threat model and calibration against 3,300 real reports


15 min read


[Generative AI](https://www.elastic.co/security-labs/category/generative-ai) ,


[Internals](https://www.elastic.co/security-labs/category/internals)


Large language models (LLMs) made it trivially cheap to generate vulnerability reports. In the first half of 2026 alone, our[HackerOne](https://hackerone.com/elastic) bug bounty program received over 1,390 reports, more than the full-year totals for 2024 and 2025 combined. Every one of them still requires human attention, so we decided to put agents against agents. If AI can generate reports at near-zero cost, AI should triage them at near-zero cost, too.


The system we built agrees with human security engineers 85% of the time, validated against 764 known-outcome reports, with triage rules calibrated iteratively against our full corpus of over 3,300. A typical report costs roughly $2 to triage. It runs an eight-stage analysis pipeline, and then a separate adversarial review independently challenges every conclusion. When reproduction is warranted, findings are reproduced in sandboxed Elastic Stack environments on ephemeral virtual machines (VMs) that self-destruct after 30 minutes. A human still makes the final call on every report.[HackerOne](https://hackerone.com/) runs its own AI-assisted triage at submission time, and we use it as a first gate: Only reports that HackerOne's AI marks as` send_to_validation` reach our pipeline, so we're not paying to re-triage what HackerOne already handles well.


##


The problem: Triage doesn't scale linearly


Bug bounty programs have a structural scaling problem. The cost of *submitting* a report is near zero, but the cost of *triaging* one is not. A senior security engineer spends 30 to 60 minutes on a typical report: reading the submission, assessing validity against product-specific context, scoring severity, determining if reproduction is feasible, and often spinning up an environment to verify the claim. Multiply that across hundreds of reports per year, and triage becomes a significant operational cost.


At Elastic, our bug bounty program on HackerOne has historically received 600 to 850 reports per year. That number is rising sharply: In the first half of 2026, we saw a huge increase in the number of AI generated reports we receive. The majority of analyst time goes toward reports that will ultimately be closed as informative or not applicable. That was already a challenge before LLMs entered the picture. The 2026 spike is the thesis made concrete: AI lowering the cost of report generation to near zero directly shows up in submission volume, while the signal-to-noise ratio drops.


We set out to build an AI agent that could handle the mechanical parts of triage, flag the reports that genuinely need human judgment, and reproduce the rest automatically.


##


AI triage architecture: Two VMs, two skills, one orchestrator


The system breaks triage into two compute-isolated phases, each running on a separate ephemeral Google Cloud Platform (GCP) VM. This separation was a deliberate architectural choice, for cost (analysis needs a small machine; reproduction needs a larger one) and for security: The analysis VM never runs untrusted code, and the reproduction VM never needs access to the full report corpus or triage history.


**Phase 1: Analysis** runs on an` e2-standard-2` VM (2 vCPU, 8GB RAM).[Claude](https://www.anthropic.com/claude) processes the report through an eight-stage assessment pipeline, and then a separate adversarial review skill independently challenges the analysis. There are no Docker containers involved, and the system doesn’t execute any code. The VM shuts down after 30 minutes, regardless of state.


**Phase 2: Reproduction** runs on an` e2-standard-4` VM (4 vCPU, 16GB RAM) only when the analysis recommends it. This VM provisions an Elastic Stack via Docker Compose, executes researcher-described steps inside a sandboxed tester container, and interprets the results. The same 30-minute auto-shutdown applies here.


[Elastic Workflows](https://www.elastic.co/elasticsearch/workflows) orchestrates the entire flow: triggering on new HackerOne reports synced into Elasticsearch, managing the VM lifecycle, enforcing concurrency limits, posting triage results back to HackerOne as internal comments, and routing to human review. We built this on Elastic Workflows, generally available (GA) in Elastic 9.4, because we wanted to be Customer Zero for the product: exercising it under real operational load and feeding prioritized feature requests back to the Workflows team. HackerOne report data is already ingested into Elasticsearch through the[HackerOne integration](https://www.elastic.co/docs/reference/integrations/hackerone) , so the triage pipeline triggers from an Elasticsearch alert rule rather than requiring an inbound webhook.


###


Why not a single VM?


We considered running both phases on one machine. The problem is blast radius. The reproduction phase runs attacker-described steps, and if something goes wrong, the environment where the AI analyzed the report (with access to the HackerOne API credentials and report data) shouldn’t be the same environment executing untrusted reproduction scripts. Splitting the phases also lets us skip reproduction entirely for obvious rejects (roughly 70% of reports are rejected at analysis and never need reproduction), saving both time and cost.


###


Why ephemeral VMs instead of containers on a long-running host?


Containers share a kernel. For a system that intentionally runs untrusted reproduction steps, we wanted stronger isolation. Each triage run gets a fresh VM from a[Packer](https://www.packer.io/) -built image, runs its phase, uploads results to Google Cloud Storage, and terminates. There’s no persistent state on the VM. If a reproduction attempt manages to escape the Docker sandbox, it lands on an ephemeral VM with no useful credentials (secrets are injected as environment variables and exist only in process memory) in a Virtual Private Cloud (VPC) with egress filtered through a Squid proxy. The blast radius of a successful escape is a throwaway machine that self-destructs in 30 minutes.


##


Vulnerability analysis pipeline: Eight stages with built-in skepticism


The core insight behind the analysis design is that vulnerability report triage isn’t a single judgment call. It decomposes into discrete stages, each with different failure modes. We encoded these as a structured Claude skill with Elastic-specific triage rules calibrated against our historical data.


###


Eight stages of AI-powered vulnerability triage


1. **Report summary:** Extract what the researcher claims, which product and version, what vulnerability type, and what the impact is supposed to be.
2. **Validity assessment:** Is this a real security issue or a misunderstanding of product behavior? This is where Elastic-specific rules matter most.
3. **Realistic exploitability assessment:** This is the most important stage. Instead of asking *Could this theoretically be exploited?* , the system asks *Who is the realistic attacker, what access do they already have, and does this give them something they don't already possess?*
4. **Product applicability:** Does this affect a product Elastic maintains? Is the version in scope per our end-of-life policy?
5. **Common Vulnerability Scoring System (CVSS) version 3.1 scoring:** Independent scoring with specific attention to metrics that analysts commonly get wrong (such as, Privileges Required too low, Scope incorrectly changed, or Confidentiality/Integrity/Availability inflated to theoretical maximum rather than realistic impact).
6. **Reproduction feasibility:** Can this be reproduced automatically? Which environment type (local Docker, Elastic Cloud, serverless)? Which product topology?
7. **Recommendation:** Reproduce, accept as theoretical, reject, or request more information from the researcher.
8. **Self-challenge pass:** Reexamine the analysis for researcher framing bias, severity anchoring, and whether the recommendation is consistent with similar historical reports.


###


Elastic-specific triage rules


Generic vulnerability analysis fails at Elastic because many "findings" are actually features working as designed. The triage rules encode both our[HackerOne program policy](https://hackerone.com/elastic) and the institutional knowledge that our security team has built over years of triaging reports. A general-purpose LLM wouldn’t have the following:


-


**Server‑Side Request Forgery (SSRF) as a feature:** Watcher HTTP input,` _reindex` with remote source, snapshot repository URLs, and inference endpoints all make HTTP requests by design. These are only vulnerabilities when a security control (like` reindex.remote.whitelist` or` xpack.http.whitelist` ) is bypassed, or when a non-admin user can trigger requests they shouldn't be able to.


-


**Denial of Service (DoS) policy:** A single crafted request that causes an out-of-memory crash is a valid product bug. Sending thousands of concurrent requests to overwhelm a service is volumetric abuse and out of scope. The distinction matters because roughly 40% of DoS reports we receive describe the latter.


-


**Information disclosure in open source software:** Source code, version numbers, stack traces, and JavaScript source maps aren’t vulnerabilities when the product is open source. This one category alone accounts for a significant share of rejected reports, and it trips up human analysts and AI models that lack this context.


-


**Category calibration:** The system knows the historical acceptance rate for each vulnerability category. Some categories have acceptance rates in the single digits; others are above 50%. This doesn't automatically reject any category, but it does raise the evidentiary bar for categories with low base rates and flag cases where the analysis is more generous than the historical baseline.


###


How the AI assesses realistic exploitability


The realistic exploitability assessment (stage 3 of the pipeline) deserves special attention because it’s where triage accuracy hinges. A report describes a vulnerability and its impact; the triage system must independently assess how realistic that impact is, given the prerequisites and deployment context. This is the same judgment call that human analysts make on every report, and it’s the stage most susceptible to anchoring on the report's framing rather than forming an independent assessment.


The analysis skill forces three concrete questions:


1. **Who is the realistic attacker?** Map the required access level to an actual user persona (such as anonymous user, authenticated user, space admin, cluster admin, or cloud account owner). A vulnerability reachable by an anonymous internet user is fundamentally different from one that requires cluster administrator privileges.
2. **Does the attacker already have equivalent access?** If exploiting the vulnerability requires privileges that already grant equivalent or greater access through legitimate means, the incremental risk is minimal.
3. **Does this require non-default configuration?** A vulnerability in a feature that requires explicit opt-in and administrative privileges to enable has a different risk profile than one affecting default installations.


The output is one of three assessments:` exploitable` (realistic attacker, meaningful impact),` constrained` (real bug but limited by prerequisites), or` theoretical` (requires conditions unlikely in production). This assessment directly influences the CVSS score and the final recommendation.


##


The adversarial review: Challenging your own AI


A single-pass AI analysis has an inherent problem: If the model makes a systematic error in judgment, there’s no mechanism to catch it. We added an independent adversarial review as a mandatory second stage, implemented as a separate Claude skill that has access to the original report but not the analysis output until it forms its own assessment.


The review process runs six checks:


1. **Independent CVSS scoring** before reading the analysis's score, to detect anchoring bias.
2. **Exploitability challenge:** Does the attacker profile make sense? Do the prerequisites already grant equivalent access?
3. **Triage rule audit:** Were Elastic-specific rules (such as SSRF as a feature, DoS policy, and open source disclosure) correctly applied?
4. **Researcher framing detection:** Did the analysis mirror the researcher's language instead of forming independent conclusions? Did the CVSS score "happen" to match the researcher's claimed severity?
5. **Category calibration check:** Is the recommendation consistent with the historical acceptance rate for this vulnerability category? An "accept" recommendation for a category with a low base rate should trigger additional scrutiny.
6. **Verdict:**` agree` (analysis stands),` adjust` (specific metrics corrected with reasoning), or` disagree` (flag for human review).


The` disagree` verdict is the critical safety mechanism. When the reviewer fundamentally disagrees with the analysis conclusion, the system doesn’t proceed autonomously. It holds the report for a human security engineer and presents both the original analysis and the reviewer's objections. In our validation testing, the adversarial review caught CVSS inflation (Privileges Required scored too low), missed triage rule application (SSRF as a feature not recognized), and severity anchoring to researcher claims in roughly 15% of cases.


##


Calibrating against 3,317 historical reports


Building the analysis pipeline and adversarial review gave us the structure, and making it accurate required calibrating against real data.


We built an evaluation pipeline that ingested our entire HackerOne report history: over 3,300 reports with known outcomes (accepted, rejected, or informative). For each report, we ran the triage skill and compared the AI's recommendation against what a human analyst had actually decided.


The process was iterative. We started with a 300-report stratified sample, ran the analysis, examined every false positive and false negative, added rules to address the patterns we found, and then reevaluated. Five calibration cycles moved accuracy on the sample from 75% to 84% and precision from 52% to 68%.


Each round revealed specific failure modes. The system wasn’t distinguishing between Kibana role-based access control (RBAC) boundary violations (valid findings) and admin-tier lateral access (already authorized). Public continuous integration (CI) systems being intentionally readable was triggering false positives. Third-party endpoint detection and response (EDR) actions routed through Elastic products are designed behavior, not vulnerabilities. Watcher SSRF with the default wildcard allowlist is a valid concern, but RFC 1918 IP access from external networks isn’t exploitable. Each of these patterns became an explicit triage rule.


Running the evaluation against the full corpus exposed category-level patterns that the 300-report sample had underrepresented. Injection had more false positives than expected, and cross-site scripting (XSS) had more false negatives because the skill was being too skeptical about XSS validity. Access control remained the hardest category. These findings drove a second round of improvements: We added few-shot examples from the historical corpus to anchor the model's judgment on ambiguous categories, restructured rule organization to reduce prompt attention drift (where adding new rules caused the model to attend less to existing ones), and tuned category-specific thresholds. The full-corpus evaluation was the beginning of a more targeted phase.


The gap between sample performance and full-corpus performance taught us the most important methodological lesson of the project: Small, stratified samples are directionally useful for rapid iteration, but they mask category-level failures that only become visible at scale. Every category where the sample underrepresented the true distribution produced surprises.


##


Reproducing vulnerabilities in a sandbox


When the analysis recommends reproduction, the system provisions an Elastic Stack environment and attempts to execute the researcher's described steps. This is the most security-sensitive part of the pipeline: You’re deliberately running steps described by an external party in an environment you control.


###


Threat model for AI systems processing untrusted input


The system processes attacker-controlled input at every stage. The primary threat starts the moment a report enters the pipeline. The full threat model covers:


- **Prompt injection via report text:** A report whose description contains instructions designed to manipulate the Claude agent into changing severity, approving a bounty, bypassing analysis steps, or leaking internal context. This is the most likely attack vector because it requires no special access.
- **Credential exfiltration:** Report text or reproduction steps that attempt to extract API keys, cloud metadata, or internal configuration from the analysis or reproduction environment.
- **Data poisoning:** Reports crafted to subtly manipulate the agent's triage patterns over time, shifting what it considers valid or invalid.
- **Sandbox escape:** Reproduction steps that attempt to break out of the Docker container to the host VM or out of the VM to the broader network.
- **Supply chain injection:** Reproduction steps that install malicious packages or download hostile scripts.
- **Resource abuse:** Triggering expensive cloud provisioning, cryptocurrency mining, or using the environment as a pivot for lateral movement.


###


Defense in depth for AI vulnerability triage


We layered defenses assuming any single layer could be bypassed:


Defense layer Protects against Mechanism


Prompt injection containment Report text manipulating the agent into changing severity, approving bounties, or leaking context. The agent cannot take external actions directly. All output goes to Google Cloud Storage (GCS), where the Elastic Workflows orchestrator reads it and posts results as an internal comment on the HackerOne report. The agent never has access to high-value credentials (the HackerOne API token stays in the orchestration layer). Even a fully compromised analysis cannot post public messages to HackerOne or create GitHub issues without going through the orchestrator. The adversarial review provides a second-pass check that can catch manipulated analysis output.


Data poisoning resistance Reports crafted to shift triage patterns over time. Agent skills are baked into the VM image at build time, not downloaded at runtime. There’s no persistent storage between runs and no feedback loop from report outcomes back to the triage rules (yet). Each triage run starts from a clean, known-good state.


Network isolation Sandbox escape, lateral movement. The tester container runs on a Docker internal network with no internet access by default. Product containers (Elasticsearch, Kibana) are on the same internal network so the tester can reach them, but neither the tester nor the products can reach the internet. When a topology requires package downloads (Fleet, Elastic Agent), only the specific product container gets a bridge network with internet access through the Squid proxy. The tester never joins the external network.


Egress filtering Credential exfiltration, supply chain injection. All VMs sit in a dedicated GCP VPC (audit-network) with no public IP addresses. All outbound HTTP/HTTPS traffic routes through a Squid proxy at` 10.0.0.10:3128` . The proxy acts as the single egress point, providing visibility into what the reproduction environment tries to reach.


Resource limits Resource abuse, fork bombs, crypto mining. The tester container is constrained to 512MB RAM and 1 CPU core with a 5-minute execution timeout. These limits prevent resource abuse and bound the impact of a fork bomb or memory exhaustion attack.


Credential isolation Credential exfiltration across phases. The HackerOne API token and Elastic Cloud API key are stored in[Google Cloud Secret Manager](https://cloud.google.com/security/products/secret-manager) and injected as environment variables only for the phase that needs them. The analysis VM gets the Anthropic API key. The reproduction VM gets the Elastic Cloud key (only if provisioning a cloud environment). No credentials are written to disk, and no VM has access to credentials it doesn't need.


Ephemeral infrastructure Persistent compromise, long-lived access. Every VM auto-terminates after 30 minutes. The Packer-built base image is immutable. Agent code is baked into the image at /opt/vuln-triage-agent, not pulled from a repository at boot. There’s no SSH access, no persistent storage, and no way to extend the VM's lifetime from inside it.


Read-only workspace Filesystem tampering, workspace modification. The tester container mounts the reproduction workspace as read-only. The script can read the test configuration and write to stdout/stderr (which gets captured for analysis), but it cannot modify the workspace or the host filesystem.


Non-root execution Privilege escalation after container escape. The triage agent runs as a dedicated` triage` user, not root. Docker group membership allows container management, but a container escape lands as an unprivileged user on an ephemeral VM in an isolated VPC.


###


Six Docker Compose topologies for Elastic Stack reproduction


Different vulnerability types target different product combinations. An Elasticsearch API bug doesn't need Kibana. A Fleet enrollment issue needs Fleet Server and Elastic Agent. We built six Jinja2-rendered Docker Compose templates covering the common topologies: standalone Elasticsearch, Elasticsearch with Kibana, Fleet with Agent, Logstash, Beats, and APM Server. The analysis pipeline's reproduction feasibility stage selects the appropriate topology based on the affected product.


##


Orchestrating AI triage with Elastic Workflows


Elastic Workflows manages the pipeline from report ingestion to human decision. The flow works like this:


1. **HackerOne reports land in Elasticsearch** via the[HackerOne Elastic Agent integration](https://www.elastic.co/docs/reference/integrations/hackerone) . Reports are just documents in an index.
2. **An Elasticsearch alert rule triggers the workflow** when a new report appears. The workflow checks VM slot availability (Google Compute Engine \[GCE\] list filter for running triage VMs). If at capacity, the run fails fast rather than queuing, and the alert refires on the next evaluation interval.
3. **Create an analysis VM** via GCP Compute Engine API. The VM boots from a Packer image, pulls the report from the Elasticsearch index, runs the analysis and adversarial review, uploads results to GCS, and shuts down.
4. **Parse analysis results.** If the recommendation is` reject` or` needs_info` , skip reproduction. If it’s` reproduce` , proceed to step 5. If the adversarial review returns a` disagree` verdict, halt and route directly to human review.
5. **Create a reproduction VM.** Provisions the Elastic Stack, runs the tester, interprets results, uploads to GCS, and then shuts down.
6. **Post an internal comment to HackerOne** with the full analysis summary, CVSS justification, exploitability reasoning, adversarial review results, and reproduction outcome. This internal comment is the primary triage record. A human triager can validate, act on, or override the assessment entirely from this comment.
7. **Human reviews and decides.** From the HackerOne report, the triager can create a GitHub issue, respond to the researcher, or reprocess the report if the first attempt hit a transient issue.


The human-in-the-loop design is intentional. The agent does the analysis and reproduction. A human makes the final call on every external action: communicating with the researcher, creating an internal tracking issue, or escalating to the product team. We aren’t comfortable with an AI system autonomously closing or triaging bug bounty reports, and neither should you be. The value is in redirecting senior security engineer attention from mechanical first-pass triage to the reports that genuinely need expert judgment.


The cost per report breaks down across the two phases:


Phase Cost per report VM spec Runs on Primary cost driver


Analysis $0.50 to $1.15 e2-standard-2 (2 vCPU, 8GB) Every report Claude API usage


Reproduction $0.80 to $4.90 e2-standard-4 (4 vCPU, 16GB) ~30% of reports Claude API usage for interpreting results


At 85% agreement with human decisions, the agent handles the bulk of the volume while the remaining 15% (the ambiguous cases, novel attack patterns, and reports requiring product-depth judgment) go straight to the people best equipped to assess them.


##


Lessons from building AI-powered bug bounty triage


###


Report framing is the hardest bias to counter


The single biggest source of incorrect triage, both human and AI, is unconsciously anchoring on the report's framing rather than forming an independent assessment. A report that describes a finding as critical primes the analyst to evaluate it in that context, even when an independent analysis of the prerequisites might reach a different conclusion. We added explicit framing detection in both the analysis and the adversarial review after finding that early versions of the system adopted report language without independent verification in over 30% of analyses.


###


Calibration data matters more than prompt engineering


The triage rules with historical acceptance rates per vulnerability category improved accuracy more than any other change. Without them, the system treated each report category equally. With them, it applies appropriate skepticism proportional to how often each category is historically accepted. But the calibration work also revealed its own trap: Rules derived from a 300-report sample looked effective until the full-corpus evaluation exposed category-level failures hidden by underrepresentation. The iterative cycle of evaluate, analyze failures, add rules, add few-shot examples, and reevaluate against the full corpus is what ultimately drove accuracy gains. No amount of prompt engineering substituted for that loop.


###


Reproduction is expensive but decisive


About 40% of reports that pass analysis end up being reproduced. Of those, reproduction confirms the finding roughly half the time. The other half either fail to reproduce (often because the researcher tested on an old, unpatched version) or reveal that the "vulnerability" is actually expected behavior under the researcher's specific configuration. Reproduction costs more (~$0.80 to $4.90 per attempt, mostly Claude API usage for interpreting results), but it converts ambiguous analyst judgment into a concrete yes-or-no answer.


###


Security of the triage system is itself a security problem


When you build a system that deliberately processes adversarial input (vulnerability reports are, by definition, descriptions of how to break things), the system itself becomes a target. Prompt injection via report text is the most likely attack vector, and it requires zero special access. We spent as much time on the threat model for the triage infrastructure as we did on the triage logic. The core architectural decision, that the agent cannot take any external action directly and all output flows through human approval, is the single most important mitigation. The layered approach on top of that (ephemeral VMs, network isolation, egress filtering, credential separation, resource limits, and read-only mounts) is defense in depth for the cases where containment alone is not enough.


##


What's next for AI vulnerability triage at Elastic


The system is in production and for several weeks has been processing HackerOne reports as they arrive. We’re collecting data on production accuracy and will publish detailed metrics once the sample size is large enough to be meaningful. The 85% agreement rate from retrospective validation is encouraging, but production performance against novel reports is the number that matters.


What we want to build next:


- Feedback loops from human triage decisions back into the calibration data, so the system learns from its mistakes over time.
- Support for more complex multistep reproduction scenarios that require browser interaction. (We have[Playwright](https://playwright.dev/) -based tester containers for this, but the orchestration isn’t yet automated.)
- Better handling of reports that reference multiple vulnerabilities in a single submission.


If you’re running a bug bounty program and drowning in triage volume, the approach generalizes beyond Elastic-specific rules. The core pattern (structured multistage analysis, adversarial review, sandboxed reproduction, and human-in-the-loop decisions) works with any product-specific triage knowledge you can encode. The hard parts are the institutional knowledge about what constitutes a real finding in your specific product, and the security controls around running untrusted reproduction steps.


#### Jump to section


- [The problem: Triage doesn't scale linearly](https://www.elastic.co/security-labs/ai-vulnerability-triage-bug-bounty-hackerone#the-problem-triage-doesnt-scale-linearly)
- [AI triage architecture: Two VMs, two skills, one orchestrator](https://www.elastic.co/security-labs/ai-vulnerability-triage-bug-bounty-hackerone#ai-triage-architecture-two-vms-two-skills-one-orchestrator)
- [Why not a single VM?](https://www.elastic.co/security-labs/ai-vulnerability-triage-bug-bounty-hackerone#why-not-a-single-vm)
- [Why ephemeral VMs instead of containers on a long-running host?](https://www.elastic.co/security-labs/ai-vulnerability-triage-bug-bounty-hackerone#why-ephemeral-vms-instead-of-containers-on-a-long-running-host)
- [Vulnerability analysis pipeline: Eight stages with built-in skepticism](https://www.elastic.co/security-labs/ai-vulnerability-triage-bug-bounty-hackerone#vulnerability-analysis-pipeline-eight-stages-with-built-in-skepticism)
- [Eight stages of AI-powered vulnerability triage](https://www.elastic.co/security-labs/ai-vulnerability-triage-bug-bounty-hackerone#eight-stages-of-ai-powered-vulnerability-triage)
- [Elastic-specific triage rules](https://www.elastic.co/security-labs/ai-vulnerability-triage-bug-bounty-hackerone#elastic-specific-triage-rules)
- [How the AI assesses realistic exploitability](https://www.elastic.co/security-labs/ai-vulnerability-triage-bug-bounty-hackerone#how-the-ai-assesses-realistic-exploitability)
- [The adversarial review: Challenging your own AI](https://www.elastic.co/security-labs/ai-vulnerability-triage-bug-bounty-hackerone#the-adversarial-review-challenging-your-own-ai)
- [Calibrating against 3,317 historical reports](https://www.elastic.co/security-labs/ai-vulnerability-triage-bug-bounty-hackerone#calibrating-against-3317-historical-reports)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=Agents%20vs.%20agents:%20how%20we%20triage%20HackerOne%20reports%20for%20$2%20each,%2085%%20as%20well%20as%20a%20human&url=https://www.elastic.co/security-labs/ai-vulnerability-triage-bug-bounty-hackerone)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/ai-vulnerability-triage-bug-bounty-hackerone)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/ai-vulnerability-triage-bug-bounty-hackerone&title=Agents%20vs.%20agents:%20how%20we%20triage%20HackerOne%20reports%20for%20$2%20each,%2085%%20as%20well%20as%20a%20human)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/ai-vulnerability-triage-bug-bounty-hackerone&title=Agents%20vs.%20agents:%20how%20we%20triage%20HackerOne%20reports%20for%20$2%20each,%2085%%20as%20well%20as%20a%20human)
