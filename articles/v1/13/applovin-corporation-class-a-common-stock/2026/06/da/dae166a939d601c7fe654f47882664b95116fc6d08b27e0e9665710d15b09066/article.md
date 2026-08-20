---
schema_version: "1.0.0"
document_id: "dae166a939d601c7fe654f47882664b95116fc6d08b27e0e9665710d15b09066"
company_key: "applovin-corporation-class-a-common-stock"
company: "Applovin Corporation"
source_id: "applovin-corporation-class-a-common-stock-news-import-d34419822e76"
canonical_url: "https://applovin.com/en/blog/measurement-lens"
published_at: "2026-06-16T00:00:00+00:00"
first_seen_at: "2026-07-24T16:57:30.934674+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:4283e775efde77f2098c4f606096a75709f273cc1b3b2663806689b7d4ee43d2"
---

# Different numbers, one channel: making sense of AppLovin through a measurement lens AppLovin | Blog

We spend a lot of time talking to advertisers about measurement. Not because measurement is the most exciting topic in performance marketing, but because it's where confusion lives. And confusion, more than budget, more than creative, is usually what holds a brand back from getting the most out of a channel.


The conversations tend to go one of two ways. Either an advertiser comes to us saying AppLovin looks strong in-platform, but their third-party tool doesn't see it. Or they come in saying their MMM is actually attributing more to AppLovin than the platform itself reports. Both end up in the same place. The disagreement between those numbers is not telling you who is right. It is telling you what each model was built to see.


## Stop waiting for one number to tell the whole story


Here is a simple way to think about it. A customer sees an AppLovin ad while playing a mobile game on a Tuesday afternoon. They don't click. Three days later, they search for the brand on Google, land on the site, and buy. On Friday, their roommate, reached by the same campaign, buys on Amazon without ever touching the DTC site.


Today, AppLovin's in-platform report sees neither of those conversions. Click-based attribution requires a click, and none happened. Your MTA tool picks up the Google search and gives it credit, maybe with a partial assist somewhere upstream. Your MMM may pick up that AppLovin spend correlates with a broader lift in demand, including the Amazon sale. Your post-purchase survey captures neither customer because they were never prompted. An incrementality test, run well, is the only instrument that would tell you with statistical confidence how many of those conversions genuinely would not have happened without the campaign.


None of those answers is wrong. They are just not measuring the same thing.


The advertiser who uses all of them to triangulate is in a much better position than the one waiting for a single source of truth that doesn't exist.


## Half of what AppLovin drives never shows up in the dashboard


Fospha, which measures AppLovin across a growing cohort of ecommerce brands, has found that last-click and MTA models are significantly underreporting AppLovin's contribution, because the channel operates at a discovery layer those tools aren't built to see.


*Source:[Fospha's AppLovin Playbook](https://www.fospha.com/reports-and-guides/applovin-playbook)*


Prescient's MMM benchmark series adds another layer:


Both partners are pointing at the same thing. AppLovin's in-platform reporting is conservative, and its contribution is showing up well beyond what last-click captures.


## AppLovin's impact is growing faster than its own reporting shows


We have now worked through a growing body of incrementality tests on AppLovin over the past 17 months since the first incrementality test came in. A few things stand out.


Tests have gotten better built over time. Advertisers have come to understand the mechanics now - what a holdout is doing, why test duration and geo balance matter, what a clean result actually requires. And because we review most designs before they launch, fewer tests go live underpowered or with a geo split that was never going to balance. None of that shows up as a single headline number. It shows up as results a brand can feel reasonably confident in and act on, instead of results we have to hedge.


The platform picture is also changing in ways that matter here. The incremental factor we observe, a brand's iROAS measured by a geo holdout experiment relative to its AppLovin in-platform reported ROAS, has more than doubled over the past year. In-platform reporting currently captures click-through attribution only and that gap between what the platform reports and what a controlled experiment reveals has been consistent with the third-party signals above: a meaningful share of what AppLovin drives isn't visible in its own reporting.


## Not ready to test yet is not a reason to stop optimizing


The brands with the most consistent results understand that an incrementality test is a moment-in-time experiment with real constraints. Holdout size, budget level, test duration, geo composition, campaign stability - all of these interact. **A few of these we hold firm on: campaigns need to be out of the learning phase and delivering steadily before a test starts, holdouts are typically sized at 20-50% of the geo footprint, and we design every test to a minimum of 90% statistical power.** A test run while a campaign is still calibrating, or underpowered from the start, is going to produce a noisier signal no matter how clean the analysis. That is why our team runs a formal readiness check on every test before it launches.


When a brand's budget is well below the threshold for a conclusive test, we don't tell them to wait. We calculate the minimum daily budget a clean read actually requires, and work with them on the signals available in the interim: MTA trends, MMM reads, in-platform performance patterns, while getting them to a setup where an incrementality test can run cleanly. Between tests, we use those signals to maintain context, track directional changes, and build a more complete picture while working toward a clean incrementality read. MER, total revenue divided by total marketing spend, is the simplest of all of them: it does not care which channel gets credit, just whether the business is getting more back than it is putting in.


The most useful thing you can do with these instruments together is triangulate. When they all point in the same direction, you have high confidence. When they diverge, that divergence is telling you something specific about your measurement setup, your conversion path, or the moment in time you happen to be in. Neither outcome is a reason to distrust the channel. Both are reasons to understand it better. And in our experience, the brands that look closer tend to find more than they expected.
