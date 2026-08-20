---
schema_version: "1.0.0"
document_id: "4fde5172fb339ea542edc47948573d86e47facc440d29f795d260c0feb7c47dc"
company_key: "yc-growthbook"
company: "GrowthBook"
source_id: "yc-growthbook-news-import-67cec260d3a3"
canonical_url: "https://www.growthbook.io/blog/edge-scaling-self-serve-experimentation-us-bank"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-13T02:36:13.694379+00:00"
fetched_at: "2026-08-13T02:36:16.762114+00:00"
content_hash: "sha256:45f463bf79e09a16bd2c2e80a8e0d8a514e11cac4abfbca8f1478361138a0e37"
---

# From bottleneck to self serve: scaling experimentation at US Bank

Running an experiment is the easy part. The hard part is running hundreds of them when every request routes through one central team, every customer interaction carries regulatory weight, and even a one percent gap in coverage is unacceptable.


That is the reality[Vijay Lal](https://www.linkedin.com/in/lal-vijay-331aa3178/) manages every day. As Lead Product Manager for Experimentation at[US Bank](https://www.usbank.com/index.html) , he sits at the intersection of marketing partners who want to test everything, engineers who have to keep a banking platform secure, and customers who expect their login page to work every single time. On this episode of The Experimentation Edge, he walked through how a regulated bank scales experimentation without sacrificing rigor, and what product managers everywhere can take from it.


## The bottleneck every experimentation program hits


Vijay's experimentation career started in 2016 at Comcast, after a redesign project for MassMutual convinced him that customer experience was where he wanted to build. At Comcast he supported sales and marketing teams running tests for prospective customers at enormous scale, on traffic volumes so large the platform itself had to be upgraded, with Adobe Analytics and Adobe Test & Target underneath.


Six and a half years later he moved to US Bank, into a similar role in a very different industry. Financial services is more regulated than telecom, and the stakes of a broken experience are higher. When money is involved, nobody gets to shrug off an error.


The problem he found is one nearly every experimentation program recognizes. Demand for experiments outgrew the central team's capacity to run them. Marketing partners wanted to test at a volume that Vijay's team could never cater to alone. Most organizations respond by hiring, queueing, or saying no.


## Self serve, with guardrails


Vijay chose a fourth option: make the platform self serve. In his words, why not make it so that anyone who does not know anything about technology can start using the platform and run experiments for customers?


That decision sounds simple. Executing it is not, and Vijay was clear about where the real work lives:


**Step 1: Simplify the complex.** An experimentation platform is not a simple tool. A friendly UI does not make experiment execution easy for people who are not proficient with the technology. The platform team's job is to compress that complexity into something a marketer can safely operate.


**Step 2: Train continuously.** Technology evolves every month. New capabilities, platform upgrades, tool changes. Self serve only works if the people using the platform stay current, so training is not a launch activity, it is a standing commitment.


**Step 3: Attach responsibility to capability.** Handing someone the power to put changes into a customer facing production environment is not a small thing. Every self serve user needs to understand the implications of what they ship, and guardrail metrics need to catch what they miss.


That last point is the one experimentation leaders should sit with. Democratization is not just access. It is access plus guardrails plus the knowledge of what the implications will be. Get all three right and a small central team can support an enormous testing program.


## Every customer accounted for


The best story of the episode is about a login widget, and it shows what experimentation discipline looks like when the stakes are real.


US Bank's login widget historically loaded after the page finished loading. Vijay's team wanted it embedded, loading with the page, for a faster and cleaner experience. The complication: the login widget is one of the most secured components a bank has, because customers enter their user ID and password into it. Rebuilding how it loads meant working through security constraints, multiple technology partners, and an architecture where the component is delivered separately as an experience fragment.


Then testing surfaced a harder problem. For customers on slower internet connections, the embedded widget might not load properly. Most teams would look at the percentage affected and call it acceptable. Vijay's team did the opposite. In his words, every customer matters. Even if five percent of customers cannot see an experience, that is a big deal, and slippage of one or two percent is not acceptable.


The solution was elegantly simple: a fallback. Any customer whose page did not load within two seconds got a front end that looked the same, with the login widget displayed slightly differently, so it always worked. Every customer was accounted for in either control or challenger. No one fell through the cracks, the experiment stayed clean, and the security bar never moved.


There is a lesson here that goes beyond banking. Your users are on fast connections and slow ones, new devices and old ones. An experiment that only works for the top deciles of performance is not a complete experiment. Designing the fallback is part of designing the test.


## Hypothesis first, metrics second


The third theme of the conversation is about how experiments get measured, and Vijay drew a sharp line between two approaches.


The traditional approach: a leader wants to run an experiment or ship a product, and a KPI gets attached to justify it. The modern approach: every experiment starts with a hypothesis, and the hypothesis drives the metrics. Not one metric in a silo, but a primary KPI for the immediate behavior, such as engagement on first visit, paired with secondary KPIs for what happens later, such as whether the customer is still engaged when they return.


Why does the order matter? Because metrics without a hypothesis can tell you what happened but never why. Vijay's example: data analysts can see customers rage clicking on a page and revenue falling. What the dashboard cannot say is that the button is disabled and the customer expects it to work. The hypothesis is what connects the number to the behavior behind it.


This also connects to a point Ashley raised about metric distance. The further a metric sits from the feature, the more noise other factors introduce. A North Star metric like revenue matters, but the experiment needs a goal metric close to the feature's immediate behavior, plus guardrail metrics to catch degraded experiences across segments, devices, and platforms that a topline average would hide.


## What this means for your experimentation program


Vijay's approach at US Bank compresses into four moves any team can apply:


1. **Make the platform self serve** so your central team stops being the bottleneck for testing velocity.
2. **Pair capability with guardrail metrics** so non technical users can ship safely to production.
3. **Design for every customer,** including the slow connections and edge cases, with fallbacks built into the experiment itself.
4. **Start from the hypothesis,** then derive a primary KPI and secondary KPIs, rather than letting a leader's preferred number define the test. None of this requires a bank's budget. It requires treating experimentation as a product with users of its own, and treating rigor as non negotiable even when only one percent of customers is on the line. Especially then.


Ready to take control of your experimentation program?[Listen to the full episode of The Experimentation Edge](https://www.youtube.com/watch?v=myw3SdPguHk)
