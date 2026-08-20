---
schema_version: "1.0.0"
document_id: "494627699337d7e343306072950273eddf474d6c8bb96a1cc1708ebea8889c64"
company_key: "yc-arintra"
company: "Arintra"
source_id: "yc-arintra-news-import-a42ad051e115"
canonical_url: "https://www.arintra.com/resources/blog/what-should-coding-operations-look-like-when-ai-can-do-90-of-the-work"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-07-30T14:05:00.786754+00:00"
fetched_at: "2026-07-30T14:05:01.685512+00:00"
content_hash: "sha256:9c08c4a98c7c55599fcdfbd3558b1c0a2bd4ba6268224c66a397cf937d41a8c1"
---

# What Should Coding Operations Look Like When AI Can Do 90% of the Work?

A customer recently walked us through their prior authorization workflow. Their authorization team handles thousands of procedures a month, but they don't have coding training. They don't really talk to the coding team either. It's more of an exception when they reach out.


What was happening was that, frankly, hundreds of thousands of dollars a month in charges were being billed with really basic mismatches between authorization, documentation, and coding. Those mismatches were causing denials.


That situation stuck with me because it gets at something I see across healthcare organizations right now. Coding sits at the center of a revenue cycle, but most organizations aren’t set up to use it efficiently. Coders are buried processing volume they can't keep up with, while the answer most leaders are reaching for is to hire more people, which doesn't solve the structural problem.


I'm the director of product management at Arintra and I want to talk about a different way to think about how coding should operate. In this new mode, AI handles the routine work and human experts focus on the cases that require their judgment.


## Why Manual Coding Can’t Keep Up


The historical model of medical coding involves hiring more coders as chart volumes rise. Sometimes that means in-house hires, outsourcing to services companies, and/or outsourcing offshore. This way of doing things is expensive and causes standardization and continuity issues, along with being hard to sustain. Outsourcing also comes with its own set of challenges.


It’s hard to find the right partner, difficult to onboard and standardize across practices, to gain insight into their work, or maintain consistency. Even if you solve all of those issues, you still can’t scale it. Healthcare data and the burden of processing it are growing faster than organizations can afford to staff for, and faster than they can recruit and maintain.


Fully automating coding without human judgment in the loop doesn't work either. You need experienced coders monitoring metrics like:


- Weighted average E/M level across the organization
- Typical charge for a given modality
- Distribution of new versus established patient visits


When something is off, an expert should have the oversight to spot it and figure out what’s happening. Without that, the AI has no correction and just keeps running as the errors compound. What is already working is a different model, one where AI does the heavy lifting for routine work and humans focus on things that require their judgement.


## A New Operating Model For Coding


In the new model, you treat the AI like a virtual coder with the same work queues, routing logic, and structure you'd build for a human coding team. What this changes is that you can route 100 percent of your volume through an AI-powered coding workflow, and depending on the specialty, about 80 to 90 percent of that gets fully automated and quickly sent to billing. The other 10 to 20 percent gets routed to a human for review.


Anything the AI flags as low confidence, or that has a data quality issue, or requires a provider to update their documentation gets routed to what we call an assisted review queue. The coding team works that queue with the AI's suggestions and rationale already there. Coding teams don’t have to code from scratch. They're reviewing and resolving the cases that require judgment.


### The Query Workflow


The coding team is also freed up to do work that wasn't possible when they were buried in processing routine, high-volume charts. Take the query workflow as an example.


When the AI downcodes an E/M level because the provider's documentation didn't support a higher code, it also surfaces the specific gap. So maybe the provider referenced a medication generally instead of documenting a specific drug and dose.


The coder sees the downcode and the reason for it, and decides whether to query the provider. If they do, the provider updates the chart before the claim goes to the insurer, and the higher-level code is what gets billed.


### Freeing Coders for Cross-Functional Work


The same logic applies across the rest of the revenue cycle. When coders aren't swamped with routine coding, they can bring their expertise to prior authorizations and denial work. This helps solve exactly the kind of silo problem I described at the start of the piece.


The silos that exist today are there because nobody has the capacity or the[intelligence layer](https://www.arintra.com/resources/blog/coding-as-intelligence-what-health-organizations-learn-when-ai-reads-every-chart) to bridge them. Once the baseline work is automated, that capacity opens up.


### The Analytics Layer


One more piece of this model is analytics and observability. You need a surface where non-technical people can monitor the system continuously. How long is it taking charts to go out the door? Is the average E/M level skewing? Is volume off from the baseline?


Anyone managing the operation needs to be able to see deviations from the trend and be able to raise a flag for things needing investigation. That kind of monitoring is part of what makes the AI accountable, and it's part of what makes this operating model work.


### What This Means for Coders


I really want to emphasize this to any leaders who are worried about what this means for their coding teams. The honest reality is that most of our customer organizations have coding teams that can only keep up with 10 to 20 percent of their volume, which only continues to grow.


Adopting AI shouldn’t be a means of replacing humans or laying people off. It's how you address the workload your organization has. You use the existing staff, put them into an AI-powered workflow, and thereby drive performance at scale.


## How One Org Cut Manual HCC Review by 70%


One of our customers had been using us for their procedure coding for some time. Arintra automated 80 percent of their CPT coding with good revenue impact, and their existing coding team had shifted into the new operating model around that work.


Arintra was also coding HCC diagnoses as part of that output. But the same organization had a completely separate team handling HCC diagnosis coding for their value-based care contracts. That team was reviewing 100 percent of their encounters manually, independently of Arintra’s work.


They came to us and said, based on what we're seeing with how Arintra codes, we think you can significantly reduce our workload. So we set up routing rules for the HCC team for cases where the engine:


- Was confident and not making changes (auto-dropping them to billing)
- Down-coded or removed an HCC diagnosis (routed to the coding team for review)
- Flagged a documentation issue (routed to the team for review with the issue flagged)


Their manual review workflow dropped from 100 percent of the volume to about 20 to 30 percent. The team kept their query workflow focused on the cases that needed investigation, but they no longer had to review 70 to 80 percent of charts that our AI engine was already handling well.


They started working on identifying codes that might be defensible with better documentation, executing provider queries, and recovering revenue that would have otherwise been lost.


This is the new operating model in practice. AI handled the volume the coding team couldn't keep up with. The team focused on the work that required their judgment. Revenue capture went up, lag times from date of service to claim submission dropped, and denial rates fell because the upstream coding was more accurate and compliant.


## The Path Forward


The opportunity for most healthcare organizations right now is real, but you can’t simply turn on a tool. Leaders will have to make some deliberate choices. My advice is to:


1. Pick a partner that understands your specific patient population, market, and payer mix.
2. Invest in the coders you already have, because they're the ones with the coding expertise you need for judgment calls, provider queries, and cross-functional work. The transition into that role deserves real support.
3. Be sure to measure what matters, such as charge lag, denial rates, revenue capture, and audit exposure. Those are the outcomes autonomous coding is supposed to move.


This is how you start building a coding operation that can keep up with your organization’s needs.


‍
