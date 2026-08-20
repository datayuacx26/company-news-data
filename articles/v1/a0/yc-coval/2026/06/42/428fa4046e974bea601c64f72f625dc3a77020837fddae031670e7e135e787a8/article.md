---
schema_version: "1.0.0"
document_id: "428fa4046e974bea601c64f72f625dc3a77020837fddae031670e7e135e787a8"
company_key: "yc-coval"
company: "Coval"
source_id: "yc-coval-news-import-66f2770dd546"
canonical_url: "https://www.coval.ai/blog/voice-agent-vendor-testing/"
published_at: null
first_seen_at: "2026-07-24T15:17:27.086157+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:7d35cc62e6358c64e750c0061723734054f79839d861a8b6498feafbca4a8c1c"
---

# Voice Agent Vendor Testing: How to Run a Bake-Off

How long should a voice agent vendor bake-off take?


A focused bake-off runs in two to six weeks, not a quarter. The long pole is building the test suite, and mining production calls compresses that because you are categorizing real conversations instead of inventing scenarios. A multi-round bake-off that culls a wide field before a deep finalist round lands toward the higher end of that range; a tight head-to-head lands toward the lower end. Running the vendors is fast when the runs happen in parallel and the scoring is automated. The teams that drag a bake-off across months almost always do it serially, one vendor and one manual review at a time, and lose the deployment window. Treat parallelism and automated grading as part of the method, not a nice-to-have.


How many production calls do I actually need to mine?


Enough to be representative of your real traffic distribution, which usually lands somewhere between 1,000 and 10,000 calls depending on how varied your use cases are. A narrow, structured agent (appointment confirmations) needs fewer; a broad customer-service line needs more. The goal is not volume for its own sake, it is coverage: every meaningful use case and caller type should appear often enough to weight. If you have no calls yet, synthesize the test suite from domain expertise and the common caller archetypes, then backfill from production the moment you are live.


Should I let a vendor see the test set before the run?


Share the criteria and the categories, withhold the specific scenarios. Vendors should know you will grade order accuracy, escalation handling, and total-confirmation readback, because that lets them set up a fair like-for-like configuration. What they should not see is the exact test suite, because an agent tuned to a known set of cases scores high on those cases and tells you nothing about the rest. A useful middle ground: give every vendor a small public sample for setup and integration, and hold the scored test suite back until the run. The vendor learns how they did afterward, never what the test contained before it.


What if no vendor passes the bake-off?


That is a legitimate and useful outcome, not a failed evaluation. It means your requirements are ahead of what the market reliably delivers for your use case. From there you either revise the requirements with eyes open about the tradeoff, run a limited production pilot with the closest two vendors to see if real traffic changes the picture, or have a build-vs-buy conversation. A forced signature on a vendor that did not clear your bar is the most expensive outcome of all.


How is a bake-off different from a proof of concept?


A POC asks “can this vendor do the thing at all?” against that one vendor’s setup. A bake-off asks “which vendor does the thing best for my callers?” against an identical, shared test suite run across every contender. A POC can be useful as a pre-filter to decide who makes the finalist set, but it cannot pick a winner, because each vendor’s POC runs under its own conditions. The bake-off’s defining feature is that the conditions are held identical so the scores are comparable.


Can I reuse the bake-off test suite after I sign?


Yes, and reuse is the point, not a bonus. As the “what happens after you select a vendor” section above lays out, the selection test suite becomes your regression and production-monitoring suite the day the vendor goes live. The new mechanic post-signing is that the suite grows: every production failure you catch in monitoring gets turned into a new test case and added to the suite, so it becomes your institutional memory of every failure mode you have seen, and the agent gets re-checked against all of them before each release. A failure you ship once should never ship twice. Because the suite is vendor- and model-agnostic, it also survives a model swap or a vendor change without a rebuild. This is the coverage the[three-layer testing framework](https://www.coval.ai/blog/the-three-layer-testing-framework-for-voice-ai-regression-adversarial-and-production-derived) describes, now seeded from the selection work you already did.


Who should own the bake-off internally?


One owner with the authority to lock the criteria and weights, usually the head of product or the eval lead, with engineering supplying the integration work and a senior stakeholder reviewing the weighted result. The failure mode to avoid is a committee that reweights criteria after seeing scores, or a senior leader who sits out the criteria-setting and then vetoes the data on a single call. Decide who holds the pen on the weights before the run, and make the senior preference one weighted input rather than a late override.
