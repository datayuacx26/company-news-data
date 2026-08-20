---
schema_version: "1.0.0"
document_id: "d762ba28e350649aa681b6c9f8585ecb0a58d809810ff5d3e42a935e68103b4f"
company_key: "yc-phases"
company: "Phases"
source_id: "yc-phases-news-import-61e19d289ed9"
canonical_url: "https://phases.ai/blog/part-11-never-written-for-ai-agents"
published_at: "2026-07-09T00:00:00+00:00"
first_seen_at: "2026-07-23T20:18:02.061944+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:bd5f7ddaa643ad667478c8ed3e80c863966a6a98f99fb79537ab0a2ca88d4638"
---

# 21 CFR Part 11 wasn't written for AI agents

On March 27, 2024, two days after the FDA told Applied Therapeutics it planned to inspect one of its clinical trial sites, a vendor working for the company deleted the electronic outcome data for all 47 subjects enrolled in the study, including the audit trails showing who accessed or modified the data and when.1 While most of the data was recovered from backups, data for eleven of the subjects was not.1 Even where the data came back, FDA still couldn't verify it, because the audit trails documenting who had touched it were deleted.1 In November, FDA sent Applied Therapeutics a warning letter citing its failure to let inspectors access and verify the trial's records.1


No AI agent was involved in what happened to Applied Therapeutics; a human at a vendor made the mistake. But it's the clearest preview available of what happens once[agents, not people, are the ones touching the record](https://phases.ai/blog/building-secure-agents-in-clinical-trials) .


That audit trail existed in the first place because of 21 CFR Part 11, the regulation that says an electronic record can only stand in for a paper one if it comes with a secure, time-stamped record of who touched it and when.2


## What 21 CFR Part 11 actually requires


21 CFR Part 11 was introduced by the FDA in 1997 in light of drug and device companies starting to move from paper to electronic records.2 It covers several conditions in addition to audit trails, such as access controls and electronic signatures.


The FDA's 2024 guidance for clinical investigations outlines exactly what the audit trail requirement has to include,2 highlighting that every audit entry needs to show:


- Who made the change (user ID and role)
- What changed (the old value and the new value)
- When it happened
- Why, where applicable


Importantly, the trail needs to log every deliberate action, such as edits, creations and deletions, including those triggered by automated checks, while also remaining immutable.3


## Where an AI agent changes the requirement


Part 11 predates agents. Every one of its requirements assumes a human is the one accountable for a change, whether that's a data manager editing a field or a monitor approving a query.


FDA's guidance on digital health technologies already lets a data originator be a device or a system instead of a person, but that category was built for something like a sensor automatically transmitting a reading, not for a system exercising judgment.2 There's no equivalent answer yet for an agent that reviews a report and expects a human to weigh in before it counts.


An audit trail that logs "record updated" under one user ID tells an inspector nothing about whether a person or a model made the call. What the agent proposed and what a human approved or changed need to be two separate entries, each with its own timestamp.


## How Phases handles this


Phases was built to support Part 11 for both humans and AI from the start. Every audit log, whether the action came from a human or an AI, is written to immutable cloud storage. What this means in simple terms is nobody, not even Phases, can edit or delete an entry. Each entry captures who made the change, the old and new values, when it happened, and a reason where applicable, which are the same fields required by FDA's guidance.2


The part that matters most is that when our agents surface a finding in a monitoring visit report, for example, that is clearly logged as an action taken by the agent. Thereafter, when a CRA or clinical lead confirms or dismisses that finding, a second, separate event is logged, which is tied to their own user ID. This means that for an inspector looking at the trail, they will see two distinct, time-stamped facts representing the agent's proposal and the human's decision.


Critically, we also track why the agent arrived at a decision, not just what it decided. Alongside the finding itself, we log the AI model used, any tool calls it made along the way (such as reading the protocol), the reasoning behind the call, and the context it was given, such as EDC data or TMF documents, to reach that conclusion.


We built our agents this way so that if the FDA comes knocking, all logs can be inspected clearly and there's no ambiguity in the record of work.


## Conclusion


Applied Therapeutics' failure had nothing to do with an AI agent. Someone at a vendor made the mistake, and the FDA still doesn't have a full explanation as to why that happened.


And that's the slightly uncomfortable reality: these types of mishaps already happen with humans in the loop. As we hand more clinical work off to agents without the same accountability built in, the same failure gets easier to cause and harder to catch. That's why 21 CFR Part 11 is only going to matter more as AI adoption increases, especially when it comes to agents.


If you want to see how our 21 CFR Part 11 compliant agents can be put to work across your TMF, monitoring, and data management, book a demo below, we'd love to chat.


---


## References


1.


FDA. Warning Letter 24-HFD-45-11-01 to Applied Therapeutics, Inc. November 27, 2024.[https://www.fda.gov/inspections-compliance-enforcement-and-criminal-investigations/warning-letters/applied-therapeutics-inc-696833-12032024](https://www.fda.gov/inspections-compliance-enforcement-and-criminal-investigations/warning-letters/applied-therapeutics-inc-696833-12032024)↩↩2↩3↩4


2.


FDA. "Electronic Systems, Electronic Records, and Electronic Signatures in Clinical Investigations: Questions and Answers." Guidance for Industry. October 2024.[https://www.fda.gov/media/166215/download](https://www.fda.gov/media/166215/download)↩↩2↩3↩4↩5


3.


21 CFR 11.10(e), Controls for closed systems.[https://ecfr.io/Title-21/Section-11.10](https://ecfr.io/Title-21/Section-11.10)↩
