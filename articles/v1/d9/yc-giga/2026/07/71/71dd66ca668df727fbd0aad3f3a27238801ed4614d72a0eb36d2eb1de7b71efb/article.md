---
schema_version: "1.0.0"
document_id: "71dd66ca668df727fbd0aad3f3a27238801ed4614d72a0eb36d2eb1de7b71efb"
company_key: "yc-giga"
company: "Giga"
source_id: "yc-giga-news-import-92619addbaaf"
canonical_url: "https://giga.ai/news/scout-in-slack"
published_at: "2026-07-27T12:00:00+00:00"
first_seen_at: "2026-07-27T02:18:41.623870+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:88de20f99735b88c440091aab7d29417d6fee2b6713887c6c53bafae9a2f3e18"
---

# Scout, now in Slack

## Every dashboard eventually leads to a Slack thread


Every support investigation starts the same way. A number moves. Someone screenshots it. And then the sentence that begins every investigation your team has ever run:


*Can someone look into this?*


It is a fair question. It is also, quietly, a request for somebody’s afternoon.


Because answering it properly means opening the tickets behind the number. Reading the summaries. Grouping them by what actually went wrong. Comparing the conversations that worked against the ones that did not. Finding the step in the agent that produced the difference. Working out how many customers it touched. Then deciding what to change, and what changing it is worth.


That is not a five minute question. It is a small research project, and it lands on whoever sits closest to the data, usually a support engineer who was in the middle of something else.


So it waits. Not because anyone is slow, but because the work is genuinely expensive. A day later someone posts a paragraph in the thread, and by then the question has either answered itself or stopped mattering.


Noticing that a number moved is cheap. Understanding why is the part that costs you a person.


## Dashboards report. They do not explain.


This is not a complaint about dashboards. They are very good at what they do. A dashboard will tell you containment is 82.6% and CSAT is 63.1%, that the second number is under your target, and it will do that continuously, for every day, without anyone asking.


What it cannot tell you is why.


Here is a case where that gap costs something real.


Containment is the share of conversations the agent handled without a person. It held in the 80s all week. CSAT slid. Those two numbers usually move together, so the pair reads as noise, or as two unrelated problems on two different teams.


They were not unrelated. They were the same tickets.


The billing refund flow was closing tickets as resolved the moment a refund went out, including for customers who had already asked to talk to a person. The ticket closed without a human, so containment counted it as a success. The customer had asked for help and been closed out, so they left one star.


Fifty six conversations, filed by the system as wins and by the customers as failures.


The dashboard said the agent was healthy. The dashboard was reading its own definition of healthy back to itself. Nothing about staring at it harder would have surfaced that, because the answer was never in the metrics. It was in the tickets underneath them, and reading those is manual.


## What an investigation actually looks like


