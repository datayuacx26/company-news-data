---
schema_version: "1.0.0"
document_id: "8bf40f0b38ac067a92dbf0adafa97fe59d3723b22903911f64e5446723c3a36d"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/sast-triage-agentic-evaluation-bits-memories/"
published_at: "2026-07-06T00:00:00+00:00"
first_seen_at: "2026-07-25T01:09:56.516023+00:00"
fetched_at: "2026-07-28T20:47:27.048275+00:00"
content_hash: "sha256:e330be8156fbb6f353a389d020de6efca985bd0bea0b85eba979e61bf5d7157a"
---

# Reduce SAST false positives with agentic evaluation and Bits Memories

Cole Maring


Static application security testing (SAST) tools are intentionally conservative. Traditional scanners identify code that appears exploitable and flag the snippet for review, even when protections elsewhere in the application prevent exploitation. Although that approach helps teams catch vulnerabilities, it also creates false positives that consume developer time, slow remediation efforts, and make future alerts easier to dismiss.


As part of[Datadog Static Code Analysis](https://docs.datadoghq.com/security/code_security/static_analysis/) in[Datadog Code Security](https://docs.datadoghq.com/security/code_security/) , Bits AI already helps teams prioritize findings by[assessing whether a finding is likely to be a true positive or a false positive](https://www.datadoghq.com/blog/using-llms-to-filter-out-false-positives/) and providing a short explanation. Many findings, however, can’t be evaluated from the flagged file alone. A function might appear vulnerable until you discover that every caller validates its inputs or that an authorization check runs elsewhere in the request path. Other findings depend on team-specific conventions that aren’t visible in the code at all.


To help teams distinguish real vulnerabilities from false positives when evidence exists outside the flagged file, Datadog Static Code Analysis includes[agentic evaluation](https://docs.datadoghq.com/security/code_security/static_analysis/ai_enhanced_sast/#agentic-false-positive-filtering) and[Bits Memories](https://docs.datadoghq.com/security/code_security/static_analysis/ai_enhanced_sast/#bits-memories) . Agentic evaluation brings repository-wide analysis to findings, and Bits Memories incorporates your organization’s knowledge into false positive assessments. In this post, we’ll explore how these capabilities help you:


-


Automatically triage findings with full repository context


-


Capture your team’s security conventions


-


Combine repository-wide evidence with organizational knowledge


## Automatically triage SAST findings with full repository context by using agentic evaluation


Agentic evaluation enables Bits AI to investigate a SAST finding before assessing it, helping improve triage accuracy. As a scan runs, Bits receives each eligible finding, the flagged line, and the source file, and then explores the repository to identify related code paths, callers, and validators. The resulting assessment is enriched by this context from across the repository.


For example, consider a[server-side request forgery (SSRF)](https://www.datadoghq.com/blog/detect-ssrf-attacks/) finding generated during a scan. The following code shows the function that triggered the finding:


```text
1   func     FetchURL  (  ctx     context  .  Context  ,   target     string  ) (*  http  .  Response  ,   error  ) {    2         req  ,   err   :=   http  .  NewRequestWithContext  (  ctx  ,   http  .  MethodGet  ,   target  ,   nil  )    3         if     err   !=   nil   {    4             return     nil  ,   err    5          }    6
7         return     http  .  DefaultClient  .  Do  (  req  )    8   }
```


Viewed in isolation, the function appears risky because it accepts a user-supplied URL and passes it directly to an HTTP client. However, the flagged function doesn’t tell the whole story. To understand whether the finding represents a genuine vulnerability, you need to know how the function is used in the application. The following code shows one of the function’s callers:


```text
1   func     HandlePreview  (  w     http  .  ResponseWriter  ,   r   *  http  .  Request  ) {    2         target   :=   r  .  URL  .  Query  ().  Get  (  "url"  )    3         if   !  allowlist  .  Valid  (  target  ) {    4             http  .  Error  (  w  ,   "blocked"  ,   http  .  StatusBadRequest  )    5             return    6          }    7
8         FetchURL  (  r  .  Context  (),   target  )    9   }
```


The caller in this example validates the URL against an allowlist before invoking the function. Bits follows those relationships automatically with agentic evaluation, inspecting callers and related code paths before assessing the finding. If every reachable caller validates the input, the evidence points toward a false positive. If a caller passes user input directly to the function, the evidence indicates a genuine vulnerability.


Repository context also improves the explanations that accompany findings. Instead of providing only generic rationale, Bits references the specific validators, wrappers, or callers that informed its assessment. Security teams can review the reasoning and understand why a finding was classified as a likely true positive or false positive.


Agentic evaluation operates within controlled boundaries. Bits receives read-only access to repository contents and can inspect code, search files, and gather evidence. It cannot modify files, execute code, or access resources outside the repository. Bits treats what it reads as untrusted evidence, not instructions. All evaluations also pass through[Datadog AI Guard](https://docs.datadoghq.com/security/ai_guard/) , which provides defense against prompt injection attempts.


## Capture your team’s security conventions with Bits Memories


Exploring a repository helps when the missing context lives in code, but some triage decisions depend on organizational knowledge that isn’t visible in the repository. One team might treat an outbound URL as safe after it passes through a central allowlist, while another team might require validation closer to the call site. General-purpose models don’t know those conventions, which makes it difficult to assess findings accurately.


Bits Memories brings organizational context for more accurate assessments. Memories are scoped per rule in your organization and apply across all of your repositories. A memory for one rule has no effect on how Bits evaluates other rules. Within that scope, Bits draws on two sources of information. The first source is your organization’s history of false positive reports for that specific rule. When you repeatedly identify a recurring pattern as a false positive, Bits uses that history as additional context during future evaluations.


The second source of information for Bits Memories is custom context that your team provides in free-text notes. You can document framework-specific behavior, internal validation libraries, authorization patterns, review guidance, and other information that isn’t visible in the repository. A custom note might explain that all requests pass through a shared authorization layer, or that a proprietary validation function implements a company-wide security standard.


The memories that you create through false positive reports and custom context serve as evidence, not a suppression list. A past report or custom note does not silence every future finding for that rule. Bits evaluates each finding and determines whether the prior context applies. If the pattern matches previous assessments, the memory contributes additional evidence. If the pattern differs from past examples, Bits relies on current evidence instead of the memory.


Scans also reflect the latest organizational knowledge. When teams submit false positive reports or update custom context, subsequent evaluations incorporate that new data rather than relying on outdated verdicts or information.


## Combine repository-wide evidence with your team’s knowledge


Agentic evaluation and Bits Memories are designed to complement each other. When used in tandem, they give Bits a more complete picture of a finding to help it reach an accurate verdict.


For example, consider the SSRF finding that we highlighted previously. Agentic evaluation can determine that every caller validates the URL before invoking the flagged function. Bits Memories can add historical context showing that your security team has repeatedly classified findings as false positives when that same validation pattern is present. The combination of evidence provides a more conclusive assessment than either source alone.


Human reviewers don’t assess findings in a vacuum. They trace data paths, consult framework conventions, and draw on institutional memory. By combining repository-wide evidence with organizational knowledge, Bits can bring that same depth of reasoning to findings.


## Get started with agentic evaluation and Bits Memories


Agentic evaluation and Bits Memories make SAST triage more accurate by extending Bits’s view beyond a single file to the full repository and your team’s accumulated knowledge. Together, these capabilities help reduce false positives, provide more trustworthy explanations, and direct developer attention toward findings that deserve investigation. To learn more, read the[documentation about AI enhancements in Static Code Analysis](https://docs.datadoghq.com/security/code_security/static_analysis/ai_enhanced_sast/) and the[blog post about our open source AI-native SAST](https://www.datadoghq.com/blog/open-source-ai-sast/) .


If you’re new to Datadog, you cansign up for a 14-day free trial to get started with Datadog Code Security and Static Code Analysis.


-
-
-
