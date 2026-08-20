---
schema_version: "1.0.0"
document_id: "9ce1a81dfbe895292e8e74fe3eae1d8b6abd4c2510c733f0432bb4f6b66641d5"
company_key: "yc-escape"
company: "Escape"
source_id: "yc-escape-rss-d1370620bfa7"
canonical_url: "https://escape.tech/blog/targeted-graphql-coverage/"
published_at: "2026-07-30T09:29:32+00:00"
first_seen_at: "2026-07-30T09:30:40.720018+00:00"
fetched_at: "2026-07-30T09:30:43.196775+00:00"
content_hash: "sha256:aea91187cd69a72893cacb29dcd78ff124ea2f14d998893896572340ec0f8df6"
---

# How Escape DAST helped Sigma Computing achieve complete GraphQL API endpoint coverage

Sigma Computing is a cloud-native analytics and data application platform built to operate directly on modern cloud data warehouses. Their integrations run deep across the modern data stack, with close partnerships across Snowflake and Databricks. At that scale - and with GraphQL powering much of the application layer - API security coverage simply has to keep pace with engineering.


When they started looking for DAST tools, they had four or five on the shortlist and one requirement that eliminated most of them immediately: real GraphQL support, not a REST crawler with GraphQL bolted on as an afterthought. Now one year in, the team are running focused CI/CD scans against every new schema mutation and have 100% confidence in their endpoint coverage. This is how they got there.


## The Problem: compliance theater and crawling that missed the point


> "It was a sink that I was not even watching for months. The scan was running in the background. I wouldn't even pay attention to it. It would report the same three or four issues that I'd already marked as false positives — but there was no action, no communication, no improvement on the platform." - ***Varun Singh, Security Engineer at Sigma Computing***


Before Escape, Sigma Computing had a scanner that ran weekly but ultimately ended up just being background noise. There were three key pain points where other tools on the market simply were not providing verifiable, actionable, or deep results:


- **Tooling with REST-centric crawling engines that is not built for GraphQL APIs.** The core issue they faced was the results (or lack thereof). Since Sigma's API surface is predominantly GraphQL, this has a distinct vulnerability class REST-focused crawlers just could not find.


> "Every other platform right now that provides API security or DAST scanning, they are REST API-focused. Most of the tools out there added GraphQL as a secondary thought." - **Varun Singh, Security Engineer at Sigma Computing**


- **Relying on SAST alone is** **"still a tremendous amount of effort ... to verify" vulnerabilities.** When conducting static analysis that doesn't actually exploit the application, they found they were only getting a potential payload that something may or may not work instead of real, exploitable, verifiable results.
- **Web crawlers that sit outside the perimeter and crawl inward, missing internal endpoints.** They were looking for a tool that didn't just sit outside the perimeter, crawl inward, and miss internal endpoints entirely.


Ultimately, the team at Sigma wanted to go beyond compliance checkboxes to find "actual verified results" that they can act on to scale their security operations. That's where Escape came in.


## The Solution: schema-first scanning, global configuration, and targeted CI/CD runs


Sigma computing went with Escape because of its GraphQL-native capacities, depth of scan results, targeted scans, and the DAST's live approach to testing giving the team proof to act on rather than a starting point to investigate.


These are the key features that made Escape the right tool for them:


- **GraphQL-native scanning:** Escape's unique, in-house dynamic security scanner is native to GraphQL and fully embraces its recursive nature, which is exactly what Sigma was missing.


> "Even obvious vulnerabilities were ignored by other platforms I tested. Escape reported alias batching and denial-of-service-based attacks - it was able to catch those." - **Varun Singh, Security Engineer at Sigma Computing**


- **In-cluster deployment:** The tool runs as a container directly in Sigma Computing's Kubernetes environment, deployed via Helm charts across six regions. By sitting inside the cluster in its own pod, Escape doesn't intercept any traffic but directly hits endpoints. A tool that terminates TLS or proxies requests through an external system creates risk in production environments. Escape's in-cluster model eliminates that concern entirely.


> "That significantly makes it non-impactful on any of my internals or any customer-serving environments." - **Varun Singh, Security Engineer at Sigma Computing**


- **AI-assisted crawling that is contextually aware** : Escape's DAST conducts contextually aware and behaviorally realistic tests that both widened and deepened Sigma's coverage compared to their previous web crawlers.
- **Platform transparency with logs that let you learn:** Every URL tested, every request made, every rule fired is visible. That transparency is what makes it possible to tune the scanner, write better rules, and give meaningful feedback to the team.


> "With other tools, I never saw logs of this kind. It was just: we finished the scan, here are the results, that's it." - **Varun Singh, Security Engineer at Sigma Computing**


## The Impact: what changed at Sigma


Now one year in, Escape has filled the missing gaps in Sigma's security operations and this impact shows up across three dimensions: vulnerability coverage, operational efficiency, and triage speed.


