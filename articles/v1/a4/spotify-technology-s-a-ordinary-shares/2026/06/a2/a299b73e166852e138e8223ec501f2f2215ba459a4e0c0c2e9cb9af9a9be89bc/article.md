---
schema_version: "1.0.0"
document_id: "a299b73e166852e138e8223ec501f2f2215ba459a4e0c0c2e9cb9af9a9be89bc"
company_key: "spotify-technology-s-a-ordinary-shares"
company: "Spotify Technology S.A."
source_id: "spotify-technology-s-a-ordinary-shares-rss-e59c2e9a43cf"
canonical_url: "https://engineering.atspotify.com/2026/6/code-with-claude-coding-is-no-longer-the-constraint/"
published_at: "2026-06-03T15:50:18+00:00"
first_seen_at: "2026-07-20T03:30:11.313168+00:00"
fetched_at: "2026-07-28T20:49:35.808686+00:00"
content_hash: "sha256:4d77f093f0d5a9f580de475c515b139513b7bbdccdf379bebd6bbf4f214e91d9"
---

# Coding Is No Longer the Constraint: Scaling Developer Experience to Teams and Agents at Spotify

What happens when coding stops being the bottleneck? At Spotify, we’re starting to find out.


Niklas Gustavsson, Spotify’s Chief Architect and VP of Engineering, recently shared how our yearslong investment in internal development platforms and engineering best practices is driving our AI transition — enabling both our teams and our agents to move faster than ever, while also providing the foundations for meeting the new challenges ahead. Watch his full talk from Code with Claude 2026 below.


Read on for key highlights.


## Adoption that went “completely bananas”


The rate of adoption for AI coding tools at Spotify has been unlike anything we’ve seen before — and it accelerated dramatically with the Opus 4.5 release late last year. Today, more than 99% of our engineers use AI coding tools every week, 94% report that AI has made them more productive, and we’re seeing a 76% increase in pull request frequency, with the vast majority of PRs authored by a developer working alongside an AI agent.


AI tool adoption dipped around the holidays across the board, but the orange spike shows Claude Code adoption skyrocketing with Opus 4.5


###### “We roll out tools internally all the time to make our developers more productive, but we have never seen the rate of adoption that we’ve seen rolling out AI coding tools.”


## We started this journey before agents


A few years ago, we noticed our production codebase was growing seven times faster than the number of engineers. Developers were spending more and more of their time on maintenance — upgrading dependencies, migrating APIs, patching vulnerabilities — and less time building features. Migrations were the number one source of developer frustration.


