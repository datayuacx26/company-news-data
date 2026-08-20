---
schema_version: "1.0.0"
document_id: "4ba1a426146b5f6f2766d3f0c070a2a4840878898f9ad3ff1c90dc99552a72ac"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/drop-the-site/"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-19T22:12:23.472113+00:00"
fetched_at: "2026-08-19T22:12:27.719897+00:00"
content_hash: "sha256:4b1f18e980cd5a9ade6f2517b67b251834d9305390a7a32b4b629d6f41f28252"
---

# Reliability Engineering in the AI Era

Engineering leaders have been claiming to “shift quality left” for years but production remains stubbornly stuck out of reach of software engineers. The realm of production remains mysterious with tools no one has access to and UIs that wouldn’t make sense to engineers anyway.


I’ve noticed a small but growing trend of large enterprises hiring Reliability Engineers instead of Site Reliability Engineers. Dropping one word looks cosmetic but I think it points to a much bigger change. We’ve dispensed with proper testing and so reliability issues have shifted into production. Unfortunately, the velocity with which AI agents generate bugs requires a re-think of this process.


🎯 Key Takeaways


- AI agents make implementation skills more portable, but they make operational context more valuable.
- Software development will split into vendor, multi-harness, on-premises, and bespoke factory patterns.
- Reliability needs one feedback loop connecting code, tests, telemetry, incidents, and customer behavior.


## The word that disappeared


Obviously the entire industry is not renaming SREs but I am noticing a trend towards this job title and it fits within the broader trend of software engineering becoming *software manufacturing* . Job titles are messy, and “Reliability Engineer” also means something very different in manufacturing.


