---
schema_version: "1.0.0"
document_id: "fe8bee02d6f23ed47e69bf963c2f3dce4b72137bc70356f2911f1304ede787e8"
company_key: "yc-wolfia"
company: "Wolfia"
source_id: "yc-wolfia-news-import-63b7007a854b"
canonical_url: "https://wolfia.com/blog/how-to-catch-dynamic-questions-vendor-portals-add-mid-review"
published_at: "2026-07-17T00:00:00+00:00"
first_seen_at: "2026-07-29T07:09:22.424766+00:00"
fetched_at: "2026-07-29T07:09:23.598442+00:00"
content_hash: "sha256:4cb9b28f77ca7e9f23e731b039655acca00b3cd4df594c48940eb59c84f51160"
---

# How to catch questions vendor portals add mid-review

**TL;DR**


- Vendor portal questionnaires are not a fixed list. Conditional logic and reviewer follow-ups add questions after you have already submitted answers.
- Recheck the portal within 24 to 48 hours of every submission. That is the window where branching questions and reviewer notes typically surface.
- A "yes" or "no" answer is the most common trigger for a new branch. Treat binary answers as a signal to watch that section for follow-ups.
- Assign one named owner per in-flight portal. Shared ownership is how new questions sit unseen for days.
- Wolfia completes questionnaires directly inside the portal and re-detects newly surfaced questions after each submission, so nothing added mid-review goes unanswered.


## Why don't vendor portal questionnaires stay fixed?


Most GRC teams treat a portal questionnaire like a form: open it, answer everything visible, submit, done. That model breaks down because two separate mechanisms keep changing the question set after your first pass. Branching logic reveals hidden questions based on your answers, and human reviewers on the buyer's side add follow-up questions after reading what you submitted. Neither shows up as a notification in most portals. You find out by logging back in.


The result is a questionnaire that looks finished on day one and is quietly incomplete on day five. If nobody rechecks it, the portal sits open past its own due date, and the assessment stalls without anyone on your side realizing why.


## How does conditional logic add new questions mid-review?


Conditional logic reveals a hidden branch of questions when your answer to an earlier item meets a trigger condition, most often a "yes" or "no". Answer "yes, we use a subprocessor" and a portal may unlock five questions about that subprocessor's access scope and contract terms that were not visible when you started the form.


