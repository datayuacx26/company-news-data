---
schema_version: "1.0.0"
document_id: "06e887119d58c83894a1076ac3cc58e3c2f53e1f772b07ed8168fe0bb064c058"
company_key: "yc-frigade"
company: "Frigade"
source_id: "yc-frigade-news-import-b5bda2b6da4c"
canonical_url: "https://frigade.com/blog/state-of-in-product-ai-2026"
published_at: "2026-07-06T00:00:00+00:00"
first_seen_at: "2026-07-24T09:12:25.171648+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:9e57fce0db89f43a95299f36a2655a0184553fea9b1da542620770cff7861675"
---

# State of in-product AI for SaaS, 2026: lessons from a year in production

Every in-product AI demo looks good. A demo is the easiest thing in the world to stage, and it tells you nothing about the part that matters: whether a user comes back the next day and uses the thing again.


This year we can finally answer that from production. In-product AI for SaaS crossed from prototype to live traffic, and across a year of Frigade Assistant usage the pattern is consistent. Users reach for the assistant to get work done, and they come back the next day to do it again. What earns it that spot is one thing a knowledge-base chatbot can't do: act inside the live product.


Three findings from that data.


## Finding 1: once a user comes back, they go deep


Distribution among users who came back to ask a second question. About 62% of returning users asked three or more.


If the assistant were only a last-ditch "I'm stuck" tool, you'd see a question or two and done. You don't. When a user comes back to ask a second question, most keep going. About 62% of returning users ask three or more, and roughly 1 in 10 go on to ask eleven or more.


Depth like that shows up once the assistant becomes something users reach for to do work, and the questions shift from "I'm stuck on this" to "walk me through this."


Products keep shipping new features and teams keep turning over, so the questions never really stop. They move from "how do I get started" to "how do I do this new thing," which is why the same users keep coming back long after they've learned the basics.


This is also the cleanest read on whether an in-product AI is working, yours or a vendor's. Total questions asked is a vanity number: a high count can just mean users are lost and clicking around. The number to watch is whether they come back to ask a second question. That's the part a demo can't fake.


## Finding 2: about 1 in 3 answers is a guided walkthrough, not a wall of text


About 35% of the assistant's responses are guided walkthroughs rendered inside the product.


About 35% of the assistant's responses are guided walkthroughs. Because it's workflow-aware, the assistant runs the user through the flow inside the live product, on the screen they're already on.


Ask a knowledge-base chatbot how to do something and you get a list of steps. You still have to map those steps onto your own screen and find the buttons yourself, because the bot doesn't know what page you're on or what state your account is in.


This is where in-product AI splits from the support chatbots stapled to a knowledge base. Intercom's Fin, and conversational support agents like Sierra and Decagon, live on the chat layer next to your docs. They answer well. They can't drive the buttons on the screen the user is looking at, which is why they can't hand back a walkthrough. About a third of our responses are exactly that walkthrough. That gap is why the category has moved toward[agentic, in-product](https://frigade.com/blog/what-a-product-using-agent-actually-does) assistants over the past year.


## Finding 3: when the assistant gives a walkthrough, people follow it


Share of walkthrough sessions where the user advanced at least one step in the same session.


A walkthrough only matters if people follow it, and they do. When the assistant answers with a walkthrough, the user steps through it about 70% of the time, advancing at least one step in the live product in that same session.


That's a rate a list of links never reaches. Plain instructions get skimmed and abandoned, because every step is one more thing to map onto your own screen. A walkthrough that runs in the product flips that. Each step is highlighted where you already are, so following along takes less work than ignoring it.


We're not claiming people finish every walkthrough. Plenty get what they came for partway through and close, and that counts as a win. The point is that seven in ten engage with the steps at all. You don't get that with a text answer.


## What this means if you are evaluating in-product AI


The second-question rate will tell you more than any demo. A demo is built to look good; whether a user comes back the next day is the part you can't stage. The users worth designing for are the ones who keep going, the people asking five or ten questions.


The other thing to test is whether the assistant can do anything in the product itself. One that only returns text and links hands the user instructions and leaves them to find the buttons. The questions people are asking are how do I do this, on the screen I'm on right now. A walkthrough shows them; text just points.


We'll run this again as the data grows, because it moves fast and the guided-walkthrough share keeps climbing every time we look. The assistants that can only talk are going to feel the way the help center already does: something you click out to when the product could have just shown you. We'd rather put the rough read out now than sit on it another quarter, so here it is.


## FAQ


**Do users actually come back to in-product AI assistants?**


Yes. Once a user comes back to ask a second question, the majority go on to ask many more. Roughly 62% of returning users ask three or more, and about 1 in 10 ask eleven or more.


**How is an in-app AI assistant different from a chatbot that points at a knowledge base?**


A knowledge-base chatbot answers with text and links. An in-product AI assistant lives inside your product and walks the user through the workflow in the live UI. About a third of in-product AI responses are guided walkthroughs. Most of those get followed. That's the structural difference, and it's why Intercom's Fin and similar chat-layer agents can't close the gap from where they sit.


**Is in-product AI replacing the help center?**


In the workflows where it's deployed, yes. Help-center traffic drops because users get the answer in-product instead of clicking out to a docs page. More on that in[The help center was a stopgap](https://frigade.com/blog/the-help-center-was-a-stopgap) .


**What predicts whether an in-product AI assistant is working?**


The second-question rate, and how deep users go after it. Both are much harder to fake than a strong demo, which is why we lead with them instead of total volume.


**Can the assistant do things for users, not just explain them?**


Increasingly, yes. Most of what the assistant does today is guide: it walks the user through a workflow in the live UI. A growing share of responses are actions it takes on the user's behalf through[tool calls and skills](https://frigade.com/features/skills) , rather than instructions to follow. Users don't always want to be walked through it. They want it done for them, and the assistant does that too.


## Methodology


We looked at aggregated, anonymized usage data from a year of Frigade Assistant production traffic. The data doesn't include customer names, end-user identifiers, or query content, so nothing here identifies anyone.


If there's a cut of the data you want to see, emailchristian@frigade.com .