Before calling this a new idea, it is worth going back to Google’s original 2016 book,[Site Reliability Engineering: How Google Runs Production Systems (PDF)](https://netman.aiops.org/~peidan/ANM2023/0.AIOpsBooks/OReilly.Site.Reliability.Engineering.2016.3.pdf) . Its contents cover release engineering, testing for reliability, software engineering in SRE, and reliable product launches. The original remit already crossed the production boundary. The shorter title may be restoring some of the breadth that the practice lost along the way.


But current software roles show a real pattern. Robinhood has advertised for a[Software Engineer focused on Reliability](https://job-boards.greenhouse.io/robinhood/jobs/6511960) , including pre-production, integration, and load testing in the remit. OpsMill describes a[Product Reliability Engineer](https://jobs.ashbyhq.com/opsmill/b4af4790-46fd-430d-96bf-d45c64fc8bc1) who turns customer incidents into regression tests, product changes, and safer defaults. PointOne calls its equivalent role[full-stack engineering at the intersection of product, infrastructure, and customer impact](https://jobs.ashbyhq.com/PointOne/af882748-1b47-47c7-8ef2-98d14bc36fb2/) .


These are not renamed operations jobs. They connect what happens in production to what gets designed, built, and tested next.


The missing word is the interesting part. “Site” suggests a place where reliability work happens. Modern reliability is a property of the entire delivery system.


## Shift left shifted blame


For years, engineering leaders told teams to shift left. The entire goal was to find defects earlier when they are cheaper to fix. In many businesses, “test in prod” is a euphemism for “my customers are my testers.”


The implementation was often less impressive. We moved a test stage earlier in the pipeline. We added a security scanner. We gave developers another checklist. Then we called the transformation complete but AI is pointing out that this was not true.


The problem was a lack of context and the wide gulf between the skills that make an engineer good at building features and the skills that help debug production. Any senior engineer knows that debugging complex systems is a discipline all its own.


QA knew how the product was supposed to behave but almost always lacked realistic production conditions. Developers knew the code but not every way customers used it. SREs had the telemetry and incident history, but usually received the change after it had crossed the release boundary. Every role had a different context.


The organizational pressure has not disappeared and in fact it keeps increasing. In healthcare, PractiTest’s[2026 State of Testing report](https://www.practitest.com/state-of-testing/) says 65.4% of teams reported increased workloads, while budget growth was just 11.5% and team downsizing reached 53.8%. “Everyone owns quality” sounds empowering until it becomes “nobody has the time.”


## AI flattens the wrong gap


AI coding agents change this division of labor, but not because they make expertise irrelevant.


In three field experiments covering 4,867 developers,[Microsoft Research measured a 26.08% increase in completed tasks](https://www.microsoft.com/en-us/research/publication/the-effects-of-generative-ai-on-high-skilled-work-evidence-from-three-field-experiments-with-software-developers/) among developers with access to a coding assistant. Less-experienced developers saw greater adoption and productivity gains.


That compresses part of the skills gap. An engineer does not need ten years of shell scripting to investigate logs, write a load-test harness, or modify a deployment manifest. Of course the best software engineers still distinguish themselves from AI tools and the same is true of SREs. But for most folks, the agent can handle much of the implementation.


But implementation is not judgment and judgment requires context and experience. Anthropic analyzed roughly 400,000 Claude Code sessions and found that[people made about 70% of planning decisions while Claude made about 80% of execution decisions](https://www.anthropic.com/research/claude-code-expertise) . Users with domain expertise succeeded more often, recovered from errors more effectively, and got more useful work from each instruction.


AI makes coding knowledge more portable. It does not make production knowledge optional. In fact, it rewards the person who can explain what normal looks like, which failure matters, and how to prove a change is safe.


That is reliability engineering. It embeds quality into every stage of the software development cycle by breaking down organizational and knowledge barriers.


## Production belongs in the prompt


A coding agent limited to a repository sees an incomplete system. It can read the handler but not the malformed payload that breaks it. It can edit a timeout but not see the latency distribution that made the timeout dangerous. It can generate a test without knowing which customer workflow deserves protection.


Give the agent production context and the work changes. Logs help it investigate. Traces reveal dependencies. SLOs define acceptable behavior. Incident history exposes known failure modes. Real traffic supplies the inputs developers forgot to invent.


[Control theory](https://en.wikipedia.org/wiki/Control_theory) gives us a useful model for what comes next. Its aim is to use feedback to “drive the system to a desired state”: measure the output, compare it with a reference, and use the difference to change the system. In software, observability gives us the sensor. Logs, metrics, traces, and traffic show us what happened. But a sensor is not a controller. And Observability is not hypothesis testing.


```text
flowchart LR
P[Production behavior] --> O[Observe signals]
O --> C[Compare to goal]
C --> D{Within bounds?}
D -- Yes --> P
D -- No --> A[Act or approve]
A --> V[Verify change]
V --> P


```


That is the intermediate step where much of the industry has been stuck. We observe a deviation, page a human, and open a ticket. Modern reliability needs to close the loop by comparing production behavior with a set point—an SLO, a correctness rule, or a customer outcome—and feeding the difference back into regression tests, traffic replay, release criteria, rollback decisions, and agent context. The action can be automated or human-approved; the important part is that the evidence travels back into engineering. Observability got us from blind to measured. Reliability engineering must get us from measured to controlled.


The industry already collects much of this information. Capgemini’s[World Quality Report 2025–26](https://www.capgemini.com/ch-en/insights/research-library/world-quality-report-2025-26/) found that 94% of organizations review production data. Nearly half struggle to turn those observations into quality improvements.


That is not an observability problem. It is a feedback-loop problem.


## Generation outran verification


The quality system matters more because code generation is no longer the limiting step.


DORA found that a 25% increase in AI adoption was associated with a 3.4% improvement in perceived code quality, yet a[7.2% reduction in delivery stability](https://dora.dev/research/ai/gen-ai-report/dora-impact-of-generative-ai-in-software-development.pdf) . Faster reviews and better-looking code did not automatically produce safer releases. DORA’s likely explanation was larger changes moving through systems that were not built to verify them.


Recent outages make the warning concrete. GitHub reported[10 incidents in April 2026](https://github.blog/news-insights/company-news/github-availability-report-april-2026/) and[nine more in May](https://github.blog/news-insights/company-news/github-availability-report-may-2026/) . GitHub also said traffic was growing rapidly, driven partly by AI-assisted and agentic workflows. The direct causes varied: schema migrations, shared databases, DNS, rate limiting, and capacity saturation.


AI did not cause every outage. It increased the activity flowing through systems whose failure modes were already present.


AWS’s October 2025 disruption offers the same lesson without an AI connection. A[latent race condition in automated DynamoDB DNS management](https://aws.amazon.com/message/101925/) cascaded into EC2, load balancing, Lambda, containers, authentication, and other services. Automation increased scale. It also increased the blast radius of an assumption that turned out to be wrong.


We cannot build a software factory on top of a quality process designed for artisanal code volume.


## The factories are coming


“Software factory” is quickly becoming more than a metaphor. The coding-agent vendors are moving up the stack from generating code to managing the environment around it: planning, execution, permissions, review, deployment, and monitoring.


Anthropic does not sell something literally named the Claude Software Factory. But look at the direction of travel. Claude Code is no longer just a completion engine. It can inspect repositories, run tools, execute tests, coordinate work, and operate software. Anthropic now documents[separate containment architectures for Claude Code, Claude.ai, and Cowork](https://www.anthropic.com/engineering/how-we-contain-claude) because capable agents need meaningful access to real systems.


My read is that model providers would prefer to give companies a button that says “ship,” not a box of agent parts they must integrate themselves. The better the integrated environment, the more context the agent accumulates and the more useful it becomes. That creates a strong product advantage. It also creates lock-in.


The result will not be one universal factory. We are likely to see several patterns:


- Vendor factories that combine a model, agent harness, execution environment, and deployment workflow.
- Multi-harness factories that can choose different agents and models for different jobs.
- Bespoke factories built around a company’s existing CI, platform, security, and incident systems.
- Packaged private factories that include the hardware required to run local models.


```text
flowchart TD
E[Production context] --> V[Vendor factory]
E --> M[Multi-harness factory]
E --> B[Bespoke factory]
E --> P[Private factory]
V --> R[Reliability gate]
M --> R
B --> R
P --> R


```


Warp represents the multi-harness version. Its definition of a cloud software factory covers the loop from[triage through specification, implementation, review, verification, shipping, and monitoring](https://www.warp.dev/blog/software-factory-build-guide) . Warp’s Oz control plane can run[Claude Code, Codex, and Warp Agent under shared governance and audit controls](https://www.warp.dev/blog/multi-harness-cloud-agent-orchestration) . TechCrunch described its latest incarnation as an[out-of-the-box software factory for AI development](https://techcrunch.com/2026/08/18/warps-new-system-is-an-out-of-the-box-software-factory-for-ai-development/) .


AMD, Supermicro, and Spectro Cloud take the idea further. Their[AMD Instinct Coder](https://newsroom.amd.com/news/amd-supermicro-and-spectro-cloud-launch-turnkey-solution-to-scale-enterprise-ai-coding/) packages EPYC processors, eight Instinct GPUs, Pensando networking, a Supermicro server, Spectro Cloud orchestration, and local-first model routing into one enterprise AI coding system. The factory is not merely an agent workflow. It arrives with a bill of materials.


This feels like the early web. Every company once assembled its own hosting, build scripts, deployment process, CDN, and rollback machinery. Platforms such as Netlify eventually standardized much of that path. Software factories will probably follow a similar arc: experimentation first, recognizable patterns next, packaged platforms after that.


But standardizing the factory does not standardize the product being built. A vendor can package an agent, a runner, and a deployment button. It cannot automatically know your customers, failure history, dependencies, risk tolerance, or definition of correct behavior.


A factory that generates the code, generates the test, and grades its own work is a closed loop in the worst sense. Reliability engineering must provide the outside evidence.


## Close the reliability loop


The Reliability Engineer should not become the person who owns every bug. That would recreate the QA silo with a better title and a pager.


The role should own the feedback system across whichever factory pattern the company adopts:


- Production failures become reproducible tests.
- Customer traffic becomes pre-production validation data.
- SLOs become release criteria, not dashboard decoration.
- Incident reviews update agent instructions, tests, and guardrails.
- Agents receive scoped access to the context required for the task, with audit trails and least-privilege controls.
- Quality engineers, developers, and operators share evidence instead of handing off tickets.


```text
flowchart LR
O[Observe] --> R[Reproduce]
R --> C[Change]
C --> V[Verify]
V --> D[Deploy]
D --> O


```


Traffic replay is one practical example.[Speedscale’s AI code verification workflow](https://speedscale.com/features/ai-code-verification/) can[capture real inbound and outbound application traffic](https://docs.speedscale.com/concepts/capture/) and[replay a production snapshot against a local development environment](https://docs.speedscale.com/concepts/replay/) . That turns a production observation into a deterministic pre-production check. The same evidence can help a human reproduce a failure or help an agent verify its proposed fix.


The product is less important than the pattern: observe, reproduce, change, verify, and feed the result back into the system.


## Quality is an engineering system


Dropping “Site” from SRE will not fix software quality. Neither will renaming QA, buying an agent, or adding another gate to CI.


But the title change captures the stance we need. Reliability is not a production specialty. It is the discipline that connects production truth to engineering decisions before and after release.


As software factories become products, that discipline matters even more. Factories standardize how changes move. Reliability determines whether those changes deserve to move at all.


We spent a decade trying to shift quality left. AI gives us a better option: remove the boundary.


Drop the site. Keep the accountability.
