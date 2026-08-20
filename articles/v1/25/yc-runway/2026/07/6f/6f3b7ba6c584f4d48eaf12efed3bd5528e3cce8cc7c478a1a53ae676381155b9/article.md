---
schema_version: "1.0.0"
document_id: "6f3b7ba6c584f4d48eaf12efed3bd5528e3cce8cc7c478a1a53ae676381155b9"
company_key: "yc-runway"
company: "Runway"
source_id: "yc-runway-news-import-fe61d44f24da"
canonical_url: "https://www.runway.team/blog/build-vs-buy-mobile-release-tooling-recap"
published_at: null
first_seen_at: "2026-07-22T12:25:56.018278+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:c900cd95e7f61142345d1586e1221e6a7257af12d22bfcc9cca6cd644a522bc4"
---

# Build vs. buy for mobile release tooling: lessons from Monzo, Spotify, Tuist, and Etsy

Most mobile teams never make a deliberate decision to build their own release tooling. It accumulates. Someone writes a script to avoid cutting a build by hand, then another, and a few years later the team is maintaining a small platform that nobody set out to create. By the time the build-vs-buy question comes up, it’s rarely a clean choice between two paths. It’s a decision about whether to keep investing in something that grew organically, or move it out of house.


Runway hosted a panel on exactly that decision with four people who have made the call more than once, on some of the largest mobile teams around:


- Maria Neumayer (Senior Staff Engineer at Monzo, previously Skyscanner)
- Jay Henry (Senior Engineering Manager at Etsy)
- Jacob Vesterlund (release team at Spotify)
- Pedro Piñera (CEO of Tuist, who previously built ShipIt Mobile at Shopify)


Here are the highlights from that discussion.


## Quick takeaways


- There’s no headcount or revenue threshold that decides build vs. buy. It’s a call you re-make as you grow, and it runs in both directions.
- Decide what you’re actually building first. A CI script that cuts and ships a build is a different project from a system that coordinates a hundred teams across the full release cycle.
- The real cost of building is maintenance and opportunity cost, not the first version. AI doesn’t change that; it just gets you to the maintenance trap faster, and you won’t out-build a vendor pointing the same AI at the same problem full-time.
- What you’re really choosing is the coordination system around releases, not just the software. As teams multiply, the bottleneck stops being automation and becomes shared understanding, so the tool that wins is the one everyone actually trusts as the source of truth for what’s shipping.
- The interface to releases is shifting toward terminals and agents (MCP), which raises the bar for one clean, legible release layer instead of twenty inconsistent ones.


## There’s no threshold that tells you when to buy


There is no headcount number, revenue figure, or release cadence that signals when to stop building and start buying. As Maria noted, the right answer keeps changing as a team grows, so the real work is continually re-evaluating whether what you have still fits. A 30-person company can coordinate a release with a single Slack message and be fine. Add more contributing teams, more asynchronous communication, and more stakeholders who care when their work ships, and that same approach gradually stops scaling.


Jay made the time dimension concrete. Etsy’s homegrown release orchestration layer was built in 2015, old enough now, as he put it, to be a fifth grader. The people who built it have since moved on. So the question for anyone building today isn’t just “can we build this?” but “what does this look like in ten years, and who carries it forward when the original team is gone?”


Jacob added an important reassurance: buying isn’t a one-way door. If you outgrow a tool you can build later, and if you over-built early you can move to something off the shelf. Treating the decision as reversible lowers the stakes considerably.


## Decide what you’re actually building before you decide build vs. buy


“Build vs. buy” only has a clear answer once you’ve named the specific capability you’re deciding about, because the decision looks completely different depending on where you set the bar. At the low end, a Fastlane script wired into CI can be enough: it cuts a build, signs it, and pushes it to the stores. At the high end, you’re coordinating a hundred contributing teams across the entire release cycle, where cutting and signing the build is just one step in the middle, with all the visibility and process that implies. Those are different projects with different costs, and teams get into trouble when they quietly want the second one but budget for the first.


