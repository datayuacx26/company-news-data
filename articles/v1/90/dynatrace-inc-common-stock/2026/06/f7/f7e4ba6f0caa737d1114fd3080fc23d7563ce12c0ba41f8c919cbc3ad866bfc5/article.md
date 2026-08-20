---
schema_version: "1.0.0"
document_id: "f7e4ba6f0caa737d1114fd3080fc23d7563ce12c0ba41f8c919cbc3ad866bfc5"
company_key: "dynatrace-inc-common-stock"
company: "Dynatrace Inc."
source_id: "dynatrace-inc-common-stock-rss-2f172b160f47"
canonical_url: "https://www.dynatrace.com/news/blog/how-dynatrace-and-aws-are-reshaping-cloud-operations/"
published_at: "2026-06-16T17:30:03+00:00"
first_seen_at: "2026-07-20T03:31:43.037247+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:9e36737c791013b05bddc60a2b2a2ed0f35e4786ae9e64ae62afa913572e051a"
---

# Moving from insight to action: How Dynatrace and AWS are reshaping cloud operations

If you’re running modern applications on AWS, you already have access to more data than ever: metrics, logs, traces, and events. The real advantage comes from turning that data into actionable insights that drive continuous improvement.


Today’s challenge occurs when something breaks; teams still spend too much time connecting the dots. Pulling data from different tools, correlating signals, and trying to figure out what changed. That gap between insight and action is where time is lost, and customer impact grows.


Dynatrace and AWS are working together to close that gap.


With the introduction of[AWS DevOps Agent](https://aws.amazon.com/devops-agent/) and deeper integrations across Dynatrace and AWS services[like Kiro](https://www.dynatrace.com/news/blog/dynatrace-observability-is-now-a-kiro-power/) , the experience moves beyond monitoring into AI-powered observability. It becomes a connected system that detects issues, investigates them, and feeds directly into how teams fix and improve software.


## How we got here


At AWS re:Invent, AWS introduced DevOps Agent, a new type of AI agent built to investigate and resolve issues across cloud environments. Instead of relying on manual troubleshooting, the agent works continuously across services to understand what changed and why. Dynatrace has been part of that effort since the start. After all, if these agents are going to be effective, they need real production context. That’s the Dynatrace specialty.


That collaboration evolved quickly. Sharing observability data is now a two-way integration. Dynatrace detects and understands issues.[AWS DevOps Agent investigates them](https://www.dynatrace.com/news/blog/integration-with-aws-devops-agent-autonomous-investigations-powered-by-production-context/) . The results flow directly back into Dynatrace for a complete view of what happened and what to do next.


## The next iteration of the Clouds SRE app


As customers started using the initial implementation, one thing became clear. The foundation was there, but there was an opportunity to make the experience more streamlined, more transparent, and more aligned with how teams actually operate at scale.


With the introduction of the[Clouds SRE app](https://www.dynatrace.com/hub/detail/community-cloudsreagents) , that experience has evolved in a meaningful way.


Instead of requiring users to manually configure multiple workflows, onboarding is now guided. Teams can get started faster without needing to define everything upfront. What used to take several workflows is now simplified into a more focused, purpose-built experience across AWS.


Visibility is another big step forward. Previously, there was limited insight into what agents were doing during an investigation. Now, teams have transparent tracking into agent activity, including approvals, notifications, and the ability to automatically re-run stalled investigations. That shift alone gives teams more confidence in how work is executed.


Routing and management have also become much more intuitive. Rather than managing workflows behind the scenes, teams can now use interaction profiles directly in the UI to control how agents are routed, filtered, and managed. It brings that control closer to where teams already operate.


And importantly, the scope of what agents can do has expanded. What was once limited to investigations now includes mitigation actions as well, allowing teams to move from insight to action without changing context.


Finally, there’s a stronger focus on outcomes. With built-in executive summaries and efficiency metrics, teams can now understand the impact of these workflows in real terms, not just activity.


Taken together, this is a shift from a workflow-centric model to an experience that is guided, observable, and outcome-driven.


## From investigation to resolution


Because the integration is two-way, everything stays connected. Findings from AWS DevOps Agent flow directly back into Dynatrace. Root cause, impacted services, and recommended fixes are all visible in a single place.


Teams no longer need to switch between tools or reconstruct the story themselves. The full path from detection to resolution is already laid out.


For teams running distributed systems on AWS, this changes daily operations. Instead of spending time figuring out where to look, teams can focus on fixing the issue and preventing it from happening again.


## Bringing that context to developers with Kiro


This is where the story extends beyond operations. Kiro, AWS’s agentic development environment, brings that same production context directly into the developer workflow.


Instead of waiting for a handoff from operations, developers can access real production insights while they are building and fixing code. They can see exactly what failed, understand the root cause, and apply fixes with the same context that was used during investigation.


This removes one of the biggest sources of friction in software delivery. Developers are no longer dependent on separate teams to translate production issues. They are working from the same data in real time.


Now we have a closed loop: Dynatrace detects the issue. AWS DevOps Agent investigates it. Kiro brings that insight directly into the codebase where it can be resolved and improved.


That closes the gap between production and development in a way that was not possible before.


## What this means for you


If you are running applications on AWS today, these developments can result in:


- Less time spent correlating data across tools
- Faster and more consistent incident resolution
- Fewer handoffs between operations and development
- Direct access to production context during development


Most importantly, it shortens the feedback loop. Issues are not just detected faster; they can be understood and resolved faster, and improvements can be applied to the code without delay.


## Getting started


If you are already using Dynatrace on AWS, the next step is to[connect these workflows](https://www.dynatrace.com/hub/detail/community-cloudsreagents) .


Start by enabling the AWS DevOps Agent integration with Dynatrace to bring automated investigation into your environment. From there, extend that same production context into developer workflows with Kiro so your teams can act on insights directly.


This is the fastest way to move from insight to action and start seeing the value in day-to-day operations.


For more information on how Dynatrace and AWS work together, learn how[NAIC embedded AI‑powered observability directly into the IDE](https://www.dynatrace.com/news/blog/how-naic-embedded-ai-powered-observability-directly-into-the-ide/) , or come and see us at an[AWS Summit](https://www.dynatrace.com/about/events/dynatrace-at-aws-summits/) near you.
