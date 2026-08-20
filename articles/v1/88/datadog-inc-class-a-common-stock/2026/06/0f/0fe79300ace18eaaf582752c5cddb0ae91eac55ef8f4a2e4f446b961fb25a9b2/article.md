---
schema_version: "1.0.0"
document_id: "0fe79300ace18eaaf582752c5cddb0ae91eac55ef8f4a2e4f446b961fb25a9b2"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/devex-measurement-pitfalls-ai-era/"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-25T01:09:56.516023+00:00"
fetched_at: "2026-07-28T20:47:51.174373+00:00"
content_hash: "sha256:c8f167f61f5d4f2cd988f3f37d14b4f677bd62b1ea839428afad9f072a3820c1"
---

# 5 pitfalls to avoid when measuring DevEx in the AI era

Candace Shamieh


Technical Content Writer


Teddy Gesbert


Product Manager


Developer experience, commonly known as DevEx, describes how an organization’s systems, workflows, tools, and culture affect developer productivity. A positive DevEx leads to tangible organizational benefits, including faster releases, increased innovation, and reduced technical debt. Measuring DevEx enables engineering management to quantify their team’s impact and understand where to direct improvement efforts.


While there are differing approaches to[DevEx measurement](https://www.datadoghq.com/blog/how-to-measure-developer-experience-in-the-ai-era/#what-is-devex) , attempting to quantify it in terms of individual metrics is always a temptation. The widespread use of AI has only added to that temptation, with leaders measuring individual token consumption in addition to metrics like lines of code or PR count. But tracking what is easy to count can adversely impact DevEx, and the gap between individual metrics and developer productivity continues to widen. According to the[2025 JetBrains Developer Ecosystem survey](https://resources.jetbrains.com/storage/products/research/DXDP.pdf?_gl=1*13hz50u*_gcl_au*Mzg0NjYwMDAxLjE3Nzk0ODg5MTc.*FPAU*Mzg0NjYwMDAxLjE3Nzk0ODg5MTc.*_ga*MzkxNDQyNzMyLjE3Nzk0ODg5MzU.*_ga_9J976DJZ68*czE3ODA2MDUwNzYkbzMkZzEkdDE3ODA2MDUzMjIkajYwJGwwJGgw&_cl=MTsxOzE7T2hvNVFzSXdQVVBkMjBNZGNBeTdjcjU0ZlQ2WTVmUk5hTk5rNnJ1ZVFwRlJmQ1cyYzBQM2xxUWJ6NERRZU5DUjs=) , 66% of developers do not believe that current metrics reflect their contributions.[Atlassian’s 2025 State of Developer Experience report](https://www.atlassian.com/blog/developer/developer-experience-report-2025) found that 63% of developers do not believe leaders understand their pain points, a sentiment that increased significantly from 44% in 2024.


In this post, we’ll discuss how to avoid the five most common pitfalls that organizations fall into when measuring DevEx in the AI era:


-


Measuring individual output instead of system health


-


Relying on system metrics without perceptual data


-


Equating AI adoption with efficiency


-


Treating AI token usage as a productivity metric


-


Measuring without developer buy-in


## Measuring individual output instead of system health


Output metrics like lines of code, PR count, number of commits, and story points can provide valuable insight when analyzed at a team level, but undermine developer trust, incentivize gaming, and discourage collaboration when tracked at the individual level. Mentorship, code review, task planning, and knowledge sharing are all collaborative efforts that[SPACE framework researchers](https://queue.acm.org/detail.cfm?id=3454124) have identified as “invisible” yet highly valuable work that benefits the entire organization. When individual output is tracked, developers may be incentivized to devote a disproportionate amount of time to the work that appears on a dashboard.


### Track system health to improve DevEx outcomes


[Goodhart’s law](https://en.wikipedia.org/wiki/Goodhart%27s_law) states, “When a measure becomes a target, it ceases to be a good measure.” For example, if you track the number of PRs per developer, they may split up commits to generate more PRs; if you track individual deployment frequency, they may deploy trivial changes. The metrics will improve without a parallel meaningful improvement in productivity.


Datadog engineering leadership has made it clear that the goal of measurement is never to evaluate individual performance.[We track a variety of DevEx metrics](https://www.datadoghq.com/blog/how-to-measure-developer-experience-in-the-ai-era/#which-metrics-reveal-devex-friction) , and team-level aggregation is the most granular that we use internally and surface in our product.


More than a decade of DORA research has shown that following development practices that improve system health, such as continuous integration, test automation, and fast feedback loops, also improve organizational performance. The[2025 DORA Report](https://cloud.google.com/resources/content/2025-dora-ai-assisted-software-development-report) found that this remains true in the AI era: Teams with fast feedback loops and automated testing experience accelerated delivery when using AI, while teams with systemic issues see AI magnify their instability.


By tracking system-level and workflow-level metrics that reflect how code flows through the CI, time saved through automation, and whether services meet engineering standards, you can target improvement efforts that positively impact developer productivity.


## Relying on system metrics without perceptual data


System data is precise but incomplete. A pipeline that looks healthy by every objective measure can still be a source of frustration if it interrupts developers at the wrong time or produces error output that is difficult to parse. System metrics can reveal how long code reviews take, but cannot inform you whether developers are receiving valuable feedback. Perceptual data provides context that makes the friction visible and, consequently, actionable.


Our internal H1 2026 Engineering Experience survey received over 2,400 comments. The quantitative scores revealed where friction was concentrated, while the comments revealed why. For example, many of our engineers reported that their primary method for understanding CI failures was copying error logs into an AI assistant for interpretation, since the tooling didn’t clearly surface the reason. We used that direct feedback to shape our Q2 objectives and key results (OKRs). Without perceptual data, we would not have known to prioritize an investment in failure attribution, a solution that directly relieved developer frustration.


### How to gather DevEx feedback without survey fatigue


Surveying developers too frequently produces diminishing returns. We survey developers twice annually, and host regular office hours and “coffee chats” to give developers alternative channels to share feedback in small group settings. We’ve found these mechanisms to be a useful complement for engineers who may not want to document their sentiment in a survey or raise concerns directly to a manager.


## Equating AI adoption with efficiency


Many organizations treat AI adoption as evidence that productivity is improving.[Eficode](https://www.eficode.com/) , an organization that helps companies build and scale AI-native software safely, found that this is the most common misconception about AI ROI. After surveying over 270 organizations for their 2026 report titled “[How Software Organizations are Moving from AI Pilots to AI Transformation](https://www.eficode.com/guides/how-software-organizations-are-moving-from-ai-pilots-to-ai-transformation) ,” they emphasized that AI adoption is only the beginning of a transformation, and for most organizations, AI is still nowhere near delivering any real productivity gains. Assuming AI tools are effective simply because developers are using them can lead organizations to believe that ongoing investment in infrastructure is unnecessary.


### Why self-reported AI impact is unreliable


Self-reported AI impact is unreliable. Even when working in repositories they know well, developers tend to overestimate how much AI accelerates their workflows.[METR](https://metr.org/) , an independent nonprofit research institute that evaluates frontier AI models, conducted a randomized control trial to monitor the behavior of open source developers. When asked how AI had impacted their work, the developers estimated that AI tools accelerated their workflows by 20%. In reality, using AI tools had caused them to take 19% longer to complete their tasks, despite the fact that they had worked in the repositories for over 5 years.


If adoption is mandated rather than organic, it is a reflection of compliance.[Stack Overflow’s 2025 Developer Survey](https://survey.stackoverflow.co/2025/?utm_medium=referral&utm_source=blog&utm_campaign=dev-survey-2025&utm_content=survey-results) found that while 84% of developers reported using AI tools, 46% actively distrusted the accuracy of AI’s output.


### How to quantify AI’s impact on software delivery


Datadog treats AI adoption as a starting point as opposed to a north star goal. We ask perceptual data questions in our Engineering Experience survey to understand whether developers are using AI tools, understand their limitations, and feel confident prompting. Approximately 80% of our PRs are now AI-assisted, and 92% of our engineers report using AI coding tools daily. We are past the adoption barrier and supplement self-reported usage by instrumenting AI’s impact on the SDLC.


We analyze AI-assisted versus non-assisted PRs, comparing DORA and other software delivery performance metrics across both categories. Our analysis showed that AI-assisted PRs had a slightly longer change lead time, but a much higher concurrency overall. In our case, the data reveals that AI coding assistants act as a force multiplier, enabling developers to work on more changes simultaneously.


Our adoption telemetry data comes from an[internal AI gateway](https://www.datadoghq.com/blog/llm-observability-at-datadog-security/#monitoring-in-production) that captures every interaction with coding assistants at the network layer, server side. Because attribution is logged at the gateway rather than in the developer’s IDE, it can’t be disabled or edited after the fact.


## Treating AI token usage as a productivity metric


Using token consumption as a productivity signal has been normalized. Enterprise organizations have created[token leaderboards](https://fortune.com/2026/04/09/meta-killed-employee-ai-token-dashboard/) where employees are ranked by token usage, and their leadership has been encouraged to view high consumption as a signal of developer efficiency. While tracking tokens isn’t wrong in and of itself, treating it as a proxy for productivity[incentivizes gamification](https://www.forbes.com/sites/timkeary/2026/04/13/is-the-cult-of-tokenmaxxingjust-another-fad-or-the-new-normal/) like[tokenmaxxing](https://www.inc.com/ben-sherry/what-is-tokenmaxxing-ai-productivity-hack/91328999) , creates waste, and can even cause outages.


Adding token volume to performance reviews is a misaligned incentive that leads developers to optimize for the metric rather than the organizational goals. It leads developers to run larger, more frequent prompts, not taking into account whether output is useful. Developers who use AI selectively by reviewing output, editing suggestions, and declining unhelpful completions will appear as less productive even though their approach produces better results.


Token consumption is appropriate to track to manage AI tool costs and evaluate whether a more efficient model can produce equivalent output at lower spend.


### Track delivery outcomes instead of token consumption


Instead of tracking token consumption to measure productivity, we analyze[specific metrics](https://www.datadoghq.com/blog/platform-engineering-metrics/#use-outcome-metrics-to-confirm-platform-value) that are surfaced from our AI instrumentation. Our[AI Impact](https://www.datadoghq.com/blog/ai-impact/) feature tracks metrics like PR throughput, change lead time, change failure rate, and recovery time for AI-assisted and non-AI-assisted PRs.


When AI adoption climbed 25 percentage points (pp) year-over-year from 2025 to 2026, we compared that with the simultaneous declines in CI reliability (-3 pp), test satisfaction (-4 pp), and PR review quality (-5 pp). The analysis enabled us to pinpoint how bottlenecks are shifting in the SDLC and what our platform team needs to invest in next.


## Measuring without developer buy-in


Launching a measurement program without first consulting developers signals that metrics are being collected to evaluate them rather than improve their experience. When developers discover that they are being measured before being consulted, the signal lands as surveillance rather than support. Trust erodes immediately, and gamification follows.


Although we have a dedicated DevEx team, we believe that developers should remain highly involved in the ongoing efforts to improve DevEx for our entire organization. We host an “embed” program, where DevEx team members switch roles with a developer for an entire sprint or longer. The embed rotation program provides reciprocal benefits, enabling DevEx team members to experience system and workflow pain points firsthand, and giving developers the opportunity to work on an innovative DevEx project that alleviates friction for their peers across the organization.


## Start measuring what matters to developers


The pitfalls in this post were shaped by an era where AI suggests code and a human decides what to keep, a reality that is already ending. Agentic code review, autonomous CI triage, and AI-driven test generation are transitioning from mere experiments into production workflows. When AI agents begin making decisions as opposed to suggestions, then the measurement question shifts from “Is AI helping my developers?” to “How do I evaluate work that no human performed?”


The principles in this post still hold in that environment. When the landscape changes and what needs to be measured expands, the organizations that avoided these pitfalls are better positioned to adapt as the role of the developer continues to evolve.


To learn more about how to measure DevEx, read our[How to measure developer experience (DevEx) in the AI era](https://www.datadoghq.com/blog/how-to-measure-developer-experience-in-the-ai-era/) blog post. To start using tools that can help, visit the[AI Impact](https://docs.datadoghq.com/delivery_performance/ai_impact/) ,[DORA Metrics](https://docs.datadoghq.com/dora_metrics/) ,[CI Visibility](https://docs.datadoghq.com/continuous_integration/#overview) ,[Test Optimization](https://docs.datadoghq.com/tests/) , and[Internal Developer Portal](https://docs.datadoghq.com/internal_developer_portal/) documentation. To start measuring the right metrics,sign up for a free 14-day Datadog trial .


-
-
-
