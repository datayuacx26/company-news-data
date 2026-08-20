---
schema_version: "1.0.0"
document_id: "624177df2f3cbfc74c6f09438f99e2742e3c3873466fa82099faf559c868071b"
company_key: "block-inc-class-a-common-stock"
company: "Block Inc."
source_id: "block-inc-class-a-common-stock-rss-613ed2351e85"
canonical_url: "http://engineering.block.xyz/blog/from-localhost-to-launched-safely-shipping-apps-that-anyone-can-build"
published_at: "2026-06-24T12:00:00+00:00"
first_seen_at: "2026-07-20T03:32:06.876214+00:00"
fetched_at: "2026-07-28T20:48:18.651537+00:00"
content_hash: "sha256:19614cc4bc11755f58755cd80e5ccdf9a79d40f30f59e4034acabb657397f19e"
---

# From Localhost to Launched: Safely Shipping Apps That Anyone Can Build

Building an internal tool used to be slow and expensive. It took at least one engineer writing at least one project spec, and a manager willing to put that project spec on the roadmap, followed by the actual time to build, test and deploy the tool. AI has effectively replaced (almost) all of these steps, and an idea can now become a working tool in an afternoon. But getting that tool safely into other people's hands has remained a challenge that AI alone has not been able to solve.


A sales representative, customer success manager, or data analyst can now describe a tool in plain language, watch an agent generate a working app, and have that app running on their laptop within the hour. At Block, this pattern first emerged in early 2025. Employees who had never written a line of code before were suddenly building software to unblock themselves.


But then a lot of apps just sat there, running on someone's laptop, with nowhere to go. The moment that building got cheap, a different problem moved to the forefront: there was nowhere to put these apps to share them.


This is the story of that second problem, and of Block App Kit, the platform we built to solve it.


## The bottleneck moved


A vibe-coded app isn't really a tool until other people can use it. And inside a company that handles sensitive financial data, "other people can use it" is a much higher bar than it looks. The obvious options were all a poor fit for internal tools that touch company data:


- A personal static-hosting site (think GitHub Pages) typically has no SSO in front of it, so company data can end up outside the security perimeter.
- A one-click deploy to a general-purpose hosting platform puts data outside of company access controls and IT's management surface.
- A local script that downloads remote code and prompts for SSO is, to any security team, hard to distinguish from a data exfiltration attempt.


The people closest to the problems that could benefit most from internal tooling (analysts, ops, finance, and support) could build a tool in an afternoon. What they still lacked was a sanctioned way to put it in someone else's hands. And sharing matters for more than convenience: it lets domain experts encode their knowledge into something others can use, and it keeps everyone working from consistent definitions of shared processes instead of a dozen private spreadsheets. Building was easy now; the bottleneck had moved from "can you build it?" to "is there a safe place for the right people to use it?"


And that second question isn't a model problem, and it isn't a harness problem either. A model can generate a great interface, but it can't guarantee its own safe use. It can't ensure the app is opened by the right people, against the right data. That's a platform problem.


## Separating the agent's job from the platform's job


At Block, this matters more than usual. The company is building itself to operate as intelligence. That requires capabilities that can compose with each other. An app on someone's laptop doesn't compose with anything.


The core idea behind Block App Kit is a clean split: the agent generates the app, and the platform owns everything that makes the app safe and durable.


Loading image...


Block App Kit is a deliberately thin, opinionated stack. The platform provides a managed frontend, compute, and persistence layer designed for rapid internal application development. The app management surface is intentionally tiny. And safety isn't bolted on, it's baked in. As is standard across the industry, authentication, authorization, secret management, and data-access controls are provided by the platform by default. The platform draws hard lines on what an app can connect to and who can open it, so “anyone can build” never quietly becomes “anyone can do anything.”


We chose to be opinionated about the stack rather than flexible on purpose. A more flexible system would let each builder pick their own framework, storage, and deployment target, and that flexibility would make consistent security properties impossible to guarantee. The constraints are what make those guarantees real. They also get the framework out of the way: people want to solve their problem, not research platforms, and removing those choices lets them do exactly that.


