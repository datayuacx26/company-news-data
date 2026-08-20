---
schema_version: "1.0.0"
document_id: "3ba60610ce4bf59860cf4913e912079a48afb64dc92c6a8cd050b4e3f959f7a7"
company_key: "yc-emergent"
company: "Emergent"
source_id: "yc-emergent-news-import-16a7bf482038"
canonical_url: "https://emergent.sh/news/anthropic-released-claude-fable-5"
published_at: "2026-06-11T00:00:00+00:00"
first_seen_at: "2026-07-21T18:00:01.007987+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:921db68e9f9f7f440c202213b55f923e3a1303d6add4c8bf02ec366aef3499bd"
---

# Claude Fable 5 Is Here: What Builders Need to Know

#### Update, June 27, 2026


On June 12, three days after launch, the U.S. government[issued an export control directive](https://www.anthropic.com/news/fable-mythos-access) forcing Anthropic to disable both Fable 5 and Mythos 5 for all customers worldwide. The order cited national security concerns over a reported method of bypassing Fable 5's safety guardrails. Anthropic[called the technique narrow and non-universal](https://www.anthropic.com/news/fable-mythos-access) .


On June 27, after two weeks of negotiations, Commerce Secretary Howard Lutnick[cleared Mythos 5 for redeployment](https://www.cnbc.com/2026/06/26/us-government-anthropic-claude-mythos5-ai.html) to roughly 100 U.S. organizations that defend critical infrastructure. Anthropic[confirmed on X](https://x.com/AnthropicAI/status/2070665903440871779) that it is working with the government to expand access further and restore Fable 5 for general use.


Fable 5 remains offline for all users as of this writing.[Axios reports](https://www.axios.com/2026/06/27/anthropic-fable-5-return-soon) that the restrictions could be lifted as soon as this coming week, but no date has been confirmed.


Pricing, access, and availability details below reflect launch-day information and are not current.


Every few weeks, someone tells us they have built the most powerful AI model yet. We have learned to read those claims with a raised eyebrow. This time, the interesting part is not the claim itself. It is the two months of history behind it.


On June 9, Anthropic released[Claude Fable 5](https://www.anthropic.com/news/claude-fable-5-mythos-5) . On paper, it is the strongest model the company has ever let the public touch. In practice, it is something more unusual: the public version of a model Anthropic previously decided was too dangerous to release at all.


## The model Anthropic held back


Back in April, Anthropic built a model so capable at finding and exploiting software vulnerabilities that it[refused to release it publicly](https://www.nbcnews.com/tech/security/anthropic-project-glasswing-mythos-preview-claude-gets-limited-release-rcna267234) . That model, Claude Mythos Preview, went only to a vetted group of cybersecurity firms and infrastructure providers through a program called[Project Glasswing](https://www.anthropic.com/glasswing) , with partners like Microsoft, Amazon, and Cisco testing it behind closed doors. Last week, access widened to roughly 150 organizations across 15 countries. Still no public release.


Fable 5 is Anthropic's answer to the obvious question: how does everyone else get to use a model like that?


Here is the structure. Fable 5 and the simultaneously released[Claude Mythos 5](https://emergent.sh/news/weeks-after-claude-mythos) are the same underlying model. The difference is access. Fable 5 ships to the public with new safety guardrails attached. Mythos 5 keeps full capability switched on in sensitive areas and remains restricted to approved organizations, deployed in collaboration with the US government. Both sit in a new "Mythos-class" tier that ranks above Claude Opus, which until this week was the ceiling of what Anthropic sold to anyone.


The specs tell the size story: a 1-million-token context window, 128,000-token maximum output, and a knowledge cutoff of January 2026.


## What it can actually do


Anthropic's core claim is that Fable 5 exceeds every model it has ever made generally available, that it leads on nearly all tested benchmarks, and that its advantage grows as tasks get longer and more complex.


[Source](https://www.anthropic.com/news/claude-fable-5-mythos-5)


### What early-access partners are reporting


The enterprise testimonials are mostly about long-running, complex work.[Stripe reported](https://www.anthropic.com/news/claude-fable-5-mythos-5) that Fable 5 completed a migration across a 50-million-line codebase in a single day, work Anthropic says would have taken an engineering team more than two months. Cursor's CEO said the model unlocked a class of long-running problems that older models simply could not finish. GitHub's product chief described early tests where it handled complex, drawn-out coding work with more autonomy and reliability than any previous benchmark. Vision improved sharply too: Anthropic says the model can rebuild a working web app's source code from screenshots alone.


### What independent testers found


Independent testing has backed up the broad strokes.[Simon Willison](https://simonwillison.net/2026/Jun/9/claude-fable-5/) , one of the most respected independent voices in AI tooling, spent his first day with Fable 5 and described it as "something of a beast." He prompted it to build a full Python-in-WebAssembly sandbox from a couple of uploads and a few sentences of direction; it worked. Then he turned it loose on his open-source LLM library. By the end of the day, it had written[an entire release](https://llm.datasette.io/en/latest/changelog.html#a3-2026-06-09) with new features, tests, and documentation that Willison said felt like "several days' worth of work."


[Dan Shipper](https://x.com/danshipper/status/2064393970856124501?s=20) at Every.to, a trusted AI analysis outlet, had a week of early access and concluded that Fable 5 is "the best coding model in the world."


> BREAKING:
>
>
> Anthropic just dropped Claude Fable 5—this is Mythos, made safe for public release. It is the best coding model in the world.
>
>
> We've been testing it internally[@every](https://x.com/every?ref_src=twsrc%5Etfw) for the last week or so across coding, writing, marketing, editing, and more—here's our vibe check:
>
>
> -…[pic.twitter.com/EPFpQmQqry](https://t.co/EPFpQmQqry)
>
>
> — Dan Shipper 📧 (@danshipper)[June 9, 2026](https://x.com/danshipper/status/2064393970856124501?ref_src=twsrc%5Etfw)


His team tested it across coding, writing, marketing, and editing, and documented one-shot builds of complex applications, including a fully interactive "Library of Babel" game and a browser-based "Breathwork Garden."


### What developer platforms are seeing


[CodeRabbit](https://www.coderabbit.ai/blog/fable-5-model-review) , a code review platform, found the model's real shift in how little guidance it needs: "We could give Fable 5 vague prompts and still get complete projects rather than prototype shells." Their review noted that it moves directly into implementation rather than over-explaining its plan or asking for permission.[GitLab](https://nerdschalk.com/claude-fable-5-vs-the-world-benchmarks-safety-rails-and-what-users-really-think/) , which integrated Fable 5 into its Duo Agent, reported "measurable improvement in first-shot correctness" and fewer back-and-forth iterations.


## Same model, two doors: how the guardrails work


This launch is also a preview of how frontier AI may get distributed from now on, and it is worth understanding even if you never think about AI safety.


### Fallback, not refusal


When Fable 5 detects a request related to offensive cybersecurity, biology and chemistry, or attempts to clone its capabilities into a competing model, it does not refuse. It routes the request to Claude Opus 4.8, the next model down, and tells you it did so. Anthropic says this fallback triggers in fewer than 5 percent of sessions, meaning[more than 95 percent of usage runs on the full model](https://www.securityweek.com/anthropic-launches-claude-fable-5-mythos-class-ai-with-cybersecurity-guardrails/) . The company also reports that over 1,000 hours of external bug-bounty testing failed to produce a universal jailbreak, though the UK's AI Safety Institute made early progress toward one in initial testing.


### Not everyone is buying it


The reception has been split. Anthropic product lead Dianne Penn[told Axios](https://www.axios.com/2026/06/09/anthropic-mythos-class-safeguards) the company is being "deliberately more conservative" at launch, acknowledging that some legitimate security and science work will get routed to the weaker model for now. On Reddit, early reactions on r/ClaudeAI were sharper. One widely upvoted post called the launch "[a preview of AI inequality](https://www.reddit.com/r/ClaudeAI/comments/1u1fsdi/claude_fable_5_feels_less_like_a_model_launch_and/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button) ," arguing that the public version is visibly weaker than the Mythos preview given to select partners.


### The 30-day data retention policy


The other policy change drawing attention is data retention. Using Fable 5 requires[30-day data retention for safety monitoring](https://techcrunch.com/2026/06/09/anthropic-released-claude-fable-5-its-most-powerful-model-publicly-days-after-warning-ai-is-getting-too-dangerous/) , even for enterprise customers who previously had zero-retention agreements. Anthropic says the data will not be used to train future models.


The ripple effects are already visible:[GitHub's Copilot changelog](https://github.blog/changelog/2026-06-09-claude-fable-5-is-generally-available-for-github-copilot/) notes that Fable 5 is off by default for Enterprise and Business plans because of this requirement, and admins must explicitly opt in. On X, some framed this as "regulatory capture," where access to the strongest models comes bundled with mandatory surveillance. TechCrunch suggested it could[set an industry precedent](https://techcrunch.com/2026/06/09/anthropic-released-claude-fable-5-its-most-powerful-model-publicly-days-after-warning-ai-is-getting-too-dangerous/) in which frontier model access requires data-retention policies framed as safety measures.


For everyday building, none of this is likely to touch you. App development, dashboards, marketplaces, booking systems, and internal tools sit far from the restricted zones. The precedent is the interesting part: capability and access are now two separate dials, and labs have started turning them independently.


## What it costs, and the deadline to know about


Access is the other half of this story, and it has a deadline in it.


On the API, Fable 5 costs[$10 per million input tokens and $50 per million output tokens](https://www.anthropic.com/news/claude-fable-5-mythos-5) . That is double the price of Claude Opus 4.8, but less than half the cost of the old Mythos Preview ($25 input / $125 output). It is a premium tier, not a default. Opus stays available at half the cost for routine work.


How fast the costs add up at this tier is worth knowing.[Simon Willison tracked his usage](https://simonwillison.net/2026/Jun/9/claude-fable-5/) and spent $110 worth of tokens in a single day of intensive coding, all on his $100/month Max subscription. That is one developer, one day. The model is powerful; it is not cheap.


On subscriptions, Anthropic is doing something unusual. Fable 5 is included at no extra cost on Pro, Max, Team, and seat-based Enterprise plans only through June 22.


From June 23, using it will draw on paid usage credits until capacity catches up, at which point Anthropic says it intends to fold the model back into standard plans.


## What this means for builders


Independent reviewers are talking about getting "several days' worth of work" done in hours. Code review platforms are reporting that vague prompts now produce complete projects. On Reddit, people are one-shotting entire games. The model layer just took a visible jump.


For non-technical founders, that shift changes the question. The bottleneck on most software ideas is no longer whether AI can build them. It is whether you can describe what should exist, and whether the tool you use turns that description into software a real business can run on: with a backend, a database, authentication, payments, and a domain that customers can actually visit.


If you have been curious about what Fable 5 can do, now is the time to find out. The model is available free on Claude's Pro, Max, Team, and Enterprise subscription plans through June 22, after which it moves to paid usage credits. That is a real window to test the strongest publicly available AI model against your hardest idea before the meter starts running.


That said, tracking model tiers, credit schedules, and fallback rules is not a job most non-technical builders should take on long term. It is one reason app-building platforms exist: they absorb that complexity so you do not have to. You describe the product; the platform decides which model does the work.


Fable 5 is[already live on Emergent](https://x.com/emergentlabs/status/2064395188685156715) , with no waitlist. If you have been sitting on a product idea that felt too complex to build without an engineering team, this is a good week to take it off the shelf.[Describe it to Emergent](https://emergent.sh/) and see how far a single prompt gets you now.


Stay tuned to this space for more updates like this from the world of AI and[vibe coding](https://emergent.sh/learn/what-is-vibe-coding) .
