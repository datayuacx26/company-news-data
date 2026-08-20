---
schema_version: "1.0.0"
document_id: "f60e9cc9e35ec7fb172210835db5781ba052254cfba2ce51be0062b7a048ebfc"
company_key: "atomera-incorporated-common-stock"
company: "Atomera Incorporated"
source_id: "atomera-incorporated-common-stock-rss-b90baaae6aaa"
canonical_url: "https://atomera.com/how-mst-adoption-works/"
published_at: "2025-04-08T00:44:06+00:00"
first_seen_at: "2026-07-25T01:07:25.629641+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:753400777131fb7d0d02927cc7c08ef4566ee4335abef8857bfbe01b1a78f0f5"
---

# How MST Adoption Works

Recently an Atomera investor queried generative AI chatbot *Grok* with the following question:


***How big a performance improvement is necessary to see MST adoption?***


The AI answer was actually a pretty good high-level summary of how the industry views this subject. In practice however, the decision-making process is more nuanced. I sent along an answer to the investor with some insight into that process and wanted to share with a wider audience. I hope this is helpful:


**Grok’s Response:**


*In the semiconductor industry, the threshold for performance improvement to drive adoption isn’t a fixed number—it’s a sliding scale shaped by cost, complexity, and market pressure. That said, historical trends and industry benchmarks give us a rough yardstick for what’s “big enough” to move the needle.*


*Traditionally, a full process node transition (e.g., 7nm to 5nm) delivers about 15-30% performance improvement (transistor speed or power efficiency), 20-40% power reduction, or 50-60% area scaling, depending on the metric and manufacturer. These leaps justify the massive R&D and fab retooling costs—often billions of dollars—because they keep Moore’s Law limping along and meet demands from high-stakes markets like mobile, AI, and automotive. For a drop-in technology like Atomera’s MST, which doesn’t require new equipment or a node shrink, the bar is lower but still significant. It needs to offer enough of a boost to outweigh integration effort, licensing fees, and the risk of disrupting finely tuned processes.*


*A 10-15% improvement in performance or power efficiency is often the minimum to get serious attention. Why? That’s the point where it starts rivaling the gains from tweaking existing processes (like optimizing doping or gate materials) without the headache of a full redesign. For example, when strained silicon debuted in the early 2000s, it delivered ~10-25% performance uplift and became ubiquitous because it slotted into CMOS workflows with minimal disruption. High-k metal gates later offered similar power and leakage improvements—around 20-30%—and saw broad uptake for the same reason. MST claims up to a full node’s worth of gain (15-30%), which sounds impressive, but the real-world impact varies by use case and process.*


*Context is everything. For mature nodes (28nm and above), where scaling’s stalled, a 10-20% boost could be a lifeline—think IoT or analog chips where cost trumps bleeding-edge density. Foundries like GlobalFoundries, stuck at 12nm, might bite if MST delivers 15% without new tools. But for leading-edge nodes (5nm, 3nm), where TSMC and Intel are already squeezing out every ounce of efficiency with gate-all-around or backside power delivery, MST’s gains need to stack competitively—closer to 20-25%—or offer unique perks (e.g., variability reduction) to justify a shift. Niche markets like power electronics (GaN) might adopt at even lower thresholds—say 10%—if MST cuts costs or simplifies scaling.*


*Adoption also hinges on trade-offs. A 15% performance bump might not cut it if yields drop 5% or if validation takes a year—fabs live and die by reliability and time-to-market. Conversely, if MST delivers 10% but slashes power enough to win a big 5G contract, that’s a green light. Data from Atomera’s trials (e.g., with TSI Semiconductors) suggests 10-20% gains in test structures, but full-chip results under production stress are less public. If those hold up at scale, it’s in the sweet spot.*


*So, the magic number? For broad adoption, 15-20% improvement in performance or power—sustained in mass production—is a safe bet. Less than 10%, and it’s a tough sell unless it’s dirt cheap and seamless. Above 25%, and it’s a no-brainer, inertia be damned. MST’s likely in that 10-20% zone, which puts it on the cusp—enough to tempt early adopters, but it’ll need flawless execution and a few marquee wins to go mainstream. The industry’s a graveyard of “good enough” ideas that didn’t scale; MST’s fate rests on proving it’s not just another one.*


**My Response:**


A majority of Grok’s write-up is pretty accurate, but there are a few characterizations I would dispute:


1. The first few paragraphs describing the “sliding scale” are generally accurate. Moves to newer nodes are not delivering the same gains that older nodes delivered, but they still obviously bring enough benefit to justify the high cost. For MST in mature nodes, the bar for adoption is considerably lower – even lower than the 10-15% which the write-up suggests. In licensing discussions we show customers this in great detail for their specific process. I have been talking about this for some time as a phenomenon I call “bad math”: *[Bad Math: It’s Time to Rethink Economics in a Post Moore’s Law Era](https://magazine.semiconductordigest.com/html5/reader/production/default.aspx?pubname=&edid=3f1888bd-158d-4d3e-a3e1-aa79f8feece6&pnum=20)*
2. The discussion of Strained Si and HKMG is accurate, but suggests they were easily developed and adopted, which was not the case. Each of them cost over a billion dollars to develop and after a long time were put into production (I think in both cases by Intel) before being widely adopted. This is the domino effect I’ve spoken about before.
3. The goal posts it talks about for the leading edge are completely inaccurate. The Grok analysis assumes MST alone would be added as the new technology for a leading-edge node. Leading-edge nodes would adopt MST for a variety of reasons:


- Since leading-edge nodes already use a substantial amount of epitaxy in production, **adopting MST does not mean adding new equipment** . The customer would only have to add MST while the wafer is already in the Epi tool, lowering the cost of MST significantly and making it much easier to add to their production flow. In HVM, it may mean they have to use more Epi tools, but that is natural for any product ramp. The variable cost (without royalty) of adding MST in this case becomes almost insignificant.
- **It improves their yield** : Imagine a foundry is selling wafers for $18k. Now imagine that the adoption of MST could improve yield by 1%. Simple math says that they should be willing to pay anything less than $180 per wafer for MST. Of course, we would charge much less than that and hopefully bring more than a 1% yield improvement.
- **MST would most likely be one ingredient that would help make their overall new node successful** : Certainly, they would want to see a 15-25% improvement for the whole node, but they would not demand the whole improvement be attributed to MST alone. It will be one of hundreds of changes that will add up to the whole number. MST would likely be a technology that enables many of the other improvements to be more successful.


4. Overall, Grok’s summary paragraph is misleading. For legacy nodes, customers will typically adopt MST as a new technology variant on an existing node (maybe in conjunction with a half node shrink for example). Thus, they will typically be looking for a 10-20% improvement for the OVERALL technology variant. MST will provide a big chunk of that improvement, but other things will be changed as well (like optimized implant or other design tweaks) to reach the final number. Therefore, the entire cost of the new variant development will also not be attributed to MST.


The improvement numbers in Grok’s write-up are not that different from what we hear from customers in our first meetings (bad math). They are typically focused on improving a specific figure of merit dependent on the application and if we can help them get there, they are likely to adopt as part of a new product variant. We then explain the low cost of implementing MST to derive an economic benefit. However, in most cases, customers still want to do experiments in their own fab to validate the benefit and understand any impact to reliability, yield, etc. before deciding. This takes time.


**In Summary** :


The Grok write-up is seemingly accurate in theory, but in practice customers do not make the decision on such high-level performance hurdles.
