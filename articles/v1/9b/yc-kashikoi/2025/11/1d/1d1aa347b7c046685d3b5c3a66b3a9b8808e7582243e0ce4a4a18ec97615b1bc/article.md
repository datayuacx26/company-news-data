---
schema_version: "1.0.0"
document_id: "1d1aa347b7c046685d3b5c3a66b3a9b8808e7582243e0ce4a4a18ec97615b1bc"
company_key: "yc-kashikoi"
company: "Kashikoi"
source_id: "yc-kashikoi-rss-651c469da509"
canonical_url: "https://blog.getkashikoi.com/2025/11/11/newagent-benchmark-report-claude-comparison/"
published_at: "2025-11-11T15:28:51+00:00"
first_seen_at: "2026-07-25T10:30:20.185118+00:00"
fetched_at: "2026-07-28T20:55:21.781978+00:00"
content_hash: "sha256:2f33f5a8dea9ccfa49bc7ae4bc44e7739de059814a128e8dd75b972de69e8325"
---

# NewAgent Benchmark Report

# Executive Summary


We benchmarked NewAgent against Claude3.5 (Claude) to understand its Key Strengths and areas of improvement. We also provide ourMethodology andKey Benchmark Statistics to provide context for how and why of our approach.


Overall our data shows that NewAgent and Claude are very competitive with each other with NewAgent showing 12% advantage over Claude in specific tasks.


## Key Findings


- *External Search Tasks* : Claude has a ~20% edge over NewAgent when the user wants external examples similar to the vulnerabilities being discussed in the CVE report.
- *Report Focus Tasks:* ** NewAgent shines over Claude in 30% of the conversations where the higher quality response needs to focus on details already present in the CVE report.
- *Writer Intensive Tasks:* ** For writer intensive tasks both agents are rated equally or Claude consistently has a small edge.
- The overall behavior of NewAgent and its Claude is captured in Agent Behavior Stats. It shows a healthy distribution in` writer-*` states which speaks to the wider set of capabilities that NewAgent writer supports.


