---
schema_version: "1.0.0"
document_id: "44b7946c350c9b38a75ffc9fd2ab9be1f09d8895129a3e61c29b5c98ebf2f049"
company_key: "yc-dot"
company: "Dot"
source_id: "yc-dot-news-import-8b97b36288bf"
canonical_url: "https://www.getdot.ai/blog/data-contributors-wont-write-sql"
published_at: "2026-06-06T00:00:00+00:00"
first_seen_at: "2026-07-21T16:52:13.925276+00:00"
fetched_at: "2026-07-28T21:46:31.447870+00:00"
content_hash: "sha256:da66d5cfa16a5c9d51fff3f9eb9d460833ed2f2630a505426246c80d354e82bc"
---

# Your Next Data Contributors Won't Write SQL

**Somewhere in your company this week, a RevOps manager looked at an AI agent's answer and typed: "That's wrong, we exclude trial accounts since Q3."**


That message is a contribution to your data model. Nobody wrote SQL. Nobody opened a pull request or filed a ticket and waited two sprints. A business user corrected a metric definition mid-conversation, in plain language, and went back to work.


## The skills gate is gone


For twenty years, contributing to the data model required passing a gate: SQL, git, dbt, a review process. The gate kept throughput low, and it kept most business knowledge out. The person who knew that partner-referred accounts get excluded from churn was almost never the person who could encode that rule. So the knowledge lived in Slack threads and people's heads, and the model lagged the business by months.


The data team wasn't a bottleneck because they were slow. They were a bottleneck because they were the only ones who could write.


Agents removed the writing barrier, and they did it quietly, as a side effect of letting people ask questions. Every correction, every "actually, we define it this way," every clarification a user gives an agent is a proposed change to what your metrics mean. Multiply that by everyone in the company who talks to an agent, and you have more contributors to your data model than your data team has ever had. Whether you wanted them or not.


The question isn't whether business users will shape your definitions. They already do, one conversation at a time. The question is whether those contributions land somewhere governed or evaporate into chat history.


## From author to editor


What does it take to absorb contributions from people who don't write code? A correction in chat has to become a reviewable artifact: a visible change ("active customer" now excludes trials), an author, a timestamp, an approval step. We built training in Dot around exactly this loop. The agent learns from the conversation, and what it learned is a thing a human can inspect, edit, or reject. Not a vibe buried in a transcript.


That shifts the data team's job from author to editor. Instead of writing every definition, they review the ones the business proposes. This is not a demotion. Editors decide what gets published. And the proposals are better informed than they used to be, because they come from the person who actually knows the business rule, not from a translation of a translation.


Gatekeeping becomes something closer to code review: faster, more collaborative, and visible to everyone.


## What still can't be delegated


Three things stay human no matter how good the tooling gets: responsibility, budget, and commissioning. Someone accountable decides which proposed meaning gets accepted. Someone decides what the work is in service of, and what it's worth spending on it. An agent can draft a definition and a business user can propose it, but a human has to own it.


The org question behind every metric fight, who gets to decide what "churn" means, doesn't disappear because the typing got easier. It gets sharper, because the answer now gets applied at machine speed, across every conversation in the company.


## Don't hold the gate


When I talk to data leaders about this, the first instinct is often to lock things down: route everything through the team, freeze the definitions, keep the contributors out. I understand the instinct. But the teams that win this transition won't be the ones with the strongest gate. They'll be the ones with a review loop fast enough to accept help.


*If this excites you, we'd love to hear from you.*[Get in touch](https://www.getdot.ai/) *.*
