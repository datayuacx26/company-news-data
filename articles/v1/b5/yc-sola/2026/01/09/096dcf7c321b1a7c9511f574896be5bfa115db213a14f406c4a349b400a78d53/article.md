---
schema_version: "1.0.0"
document_id: "096dcf7c321b1a7c9511f574896be5bfa115db213a14f406c4a349b400a78d53"
company_key: "yc-sola"
company: "Sola"
source_id: "yc-sola-news-import-2dc63e1087a6"
canonical_url: "https://sola.security/insights/cybersecurity-ai-report/"
published_at: "2026-01-21T09:40:04+00:00"
first_seen_at: "2026-07-24T01:34:47.750416+00:00"
fetched_at: "2026-07-28T21:26:59.511520+00:00"
content_hash: "sha256:8eb3c1330ae2abdec6a000a1d0bd9f4a60d5dda8528f050d6ec89c09c4b4510a"
---

# AI for Cybersecurity: What Practitioners Asked in 2025

While other industries have raced toward AI integration, cybersecurity teams remain the “final holdouts” of the revolution. Research from ISC2\[1 \] highlights this measured pace, finding that by mid-2025, only one in three cybersecurity professionals had actually integrated AI into their daily operations.


As AI platforms purpose-built for security mature, this adoption is expected to accelerate. We have the unique opportunity to explore how security practitioners interact with such a platform, and what they are most eager to solve when faced with the broad question, “what can we help you secure today?”


This report provides a snapshot of the patterns, topics and behavior observed across 2,052 users in the Sola Security platform between May and December 2025. The data is anonymized and aggregated, but the patterns are clear: where attention is focused, how questions evolve, and what it means for how security teams will work in 2026.


### Key takeaways


As we enter 2026, data from 2025 reveals that AI in cybersecurity has established itself primarily as a **comprehension engine** . While industry narratives often focus on autonomous response, practitioners are using AI to solve a “clarity bottleneck”.


- Nearly 60% of all AI prompts focus on understanding and investigating data rather than taking automated action.
- Four domains dominate 67% of focus: Application Security, Cloud and Infrastructure Security, Security Operations, and Identity and Access Management.
- Application Security is the primary area of friction, commanding 25.8% of all practitioner attention.
- Security domain focus shifts as teams grow: Small organizations focus on cloud configurations, midsized organizations focus on application vulnerabilities, and larger organizations focus on identity and access.
- Users demonstrated growing maturity in AI usage, with intent shifting from initial discovery to continuous monitoring, which grew by 8.8 percentage points in the latter part of 2025.


## Security teams search for clarity within their data


**AI in cybersecurity has established itself primarily as a comprehension engine.** While industry narratives often focus on autonomous response, practitioners are using AI to solve a “clarity bottleneck”.


