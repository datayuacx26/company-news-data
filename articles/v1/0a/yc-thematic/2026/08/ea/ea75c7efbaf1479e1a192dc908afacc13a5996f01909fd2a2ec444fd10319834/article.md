---
schema_version: "1.0.0"
document_id: "ea75c7efbaf1479e1a192dc908afacc13a5996f01909fd2a2ec444fd10319834"
company_key: "yc-thematic"
company: "Thematic"
source_id: "yc-thematic-news-import-d60ff6e474dd"
canonical_url: "https://getthematic.com/insights/ask-customer-feedback-questions-plain-english"
published_at: null
first_seen_at: "2026-08-04T09:39:53.464397+00:00"
fetched_at: "2026-08-04T09:43:50.004226+00:00"
content_hash: "sha256:5d35bfd3c64f7d76fe152e151e950ce6c4756271d5dc8c71c23b1247a4efe169"
---

# Can You Ask Questions About Your Customer Feedback in Plain English and Get an Answer You Can Defend?

Someone in a leadership meeting asks why the score dropped in one region last quarter. The insights team says they'll come back with an answer. A week later the moment has passed and the decision got made without them. Meanwhile anyone in the building can paste a few hundred comments into a general-purpose chatbot and have a confident paragraph before the meeting ends. Speed was never the problem. The problem is what happens when someone asks a follow-up question and nobody can show where the answer came from.


