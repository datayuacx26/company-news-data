---
schema_version: "1.0.0"
document_id: "3a038e8152fb89de0d9ac038506a7773c6a59e8dbf9f5b55c510931b0e403909"
company_key: "yc-infracost"
company: "Infracost"
source_id: "yc-infracost-news-import-5e947f59e679"
canonical_url: "https://www.infracost.io/resources/blog/shift-finops-into-the-agent-with-infracost-cruise"
published_at: "2026-05-28T14:37:00+00:00"
first_seen_at: "2026-07-23T23:57:22.274812+00:00"
fetched_at: "2026-07-28T21:46:32.935029+00:00"
content_hash: "sha256:02c6015a39ff5cd5a99ba33e3ce7a697600de633c855a0eab647b645058a573c"
---

# Shift FinOps into the Agent with Infracost Cruise

### Building a FinOps practice that makes sense for AI-generated infrastructure as code


AI has permeated how we do work — it’s not unheard of to spend much of our day within our LLM of choice. We live in an era of[“token maxxing”](https://en.wikipedia.org/wiki/Token_maxxing) , much to the horror of FinOps teams everywhere. Engineers are using coding agents to write infrastructure at a rapid clip, and in turn, additional cloud waste follows. Cloud waste was an issue *before* agents existed. How can FinOps teams keep up at this speed?


Rather than rely on AI-powered FinOps dashboards, what if we could use AI to help prevent and remediate cloud waste faster?


That’s the question we asked ourselves when we built[Infracost Cruise](https://www.infracost.io/product/cruise) , our first AI-native suite of tools. Infracost Cruise enables platform and FinOps teams to prevent, govern, and remediate cloud waste with agentic workflows that work across the infrastructure lifecycle. We’re not just shifting FinOps left — we’re shifting everywhere — in the coding agent, the IDEs, the CI/CD pipeline, as well as post-deploy.


## Integrating FinOps Across the Infrastructure Lifecycle


To be sure, this is a bold claim. We didn’t want to just ship an MCP server and call it a day. For agents to be effective, they need[better context into our FinOps policies](https://a16z.com/your-data-agents-need-context/) . They also need human-in-the-loop workflows that integrate with the tools that engineers, platform, and FinOps teams use everyday.


So that’s what we built. Cruise is an extension of the Infracost Platform. It includes:


1.


**A FinOps Context Engine** that not only includes your policies, but your custom price books and budgets too. This ensures that all teams work from the same system of record, agents are accurate with deterministic inputs, and that agents can sufficiently learn an grow based on inputs from your team.


2.


**Three agent toolkits** that work across the infrastructure lifecycle. So the teams that build, govern, and remediate cloud waste can use these agents within the workflows and tools they use everyday — in the coding agent, the CI/CD pipeline, and within remediation workflows.


###


### Beyond VSCode: Building Cost-Aware Infrastructure with Infracost Dev


For engineers, Infracost formerly shifted FinOps left into the local workflow with our[VS Code extension](https://www.infracost.io/resources/blog/jul-2022-update) . Rather than simply improve that, we took it a step further by integrating cloud cost awareness into *every* major IDE and coding agent. So if you lead a major platform team, take a sigh of relief. Your engineers will be happy to know that[their local workflow is supported](https://www.infracost.io/docs/#ide-extensions) . 🎉


The experience for engineers is pretty sweet. Rather than having to tell your coding agent to optimize for cost every single time and then validate that it didn’t hallucinate a SKU Azure doesn’t even have,[Infracost Dev](https://code.dev/) ensures your coding agent builds cost-aware infrastructure from the start. It validates this by providing real-time prices from the cloud providers’ APIs, and helps engineers make tradeoffs between cost and other requirements.


Infracost Dev also reduces tokens by 61% and improved accuracy by 55% over a baseline Claude configuration without it. See[this post](https://www.infracost.io/resources/blog/we-cut-claude-s-token-usage-79-by-redesigning-our-cli-for-agents) for more benchmarks.


##


## Defining the FinOps Golden Path Across Coding Agents and CI/CD Pipeline with Infracost Check


Software teams are using coding agents, and this barrage of code requires infrastructure teams to rely on them to use AI-assisted infrastructure as code as well. Aside from the PR review nightmare and drift concerns, bespoke Terraform configurations created by any number of models also introduce immense risk of FinOps violations and cloud waste. After all, LLM models have no standard for what “cheap/fast/good” looks like at *your company,* and they tend to hallucinate if they’re not given the right information upfront.


The "golden path" doesn't just shift into the IDE and the CI/CD pipeline. It now shifts into the coding agent itself.


***Shifting FinOps in the agent harness***


Rather than expecting engineers to use prompts that optimize for cost every single time, platform teams are now looking to shift guardrails and best practices into the agentic workflow. This practice, called[“harness engineering”](https://mitchellh.com/writing/my-ai-adoption-journey) by Terraform creator Mitchell Hashimoto, is the practice of promoting quality results from coding agents. The tech industry at large has discovered that **context is king** for delivering quality results. Not only should agents learn from our policies — our policies should learn from responses from agents and users too.


[Infracost Cloud](https://www.infracost.io/docs/infracost_cloud/get_started/) used to just lay out the FinOps golden path for CI/CD, and now it integrates those same policies and best practices into the local workflow — including the coding agent. This ensures they're constantly taken into consideration in every agent coding session for every developer in your organization. FinOps then shifts into the agent itself, including:


-


Tagging policies


-


Cost guardrails


-


FinOps best practices


-


Custom pricebooks


-


Budgets


This is called Infracost Check. Platform teams can integrate FinOps policies not only in the CI/CD pipeline, but directly into the IDE as well as the coding agent. One customer implemented this across their organization, and now sees 95% FinOps compliance before infrastructure ships.


Watch this demo where Glenn uses Claude to build infrastructure that is 100% compliant with our tagging policy *before* it is deployed.


##


## Diagnose and Action FinOps Issues Across your Entire Cloud Estate with Infracost Fix


“I really enjoy finding tagging issues and then haggling engineers to fix them.” — *no one ever*


DevOps teams that live and die by DORA metrics generally aren’t very enthusiastic about stopping everything to fix tagging issues or switch out instant types. The easiest way for FinOps teams to help them? Treat it like any other fix in the codebase — submit it as a pull request to be fixed. This works at a smaller scale, but across the IaC and even cloud estate? It just doesn't scale.


If you’ve ever used Infracost[Issue Explorer](https://www.infracost.io/docs/infracost_cloud/issue_explorer/) , you’ll recognize that Infracost is great at scanning your entire IaC estate and identifying specific FinOps violations within the code. Rather than relying on this dashboard and periodically opening pull requests to fix the issues yourself, you can now automate much of this workflow using agents. We call this capability Infracost Fix, and it’s currently in private beta. Fix provides continuous monitoring that triages findings, drafts fixes, and enables you to either open pull requests to fix issues or action them through your ticketing systems.


### How to get started with Infracost Cruise


If you’re an engineer who just wants to use Infracost Dev to build IaC without going broke — what are you waiting for? Just[download it](https://www.infracost.io/docs/) and get started.


If you’re a Platform, FinOps, or Engineering leader — reach out. We know all the FinOps vendors promise to save you money. But unlike a dashboard product, Infracost will actually prevent that spend in the first place.[Contact us](https://www.infracost.io/schedule-demo) and we’ll show you a demo of Infracost Cruise in action.