Sixty percent of all prompts in the dataset fall into two categories: *Discover* (36.7%) and *Investigate/Explain* (18.8%). Most security teams aren’t asking AI to remediate, automate, or take action, at least not yet.[They’re searching for data clarity](https://sola.security/blog/agentic-workflows-security-operations/) .


The median prompt length is 11 words, often focused on immediate clarity: “Show me all AWS S3 buckets with public access,” or “explain this vulnerability class at a high level and suggest typical mitigations.”


This indicates that security practitioners know where to look, but want clarity about what they’re seeing and how to tackle it.


Security teams aren’t short on data, quite the opposite; reports and studies show that they are overloaded with tools, alerts and findings. A possible conclusion is that **the problem isn’t the volume of data, it is the lack of context** .


Security tools are able to answer the question “ *what happened?* “, but they can rarely answer “ *why does this matter?* “, “ *what should I prioritize?* ” or, “ *is this actually exploitable?* “. That is one of the possible reasons why practitioners turn to AI and ask those questions instead. Discovery and explanation dominate because clarity and context are still the bottleneck.


When digging into the prompts themselves, the desire for context becomes obvious. The questions being asked aren’t theoretical, they’re specific to the users’ stack, vulnerabilities and access sprawl.


## Four domains account for 67% of security questions


Four domains dominate the attention economy: Application Security commands 25.8% of all prompts, followed by Cloud and Infrastructure Security, Security Operations, and Identity and Access Management. Together, these four account for more than two-thirds of where practitioners focus their questions.


**Within Application Security, the top pain point is risk assessment** (9.9% of prompts), with “find security issues” as the most repeated phrase. Other top pain points are suspicious activity detection and excessive privilege concerns. Practitioners consistently mention GitHub, OWASP frameworks, and API security, revealing a focus on code repositories and web application vulnerabilities. The pattern shows teams want immediate understanding of their AppSec status.


**Cloud and Infrastructure questions cluster around exposure and misconfiguration.** Data exposure dominates at 18.0% of cloud prompts, with practitioners repeatedly asking about “publicly accessible” resources and misconfigurations. Multi-cloud correlation emerges as a clear pattern, with users consistently asking about cloud and GitHub, indicating integrated DevSecOps security concerns. These patterns can be explained by teams’ needs to understand blast radius across fragmented consoles.


**Security Operations is the third most popular security domain to ask about** . Suspicious activity detection accounts for 16.0% of SecOps prompts, with sign-ins and events as dominant keywords. Unsurprisingly, nearly one-third of all SecOps prompts (30.1%) ask AI for creating dashboards, alerts, or workflows, the highest creation rate of any domain. The operational focus reveals teams building simplified SOC capabilities, rather than querying for one-off answers.


**Identity and access management** **centers overwhelmingly on MFA enforcement and cross-platform privilege sprawl** . Authentication concerns appear in 39.3% of IAM prompts, with “without MFA” as the most common phrase. Excessive permissions and access issues account for 8.1% of prompts, with practitioners asking about admin roles across GitHub, AWS, Azure, MongoDB, and Okta in the same query. The challenge is fragmentation: proving who has access to what requires stitching together permissions from systems that don’t talk to each other. Unsurprisingly, 42.7% of IAM prompts mention workflows, indicating teams want continuous monitoring rather than one-time audits.


When you layer *intent* on top of the security domains analysis, clearer patterns emerge, and some combinations show up more than others.


## Attention Hotspots: AppSec discovery leads the way


Each prompt falls into one security domain and one action type (intent). When you layer these together, patterns emerge, and not all combinations carry equal weight. A few gravity wells dominate where practitioners spend their time:


**AppSec +** ***Discover*** **totals 14.8% of all prompts.** Teams want to know what vulnerabilities exist before deciding what to fix.


**AppSec +** ***Explain/Investigate*** **comes next,** highlighting the move from what to why, and understanding context for prioritization.


**Cloud +** ***Assess Risk*** **accounts for 6% of all prompts.** Those focus on questions around blast radius and ranking risky misconfigurations. Practitioners need to understand exposure levels *contextually* .


***Discovery*** **in IAM completes the top four.** Access audits, proving least privilege, prep for SOC 2 are just a few reasons teams need this information, particularly across data sources.


## Security domain focus shifts as teams grow


Data shows that as organizations grow, security attention migrates in a predictable pattern:


- Small teams focus on cloud configurations
- Mid-market teams focus on application vulnerabilities
- Larger teams focus on identity and access


The shift reveals where risk becomes most acute at each stage of growth:


**Among startups and small businesses** (up to 100 employees), cloud and infrastructure security is the main focus. Questions like, “what’s misconfigured in our AWS account?” are common in environments that are smaller and contained, and where a single misconfiguration can pose enormous risk.


**For mid-market organizations** (100-1,000 employees) **,** application security takes over as the primary concern. Vulnerability management becomes the dominant friction point as codebases grow, development teams multiply, and the number of repos multiplies. The questions shift to “what’s risky in this code?” because teams ship faster with more developers, and keeping track of what’s exploitable becomes the bottleneck.


**In larger organizations** (1,000-2,000 employees), on the other hand, Identity and Access Management rise to the top. Access governance and audit evidence become critical at scale. Proving least privilege, satisfying auditors, and reducing privilege creep require systematic discovery and validation. The questions sound like “Who has permissive access and should they?”


## Breaking the silos: What users actually asked


Keyword analysis reveals what practitioners bring to the conversation. These patterns show up directly in the language people use, independent of how we classified the prompts.


Over 15% of requests and questions included specific providers. Cloud providers such as AWS, Azure, and GCP are named directly in 8.9% of prompts. Meanwhile, source code management tools show up in 6.7%. GitHub and GitLab references appear when teams want to understand repository risk.


In addition, prioritization language appears in 4.8% of prompts. Words like “critical,” “high-risk,” and “urgent” signal the need for assistance in context and prioritization.


### Examples prompts practitioners typed in 2025:


Show me my exposure to the React CVE-2025-55182 vulnerability across all my repositories. Which apps are using affected versions?


List the consultants who still have active access but haven’t used our systems in 90+ days. Include who they are, what they can access, and what to revoke.


Scan all our GitHub repos for these incident NPM packages + versions and tell me if any repo references them.


Something looks weird in my AWS environment. Can you check for any suspicious activity, resource anomalies or signs of a breach?


Flag employees who are leaving and summarize their remaining access, security posture, and any risky recent activity.


## Notable evolution from discovery to monitoring


While security teams may be slower than others to adopt AI, their relationship with it is evolving rapidly. Comparing questions from early adopters (May through July) to later users (November through December) shows clear evolution of intent patterns:


Questions seeking AI assistance to **Monitor** and **Track** grew 8.8 percentage points in this later period, while requests to **Report** and **Communicate** grew 2.6 percentage points. Meanwhile, the **Discover** intent pattern dropped 6.9 percentage points.


Early users asked “what is this?” while later users asked “keep watching this.” The workflow emerging from the data follows a pattern: “Show me what’s there” (Discover), then “explain why it matters” (Investigate), then “now watch it over time” (Monitor). Before AI can act, teams need it to decode. But once teams understand their environment, they want AI to help them track changes.


**AI for cybersecurity is maturing from a question-answering mechanism into operational infrastructure.** The shift reveals something important about how practitioners want to work. AI starts as an interpretation layer helping to understand alerts, findings, and configurations. But teams don’t want to ask the same questions repeatedly, they want to ask once, understand the answer, and then monitor whether anything has changed.


The pattern isn’t unique to security. Andrew Ng, a leading AI researcher, educator, and entrepreneur, describes the same evolution\[2 \] across enterprise AI. “Instead of just prompting an element to get a response, you can map out a much more complex workflow,” he explains. Ng predicts businesses will spend the next decade “figuring out how to implement very complex workflows in these iterative[multi-step agentic workflows](https://sola.security/blog/agentic-workflows-security-operations/) .”


## The practical AI for security


This report reflects what thousands of security practitioners asked Sola to help them understand, assess, and monitor throughout 2025. Sola is the practical, contextual AI platform for security teams, built to meet practitioners and security leaders where they actually are, not where vendors wish they were.


If you’re interested in exploring how AI can help your team build continuous clarity across your security stack, simply sign up for free, connect your data sources and start answering key security questions.


[Try Sola for free](https://auth.sola.security/oauth/account/sign-up)


## Methodology


- **Dataset:** 7,592 prompts from 2,052 unique users across 2,000 workspaces.
- **Time frame:** May 28, 2025 through December 31, 2025.
- **Classification:** Each prompt was tagged to one security domain (Pillar) and one action type (Intent). Classifications were done programmatically based on prompt content and user behavior patterns.
- **Source:** Sola Security platform usage data, anonymized and aggregated. All firmographic data in this report was willingly shared by users of Sola; data that refers to firmographics excludes users who did not explicitly share any business information.
- **What we don’t claim:** This report does not measure success rates by AI, detection accuracy, or remediation outcomes. We report attention and friction patterns based on what practitioners asked for help with, not the prevalence of actual threats or vulnerabilities in their environments.


### References


\[1\][ISC2 Research Reveals Cybersecurity Teams Are Taking a Cautious Approach to AI Adoption](https://www.isc2.org/Insights/2025/07/ISC2-Research-Cybersecurity-Teams-Cautious-on-AI-Adoption)


\[2\][From Models to Agentic Systems: The Next Enterprise AI S-Curve | Andrew Ng joins VB Transform 2025](https://www.youtube.com/watch?v=lB8jythC-vQ)
