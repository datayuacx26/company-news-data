---
schema_version: "1.0.0"
document_id: "546b1475c19990dfde66bdc4c69c52aaf83d31ad36276f613c120deed6ae4bdc"
company_key: "yc-emergent"
company: "Emergent"
source_id: "yc-emergent-news-import-fc8f7e880d65"
canonical_url: "https://emergent.sh/blog/emergent-emmy-launch"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-08-01T00:03:07.102401+00:00"
fetched_at: "2026-08-01T00:03:09.053805+00:00"
content_hash: "sha256:64d416e3256782b527d576c5af3bf5cc1b868e2020ac93d78810cc7ef8120ec6"
---

# Meet Emmy: Your Always-On Emergent Assistant

The hardest part of building your first app is not the building. It is not knowing what you do not know. Which agent to pick. Whether that error in the log actually matters. What that button is about to cost you. What happens after you hit publish.


Emmy answers all of that, and she costs nothing to use.


Emmy is built into Emergent, and she knows what you are working on. You never have to explain your project before asking a question. Screenshot whatever is confusing you, ask what it is, and she answers about your build.


## What we built


Emmy is a chat assistant built into Emergent and available from every page in the product. The chat icon sits in the bottom right corner of your screen. She is free to use and talking to her costs no credits, so there is no reason to ration your questions.


Her value comes from the fact that she is not a separate destination. You do not leave your build to go ask a question somewhere else, wait for a reply, and come back with the context lost. The conversation happens where the work is happening.


There are limits, and they are worth knowing. Emmy does not read your code or look through your account history. What she has is enough context about your current project to help without you explaining it from scratch.


## How to use Emmy when you are starting out


The blank prompt box is where most first builds stall. The idea is usually fine. What stops people is not knowing how much detail the agent needs, and worrying that a bad first attempt burns credits they cannot get back.


### 1. Describe the idea in plain language


Open the chat from any page and start typing. You do not need to write a specification. A line like "I want something to keep track of my client projects" is enough to work with, and if even that feels like too much, there are suggested starting points you can tap instead.


Emmy asks follow-up questions from there. What the app needs to do, who is using it, what has to exist on day one versus what can wait. These are the questions that, left unanswered, tend to surface halfway through a build and stall it.


### 2. Take the prompt she writes


Emmy turns that conversation into a complete build-ready prompt. Use it as written, edit it, or ask her for a different version if the framing is off.


### 3. Ask about any option you do not recognize


Emergent gives you choices you will not have opinions about yet, from which agent runs your build to whether you need GitHub connected. Ask Emmy about any of it and she answers in context, so you are not reading documentation in a second tab while the idea goes cold.


Agent selection is the clearest example. Ask which one should build your app and she lays out the tradeoff for each. E-1 is predictable but expects you to direct it step by step. E-2 is built for reliability and complex integrations.[E-3](https://emergent.sh/blog/introducing-e-3-autonomous-app-building-on-emergent) maps out the whole app first, creates a build plan, then builds it in one pass. Then she points to the one that suits what you have just described to her.


## How to use Emmy during the build


Momentum is fragile. Something stops you mid-build, and it is not always a broken deploy. Sometimes it is just a question you are unable to answer. Either way, the usual path is a ticket and a wait, and by the time someone replies you have lost the thread. Emmy closes that gap.


### 1. Screenshot whatever is confusing you and ask


Emmy takes image attachments. If something on screen does not match what you expected, grab it, drop it into the chat, and ask what you are looking at.


A real example from a build: the project was set to run on E-3, but the progress panel kept showing an E-1 agent doing the work. Fair thing to be alarmed about. Emmy answered it in one reply. E-3 is still running the plan and the testing, and it uses E-1 underneath to carry out specific file edits, which is what surfaces in the panel. Emmy resolves the doubt is seconds without the user having to pause the build.


### 2. Ask what your credits will actually cover


Ask what is left in your balance and Emmy gives you the number. Then she tells you whether it is enough to finish what you are currently building, or whether you will run short partway through.


### 3. Ask how to get the app live


Deployment is where first-time builders stall most often, because the steps are unfamiliar and the cost of getting them wrong feels high. Ask Emmy and she gives you the actual sequence in order, from opening Publish through running a health check to starting the deployment.


She also tells you what to expect on the other side of that button: roughly how long the first deploy takes, what it costs on your plan, and that you finish with a live production URL. The things people typically discover the hard way get flagged upfront, like preview secrets not carrying across to production on their own.


Some things still need a person, and Emmy will say so rather than looping. When she escalates, your context travels with the handoff, so you are not restarting the explanation with someone new. She also notices the points where builds tend to stall and offers a next step, which you can take or ignore.


[Follow the full build](https://emergent.sh/tutorials/how-to-build-an-app-with-emmy) step by step, from a single sentence to a live client project tracker.


## Availability


Emmy is live now, free to use, and costs nothing in credits. Open any page of Emergent in your browser and the chat icon is in the bottom right corner.


She is web only for now. Emmy is not available on mobile web or in the mobile app yet.


If you are new to the platform, she is a reasonable first stop before you write anything into the main prompt box. If you have been building for months, she is most useful the next time something goes wrong.


## Start where you are


You are never building alone on Emergent. No builder should ever be clueless here, and Emmy is how we make sure of it.


If you have an idea you have not started because you were not sure how to describe it, open the chat and describe it badly. That is what she is for. If you have a build sitting half-finished because something broke and you did not want to open a ticket, bring her the problem.


[Start Building](https://emergent.sh/) on Emergent and say hello to Emmy.
