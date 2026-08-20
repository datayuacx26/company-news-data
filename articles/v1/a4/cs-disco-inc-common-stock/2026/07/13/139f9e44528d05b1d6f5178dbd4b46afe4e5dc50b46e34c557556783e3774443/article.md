---
schema_version: "1.0.0"
document_id: "139f9e44528d05b1d6f5178dbd4b46afe4e5dc50b46e34c557556783e3774443"
company_key: "cs-disco-inc-common-stock"
company: "CS Disco Inc."
source_id: "cs-disco-inc-common-stock-news-import-f96a79529b37"
canonical_url: "https://csdisco.com/blog/schulte-v-linkedin-genai-document-review"
published_at: "2026-07-24T00:00:00+00:00"
first_seen_at: "2026-07-25T03:54:56.959959+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:ea6d4fe50c71c35657aa4f5718c66c0cfa09cafac418a3c0aba6e6468fdb7ca4"
---

# Schulte v. LinkedIn: Courts Treat GenAI Review as TAR

"Is it defensible?" is the question we hear most often when a legal team is deciding whether to trust our generative AI review solution, Auto Review, with its first-pass document review. It's a fair question, and until recently, the honest answer was: probably. But no court had said so directly.


That changed on June 30, 2026. In *Schulte v. LinkedIn Corp.* , a federal magistrate judge in the Northern District of California ruled on a producing party's use of generative AI in document review, and the ruling reads like a direct response to the objection we hear in nearly every conversation about AI-powered review.


The short version: The court didn't treat generative AI review as something new that needed its own rulebook. **It treated it as a form of technology-assisted review (TAR), governed by the same reasonableness and proportionality standards that have applied to document review for over a decade.**


That's a meaningful data point for anyone building a business case for GenAI review, and it's worth walking through what the court actually said, and did not say.


## **What happened in Schulte v. LinkedIn**


*Schulte v. LinkedIn Corp.* is a putative antitrust class action pending in the Northern District of California, with plaintiffs alleging that LinkedIn used practices that let it overcharge Premium subscribers in violation of the Sherman Act. The underlying antitrust claims aren't why this case is making the rounds in ediscovery circles. The discovery dispute is.


Under the case's electronically stored information (ESI) protocol, LinkedIn disclosed that it would use a generative AI-powered review tool to make responsiveness determinations, after first applying 25 negotiated search terms to narrow the custodial document population. Plaintiffs pushed back on three fronts. They asked the court to:


- Bar the pre-culling and instead compel LinkedIn to run its GenAI tool across the full, un-culled custodial population
- Compel disclosure of additional validation metrics, including elusion estimates and reviewer counts
- Treat the use of a generative AI tool as a final decision-maker on responsiveness as something requiring heightened scrutiny


Magistrate Judge Laurel Beeler denied all three requests.


‍


💡 **A quick aside on "discovery on discovery":** This is the doctrine that governs how much a requesting party can probe the other side's collection and review methodology. Courts generally disfavor it, on the theory that litigating the litigation process itself is its own kind of burden. A party usually has to point to a specific, demonstrated deficiency in what came out of the process, not just a general concern about how the process worked. Judge Beeler applied that same doctrine here.


‍


## **Why the court's framing matters: GenAI review is a form of TAR**


The part of the ruling that will get cited for years is the framing, not just the outcome. The court didn't carve out new discovery rules for generative AI. It applied the existing TAR framework, the same one that has governed predictive coding and technology-assisted review since the early 2010s, directly to LinkedIn's use of generative AI-powered review.


On the pre-culling question, the court found that using negotiated search terms to reduce the document population before AI-assisted review satisfies the reasonableness and proportionality standards of Federal Rules of Civil Procedure 26(b) and 34(b)(2), the same conclusion many courts have reached for years about pre-culling ahead of traditional TAR. On the metrics question, the court held that the "discovery on discovery" standard, developed over more than a decade of TAR litigation, applies with equal force to generative AI review: speculation that the process might be inadequate isn't enough to compel additional disclosures.


That's the throughline worth internalizing: the underlying technology changed, but the standard for defending it did not.


## **What this means for legal teams evaluating GenAI review tools**


For legal teams and litigation support professionals who've been waiting for a court to weigh in before committing to GenAI review, **this ruling removes one real obstacle: the fear that a court might treat generative AI as categorically different, and impose a novel, heavier evidentiary burden just because the underlying technology is new.**


That's meaningfully different from saying the burden disappears. The court didn't say producing parties get a pass on validation. It said the validation standard is the same one that has always applied to TAR: a reasonable, proportional, and (if challenged with a specific showing) demonstrable process.


LinkedIn didn't win this dispute by staying quiet about its methodology. Coverage of the ruling notes that LinkedIn had already disclosed which platform it was using, confirmed that no training set was required, and described its human quality-control process, well before the plaintiffs raised any objection. That transparency, offered voluntarily and early, appears to have shaped how the court viewed the rest of the dispute.


This is precisely the posture DISCO’s Auto Review is built for: a documented, statistically grounded validation process behind every review decision, so the answer to "can you show your work?" is already built into the workflow rather than assembled after the fact.


## **What to build into your ESI protocols now**


The practical lesson from *Schulte* has less to do with generative AI specifically and more to do with timing. Courts across recent GenAI discovery disputes, including this one, keep rewarding parties who settle validation terms at the protocol stage, before a fight breaks out.


A few takeaways worth carrying into your next ESI protocol negotiation:


- **Disclose your GenAI tool and methodology early** , the same way you would for any TAR workflow. Early, voluntary transparency appears to be doing real work in how courts are ruling on these disputes.
- **Negotiate specific validation metrics into the protocol upfront** if you want a documented right to them later. Courts have been reluctant to compel new disclosures after the fact absent a specific, demonstrated deficiency.
- **Treat AI governance as a workflow question, not just policy language.** A rigorous statistical validation framework needs to be built into how review actually runs, not just described in a document nobody reopens until there's a dispute.


This is exactly the kind of protocol design work our AI Consulting team helps litigation teams get right before a matter is underway, not after opposing counsel has already filed a motion to compel.


*Schulte v. LinkedIn* is one decision from one district, and it will not be the last word on generative AI in discovery. But for a question that's dogged nearly every GenAI review conversation in this industry, it's the clearest answer yet: the technology is new, the standard for defending it isn't.