With the platform handling safety, the remaining job was to make building on it as easy as possible. This is where the agent comes in. The agent can drive the same few steps that an engineer would follow (scaffold, build, deploy) without the human builder needing to understand what's happening under the hood. We first built those instructions into an MCP server, backed by the CLI that an agent would use to scaffold, deploy, and manage an app. That worked, but it had to be installed and enabled in each agent setup, one person at a time. Fortunately, Block has an internal agent tooling platform that makes it easy to distribute skills to employees, so we repackaged the MCP as a skill and distributed it that way. The skill is the part that knows when to reach for Block App Kit and how to follow the safe path, as well as how to troubleshoot common issues, interact with common data sources correctly, and more. Further, the skill is always improving: once per week we audit our support channel and feed new learnings back into the skill.


## What was hard


The hard part was not writing code. Frontier models became great at one-shotting an app around December 2025. The hard parts were the unglamorous platform problems: identity, data, and secure connections to internal systems. We wanted to emphasize the controls in place to reduce mishandling of data.


For Block App Kit, security was a critical design partner from the start. They understood the threat models to consider and the behaviors to prevent, and guided the design so that those behaviors were structurally prohibited rather than merely discouraged.


Meanwhile, we had to assume that the builder would have no coding experience whatsoever and that their agent would be on the hook for producing a high-quality, accurate and secure app. The tempting shortcut there was to ship a "simple mode" to produce a highly constrained app. For example, we initially tried representing apps with a JSON schema and having the platform render that schema. We promptly rejected this approach. It was frustrating to build with and limited in its usefulness. We learned that the interface couldn't be the app's code or a constrained schema; it had to be the handful of commands the agent uses to scaffold, build, and deploy the app.


Finally, we learned early on that a tool that's easy to use and quietly wrong is more dangerous than one that doesn't exist at all. During the Block App Kit pilot, in a controlled setting before broad rollout, one of our builders shared a measurement dashboard that looked excellent (clean charts, clear layout, a confident up-and-to-the-right trend). Upon further inspection, we realized that it looked excellent because the underlying data was wrong. A subtle flaw in the sampling logic produced plausible but inaccurate numbers, and nothing in the build-and-deploy path caught it. The fix had two parts. First: make it easy to connect apps to[authoritative internal data sources](https://engineering.block.xyz/blog/building-the-data-foundation-for-automated-analytics) rather than arbitrary endpoints. Second: put test scaffolding in the default template so that a wrong number gets caught before it ships, not after someone has made a business decision on it. The solution had to live in the template, where the AI agent would get blocked by it, not in a choice the builder had to make.


## What's happening now


Block App Kit launched in mid-March. In the quarter since, weekly app views have grown by more than an order of magnitude, weekly active users have climbed from the low hundreds into the thousands, and the catalog now spans well over a thousand distinct apps, with hundreds more launching every week.


The apps are tackling real, complex business problems. A support team built an app to triage incoming tickets and route them to the right specialist automatically. A sales team built an app to analyze rep call recordings against their win rates and quota attainment. A people-ops team built a tool that answers plain-language questions across internal policy documents that used to live scattered across wikis and shared drives.


The builders are mostly non-engineers. Roughly four in five Block App Kit users sit outside of engineering, in Sales, Support, Legal and Compliance, Finance, and Marketing, spread across more than fifty different orgs. That spread isn't an accident. Block is organizing around capabilities, not org charts. When the people closest to customer reality can build and deploy capabilities themselves, the company's intelligence layer gets richer faster than any centralized team could manage.


## What we learned


The clearest signal that we'd solved the right problem came from endorsement: the security org pointed people toward Block App Kit as the sanctioned way to build and ship these apps. When the team responsible for preventing data exfiltration decides your platform is the route they want people to take, the safety-by-design bet has paid off.


The broader lesson is that the final mile of AI-built software isn't a model problem (or a human problem for that matter). The models are already capable builders. Non-engineers are eager to build. The gap is platform infrastructure: identity, access control, secret management, authoritative data connections, and a deployment path anyone can follow without making an unsafe choice along the way. Closing that gap turns individual tools into capabilities that can compose. Block needs that to operate as intelligence.


## Let's talk


If your company is wrestling with the same problem, or you're interested in trying Block App Kit, we'd love to hear from you. Please reach out tojudeedwards@block.xyz . Our team is always happy to trade lessons with others who are also working on interesting problems in the Applied AI space.
