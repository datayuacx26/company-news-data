---
schema_version: "1.0.0"
document_id: "3f80ad9a34513dc4f16e96babcb2c0f22092a1ce29cc8664749b6c58df713d9f"
company_key: "yc-skyhook"
company: "Skyhook"
source_id: "yc-skyhook-news-import-0a5e972b70e5"
canonical_url: "https://www.skyhook.io/blog/best-of-breed-tools"
published_at: "2026-04-01T00:00:00+00:00"
first_seen_at: "2026-07-26T00:39:47.906327+00:00"
fetched_at: "2026-07-28T22:16:19.189952+00:00"
content_hash: "sha256:5dec96244285efa72d385900352fba7b91986975d51cf7736da9d7e2f83eadd6"
---

# Why Best-of-Breed Tooling Wins: The Case for Flexibility in Your Infrastructure Stack

The CNCF landscape has over 1,000 logos. If you've ever zoomed into that chart and felt overwhelmed, you're not alone. But that density isn't a problem to solve - it's a signal to read.


It means the cloud-native ecosystem has produced specialized, purpose-built tools for every layer of the stack: GitOps, observability, secrets management, service mesh, certificate handling, cost optimization. And in nearly every category, the specialized tool outperforms the "good enough" module bundled inside an all-in-one platform.


This is what "best-of-breed" means in practice. You pick ArgoCD for GitOps because it's the best at GitOps. You pick Prometheus for metrics because it's the best at metrics. You don't pick a platform that happens to do both, mediocrely.


Most teams already operate this way. The question is whether they're doing it intentionally - or just accumulating tools and paying the price.


## The All-in-One Promise


All-in-one platforms sell a compelling story: one vendor, one UI, one contract, one throat to choke when things break. For a team of five spinning up their first cluster, that simplicity is genuinely valuable.


The problem shows up later.


**Feature depth plateaus.** An all-in-one platform spreads engineering effort across dozens of capabilities. The GitOps module is "fine." The monitoring is "adequate." But none of it matches what a dedicated team building a single-purpose tool can deliver. You end up working around limitations instead of leveraging strengths.


**Pricing becomes a trap.** Some platforms use "success penalty" pricing - the more you grow, the more you pay, often in ways that aren't obvious upfront. self-managed Red Hat OpenShift, for example, ties licensing to the underlying hardware (subscriptions are sold per core-pair or per bare-metal node). Upgrade to higher-core nodes and your bill jumps, even if your workload hasn't changed.


**Lock-in compounds over time.** Moving away from an opinionated all-in-one platform often means re-architecting networking, security policies, and deployment workflows from scratch. The longer you stay, the harder it gets to leave. This isn't a theoretical risk - vendor lock-in is one of the most cited concerns in cloud strategy surveys year after year.


**The AWS CI/CD example.** Many teams adopt CodePipeline, CodeBuild, and CodeDeploy because they're already on AWS - one ecosystem, one bill, tight integration. In practice, each tool is mediocre at its job compared to the specialized alternative. CodeBuild is slower and less flexible than GitHub Actions. CodeDeploy lacks the drift detection and reconciliation of ArgoCD. CodePipeline's UI makes debugging a chore. Teams end up migrating to GitHub Actions + ArgoCD anyway - but now they've burned months building on AWS's all-in-one suite first.


## Why Best-of-Breed Won


This isn't a theoretical debate. The market already decided.