Scout is the agent that runs in the[Giga console](https://giga.ai/scout) . It now works in your Slack, in the thread where the question gets asked. Not as a status bot that reads numbers back to you, but as the thing that goes and finds out.


Here is that investigation, as it actually ran.


Thread


#support-ops


⋮


✕


SP


Swapnil Panwala


Jul 27th at 7:06 AM


CSAT dropped again yesterday and containment looks fine. Not sure what to make of it.


2 replies


CF


Chris Fisher


Jul 27th at 7:08 AM


@Scout


pull contained and CSAT for yesterday. Can you also explain the theme around the ratings of 1?


✅ 1


Scout


APP


Jul 27th at 7:09 AM


Here is yesterday, closed out.


- Containment **82.6%** , 5,120 chats, 890 escalated
- CSAT positive **63.1%** , 540 rated, 341 positive


Containment held in the 80s. CSAT is the one to look at: your target is 80%, so yesterday came in well under it. There are 118 one star ratings and 112 of them have summaries, so I can tell you why rather than guess. Want me to go through them?


Also send to **#support-ops**


The question your team already asks, answered where it was asked. The composer is live. Ask it something.


It starts where a good investigator starts: is this worth investigating at all. The number arrives next to your target rather than next to last week, so the first line tells you whether to keep reading.


Then it says what it has to work with. 118 one star ratings, and summaries on 112 of them. That is the evidence, stated before anything is concluded from it, which is the order those two things have to come in.


Thread


#support-ops


⋮


✕


CF


Chris Fisher


Jul 27th at 7:11 AM


go ahead


✅ 1


Scout is reading 112 ticket summaries ** ** **


Scout


APP


Jul 27th at 7:13 AM


I read all 112 summaries and clustered them. Ticket status accounts for most of it: **56 of the 118** were closed as *resolved* and 44 were abandoned mid conversation. The last group only shows up in the summaries.


- **56** closed as resolved, customer still needed a person
- **44** customer left mid conversation
- **18** answered a different question than the one asked


The first group is the lever. Those close as successes and still land one star, so containment looks healthy while CSAT drops. Six have no summary, so treat the bucket split as close but not exact.


Also send to **#support-ops**


112 summaries read and clustered. 56 of the 118 closed as resolved.


Then it reads them. All 112.


The clusters are the finding, and Scout is careful about where each one comes from. Ticket status is a field, so it already knows that 56 of the 118 closed as resolved on customers who still needed a person, and that 44 walked away mid conversation. The third group only exists in the summaries: 18 got an answer to a question nobody asked. Three different failures wearing the same one star rating, and only the first one explains why containment and CSAT came apart.


That is the move a dashboard cannot make. Not because the data is hidden, but because the pattern only exists once somebody has read the conversations and decided what they have in common.


Thread


#support-ops


⋮


✕


CF


Chris Fisher


Jul 27th at 7:15 AM


can we fix the first group?


Scout


APP


Jul 27th at 7:16 AM


Yes. The billing refund flow closes the ticket once it has issued a refund, even when the customer has already asked for a person. That is the step producing those 56.


Here is what I would change:


- Hand off to a person when the customer asks, instead of closing as resolved
- Stop counting a handoff as contained, so the rate stops flattering itself


Those 56 are about **10 CSAT points** on yesterday's rated volume, and it is one flow, so the change is small. I would not bank on the exact number until it has run.


I can build it and test it against the current agent, and hold it for your approval before anything goes live. Want me to start?


Also send to **#support-ops**


The step producing it, the change, and an offer to test it first.


And it does not stop at the diagnosis. It names the workflow step producing the problem, proposes the two changes that would fix it, puts a number on what fixing it is worth, and offers to build it and run it against your current agent before anything reaches a customer.


Question, evidence, pattern, root cause, blast radius, fix, what the fix is worth, validation. That is the shape of the answer your support engineer eventually writes. It arrived in the thread, in minutes, while the question was still live.


Dashboards identify symptoms. Investigations identify causes. Only one of those has been automated until now.


## The part that makes the rest worth trusting


Six of the 118 ratings had no summary. Scout said so, in the same message as the finding: the counts hold, but treat which bucket those six landed in as close rather than exact.


That is a small detail and it is the most load bearing thing on this page. An investigator who never reports what they could not check is not being confident. They are guessing, and you have no way to tell which parts.


The same rule applies when the limiting factor is not in the agent at all. A knowledge base article that was never written. An API the agent has no access to. A field nobody fills in. Those are not problems we can fix from our side, and the honest answer is to say so and attach the tickets that prove it, rather than propose a change that was never going to work.


An investigation you can act on has to tell you where its own edges are. Scout is built to say “I could not verify this” rather than round it off into something that sounds finished.


## Not a bot. An investigator.


Scout is not a support bot for your customers. It never touches them. It is for the team that runs the agents: the people who currently open a ticket with us, or ask an engineer, or wait.


It is in Slack for one reason, which is that Slack is where the question already gets asked. The thread is the unit of work, so Scout holds the context of the one it is in instead of answering each message cold. Ask a follow up and it is still on the same investigation.


And it is the same agent that runs in the console. Nothing is simplified for the chat window, and nothing ships without your approval.


## Turn it on for your workspace


Scout in Slack is rolling out per team, in beta.[Talk to us](https://giga.ai/contact) and we will switch it on for yours.


The threads shown above use sample data.
