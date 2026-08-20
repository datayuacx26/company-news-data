---
schema_version: "1.0.0"
document_id: "b1157ab13bf3dae5096cc3a360016a781d63f508dcc9e07c97f8097005da295e"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/policy-engines-for-ai-agents"
published_at: "2026-02-17T12:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:54:21.606053+00:00"
content_hash: "sha256:757c834f323816590f83ced3d3409dad32e4602eef42e5f312a6e9977d4fb963"
---

# Policy Engines for AI Agents

## TLDR


There are people out there putting forward the idea that policy engines are a good way to do authorization for AI agents. In this video, Jake breaks down why that thinking falls short and how relationship-based access control (ReBAC) is better suited for the agentic future.


- **AI agents should be treated like people, not scripts.** They'll get dynamic, evolving access — shared documents, group memberships, team roles — not pre-canned static policies. If we could decide everything ahead of time, they'd just be workflows, not agents.
- **Policy engines look fast but hide costs.** The impressive microsecond evaluation benchmarks ignore that you still have to collect and assemble all the data before feeding it to the engine. The policy itself is stateless and unaware.
- **Cedar example walkthrough.** The official Cedar playground example needs ~200 lines (131 schema + 57 data + 12 policy) to express a simple photo-sharing model. The equivalent in SpiceDB is ~20 lines of schema + 4 lines of relationships — because data and policy are unified into a single permission system.
- **ReBAC naturally fits agent authorization.** Agents become first-class objects with relationships, just like users. No need to wedge them into a "user" concept like the old service account anti-pattern.
- **Policy engines still have their place** — simple, data-already-present decisions like IP allowlists or URL routing. But for the dynamic, relationship-heavy world of AI agents, they're a square peg in a round hole.