More information about all of these conclusions can be found in theKey Findings section and in the[Task Performance Table](https://docs.google.com/spreadsheets/d/1Yvs1g65nnhaW8s7FQ0BBkNU8TUlZlyzeidD7u_4bw_s/edit?gid=2022325617#gid=2022325617&range=A2) .


## Key Recommendations


- Our key findings related toExternal Search Tasks and *Report Focus Tasks* indicate that NewAgent’s output could be greatly enhanced when the right external knowledge is available in its context. This can be done by developing or improving an existing retrieval system backing NewAgent. Using our benchmarking system we can help the team achieve the right tradeoff between focusing on the CVE report while tapping into external knowledge sources to best serve the user’s needs.
- The key findings on the *Writer Intensive Tasks* indicate that the NewAgent team can streamline resources (prompts/code) by “passing through” such tasks to Claude and avoid the need to maintain them.


## Findings Index


Result Category Recommendations Link Results Table


External Search TasksRecommendation[https://docs.google.com/spreadsheets/d/1Yvs1g65nnhaW8s7FQ0BBkNU8TUlZlyzeidD7u_4bw_s/edit?gid=2022325617#gid=2022325617&range=A2](https://docs.google.com/spreadsheets/d/1Yvs1g65nnhaW8s7FQ0BBkNU8TUlZlyzeidD7u_4bw_s/edit?gid=2022325617#gid=2022325617&range=A2)


Report Focus TasksRecommendation[https://docs.google.com/spreadsheets/d/1Yvs1g65nnhaW8s7FQ0BBkNU8TUlZlyzeidD7u_4bw_s/edit?gid=2022325617#gid=2022325617&range=A5](https://docs.google.com/spreadsheets/d/1Yvs1g65nnhaW8s7FQ0BBkNU8TUlZlyzeidD7u_4bw_s/edit?gid=2022325617#gid=2022325617&range=A5)


Writer Intensive TasksRecommendation[https://docs.google.com/spreadsheets/d/1Yvs1g65nnhaW8s7FQ0BBkNU8TUlZlyzeidD7u_4bw_s/edit?gid=2022325617#gid=2022325617&range=A18](https://docs.google.com/spreadsheets/d/1Yvs1g65nnhaW8s7FQ0BBkNU8TUlZlyzeidD7u_4bw_s/edit?gid=2022325617#gid=2022325617&range=A18)


Simplification TaskRecommendation[https://docs.google.com/spreadsheets/d/1Yvs1g65nnhaW8s7FQ0BBkNU8TUlZlyzeidD7u_4bw_s/edit?gid=2022325617#gid=2022325617&range=A34](https://docs.google.com/spreadsheets/d/1Yvs1g65nnhaW8s7FQ0BBkNU8TUlZlyzeidD7u_4bw_s/edit?gid=2022325617#gid=2022325617&range=A34)


Agent Behavior OverviewRecommendation[https://docs.google.com/spreadsheets/d/1Yvs1g65nnhaW8s7FQ0BBkNU8TUlZlyzeidD7u_4bw_s/edit?gid=0#gid=0&range=A1](https://docs.google.com/spreadsheets/d/1Yvs1g65nnhaW8s7FQ0BBkNU8TUlZlyzeidD7u_4bw_s/edit?gid=0#gid=0&range=A1)


## Key Benchmark Statistics


- We conducted 50 interviews consisting of 3 questions spanning 12 CVE reports with 3-5 interviews per report to ensure diversity of topics.
- Our security engineer was encouraged to simulate questions with the following intents:` ask_source, ask_simplify, ask_actionable_steps, ask_title, ask_detail, search_similar, ask_summary, review, ask_templatize, ask_update` in a logically consistent manner.


## Methodology


### Setup Security Engineer Simulator


- Kashikoi built a user simulator that simulates a security engineer who would typically use the NewAgent Writing Assistant.
- This security engineer simulator is not prompt-based but consists of a world model which steers an LLM to interview NewAgent.
- This provides us several advantages in benchmarking. The key advantage is that the system is able to autonomously interview NewAgent, adapting the questions and their sequence to NewAgent’s responses, ensuring that follow up questions are logical while aligned with a typical NewAgent user.
- Our model was encouraged to simulate questions with the following intents:` ask_source, ask_simplify, ask_actionable_steps, ask_title, ask_detail, search_similar, ask_summary, review, ask_templatize, ask_update` in a logically consistent manner. It was also modeled to ask for simplification and explanations around sources of reported vulnerabilities more often than other questions at the start of an interview. Such questions are a good starting point for better follow up questions.
- Kashikoi also built a wrapper around the NewAgent GraphQL endpoint, enabling us to send and receive messages between the simulator and NewAgent just like a real user would.


### Run NewAgent and Baseline Interviews


- We chose Claude3.5Sonnet (Claude) as a baseline agent for NewAgent’s evaluation. In the future this could be another version of NewAgent, or any other agent of your choice.
- Since we use the same Kashikoi security engineer world model to perform the interview with both agents, we are able to follow the same state sequence for both interviews.
- As one would suspect, these state sequences do not always align. When the sequences are aligned, we can observe the differences in *performance;* and when the state sequences do not align, we can observe *behavioral* gaps. These give us a wholistic view of both agents’ performance.
- We ran two parallel sessions; one with NewAgent as the agent and one with Claude. These sessions consisted of 50 interviews with 3 questions per interview. These interviews were conducted over April 4-15, 2025.
- We used the data that was provided to us in theseNewAgent CVE Reports to ensure a diversity of topics in the interviews.
- Do note that all of these interviews were multi-turn, and we did not perform any testing of single turn questioning. One turn is a user question combined with an agent’s response.


### Run Evaluation


- After simulating the interviews, we ran an independent evaluation of them using OpenAI’s o3-mini-2025-01-31. This model was chosen since Claude is our baseline agent; if we did not do this it could potentially be biased in evaluating itself.
- Our world model provides an advantage as we can tie in specific evaluation questions for the behavior of the agent without worrying about the exact form of the CVE and/or user question that is input to both agents.
- Please seeEvaluation Criteria for more information about our evaluations.


## Key Findings


- ***External Search Tasks*** : Claude has a ~20% edge over NewAgent when the user wants external examples similar to the vulnerabilities being discussed in the report:Task Performance Table
- A snippet from an interview session and its evaluation


- ***Recommendations***


- It is possible that the NewAgent team was not intending its agent to service the search use case. If this is the case, we’d be happy to reflect that change in the security engineer world model and move the search states back into the benchmark when the team wants to focus on the search use case.
- We could easily run the same benchmark against more specialized search or RAG agents like Perplexity to help the team understand the headroom that NewAgent’s internal retrieval stack has.


- ***Report Focus Tasks*** : NewAgent shines over Claude in 30% of the conversations where the higher quality response needs to focus on details already present in the CVE report: Task Performance Table.


- These tasks are represented by` user-ask_actionable_steps=>(writer-instruct,writer-suggest_fix` ) ,` user-ask_detail=>(writer-give_details,writer-inform)` in the benchmark.
- NewAgent performance is articulated well in this example evaluation snippet:


- This result validates the need for an agent like NewAgent which benefits from the rich report data the NewCo platform has over and above that available to LLMs to service NewCo users.


- ***Recommendations***


- This result when placed in the context of the search performance indicates that NewAgent’s output could be greatly enhanced when the right external knowledge is available in its context. This can be done by developing or improving an existing retrieval system.
- We would love to work on creating a benchmark that helps the team achieve the right tradeoff balance in NewAgent’s context, one of focusing on the report while tapping into external knowledge sources to best serve the user’s needs.


- ***Writer Intensive Tasks*** :For writer intensive tasks both agents are rated equally or Claude consistently has a small edge:Task Performance Table.


- These tasks are represented by` user-ask_templatize ⇒ writer-templatize_submission` ,` ask_title ⇒ writer-suggest_title` and` user-ask_summarize => writer-ask_summarize` in the benchmark.
- ***Recommendations***


- This suggests that writer intensive tasks can be passed through to Claude and the NewAgent team does not need to maintain extra prompts/code for them.
- Running our benchmark on a continuous basis would also help detect model shifts and alert the team to when they could stop “passing through” such user questions to Claude directly.


- ***Simplification Task** :*` user-ask_simplify ⇒ writer-simplify` behavior is one of the most commonly displayed across both agents:[Agent Behavior Stats](https://docs.google.com/spreadsheets/d/1Yvs1g65nnhaW8s7FQ0BBkNU8TUlZlyzeidD7u_4bw_s/edit?gid=0#gid=0&range=A1) . NewAgent and Claude are rated to be equally good 80% of the time while Claude is rated better on conciseness 23% of the times. This behavior is observed more often (+4) in Claude than NewAgent:[Task Performance Table](https://docs.google.com/spreadsheets/d/1Yvs1g65nnhaW8s7FQ0BBkNU8TUlZlyzeidD7u_4bw_s/edit?gid=2022325617#gid=2022325617&range=A2) .
- The overall behavior of NewAgent and its Claude is captured inAgent Behavior Stats. It shows a healthy distribution inwriter-*states which speaks to the wider set of capabilities that NewAgent writer supports.


- The most common behavior for NewAgent’s writer is` user-ask_title → NewAgent-suggest_title` whereas the baseline agent’s most common behavior is` user-ask_simplify => writer-simplify` . This is further evidence of the preferred behavior of NewAgent writing use case as a writing assistant (as we presume is intended by its prompt instructions).
- **To put this in perspective, imagine if NewAgent was only restricted to writing summaries. Our interviewer’s adaptive process would start with questioning NewAgent on a wide range of writing tasks but then converge on a discussion over summarization simply because NewAgent’s responses would only be around that programmed action. Consequently, a raw count of states would be skewed toward the**` **writer-summarize**` **state.**
- A healthy distribution over different writer states is the desired situation here which is what we observe overall across the benchmarking interviews.


## Future Work


1. Enhance NewAgent’suser simulator model as per the team’s testing needs, including but not limited to creating net new models for features and behaviors NewCo wants to build for.
2. Create a set of static tests from these simulated sessions to help developers iterate over a fixed set of questions until the desired performance is achieved on the set.
3. Instead of only using Claude as our baseline, we could replace it with another version of NewAgent to give key insights into the impact of prompt and system changes the team may be experimenting with.
4. Run autonomous benchmarking for other NewAgent use cases and regularly test the capabilities of NewAgent while it is in active development.


## Appendix


### Results Sheets


- Task Performance –[https://docs.google.com/spreadsheets/d/1Yvs1g65nnhaW8s7FQ0BBkNU8TUlZlyzeidD7u_4bw_s/edit?gid=2022325617#gid=2022325617&range=A2](https://docs.google.com/spreadsheets/d/1Yvs1g65nnhaW8s7FQ0BBkNU8TUlZlyzeidD7u_4bw_s/edit?gid=2022325617#gid=2022325617&range=A2)
- Agent Behavior –[https://docs.google.com/spreadsheets/d/1Yvs1g65nnhaW8s7FQ0BBkNU8TUlZlyzeidD7u_4bw_s/edit?gid=0#gid=0&range=A1](https://docs.google.com/spreadsheets/d/1Yvs1g65nnhaW8s7FQ0BBkNU8TUlZlyzeidD7u_4bw_s/edit?gid=0#gid=0&range=A1)
- All States Performance –[https://docs.google.com/spreadsheets/d/1Yvs1g65nnhaW8s7FQ0BBkNU8TUlZlyzeidD7u_4bw_s/edit?gid=466611144#gid=466611144&range=A2](https://docs.google.com/spreadsheets/d/1Yvs1g65nnhaW8s7FQ0BBkNU8TUlZlyzeidD7u_4bw_s/edit?gid=466611144#gid=466611144&range=A2)


### State Graph for interviews using report #2978866


### Evaluation Criteria


All of our evaluations for this report were of the[LLM as Judge](https://huggingface.co/learn/cookbook/en/llm_judge) type – where we wrote prompt templates to have o3-mini evaluate and compare NewAgent vs. Claude. Our evaluation prompts were customized to the user simulator model states. This means the evaluations are always relevant given the context of the situation. We’ve provided a subset of our evaluation prompts below:


- “Is NewAgent’s template more relevant than Claude’s template from a security engineer’s perspective?”
- “Is NewAgent’s template more concise than Claude’s template from a security engineer’s perspective?”
- “Is NewAgent’s template more accurate than Claude’s template from a security engineer’s perspective?”
- “Does NewAgent ask better follow up questions if it doesn’t have all the information required to generate the template compared to Claude?”
- “Is NewAgent’s report update more relevant than Claude’s report update from a security engineer’s perspective?”
- “Is NewAgent’s report update more concise than Claude’s report update from a security engineer’s perspective?”
- “Is NewAgent’s report update more accurate than Claude’s report update from a security engineer’s perspective?”
- “Does NewAgent ask better follow up questions if it doesn’t have all the information required to update the report compared to Claude?”


### NewAgent CVE Reports


These are the NewAgent CVE reports we chose from those provided to us by NewCo.


- List of NewCo reports


- *Note: While NewCo had reports for this use case, we can also use our simulation engine to create a diverse set of test resources.*


### Like this:


Like


Loading…
