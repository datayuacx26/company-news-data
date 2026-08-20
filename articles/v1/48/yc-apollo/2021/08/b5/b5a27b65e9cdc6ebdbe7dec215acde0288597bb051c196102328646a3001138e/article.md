---
schema_version: "1.0.0"
document_id: "b5a27b65e9cdc6ebdbe7dec215acde0288597bb051c196102328646a3001138e"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/10-best-practices-for-schema-stewardship"
published_at: "2021-08-19T09:04:49+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:b1fd912d7fa6d6cfbef6147fe9d2a5941c1658f4c0d598d2d004af6b686f5f8a"
---

# 10 Best Practices for Schema Stewardship (Part 1 of 2)

Schemas are at the center of GraphQL’s ability to capture a *shared* and a *unified* representation of our data, services, and digital capabilities. Yet, how do we manage a process for agreeing on this representation? How do we ensure it balances the needs of all the parties involved, and leads to the outcomes we seek? And how do we avoid creating a process bottleneck that slows everyone down?


At Apollo I have the opportunity to meet graph champions from many leading companies and hear their answers to these questions. In a recent[recorded tech talk](https://www.apollographql.com/events/tech-talk/thank-you/establishing-your-schema-governance-best-practices?referrer=blog) I tried to pass on what they shared and what I’ve learned. In part 1 of this blog post I’ll summarize the **first five best practices** presented in that talk, and in[part two I’ll cover the second five](https://www.apollographql.com/blog/community/graphql-champions/10-best-practices-for-schema-stewardship-part-2-of-2/) .


In the table below you can see the full list. Each offers a practice your teams can adopt to improve the way you work with others to define, evolve and improve your GraphQL schemas and the graph they represent. Let’s cover the first five beginning with a decision about terminology.


## Best Practice **#1** – Adopt a stewardship mindset


#1 Stewardship is a more effective mindset than rules-based, authority-based governance. It reminds us we are working collectively to improve and tend, rather than dictating what will be.


In Justin Abrahms’ recent post[Governance versus Stewardship](https://justin.abrah.ms/2021-08-09-governance-versus-stewardship.html) , he captures the key conceptual difference between governance and stewardship.


- **Governance** is a process where you *ask other people for permission* to do things and the governing body serves to enforce consistency in things.


- **Stewardship** is much *more collaborative* process and involves *tending* **** to what exists, *guiding* rather than dictating what will be.


Given our need to create a consensus schema across many teams, I hope you’ll agree with Justin and I that the term stewardship is much closer to what we’re after than governance.


## Best Practice #2 – Begin by understanding all the teams, their motivations, and their struggles


#2 Understanding leads to empathy, which leads to bridge building and trust. Trust is what you are after when asking for change.


I think it’s fair to characterize the effort to create a coherent unified graph schema with a multitude of different teams and different agendas as a play–certainly a drama, often a comedy, but hopefully not a tragedy. In any collective effort, it’s difficult to make much progress without understanding the stage, the player’s roles, and how their experiences shape their motivations, so let’s explore those. In the diagram below we see the graph illustrated as a new layer between the applications and services. And in a real sense, the graph teams are similarly “in the middle”.


To better understand one another, we can review the unique experience and perspectives that each team brings. What they seek, and what they struggle with reveals a lot about their behaviors and motivations. You can see those summarized in the table below.


The goal of understanding is not to find and correct faults, it’s to create empathy which leads to trust. Trust is key when you are asking anyone, and any team, to try new ideas and new approaches. This leads directly to our next best practice.


## Best Practice #3 – Assume good intent


#3 Invite teams to explore new ways that might work better, rather than trashing their current practices. Explore the source of their resistance and lean toward enthusiasm and encouragement rather than criticism.


Often when we discover a new, and better way, we understandably are excited to share our findings with others. But more than sharing, let’s be honest, we want to convince, to persuade. In my role at Apollo I’ve had the opportunity to listen to many graph journey stories, and one common theme I hear is frustration with individuals and teams that “just don’t get it”–followed by a litany of complaints of wrong doings and wrong thinking. While I can’t share those specific stories, I’ll share one of my own.


When working to spread the graph at my previous company, the second team to adopt GraphQL was resistant to let go of long held beliefs that the best API design was one that insulated their API design from front-end concerns–a pure API if you will. I repeatedly peppered them with evidence and arguments that their proposed schema would lead to complex and slow frontend code and used every critical argument I could muster. The more they resisted the harder I pushed, refusing to allow their schema to be approved. After several months of back and forth on iterations, each better, but none ideal from our point of view, we were at a stalemate.


Here’s the rub–there’s a thin line between sharing a new approach and dogmatically telling everyone in earshot the old approach is wrong. The harder I pushed our new approach, attempting to overcome any and all objections, the more I created an equal and opposite defensive reaction.


The stalemate was only broken when we realized this team’s intent wasn’t wrong at all. They were struggling to balance all the technical and schedule challenges presented by our request to completely rethink their very complex domain. We immediately pivoted our approach, congratulating all involved on the progress we had made together. We then focused attention on how much better their new GraphQL schema was for clients than their old REST Apis. In short, we emphasized our common purpose and found common ground. We celebrated the progress and focused our attention on encouraging further iteration and they gained real world experience in the field. The stalemate was broken, the schema rolled out to the clients, customers benefited, and we publicly thanked the team for their patiences and teaching us an important lesson.


When you hit a wall convincing others their old ways are “wrong”, hitting it harder is not likely to help. Encouragement and invitation are more effective than criticism when asking others to try out new ideas.


## Best Practice #4 – Lean into the Friction


#4 Improvement begins with recognizing the pain of the current process, or the lack of a common goal.


When designing schemas, systems or processes, it’s hard to make the right tradeoffs without a shared understanding of the goals and outcomes you seek. To bring this point home I asked the talk audience to describe the overarching goal for their company’s graph.


The largest contingent, 42%, selected long term strategic value as their number one goal. This supports the growing consensus that the graph is a new strategic layer of the software stack. Another 27% chose a classic GraphQL use case–easing and accelerating product delivery.


Yet, the slice of the pie that catches my eye is the **nearly 1/3** of the audience that indicated **their company has no single shared goal for their graph** . Collaborative efforts and consensus building across teams is challenging in any case, but it’s impossible if we don’t make the effort. If each team follows only their narrow self-interest, we invite the[Tragedy of the Commons](https://www.youtube.com/watch?v=CxC161GvMPc) .


Image: Sketchplanations


Self-interests and internal team priorities aren’t bad, we all have them, but when they are the only measure of good, it’s very unlikely we will find the shared consensus and tradeoffs necessary to create something truly great for the larger org. When there are competing visions for the graph we end up with siloed graphs and no single source of truth. The early benefits of GraphQL, easier client development, efficient queries come quickly but the larger transformative benefits of a composable whole becoming richer with each new addition eludes us.


To bring this point home, here are the words of two large companies from their own internal assessment of their graph program.


#### Quote from major e-commerce company


We extract far too little benefit from our current implementation of GraphQL today, as it is rife with anti-patterns, duplication of types and an inconsistent application of GraphQL conventions. No formal GraphQL team or support mechanisms are seen”


#### Quote from major fintech company


We’re trying to have mobile and web use the same graph, but we don’t have enough authority, and I don’t have a graph product owner. We’ve given up on standardizing schema designs and teams are starting to do whatever they want.


Those quotes highlight the stakes that hinge on effective schema stewardship. Whether the pain comes from an ineffective schema review process, or from the complete lack of a shared goal — the cost is the same–the full promise of the graph is unrealized. Frustration and ultimately disillusionment set in. And most damaging, failure teaches teams to avoid pursuing common goals and return to do only what creates near-term, local benefit.


As graph stewards, our first step to avoiding this negative outcome is to recognize and call attention to what’s not working or what’s missing in the current process, and begin the work to improve it. Which takes us to our next best practice.


## Best Practice **#5 – Adopt a client-centric mindset**


#5 The measure of a GraphQL schema’s fitness is its value to clients. It’s crucial to remind schema authors their goal is to provide service, not just provide access to their service.


While GraphQL can be used for service-to-service APIs, it’s uniquely suited and most powerful when used by client applications. If you agree, it suggests that the best way to measure the quality of a schema, or it’s fitness, is to measure it’s utility to those client applications. If the schema is dry, orthogonal and perfectly abstract we might traditionally say it’s a good API because it’s likely efficient and client agnostic. Yet I invite any service team API author to review the client side code that is used to consume, orchestrate and transform their API results into rich customer experiences. All too often that code is complex, brittle, difficult to write and execute quickly, and duplicated by each and every client.


Architects who design buildings and structures practice this every day. If you ask an architect to design you the perfect room or house, they immediately start with your needs. Who will use it, and how? What is your daily routine like, etc.? It’s the same with schemas. When evaluating what makes a schema good, and how to decide between schema design tradeoffs, you can’t go wrong by making sure that the teams that are going to use them weigh in.


## **Part 1 Recap**


In this post we covered a lot of ground but we’re only half through the top ten best practices for guiding your teams toward the unified graph schema you seek. So let’s recap.


We began by considering the term Governance itself and chose Stewardship as a term which better captures the collaborative approach we need. We then explored how the empathy and trust we need to effect change is rooted in our understanding of the past experiences and future motivations of all the players involved. In best practice #3, we expanded on that theme of empathy by arguing that when seeking change, an invitation and encouragement work better than criticism. In best practice #4, we explored the consequences of failure and inaction by exploring the tragedy of the commons and sharing some real world quotes from graph champions who have felt them first hand. And finally we wrapped by reminding ourselves that all this work is aimed at creating better experiences for our customers, and our first customers are those clients who consume the schemas we write.


Checkout[part 2 to learn about the rest of the ten best practices](https://www.apollographql.com/blog/community/graphql-champions/10-best-practices-for-schema-stewardship-part-2-of-2/) where we will explore a key decision–which cross-team organizing model can best help your schema collaboration efforts.


**Keep moving forward!**


Written by


Dan Boerner


[Read more by Dan Boerner](https://www.apollographql.com/blog/author/danboerner)
