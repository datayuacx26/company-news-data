---
schema_version: "1.0.0"
document_id: "ce9fbde67423bad216bbf0dd258b6f795216f31bef47ab38a7f3bc688b9a4b8b"
company_key: "yc-ai-2"
company: "&AI"
source_id: "yc-ai-2-rss-536318f0f92f"
canonical_url: "https://www.tryandai.com/blog/ironburg-valve-skilled-searcher-ipr-estoppel"
published_at: "2026-07-03T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:52.502979+00:00"
fetched_at: "2026-07-28T20:47:34.280666+00:00"
content_hash: "sha256:3ca906f289ecfae04bf1fad3c7b484b3701a158cefd0272e043a91dd76a49ceb"
---

# Ironburg v. Valve: Skilled-Searcher IPR Estoppel

[Back to insights](https://www.tryandai.com/blog)


# Ironburg v. Valve: Skilled-Searcher IPR Estoppel


July 3, 2026


[Caleb Harris](https://www.linkedin.com/in/caleb-m-harris/)


IPR estoppel turns on a counterfactual. Under 35 U.S.C. § 315(e)(2), a petitioner cannot later assert invalidity grounds it "reasonably could have raised" during an inter partes review, and courts test that with the skilled-searcher standard: would a diligent searcher have found the art before the petition? In *Ironburg Inventions Ltd. v. Valve Corp.* , No. 24-2088 (Fed. Cir. June 18, 2026), the Federal Circuit did not rewrite that standard. It clarified the evidentiary showing the standard demands, and reversed two estoppel findings against Valve because that showing was missing.


The opinion turns on three ideas kept apart: whether a reference was accessible, whether it was findable, and whether the invalidity ground built on it would reasonably have been discovered. Section 315(e)(2) reaches grounds that reasonably could have been raised before the petition, not art that merely sat in a searchable database.


## Stark's two-step architecture


Judge Stark joined the opinion and concurred separately with the cleanest map of the inquiry. Building on *Ingenico Inc. v. IOENGINE, LLC* , 136 F.4th 1354 (Fed. Cir. 2025), which tied estoppel to invalidity grounds rather than references, he frames the patentee's burden in two steps. First, were the references findable by a skilled searcher conducting a diligent search? Second, would that searcher reasonably have discovered the invalidity ground built on the references found? Proving a reference findable does not prove the ground discoverable. The question is always which one is on the table: finding Kotkin, or finding the Kotkin invalidity ground.


A further line sits inside step one. A reference can be accessible without being findable: a classification search that also returns tens of thousands of other references makes a reference accessible, not reasonably findable. Accessibility is the floor. Findability is what the estoppel proponent must prove.


## Volume isn't findability


That is where Ironburg's case broke down. It relied on a classification search returning 26,333 references and argued the key reference, Kotkin, was in the set. A classification search alone, producing an unreviewably large result set with no evidence of how a diligent searcher would narrow it, is not enough; "something more" is required. What the court did not say matters too: it did not hold that a 26,333-reference search can never count as diligence, that manual review is always required, or that Valve's proposed standard was correct. A raw hit list may prove accessibility; without narrowing, reviewability, or workflow, it does not prove findability.


## Hindsight stays fatal


The three-reference obviousness combination failed differently. To prove it discoverable, Ironburg leaned on 2023 searches that used forward-and-backward citation searching with no date restrictions. Run that way, a citation search walks outward from a known reference to everything connected to it, including art that postdates the petition, such as a 2023 Ironburg design patent that cited the target reference because later litigation had surfaced it. The court drew the corollary explicitly: evidence that shows only what a searcher could find after the petition date is likely irrelevant to what could have been raised before it. A belated fix, date filters applied to a single struck search string, was "too little too late," and a supplemental search run only because a reference "had not yet been found" was hindsight by admission.


## The burden point


None of this shifts the burden. The party asserting estoppel carries it, and in the common posture that is the patent owner. *Ironburg I* held as much, and the remand kept it there: Ironburg had to prove Valve's grounds reasonably could have been raised, and on this record it could not.


## What it means for AI-era search


Read forward, and as implication rather than holding: Ironburg says nothing about semantic search. But the skilled-searcher standard is defined by the tools a competent searcher would use, and those tools are changing. As semantic and embedding-based retrieval becomes ordinary practice, surfacing conceptually related art that keyword queries miss, litigants will fight over three things: whether a diligent searcher would have used those tools at the relevant petition date, how the corpus was defined, and whether the path to the reference, and then the ground, is reproducible. That is a more honest reading than any claim that AI has already widened what estoppel can bar.


## Building the record


The operational lesson is about the record, not the tool count. Petitioners should preserve the search itself: databases searched, date cutoffs, classification paths, keywords, semantic concepts, citation-search settings, result counts, narrowing steps, and the point at which the set became reviewable. For patent owners asserting estoppel, the lesson is symmetrical. Do not show only that the reference sat in a database; show the reproducible path a diligent searcher would have taken to the reference, and then to the ground.


## Where &AI fits


We did not build[&AI Search Agent](https://tryandai.com/product/search) to "solve" Ironburg. No tool preserves grounds on its own; petition strategy, stipulations, invalidity contentions, and record development are counsel's work. What the agent does is produce the record the decision rewards.


Most "AI search" tools run one query, rank the results, and stop. Search Agent is fully agentic, running the toolkit a senior searcher would reach for: semantic and boolean search over patent databases, citation graph traversal, prosecution history analysis, web-based strategies, and more, across global patents and applications, non-patent literature, clinical trials, system art, and infringing products in view of a target's claims.


It is auditable by default, which is the part that matters after Ironburg. A search result you cannot defend is not a search result, so Search Agent surfaces every step: which tool it called, which query it ran, which references it pulled, and which it discarded and why. You can audit the trace, redirect the agent, or hand the session to a colleague who picks up where you left off. That trace is the reproducible, date-disciplined path between "accessible" and a find you can sign your name to.


When an external search costs weeks and thousands of dollars, you have to be right on the first pass, so teams keep scope narrow and rarely course-correct. Search Agent compresses that loop to minutes at a fraction of the cost, so scope can be broad and revisited. On internal evals against IPR-instituted prior art, it returns a family match in the top five in 80% of cases and an exact match in 72%. More on the evals soon.


[Try searching today with a 7-day free trial](https://platform.tryandai.com/sign-up) , credits included. See[pricing](https://www.tryandai.com/pricing) or[book a demo](https://www.tryandai.com/book-demo) .


## Frequently asked questions


What did the Federal Circuit decide in Ironburg v. Valve?


In Ironburg Inventions Ltd. v. Valve Corp., No. 24-2088 (Fed. Cir. June 18, 2026), the court did not rewrite the skilled-searcher standard for IPR estoppel under 35 U.S.C. § 315(e)(2). It clarified the evidentiary showing the standard demands and reversed two estoppel findings against Valve because that showing was missing.


What is the skilled-searcher standard for IPR estoppel?


Section 315(e)(2) bars a petitioner from later asserting invalidity grounds it "reasonably could have raised" during an inter partes review. Courts test that with the skilled-searcher standard, which asks whether a diligent searcher would have found the relevant art before the petition was filed, not merely whether the art existed in a searchable database.


What is the difference between accessibility, findability, and discoverability?


Ironburg keeps three ideas apart. Accessibility is the floor: a reference merely sitting in a searchable set. Findability is whether a skilled searcher conducting a diligent search would actually have surfaced that reference. Discoverability goes further, asking whether the invalidity ground built on the found references would reasonably have been discovered. Proving a reference findable does not prove the ground discoverable.


Why did Ironburg's 26,333-reference classification search fail?


The court held that a classification search alone, producing an unreviewably large result set with no evidence of how a diligent searcher would narrow it, does not prove findability; "something more" is required. A raw hit list may prove that a reference was accessible, but without narrowing, reviewability, or workflow it does not prove the reference was reasonably findable.


How does hindsight affect a prior art search in an estoppel analysis?


Evidence showing only what a searcher could find after the petition date is likely irrelevant to what could have been raised before it. In Ironburg, undated forward-and-backward citation searches run in 2023 swept in art that postdated the petition, and a supplemental search run only because a reference "had not yet been found" was treated as hindsight by admission.


What does Ironburg mean for AI and semantic prior art search?


The opinion says nothing directly about semantic search, but the skilled-searcher standard is defined by the tools a competent searcher would use, and those tools are changing. As semantic and embedding-based retrieval becomes ordinary practice, litigants will contest whether a diligent searcher would have used those tools at the relevant petition date, how the corpus was defined, and whether the path to the reference and then to the ground is reproducible.


## Related insights


[Best AI Prior Art Search Tools for Patent Litigators (2026) Compare the best AI prior art search tools for patent litigation in 2026 — coverage, citations, charting, and how AI stacks up against search firms. June 17, 2026](https://www.tryandai.com/blog/best-ai-prior-art-search-tools)[Zip Top v. SC Johnson & Son: Another Familiar Claim Construction Reminder from the Federal Circuit The Federal Circuit declined to let unclaimed embodiments broaden the scope of a "spout" limitation. January 21, 2026](https://www.tryandai.com/blog/zip-top-v-sc-johnson)
