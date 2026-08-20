---
schema_version: "1.0.0"
document_id: "025a0dc036ee20d0e94fe165cf50193319e1f2339a29861ade135f5f3493237c"
company_key: "yc-runway"
company: "Runway"
source_id: "yc-runway-news-import-fe61d44f24da"
canonical_url: "https://www.runway.team/blog/should-you-build-or-buy-your-mobile-release-management-platform"
published_at: null
first_seen_at: "2026-07-22T12:25:56.018278+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:0ce7af4022aa9a98bd89267a9b770456cc87a8c86e0ea4c4ab2739e2b5014393"
---

# Should you build or buy your mobile release management platform?

For most mobile teams, buying a release management platform beats building one. The initial scope looks manageable: CI/CD, store submissions, maybe a dashboard. But mobile release management turns out to be 12 interconnected problems touching different teams, tools, and parts of your stack. Yet every team that builds internal tooling hits the same inflection point eventually: The tool that was supposed to solve the releases problem becomes the releases’ problem.


It's a reasonable instinct. You've got great engineers, you know your process, and it feels like a two-quarter project at most.


We've watched this play out hundreds of times, with teams at companies like DoorDash, Skyscanner, and SoFi. At Etsy, their[team built an internal release management platform called Ship](https://www.runway.team/blog/build-vs-buy-retrospective-etsys-ship-in-house-release-platform) . It took over two years with dedicated engineers, requiring a complex state machine, GitHub webhooks, Slack and email notifications, and a system for rotating release drivers. The tool was successful and much-loved. But it still broke down. Over time, the team supporting Ship shrank as engineers moved to other priorities, while the system grew more complex. The engineer who co-led the project, Sasha Friedenberg, now says that if a purpose-built, third-party tool had existed at the time, he'd have told any team to just buy it.


The trajectory is remarkably consistent, and it almost never ends where teams think it will (even at the best engineering orgs in the world).


Below, we’ll break down what actually happens when mobile teams try to build release management tooling in-house and, specifically, why internal tooling so consistently ends up creating more problems than it solves.


## Mobile release management looks like one problem. It's actually 12.


When mobile teams first sit down to scope out internal release tooling, there's an understandable tendency to frame it around the most visible pain: CI/CD, maybe App Store submissions, maybe a dashboard to track what's going out the door. It looks containable.


Then you start mapping the full surface area. iOS and Android have fundamentally different branching strategies, submission flows, and rollout behaviors. Google Play and App Store Connect have completely different APIs, staged rollout mechanics, and review processes and they change on their own timelines, without warning. Version numbering that works on one platform creates conflicts on the other. Engineers who own builds have different needs than PMs who need release visibility, QA who needs to sign off on candidates, or marketing who needs to time announcements.


What looked like one problem turns out to be 12, you need:


1. **Version bumping** to keep valid build numbers in sync across two platforms with different versioning schemes.
2. **Branch creation** to prevent circular CI dependencies.
3. **Build distribution** to route builds to the right testers and tracks.
4. **Rollout scheduling** to manage two completely different staged rollout behaviors across two stores.
5. **Crash monitoring** to link spikes to specific versions across platforms.
6. **Hotfix coordination** to route fixes while a rollout is already in progress.
7. **App Store submission workflows** to manage binary uploads, metadata, and review processes with unpredictable timelines.
8. **Release notes** to compile changes across dozens of PRs and multiple teams.
9. **Stakeholder notifications** to keep PM, QA, and marketing in the loop at the right moments.
10. **Rollback procedures (built yourself)** because neither platform provides reliable native rollback.
11. **Cross-platform sequencing** because iOS and Android rarely ship at the same time.
12. **Compliance gates** to manage approvals and audit trails that vary by team and region.


Each one touches different teams, different tools, and different parts of your stack.


This isn't hypothetical. One team we work with discovered a chicken-and-egg problem with branch creation automation during their first week of scoping: The workflow that creates the release branch couldn't be triggered from the release branch because it didn't exist yet. That required rearchitecting how their entire kickoff process worked. And that was just week one.


Every team (re)discovers scope like this once they start building. Each discovery adds weeks to the timeline. The time it takes to build is not actually the biggest problem. The biggest problem is what happens after you ship it.


## You shipped it. Job's done, right?


You built the tool, you deployed it, and for a while it works. Releases go out. The team adapts. There's a sense of accomplishment. You solved the problem!


Then the ecosystem moves. Apple updates the App Store Connect API. Google changes staged rollout behavior in Play Console. Your team decides to go from biweekly to weekly releases. A key engineer who built the tool moves to another team. And quietly, steadily, the thing you shipped starts falling behind the thing you need.


This is the part nobody plans for when they scope the build because the tool isn’t just a project, it’s a product in and of itself. It needs ongoing investment just to stay current, let alone improve. But nobody inside your org signed up to own a release management product. The people who built it have other priorities now. The institutional knowledge of how the tool works thins out. Edge cases that nobody anticipated start stacking up.


