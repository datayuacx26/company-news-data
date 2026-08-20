---
schema_version: "1.0.0"
document_id: "14c0c85e64f681cc1012e9a48a8b1f5862a3000a25f59402a520d0b598cec8a0"
company_key: "yc-howdy-com"
company: "Howdy.com"
source_id: "yc-howdy-com-news-import-7de6c48bfb23"
canonical_url: "https://www.howdylatam.com/blog/react-ba-meetup-recap-buenos-aires"
published_at: "2026-07-30T19:33:13.230+00:00"
first_seen_at: "2026-07-31T23:44:21.659856+00:00"
fetched_at: "2026-07-31T23:44:22.904459+00:00"
content_hash: "sha256:9e686bc5a396e18901cd2de07fc65d9a31eab894839ef8015d71770aeec62d83"
---

# React BA Meetup: Buenos Aires Talks Reactivity and Real Engineering

On June 4th, we met up with the React community in Buenos Aires. We sponsored the React BA Meetup at La Maquinita, a space built for exactly this kind of gathering, and the room filled up fast. Thirty people stayed until the end, on a night that started with empanadas and cold beer and ended with two talks that gave everyone plenty to think about.


## **A Backend That Reacts as Fast as Your Frontend**


Raúl Ahumada, from OneInfo Consulting, opened the night with a question anyone who has built a real time app has asked before: why can't the backend be as reactive as the frontend? His talk centered on Convex, a database built specifically to solve that problem.


The core idea is easy to explain and hard to pull off with traditional tools: when two people work on the same piece of data at the same time, the update needs to show up instantly, without the delay of a manual request that breaks the experience. Raúl showed it live with a chat app built in React, connected to Convex, updating without a single call to a traditional API. When two users typed at the same time, the change appeared instantly on both screens.


As with any live demo, there was a moment of technical chaos: screens that would not share, a mic that kept cutting out, some last minute improvising. Nothing a senior developer has not lived through before in a stand up or a client demo. The room took it in stride, and Raúl kept going without losing his train of thought.


## **Planning Before You Code, Even When AI Does the Heavy Lifting**


The second talk came from Darío Macchi, Dev Advocate at Howdy, and it tackled a problem every team using AI to generate code knows well: when you give a model too little context, the model decides for you. And those decisions, made silently, show up later as a pull request full of unexpected feedback.


Darío walked through how his team started fixing this by writing short context documents before assigning a task, whether to a new hire or to an AI agent. The point is not to document the entire project, but to write down the specifics: what to touch, what to leave alone, how data flows, where decisions have already been made. That habit, which started as a different way to run onboarding, ended up producing something the team never had before: real project documentation, written because it was needed, not because someone asked for it.


The talk closed with an idea the room kept turning over: separating thinking work from execution work more clearly, so humans plan during the day and AI can move forward on its own at night, inside well defined limits. It is still a work in progress, but the room clearly got where it was headed.


## **What the Community Took Home**


90% of the people in the room that night had never heard of Howdy before the event. For us, that is exactly the point of showing up to gatherings like this one. It is not about selling anything, it is about being where the community already is, listening to what matters to them, and adding something real to the conversation.


The React BA organizer wrote to us the next day to thank us for co-organizing the event. He talked about an engaged audience, strong attendance, and interest in doing more together, whether through React Buenos Aires or any other idea that comes up.


We walked away with the same feeling. A technical talk that sparks real questions and a crowd that sticks around to keep talking after the event already say a lot about how the night went. We are already thinking about the next one.
