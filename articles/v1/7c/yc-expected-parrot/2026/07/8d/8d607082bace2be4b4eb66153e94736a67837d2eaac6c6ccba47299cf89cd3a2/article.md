---
schema_version: "1.0.0"
document_id: "8d607082bace2be4b4eb66153e94736a67837d2eaac6c6ccba47299cf89cd3a2"
company_key: "yc-expected-parrot"
company: "Expected Parrot"
source_id: "yc-expected-parrot-rss-aebdb8c877b2"
canonical_url: "https://blog.expectedparrot.com/p/a-patriotic-experiment-in-ai-powered"
published_at: "2026-07-16T16:20:26+00:00"
first_seen_at: "2026-07-27T02:15:59.471977+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:3b0f236bb8baffe7cabac5b2d76b73c732515ecec92f08a7a3828e84222919af"
---

# A Patriotic Experiment in AI-Powered Research

# A Patriotic Experiment in AI-Powered Research


### Expected Parrot can be used for reproducible data labeling.


[John Horton](https://substack.com/@johnjhorton)


Jul 16, 2026


**tl;dr - Using LLMs to explore historical documents is a fantastic application and Expected Parrot makes it even better/easier.**


Around the 4th of July my family and I saw a


[performance of the musical 1776](https://www.capenews.net/arts_and_entertainment/college-light-opera-company-celebrates-america-s-250-with-1776/article_89121006-2af8-4a3d-8755-7c5d45ae695f.html) , in which John and Abigail Adams play an important role. The Massachusetts Historical Society has


[digitized much of their correspondence](https://www.masshist.org/digitaladams/archive/browse/letters_1774_1777.php) . While it is convenient they are already digitized, vision-capable LLMs are remarkably good at reading old handwriting:


Thanks for reading! Subscribe for free to receive new posts and support my work.


Letter from Abigail Adams to John Adams


Her letter reports some bad behavior by the British (namely abusing a nice mahogany table and curtains/cushions):


This reminded me of one of my favorite


[Onion bits](https://theonion.com/third-amendment-rights-group-celebrates-another-success-1819569379/) :


*“This is a proud day for quarters-owners everywhere,” said the organization’s president, Charles Davison, in his keynote address. “Year after year, we have sent a loud and clear message to the federal government and to anyone else who would attack our unassailable rights: Hands off our cottages, livery stables, and haylofts.”*


This little excerpt gave me an idea: I should go through a bunch of her letters and compile her reports on British activity.


There’s already a growing recognition


[among economic historians](https://www.nber.org/system/files/working_papers/w35374/w35374.pdf) that LLMs can enable the use of data that otherwise would have been too expensive to use. That being said, using LLMs in this way forces researchers to play software engineer. And while coding agents help, there’s a better way: the Expected Parrot research agent and write the code to do this kind of work based on natural language. You might be thinking: wait - isn’t


[EDSL](https://github.com/expectedparrot/edsl) / Expected Parrot


[something for surveys](https://blog.expectedparrot.com/p/test-your-survey-on-synthetic-respondents) ? Yes, they are, but it turns out that the survey paradigm—with structured question types, piping, skip-logic, multi-modal/multi-model support, etc.—is also phenomenal for setting up and verifying complex labeling flows with LLMs.


## Expected Parrot Workflow


To start, I had Codex scrape the Historical Society website for letters during the Continental Congress years. Then I uploaded the letters to the Research Agent as a zip file and explained what I wanted done:


After doing some exploration and digging, the Research Agent asks me some questions to clarify my goals and the scope:


I also asked for multiple models (Expected Parrot supports a huge number out of the box):


comes up with a plan for me to review:


A key part of the plan is the skip logic: a screening question to first see if the letter mentions British behavior and then, if it does, have it label from a list of activities. It’s smart enough to use a checkbox question here since one letter could mention several kinds of activities.


It also helpfully added a question to summarize the letter contents with respect to British activities in a 3rd question:


Note also the mustache


` scenario.letter_text` in the prompt. This is expanded to when sent to the model with the actual scenario (in this case all the letters). This keeps prompts legible and the labeling instructions/context separate from the raw data. The actual


` ScenarioList` is constructed like this. A key point for reproducibility/legibility is that the coding agent is writing Python files to implement this labeling flow:


Because EDSL has a declarative syntax, the actual labeling job sent to Expected Parrot’s services is concise:


Code for actually launching / running and EDSL study


## Upshot: A written report on what we learned


The Research Agent executes the labeling flow and analyzes the data:


I


It doesn’t hide areas of model disagreement but instead surfaces them in helpful comparison visualizations:


If you wanted to, you could then send some sample of these letters to be labeled on Prolific. Another upshot of using coding agent for this task is that you can have it create custom HTML pages on the basis the data and share them with others. To me, it was natural to want an interactive timeline, which you can play with


[here](https://www.expectedparrot.com/research-agent/share/UtIfBAxFXTkRgHnLh9iujQ) :


As our research agent is based on a general coding agent, you can easily have it make bespoke HTML pages like this and then share them.


## Commercial Applications!


As we, Expected Parrot, do have investors who would probably not be excited about the “#1 Tool for Historical LLM based-workflows” let me ruin my perfect Pangram rating for everything above by asking Claude Fable what, based on the above, would be the commercial applications of this tech:


> <🤖> While EDSL is an open source package, Expected Parrot is a company—and the same workflow that made quick work of Abigail Adams's letters is exactly what production teams need. Swap the correspondence for insurance claims, contracts, support tickets, clinical notes, or survey open-ends, and the pattern holds: a screening question, some skip logic, a checkbox list of categories, and a summary—now running across thousands of documents and several models at once. The features that felt like nice-to-haves on a hobby project turn out to be the whole ballgame in commercial settings. Structured question types and declarative syntax keep the labeling logic legible and auditable. Reproducible Python files mean your pipeline isn't trapped in a chat window. And the multi-model disagreement visualizations do real work: instead of asking a stakeholder to trust a black box, you can
>
>
> *measure* where the models agree, route the contested cases to human reviewers on Prolific or internal teams, and hand over something defensible. That last part matters most. In regulated or high-stakes work—underwriting, compliance review, adverse-event abstraction—"the LLM said so" isn't an answer. A transparent flow that shows its screening logic, surfaces its uncertainty, and keeps a human in the loop for the hard cases is. Turns out the tool built for surveys was built for this all along. </🤖>


Want to try this out? Sign up at


[expectedparrot.com](http://expectedparrot.com/) . Accounts come with $25 in free credits and you can earn more by referring others. Also, this just scratches the surface of what’s possible.


Thanks for reading! Subscribe for free to receive new posts and support my work.