Visit[authzed.com/ai-authorization](https://authzed.com/ai-authorization) for more on how AuthZed supports AI agent authorization.


## Full Transcript


**Jake:** There are people out there who are putting forward this idea that policy engines are a good way to do authorization for AI agents, and I just feel like I need to get out there and sort of clear up what policy agents are and what the requirements for using one are and how they sort of fall down in some of these more interesting or more complicated use cases that we're gonna be deploying AI agents into.


One of the reasons that I think that this won't work in the future is that right now we're sort of treating AI agents a little bit like they're computers, and at their ideal, or at their most effective form, we're gonna be treating AI agents a lot more like we treat humans.


**Sohan:** And when you say talking about treating AI agents as humans, what do you really mean? Like what does that entail?


**Jake:** We're not gonna like feed them and read them bedtime stories and things like that, right. When I say treat them like humans, I mean instead of sort of pre canning or pre-deciding what all of their access is, and writing sort of a script or, dare I say, policy for how and what they can access — I think that the access control will be a lot more dynamic like it is with people. Like we will share documents, we'll share data. We'll add them to groups, we'll add them to teams. And for all the same reasons that humans get added and shared to those things, the AI agents, we'll be treating them like a human in that way.


**Sohan:** And honestly, I just see this as a start and it's going to evolve to something far more complex in the next like three to four years. It's not going to be something as simple as a very simple workflow that involves an LLM, for instance. So I feel things like access control and thinking about these things for agents will be even more complex.


**Jake:** Absolutely. So before we really get into the examples and the nitty gritty details, I wanna just talk sort of philosophically about what a policy engine is and how that contrasts with what we're doing with AuthZed and SpiceDB, which is based on this concept of ReBAC or relationship based access control.


A policy engine is just a computer program. And as you know, computer programs can be arbitrarily complicated. We're gonna run it over some piece of data or some bits of data, and it's going to create an answer. And it's going to either say yes or no based on the data and the program that it's been given.


The cool parts about policy engines is that they're sort of infinitely flexible. They don't really have any preconceived notions about who should have access to what or why. And they're also extremely high performance because you have to go and collect all of the data and feed it to this policy engine, but the part — so you get these numbers that are being kicked around like, oh, this policy engine evaluates in 50 microseconds or 600 nanoseconds.


**Sohan:** Right. I've seen those.


**Jake:** Crazy, astounding numbers. And you're like, wow, that's amazing. I need some of that. But what they gloss over is the fact that you have to go and get all of the data and feed it to the policy engine.


And so, since Cedar is sort of the talk of the town, I thought I'd just walk through a Cedar example that I just pulled. I went to the Cedar playground, cedar policy.com. I didn't try to handpick anything — this is straight outta the website. This is straight off of their website, and it's the very first example that they show.


So I was looking through this thing and it says, all right, it's gonna be for a photo sharing app — a simple access example. So let's see what we have here. We have the three pieces of data. And by the way, these are the same sort of three pieces of data that we use in SpiceDB. It's like, who are we talking about? What are they trying to do and what are they trying to do it to? And then I looked through this before the recording, but then they go into all this data and the data defines that there is a user named Alice. And Alice belongs to a group called Alice Friends and AV team.


And then there's a photo called vacation photo. And the vacation photo belongs to an account called Ahmad. Then they define two groups called Alice Friends and AV team. And so that's 57 lines of data. And so then I'm going into here and I'm like, all right, let's see the schema.


So first they have these policies listed in the playground and the policies here say that Alice is allowed to view the photo vacation photo. And all of this data is hard coded into the policy. And that doesn't make a lot of sense to me, but let's stick with it. Then we have that Stacy is allowed to view a photo when the photo belongs to an account named Stacy.


Okay. And I'll point out that as far as I can tell this Stacy doesn't have anything to do with this Stacy.


**Sohan:** Right, because there could be two people named Stacy also.


**Jake:** Could be two people named Stacy. Maybe accounts are named after pets and someone's got a pet named Stacy. I don't know. But these are just two strings.


And if these strings — this could have said something else and it could belong to Ahmed's account for all it matters. So what this is not expressing is this is not expressing that Stacey can view Stacey's own photos. Which is usually something that we would wanna do for like a photo app.


And then we go down to the schema and then the schema talks about all of the different entity types that can exist. We've got a common type called a person, a common type called a context. We've got users. Users can belong to user groups. We've got user groups, we've got photos. And photos can belong to albums or accounts.


And then we've got albums and accounts, and then we've got actions. So what are we allowed to try to do to a photo? We're allowed to try to view it, create it, or list it. This list photos — as an authorization person, this seems like it's in the wrong spot to me. So they're saying that you're going to try to list photos on a resource type of photo.


Usually you try to list things on collections. So like a photo album or on an account. But yeah. So then we've got 131 lines of schema to define what are all the different object types and how do they all sort of relate to one another.


**Sohan:** Wait, how many lines again, Jake? That was...


**Jake:** 131 lines of schema.


**Sohan:** Holy. Okay.


**Jake:** 12 lines of policy. And then another 57 lines of data.


**Sohan:** Oh, okay.


**Jake:** So this is all just to put together this picture of is Alice allowed to view this photo, vacation photo? And they build up all of this information about Alice belongs to this group and that group, and these groups exist. And then as far as I can tell, they just ignore all of it and directly give Alice access to this photo with an inline policy.


**Sohan:** Yeah.


**Jake:** Which is fine. But in this particular model, every time Alice got access to a photo, you would have to write a block like this. In ReBAC, we look at the underlying assumption that most access controlled decisions are actually based on relationships.


**Sohan:** Mm-hmm.


**Jake:** And that can be relationships between people and people, data and data, people and other data. It's sort of arbitrary, but we are defining sort of a web of relationships. In this case, we kind of got there a little bit with this policy engine where we started saying things like Alice belongs to these groups and like maybe later on we could say these photos have been shared with these groups, or something along those lines. And then we're gonna make our access control decisions based on, like, Alice has access to the groups that these photos are in. Or Alice can see her own photos, things like that.


So this just tends to be the more natural way to express these kinds of access control decisions based on the relationships of how data is related to other data. What we do with our approach is we actually combine all of the data and the policy together into a single entity that we call a permission system.


**Sohan:** Yep.


**Jake:** And this is super cool and super useful because anyone can go and ask these questions whether they have the ability to bring the data to the decision or not because the data is all baked into the authorization permission system ahead of time.


**Sohan:** Yeah.


**Jake:** And so anyone can ask the same question and get back the same answer.


And we can also optimize performance around the data that we already have. So we can say things like, oh, Alice is a really popular person. People are trying to look at her photos a lot. And so we can start to do things like cache those access control decisions or pre-compute de-normalized, those kinds of things. So we can really make decisions a lot faster than going and assembling the data and feeding it to a completely naive, completely unaware computer program every single time we wanna make a decision.


So just to make this concrete, I modeled that data, that same Cedar policy.


**Sohan:** This is the same photo sharing app, correct?


**Jake:** This is, well this is sort of like the same lack of a photo sharing app — permissions model modeled in SpiceDB. So in here we've got all of the entities and what they can be shared with, and everything defined in about 20 lines of SpiceDB schema.


**Sohan:** Mm-hmm.


**Jake:** You'll notice here that permission view, create and list are all set to nil. And that was because the policy in the Cedar example didn't actually define who was allowed to view, create or list. Just kind of put those things out there and said that people should be able to view, create or list. And then we created, I created these relationships to capture that data in sort of the 57 lines of data.


I cheated a little bit because I actually looked at their other example as well and found that Stacy had a photo that was part of an account named Stacy.


**Sohan:** Okay.


**Jake:** So I baked that information in there as well. But yeah, so all of this — these four lines of relationships and 20 lines of policy or schema as we call it, have defined the entire sort of what they had in their example.


**Sohan:** Yeah. And those 20 lines include three lines of comments. So just say — yeah. So we sort of have like policy engines which do things in a certain way. And we of course have ReBAC and you've sort of elucidated the difference between the two.


We are also seeing this whole wave of like AI and RAG and agents and MCP, and there's so much talk about agents. So how does all of this tie into authorization for AI agents specifically?


**Jake:** Yeah, I think at the beginning we talked about how we're going to treat AI agents more like people.


**Sohan:** Yeah.


**Jake:** But I think, let's actually just make it concrete. Let's go and add.


So it was that simple to add the concept of agents to our permissions model. And then we could start doing things like we could add shared with, and that could be either a user or an agent. So we could directly share a photo with either a user or an agent, and we could start using that information in our access control decisions.


So there's nothing special about users in AuthZed and in SpiceDB. And even if you distill it to relationship based access control, there's really nothing special about users or agents. They're all just objects and they have relationships between them. So in this new future where we're going to be sharing data directly with agents, we definitely want to be able to flexibly add that to our permissions models, and we wanna be able to start talking about agents as first class citizens in the permissions model.


One of the failure modes that I've seen in the past is trying to wedge — in the past it was always about service accounts. But one of the failure modes was trying to wedge the concept of a service account into a user, which is like a natural person. So people came up with this concept that it would be really great if every entity that was interacting in our system were a quote unquote user. But when you really distill it down, service accounts aren't like users.


We're gonna have a lot more agents than we had people in the future. And so we really wanna handle these things differently and we wanna think about them differently when it comes to our permissions model specifically.


So I hope that helps to sort of highlight the differences about the way we think about these things, because at the end of the day, if it was as simple as like knowing what they were gonna do ahead of time, they would just be workflows. They wouldn't actually be agents.


But yeah, there's definitely use cases though where policy engines do make sense. That's really similar to in computing — when someone like an HTTP request is showing up and you're just verifying, did it come from a known IP address or is it for a particular URL? Things like that where all of the data is already there ahead of time. In those cases, you definitely want to leverage the speed of a policy engine because the decision is being made with data that you already have.


But I hope I made a good case for why these things aren't ideal and especially not as we start to create more and more agents and have a lot more context that's required to make decisions about what these agents are allowed to do and why you may wanna reconsider trying to jam that square peg through the round hole of using a policy engine for everything.


**Sohan:** Well, you convinced me, Jake. Thank you for these thoughts. Folks do check out[authzed.com/ai-authorization](https://authzed.com/ai-authorization) for more details. Don't forget to subscribe for more content on the world of authorization and see you in the next one.


On this page


- TLDR
- Full Transcript


## Related


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Engineering Build a production-grade Agentic RAG system using AuthZed Cloud Learn how to build a production-grade Agentic RAG system on AuthZed Cloud, where authorization is hardcoded into the LangGraph pipeline—not a prompt instruction the agent can skip. Apr 15, 2026 · 7 min](https://authzed.com/blog/build-production-grade-agentic-rag-authzed-cloud)[Engineering Build a production-grade Agentic RAG system using AuthZed Cloud Learn how to build a production-grade Agentic RAG system on AuthZed Cloud, where authorization is hardcoded into the LangGraph pipeline—not a prompt instruction the agent can skip. Sohan Maheshwar · Apr 15, 2026 · 7 min](https://authzed.com/blog/build-production-grade-agentic-rag-authzed-cloud)


[Engineering Introducing SpiceBox and spicedb-dev AI coding agents need better permissions, and so does the code they write. SpiceBox enforces fine-grained permissions on AI coding agents using SpiceDB, while spicedb-dev gives agents the authorization context they need to generate code with proper access control from the start. Both are open source. Apr 8, 2026 · 7 min](https://authzed.com/blog/spicedb-dev-and-spicebox-add-permissions-for-ai-coding-agents)[Engineering Introducing SpiceBox and spicedb-dev AI coding agents need better permissions, and so does the code they write. SpiceBox enforces fine-grained permissions on AI coding agents using SpiceDB, while spicedb-dev gives agents the authorization context they need to generate code with proper access control from the start. Both are open source. Joey Schorr · Apr 8, 2026 · 7 min](https://authzed.com/blog/spicedb-dev-and-spicebox-add-permissions-for-ai-coding-agents)
