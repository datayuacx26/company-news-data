---
schema_version: "1.0.0"
document_id: "2946df5f3c527f3ebb097a451c56c906b5539c8976508bb9338497d8da799a97"
company_key: "yc-emi-labs"
company: "Emi Labs"
source_id: "yc-emi-labs-rss-87232385bc09"
canonical_url: "https://medium.com/@EmiLabsTech/delivering-value-on-your-first-week-at-a-tech-startup-d337838969cc"
published_at: "2021-10-13T00:35:07+00:00"
first_seen_at: "2026-07-27T09:02:34.241036+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:d0aea4e924ad233cfbdc9766ba31f8ccd16e14840f271b8465fd5b7c19e877a4"
---

# Delivering value on your first week at a tech startup

Startup Lessons


Tech


Bootstrapping


First Week


Value Creation


# Delivering value on your first week at a tech startup


[Emi Labs Tech - Ravens](https://medium.com/@EmiLabsTech?source=post_page---byline--d337838969cc---------------------------------------)


4 min read


·


Oct 13, 2021


--


*by*[Diego Schvartzman](https://www.linkedin.com/in/diego-schvartzman-3a54112/)


## My first week at Emi


I was the third person to join the engineering team at Emi. I wanted to join a startup at an early stage to build a great tech company while learning as much as I could, to surround myself with exceptional people, and to make a meaningful positive impact on the world.


Right after joining, everyone at Emi spent a lot of time so I could understand the mission, the business, the culture, and the tech stack.


Andy, the CTO, tasked me with scaling Emi’s conversational design capabilities. Designing a conversation is part of Emi’s core business — that’s the way our bot knows how to talk to candidates applying for a given position — and it was, at the time, the undisputed bottleneck for onboarding new customers.


## The challenge


Conversations at Emi are highly customized, feature-full complex graphs with dozens of nodes and intricate connections, programmatically built in hundreds if not thousands lines long python modules, they are run in our in-house conversational engine.


It could take the CTO from a few hours up to a few days to build a complex conversational script. This clearly wasn’t acceptable going forward. Someone besides Andy, and eventually myself, should be able to do this, and do it much much faster. At the same time, we wanted to keep the flexibility that we had by coding it.


The conversational script was a python module that generated a json object, which was the actual thing that the bot engine reads for execution.


Andy purposely didn’t give me any prescription about how to solve the problem, so I could figure out my own ideas and we could compare our thoughts later.


## The idea


I thought I could probably gain a quick win by refactoring the code, and adding utility libraries, but that wasn’t going to cut it long term, so it wouldn’t really be a small step within a larger vision.


Another pitfall I almost ran into was trying to improve the json format of the conversational scripts to something much simpler, clearer, and normalised. This would have taken weeks, and required to recode the bot engine, which I knew nothing about and had no unit tests at the time. More importantly, the short term value of such a venture was far from clear, apart from satisfying my obsession with it.


After analysing several options and validating with Andy, the more attractive initiative was to build a visual editor for the graph. The editor would be a fancy, friendlier way to generate the json conversational script.


We would still have the flexibility to build conversations in python if we want to, but not with the UI editor. At least not for now. Long term we could keep improving the editor by adding the features we need to keep replacing programmatic flows.


## Deliver value every week


As I was analysing the problem I was presented with, I overheard Mate — the CEO — talking about the cultural value of some other company — probably Stripe — in which they strive to deliver something amazing every week. Although it might sound unrealistic, I found it inspiring, and overall a good mindset to approach work. Learning these lessons is one of the reasons I joined. Rapid prototyping, failing fast, tangible value every week. That’s the path to exponential value. That’s how you get exponential growth.


At Emi we have all hands meetings at the end of each week, in which there’s a section for brief demos, and I thought it would be cool to demo real value on my first week, knowing that no one really would be expecting it.


So on top of figuring out the long term vision for scaling Emi’s conversations, I would have to have a first iteration implemented and real value delivered in just a few days.


## The EOW demo


I made a list of the requirements for the visual editor of conversational scripts, and started looking for whatever tool was already available.


After some digging, I decided to give a try at Uber’s react-digraph open source project.


I checked it out and found an example UI with a split view between a graph and a json representation of that graph. This made total sense for me, UI controls for manipulating the graph would take time to implement and, until then, people could manipulate the json and see changes in the graph in realtime.


Having just a few hours left, I made a tool to convert Emi’s json format to the json format that my modified version of react-digraph uses (I made a few changes to the tool in order to display the graph data I wanted to).


I picked the most complex conversation we had, exported to the tool and, in no time, I was showing it at the demo.


This was the first time the team got to see a graph representation of a conversation. The non tech team stared in awe, realizing the sheer challenge Andy had been dealing with.


Press enter or click to view image in full size


A blurred look of the first version of the tool


For me, it was a learning experience, and a validation of the first small step in what would become a full fledged visual editor.


In a few weeks we had a functional tool, and two years later most of our conversations are being written, tested, and deployed with the editor.


Every week I try to deliver something awesome. Most of the time I fail. But as a manager, and part of the onboarding team at Emi, I do my best to receive newcomers with a clear 90 day plan, individual objectives, and to enable them to deliver value fast. A problem to solve and context to solve it. That’s the fair chance anyone should get.
