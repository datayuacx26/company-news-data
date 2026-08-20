---
schema_version: "1.0.0"
document_id: "814410540f5f1c141613131961ca73132baee61ffb14acf0bb1ca89c0b8d7fe9"
company_key: "yc-embrace"
company: "Embrace"
source_id: "yc-embrace-rss-35f82ba9b7dd"
canonical_url: "https://embrace.io/blog/the-future-of-ai-in-observability/"
published_at: "2026-06-05T08:04:35+00:00"
first_seen_at: "2026-07-20T23:20:15.781670+00:00"
fetched_at: "2026-07-28T20:49:31.266141+00:00"
content_hash: "sha256:ccd527f568c8204316c3ce898cb71b39d08ee0a1ee616b511d7ec1ce9e22cb74"
---

# What 300 frontend engineering teams told us about the future of AI in observability

We surveyed 300 frontend engineering teams. 89% use AI in their workflows, but only 8% apply it to observability. See what drives the gap and how your team compares.


We recently[surveyed 300 engineering professionals](https://embrace.io/guides/ai-observability-gap-report/) across 16 countries to find out where web and mobile teams actually stand on AI and observability. After all, you can’t throw a stick without hitting a new AI SRE solution, but is the same true for frontend teams? Are they actually using AI in their observability practices today? Perhaps more importantly, are they even ready for it?


Let’s start with what’s probably obvious in 2026 when it comes to using AI in software engineering. Almost everyone is using AI. Eighty-nine percent of respondents use AI tools somewhere in their workflow, and 52% reach for them every single day. Agentic engineering has not only arrived, it’s kicked down the door and parked itself on the couch.


However, it gets interesting when we look at where teams are using AI:


- Seventy-eight percent use AI for code generation.
- Sixty-three percent use it for debugging.
- Fifty-seven percent use it for code review.
- Fifty-five percent use it to write tests.


You might be nodding at these numbers because this tracks with how your team uses AI today. But what’s interesting is that only 8% of teams actively use AI for observability tasks. And 29% did not even know that was an option.


That is the finding that should give you pause.


You’ve probably known for a while that developer adoption of AI was on an “up and to the right” trajectory. For example, last year’s[Stack Overflow Developer Survey](https://survey.stackoverflow.co/2025) , the largest of its kind with nearly 50,000 developers worldwide taking part, found that 84% of respondents are using or planning to use AI tools. It also found that more than half of professional developers use them daily.


Our survey data, focused on web and mobile teams, shows the same pattern. AI tools have made it into the terminal. They’ve made it into the IDE. They’ve made it into everywhere your teams are writing, testing, and shipping software.


But they haven’t made it into the places where you’re finding out how your users actually experience your software.


In fact, there’s a sizable gap when it comes to web and mobile teams using AI for observability.


Let’s explore why this is happening.


## Where frontend teams actually stand when it comes to observability


Before we get to AI, let’s start with the observability gaps affecting frontend teams today. After all, bolting AI onto a weak foundation isn’t a reliable way to improve outcomes long-term.


So, do frontend teams even need better observability?


Ninety-three percent of the teams we surveyed say frontend performance is at least somewhat critical to their business outcomes. However, 74% of teams are stuck at observability maturity levels 2 and 3 (on a scale of 1-5). They find themselves caught somewhere between reactive firefighting and proactive prevention. In fact, only 8% of respondents assess their teams as having strategic observability practices.


What’s even more surprising is that only 5% of surveyed teams have fully correlated their frontend and backend observability. And out of four key capabilities, “tying frontend issues to backend causes” had the lowest confidence scores, with almost half of respondents rating themselves as “neutral” or below.


To put that into perspective, almost half of engineers struggle to connect user-impacting issues, like a slow screen load, to the backend service that caused it.


We call this **the** **explanation gap** .


Most teams can detect that something is wrong, but they can’t explain why. And these visibility gaps are incredibly costly, leading to:


- Long issue remediation timelines
- Burnout in engineering teams
- Degraded user experience
- Negative brand perception
- Decreased business revenue


Look no further than[DORA](https://dora.dev/) ‘s decade of research on what separates high-performance teams from everyone else. It’s the ability to detect and recover from failures in production. And data from the[2024 DORA Report](https://dora.dev/research/2024/dora-report/) showed that elite teams recovered from failed deployments 2,293 times faster than low performers.


That’s the difference between resolving an issue in under an hour compared to somewhere between a week and a month.


For engineering teams that want to close that gap, they need visibility into what is happening across the entire stack.


## What’s preventing frontend teams from achieving better observability?


The dominant barrier to better observability is time and resources, cited by 62% of respondents. This is followed by:


- Not enough automation, at 40%
- Poor signal-to-noise ratio, at 37%
- Budget constraints, at 31%
- Data volume costs, at 29%


But these survey aggregates hide the more useful story. After all, a five-person startup and a 5,000-person engineering org do not hit the same wall.


If your team is small, 1 to 50 engineers, the automation gap bites hardest. Fifty-nine percent of small teams cite insufficient automation, against just 29% of mid-size orgs. Since you cannot throw more headcount at the problem, every manual workflow compounds faster.


If you are running an engineering organization of 1,001 and up, the wall is a different one. Budget, data volume costs, and leadership buy-in all spike to 40%. Those are the problems that grow with scale.


What’s interesting about these constraints is that they cannot be solved by just adding AI. While using AI in observability workflows can reduce toil and improve automation, it will only help if your instrumentation and tooling give it powerful context to work with.


## Mobile and web teams don’t have the same visibility


Mobile and web teams are not just building two flavors of the same frontend. They’re contending with vastly different app, resource, device, and environment constraints. This impacts how they build, ship, test, and release new versions.


All this to say, it’s not surprising that mobile and web teams report different observability profiles.


Web teams are more than twice as likely to be well instrumented, at 47% versus 21% of mobile teams. Mobile teams, in turn, are almost four times more likely to still be running on basic monitoring, at 26% versus 7% of web teams.


The core observability friction differs, too. Mobile teams name tooling complexity as their top barrier, at 37%. Web teams name a lack of leadership buy-in, at 33%. In other words, one group is wrestling with their tools, while the other is wrestling with the org chart.


It makes sense that mobile teams struggle with visibility as well, because while they all use crash reporting, only 37% use real user monitoring (RUM) solutions in production. This is compared to 87% for web teams.


As end-users demand better, faster, and more streamlined digital experiences, mobile teams are starved for the context needed to truly measure performance and reliability effectively.


This visibility gap translates directly into a lack of awareness into how AI can help with observability. Forty-seven percent of mobile engineers do not know AI can help with observability at all, against 17% of web engineers.


One explanation for this disconnect could be the relative difference in observability maturity between these teams. After all, AI is only as helpful as the data you provide it. Mobile teams might be better served by building stronger observability foundations before reaching for AI assistance at this stage.


## The biggest AI gap that’s affecting frontend teams


While 89% of survey respondents actively use AI, only 8% use it for observability. And by no means can this difference be attributable solely to a lack of awareness. For example:


- Twenty-eight percent have experimented with using AI for observability.
- Thirty-five percent are aware of it but have not started.
- Only 29% did not know it was an option.


It’s not that a vast majority of frontend teams aren’t interested in using AI for observability. They’re just sitting at earlier stages of awareness and trial.


What’s most surprising is that bottom segment. In 2026, nearly a third of teams have never heard of AI for observability.


You do not have to take our word for this pattern, either.[Stack Overflow’s 2025 Developer Survey](https://survey.stackoverflow.co/2025) asked nearly 50,000 developers which parts of their workflow they use AI for.


“Deployment and monitoring” came in dead last.


In fact, only about 17% of developers use AI for it at all, and 76% reported that they have no plans to start.


These two surveys had completely different methodologies, yet they reached the same structural conclusion. Observability is the last workflow AI has touched.


In terms of explaining why this gap exists, our survey points to three clear reasons.


### Lack of awareness


For the 29% of respondents who do not know AI can help with observability tasks, this is a tooling visibility problem. AI for frontend observability has not been productized and marketed to the same extent as GitHub Copilot or Cursor. And while AI SRE tooling is drawing significant attention, adoption, and investment, the frontend observability space has gotten far less of each.


### Lack of observability foundation


Teams at low observability maturity levels (1-2 out of 5) are the least likely to have encountered AI for observability, and 67% of level-1 teams do not know it exists. There is a precondition hiding in that number. You need enough instrumentation to give AI something to meaningfully analyze.


Pointing AI at an uninstrumented app is like handing a detective a crime scene with no evidence. The reasoning engine is fine. There is just nothing for them to reason about.


Engineering teams need to collect high-fidelity datasets to understand how users experience their web and mobile apps. If you cannot connect all the signals that show where performance and reliability impact user behavior, neither can AI.


### Lack of trust


Engineers are broadly positive about AI, with 77% viewing it favorably. However, they have several concerns about turning it loose in production. The top one is hallucinations, named by 68% of respondents. During an active incident, an AI that gives you a confident wrong answer can be worse than no answer at all.


But lack of trust doesn’t necessarily mean a lack of opportunity.


Seventy-two percent of teams believe AI will be very important or mission-critical to observability within two to three years. And the demand for AI runs ahead of the confidence gap in every single observability capability we tested. As an example, take pattern and anomaly detection. Thirty-seven percent of teams lack confidence in it, but 75% want AI to help them do it.


Even confident teams see AI as a force multiplier, not just a way to patch a weakness.


## What it takes for AI to earn engineers’ trust


If 72% of teams expect AI to matter but only 8% are using it, what is standing between those two numbers? It comes down to trust, and the engineers we surveyed were specific about what would earn it:


- Sixty-nine percent want verifiable outputs, so they can check the AI’s work instead of taking it on faith.
- Sixty-six percent want transparency in how it reaches a conclusion, because nobody wants a black box making calls during an incident.
- Fifty-seven percent want human-in-the-loop workflows, where AI is the starting point for an investigation rather than the final word.


Stack Overflow’s 2025 data backs up this finding as well. The single most-cited frustration with AI tools, reported by 66% of developers, was “AI solutions that are almost right, but not quite.” In observability, especially during outages, “almost right” answers can easily lead to longer resolution timelines and costlier outcomes than relying on manual efforts.


What teams need in order to trust AI shifts as they mature:


- Reactive teams (levels 1-2 out of 5) lean hardest on guardrails, with 74% wanting verifiable outputs and 70% wanting a human-in-the-loop.
- Proactive teams (levels 3-4 out of 5) have largely moved past that. Only 38% still demand human-in-the-loop, and 42% want peer proof and case studies before they commit.
- Strategic teams (level 5 out of 5) have a different worry entirely. Data control ranks in their top three at 57%, highlighting a shift towards governance as a key trust priority.


One more preference cuts across every tier. Sixty-eight percent of teams want AI embedded in the dev environment and observability platform they already use, not a separate tool that forces them to context-switch.


They don’t want or expect AI to replace their observability tooling.


They want it woven throughout how they’re already working.


## Are you ready for observability in the age of AI?


AI is not just changing how engineers write code. It is changing how much code ships and how fast. The[2025 DORA Report](https://dora.dev/research/2025/dora-report/) found that a 25% jump in AI adoption lines up with a 7.2% drop in delivery stability, because faster code means bigger changes, and bigger changes break in production more often.


The same tools driving 89% of teams to actively use AI are also quietly raising the cost of not knowing what happens after you ship. In the age of AI, your need for observability is going up, not down.


That is why what engineers want from AI is so telling. Pattern and anomaly detection tops the list, followed by regression detection and then root cause analysis.


Those are not random wishes.


They map almost exactly onto the explanation gap, where teams can detect that something is wrong but can’t explain why. Engineers want AI to do the very things their teams cannot do well today.


The teams that can bridge this gap are the ones investing in their observability foundations now. For example, our survey results show that strategic teams are experimenting with AI at three to five times the rate of reactive teams.


Observability is challenging. You don’t have to have it all figured out now. What’s important is knowing where your team stands and what steps you can take to improve how you deliver value to users.


As a first step,[take the frontend observability maturity self-assessment](https://get.embrace.io/observability-assessment) . In two minutes, you’ll see your maturity score, how you compare to peers, your biggest observability gaps, and get a personalized action plan.


Deliver incredible mobile experiences with Embrace.


Get started today with 1 million free user sessions.


[Get started free](https://dash.embrace.io/signup/)


Author


[Colin Contreary](https://embrace.io/author/colin/) Colin Contreary is the head of content at Embrace. He brings 16 years of writing experience across comedy, musicals, and technical writing. Colin has been with the Embrace team since 2020. His passion for technology makes him a notable member of the team. He has self-taught web development skills that support his technical writing.


Related Content


observability


21 May 2026


• 4 min read


### [Beyond the golden signals: Observability built around the end-user](https://embrace.io/blog/beyond-the-golden-signals/)


The golden signals are not wrong, but they only tell you server-side health. You need user-focused observability to tell you whether users are succeeding.


AI


13 March 2026


• 4 min read


### [Embrace MCP Server adds web support and new tools for spans, logs, and exceptions](https://embrace.io/blog/embrace-mcp-server-web-support-spans-logs-exceptions/)


The Embrace MCP Server now has web support as well as new tools for querying span, log, and exception data.


observability


6 March 2026


• 4 min read


### [Monitoring Vs Observability Vs Telemetry: What’s the Difference](https://embrace.io/blog/monitoring-vs-observability-vs-telemetry-whats-the-difference/)


In this article, we’ll clarify what sets monitoring, observability, and telemetry apart, and why a clear grasp of these terms is crucial for building resilient systems.
