---
schema_version: "1.0.0"
document_id: "da3e8a4e687b4f1725f223f0677403f0ab9aef0a519876e7ab649febb5960ef2"
company_key: "yc-opentrons"
company: "Opentrons"
source_id: "yc-opentrons-rss-767f8d415bdd"
canonical_url: "https://opentrons.com/archives/news/unifying-opentrons-design-systems-lessons-from-3-years-of-unification-work"
published_at: "2026-02-13T14:00:00+00:00"
first_seen_at: "2026-07-25T17:41:16.998093+00:00"
fetched_at: "2026-08-20T03:44:47.604730+00:00"
content_hash: "sha256:e36d2b232c47163632853e3a82b39e64af9a13a26c6c7437b753872418016e63"
---

# Unifying Opentrons’ design systems: Lessons from 3 years of unification work

When I first started the process of unifying our design system, I remember being more overwhelmed than I’d ever felt in my career. This work was unlike any other process I had undertaken before. I wasn’t even clear on what the end goal needed to be…a three-tiered tokenization structure with a standalone website with thorough documentation on each component in the system?


I consumed every resource I could get my hands on to try and wrap my head around *what* the system needed to be. Most of what I read came from large, multi-brand teams with full-time design system and engineering support. Little did I know, I was looking at the wrong guidance for what I was building for my team.


My hope is that this guide helps someone on their own journey in the world of design systems. It is best served for someone responsible for unifying a system for a smaller design team (less than 8) with limited resources. I’ll go through all the mistakes I made when I first started and how I think about things differently now, three years into the unification process.


Let’s begin.


#### Lesson #1: When looking for guides, pay special attention to teams that resemble your size and constraints


Out of all the free content you can get around building a design system, a majority of it skews towards multi-brand, large, well-resourced teams. It makes sense. They have better resources, more funding to explore the fringes of what design systems can do and as a result, get more visibility for it. These designers often dedicate 100% of their time to design systems and have multiple designers and engineers on their teams.


That kind of resourcing was not what I had at Opentrons when I first started. By only hearing from voices who had different needs and level of resourcing than me meant I thought I needed as complex of a system as Atlassian or whatever big tech company Nathan Curtis was hired to help with.


The reality is, is that I would be devoting 25% of my resources max, with 15% of an engineers time to build a design system to start.


Questions I’d ask now are:


- What level of complexity is most beneficial to designers and engineers now?
- What is most important to build in my first 6 months?


Documentation example of a designer proposing an edit to an existing component in our Component Sync with Engineering during our design process


#### Lesson #2: Design systems are more about systems of thinking and communication than components


The easy part are the components. The hard parts are the cultural shifts in process *for how designers and engineers* *make decisions together.*


When I started at Opentrons, design and engineering each made reasonable choices within their own context around what should be a component, what logic it should have, how reusable it should be. We prioritized speed over intentionality and in turn, had design systems for each product containing components with overlapping logic. With every feature we created more tech debt.


Regardless of if you’re building a design system from scratch or unifying existing systems— *building components is for your present; building systems is for your future.*


Here’s a common example: You’ve just published a beautiful component library to Figma that is reflected in the codebase and handed it off to your design team. A month later a designer encounters a new UI problem and discovers that none of the existing components quite work for their use case. They have two choices:


1. Build a new component that matches their needs completely and add it to our design system.
2. Modify an existing component to make it fit their needs.


How does that designer make a decision that serves both current and future needs? What if an engineer, upon receiving the designs, decides to make a different decision?


These decisions happen all the time in feature work and without a shared mental model around how/why we build components, our inconsistencies will become future tech debt.


At Opentrons, the real work was in building the collaborative scaffolding between individuals and teams:


- Review PRD’s early to include design system scope
- Partner with engineers to build, unify and deprecate components together through feature work
- Upskill designers to think systemically instead of locally
- Establish shared naming, logic, structure and documentation patterns
- Create a separate DQA components enable faster feature shipping times
- Among many others! (seriously)


Changing existing culture of how two teams collaborate takes time. But the invisible layer of systems thinking; shared language, logic and structure, is what determines whether a design system thrives or decays.


Questions I’d ask now are:


- What systems and touch points exist to maintain integrity, for the present and our future?
- (If unifying an existing system) What are engineers thinking about when building a component?
- How much do I know about how engineers build their components?


The first prioritized list of components to add to Helix, made without dev input


#### Lesson #3: You’re not going anywhere without an engineering counterpart


When I first started working on design systems I went all out. I dove straight into researching how to build it, broke it out into phases over the course of the year, and prioritized outstanding components all without engineering insight.


Granted, I didn’t have engineering buy-in at the start but looking back I wish I slowed down and spent my energy figuring out who on engineering could be my partner-in-crime and building the relationship to make that happen.


A design system succeeds only when *both* design and engineering build and maintain it together.


Questions I would ask myself now are:


- Who on the engineering team is genuinely excited about this work and willing to partner?


#### Lesson #4: Get help when you’re stuck


I was lucky. When I started, ADPList had just launched and I got to speak with incredible mentors like Davey Fung, the host of Design System Hours Podcast and Alex Fortney, who’s leading the design system at Clay. Their guidance helped unlock doors that I had been jamming my shoulder into for weeks.


If you’re a mid to senior level designer, youy likely didn’t learn about design systems in school. This work is nuanced, multi-disciplinary and deeply collaborative.


Questions I’d ask now are:


- Who in or outside my network can support me?
- What education is my company willing to fund?


#### Lesson #5: Not everything needs to happen at once


Ah. Honestly, this lesson for me reached beyond the realms of just my working life. I am so used to being able to deliver a complete package of how a feature should look and behave like. I abashedly admit, that up until recently in my career, I felt like “perfection” was a state that could be achieved in design. Pixel-perfect as they say, right??


I have been humbled these past three years. What do you mean, I couldn’t deliver a 30 component deep design system with perfect documentation, well thought out variables and naming structures all built into Figma all within 6 months as a part-time systems designer?


Systems take time.


Systems take teamwork.


And systems are always evolving.


#### So, in short, if I were to do it all over again…


**Lesson #1: Give yourself patience as you wrap your head around what your system needs to be.**


Study design systems at scale *and* ones built by teams your size (if you can find them, they’re not often published!).


**Lesson #2: Give your team space to develop new ways of working with one another.**


You’re changing culture first, components second. Changing culture takes time and trust.


**Lesson #3: Find a teammate, since you can’t do this alone.**


Build strong relationships with Engineering counterparts to move together every step of the way.


**Lesson #4: Give yourself support when needed if you get stuck.**


Ask for help! Ask for funding to take a class or attend a conference. Design system people are very passionate about their work and there are other people in your same exact position that can support you along the way.


**Lesson #5: Give yourself time to build your design system.**


Know that not everything needs to happen at once. Experiment with your processes, change small things frequently and make getting feedback part of your process.


And remember, above all—whatever is built needs resources to be maintained.


If you find yourself in the middle of creating or unifying a design system from scratch and need some mentorship, set up a time to chat on[ADP List](https://adplist.org/mentors/melanie-mencarelli) !


The post[Unifying Opentrons’ design systems: Lessons from 3 years of unification work](https://opentrons.com/archives/news/unifying-opentrons-design-systems-lessons-from-3-years-of-unification-work) appeared first on[Opentrons](https://opentrons.com/) .