> "I have 100% confidence that I have complete coverage over all my endpoints. I've configured the platform once — I don't have to worry about the configuration, the scan configs, or any of that. All I need to do is review the findings when they're created." — ***Varun Singh, Security Engineer at Sigma Computing***


### **A DAST layer that complements existing coverage**


Escape surfaced GraphQL-specific vulnerabilities — alias batching, denial-of-service attacks — that Sigma's DAST tooling hadn't previously been able to detect. Beyond that, in-cluster deployment uncovered MySQL instances running on default credentials in staging: developer test environments that had never been cleaned up. Their previous tooling simply did not have the capabilities to surface these findings.


Sigma already runs a layered security stack, with CNAPP tools covering much of this ground. What Escape adds is a DAST capability that this layer was missing — and that overlap is by design. When multiple tools independently report the same class of issue, there's far less chance that something slips through, regardless of why.


> "I noticed there were some MySQL instances running with default credentials — employees had spun up SQL clusters just for testing their own implementations and forgotten to clean them up. Escape got it. If this had been in production, I would be worried. I'm glad Escape reports those things." — ***Varun Singh, Security Engineer at Sigma Computing***


### **Proactive CI/CD coverage and targeted scanning**


*"The support through MCP APIs has been tremendous because I could do focused scans. I self-engineered a few things: take a look at my own schema, see if any new mutations or queries have been added, and once those are available, use the CI/CD actions Escape supports to just do a focused scan — rather than running a complete 8- or 10-hour scan. That gives me proactiveness rather than waiting for a weekly scan to finish." - **Varun Singh, Security Engineer at Sigma Computing***


With the support and flexibility that Escape offers, Sigma's security testing can now happen at the moment the code changes, not on a fixed cadence.


### **Triage time cut by exploitability evidence**


> "Using the payload Escape is generating, I'm able to quickly test it out and get good results. That is where I say it's not purely DAST. Because of the innovation in how Escape supports different mechanisms, **it's already strides ahead of traditional DAST.** " - **Varun Singh, Security Engineer, Sigma Computing**


Where SAST tools handed the team a list of hypothetical findings that each took 2–3 hours to verify manually, Escape's live testing produces confirmed results with payloads attached. They extended this further with an internal pipeline correlating Escape findings against source code, using Escape's Copilot as a reasoning layer to deliver a true/false positive verdict before anything reaches the ticket queue.


> "Since your Copilot is amazing — if I ask it, does this vulnerability look like a true positive or false positive? — it reasons perfectly. It tells me whatever available information it has." - **Varun Singh, Security Engineer, Sigma Computing**


### **Configuration flexibility that improves pace**


Global and scan-specific configuration gave Sigma precision control over what the scanner touches. Custom filters then let them apply their own risk ratings on top of Escape's findings, enabling customers to customize the platform to suit their specific security workflow.


> "What I've built recently, with production and staging vulnerabilities and the ratings that I care about, has improved my team's pace tremendously. Because what we'd seen previously is these tools throw a lot of information at you, and then you're wasting time triaging." - **Varun Singh, Security Engineer, Sigma Computing**


### Reduced maintenance burden and ongoing support


Once configured, Sigma no longer had to worry about reconfiguration. All they had to do was review findings when they arrive instead of managing a week-to-week scan.


Sigma also found the immediately available support from the Escape team to be incredibly valuable in speeding up their security processes.


> "The support from Escape side has been amazing. Within 7 to 8 hours I get my answer. They’re also willing to jump on calls to debug and fix things together." - **Varun Singh, Security Engineer, Sigma Computing**


## Looking to the future


Having expanded their DAST coverage, Sigma is now also looking to explore Escape's AI pentesting solution.


> "I've utilized open-source tools like PentestGPT with Claude models and others. It's like watching a zoo animal. I liked Escape's approach where they said: you don't have to worry about tokens or anything, we have our own custom engine and model that will take care of most of these things. All you need to do is configure the scan and the block and allow lists, and then let it run for 8 to 12 hours — and it will come up with actually conscious cases that will be of benefit to you." - **Varun Singh, Security Engineer, Sigma Computing**


What makes the AI pentesting solution distinctively interesting is that the AI pentesting tool pulls directly from the knowledge and context it has gathered having run DAST continuously against Sigma Computing's stack.


> "Escape will have an edge over others in that it is living — it is already understanding the logic. It will be utilizing what it gathered from DAST. And then I could provide it with my own knowledge." - **Varun Singh, Security Engineer, Sigma Computing**


## Why Sigma Computing recommends Escape


> "If you want something that is really flexible in terms of the use cases you want to build, as well as complete transparency — and actually learning from those to improve the platform — Escape does a great job." - **Varun Singh, Security Engineer, Sigma Computing**


For security engineers running GraphQL-heavy stacks or covering a broad surface on a small team, Varun was direct: most DAST tools will miss the vulnerabilities that matter most in GraphQL. Schema-based targeted scanning removes the coverage uncertainty that crawling introduces, detailed per-scan logs mean you can tune the scanner rather than just read its output, and the flexibility to build custom rules and integrations means the platform fits the way you actually work.
