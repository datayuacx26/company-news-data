---
schema_version: "1.0.0"
document_id: "5496dfeab901eedc38f7e8ce719319f48fcbeccc332aa311e6734934af13fb27"
company_key: "pagerduty-inc-common-stock"
company: "PagerDuty Inc."
source_id: "pagerduty-inc-common-stock-rss-6c10cddc543b"
canonical_url: "https://www.pagerduty.com/blog/ai/shift-left-ai-code-review/"
published_at: "2026-08-12T13:00:43+00:00"
first_seen_at: "2026-08-13T14:35:14.070678+00:00"
fetched_at: "2026-08-13T14:35:16.072310+00:00"
content_hash: "sha256:56d44f50eba65d8820990471ff6fe6d9d43282796e193199f410ad9e37c7c0da"
---

# AI Is Outpacing Code Review. Here’s How to Catch Up (Without Slowing Down) by PagerDuty

In a 2025 analysis spanning over 100 large language models, Veracode found that


[nearly half (45%) of AI-generated code causes known security issues and vulnerabilities](https://www.veracode.com/blog/ai-generated-code-security-risks/) . Novel risks are being introduced into your operations systems faster than humans can manage or review. At the same time,


studies suggest that


[human review isn’t all that effective, especially beyond 400 lines of code](https://smartbear.com/learn/code-review/best-practices-for-peer-code-review/) .


But AI-generated code isn’t inherently bad. It just doesn’t always work across your whole system.


In microservice and cloud-native environments, no single engineer (or coding agent) understands the full system state. One engineer’s agent may generate code that works beautifully in development but crashes a downstream system in production—even without a single syntax error.


Most teams try to solve this with manual human review, which has turned code review into the new bottleneck. Rather than slow down, teams often fall into what


[Google’s CTO team calls “approval fatigue”](https://cloud.google.com/transform/when-ai-writes-the-code-who-reviews-it-cto-google-cloud) —a continuous stream of micro-approvals that produces low-grade exhaustion, where developers start rubber-stamping code reflexively.


But recent studies have shown that traditional review of AI code may not be that effective at all, since a human engineer’s ability to catch errors typically


[drops off after about 400 lines of code](https://smartbear.com/learn/code-review/best-practices-for-peer-code-review/) .


The efficiency gains from AI-generated code are too valuable to give up by slowing production. But AI-generated code is too risky to let into your system without some kind of review. So how can teams review code effectively without throttling their coding agents?


Leading teams are mitigating risk by building operational context into their AI agents, so the agents learn from incidents to push risk signals upstream into the developer’s environment. Instead of catching risks after deployment, developers can catch issues before they impact production—and before they cost anything.


Here’s how it works:


##### **Shifting left is the only way to solve the AI code bottleneck**


What makes human engineers good reviewers isn’t catching errors. It’s their operational memory. They know the unwritten history of past incidents, fragile edge cases, and architectural quirks that never made it into your documentation.


The problem is that human memory can’t scale. No single person can cross-check every PR against the entire operational history of a massive enterprise network. But with shift-left technology, engineering teams gain new visibility they didn’t have before.


**Shifting left** means moving critical validation checks earlier in the software development lifecycle to catch issues


*while* code is being created (rather than after it ships).


Catching hazards at the workstation before a pull request is even opened keeps bad code out of the review queue and spares human reviewers from playing detective on code that was never ready to ship.


Engineering teams already shift left in several ways that don’t require extensive manual review:


- **Linters and static analysis tools** like ESLint or SonarQube catch formatting issues and other obvious errors.


- **Static application security testing and dependency scanners** like Snyk or Dependabot flag hardcoded secrets and known CVEs in open-source libraries.


- **Local unit and integration tests** verify that isolated functions behave as expected.


Traditional shift-left tools can verify if the code is syntactically sound and secure, but they’re unable to predict whether it will trigger a cascading database failure under live traffic. Those static tools have zero awareness of live system dependencies or historical failure patterns—that requires the operational memory and knowledge earned by senior engineers.


**Shifting left in the age of AI means moving that human operational memory into the coding process itself.**


##### **How to shift left: Bringing context to the workstation**


To shift left effectively, teams first need to capture operational context automatically to create a system with more comprehensive recall than any individual can achieve.


Organizations also need to make context available where developers already work. If checking operational risk means opening a separate dashboard or logging into another tool, developers won’t do it. The signal needs to live directly inside the terminal or IDE.


PagerDuty redefines what it means to shift left. By connecting all your past incident data directly to the developer environment, PagerDuty elevates shift left strategies beyond static code analysis in three steps:


- **Capture incident data automatically.** PagerDuty continuously ingests telemetry, resolution history, chat threads, and post-incident reviews as incidents happen.


- **Build structured operational memory.** PagerDuty’s SRE agent captures raw incident data and turns it into contextual risk signals tied to specific services and files.


- **Surface risk at the workstation.** Teams can push those signals into native developer tools, flagging operational hazards and providing a risk score before code ever reaches production.


##### **Modern examples of shifting left with AI**


Here are a few examples from organizations that are putting this thinking into practice:


###### **Intuit scores risk before shipping**


At one San Francisco session from our recent PagerDuty on Tour event,


**Intuit** shared how their engineers catch risk before it ships. As a developer writes code, an AI agent checks the change against the services it touches, how far its blast radius reaches, and how many other systems depend on it. That produces a risk score that tells the developer how safe the new code is to ship (before deployment).


###### **PagerDuty shifts left with Claude Code and GitHub**


At one PagerDuty on Tour session in London, PagerDuty showed how its new


[Claude Code plug-in](https://claude.com/plugins/pagerduty) surfaces risk right inside the terminal. It detects the service a developer is working on, then runs a quick check against 90 days of incident history and post-incident reviews. In about two minutes, it flags risky patterns like missing retry logic or code that resembles a past incident, and returns a risk score with next steps.


For teams using GitHub,


**PagerDuty’s Incident Responder custom agent** brings the same context into the pull request. A developer can ask directly in the comments whether a change is safe to deploy. The agent checks live dependency health and flags unresolved issues from past reviews, so the team gets a clear safety signal before merging.


##### **Ship faster. Break less. Shift left with PagerDuty.**


AI coding gives you speed, but speed without safety just moves more work downstream. To protect production, teams need a way to spot AI-generated logic errors long before they reach a pull request.


Because PagerDuty powers the system you already use to resolve incidents, it holds the exact operational data needed to prevent them in the future. By checking uncommitted code against past incident patterns and live system topology, PagerDuty flags known failure modes while the blast radius is still zero.


Less time spent firefighting incidents also gives your engineers their time back. Fewer alerts and incidents means more room for the high-value architecture and capacity work that moves your product forward.


**Ready to eliminate the AI review bottleneck?**[Schedule a PagerDuty demo today](https://www.pagerduty.com/request-a-demo/) to see operational context in action.