Maria’s experience showed how that gap forces a decision. At Skyscanner, the team had grown its own tooling to the point where almost no one understood how it worked anymore; the knowledge had narrowed to one person who’d since moved into management, so every broken build meant going to ask him. Getting that tooling to the standard they actually wanted would have meant a large, sustained investment. They chose to buy, so they could put their energy into the release process itself instead of rebuilding the parts every mobile team needs anyway: talking to App Store Connect and Play Console, wiring up observability, handling the full cycle. None of that was where their advantage lived.


Jay described the inverse exercise on the build side: a deliberate audit of which parts of the stack were still genuinely Etsy-specific and which the industry had since caught up on. Etsy worked through its platform that way across 2025, asking the question piece by piece. The honest build case isn’t “we can build it.” It’s “this specific part is ours, and the rest isn’t worth our engineers’ time.”


Pedro named the trap that sits underneath the build side. The biggest appeal of building in-house, that you can shape it exactly to how your teams already work, is also its biggest liability. At Shopify, moving fragmented teams onto one standard was far more of a human problem than a technical one, and every “can it also do this?” request risked turning the tool into a Frankenstein that hard-coded one team’s habits at everyone else’s expense. Many of those habits, he noted, had been arrived at more or less by accident in the first place. Gabe added that vendors face the same pressure one level up: a release platform has to serve many teams without collapsing into either rigidity or sprawl.


## Release management is a coordination problem before it’s a tooling problem


The clearest through-line of the hour was that release management is fundamentally about coordinating people, and the tool’s real job is to make that coordination legible. Every example the panel raised was a version of that point. Jacob described Spotify’s dedicated, full-time release manager and noted that most of the role isn’t clicking buttons; it’s talking to teams, surfacing blockers, and keeping a hundred teams shipping into three apps each week roughly in sync. Pedro’s most valuable change at Shopify wasn’t the automation but the standardization: suddenly there was one way to do a hotfix instead of twenty, which gave everyone a shared language.


That shared language breaks down fast outside the mobile org. Jay pointed out that asking a stakeholder for the state of a release can produce an answer that’s unintelligible to them. One place anyone can read, regardless of role, is the difference between alignment and losing people in a maze of spreadsheets and Slack threads. Maria made the same point from the user’s side: replacing “scroll back through the Slack channel to find when the app went live” with a single source of truth was a real improvement on its own.


That framing also changes how you measure the decision. Jay’s advice was to pair the hard math (maintenance hours, dollars, opportunity cost) with the soft math of whether people actually use the tool and trust it. Jacob went furthest: Spotify’s primary metric for release tooling is internal developer satisfaction, tracked quarter over quarter, ahead of latency or cost.


Put together, these points reframe what’s actually on the table. When you choose build or buy, the thing you’re really choosing is the coordination system around the software, not just the software itself. That’s what has to scale, and it’s what your engineers and stakeholders will judge. A tool that automates flawlessly but leaves everyone guessing about the state of the release hasn’t solved the problem the panel kept describing.


## What AI does and doesn’t change


AI comes up immediately in any build-vs-buy conversation now, because it appears to lower the cost of building. If a small team can stand up a release tool in a weekend with an assistant, maybe the build side just got more attractive. The panel’s view was that AI changes how fast you can build something, not the parts of the decision that actually bite.


Jay’s framing was that the maintenance trap still exists; AI just lets you reach it faster. Generating the first version is easier than ever, which can create the impression that the long-term math has changed. It hasn’t, because the cost was never mostly in the first version.


He added a second point worth following all the way to its conclusion. Whatever AI tooling you have, the vendor building a competing product has the same tooling, pointed at the same problem, by a team whose full-time job is making that product good. So building with AI gives you no durable edge on the build side; if anything it widens the vendor’s lead, because they apply the same acceleration on top of years of focus you’d be starting without. The place AI helps you build is rarely the place that justifies owning the tool.


