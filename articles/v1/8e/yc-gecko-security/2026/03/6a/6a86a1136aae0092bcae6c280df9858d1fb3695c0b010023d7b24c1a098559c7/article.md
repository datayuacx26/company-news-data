---
schema_version: "1.0.0"
document_id: "6a86a1136aae0092bcae6c280df9858d1fb3695c0b010023d7b24c1a098559c7"
company_key: "yc-gecko-security"
company: "Gecko Security"
source_id: "yc-gecko-security-news-import-02715d60c1a6"
canonical_url: "https://www.gecko.security/blog/business-logic-appsecs-unsolved-problem"
published_at: "2026-03-02T00:00:00+00:00"
first_seen_at: "2026-07-21T21:22:16.875303+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:b0c04fa1309970b2c32084229e70112850d29fc40aa1dc51c05fe4786c7ee953"
---

# Business Logic Is AppSec's Unsolved Problem

I recently gave a[talk](https://www.youtube.com/watch?v=CwSXtcRLC_w&list=PLcrUMxzVpi6xbQi1AT-W_e1TsQ1liaQng&index=4) at BSides London on using LLMs to find business logic vulnerabilities in source code. It was one of the most attended talks at the event, which I think reflects just how much interest there is in this space right now.


This post is a written companion to that talk, covering roughly 8 months of research and development between myself and the team at Gecko Security.


## How Vulnerability Trends Have Shifted


There have been whole classes of vulnerabilities that we used to say required humans to detect. To understand why that's the case and how LLMs can help, it's worth looking at how vulnerability trends have evolved over the past decade.


When OWASP released their top 10 list in 2017, injection attacks sat in 1st place. At the time, injection vulnerabilities were everywhere, especially in legacy code, and pretty much every pentest ended with some form of a dumped database. By 2021, injection attacks had dropped to 3rd place, and in the 2025 release at the end of last year, they fell to 5th.


That's good security. We actually fixed a class of vulnerability at scale. The way we got there was through a combination of shifts that made it harder to write vulnerable code in the first place. Modern frameworks evolved to become secure by default. Parameterized queries and prepared statements became the standard. Newer frameworks adopted ORM libraries with built in protections and developers were somewhat educated on how injection attacks worked. The industry collectively moved past injection attacks to the point where they became a meme.


But if you track a different category across the same timeline like broken access control, you see the opposite trend. It moved from 5th in 2017, to 1st in 2021, and has remained there through to the 2025 list released last month, with no sign of the kind of systemic mitigation that worked for injection attacks.


### What Broken Access Control Is


OWASP defines broken access control as 'failures where users can act outside of their intended permissions, typically leading to unauthorized information disclosure, modification, or destruction of data'. In practice, this means a user being able to access server data they shouldn't see, access other users' data (breaking user or tenant boundaries), or elevate their privileges from something like a read only user to an admin.


The mapped CWEs under this category include path traversals, improper authorization, improper authentication, insecure direct object references (IDORs), and around 30 others. Some examples we've found at Gecko include[broken access controls](https://www.gecko.security/blog/caldotcom-broken-access-controls) in Cal.com and[authorization bypass](https://www.gecko.security/blog/cve-2026-21894) in N8N, which are fairly representative of the kinds of subtle logic issues that slip through automated tooling.


When OWASP announced their 2025 release candidate, they justified broken access control at number one by stating that


> *"100% of applications tested were found to have some form of broken access control"*
>
>
> — OWASP 2025


Their previous surveys tested around 500,000 applications, and I'd expect the same or larger sample size this time, meaning that every single application in the dataset had some variant of this issue. The scale of the problem also prompted OWASP to create a separate dedicated[top 10 list](https://owasp.org/www-project-top-10-for-business-logic-abuse/) just for business logic issues at the start of last year, which goes deep into categorizing and building a framework around these vulnerabilities.


### Real World Impact


That data is useful for understanding the scale, but it's useful to look at real examples of broken access control being found and exploited in the wild to understand the actual impact on businesses and users. In July 2025, GitLab disclosed three CVEs ([CVE-2025-4972](https://nvd.nist.gov/vuln/detail/CVE-2025-4972) ,[CVE-2025-6168](https://nvd.nist.gov/vuln/detail/CVE-2025-6168) ,[CVE-2025-3396](https://nvd.nist.gov/vuln/detail/CVE-2025-3396) ) that allowed users to bypass group level restrictions on invitations and forking. These were relatively straightforward vulnerabilities with low severity scores. But because GitLab hosts a huge number of open source repositories and focuses on privacy and security features, the impact was more noticeable than the severity scores might suggest. If you look at GitLab's stock price and map out when these CVEs were published on July 9th, there was a 10% decline on that day.


It's hard to say whether there's direct causation, but it's worth noting as a potential indicator of the business impact these vulnerabilities carry.


A more dramatic example is the[Optus breach](https://www.csoonline.com/article/573731/a-third-of-australian-population-likely-affected-in-optus-cyberattack.html) from 2022. Optus, the second largest telecoms company in Australia, suffered a breach that exposed 10 million user records, roughly a third of the country's population. The breach came down to two failures of broken access control working together. First, an attacker found an exposed endpoint for querying user data that had no access control around it, no session validation, no middleware checks, anyone could call it. Second, instead of using high entropy unique identifiers for user IDs, the system used sequential numeric IDs. Combining these two issues allowed an attacker to write a simple Python script to enumerate all user IDs and pull every customer record.


### Why This Will Get Worse


A big part of OWASP highlighting these issues and creating a dedicated business logic top 10 is that they see these problems getting worse over the next few years, and I think there are a few reasons to agree with that.


The most talked about reason is vibe coding and AI generated code. There are now several studies showing that AI generated code introduces more bugs, and while most of the discussion focuses on that headline finding, some of the research goes deeper in interesting ways. A recent[paper](https://arxiv.org/html/2512.03262v1) from CMU researchers, "Is Vibe Coding Safe?", built a benchmark to test LLM models on secure code generation and found that while 60% of features generated by AI work correctly, only 10% meet security standards.


Dan Boneh, a professor at Stanford,[ran experiments](https://ee.stanford.edu/dan-boneh-and-team-find-relying-ai-more-likely-make-your-code-buggier) that revealed something arguably more interesting. He gave two groups of engineers the same programming task, one group with access to AI coding tools and one without, and then asked both groups whether they thought their application was securely developed. The group without AI expressed uncertainty and said they weren't sure. The group with AI was confident their code was secure. In reality, the group without AI actually performed marginally better. The takeaway from Boneh's research is that engineers using AI write less secure code but trust the output more, and that gap between confidence and reality is where vulnerabilities slip through.


Modern architecture is compounding this. The growing shift towards cloud, containers, and microservice based applications increases the number of trust boundaries, exposed endpoints, and surfaces where logic flaws can appear. Looking ahead, there are going to be significant challenges with agentic identity around authentication and authorization, especially as it gets tied to MCP and similar protocols.


On the attacker side, there's a growing body of evidence that offensive capabilities are being automated too. In late 2025, Anthropic released a[report](https://www.anthropic.com/news/disrupting-AI-espionage) about detecting what they described as the first AI orchestrated cyber espionage campaign, and[several other](https://factory.ai/news/droid-neutralizing-fraud) AI companies announced similar findings shortly after. There were mixed opinions from the community on the reality of these specific claims, but the more important conversation isn't about whether this is happening right now. It's about what happens when it becomes routine. If attackers can continuously run campaign level scanning against everything with a public IP address, being vulnerable and being hacked stop being two separate steps, and the urgency to find these issues proactively increases significantly.


## Why Business Logic Bugs Never Got Solved


We know business logic vulnerabilities are everywhere, we've seen the impact from real world examples, and we know the problem is going to get worse. So the natural question is why don't we have a way to systematically find and fix these issues the way we did with injection attacks?


The answer comes down to a fundamental difference in what these vulnerabilities are. Injection attacks are about breaking syntax, they exploit how code is structured, and you can fix them by enforcing correct syntax through parameterization and prepared statements. Business logic bugs are about breaking intent, they exploit the gap between what a system is supposed to do and what it actually does, and that gap is unique to every application. You can't parameterize away a missing authorization check because you first need to know what the correct authorization policy should be.


The distinction that matters here is between syntactic and semantic vulnerabilities. Most common web application vulnerabilities are syntactic issues, problems with the structure of the code that can be caught by pattern matching. But business logic vulnerabilities are semantic. They emerge from deep interactions across systems, state transitions, and multi layer logic. You need to understand the system's meaning, not just its syntax.


### Why Automated Tools Fall Short


This is exactly why legacy SAST, DAST, and runtime tools have fallen short. Static analysis tools scan source code and pattern match against rule bases. What makes them powerful for other vulnerability types makes them fundamentally incomplete for business logic issues, because logical flaws require understanding long sequences of dependent operations that don't fit well known patterns. DAST tools work on running applications by sending inputs and observing responses, but they're exploration techniques that struggle with deep code understanding. Runtime tools monitor live traffic to block malicious payloads, but business logic bugs don't use payloads or patterns, they abuse intended functionality.


### Where That Leaves Us


This is where human researchers come in, and why manual testing has been the gold standard for finding business logic vulnerabilities. What makes a good pentester or code auditor effective is their ability to understand the context of an application, think creatively about how it could be abused, and chain multiple smaller issues into bigger exploits. They read API documentation, understand the business model, and think like both a user and an attacker. This is fundamentally different from how any of the automated approaches work, which is why pentests exist, why bug bounties are a valuable part of application security programs, and why we're struggling to keep up with the rate at which these issues are being committed to code.


This involves reasoning, which is exactly what LLMs are built for. The question that drove our research is whether we can get the scaling benefits of automated tools while preserving the bug finding methodology and understanding that makes human researchers effective.


This is the problem we set out to solve at Gecko Security. We built a system that uses LLMs to follow the same workflow a human researcher would, learning the application's logic, threat modeling against it, and tracing call chains from source to sink. Rather than pattern matching, the LLM reasons about what the code is supposed to do and whether that intent is actually enforced.


If you're dealing with business logic vulnerabilities that your current tools miss,[book a call](https://cal.com/geckosec/30min) with our founders and see what it finds.
