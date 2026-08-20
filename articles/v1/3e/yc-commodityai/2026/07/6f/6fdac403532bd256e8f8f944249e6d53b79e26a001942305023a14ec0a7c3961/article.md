---
schema_version: "1.0.0"
document_id: "6fdac403532bd256e8f8f944249e6d53b79e26a001942305023a14ec0a7c3961"
company_key: "yc-commodityai"
company: "CommodityAI"
source_id: "yc-commodityai-news-import-2bca2e2104a7"
canonical_url: "https://www.commodityai.com/posts/rethinking-the--operation-designing-processes-for-an-ai-native-system"
published_at: "2026-07-21T12:00:00+00:00"
first_seen_at: "2026-07-23T06:13:10.102548+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:2af65fdc42256766259cb8d5d344efbe7d20dbeeac420944e838fc118b154766"
---

# Rethinking Commodity Processes for an AI-Native System

**Key Takeaways:**


- To successfully implement an AI-native system, you cannot simply port your old processes into new software.
- Legacy processes are fossils of old constraints built for systems that merely stored data while humans did the reading and typing.
- Modern AI systems can actually read and connect information autonomously.
- To unlock this value, you must take a step back and redesign your operation: standardize your data inputs, strip workflows down to only what your core decisions require, and sequence your rollout to build operator trust before aiming for full automation.


---


On a recent call, a customer asked me a question I've come to think of as the implementation question: "The way I run my business was designed around the systems the industry has always used. How do I take a step back and rethink it philosophically rather than just follow what the industry decided for us?"


This note is my honest answer. It's short on theory and long on scar tissue: the patterns I've seen watching commodity traders implement systems, and the small set of rules we now treat as non-negotiable.


Start with what the exercise actually is. Every business carries a set of requirements, namely the decisions it must make, the obligations it must meet, and the questions it must be able to answer. A system is just a machine for meeting those requirements. The mistake almost everyone makes is skipping the requirements and copying the process: the specific sequence of steps some team invented years ago to meet those requirements given the tools they had at the time.


Processes are fossils of old constraints. The spreadsheet columns, the double entry, the Friday-night reconciliation: each one made sense against a limitation that may no longer exist. When the tools change and the process doesn't, you end up doing archaeology every day without knowing it.


This matters more right now than it has in decades, because the tools just changed shape. Systems until now were databases with forms on top: they stored what people typed and demanded that the typing be done their way. An AI-native system is different in kind, as it reads. It takes the emails and documents your business already produces, classifies them, pulls the information out, and connects it to the right contract, shipment, or letter of credit.


If you squeeze a process that was built around your old ERP into an AI system, it might work, but you're fitting a square into a circle. The step back is asking: with a system that reads, what shape should this process be?


So my method, whenever I'm asked to rethink an operation, is not to start from a blank page. I start from the pitfalls.


## The Five Pitfalls


Anchor on these first. Every one of them looks like a good idea at the moment of decision, which is what makes them pitfalls.


- **Buying flexibility instead of judgment:** The classic ERP sale pitches infinite flexibility as insurance against unknown unknowns. But flexibility isn't a feature; it's deferred design work, billed later at a worse rate. Without a strong systems team, it degrades into "configured by whoever got there first."
- **Many ways of doing the same thing:** Going from one way to two doubles complexity. Exceptions also poison automation: a system can learn one way brilliantly and twenty ways poorly.
- **Porting the old process into the new system:** The square into the circle. Teams recreate their spreadsheets inside the new system because the process is familiar. But if a step exists because "the old system couldn't do X," and the new AI system does X on its own, the step should die, not migrate.
- **Tribal knowledge as architecture:** "That supplier never puts the reference on the invoice, you just have to know it's them from the format."These are undocumented dependencies on individual people. Tell the counterparty plainly what you need on their documents; most comply within a cycle.
- **Spending trust you haven't earned:** Implementations die from a team that stopped believing. Trust is the rollout currency, meaning a simple task working robustly every single day buys you the right to automate the next one.


## The Principles That Fall Out


- **One way of doing everything:** Use this as an ideal, not a rule. It forces the question at every fork in the road: is it absolutely necessary to create a second way of doing this?Split deliberately, and never let a second way appear by drift.
- **Opinionated defaults beat blank pages:** Good systems arrive with a point of view. Your business has real nuances, but nuance should be the justified exception on top of a standard baseline, not the starting posture.
- **Clean data, in one place, before anything clever:** The foundation of any real AI capability is unglamorous: capture every document, classify it correctly, extract the information, link it to the right entity. Data quality isn't a data team's hobby; it's the input discipline of the operators, every day.
- **Pick your anchors early:** Decide up front which identifiers tie your world together (booking number, contract reference) and put them on everything. Heroic matching by the AI is a fallback, not a design.
- **Track what a decision needs, nothing more:** For any process, ask: what decisions does this data actually feed?Data you capture "because we might need it" is not free. Its presence obscures the fields that matter.
- **Let corrections do the teaching:** Start with every document reviewed by a person. Every correction an operator makes should permanently teach the system. Early friction isn't the system failing; it is the system being taught by the people who know the business best.


## How to Actually Take the Step Back


A concrete exercise, doable in an afternoon with the two or three people who really run the operation:


1. **List the decisions, not the steps:** Not "we update the tracker on Friday" but "we decide when documents go to the bank."The decisions are the requirements; everything else is habit.
2. **Name the data required:** For each decision, name the data it truly needs and where that data first appears in the world.
3. **Ask the fossil question:** Which limitation made this step necessary?If the limitation is gone and the system can read the document on its own, retire the step.
4. **Choose anchors and standards:** Announce them upstream to your own team and to counterparties. One reference on every document; one way per process unless a split is justified out loud.
5. **Sequence by trust, not ambition:** Pick the simple, high-frequency thing and make it boringly reliable first. Expand only on proof. The goal isn't immediate full automation; it's getting to a point where "nobody would go back."


One warning: none of this means stopping the shipments while you redesign the company. Revolution is for the design; evolution is for the rollout.


## The Honest Summary


Take your requirements seriously and your processes lightly. Anchor on the known pitfalls. Default to the standard way, capture what decisions need, and let the people who run the operation teach the system until it deserves their trust. None of it is clever, as all of it is discipline. Happy to argue about any of this over a call; disagreement is usually where the good design comes from.