Instead of asking hundreds of teams to manually update their components one by one, we imagined a different approach. What if we used automation to make changes across hundreds or even thousands of software components at once? That idea became Fleet Management, and the underlying system we built to execute it is called[Fleetshift](https://backstage.spotify.com/fleetshift) . Fleet Management has been running at Spotify for several years now. To date, we’ve merged more than 2.5 million automated maintenance PRs, the vast majority auto-merged with no human in the loop.


The graph shows the overall growth of automated PRs at Spotify — with the green section representing PRs that were auto-merged


###### “Instead of doing this component per component and fairly manually, can we imagine a way where we do this as a way to mutate our entire fleet of components?”


## Meet Honk, our background coding agent


Fleet Management worked beautifully for simple changes, but complex code modifications — replacing API calls, refactoring usage patterns — pushed our deterministic scripts to the breaking point. When you run a script across millions of lines and thousands of components, you hit every corner case.


As LLMs matured, we saw an opportunity. What if, instead of writing ever-more-complex deterministic scripts, we could use a model to handle the code modifications?


###### “It has a silly name and a silly icon, but it’s a very useful tool, as it turns out.”


After many iterations, the result is[Honk](https://backstage.spotify.com/how-spotify-built-honk) , our background coding agent. It may have a silly name, but our fine feathered coding friend has become an essential part of our everyday operations. Under the hood, Honk runs Claude using the Agent SDK, wrapped inside our own harness and deployed in Kubernetes pods so we can schedule many sessions concurrently across our cloud environment. It has access to a set of trusted tools, including the ability to run builds in our CI environment across multiple operating systems to verify that its changes are correct.


Honk integrates directly into our Fleet Management tooling: Fleetshift helps humans manage the orchestration — identifying targets, scheduling changes, tracking progress — while Honk sits in the middle doing the actual code modifications. A team running a migration can see at a glance how many PRs have been created, how many have been merged, and which ones need attention. Our most recent Java migration across our backend services took three days.


###### “What used to be hundreds of teams doing migrations for their components, taking weeks and weeks or months, now can be done by a single engineer in a few days.”


Developers being developers, they quickly figured out new ways to take advantage of our surprisingly capable, self-sufficient background coding agent. Honk is now available over Slack, where engineers can mention it mid-conversation — a natural source of context — and it will fly off, work on the problem, and come back with a PR.


Just another day on the Goose Farm: Our internal real-time dashboard shows current activity in Spotify’s Fleet Management system. Each goose represents an active background coding session powered by Honk.


And with Honk v2, we’re introducing multiplayer collaboration: shared agent sessions, team projects, and agent orchestration through Chirp. We’re excited for a world where agents collaborate with multiple developers and teams, not just one person at a terminal.


## Developer experience is for agents, too


One of Spotify’s oldest engineering principles is: “The fewer technologies we are world-leading in, the faster we go.”


It’s an idea that predates AI at Spotify by many years. By standardizing on a focused set of technologies, we build deeper expertise, eliminate unnecessary decisions for teams, and make it far easier for engineers to collaborate across the codebase. A typical backend service at Spotify looks very similar to every other backend service — same technology stack, roughly the same design patterns.


That principle has turned out to be just as important for agents. When Claude has a lot of other code to reference and that code is consistent,[it performs significantly better](https://engineering.atspotify.com/2026/4/background-coding-agents-dataset-migrations-honk-part-4) . We’ve seen this clearly: in our more fragmented codebases, agent performance is measurably worse.


###### “If Claude has a lot of other code to look at, and that code looks roughly consistent, Claude will do a better job. That’s what we’re seeing.”


The starting point for this consistency is[Backstage](https://backstage.spotify.com/) , our open source[internal developer portal](https://backstage.spotify.com/backstage-101) (IDP). Before Backstage, Spotify had roughly a hundred different internal tools — one for deployments, another for CI, another for A/B tests. It was fragmented and confusing. Backstage consolidated all of that into a single pane of glass built around a catalog of our software components. Today, for anything a developer needs to do with one of our components, they do it in Backstage.


Both teams and agents can find everything they want to know about any component in the Backstage Software Catalog


And as it turns out, that’s equally useful for agents. We expose Backstage’s capabilities as MCPs and command-line tools, so Claude can look up who owns a component, read its documentation, or ping the responsible team on Slack.


We also use Backstage to drive standardization through what we call[Soundcheck](https://backstage.spotify.com/partners/spotify/plugin/soundcheck/) and golden state. Golden state defines the recommended technologies and practices for each type of component. Soundcheck provides a UI where teams can self-assess their components against those standards. Combined with static analysis and linting, these standards become active guardrails — when Claude works in our codebase and uses a pattern we know isn’t optimal for our infrastructure, it gets immediate feedback from our lint system and corrects itself.


###### “When Claude works in our codebase, it will get immediate feedback on if it’s using the right set of technologies and right set of design patterns.”


This feedback loop works for developers and agents alike, and it’s been one of the most effective ways we’ve found to drive consistency at scale.


## Coding is no longer the bottleneck


As coding velocity increases, the constraints shift toward human decisions. Spotify has always had more ideas than capacity to build them — but now anyone can open Claude in our client monorepo and prototype a feature idea in minutes instead of days. Even our CEO is building prototypes this way.


###### “This has brought prototyping from something that could take days or weeks to literally taking minutes now.”


The flip side: we now have 76% more PRs to review. We’re learning where to apply human judgment — auto-merging what’s safe, focusing review where it matters most — and rethinking how we plan and prioritize as the bottleneck moves from coding to decision-making.


The investments we made years ago in Fleet Management, Backstage, and engineering standardization have positioned us well. And we’re excited about what comes next.


[Fleetshift](https://backstage.spotify.com/fleetshift) *and*[Honk](https://backstage.spotify.com/how-spotify-built-honk) *are available as part of*[Spotify Portal](https://backstage.spotify.com/) *for Backstage. If orchestrating complex code changes at scale is relevant for your organization,*[reach out](https://backstage.spotify.com/contact-us/try-portal/) *to our platform team for a personalized walkthrough.*
