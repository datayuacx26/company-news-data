---
schema_version: "1.0.0"
document_id: "9f666e5109fcc436fc6080b4458ad800d2b15451dc96d7d43e6eed82c654cac6"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2023-02-22-technical-decision-making/"
published_at: "2023-02-22T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:d1745858c710a6b76d131e7db4f3372967e2fd65cea462cd6b97b0dd746ff986"
---

# Technical Decision-Making

As a part of the series of posts already mentioned on WARP -[A Web Application Rewrite Project](https://tech.trivago.com/post/2022-05-16-warp-a-web-application-rewrite-project/) , we are disclosing our process of making technical decisions. We hope that you find this process helpful. Maybe you can even pull something out for your own projects.


## TL;DR


This is a general outline for making technical decisions and should be taken as a set of guidelines rather than rules. Its purpose is to provide a toolset to help with efficiency, gathering appropriate input, ensuring transparency and high likelihood of buy-in.


## Determine Scope / Impact of your decision


This process must not be applied to every little decision. Some examples and guidelines when this process should be applied:


- Does it affect multiple people/teams?


- Change of an API Scheme
- Workflow changes (tasks, PRs, releases, tooling)


- Will it cause additional work or adaptations in other areas?
- Does it carry a financial impact?
- Will the change incur additional technical debt, cause maintenance, etc.?
- Will the change force large investments in (re-)learning, e.g., by introducing a new library or technology? Is there a current solution that can solve this?


Always be sure when introducing a custom solution instead of using a readily available one that you have reviewed the possibilities for external solutions.


## Time boxing


This will be heavily dependent on the decision being made, so it will be up to you to set a reasonable timeline for your decision. However, it is important to have a fixed deadline for the decision to be made as to not leave it open ended to never resolve or to extend much longer than necessary.


## Process


1. Create a Design Document:[Template](https://tech.trivago.com/doc/Design_Document_Template.md)
2. Have the document reviewed by all people affected by the change.
3. Form a working group to review the proposal and for making a decision
4. Decision meeting
5. Documentation of the decision outcome in the design document


## Roles in a decision-making process


Role Responsibilities


Decision owner / initiator / project lead / moderator - Explain the context
- Define what the decision is about, including technical implications, alternatives, tradeoffs, possibly an example implementation
- Call for participation
- Possibly: Choose final group to take part in decision meeting
- Possibly: Maintain a document to asynchronously discuss beforehand
- Make sure the decision is documented in an Architecture Decision Record (ADR)


Participants - Acquire knowledge to form an opinion


Observers (optional) - Listen and learn


## Decision owner


By default, decisions are owned by the project lead, who is ultimately responsible for every bigger decision. Alternatively, the project lead can assign a decision owner. It can be a good idea to have the decision owner not be the individual proposing the change when possible to help avoid bias.


## Participants in the decision process / meeting


In general, the following people should be invited to a decision process:


- Whoever is affected by the decision must at least be represented
- People who have expertise on the topic
- Stakeholders


That said, a decision working group should typically have 4-6 participants, so that the discussion stays focused and efficient. With high-impact decisions, it can also be more.


When people are interested in the discussion, they can join as observers, unless the decision owner for some reason excludes observers from the decision meeting (should be the exception).


## High-impact decisions


When the impact of a decision is very high, these are some best practices:


- Communicate earlier and more often, to a wider audience.
- Present the topic in a guild meeting, or in some other live event (not just in a document).
- Additionally, provide a document where people can ask questions and comment.
- Potentially allow for more observers in the decision meeting.


## Commitment


Some topics are complex and require preparation. In those cases, the decision owner is expected to set up a document where participants can discuss different points, and explain their perspective in advance. If a participant fails to do this, they cannot expect to take part in the decision meeting (unless as an observer). This ensures that all participants are informed, and show commitment to the topic.


## Arguments


Constructive arguments, collective experience and alternatives exploration are the root of fruitful and solid decisions. In that sense, every argument must:


1. Be stated with well-mannered phrasing
2. Support the claim with technical facts
3. Include the change criteria that would resolve it


## The decision making process


The decision making process follows a certain structure and aims for maximum transparency:


- All arguments are collected, in a way that is visible for everyone.
- If possible, identify the arguments that are truly important already in a preparation document, and list the remaining ones separately.
- Every comment/feedback is addressed.
- If possible, the group makes a close-to-unanimous decision. This is the happy path.
- Focus on the 3-4 biggest options. The decision owner has to make the decision in the end, but has to explain their decision with respect to these options.
- Note: The decision owner is not bound to a majority vote. However, they need to be aware of the consequences and take responsibility if they go against the majority.
- A decision always has to be tied to action points: We decided A, so we will do B, C, and D next week.
- The decision is documented. The decision owner is responsible for this.


## Revisiting a decision


Revisiting a decision is nothing else than making a new decision that supersedes the old one. If someone sees the need to revisit a decision, they should address the owner of the original decision or the project lead.


## Code of conduct


Even though the meeting owner has a special responsibility to maintain a respectful, friendly, and productive atmosphere during the meeting, this is also the job of every single participant. Everybody is responsible to raise awareness about this during the meeting should it be necessary (e.g., when things become tense, to remind people of importance of listening to each other and compromise).


## Conclusion


Making decisions that will have a lasting impact is hard. There is often a lot of complexity and uncertainty. Using a structured process like the one presented here makes sure that the best-suited people collaborate on the decision, and really contribute. It is not a guarantee that every decision made will be 100% perfect, but it increases the likelihood of success, and, above all, transparency. This is already a lot better than one or two people deciding everything, without sharing their reasoning.
