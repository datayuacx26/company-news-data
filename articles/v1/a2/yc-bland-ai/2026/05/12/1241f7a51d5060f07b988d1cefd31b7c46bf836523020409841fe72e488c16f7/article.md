---
schema_version: "1.0.0"
document_id: "1241f7a51d5060f07b988d1cefd31b7c46bf836523020409841fe72e488c16f7"
company_key: "yc-bland-ai"
company: "Bland AI"
source_id: "yc-bland-ai-news-import-c77b84ac3c61"
canonical_url: "https://www.bland.ai/blog/bland-evals-evaluate-real-calls-for-quality-at-scale"
published_at: "2026-05-28T19:00:00+00:00"
first_seen_at: "2026-07-21T10:41:07.173500+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:b04f6b32891e293b41c36ca806fc9cc30408432420b9d2975d0162663969be85"
---

# Bland Evals: Evaluate real calls for quality at scale

We built Evals to answer one question: Why are calls failing?


Bland Evals walkthrough


Evals automate the manual work of reading, searching, triaging, debugging, auditing, and listening through call logs at scale.


Today, the only way to know something is wrong is when your call success rate drops. You have to comb through each call, read every transcript, and listen to audio just to pinpoint the issue. You may have to do this 100 times to find out how widespread the problem is. Evals can pull those signals from up to 5,000 calls at once, faster than you can drink a cup of coffee.


Evals are essentially LLM judges that read transcripts and listen to audio to measure the quality of your calls. They can grade specific dimensions of a call such as resolution, tone, hallucination, audio quality, and more. You can run them on real or test calls to get per-call verdicts and aggregate scores. You can also track quality over time, compare prompt or pathway changes, and catch regressions before they reach production.


An Eval Agent is like your own custom version of Norm that you can configure to go through all your calls. You give the agent a prompt or rubric, choose what context it can use, and it scores, labels, explains, and cites evidence and reasons across calls.


[Norm](https://www.bland.ai/blog/norm-build-voice-agents-from-prompt) can even help you create Eval Agents from customer issues to answer “How often is this happening across my calls?”


Getting started is simple:


1. **Create a new Workbench.** A Workbench combines multiple Eval Agents and runs them together across selected calls. This produces broader signals like overall scores, failure-mode patterns, QA trends, target-hit rates, and analytics across call batches.
2. **Select up to 5,000 calls to grade.** Or use Norm to create the sample set.
3. **Attach up to 10 eval agents to gather signals.** You can start with one of our pre-built templates or build your own.
4. **Set a pass threshold.**
5. **Run an experiment** to score a batch of calls.


We’ve built templates for 14 of the most common eval agents. These are helpful starting points, but the real value comes from tailoring prompts and agents to the customer’s actual workflow, goals, and requirements.


Templates include:


- **Conversational quality** - Did the agent listen, acknowledge, and adapt to the customer? Or did they ignore or talk over them?
- **Resolution** - Did the call end with the desired outcome? Did it answer the customer's question, resolve their issue fully, or take the right next step?
- **Discovery** - On outbound calls, did the agent ask open-ended questions to understand the customer's needs?
- **Scheduling clarity** - Were scheduling details communicated clearly?
- **Prompt and pathway adherence** - Did the agent follow its prompt, persona, and pathway structure?
- **Lead opportunity handling** - Did the agent detect when the lead signaled a real opportunity?
- **Audio quality** - Was the audio clear for both parties?
- **Bland tone** - How was the agent’s style of speaking? Was it on- or off-brand?
- **Issue understanding** - Did the agent demonstrate understanding of the customer’s issue?
- **Objection handling** - When the customer raised an objection, did the agent respond with the right framing?
- **Appointment booked** - Was the appointment confirmed during the call?
- **Hallucination detection** - Is the agent making things up or stuck in an infinite loop?
- **Runtime decision quality** - Did the agent make good decisions?
- **Transfer request behavior** - How often are customers asking to be transferred? Diagnose why and track transfers over time.


You can also save your own agents as reusable templates for your organization.


For each call in a run, every attached agent produces a verdict — for example, the call was perfectly resolved, but the agent's tone was off-brand. Individual verdicts are combined into one weighted score per call, then compared against the pass threshold. Calls that don't pass can be drilled into to see exactly where things went wrong.


One note on scoring: verdicts flagged as insufficient evidence (when a call lacked enough context to grade) are excluded from the averages so they don't skew your results.


**Score distribution** gives you a bird's-eye view of call quality so you can see how calls are performing in aggregate.


When to use Evals:


- Someone would otherwise manually review call logs
- A customer wants to know how often an issue happens
- You want to know why a call is performing better or worse
- Need QA/compliance scoring at scale
- Want to audit failure modes across calls at scale
- Need qualitative signals from conversations or audio


Example use cases:


- Hallucination / unsupported claim detection
- Prompt or pathway adherence reasoning
- Transfer quality and caller refusal analysis
- Ability to reason on how good or bad a lead is based on the conversation
- Sentiment, engagement, and call-flow scoring
- Failed transfers, system issues, missing data, or bad outcomes
- Audio defects when recordings are available
- Labeling and categorizing calls by applying pathway tags to automatically flag issues


Evals are the latest addition to our suite of monitoring tools to ensure your calls sound amazing and deliver real ROI in production. Give it a try and let us know what you think. And if you’d like to learn more, book a demo[here](https://www.bland.ai/book-a-demo) .
