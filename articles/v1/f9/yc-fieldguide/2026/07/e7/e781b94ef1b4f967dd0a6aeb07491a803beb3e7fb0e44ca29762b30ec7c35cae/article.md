---
schema_version: "1.0.0"
document_id: "e781b94ef1b4f967dd0a6aeb07491a803beb3e7fb0e44ca29762b30ec7c35cae"
company_key: "yc-fieldguide"
company: "Fieldguide"
source_id: "yc-fieldguide-rss-652d61b18f53"
canonical_url: "https://www.fieldguide.io/blog/review-is-the-new-prepare"
published_at: "2026-07-29T12:44:02+00:00"
first_seen_at: "2026-08-18T01:47:26.942382+00:00"
fetched_at: "2026-08-18T01:47:28.276336+00:00"
content_hash: "sha256:2efe2fd3ff11c46d3f866aad890a2c26dac9b0abe33f2f52f9b5192f8e784b97"
---

# Review Is the New Prepare: The Auditor's Job in an Agent-First World

Ask a practitioner where their hours actually go and the answer has been the same for decades: preparation, pulling evidence, tracing amounts, ticking and tying, formatting workpapers, chasing requests, building the file so that someone more senior can finally look at it. Review got whatever time was left over, because preparation was the job.


Today, that structure is inverting faster than most firms have anticipated. The reason is agents. Most AI in the profession (chat tools, copilots, point automations) assists the work, with a human triggering each step. Agents actually *do* the work: they execute multi-step procedures on their own while the practitioner reviews the result. Fieldguide's Field Agents are purpose-built agents for audit and advisory, aligned to the phases of an engagement and to the firm's methodology. When Field Agents execute the testing, validate the evidence, and document the results with citations, preparation stops being where practitioner time is spent. The hours migrate into review.


Most firms read this transition backwards. What looks like a warning sign is often evidence the model is working, and what looks like a win is sometimes the real cause for concern. The model itself is simple: practitioners and Field Agents on every engagement, with the agents planning, executing, and documenting the work while the humans review, judge, and advise. The firms that learn to read those signals correctly are the ones whose engagements will actually get faster over time.


## Rising review time means the model is working


Here is what the shift looks like on a routine control test. Your team assigns the test to the Field Agent that executes evidence collection and controls testing. From there, the Field Agent takes the test end to end: it matches evidence to samples, validates the data, executes the procedure, and documents the conclusion with citations back to the source documents. The preparation that used to consume a staff auditor's entire week is finished before the team's morning coffee. Instead of starting from a blank workpaper, the practitioner opens completed work that needs their judgment.


Among our customers, the pattern in the first weeks of rolling out Field Agents is remarkably consistent. Preparation time drops sharply, and review time, at least at first, rises. Most teams read the second number as a problem, when it is usually evidence of the opposite. Rising review hours mean the agents are producing enough completed work that your people now spend their day exercising judgment. Not every increase in review hours is the good kind, though. Anomaly detection and analytics point tools also drive review hours up, but for a different reason: they flag anomalies that your team still has to investigate from scratch. As such, these tools create more work for your team without completing any of it themselves. Review hours should rise because finished procedures are arriving with conclusions and citations attached, and humans are deciding whether those conclusions hold. A rollout where review time never moves is the one that should worry you, because it means the agents are not executing enough work to matter.


The industry has lived this curve before. Early document extraction at 50% accuracy cut data entry in half while review effort climbed, and skeptics called it a wash. Then accuracy improved, the scales tipped, and manual data entry quietly ceased to exist as a job. Agentic AI is on the same curve with a far higher starting point and an improvement cycle measured in months. The firms that treated the review-heavy phase as an investment built the muscle early. The firms that treated it as a defect waited, and then had to build the same muscle later, under more pressure.


## Why the profession mistook the tax for the training


But this shift runs into a strong developmental objection: young auditors will never learn the work if agents do the preparation for them. The objection has it backwards, because preparation was never the source of judgment.


Strip away the preparation mechanics and an audit is the application of professional skepticism and judgment to evidence, concluded by a human who signs their name to it. That act of review and sign-off is the legal and professional core of the work. It is the part clients pay for, the part regulators require, and the part that cannot be delegated to a machine. Preparation was the tax the profession paid to get evidence into a reviewable state, and because generations of auditors spent their formative years paying it, the profession learned to call the tax an apprenticeship.


But nobody became a trusted advisor by ticking and tying. They became one by reviewing enough work, and enough kinds of work, to develop judgment. Ask any partner where that instinct came from and they will describe the review notes they got and the review notes they learned to give rather than the formatting. So when Field Agents take over execution, they remove the tax and leave the training. The auditor's job is concentrating into review, which was the highest-impact work on the engagement long before agents arrived. What agents change is how much of the auditor's day it fills.


## An agent that sometimes cannot conclude is the one you can trust


If the practitioner cannot verify agent output quickly and confidently, the agent has no real impact. Execution without verifiable review makes for an impressive demo and nothing more. As agents executing the work become table stakes, the review experience becomes the product. And building that product at Fieldguide has taught us a few things that cut against the prevailing wisdom about AI.


