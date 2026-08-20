---
schema_version: "1.0.0"
document_id: "5e185160c50f02edbd75b490f9b79eb372142255c4e8048c53184ba9d84344b2"
company_key: "yc-escape"
company: "Escape"
source_id: "yc-escape-rss-d1370620bfa7"
canonical_url: "https://escape.tech/blog/modern-ai-powered-pentesting-tools-in-depth-benchmark/"
published_at: "2026-06-30T16:18:27+00:00"
first_seen_at: "2026-07-20T23:23:55.281376+00:00"
fetched_at: "2026-07-28T20:47:51.174373+00:00"
content_hash: "sha256:e0face714dacbc3058865d4cd72f7b3d536c1637811e5813eabd510b3b2ba4c1"
---

# Modern AI-powered Pentesting Tools In-Depth benchmark

If you're evaluating AI for offensive security right now, you're weighing two questions at once. The first: why not skip the tooling and point a frontier model at your apps yourself? The second: among the AI pentesting tools you could actually buy, which one earns the budget?


We built this benchmark to answer both. Cascade, Escape’s multi-agent AI pentesting engine, went up against a single frontier model (Claude Opus 4.8) driven directly at the same targets. The only variable is the harness around them. That's where this gets interesting, so that's where we spent our time.


We also compare the results with two other AI pentesting platforms, Aikido and XBOW, using the independent[measurements Doyensec published in their recent comparison](https://doyensec.com/resources/ComparingAIApplicationSecurityTestingPlatforms_Doyensec.pdf) that used the same target applications.


## What this benchmark is and what it isn't


Before we dive in, a note on where the numbers come from:


- **Cascade vs the Claude Opus 4.8 is a controlled comparison.** Both are run by us, on the same targets, in the same information conditions, with the same validation standard and the same person assigning severities. The delta between them is internally valid, it's the core result of this paper.
- **Cascade vs Aikido and XBOW is an external reference, not a controlled test.** Those two tools were[measured by Doyensec](https://doyensec.com/resources/ComparingAIApplicationSecurityTestingPlatforms_Doyensec.pdf) , on their infrastructure, in March–April 2026 and **their findings were validated and re-severitized by Doyensec's reviewers.** Cascade's findings in this round are Escape-reported and Escape-validated. Any number that sets Cascade beside Aikido or XBOW is grounded in this point of reference.


This paper synthesizes both studies.


## Background


This is a build-vs-buy benchmark, because that's the comparison we see most in proofs of concept.


When a team evaluates[AI pentesting tools](https://escape.tech/blog/best-ai-pentesting-tools/) today, the alternative on the table is sometimes another vendor, but more and more often the question is "can we point a frontier model at it ourselves, with our own business context, without spending hundreds of dollars a day on tokens?"


That instinct is reasonable. The models get better every month. This benchmark answers two questions security teams ask when evaluating AI pentesting:


**1) Should you build on a frontier model internally, or buy a dedicated solution?** To find out, we ran Claude Opus 4.8 directly against each target (no agent harness, no cross-asset memory, no orchestration) and compared it against[Escape's AI pentesting solution "Cascade"](https://escape.tech/blog/introducing-cascade-the-multi-agent-penetration-testing/) , our multi-agent multi-model system that plans, holds context across an engagement, drives authenticated multi-persona flows, validates findings, and routes each step to whichever model handles it best. Both ran in black-box and white-box mode. The delta between them is entirely down to the harness.


**2) If you buy, which tool gives the best coverage?** To answer that without self-reporting, we reused target applications from Doyensec's independent benchmark of AI pentesting platforms, so Aikido and XBOW figures are their measurement, not ours, and our numbers sit next to a methodology people already trust.


[Discover another benchmark comparing Escape vs various open-source AI pentesting tools (Shannon, Strix, PentAGI)](https://escape.tech/blog/benchmarking-agentic-ai-pentesting-tools/)


## Target applications


We tested four apps, picked to separate discovery from recall.


1. [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/) **.** A lot of evaluators ask for it, so we included it, but **we would absolutely not consider it primary evidence** . Juice Shop is **deliberately vulnerable and has been documented in thousands of write-ups** , which means it lives in every frontier model’s training data. A model can recall its vulnerabilities as easily as find them. We report the numbers and exclude the app from any conclusion.
2. [Duck Store.](https://duck-store.escape.tech/) Because Juice Shop is contaminated, we built our own deliberately vulnerable app that no model has memorized. **It produced one of the benchmark’s revealing moments: the lone model was observed reaching for public write-ups mid-run instead of working the application. It still tried to look up the answer instead of breaking app itself.**
3. [Fider](https://github.com/getfider/fider) **and**[Photoview](https://github.com/photoview/photoview) **.** Two real open-source applications (a feedback tool and a photo gallery) shared with Doyensec’s study. Real code, in active use, with no hundreds of published vulnerability walkthroughs to lean on. These are the apps that carry the comparison.


So the four apps form a gradient: from “the answers are everywhere online” (Juice Shop), through “the model will go looking for them” (Duck Store), to “there’s almost nothing to look up” (Fider, Photoview).


## Test environment setup


Each app was tested with both Cascade (multi-model) and Claude Opus 4.8, and each of those in two information conditions: black-box (work the running application, no source) and white-box (same target, repository provided).


On Fider and Photoview, we added Aikido and Xbow as external references. Findings on Photoview were deduplicated before counting, and the same rule should be applied to every tool before anything is published.


## What the Cascade harness adds


Before the numbers, it’s worth being precise about what the harness is because the evaluation that follows is really a test of these four capabilities, none of which come from a bigger model.


Standalone model’s capabilities for security are improving. However, using them for autonomous security assessments without dedicated harnesses quickly hit critical roadblocks in practice. Misclassification of severity, parts of the target arbitrarily neglected during the assessment, instructions following collapse for large targets: raw models without harness struggle to keep the quality, exhaustivity, coverage and control that security teams require to have an output that is actually actionable. . The Cascade harness supplies exactly those missing parts:


Cascade's architecture


**Multi-model orchestration:** different models are routed to planning, reasoning over code, and generating exploit attempts, instead of one model doing everything in a single thread. In the data, this is what lets the system keep producing on long engagements where a single context window stalls.


**Persistent context:** what’s learned in one part of an engagement carries into the next, so coverage accumulates rather than resetting each run. Escape’s attack surface management feeds it the map of what’s exposed before testing starts. This is what should show up as the system finding more on novel apps, where there’s nothing to recall and everything has to be discovered and remembered.


**Authenticated, multi-persona testing:** logging in as different users and pivoting between them is how authorization flaws like BOLA and IDOR surface, and a stateless prompt cannot drive those flows. This is the capability we expect to see in the high-severity column specifically.


**Exploit validation** : findings are checked and carry reproduction evidence, which is what lets a reviewer act on them without re-verifying by hand. This is what should keep the harness’s reported findings from being padded with noise, the failure mode we’ll see in some of the other tools.


None of this is exotic. It’s the engineering most teams would try to end up rebuilding themselves the moment they tried to turn a model into a tester. The key questions remain **efficiency, costs, and maintenance.** The benchmark below is structured to show each of the harnesses' capabilities leaving a fingerprint in the results.


## Findings evaluation


We evaluate on three axes, in deliberate order of how well each one resists gaming. Total findings are the easiest number to inflate, since a tool can pad them with informational noise. Severity distribution is harder to game. High-severity count is hardest of all. So we read the table left to right but weight the conclusions right to left.


#### The full results matrix


Number of true positive findings for all severities per app - before severity readjustments App Claude Opus
4.8
(black-box) Claude Opus
4.8
(white-box) Cascade
(black-box) Cascade
(white-box) Aikido Xbow


Juice Shop 23 24 36 49 - -


Duck Store 18 17 19 20 - -


Fider 7 7 26 28 17 24


Photoview - 8 12 28 32 7


### **Axis 1: the harness multiplies the same model**


Wrap the same models in the harness and the output changes by a multiple. On the two real apps (Fider and Photoview) the Claude Opus 4.8 alone returns 7 and 8 findings (white-box); inside Cascade’s harness it returns 26 & 12 (black-box) and 28 and 28 (white-box). That’s roughly four times the yield from the engineering alone, since the underlying models are the same.


Harness' impact on findings. On novel code the harness produces ~4× the findings. The cream band marks apps whose vulnerabilities are publicly documented, where the bare model can lean on recall.


The contamination gradient is why the comparison is fair rather than flattering. On Juice Shop and Duckstore (where the answers are public) the bare model keeps pace. We’ve seen in agent reasoning that the model accessed the results. That’s the recall effect: the model isn’t discovering those vulns so much as remembering them. So if anything, the bare-model column is inflated in the documented zone, which means the harness’s true contribution is understated there and only shows its real size once recall stops helping on Fider and Photoview. The harness, not the model, is what carries novel code.


The reading is narrow and specific: a Opus 4.8’s apparent skill on familiar benchmarks is substantially recall, and it generalizes poorly to code it hasn’t seen written up. That’s not a knock on the model, it’s the reason the harness exists. T **he persistent context and orchestration described above are precisely what let the system keep discovering on an app it has never seen, where the model alone has nothing to fall back on.**


### Axis 2: weighting by severity answers that question


Raw counts treat a critical and an informational note as one unit each. They aren’t. Splitting the clean-app results by severity changes the picture for every tool:


Severity breakdown


### Validated findings


True positives, false positives, and high-severity counts after manual validation. Aikido & XBOW adjusted as per Doyensec Severity review.


Photoview


Photoview — validated findings by tool Tool True positives False positives Critical & High-severity Medium severity


Aikido 32 0 6 9


Xbow 7 0 2 2


Cascade (white-box) 28 0 7 9


Fider


Fider — validated findings by tool Tool True positives False positives Critical & High-severity Medium severity


Aikido 17 2 3 6


Xbow 24 1 3 7


Cascade (white-box) 28 0 4 12


Two things surface that the totals bury. First, the headline totals carry informational padding: Xbow’s 24 on Fider includes 6 informational findings, and Aikido’s 32 on Photoview includes 8. Strip the informational tier and Fider reads Cascade 28, Xbow 24, Aikido 17; Photoview reads Cascade 28, Aikido 24, Xbow 5. Second, and we’ll say it plainly, Aikido’s larger Photoview total is real. It reported more findings there than Cascade did. The question is what those findings are worth.


To collapse the noise problem into one number, score each tool by severity: Critical and High×5, Medium×2, Low×1, informational findings count for nothing. The weights are arbitrary, but the ranking is not: it holds under any scheme where High > Medium > Low:


Severity weighted adjusted score


This is the cleanest single read in the benchmark. On Fider, Cascade scores 55 against Xbow’s 37 and Aikido’s 35.


On Photoview, the app where Aikido out-counted Cascade on raw findings, the weighted score reverse the order, 65 to 57, because Aikido’s extra findings were low-severity and informational. This isn’t only our read: in Doyensec’s independent validation, 11 of Aikido’s 32 Photoview findings had their severity downgraded, most of them to informational. Those are real findings, not false positives, but they’re simply low-value, and the weighting is what stops a raw count from rewarding them. Volume and security value are not the same axis, and the gap between them is where a finding count misleads.


### Black-box vs white-box


A second result falls out of the same data, and it matters for any team that can’t share source: vendor code, IP restrictions, a policy that says external testing doesn’t read the repo.


We compared results across multiple severities for Cascade's black-box and white-box findings:


Black box vs white box Cascade comparison


Run Cascade black-box, with no source at all, and the high-severity findings barely move versus white-box: Duckstore 14 → 15, Fider 4 = 4, Photoview 7 = 7. Source access does add findings — Photoview’s total climbs 12 → 28 — but that uplift lands in the medium and low tier, the code-path-dependent issues you reach by reading a function. The criticals are already caught from the outside. So black-box isn’t the compromised mode; it’s the realistic one — the same view an attacker has — and on these apps it didn’t miss a single high-severity bug that source access would have caught.


## Time, delivery, and signal quality


Finding counts are only half of what a security team buys. The other half is how long the assessment takes and how much of the output is trustworthy enough to act on without re-checking. For the two commercial platforms we can report this directly, because Doyensec measured it in their independent study; the figures below are theirs, from their environment in March–April 2026.


Metric Aikido Xbow Cascade Claude


Setup Under 20 min Several days,
sales-gated Under 15 mins Under 15 mins


Scan time ~8 hours per app Fider >1 week
(crashes,
restarts);
Photoview ~2
days ~1.5 hours per
app ~1 hour per app
(no harness)


Report delivery Immediate Up to 5 days after
scan Immediate Immediate


False-positive rate
(validated) total across
Fider and Photoview 4% (2/49)
2/19 fider 3% (1/31) 0% (38 black box,
56 white box) 0/7; 0/8


Aikido and Xbow figures are from Doyensec’s independent study


Two things stand out, and both are worth stating plainly. First, on precision the commercial platforms did well: an independent Doyensec study confirmed roughly 96–97% of their findings as real, with only a handful of false positives between them.


Second, operational cost varied sharply: **Escape and Aikido set up in minutes and reported in hours,** while XBOW's run on Fider took over a week with crashes, restarts, and more than twenty support emails.


Read across all three measurement views, Cascade is the only tool that stays in the top tier on both clean apps: raw count, severity-weighted score, and high-severity alike. The others each have a soft spot. Xbow is strong on Fider but thin on Photoview. Aikido leads Photoview on raw count, but slips behind once findings are weighted by severity, a third of its Photoview findings were independently downgraded. The Claude Opus 4.8 only keeps pace where the answers are already public. None of these are bad tools; the point is that each is uneven across app types, and Cascade is the one that isn’t.


For a workload meant to run continuously against whatever an engineering org ships next, that consistency is the property worth weighting, more than any single app’s leaderboard, where the order changes depending on which app and which metric you pick.


## Conclusion


Everyone evaluating new frontier models, and on a benchmark those models have seen, they look strong. Point them at an application with no published walkthrough, and they find a fraction of what’s there and almost none of the high-severity issues.


The harness and understanding business context is what closes that gap, and the benchmark shows each piece doing its job: orchestration and persistent context carry the discovery on novel code, authenticated multi-persona testing produces the high-severity authorization bugs, and validation keeps the findings free of the noise that pads the other tools. That’s the real answer to "why not just do it ourselves", the model is the easy part to acquire and the hard part to turn into a pentester. At scale. In an organization. Without the need for maintenance.


If you want to see how it performs on your codebase,[book a demo with the Escape team.](https://escape.tech/book-a-demo)


**Access to Anthropic’s Fable 5 and Mythos 5 have been restricted by request of the US Government, thus we haven’t been able to include them in the benchmark yet. We plan to update those benchmarks once we get back access to the related models.**


💡 **Want to learn further how Escape applies AI to automated pentesting?** Explore these guides to learn more about novel vulnerabilities and optimize your workflows:


- [Two Critical Vulnerabilities, One AI Pentester: How Cascade Found an Unauthenticated RCE and Walked Around the WAF](https://escape.tech/blog/how-ai-pentesting-found-an-unauthenticated-rce-and-walked-around-the-waf/)
- [How Escape AI Pentesting Exploited SSRF in LiteLLM](https://escape.tech/blog/how-escape-exploited-ssrf-in-litellm/)
- [Apple's App Store Source Map Leak: A Preventable Vulnerability We Found in 70% of Organizations](https://escape.tech/blog/apple-app-store-source-map-leak/)
- [How one of the leading FinTech platforms used Escape to automate business logic security testing at scale](https://escape.tech/blog/fintech-platform-ai-pentesting-customer-story/)
