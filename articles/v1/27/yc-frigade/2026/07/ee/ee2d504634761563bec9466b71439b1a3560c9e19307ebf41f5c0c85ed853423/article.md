---
schema_version: "1.0.0"
document_id: "ee2d504634761563bec9466b71439b1a3560c9e19307ebf41f5c0c85ed853423"
company_key: "yc-frigade"
company: "Frigade"
source_id: "yc-frigade-news-import-b5bda2b6da4c"
canonical_url: "https://frigade.com/blog/answering-is-the-easy-half"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-24T09:12:25.171648+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:4858413e1f8cb8152c7a38b90d35ce25816c22440c55f79637381579d0ca760b"
---

# Answering is the easy half

Today we're launching Skills, the biggest upgrade to the Frigade Assistant since we shipped it. The assistant already lives inside your product, answers your users' questions, and walks them through it. With Skills, it can take the action for them.


That gap matters more than it sounds. Watch someone use an in-product assistant for a few minutes and you'll see the same moment again and again. They ask it to do something: change the billing address, add a seat, re-run the export, cancel the order. And the assistant, helpful as it is, tells them how. We've seen this across customers in logistics, legal, and field services, and it's always the same. People don't open an assistant for directions, they open it to get the thing done.


Answering questions is the easy half of an in-product agent. Models are good at it, and getting an assistant to explain your product is mostly a retrieval problem. The half that changes how the product feels is letting the agent take the action. That's Skills.


## What a Skill is


A Skill is an action the assistant can take inside your product: a lookup, a create, an edit, a delete. They come from the assistant itself. You could wire up a tool call for every action by hand, and we started there, but it turns into an integration project that never ends and breaks every time the product changes. So the assistant learns the actions on its own. It uses your product the way a new user would, watching what each action does and writing it down so it can repeat it, and when the product changes it re-learns and the Skill updates itself. Christian led the build, and what he kept optimizing for was that you should never have to define your product to us.


To see how far that learning goes, we pointed the assistant at products we don't own. It learned to drive Jira with nobody mapping a single action for it.


Finding and pulling up a ticket in Jira.


Then we pointed it at Spotify. No integration, no setup, just the assistant working out how the app behaves and driving it end to end.


Playing an album, learned by using the app.


## You approve what it can do


An agent that teaches itself to take actions raises the obvious objection, and it's the right one. You don't want an agent one prompt away from deleting the wrong record. So a Skill only goes live when a person approves it. The assistant proposes the action, you review it, and you turn it on with no code. You decide what it can touch. Because the assistant only acts inside what the user can already see and do, it never becomes a way around your own permissions, and if you self-host with your own LLM keys, the action runs in your environment and never leaves it.


## Why this matters now


Skills does not replace explaining. The assistant still[answers questions](https://frigade.com/blog/how-to-deflect-support-tickets) and walks people through your product, and a lot of the time that is exactly what they want. But not always. Sometimes a user does not want to learn the steps, they want the thing done, and until now the only answer was to hand them[the steps](https://frigade.com/blog/onboarding-tours-break) anyway. Skills is for those moments: the assistant does it, in the product, instead of describing how.


That expectation is already shifting. Users are watching agents do the work for them in the tools they open every day, and they bring that expectation into your product. Every assistant is going to have to do more of this, especially as products keep changing and the actions a user might want keep growing. Okay, I am biased, but I actually think doing is quickly becoming the default, and an assistant that can only explain is going to feel a step behind.


## Get started


Skills are live now in the Frigade Assistant. If your product has real workflows in it, the kind where users keep asking your team to just do it for them, point the assistant at your app and see what it learns to do.[Book a demo](https://frigade.com/demo) and we'll get you set up. Either way, the teams that teach their product to act first will set the bar their users carry into every other tool they touch.