Yes, you can ask questions of your[customer feedback](https://getthematic.com/insights/customer-feedback-analysis) in plain English and get an answer that holds up. Thematic Answers does this. What makes the answer defensible isn't the language model, it's what the question is asked of. Answers queries feedback that Thematic has already cleaned, themed, and scored, rather than free-associating across raw text. Every answer shows which datasets and filters it used and how many sentences it covered. Each section carries a citation you can click into to reach the underlying comments.


That distinction is the whole article. Below: what "asking questions of your feedback" actually means, what separates an answer you can defend from one you can't, how Thematic Answers is built, and how to test any tool that claims to do this.


## What does "asking questions of your feedback" actually mean?


Two different things get called the same name, and they behave nothing alike.


The first is pointing a general-purpose assistant at a pile of raw comments. The second is querying a dataset that's already been analyzed, where themes, sentiment, and scores exist as structured data before the question is ever asked.


The difference shows up across four dimensions:


- **What the model reads.** Raw comments in the first case,[coded themes](https://getthematic.com/insights/coding-qualitative-data) and computed scores in the second.
- **What limits the answer.** A context window in the first case, the size of the analyzed dataset in the second.
- **What you can check.** A plausible summary in the first case, a traceable path back to specific comments in the second.
- **What happens twice.** Ask the same question twice of raw text and you can get two different answers. Ask it of a fixed analytical layer and the underlying numbers don't move.


Thematic has written separately about what goes wrong when you[point ChatGPT or Copilot](https://getthematic.com/insights/chatgpt-copilot-feedback-analysis) at customer feedback, and about why AI tools[give different answers](https://getthematic.com/insights/why-ai-gives-different-feedback-answers) to the same question. This piece is about the other approach.


## What makes an answer defensible?


An answer is defensible when a skeptical colleague can check it without taking your word for anything. In practice that means five things.


**Traceability.** You can get from a sentence in the answer to the specific customer comments behind it. Not a sample, not a representative quote the model picked, but the actual matched set.


**Scope transparency.** The answer states which data it covered: which datasets, which filters, how much text. An answer over 400 comments and an answer over 40,000 read identically unless the tool tells you which one you're looking at.


**Sample honesty.** When the question lands on thin data, the answer says so instead of generalizing confidently from eleven responses.


**Stated limits.** The tool is explicit about the questions it can't answer well. A system that attempts every question equally tells you nothing about its own reliability.


**Auditability.** Someone other than the asker can see what was asked and what came back. Most tools ignore this one entirely, and it's the one procurement asks about.


Confidently stated but unfounded output is the well-documented failure mode of generative systems. Provenance isn't a nice-to-have layered on top of a question-answering feature. It's the feature.


## Where do most AI feedback tools fall short?


The common failures cluster in a few places:


- The answer arrives with no indication of how much data it covers.
- Quotes are shown, but there's no path from a claim to the full set of comments behind it.
- Filters you applied are silently ignored or silently enforced, with no way to tell which.
- The tool answers every question with equal confidence, including the ones it handles badly.
- Nobody but the person who asked can see what was asked.


One request separates the tools at demo time: **ask the same question twice, then ask the vendor to show you exactly which comments the answer was built from.** If the second half produces a shrug, the answer was never checkable.


## How does Thematic Answers work?


[Thematic Answers](https://getthematic.com/product/answers) takes a plain-English question, decides how to investigate it, and returns a narrative answer with visuals and citations. Under the surface, the agentic engine makes **up to 10 tool calls per question** , replacing the single fixed query the earlier version ran. That's what allows multi-part answers to composite questions rather than one chart and a sentence.


**Answers queries analyzed data, not raw text.** Thematic cleans, themes, and categorizes feedback before any question is asked, and Answers draws on that layer plus the underlying verbatims. You're asking questions of meaningful data, not of everything.


**Every section carries its own citation.** Thematic returns per-section citations you can follow into the Analysis tools, so a claim in paragraph three leads to the themes and comments behind paragraph three specifically.


**The "Used data" panel shows the scope.** Thematic exposes which datasets and filters the answer used and how many sentences it covered, including any comparisons the agent made along the way.


**Thin data announces itself.** On low-sample questions, Thematic returns the verbatims along with the matched and total counts, so a weak answer looks weak instead of confident.


**The supported question types are published.** Thematic is explicit about what the engine handles well and what it doesn't:


Handled well Not handled well


Explaining one aspect of the data Emerging trends


Comparing two segments Comparing themes to each other


Retrieving a score such as NPS or CSAT Retrieving comments for a specific X


Scores broken down by dimension Feedback quantity by dimension


Explaining what caused a score to change


**Administrators can audit every question the organization asks.** With the Manage Answers permission, an administrator opens an Audit answers page. It lists every question asked across the account, who asked it, when, and the answer that came back. The list is searchable. This is what turns a self-serve feature into something a[governance team](https://getthematic.com/insights/auditable-transparent-ai-feedback-analytics) will sign off on.


Two caveats worth knowing. Agentic answers take roughly 1.5 times longer to return than the older single-query engine, because the agent is doing more work. And filters you apply are treated as hints to the agent rather than hard constraints, so check the "Used data" panel when the exact filter set matters.


## What does this look like in practice?


[Community Health System](https://getthematic.com/insights/community-health-systems-actionable-insights) is a not-for-profit healthcare network serving California's central San Joaquin Valley. It runs an annual employee engagement survey collecting open-ended feedback from staff across 250 departments. A director who wanted to know what their team was saying used to book time with the Experience team and walk through comments live.


The Organizational Development team used Thematic's Answers feature to generate a summary and a set of recommended next steps for every one of those departments. The question was a plain-English one, scoped deliberately: "What are the five next steps based on the obstacles question for this department, excluding pay, staffing, and benefits?" They cut those factors because middle managers can't control pay or benefits, and a recommendation nobody can act on is noise.


The result: 250 one-page department reports in a single three-day sprint. Preparation dropped from about an hour per department to about 20 minutes, saving more than 160 hours per survey cycle.


The team still edited the output. They replaced recommendations that no longer made sense given system-wide changes, including one suggesting the return of a supervisor role that had just been phased out. As Alexandra Clifton, Organizational Development Partner at Community Health System, put it: "The only reason we built these single-page reports is because we had Thematic. Before, we were doing everything in an Excel spreadsheet and just generating information and having conversations with people in real time."


That's what a defensible answer looks like in practice. Fast enough to reach 250 departments at once, traceable enough that a human caught the one recommendation that had gone stale.


## A buyer's checklist for question answering over feedback


Ask any vendor these six questions before you sign anything:


1. Is the answer generated from raw comments, or from an analyzed layer of themes and scores?
2. Can I click from a specific sentence in the answer to the specific comments behind it?
3. Does the answer show how much data it covered, including which filters were applied?
4. What happens when I ask a question the tool can't answer well? Show me one.
5. Can an administrator see every question asked across the organization, and who asked it?
6. If I ask the same question next quarter, what would make the answer change?


Question five is the one most tools fail. Question six separates a stable analytical layer from a model improvising each time.


## The short answer


Yes, you can ask questions of your customer feedback in plain English and get a defensible answer, but only if the tool is querying data that was analyzed before you asked. Thematic Answers runs up to 10 tool calls against already-coded themes and scores. It cites each section back to the underlying comments and shows which datasets and filters it used. Administrators get an audit trail of every question the organization has asked. Run one test: ask a question, then ask to see every comment behind the answer. If you can't get there in two clicks, the answer was never yours to defend.
