---
schema_version: "1.0.0"
document_id: "ece79d0b8c38d58ef09313c11edb67966cfe8024021fcfe0e605b1f0811b763d"
company_key: "yc-pibit-ai"
company: "Pibit.ai"
source_id: "yc-pibit-ai-news-import-b491cbbe82b1"
canonical_url: "https://pibit.ai/blog/underwriting-ai-authority-not-extraction-problem"
published_at: "2026-07-06T00:00:00+00:00"
first_seen_at: "2026-07-22T08:59:11.961361+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:74b9c69129a8ee8bdc8a725e2676c78f31818238994dba61a863d50cd032cc35"
---

# Underwriting AI doesn't have an extraction problem. It has an authority problem.

**Most underwriting AI pilots answer the extraction question, then run straight into the authority question. Can the model read a submission package? Usually yes, at least well enough to get attention. That still does not tell you the thing that matters, which is whether your organization trusts the output enough to let it influence workflow.** I have been changing my mind about underwriting AI over the past few months. I am still learning this in conversations with carriers, MGAs, and underwriting teams, and my view will keep evolving. But this is where I have landed for now.


Reading a submission is not the same as being allowed to act on one. Can the system clear an appetite screen? Trigger a referral? Generate a broker follow-up? Create a subjectivity? Write back into a system of record? Those are authority questions, not accuracy questions, and most pilots never get to them.


### Key takeaways


- The hard part of underwriting AI is not extraction. It is authority: whether the organization trusts the output enough to clear appetite, trigger referrals, raise subjectivities, or write back to the system of record.
- McKinsey has estimated up to 40% of large commercial underwriting time still goes to administrative work, including rekeying data and manual analysis. That is usually framed as an efficiency problem. It is also a control problem.
- Submission intake decides more than people admit. By the time an underwriter sees a clean summary, the condition of the file has already shaped the judgment.
- The useful system does not just summarize the file. It reconciles it, and shows what it used, what it ignored, what conflicts, what is missing, and what is only a low-confidence assumption.
- Every flag needs an operating answer (hard stop, broker question, referral, subjectivity, overridable field, deprioritize, decline early). Without that, AI just builds a better exception queue.


## The 40% is a control problem, not just an efficiency problem


McKinsey has estimated that up to 40% of large commercial underwriting time is still spent on administrative work, including rekeying data and running manual analyses. That number usually gets discussed as an efficiency problem: too many expensive people doing clerical work.


I think it is also a control problem. If a third of the job is spent assembling, checking, and moving inputs around, then the quality of that assembly is already part of underwriting. It is not prep work that happens before the real decision. It is shaping the decision. This is the same reason[most underwriting AI pilots stall before they reach production](https://www.pibit.ai/blog/why-78-of-ai-underwriting-pilots-stall-and-what-accuracy-has-to-do-with-it) : the pilot proves the model reads, and then nobody can answer who is accountable when it acts.


## Submission intake decides more than people admit


My core conviction is that intake quietly decides outcomes the org attributes to underwriting skill. It decides which named insured gets carried forward. Whether the locations in the application match the SOV. Whether payroll, revenue, vehicle count, or TIV actually tie across documents. Whether loss runs are valued recently enough to matter. Whether open reserves are treated as signal or noise. Whether a missing policy year becomes a broker follow-up or just disappears into the file.


By the time an underwriter opens a clean summary, a lot of judgment has already been shaped by the condition of that file. We have written before about how[what underwriters actually see is rarely the full picture](https://www.pibit.ai/blog/what-underwriters-actually-see-isnt-the-full-picture) . If the inputs were assembled loosely, the decision inherits that looseness, and no amount of downstream pricing sophistication recovers it.


## An evidence layer, not a general assistant


This is why I am less interested in underwriting AI as a general-purpose assistant and more interested in it as an evidence layer that sits before underwriting judgment gets applied. A useful system is not just summarizing the file. It is reconciling it:


- Application against schedules
- Schedules against loss runs
- Broker email against the stated exposure
- Outside data against what was actually submitted


And it should be explicit about its own work. It should show what it used, what it ignored, what conflicts, what is missing, and what is only a low-confidence assumption. That transparency is the whole point. An underwriter confirms a reconciled,[source-linked file](https://www.pibit.ai/blog/field-level-provenance-ai-underwriting) far faster than they rebuild an unreconciled one, and they trust it more when they can see where every value came from.


## Every flag needs an operating answer


Here is the uncomfortable part. Surfacing a discrepancy is easy. Deciding what it means operationally is the actual design problem. Every flag needs an answer:


- Is it a hard stop?
- A broker question?
- A referral note?
- A subjectivity?
- An overridable field?
- A reason to deprioritize the account?
- A reason to decline early?


Without that design, AI does not remove work. It just creates a better-organized exception queue, and a queue is not a decision. The value shows up only when each flag routes to a defined action inside the workflow the underwriter already runs.


## Where should AI sit, and who buys it


This is also why the buyer is not obvious. Underwriting owns the decision. Operations owns pieces of the work. Technology owns the system risk. Finance asks why to invest in new tooling. Data teams may already be building parts of this internally. Everyone is directionally aligned that AI belongs in the workflow. Not everyone is solving for the same thing.


And nobody wants another source of truth sitting next to the policy admin system, the rating engine, the CRM, the document repository, and someone's spreadsheet. Which is why the wedge matters more than the model. The right entry point is narrow, sits on top of the systems already in place, and earns authority one defensible decision at a time rather than asking for it up front. That is the version of underwriting AI I am betting on: not a smarter narrator of the file, but an evidence layer the organization can actually trust to act. It is the thesis behind how we built the[CURE platform](https://www.pibit.ai/platform) , and the[extraction and reconciliation work underneath it](https://www.pibit.ai/blog/certificate-of-insurance-processing-commercial-pc-coi-extraction) .


I will probably keep refining this. But the reframe I am most sure of is this one: the question was never whether AI can read a submission. It is whether your organization trusts it enough to let it act.


See how the evidence layer works on the[CURE platform](https://www.pibit.ai/platform) .