Maria added the durability angle. You can vibe-code a release tool quickly, but App Store Connect and Play Console keep changing the rules underneath it, and a system on your critical path has to stay stable and legible to people other than its author. If you can’t release, you can’t reach your customers.


The workflow shift, on the other hand, is already underway, and it points somewhere specific. Jacob is seeing Spotify developers pull release data through their terminal and through Claude rather than the UI, and ask to stop opening a dashboard for things like reporting test results. To keep that from fragmenting, his team is moving toward exposing a single clean MCP server instead of letting twenty inconsistent ones spring up. Pedro pushed the idea furthest: a release process is essentially a state machine coordinating systems and people, and if chat and agents are becoming the primary interface, it’s worth asking how much of it should live in a dashboard at all. The throughline is that automation isn’t removing the need for a shared release layer; it’s raising the bar for one, because an agent needs a consistent, legible source of truth to act against just as much as a person does. The counterweight, from Jacob: teams still tend to build too much, too early, because it’s a fun problem. If you have to ask whether you should build something, the answer is usually no.


## Decide deliberately, and keep deciding


The panel’s closing advice wasn’t a formula. It was a short list of questions to keep asking. Look honestly at your engineers and how much time they can actually give to building and maintaining this. Ask whether the thing even needs to be bespoke, or whether you’d be making it specific just to make it specific. Weigh what those same people could be building instead. And remember the decision is reversible. Whether you’re Spotify maintaining a deep in-house platform or a ten-person team wiring your first release into CI, the work is the same: keep asking the question rather than answering it once and forgetting.


Pedro offered a counterpoint worth keeping. Many of the best tools start with someone building for the joy of it. Tuist began as a side project, and Spotify’s Backstage, as Jacob confirmed, started as an internal tool before becoming open source and then a product other companies buy. Sometimes the right call really is to build, because building is how people come to understand a problem deeply enough to solve it well. The counterweight, as Gabe noted, is that one person’s fun can become the team’s pain a few years later, once they’ve moved on and someone else is maintaining the fifth-grader.


Which lands on the answer a good senior engineer gives to almost everything: it depends. The value of the session was making clear what, exactly, it depends on.


The full recording is available if you’d like to hear Maria, Jay, Jacob, and Pedro make these points in their own words 👉[watch the recording](https://youtu.be/_Cb9C_p_7wA) .


## FAQ


### When should a mobile team build vs. buy release tooling?


There’s no headcount, revenue, or release-cadence number that flips the switch. Treating it as a one-time decision is the actual mistake. It’s a call you re-make as your team grows, and it runs both ways: you can build now and buy later, or the reverse. The useful question isn’t “can we build this?” It’s whether this specific piece is genuinely yours to own, and whether your best engineers’ time is better spent here than on the product only your team can build. When the honest answers are “not really” and “no,” that’s your signal to buy.


### What’s the real cost of building mobile release tooling in-house?


The cost that bites isn’t building the first version. It’s everything after. An in-house release tool quietly becomes critical infrastructure someone has to keep alive as App Store Connect and Play Console keep changing the rules underneath it, usually years after the people who built it have moved on. (Etsy’s homegrown tooling dates to 2015, old enough now, as Jay put it, to be a fifth grader.) But the bigger line item is the one that never shows up on a spreadsheet: the product work your best engineers aren’t doing because they’re babysitting releases instead.


### Does AI make it cheaper to build your own release tooling?


AI makes the first version cheaper, which is exactly the part that was never the problem. The maintenance trap doesn’t go away; you just reach it faster. And whatever AI you point at the problem, the vendor whose full-time job is this is pointing the same AI at it, with years of focus you’d be starting without. There’s a durability catch, too: you can vibe-code a release tool in a weekend, but App Store Connect and Play Console keep moving, and anything on your critical path has to stay legible to people other than its author. If anything, agents raise the bar for a single, clean release layer instead of lowering it. An agent needs one trustworthy source of truth to act against, the same way your team does.
