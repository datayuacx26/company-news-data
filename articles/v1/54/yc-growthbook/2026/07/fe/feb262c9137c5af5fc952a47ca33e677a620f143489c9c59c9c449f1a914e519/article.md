---
schema_version: "1.0.0"
document_id: "feb262c9137c5af5fc952a47ca33e677a620f143489c9c59c9c449f1a914e519"
company_key: "yc-growthbook"
company: "GrowthBook"
source_id: "yc-growthbook-news-import-67cec260d3a3"
canonical_url: "https://www.growthbook.io/blog/how-fintech-teams-use-feature-flags-to-deploy-safely"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-07-28T16:26:38.638236+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:19963eb4d9af9cb5d0733937eede75e974312ac377562218aff2636db87bcd03"
---

# How fintech teams use feature flags to deploy safely

In January 2026, DownDetector reported[roughly 4,000 user reports](https://www.bbc.com/news/articles/cdexzl9jg9no) about Monzo. Its mobile banking app stopped working for 2 hours, leaving thousands of customers unable to log in or make payments. For a fintech company serving 14 million customers, even a brief outage could put years of trust at risk.


When you’re running digital infrastructure for financial services, you can’t afford to continue using legacy processes.


Every software deployment you do carries a unit of risk with it, especially if you’re using all-or-nothing deployments. If something goes wrong, it can trigger a domino effect leading to significant financial consequences for you *and* your customers.


That’s why[fintech](https://www.growthbook.io/industry/fintech) organizations have started using[feature flags](https://www.growthbook.io/products/feature-flags) to have more control over the process.


In this guide, we’ll explain why fintech organizations need feature flags and how they can consider using them.


## Why fintech companies need to adopt feature flags for deployment


Here are a few reasons why fintech companies need feature flags:


### Every failed deployment is a financial event


Every time your[engineering team](https://www.growthbook.io/roles/engineering) deploys a new feature *without* proper testing and observability, you’re increasing the surface area of risk. That’s because a problematic feature in your app prevents customers from accessing their funds or using them for other purposes.


Even one incident could result in heavy regulatory scrutiny. Cross-border payments fail at an[average rate of 11%](https://www.pymnts.com/wp-content/uploads/2024/02/PYMNTS-Cross-Border-Sales-and-the-Challenge-of-Failed-Payments-February-2024.pdf) . And 82% of merchants don’t even know why payments fail in the first place.


That’s not a risk you should take when your customers trust you to know what’s happening under the hood.


### Compliance is the basis of everything you do


Because of the nature of the work you’re doing, you’re also answerable to regulatory bodies across borders. You’re essentially operating in an environment that very few organizations do.


For instance, the EU’s[Digital Operational Resilience Act (DORA)](https://www.eiopa.europa.eu/digital-operational-resilience-act-dora_en) took effect in January 2025. So now you’re required *by law* to ensure your digital infrastructure can recover quickly when something goes wrong. That’s why banks like Monzo had a failsafe called “Monzo Stand-In” that took over when it experienced an outage in January 2026.


Feature flags are another way to make sure you can automate the fallback path.


### Deployment safety requires data, but you can’t use it


Typically, to enable safe deployments, you need a continuous feedback loop. You ship a change and see how real users interact with it. The rollout behavior changes based on the metrics you choose to observe.


But in fintech, those signals are regulated financial data. You can’t export it without having the proper privacy protections in place.


Feature flags can bring rollout control in the same trusted environment as the financial data, as long as you’re using a compliant platform.


## How are fintech teams using feature flags to ship safely?


The most basic use case and benefit of feature flags is decoupling deployment from release. It reduces the risk of deploying code, but also gives you more flexibility to track and manage new features without rushing to deploy everything at once.


Here are a few other use cases of feature flags for fintech companies:


### 1. Progressive rollouts for payment and onboarding features


Let’s say you’re rolling out a redesigned checkout flow. If you push it to 100% of users on day one and the conversion rate drops, you’ve just broken the revenue path for your entire user base. That’s the problem progressive rollouts fix.


Here, you start small by exposing the new flow to 1% of traffic and defining what “safe to expand” looks like before you launch. It could be that the conversion rate improves or holds steady, or that the error rates stay flat. You only move on when those conditions are met.


GrowthBook’s[Monitored Ramp-Up](https://www.growthbook.io/blog/growthbook-4-4) automates the progression. You configure each stage with a target percentage and time window, and the platform moves traffic forward on a schedule. A fintech rollout might look like:


- Internal QA team first
- 5% of free-tier users
- 25% of free-tier users
- Approval gate before paid users enter the mix
- 25% paid to full rollout


### 2. Automated rollbacks with guardrail metrics


Progressive rollouts limit your blast radius. But they still assume someone on your team is keeping an eye on things. That’s not the reality your engineers deal with.


GrowthBook’s[Safe Rollouts](https://docs.growthbook.io/features/safe-rollouts) remove that dependency. You define guardrail metrics at flag setup time, such as payment conversion rate or transaction error rate, and the platform continuously monitors them as traffic ramps up.


If any guardrail metric shows a statistically significant regression, the platform automatically reverts the rollout.


[Source](https://docs.growthbook.io/features/rules#choosing-a-rule-type)


### 3. Compliance gating and jurisdiction-based targeting


A World Economic Forum report found that[60% of fintech companies](https://reports.weforum.org/docs/WEF_Future_of_Global_Fintech_Second_Edition_2025.pdf) operate in multiple jurisdictions, while 31% operate in multiple regions. This means that if you’re releasing a new feature or an update, you need the ability to control where it’s released first.


For example, if you’re releasing a new lending feature and it’s approved in the US but not in Singapore, you have three choices:


1. Maintain separate code branches per region
2. Delay the entire launch till approval
3. Release in one region through granular targeting


GrowthBook’s[Forced Value rule](https://docs.growthbook.io/features/rules) lets you implement option 3 with feature flags. You can define the segment as US-only, deploy the feature to all environments, and make it live only for those accessing it from the US.


When you get the approval for Singapore, just update the[targeting condition](https://docs.growthbook.io/features/targeting) .


[Source](https://docs.growthbook.io/features/rules)


### 4. AI model rollouts for fraud detection and credit underwriting


World Economic Forum’s report also found that[80% of fintechs](https://reports.weforum.org/docs/WEF_Future_of_Global_Fintech_Second_Edition_2025.pdf) are now implementing AI across multiple business lines. They’re either adopting it for software development or[shipping AI features](https://www.growthbook.io/blog/ship-fast-with-safety-net-feature-flag-management-agentic-era) .


Let’s say you’re releasing a new fraud detection model. If the model misfires, it might give you too many false positives and reject legitimate ones for no reason.


Instead of rolling out a bug to your entire user base, just wrap it in a feature flag. Roll it out to 5% of traffic and monitor your guardrail metrics to decide whether to scale up or reverse the rollout.


**Note:** Within GrowthBook, you can even experiment[on prompts and AI model versions](https://www.growthbook.io/platform/ai-native-development) to see what works best. And all of this works with feature flags being the main delivery mechanism.


[Source](https://www.growthbook.io/industry/ai-software)


## How feature flags satisfy fintech compliance requirements


Here are some of the features your feature flagging platform needs to help you stay compliant with fintech regulations:


### Audit trails and revision history


Every time you go through an audit, your auditor might ask you to reconstruct every change to your payment processing flag over the last six months. Things like who enabled it, when, what the previous state was, and who approved it.


You should be able to pull that up in a minute. In fact, if you’re going through[SOC 2 Type II audits](https://www.growthbook.io/platform/security) , this is a baseline requirement.


GrowthBook’s[audit logs](https://docs.growthbook.io/account/audit-logs) capture every flag event, with timestamped records available across all plans. You can pull up the version history for any flag and compare revisions side by side to see exactly what changed.


[Source](https://www.growthbook.io/blog/ship-fast-with-safety-net-feature-flag-management-agentic-era#flag-revisions-approvals-audit-logs-maintain-control-and-visibility)


### Approval workflows and the four-eyes principle


You don’t want to be in a situation where an engineer pushes a change that hits production immediately. It could result in smaller bugs or full-blown incidents. That’s one of the reasons the “four-eyes principle” is a baseline expectation these days.


Essentially, one person creates the flag with specific rules, and another person approves it before it goes live.


In GrowthBook,[approval workflows](https://docs.growthbook.io/features/publishing-and-approval-flows) serve as configurable gates. You can draft the change and submit it for review. Your peer or manager can approve it before it goes live. Irrespective of whether you’re creating a kill switch or a saved group, you can require approval for everything.


### Stale flag management


As you create more flags, you’re also creating more technical debt. Because feature flags are typically temporary, once a feature ships, engineering teams forget to remove them. The problem is that it’s still active in your codebase and could cause incidents later.


Make sure your feature flagging platform can detect these stale flags. For instance, GrowthBook automatically marks[feature flags as stale](https://docs.growthbook.io/features/stale-detection) if:


- They haven’t been active for two weeks.
- They aren’t in any active environments (the flag is disabled everywhere).
- Have one-sided rules that send 100% of traffic to a single variation.


You can also use its[MCP server](https://www.growthbook.io/platform/mcp-server) to find feature flags and pinpoint where they exist (via[Code References](https://docs.growthbook.io/features/code-references) ). And then use the cleanup skill to remove them.


### Keeping financial data in your infrastructure


Most feature flag vendors need your data on their servers to evaluate experiment metrics. Very few of them offer self-hosted deployments or a warehouse-native architecture. But in the fintech industry, that could result in heavy penalties *and* reputational risk.


So choose a platform that:


- Offers a[warehouse-native architecture](https://www.growthbook.io/platform/warehouse-native) that queries existing data warehouses for anything.
- Offers[self-hosted deployment](https://docs.growthbook.io/self-host) with air-gapped installations.
- Has[compliance](https://www.growthbook.io/platform/security) certifications such as SOC 2 Type II, GDPR, COPPA, or CCPA.


## How Upstart cut experimentation time from days to hours with GrowthBook


[Upstart](https://www.growthbook.io/customers/upstart) is an AI-powered lending marketplace that connects consumers with over 100 banks and credit unions. Its engineering team runs experiments against real credit models and transaction data.


Before GrowthBook, Upstart’s setup was fragmented. They ran a third-party tool for feature flags alongside two separate in-house experimentation platforms. As a result, engineers kept switching between tools, and analysis depended on manual work.


Since it was a financial services company, they couldn’t let sensitive financial data leave their infrastructure. That’s where GrowthBook’s self-hosted deployment model solved the compliance problem, as all experimental data remained within its infrastructure.


Upstart’s team consolidated three platforms into one, GrowthBook, and cut experimentation time from days to hours. But they were also able to clean up legacy feature flags, which also helped them reduce technical debt in the long run.


> *“We had to rally our teams for this project and get our old tools cleaned up, but the process is significantly improving the quality of our systems and reinforcing best practices around experiments and feature-flagging hygiene. GrowthBook allowed us to uplevel our code, speed up decision-making, and focus on what we do best—building a world-class AI lending marketplace.” ‍* — Diego Accame, Director of Engineering, Growth, Upstart


### What your feature flag platform needs to handle in fintech


If you’re actively evaluating feature flagging tools, these are the questions you should ask before choosing one:


**Criteria** **What to ask a feature flagging vendor**


Automated guardrail rollback Can the platform automatically revert a rollout when a metric degrades, without human intervention?


Approval workflows Can you require approvals for production while letting staging flow freely? Does the gate cover all flag change types?


Audit trail coverage Can you reconstruct the state of any flag at any point in time? Is the audit log gated behind enterprise pricing?


Warehouse-native data Does your financial data stay in your infrastructure, or does it flow through the vendor's servers for metric evaluation?


Self-hosted deployment Can you run the platform on your own infrastructure, including air-gapped environments?


Stale flag detection Does the platform automatically surface flags that have outlived their purpose? Can you action them programmatically?


Open-source transparency Can your security team inspect the source code before procurement signs off?


AI agent governance Do agent-triggered flag changes go through the same approval gate as human ones?


Local SDK evaluation Do flag decisions happen locally in the SDK, or do they require a network call to the vendor's servers?


Kill switch speed Can you disable a feature in seconds via a toggle, or does rollback require a full deployment?


**Note:** GrowthBook does all of this and more. Learn more about[how we help fintech organizations](https://www.growthbook.io/industry/fintech) .


## Use feature flags to ship faster with confidence without risking your reputation


Monzo’s two-hour outage reminded the industry what fintech teams already know: when you’re handling other people’s money, there’s no such thing as a minor production incident. And at least in the bank’s case, they had a fallback path in place.


But the reality is that most fintech organizations don’t. This is why you should consider adopting feature flags. They give you the infrastructure to ship fast without taking on that risk. Whether you want to test in production or create fallback paths, you can do so without building excessive tooling.


If you’d like to see how feature flags can improve your deployment process,[try GrowthBook for free](https://www.growthbook.io/get-started) or[book a demo](https://www.growthbook.io/demo) .


‍
