---
schema_version: "1.0.0"
document_id: "c804c7030d0aa1214ad2e90f616d6396ce6ae264d7988e57a4b0b7886241b318"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-news-import-c7466264c659"
canonical_url: "https://www.usetusk.ai/resources/ai-harnesses-are-not-wrappers"
published_at: "2026-02-03T23:28:28.103+00:00"
first_seen_at: "2026-07-26T03:20:25.565537+00:00"
fetched_at: "2026-07-28T22:22:17.354714+00:00"
content_hash: "sha256:28c3460a3a81468f9e2a29892165282c37b36c436988b806e204da5d96b5ef6b"
---

# AI Harnesses Are Not Wrappers (They're Not Blinders Too)

> You put a harness on a horse to steer it to where you want to go, like how AI harnesses guide a coding agent to the end objective of writing working code.
> ‍


## **Blinders don't help you ride**


Everyone's talking about guardrails today. Claude Code has[settings.json](https://code.claude.com/docs/en/settings) to restrict file access. NVIDIA has their[NeMo Guardrails](https://developer.nvidia.com/nemo-guardrails) for enterprise. We ourselves created[Fence](https://github.com/Use-Tusk/fence) as a container-free sandbox for terminal agents.


Guardrails are table stakes now. **They're also not enough** .


In the physical world, blinders are things you put on a horse to keep it from spooking. They restrict what it can see, which makes a horse with blinders on calmer. But a calmer horse doesn’t mean it’s going to go the right direction.


In this analogy, guardrails are blinders. They stop your agent from doing bad things, but they don’t necessarily help your agent do correct things. A harness is different. You put a harness on a horse to steer it to where you want to go, like how AI harnesses guide a coding agent to the end objective of writing working code.
‍ **‍**


## **A harness implies a rider**


There’s a meme going around about how we’re just replacing the term “wrapper” with “harness”.


> we shouldn't have allowed this to happen[pic.twitter.com/QJEkJMCvqJ](https://t.co/QJEkJMCvqJ)
>
>
> — AJ Stuyvenberg (@astuyve)[January 20, 2026](https://twitter.com/astuyve/status/2013648715589001530?ref_src=twsrc%5Etfw)


Here's what people are missing.


The harness implies a connection between the rider and horse. It’s not random clothing you put on a horse to make it look nicer. The harness exists because there's someone holding the reins with information of the route and destination.


Similarly, building a harness for AI agents implies providing both:


- **Specialized tools** for navigating the route
- **Proprietary data** that clarify the destination


Take the coding example of fixing a bug with Claude Code. The route here would be your ability to verify that code changes are correct. You’d do that by generating and **running unit/integration tests** . The proverbial horse can then iterate on its path according to test execution output.


Proprietary data, which serves as “rider knowledge”, is the part of the harness that helps the agent decide what the goal is. For coding agents, this is your **observability data** that provides context for root cause analysis. Now the agent knows what exactly it needs to fix, just like the horse knows exactly where it needs to go.


If harnesses were just wrappers, you’d take the junk off the horse and it would still take the same path to the same place. That’s the critique of AI wrapper startups; they’re just fancy leather straps.


This should be the dominant philosophy of any AI company building harnesses. For us at Tusk, we’ve invested heavily into the complete flow of capturing observability data, generating tests, executing them, and sending the context back for iteration.


*Observability data enabling AI-powered root cause analysis*
‍


## **Put the harness where the agent works**


The tech industry spent a decade moving everything to the cloud. CI/CD pipelines, remote testing, serverless functions. Harnesses reverse this.


If your agent works locally, the feedback loops need to be local too. A coding agent can't iterate effectively if it has to wait 20 minutes for CI to report back.


The latency between making a change and knowing if it works is now measured in seconds, which means moving the tests and verification to where the agent runs (hint: CLI). The cloud pipeline is still important, but becomes more of a sanity check.


The human’s role changes in this world. You're reviewing harness output more than you review the actual code. And that means checking whether your unit tests pass or approval tests are catching a regression.


The proverbial horse is now a muscle-bound racehorse. If only we had something to steer it.


‍
