---
schema_version: "1.0.0"
document_id: "95a3784aa4ab02dae8e00d8530e3b2136dbe630e3b8e0ce04548a9765e8dad5b"
company_key: "yc-mage-legal"
company: "Mage Legal"
source_id: "yc-mage-legal-news-import-389f7de0f02c"
canonical_url: "https://magelegal.com/blog/what-i-got-wrong-about-legal-ai"
published_at: "2026-04-26T00:00:00+00:00"
first_seen_at: "2026-07-22T03:05:30.029696+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:c446eb16054da97df1158c60db64b42cb281fd1a51a308f615c82e3f26560d77"
---

# What I Got Wrong About Legal AI

This is the reflective piece. Three things I said publicly or privately in 2023 about where legal AI was headed, what I got wrong, and what I think now. I'm writing it because I get asked the predictions question on roughly every other founder call, and giving the same forward-looking answers I gave three years ago would be dishonest.


## Wrong: better models would solve most of the quality problem


In 2023 I thought the central legal AI problem was model quality. The reasoning: GPT-3.5 and early GPT-4 were below what attorneys needed; the next generation of models would close the gap. The chassis around the model would matter less.


Partly right, mostly wrong.


The next generation did close most of the model-quality gap. Claude 3.5 and 4, GPT-4o and 5, Gemini 2.5 and 3 — these are the models the work runs on now, and they are good enough at language understanding that the model layer is no longer where most of the failure modes live.


What I missed: the chassis around the model became the differentiator the moment the model layer commoditized. Hallucination still happens; the architecture catches it or it doesn't. Multi-document reasoning still requires explicit structured-extraction passes; off-the-shelf RAG fails on amendment chains. Output voice is engineering work, not a model property. We covered this in[The F1 Engine Problem](https://magelegal.com/blog/f1-engine-problem) .


I was right that better models would help. I was wrong about the locus of the remaining gap. By 2026, two products running on the same frontier LLM produce dramatically different output because the chassis is dramatically different.


## Wrong: partners would adopt chat interfaces fast


In 2023 the consumer-AI mental model dominated. Everyone had used ChatGPT. The bet was that partners — busy, smart, time-pressured — would adopt a conversational interface for legal work the same way they'd adopted iMessage and Slack. Type a question, get an answer, move on.


What actually happened: partners adopted slower than I expected, and not because they were resistant to AI. They adopted slower because the conversational interface didn't fit how they actually work.


A partner's day on a deal isn't "ask a question, get an answer." It is: review the associate's memo, push back on three things, ask the associate to re-run the indemnity-package analysis with a different basket assumption, send a draft to the client, prep for the negotiation call. Conversational chat doesn't fit that flow. The shape of partner-grade work is iterative, document-anchored, and integrated into a multi-day workstream.


The tools that have won partner adoption in 2026 are the ones built around the workflow, not the chat. Mage's interface is workflow-shaped — by the time the partner logs in on Day 2, the work is partly done and the partner reviews findings rather than ask the system to find them.


I was right that AI would penetrate legal practice. I was wrong about the user experience that would carry it. The conversational pattern works for some workflows (research questions, drafting first cuts, brainstorming) and not for others (deal-team coordination, partner review of associate work). Two different products.


## Wrong: trust posture was a procurement checkbox


In 2023 I treated security and privilege posture as a procurement question. Get the SOC 2, write the no-training clause, sign the DPA, move on. The hard work was the product; the security work was a tax.


That was wrong by a lot.


What actually happened: trust posture turned out to be the rate-limiter on adoption velocity in serious M&A practices. Firms that took adoption seriously asked questions that went well past the SOC 2 report — DPA penalties for breach, single-tenant infrastructure, where data sits geographically, who at the vendor has access to what under what conditions, what happens to data on contract termination, whether the vendor has been pen-tested by an external party. The questions are not procurement questions. They are strategic questions about whether the firm can place privileged content on the platform at all.


Vendors who treated trust posture as strategic were the ones who got deployed first. Vendors who treated it as a checkbox stalled in legal review for months and lost the slot. We documented our specific posture on the[security page](https://magelegal.com/) ; the level of detail there is the bar.


I was right that security mattered. I was wrong about how much it mattered relative to product features. Trust posture is the gating constraint on adoption, not a procurement question. We treat it that way now. We didn't in 2023.


## What this means going forward


The honest reflection is that the predictions I made in 2023 had the right vector and wrong magnitudes. Better models, more adoption, more security work — all true. The relative weights I assigned were the part that didn't survive contact with reality.


What I am confident in now, with appropriate humility:


- **The model layer is commodity-trending.** The chassis is where the work is and will be.
- **Partner adoption is a leadership decision, not an interface property.** The tool that wins is the one that integrates with the firm's existing workflow and is championed by a credible partner.
- **Trust posture is strategic.** It gates everything else; it is not a checklist.
- **Specialist tools beat generalist tools on high-volume practices.** A general physician and a cardiac surgeon serve different needs.


What I am probably still wrong about: I don't know yet. The predictions I make today are probably similar in shape to the ones I was wrong about three years ago — the right vector, the wrong magnitudes. The forcing function is shipping product and watching it fail in instructive ways.


The companion reading, for whoever wants the long-form views on the topics above:


- [The F1 Engine Problem](https://magelegal.com/blog/f1-engine-problem) — chassis vs. engine
- [LLM Hallucination in Contract Analysis](https://magelegal.com/blog/llm-hallucination-in-contract-analysis) — chassis catching model failures
- [Why We Built Mage After Kirkland](https://magelegal.com/blog/why-we-built-mage-after-kirkland) — the longer origin story
- [Legal AI for M&A: The Practitioner's Guide](https://magelegal.com/blog/topics/due-diligence) — what I think about the category now


— Raffi
