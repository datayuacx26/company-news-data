---
schema_version: "1.0.0"
document_id: "a372e9d1875da7c956f4b5ab432a4096b3737cc7e2f8c1cc1efb4f62ae353c0d"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/turos-spicedb-success-story-how-the-leading-car-sharing-platform-transformed-authorization"
published_at: "2025-09-15T13:49:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:56:00.447075+00:00"
content_hash: "sha256:b5df9a7bc75309114b9420b85fa61d71eb1869636e2a94a86a03f0663e4c22a1"
---

# Turo's SpiceDB Success Story: How the Leading Car-Sharing Platform Transformed Authorization

*This talk was part of the Authorization Infrastructure event hosted by AuthZed on August 20, 2025.*


## Hosting Teams: A Case for SpiceDB


Andre, a software engineer at[Turo](https://turo.com/) , shared how the world's leading car-sharing platform solved critical security and scalability challenges by implementing SpiceDB with managed hosting from[AuthZed Dedicated](https://authzed.com/products/authzed-dedicated) . Faced with fleet owners having to share passwords due to rigid ownership-based permissions, Turo built a relationship-based authorization system enabling fine-grained, team-based access control. The results speak for themselves: **"SpiceDB made it trivial to design and implement the solution compared to traditional relational databases"** while delivering **"much higher performance and throughput."** The system proved remarkably adaptable—adding support for inactive team members required **"literally one single line of code"** to change in the schema. AuthZed's managed hosting proved equally impressive, with only one incident in over two years of production use. As Andre noted, **"ultimately hosting with AuthZed saved us money in the long run"** by eliminating the need for dedicated infrastructure engineering, allowing Turo to focus on their core business while maintaining a **"blistering fast"** authorization system.


**On Reliability and Expert Support:**


> "In over two years \[...\] of operations in production, we had a single incident. And even then in that event, they demonstrated the capacity to recover from faults very, very quickly."


**On Business Focus:**


> "For over two years, Turo has used AuthZed's \[Dedicated\] offering where they're responsible for deploying and maintaining all the infrastructure required by the SpiceDB clusters. And that gives us time back to focus on growing our business, which is our primary concern."


---


## Full Transcript


*Talk by Andre Sanches, Software Engineer at Turo*


Hello, everyone, and welcome. I'm Andre, a software engineer at Turo, working with SpiceDB for just over two years now. I'm here to share a bit of our experience with SpiceDB as a product and AuthZed as a hosting partner. Congratulations, by the way, to AuthZed for its five-year anniversary. It's a privilege to be celebrating this milestone together. So let's get started.


### Introduction to Turo


First, a quick introduction to those who don't know Turo. We're the leading car-sharing platform in the world, operating in most of the US and four other countries. Our mission is to put the world's 1.5 billion cars to better use. Our business model is similar to popular home-sharing platforms you may be familiar with, with a fundamental difference. Vehicles are less expensive compared to homes, so it's common that hosts build up fleets of vehicles on Turo. In fact, many of our hosts build successful businesses with our help, and therein lies a challenge that we solved with SpiceDB.


### The Challenge


Hosts have responsibilities, such as communicating with guests in a timely manner, taking pictures of vehicles prior to handoff, and again, upon return of the vehicle to resolve disputes that may happen, managing vehicle schedules, etc. These things take time and effort, and as you scale up your business, fleet owners often hire people to help. And the problem is, in the past, Turo had a flat, ownership-based permission model. You could only interact with the vehicles you own, so hosts had no other choice but to share their accounts and their passwords. It's safe to say that folks in the target audience of this event understand how big of a problem that can be.


Moreover, third-party companies started sprouting all over the place to bridge that gap, to manage teams by way of calling our backend, which adds yet another potential attack vector by accessing Turo's customer data. So, it had become a large enough risk and a feature gap that we set out to solve that problem.


### The Solution


The solution was to augment the flat, ownership-based model with a team-based approach, where admin hosts, meaning the fleet owner, can create teams that authorize individual drivers to perform specific actions, really fine-grained, on one or more of the vehicles that they own. Members are invited to join teams via email, which gives them the opportunity to sign up for a Turo account if they don't yet have one.


So, the solution from a technical standpoint is a graph-based solution that enables our backend to determine very quickly, can Driver ABC perform a certain action on vehicle XYZ? In this case right here, can Driver ABC communicate with guests that booked that certain vehicle? SpiceDB made it trivial to design and implement the solution compared to traditional relational databases, which is most of our backend. Moreover, it offloaded our monolithic database with a tool that offers much higher performance and throughput.


### Implementation Details


Anecdotally, the simplicity of SpiceDB helped implement a last-minute requirement that crept in late in the development cycle—support for inactive team members, the ones who are pending invitation acceptance. Prior to that, the invitation system was purely controlled in MySQL. And we realized, you know what, if we're storing the team in SpiceDB, why not make it so that we can store inactive users too? And the reason I'm mentioning this is this impressed everybody who was working on that feature at the time, because it was literally one single line of code that we had to change in the schema to enable this.


So I'll talk more about this in a second where I show some technical things. But the graph that I just mentioned then roughly translates to this schema. So this is a simplified but still accurate rendition of what our SpiceDB schema looks like. Hopefully this clarifies how driver membership propagates to permissions on vehicles, if you're familiar with SpiceDB schemas.


Some noteworthy mentions here are self-referencing relations, this one up here, or all the way up there. So basically, this is how we implemented the inactive users. If you notice that there, there's the member role and then an active member role. And by way of adding a single record that connects the member role with an active member role in the hosting team, you can enable and disable drivers. So this was so incredibly impressive at the time, because we thought we're going to have to change the entire schema and a whole bunch of other changes. And no, that's all it took.


And again, it's one of those things that once it clicked, if you're familiar with the SpiceDB but not with the self-referencing relation, looking at this, that #member role and pointing to a relation in the same definition, it kind of looks a little daunting. It did to me. I don't know—you're probably smarter than I am, but it was daunting. But then one day it just clicked and I'm like, hmm, okay, that's how it is. And I was super stoked to continue working with SpiceDB and I'm going to implement more and more of the features. And help the feature team, actually, because it was a separate feature team that was working on this. So that self-referencing was interesting.


### Namespacing Feature


The other noteworthy mention here is the same namespaces. If you notice in front of the definition, there's a hosting teams forward slash. This is how we separate the schema into multiple copies of the same schema in the same cluster. So we have an ephemeral test environment in which we create and destroy on command sandbox replicas of our entire backend system. This enables us to deploy dozens, if not hundreds, of isolated copies of the schema, along with everything else in our backend, to test new features in a controlled environment that we can break, that we can modify as we see fit without affecting customers. And the namespacing feature in SpiceDB allowed us to use the same cluster for all those copies and save us some money. So we don't have to stand up a new server. We, you know, there's no computational costs or delays or any of that in provisioning computing resources and this and that.


So the feature was released the week of, you know, us going pre-live, in a test environment. And we were probably the first adopters of this and it was really cool.


### Performance


So let me see at a high level, this is how our hosting team feature works. You can see, let me use the mouse here. You can see how permissions propagate to teams. So, team pricing and availability goes to the relation of the team in the hosting team. Hosting team has the pricing and availability for active member roles or admin role. Plus sign, as you all know, is a or, and then it connects to the driver. Simple, fast. This is blistering fast.


One other query that we make to SpiceDB very, very often—matter of fact, this is the single most, you know, issued query to SpiceDB at any given time—is, is the currently logged in user a cohost. And that's done for everybody. Even if you're not a cohost, this is how we determine whether you're a cohost or not. That will then drive UI, you know, decisions, what, what widgets to show. You know, only if you're, if it's pertinent to you, if you're a cohost, if not, then there's no, no reason to. To pollute the UI with, you know, cohosting features. Yeah.


### User Interface


And this is what the UI looks like. So, you, on a team, you have cohosts and you can add or invite, here's an interesting thing. The code name of the project was cohosting. It ended up being hosting teams because we then used the nomenclature cohosts to add people to teams. So, here you have your cohosts. You can invite them by email. They get an, an email that points them to sign up to Turo. If they already have an account, they can just log in. And the moment they log in, it automatically accepts the invitation.


Next you have the fine grain permissions of what your group can, or your team can do. In this case, we have trip management enabled. This is the base actually, you know, the base permission that you have to grant to everybody on the team. And then there's pricing and availability that allows you to set prices for vehicles, discounts, you know, see finances and all that stuff. So you can imagine why that's, you know, why it's very nice to be able to toggle this and not let, you know, just any cohost that has no business looking at your finances, you know, just hiding it from them by way of untoggling the permission here. And then you have your vehicles. The list shows all the vehicles you own. You just toggle the ones you want, save, and you're off to the races. Your hosting team is in place and working.


### AuthZed Hosting Partnership


So also that as a hosting partner, when you're considering using, you know, a big challenge of adopting a new system is setting it up and running it in a scalable and reliable way. You have to manage, you know, security issues. You have to manage our scaling. You have to manage all kinds of, you know, infrastructure challenges. And that costs money. In this day and age, it's really hard to find engineers who understand infrastructure well enough to manage all the moving parts of a highly scalable system such as SpiceDB.


For over two years, Turo has used AuthZed's fully hosted cloud offering where they're responsible for deploying and maintaining all the infrastructure required by the SpiceDB clusters. And that gives us time back to focus on growing our business, which is our primary concern. So this is a great opportunity actually to give AuthZed a shout out for their excellent reliability.


In over two years, over two years and three months now, actually of operations in production, we had a single incident. And even then in that event, they demonstrated the capacity to recover from faults very, very quickly to pinpoint the problem incredibly quickly. And, you know, take care of it. I think the outage was, we were out for like 38 minutes, something like that. It was, you know, we've had other partners that things were much, much more challenging. So, and once in two years, the root cause, the entire handling of the outage was very, very, you know, nice to see. Because it involved thorough analysis, post-mortems and making sure that it doesn't happen again, putting in safeguards to ensure that it doesn't happen again.


So everything was, you know, systems fail. We understand that. And how we deal with it is how, is what shows how, you know, how good you are. And with AuthZed, we rest, you know, easy knowing that we're well taken care of. And ultimately hosting with AuthZed saved us money in the long run because it would otherwise take a lot of engineering time and effort just to keep the clusters running. So if your company is considering adopting SpiceDB, I would highly encourage you to have a chat with AuthZed about hosting as well. From our experience, it's well worth the investment.


On this page


- Hosting Teams: A Case for SpiceDB
- Full Transcript
- Introduction to Turo
- The Challenge
- The Solution
- Implementation Details
- Namespacing Feature
- Performance
- User Interface
- AuthZed Hosting Partnership


## Related


[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Cormac Foster · Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)


[Company Identiverse 2026: The Conference about Identity's Authorization Problem AuthZed is sponsoring Identiverse 2026, where three of the four featured summits point to the same underlying issue: the assumptions that underpinned IAM for two decades have collapsed, exposing authorization as the missing layer. Jun 10, 2026 · 3 min](https://authzed.com/blog/identiverse-2026-identity-authorization-problem)[Company Identiverse 2026: The Conference about Identity's Authorization Problem AuthZed is sponsoring Identiverse 2026, where three of the four featured summits point to the same underlying issue: the assumptions that underpinned IAM for two decades have collapsed, exposing authorization as the missing layer. Cormac Foster · Jun 10, 2026 · 3 min](https://authzed.com/blog/identiverse-2026-identity-authorization-problem)


[Industry Calculate the Authorization Tax: Quantifying the Costs of Home-Grown AuthZ Everyone is paying for authorization. Here are five inputs to help you quantify your Authorization Tax—and two paths to reduce it. May 18, 2026 · 6 min](https://authzed.com/blog/calculate-the-authorization-tax)[Industry Calculate the Authorization Tax: Quantifying the Costs of Home-Grown AuthZ Everyone is paying for authorization. Here are five inputs to help you quantify your Authorization Tax—and two paths to reduce it. Cormac Foster · May 18, 2026 · 6 min](https://authzed.com/blog/calculate-the-authorization-tax)