The first is that an agent that produces a conclusion every single time is actually less trustworthy. A professional-grade Field Agent does not force a determination when a document fails to process or the evidence chain is weak. The right output in that moment is a clear flag for human attention. Vendors demo AI that always has an answer. Practitioners should ask to see what happens when it actually shouldn't have one.


The second is that chat, the celebrated interface of the AI era, is the wrong tool for much of review. Chat is excellent for direction and terrible for correction. When an extraction comes back wrong, the practitioner should key in the right value and move on, with the fix recorded, rather than negotiating with a conversational interface. Review has to be efficient at the level of the individual field, because that is where review actually happens.


The rest is unglamorous but essential. Every conclusion must carry citations that take the reviewer straight to the exact location in the source document, so verification is visual confirmation rather than archaeology. Output must arrive as a review package, everything relevant to a control or procedure in one place with exceptions surfaced first, so the path from "agent completed" to "signed off" is measured in minutes. And the platform must keep a record that survives scrutiny: what was reviewed, what the human changed, what was sent back as well as why, and who signed off at each step. That is why we baked human-in-the-loop review into the platform itself, as architecture rather than a compliance disclaimer.


## Why we built the Agent Review Experience


Agents changed what reviewers need. When Testing Agent completes controls across an entire engagement, the job in front of your team is no longer building workpapers but judging finished ones, at volume. That workflow deserved a surface designed specifically for it. So we built the Agent Review Experience, a dedicated space for reviewing agent-tested controls from first look to sign-off.


Once Testing Agent has run, each control opens as a single review package: the agent's summary table, the test results with a toggle that filters straight to exceptions, the sample details, and every piece of evidence with citations that open inline to the exact annotated location. A reviewer moves through controls with keyboard shortcuts, corrects an output where the agent got something wrong (the edit is saved, attributed to the reviewer, and never overwritten by a rerun), and closes each control with a sign-off, or a sign-off plus an exported artifact with structure and citations preserved.


It is the argument of this piece in miniature: consolidating review into one surface is what turns agent speed into engagement speed.


## The most junior person at your firm just got promoted


In the old model, a first-year spent busy season on extraction, tracing, and formatting, and encountered real judgment only after years of paying their dues. In the agent-first model, the preparer becomes the first reviewer. Their job on day one is to evaluate completed work: check the conclusions against the evidence, correct what is wrong, and escalate what deserves a manager's attention. The responsibilities that used to arrive in years three through five now show up on day one.


From there, the effects compound. By the time work reaches the manager, it has been executed by a Field Agent and reviewed once by a human, which means manager and partner review gets sharper too. Nobody is buried in prep, so the senior eyes on the file go to risk, judgment calls, and client conversations.


For recruiting, this may be the most consequential inversion of all. The common fear is that AI makes the profession less attractive to young people. And yet, the opposite is more likely. Firm leaders increasingly describe their hiring challenge as one of skills rather than headcount: finding people ready for how the work is changing, and figuring out what their teams should look like with agents in the mix. A career that starts with judgment instead of data entry is a career the next generation might actually choose, and the firms already operating this way are becoming the ones that talented people seek out.


## Why faster execution slows firms down


One paradox remains, and it deserves the most attention from practice leaders. If agents accelerate execution and nothing else changes, the engagement does not get faster. The bottleneck moves to review and sits there. Firms that adopt agents without rebuilding the review layer end up with completed work queuing in front of the same review process they ran in 2023, which is how a firm winds up with faster agents and slower engagements at the same time.


So when you evaluate platforms, weigh the review experience more heavily than the execution. Few vendors can show real agent output today, and even among the ones that can, the differences show up in review. Ask to see the AI citations, the sign-off chain, the exception handling, and what happens when the output is wrong.


Then retrain your teams, because reviewing agent output is a different skill from reviewing a first-year's workpapers. Agents fail differently than humans do: consistently, at scale, and sometimes without obvious tells. And perhaps most importantly, decide on purpose where the reclaimed hours go: deeper procedures, expanded scope, new engagements, richer client conversations. The firms that answer that question intentionally will grow revenue faster than headcount. The firms that do not will have quieter busy seasons and the same P&L.


Review was always the profession's signature act. The auditor's opinion, the advisor's conclusion, the human name on the file: none of that changes in an agent-first world. What's new is that everything upstream of it now runs at agent speed, and the practitioners who master review will be the ones around whom the profession is built.


Dhruv Dhingra


VP, Product at Fieldguide


## Related posts


[View All Posts chevron_right](https://www.fieldguide.com/blog)


- [How AI is rewriting the rules of PCI compliance reporting April 28, 2026 Solutions](https://www.fieldguide.com/blog/blog-test)
- [Grant Thornton Advisors Partners with Fieldguide to Power Next-Generation Risk and Controls Services with Agentic AI July 14, 2026 News](https://www.fieldguide.com/blog/grant-thornton-partnership-compliai)
- [Field Board: A Kanban-Style Command Center for Advisory & Audit June 25, 2026 Product Releases](https://www.fieldguide.com/blog/field-board-a-kanban-style-command-center-for-advisory-audit)
- [Maloney + Novotny Modernizes Assurance Practice with Fieldguide’s AI-Native Platform June 22, 2026 News](https://www.fieldguide.com/blog/maloney-novotny-modernizes-assurance-practice-with-fieldguides-ai-native-platform)