This is standard practice across vendor risk platforms, not a bug or an edge case.[Shared Assessments' SIG questionnaire](https://sharedassessments.org/sig/) is built around exactly this structure: a core question set plus supplemental branches that only apply depending on how you answered the gating item. If you screenshot or export the question list at the start of a review, treat that export as a starting point, not a final inventory. The full set only reveals itself as you answer.


The practical tell is a binary answer. Any time you answer "yes" or "no" to a question about a control, a subprocessor, a data flow, or a certification, assume there is a reasonable chance a new branch opened somewhere in the form. That is worth a targeted recheck of that section specifically, not just a general glance at the top of the page.


## Where do reviewer follow-up questions come from?


The second source of new questions is a person, not the software. A reviewer on the buyer's side reads your submitted answers and adds a clarifying question directly into the portal, often without emailing you separately. This happens most on answers that are accurate but too brief for the reviewer's risk model, or where your answer references a policy or control the reviewer wants more detail on.


Reviewer follow-ups are less predictable than conditional branches because there is no fixed trigger. They can appear a day after submission or two weeks later, depending on the reviewer's queue. The only reliable way to catch them is a recheck cadence, covered next, because there is no branching logic to reverse-engineer. It is simply a person reading your work and deciding they want more.


## How do you catch new questions before a deadline slips?


Recheck the portal within 24 to 48 hours of every answer submission, since that is the window where both conditional branches and reviewer follow-ups tend to surface. Compare the current question count and section list against what you saw last time, and treat any new item as something to answer immediately rather than batching it for later.


The recheck does not need to be exhaustive every time. Log in, check the total question count against your last visit, and scan section headers for anything that was not there before. If the count moved, work through the diff instead of re-reading the entire form. This is a five-minute check when it is scheduled, and a scramble when it is not.


Two things make this recheck reliable instead of ad hoc. First, tie it to a specific event (you just submitted an answer batch) rather than a vague "check back sometime" reminder, since the highest-probability window for new questions is right after a submission triggers a branch. Second, log the check itself, even just a timestamp in a shared tracker, so the next person picking up the review knows the portal was already looked at and when.


## How often should you recheck an in-flight portal?


Set three checkpoints for any questionnaire that stays open longer than a few days: within 48 hours of your first submission, at the midpoint of the review window, and again 48 hours before the due date. Reviews that close within a day or two of opening only need the first checkpoint. Longer reviews, especially DDQs and enterprise SIG assessments that can stay open for weeks, need all three because reviewer follow-ups can arrive at any point in that window.


The midpoint check matters more than it looks. Early questions get answered fast because the team is focused on the review. By the midpoint, attention has moved to the next deal or the next questionnaire, and a follow-up question added on day 6 of a 14-day review can sit for a week before anyone notices. Put the midpoint check on a calendar, not a mental note, if you want it to actually happen.


## Who should own portal monitoring on your team?


Assign one named person to each in-flight portal, not "whoever is free" or "the person who submitted it originally." Ownership fails silently when it is implicit. The most common failure pattern is not that nobody knows how to recheck a portal. It is that everyone assumes someone else already did, so nobody does. If your team runs[security questionnaires with an understaffed team](https://wolfia.com/blog/prioritize-security-questionnaires-understaffed) , a clear owner matters even more, since there is less slack to catch a missed check downstream.


The owner does not need to answer every new question personally. Their job is to run the recheck cadence, spot new items, and route them to the right subject matter expert, similar to how you would[reduce back-and-forth with buyers](https://wolfia.com/blog/how-to-reduce-questionnaire-back-and-forth-with-buyers) by having one point of contact instead of a rotating cast. Write the owner's name into the questionnaire tracker alongside the due date, so it is visible to anyone checking status, not buried in a Slack thread.


## What happens if a question sits unanswered too long?


An unanswered follow-up does not just delay your response. It can hold the entire assessment open past its due date on the buyer's side, which pushes back the deal, the renewal, or the vendor approval the questionnaire was gating in the first place. Some risk teams treat a stale, unanswered follow-up as a signal on its own, separate from whatever the underlying question was actually about.


If you are already tight on time, this is the same failure mode covered in[what to do when a questionnaire deadline is 24 hours away](https://wolfia.com/blog/what-to-do-when-a-questionnaire-deadline-is-24-hours-away) : the questions you did not know existed are the ones that blow the deadline, not the ones you saw on day one and simply ran out of time to answer.


## How Wolfia catches new portal questions automatically


Wolfia is built for GRC and security teams handling exactly this workflow: questionnaires that keep changing after the first submission. Instead of relying on a person to remember the recheck cadence, Wolfia completes questionnaires directly inside the vendor portal through the Portal Agent (covering platforms like OneTrust and ServiceNow) and re-scans the question set after each submission.


A few specific pieces of how this works in practice:


Wolfia's Portal Agent submits answers inside the portal itself rather than working from a static export, so it sees the same live question set a human reviewer would see, branches and all. When a conditional branch opens after a "yes" or "no" answer, Wolfia re-detects the new questions on its next pass instead of stopping at whatever question count it started with.


Every generated answer carries a source citation back to the underlying policy or control documentation, so a reviewer who adds a follow-up question gets a traceable answer, not a generic restatement. If a new question needs a human decision before it goes back into the portal, Wolfia routes the answer to the right reviewer for approval rather than submitting it unchecked, which keeps a person in the loop on anything sensitive without requiring them to babysit the portal itself.


The self-maintaining knowledge management dashboard means the answers Wolfia pulls for a newly surfaced follow-up stay current without a separate library-grooming step, so a branch that opens six weeks into a long DDQ review still gets an accurate answer instead of a stale one.


## Final Thoughts


A vendor portal questionnaire is a moving target for as long as the review stays open. Conditional logic and reviewer follow-ups both add questions after your first submission, and neither one announces itself. The fix is procedural before it is a tooling problem: recheck on a schedule tied to your submissions, watch binary answers as the most likely trigger for a new branch, and put one name on the owner line so a new question has somewhere to land. Where the tooling helps is removing the part that depends on someone remembering to log back in, which is exactly the gap Wolfia's Portal Agent is built to close.