And the maintenance is an opportunity cost that compounds beyond just time invested. Every product velocity improvement your team wants to make now requires a *tooling* improvement first. Want to increase release cadence? Update the tool. Add a new app or platform? Extend the tool. Automate rollbacks based on crash metrics? Rebuild the tool. The thing that was supposed to enable faster shipping becomes the thing that constrains it.


The teams who have gone furthest down this path are some of the best engineering organizations in the world. Their stories are worth studying precisely because these aren't teams that lacked talent or resources.


[Skyscanner built an in-house release platform called Skytrain](https://www.runway.team/customers/skyscanner) that served them well enough for a while. But over time, it became what they internally described as "a house of cards" that required constant maintenance, directly blocking faster release cycles. Product managers were asking why launches were delayed because of their own internal tooling. When they replaced it with a purpose-built, third-party platform, they doubled their release velocity.


Back Market built exactly the custom tool they wanted that was tailored to every quirk of their process and concluded it was not sustainable. The tool required ongoing script maintenance for edge cases like holiday blackout periods, submission timing nuances across platforms, and constantly evolving automation rules. The customization that felt like a feature turned out to be the thing that made maintenance unbearable. After switching, release manager satisfaction rose 10% and autonomy rose 5%.


One more, because the pattern matters: A major engineering org we’ve talked with spent more than three years building an internal release management system. Every check-in showed progress, but the goalpost kept moving with new store APIs, new compliance requirements, new team needs. They were deep enough in that sunk cost prevented them from considering alternatives, but never close enough to done that the tool solved the full problem.


These aren't cautionary tales about bad engineering. These are cautionary tales about a problem that refuses to stay solved.


And the breakdown doesn't announce itself gradually. It tends to surface during the moments that matter most: a critical security patch that your tool can't fast-track, a hotfix during a staged rollout that your tool has no workflow for, a high-stakes launch where the process knowledge has quietly evaporated.


Mobile release management is unique compared to other internal tooling problems because it sits at the intersection of so many teams, tools, and processes. When it breaks, the blast radius is enormous and goes beyond engineering to impact product timelines, marketing launches, and leadership visibility into what's actually shipping. And the fix is never as simple as patching a script, because the breakage is rooted in the growing distance between how the tool was originally designed and how the team actually works today.


## "But we're not building the whole thing — just a dashboard."


This is the most common version of the build instinct, and it deserves its own callout because it's the one that catches teams most off guard.


The pitch sounds reasonable: We're not going to build an entire release management platform. We just need a dashboard. Or just a Slack bot. Or just an automated submission script. Something small, something scoped, something that addresses the one pain point that's driving everyone crazy right now.


The problem is that release management is an interconnected system, not a collection of isolated tasks. Every piece links to every other piece in ways that aren't obvious until you start pulling on threads. You build a dashboard that shows release status. But now someone asks: Can I stop a release from it? Can I roll back? Can I triage a crash during a staged rollout? Can I get sign-off from QA? Can I route a hotfix? Each of those is a different workflow, touching different APIs, different teams, different parts of the process. And each one felt like it was out of scope when you started.


One mobile team in the language learning industry set out to centralize and automate their release process. They thought they were just automating branch creation. Within the first week, they discovered that the automation required rethinking their entire release kickoff workflow because of a dependency they hadn't anticipated. Another team started with a simple visibility layer and within a quarter was building notifications, rollback workflows, cross-platform sequencing, and compliance gates. They didn't plan on building a platform. They just kept needing "one more thing."


This is the gravity of release management: Every point solution gets pulled toward becoming a platform, because the underlying problem is fundamentally interconnected. The dashboard is week one. By month three, you're maintaining a system that touches CI, source control, app store APIs, crash monitoring, stakeholder communications, and compliance workflows.


There's another problem with point solutions that's easy to miss: They don't just leave gaps, they create new seams. Now your release process is split between the thing you built and the manual work that fills in around it. Those seams are where balls get dropped, where handoffs go wrong, where new team members get confused about which part of the process is automated and which part still needs a human. A point solution can actually make the overall process *less* coherent than the fully manual version it replaced, because the manual version at least makes every handoff visible.


The teams that have navigated this well are the ones who recognized early that what they actually needed was platform that addressed the entire lifecycle holistically. That's a fundamentally different thing to build, maintain, and evolve. And it's exactly the kind of thing where a dedicated third-party vendor has a structural advantage, because they've already encountered and solved for the interconnections that your team is about to discover one by one.


## Will AI make it easier to build internal release tooling?


Some teams think AI will help them build release tooling faster. That tools like Cursor or Copilot will make the "build" side of build vs. buy cheap enough to tip the scales. And AI will absolutely help write the code. But the hard part is knowing *what* to build (and maintaining it as the landscape changes). AI doesn't know that your App Store Connect integration needs to handle managed publishing edge cases, or that Google Play's staged rollout API behaves differently than what the docs say, or that your version numbering scheme will break when iOS and Android run on different cadences. Yes, AI-assisted development is amazingly fast. But it has an equally-amazing chance of helping you build a bunch of foot guns for yourself because you have no way of knowing if the code is correct.


AI accelerates code production. It doesn't accelerate domain understanding. And domain understanding is the entire game when it comes to mobile release management. And even if it did, if AI genuinely made building release tooling cheap and fast, the better question is: why would you point that extra capacity here? If AI is giving your team more bandwidth, spend it on shipping features, improving the product, winning customers. Not maintaining release plumbing.


Secondly, AI puts even more strain on internal tools. AI coding assistants are making developers write code faster, dramatically faster. One team we work with saw their PR volume triple in three months. Industry-wide,[91% of engineering organizations](https://getdx.com/blog/ai-assisted-engineering-q4-impact-report-2025/) now use AI coding assistants, merged PRs are up 23% year over year, and AI users merge roughly 60% more PRs than non-AI users.


That's the input side of the equation accelerating. The output side (e.g. getting tested, validated, approved code safely into users' hands) isn't keeping up. More code means more merge conflicts, more testing gaps, more things that can go wrong during rollout. And the quality signal is getting noisier. Three-quarters of the engineering managers we surveyed in our upcoming 2026 State of Mobile Release Management Report have already added extra approval or validation gates specifically for AI-generated code, because the volume and nature of AI-assisted changes demands more coordination and QA oversight, not less. The[2025 DORA report confirms the pattern](https://cloud.google.com/blog/products/ai-machine-learning/announcing-the-2025-dora-report) : AI adoption continues to have a negative relationship with software delivery stability, even as it boosts throughput and individual productivity.


If your homegrown platform is already struggling to keep pace with your team's current velocity, it's about to face a step-function increase in pressure from AI-accelerated development. And it won't adapt, because the people who built it are working on other things, and the mobile ecosystem is changing faster than any internal team can track.


## This is an infrastructure problem. Treat it like one.


Every conversation about build vs. buy for mobile release management eventually arrives at the same underlying question: Is this a problem your team should be solving at all?


Not whether they *can* (of course they can, at least in part, for a while), but whether they *should* because it's the thing that makes your product special, the thing that gives you a competitive edge, the thing your best engineers are excited to work on.


For virtually every mobile team, the answer is no. Release management is infrastructure. Like your cloud provider, your CI, your monitoring stack. It's something that should just work, reliably, every time, without requiring specialized knowledge to operate. You wouldn't build your own cloud. You wouldn't build your own CI from scratch. The reason teams still consider building their own release management platform is that the category is newer and the problem is less visible.


Infrastructure problems are exactly the ones that make the most sense to hand to a dedicated third-party vendor that lives and breathes this every day. Boring problems need to be solved by people who find them interesting and for whom solving them is the entire business, not a side project that competes with product work for engineering attention. That's why internal release tooling inevitably gets deprioritized, and why the team supporting it inevitably shrinks. Nobody inside your org wants to own this long-term. That's exactly why it breaks.


Only 12% of the engineering managers we surveyed for our 2026 Report said building internal tooling would be their first move for improving releases. Most teams already sense the maintenance burden isn't worth it.


The teams that have built, maintained, watched it break, and eventually switched all arrive at the same place: The tool that was supposed to solve the releases problem had become the releases’ problem. And the path forward was handing it to people whose entire job is making sure it never breaks.


If you want to dive deeper into the build vs buy conversation, register for[our live virtual discussion](https://www.runway.team/webinar/build-vs-buy-mobile-release-tooling) at 10 am PT/1 pm ET, May 28 with expert engineers from Monzo, Spotify, and Tuist.


## FAQ


### **How do I reduce manual work during mobile publishing so devs spend more time building than shipping?**


Manual work accumulates across the entire release process: coordinating approvals, managing store submissions, configuring staged rollouts, keeping stakeholders informed, and handling the edge cases that scripts weren't built for. A release management platform automates and connects those steps so the process runs consistently regardless of who’s running the release. Releases become repeatable enough that any engineer can run one, not just the one or two who built the process.


### **What are the benefits of using a unified platform for mobile app release management?**


Releases run the same way regardless of who's driving. Approval gates, store submissions, and rollout thresholds stop depending on institutional knowledge. Dev, QA, product, and marketing can all see where a release stands without tracking someone down in Slack. When something goes wrong, the failure point is immediately visible instead of reconstructed from logs across multiple tools. And because the process lives in a platform instead of someone's head, any engineer can run a release.


### **Is it worth paying for mobile release management vs building internal tooling?**


For most teams, yes. The build cost is feasible. The maintenance cost compounds. Keeping scripts current with App Store Connect API changes, handling OS updates that break automation, rebuilding tooling when a CI/CD provider changes its format is all work that falls on senior engineers who should be building product. The real comparison is your team's ongoing maintenance hours versus a platform subscription maintained by people for whom this is the only product.
