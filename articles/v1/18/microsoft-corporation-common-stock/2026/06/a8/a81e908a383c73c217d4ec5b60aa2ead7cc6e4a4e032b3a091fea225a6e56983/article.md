---
schema_version: "1.0.0"
document_id: "a81e908a383c73c217d4ec5b60aa2ead7cc6e4a4e032b3a091fea225a6e56983"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-0d567709f64e"
canonical_url: "https://www.microsoft.com/en-us/power-platform/blog/2026/06/04/advanced-connector-policies-are-generally-available/"
published_at: "2026-06-04T15:00:00+00:00"
first_seen_at: "2026-07-20T04:34:28.280378+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:c34ebd7dd30c1756bf581dd4ea8e4aeca743dd92fd7168f21a84e746076ef9d0"
---

# Advanced connector policies are generally available

Every admin we meet wants the same two things: let their teams build and keep the business safe while they do it. For nearly a decade, **data loss prevention (DLP) policies** helped you hold that line — sorting connectors into business, non-business, and blocked so that sensitive data and the open internet never met inside the same app or flow. It worked well. But the world your makers operate in has changed.


Copilot, agents, and AI-first projects have multiplied both the people who build and the places they build in. A tenant that had a few dozen environments two years ago can have thousands today. And what you need to govern is no longer just which connectors — it’s which actions and MCP servers inside them that AI tools utilize. Today, we’re making **advanced connector policies (ACP)** generally available to meet that moment.


## Governance built for how people actually work today


The old model asked a lot from administrators. Every connector had to be sorted into a bucket, and a single environment could be touched by several overlapping DLP policies at once — a tenant-wide rule here, an exception there, even an environment-specific DLP policy a maker created themselves. Predicting what one small change would do often meant holding several rule scopes in your head and hoping to avoid a “scream test”, the DLP wizard was optimized for placing policies yet made it hard to identify the effective policy on a given asset. ACP replaces that guesswork with one simple idea: **every environment has at most one policy in effect** — inherited from an environment group or set directly on the environment. That’s the whole mental model.


## What changes with ACP


ACP is a ground-up redesign of how you manage what your apps, flows, and agents can use from a connector perspective. The headlines:


**• Govern what used to be non-blockable.** On managed environments and environment groups, you can block all connectors and actions. In classic DLP policies some connectors cannot be touched.


**• Goodbye business and non-business.** The old classifications are gone. There’s one clear question: is this connector or action allowed vs blocked.


**• Govern your AI tools.** Agents reach out to the world through MCP servers; ACP lets you block an MCP server just like any other connector or action.


**• An allowlist, not a sorting exercise.** You start from “nothing extra is allowed” and add the connectors your teams need. When a brand-new connector appears on the platform, it’s blocked until you decide — so nothing slips in just because it’s new.


**• Down to the individual action.** Allow a connector but switch off a risky action or an old, deprecated one. For the first time you can see which actions are deprecated, which are internal, and which are triggers — right where you set the policy.


## Where ACP shines: scalability


Massive volume of environments and assets that customers manage today in the age of AI are the reasons why ACP was built. With **personal developer environments (PDE)** and **environment routing** , a new maker creating their first app, agent, or flow can automatically get a dedicated environment created just for them. That’s great for maker productivity, but it made classic DLP’s include and exclude mechanics nearly impossible to keep current. Every new environment introduced another policy-scoping decision, another exception to track, and another chance for governance to drift.


ACP changes that model completely: because it is a native part of environment groups, the right connector policy follows the environment automatically. As soon as a new environment is created and routed to a group, the correct policy snaps into place — with zero friction for makers and no ongoing environment-by-environment overhead for IT.


## The shift to earlier feedback


ACP has enforced policy at runtime throughout public preview this past year. That means when an app, flow, or agent invokes a connector, the platform performs a last-mile check against the effective policy and blocks the action if it is not allowed. Runtime enforcement is essential because it protects the business at the exact moment data could move — but it also comes at the very end of the maker journey. A maker could build a new asset, wire up connectors and actions, and only discover at runtime that the experience could never successfully run because it violated policy.


With this GA release, we are shifting that feedback much earlier. Now, when a maker first adds a connector or action to an app, flow, or agent, ACP can tell them immediately whether that choice is allowed in the environment they are building in. Instead of waiting until the asset is complete — or worse, until it runs — makers get clear guidance while they are still designing. And soon, we will go one step further: blocked connectors and MCP servers will be greyed out up front, so makers can focus only on the tools that are available, compliant, and expected to succeed.


## What comes next


As we look ahead, we know there are still important capabilities in classic DLP that customers rely on today — especially custom connectors and endpoint filtering. Until those experiences fully land in ACP, customers can use ACP and DLP together in[mixed mode](https://aka.ms/LearnACP) , combining the strengths of both systems where they need to. That means using ACP for its simpler model, action-level control, and MCP governance, while DLP continues to cover the remaining scenarios that have not yet reached parity. We are also building a new feature called “ACP only mode” which is in public preview now and will be GA soon, allowing you to easily ignore DLP for an environment or group of environments where needed and reducing the need to continue to include or exclude environments from your DLP policies. This is the easiest way to onboard to ACP for customers who don’t need those extra capabilities as you can leverage environment groups, routing, ACP and ACP only mode to completely migrate away from DLP.


## Getting started


You can apply ACP two ways: define it once on an **environment group** to govern a whole fleet or set it directly on a **single environment** for the high-risk, pilot, or regulated ones that need their own rules. You’ll find it in the Power Platform admin center under **security > data and privacy** for a single environment, or on the **rules** tab of an environment group to manage at scale. DLP isn’t going anywhere overnight — you can run both side by side while you migrate, and switch to a single, clean ACP-only posture when you’re ready.


Before making connector policy changes, we also encourage customers to review Power Platform inventory, which now includes preview visibility into connector and operation usage across apps, flows, and agents. That foundation creates a path to impact analysis for ACP changes, helping admins understand ahead of time which resources, connectors, and actions could be affected before they publish a policy update.


Governance shouldn’t slow your teams down; it should give them a safe lane to move fast in. That’s what advanced connector policies are built for. Explore the documentation at **aka.ms/LearnACP** , try it in a single environment or group, and tell us what you think.


[Learn more](http://aka.ms/LearnACP)