**The numbers:** The CNCF hosts 200+ projects with hundreds of thousands of contributors worldwide. And the pull is accelerating: the[CNCF and SlashData State of Cloud Native Development report](https://www.cncf.io/announcements/2026/03/24/cncf-and-slashdata-report-finds-cloud-native-community-reaches-nearly-20-million-developers/) put the cloud-native developer community at 19.9 million in Q1 2026 - up from 15.6 million two quarters earlier, a 28% jump in six months, and now 39% of all developers worldwide.


**The ecosystem is specialized by design.** Kubernetes itself was built as an extensible platform, not a monolith. Its power comes from CRDs and the operator pattern - standardized APIs that let specialized tools plug in. cert-manager handles TLS. External Secrets Operator handles secrets. Argo Rollouts handles progressive delivery. Each one does its job better than any platform's built-in equivalent.


**Multi-cloud is the default.** Flexera's[State of the Cloud report](https://www.flexera.com/blog/finops/the-latest-cloud-computing-trends-flexera-2025-state-of-the-cloud-report/) has multi-cloud adoption near 89%, with organizations using an average of 2.4 public cloud providers. When your infrastructure spans AWS, GCP, and Azure, an all-in-one tool that only works well on one provider becomes a liability, not a convenience.


**Best-of-breed gives you leverage.** When you're not locked into a single vendor, you can negotiate. You can switch. You can adopt a better tool when one emerges without re-platforming your entire stack. That optionality has real economic value - especially for teams that plan to be around for a while.


## The Integration Tax Is Real


Here's where best-of-breed advocates usually lose credibility: they pretend integration is free. It isn't.


Engineers routinely juggle a handful of separate tools to build and ship a service, and the context-switching between them is not free - it's hours a week per engineer that never reach a product backlog. Across a team, that adds up to real money.


This is often called the "DevOps tax" - the cost of integrating, maintaining, and troubleshooting a multi-tool pipeline, measured in engineering hours that could have gone toward shipping features. GitLab's[DevSecOps survey](https://about.gitlab.com/blog/2022/08/24/too-many-toolchains-a-devops-platform-migration-is-the-answer/) has tracked it climbing: by 2022, nearly 40% of developers said they spent a quarter to half of their time just maintaining and integrating their toolchain, and 69% wanted to consolidate.


The failure mode looks like this: your CI/CD pipeline works, but the handoff to your GitOps tool requires a custom webhook. Your monitoring catches issues, but alerting lives in a different system with its own configuration language. Your secrets management is solid, but rotating credentials requires touching three different tools. Each integration point is a potential break point.


This is the honest tradeoff. Best-of-breed gives you the best tool for each job. It also gives you the job of making them work together.


## The Third Option: Best-of-Breed with a Platform Layer


The choice isn't "all-in-one platform" versus "duct tape and scripts." There's a third option that's become the dominant pattern among teams that have figured this out.


Team Topologies authors Matthew Skelton and Manuel Pais call this the **"thinnest viable platform"** - the smallest set of APIs, documentation, and tooling needed to let teams move fast without rebuilding the same integration work over and over.


In practice, this means a platform layer that **orchestrates** best-of-breed tools rather than **replacing** them.


Spotify built Backstage for exactly this reason. Their engineers were using dozens of specialized tools but had no unified way to discover, configure, and operate them. Backstage doesn't replace those tools - it provides a portal layer on top of them. It's now a CNCF incubating project used by thousands of companies, with a large community plugin ecosystem.


The architecture that's emerging across the industry looks like this:


Layer Role Examples


**Developer Portal** Discovery, self-service, catalog Backstage, Port


**Platform Orchestrator** Glue, automation, golden paths Humanitec, Kratix


**Best-of-Breed Tools** Specialized capabilities ArgoCD, Prometheus, Terraform, cert-manager


**Infrastructure** Compute, networking, storage AWS, GCP, Azure, on-prem


The platform layer doesn't own your tools. It connects them. Your ArgoCD config stays in your repo. Your Prometheus rules stay where they are. The platform layer handles the integration work so your engineers don't have to.


## What to Look for in a Platform Layer


Not all platforms are created equal. Some claim to "orchestrate" best-of-breed tools while quietly introducing their own lock-in. Here's what matters:


**Your configuration lives in your repos.** If the platform stores your deployment configs, Helm values, or Kustomize overlays in its own proprietary format or database, you've traded one form of lock-in for another. Everything should be standard YAML/HCL in Git repos you own.


**You can swap any tool.** If you decide to move from Prometheus to Grafana Mimir, or from nginx-ingress to Gateway API, the platform should make that easier - not harder. The test: can you replace any single tool in your stack without re-platforming?


**It works across clouds.** AWS today, GCP tomorrow, hybrid next year. A platform layer that only works on one cloud is just lock-in wearing a different hat.


**It provides opinionated defaults, not mandates.** The[80-20 rule](https://www.skyhook.io/blog/golden-paths-and-the-80-20-rule) applies here. Golden paths should cover 80% of use cases. For the other 20%, you need the freedom to deviate. Paths, not cages.


**You can leave.** The ultimate test. If you rip out the platform layer, you should be left with standard Kubernetes manifests, standard Helm charts, and standard CI/CD pipelines that still work. The platform made your life easier - but it didn't make itself a dependency.


This is the philosophy we built[Skyhook](https://www.skyhook.io/) on - and the reason it isn't a row in that table. Assembling those layers yourself - a portal, an orchestrator, the best-of-breed tools, and the seams between them - is a platform team's full-time job. Skyhook collapses the stack: we orchestrate best-of-breed tools (ArgoCD, Prometheus, cert-manager, External Secrets, and dozens more) without replacing them, wire them together, and put a self-service surface on top - so a small team gets this whole architecture without staffing a platform team to build it. And it holds itself to every test above: your config lives in your Git repos as standard YAML, you can swap or modify any tool directly, and if you leave, you take everything with you.


## Making the Call


If you're evaluating your infrastructure tooling strategy, here's a practical framework:


**Go best-of-breed with a platform layer if:**


- You want to use the best tool for each job without the integration headache
- You run multi-cloud or plan to
- You want to adopt new tools as the ecosystem evolves without re-platforming
- You care about avoiding vendor lock-in from day one


The good news: you don't need a large platform team to make this work. A platform layer handles the orchestration so even small teams get the flexibility of best-of-breed without the overhead of stitching it all together themselves.


**Audit what you already have.** Most teams are already running best-of-breed tools - they just haven't invested in the platform layer to make it manageable. The integration tax is a solvable problem. You don't need to consolidate into a monolith. You need better glue.


The CNCF landscape isn't going to shrink. The ecosystem will keep producing specialized tools that outperform bundled alternatives. The teams that thrive will be the ones who figured out how to use the best tool for each job - without drowning in the work of making them talk to each other.
