---
schema_version: "1.0.0"
document_id: "8e32fb2ca37e72a21f6dfc66d75734407b3d59cbe920e14d9780b7f837fc70d6"
company_key: "gitlab-inc-class-a-common-stock"
company: "GitLab Inc."
source_id: "gitlab-inc-class-a-common-stock-atom-8616b2ef668b"
canonical_url: "https://about.gitlab.com/blog/gitlab-duo-security-review-flow/"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-20T03:30:07.155656+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:910b90b526d21442d5a8b41ee8c86a88e2647501c4a5b50904c404721d9addf4"
---

# GitLab Duo Security Review spots logic flaws scanners miss

Static scanners excel at catching vulnerabilities that fit a known pattern, like unsanitized query inputs, hardcoded secrets, and unsafe deserialization. They struggle against flaws in your application’s logic, where there is no pattern to match — only valid code doing the wrong thing for your domain. Undetected, these flaws surface late and cost more to fix.


Security Review Flow, now in public beta, scrutinizes code changes the way a security engineer would. It traces intent rather than matching signatures to catch logic flaws before they hit production. It's a major step toward uncovering dangerous flaws that scanners usually miss.


## Where pattern-based scanners go blind


The most damaging application vulnerabilities often look correct line by line, but violate context the code doesn't contain, like your authorization model, data sensitivity rules, and intended workflows. Consider three of the most common vulnerability classes:


**Access and authorization:** Whether a user may read or change a resource is defined by your authorization model, not any language construct. Broken object level authorization (accessing another user's data by changing an ID) has topped the[OWASP API Security Top 10](https://owasp.org/API-Security/editions/2023/en/0x11-t10/) since 2019.


**Data exposure:** Serializing an object and returning it is ordinary, correct-looking code. Whether it leaks depends on which fields are sensitive and who receives them — facts about your domain, not your syntax.


**Control flow and workflow:** Business-logic and race-condition flaws occur when valid operations run in the wrong order, repeat unexpectedly, or get manipulated. Examples include checkout reachable without payment, a state re-entered under a race, or a parameter tweaked to change a price.


Catching these flaws has previously required manual security review, which is expensive to scale across every merge request (MR), or penetration testing and bug bounties, which arrive too late. The result is a growing gap between the pace of development and how quickly security expertise can be applied.


## Bring security judgment to every MR


Security Review Flow, a foundational flow on GitLab Duo Agent Platform, closes that gap by reasoning about what your code is meant to do. It detects exactly the classes of flaws described above: Broken object level authorization and function level authorization, missing authorization on state-changing operations, information disclosure, mass assignment, business logic errors, and race conditions in stateful workflows.


It complements traditional scanners and human analysis rather than replacing them, and it reviews code at the point of change, when a fix is cheapest. GitLab's own application security team has used Security Review Flow across internal MRs throughout its development.


**See Security Review Flow in action:**


## How it works


When your MR is ready, request a review from` Duo Security Review` , the same way you would from a person. It analyzes the diff in context: the original files, changed lines, MR discussion, and related code. Its reasoning is optimized for precision, and an independent validation pass examines each finding to filter out likely false positives.


Findings appear as diff threads on the relevant lines, along with a summary in an internal note. On public projects, they’re confined to the internal note, so security details aren’t exposed.


Each finding arrives with the context reviewers need:


- **Vulnerability type** , with a CWE reference
- **Severity** : critical, high, medium, or low
- **Tier** : Tier 1 (Exploitable), Tier 2 (Logic Flaw), or Tier 3 (Design Issue)
- **A plain-language explanation** of the issue
- **A suggested fix** , when one is available


Severity determines the reviewer state: A critical or high finding sets it to *Request changes* , while medium or low findings result in *Comment* . The flow never approves, even when it finds nothing — a human always owns the final call.


From there, mention your organization’s Duo Security Review service account in a comment thread to ask a question, discuss remediation, or challenge a finding. Resolve each finding by applying the fix as a standard MR suggestion, dismissing it as a false positive, or accepting the risk. After committing your fixes, request a fresh review to check what changed.


## Run your first Security Review Flow


Security Review Flow is in public beta for GitLab Ultimate customers. It is available on GitLab.com, GitLab Self-Managed, and GitLab Dedicated.


Learn how to get started in the[Security Review Flow documentation](https://docs.gitlab.com/ee/user/duo_agent_platform/flows/foundational_flows/security_review/) .


You can get access to Security Review Flow with a[free trial of GitLab Duo Agent Platform](https://gitlab.com/-/trials/new?glm_content=default-saas-trial&glm_source=about.gitlab.com/gitlab-duo-agent-platform/) . Already a GitLab Ultimate subscriber?[Turn on Duo Agent Platform](https://docs.gitlab.com/user/duo_agent_platform/turn_on_off/) and use the[GitLab Credits included with your subscription](https://docs.gitlab.com/subscriptions/gitlab_credits/#included-credits) .


Cost varies with the complexity of the diff and the model you select, so try it on a few MRs before running it broadly. Pricing may be updated at general availability.


Share what you find in our[feature feedback issue](https://gitlab.com/gitlab-org/gitlab/-/work_items/600304) , so your input shapes what we build.


##


Are you trading speed for security?


[Get your security maturity score](https://about.gitlab.com/assessments/security-modernization-assessment/)


Quiz will take 5 minutes or less
