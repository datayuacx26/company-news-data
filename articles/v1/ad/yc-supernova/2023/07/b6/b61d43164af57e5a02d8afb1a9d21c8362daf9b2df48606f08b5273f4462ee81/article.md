---
schema_version: "1.0.0"
document_id: "b61d43164af57e5a02d8afb1a9d21c8362daf9b2df48606f08b5273f4462ee81"
company_key: "yc-supernova"
company: "Supernova"
source_id: "yc-supernova-rss-864f3bee1480"
canonical_url: "https://www.supernova.io/blog/design-systems-documentation-is-a-moving-target-q-a-follow-up"
published_at: "2023-07-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:40.084347+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:59b97f7d0cb4d7d416fe00c08b29a2aa4655a161b71e7d5764af4aef880dcfc1"
---

# Design Systems Documentation is a Moving Target — Q&A Follow-Up

We hosted an expert panel, “Design Systems Documentation is a Moving Target” with industry leaders[Alberto Calvo](https://twitter.com/pumpkin) (Senior Manager, Design Infrastructure at Maze),[Luke Finch](https://twitter.com/LC_Finch) (former Product Designer at NewsKit), and[Romina Kavcic](https://twitter.com/rominadesigner) (creator of[Design System Guide](https://thedesignsystem.guide/?utm_source=supernova&utm_medium=referral&utm_campaign=ds-docs&utm_content=blog) ).


If you missed the live panel, you can still[watch the recording](https://www.supernova.io/design-system-documentation-is-a-moving-target?utm_source=blog&utm_medium=link&utm_campaign=ds-docs&utm_content=q&a_blog) or[read the summary](https://www.supernova.io/blog/design-system-documentation-is-a-moving-target-panel-recap?utm_source=blog&utm_medium=link&utm_campaign=ds-docs&utm_content=q&a_blog) of the discussion. We got a ton of audience questions that we weren’t able to answer during the talk, so our panelists have graciously shared their thoughts below.


Let’s dive in!


### Q: How do you organize design system documentation, and what points should you cover in documentation?


**Luke** : Documentation should be a reflection of your communication structure and knowledge. The best way to organize your documents is in a way that makes sense to you and your colleagues. Cover what people find useful. Use all the UX research techniques available to get a good understanding of people's wants and needs. Make sure you’re also documenting how you document.


**Romina** : Add all the relevant and helpful information for your consumers. No need to overcomplicate the process. Make it as simple as possible and meet people where they need you and the documentation.


**Alberto** : Covering the foundations solidly allows you to reference the reasoning that led you to make confident design decisions in other, more specific parts of the system without explaining everything all over again. We have decided not to reinvent the wheel: Foundations, Components, Patterns, Processes, and Resources. We always go from the most general to the most specific within each section.


### Q: Did you split up the documentation (while still being collaborative) based on each person's niche area? Example: A developer would help articulate and write for React components/frameworks etc. A product designer would articulate UX behaviors.


**Alberto** : Not quite. The designer starts our process, and we gradually refine it together. On the other hand, each discipline is in charge of its examples, and our engineer covers the migration documentation part.


**Luke** : To a certain degree. A component would have an owner, usually the UX designer who picked up the first investigation tickets. But the whole process is very collaborative and takes contributions from all parties.


**Romina** : There should be one person responsible for writing who gets more info from all sides. It is much easier to go faster because not everyone is skilled in writing and explaining.


### Q: Does it make sense to have design system documentation and UX writing in the same place?


**Alberto** : Absolutely. UX writing is a fundamental part of the experience, just as important as design or code. If we isolate it, we are losing an opportunity to consolidate all knowledge in a single source of truth. Combine forces to make something great.


**Luke** : For a system where a design system serves a single product or brand — go ahead! The issue comes when you’re handling multiple products or brands, which may need a more nuanced level of UX writing. In such cases, I think providing strong guidelines and best practices is a good starting point to let future writers adapt to their needs.


**Romina** : I agree. UX writing should be part of the guidelines. I believe better UX writing can make a huge difference in understanding the UI.


### Q: Do you split design and technical/development documentation?


**Alberto** : I believe that the documentation must be able to serve anyone, regardless of the discipline. Even people from other departments who aren’t makers. For that, you must find ways for everyone to understand it without drowning in the most technical details of any discipline. It's a tricky balance, but there are ways to achieve it with the right team and tools. We keep iterating to get it right.


It really depends on your design system setup, and what the goals of your system and documentation are. If you’re trying to establish design and development standards across a business, then it may be a sensible idea — as design style guides and development standards rarely crossover. However, for most people, documentation is about promoting shared learnings at the component level: What is this component? What does it do? How can its appearance be changed? - those questions are relevant to both parties.


**Romina** : It’s merged. The team should understand all the aspects that build the product.


### Q: Can you tell us about your experience with component documentation for developers? What is the fidelity of the documentation you have, and what does it usually include, how do you define component properties and component names?


**Alberto** : It’s one of our improvement areas. We haven't managed to bring all the documentation and practical code examples to Ariane yet, but we're still working on it. We do it together to define the components' names and their props. It's one of the most complicated parts of the process and also one of the most enjoyable. Sometimes we spend hours debating all the implications that a simple name can have on understanding the system and its use. Words matter.


**Romina** : Honestly, it depends. :) You can find more inspiration in the public design systems.


### Q: How do you make people read the documentation and familiarize themselves with the work that has been done already?


**Romina** : I love adding “personalized” tips and learning. I love adding some fun content (memes, illustrations, comments, helpful links). It is also very important to shorten everything because no one has time to read essays.


**Luke** : Answer any questions you get with a link to the docs. If you don’t have a link to an answer, it’s a good indicator of what needs to be documented next.


**Alberto** : I believe it’s everyone's responsibility to learn how to get the best out of the tools at their disposal. We realize that not everyone reads every detail of what we write when we post it, but the more nuance we cover, the easier everyone’s job will be afterward when the issue arises. Not only because we know that we can provide a link to a specific part of the documentation without explaining everything again but because each detail results from a conversation that has led us to decide and internalize it.


### Q: Do you count the use of documentation as a form of design system adoption?


**Romina** : I prefer talking with people and understanding what they need and how we can help them optimize their process and make everything even more useful.


**Alberto** : It's not something we measure, but it certainly shows when someone has invested time in understanding the system and when they haven't.


**Luke** : It’s certainly a factor that can be used to help paint a larger picture of systems adoption.


### Q: What to do when we have a federated design system and some designers steal components from other teams to fit in their design, ignoring some stuff that is documented?


**Alberto** : When that happens, it's a sign that something is not working, and you have to sit down and talk, find the best compromises, and, depending on the context, sometimes make concessions. It’s crucial to be pragmatic and consider what’s best for the company at that particular moment. Then, learn from experience, and iterate for the next time, whether in communication, deliverables, or building a team spirit where people work together to create something better instead of following our individual preferences.


**Luke** : Use it as a learning opportunity for both parties. Why do you feel like you’re being ignored? Are you not promoting your documentation enough? Are you being too controlling in forcing consistency for the sake of consistency?


Maybe the component from elsewhere serves their immediate needs better than what’s in the system.


Enforcing design system usage, and making people refactor their code to improve system adoption is an expensive task.


**Romina** : Building a design system is more about building relationships than anything else. Ensure you work on transparent communication and share better workflows than the one you mentioned.


### Q: Where do you document components for internal use, in a fast way? As in.. not on a public website.


**Alberto** : Figma's metadata, code comments… try to meet people where they are.


**Luke** : I’m a big fan of putting knowledge as close as possible to where it's relevant. GitHub discussions, issues, wikis, etc. Or, add some annotations to a design file.


**Romina** : Figma.


### Q: What's the ideal or best practice based on your use case to provide documentation for component versioning?


‍ **Alberto** : We started using Semver in our Figma libraries because it has worked well for me in other experiences, but over time we have discovered that in a small team like ours, we didn't need such a rigid process. We solve it with good communication :)


**Romina** : You can have logs where you mention all the changes, but we don’t have a workflow for component versioning. There's only one version that is valid, and that is the latest one.


---


Design system documentation is a living, breathing thing. As you heard from our experts, there are a number of ways to approach it. We hope their advice is helpful for you and your team when writing and updating design system documentation.


If you’re looking for a tool to house your documentation, check out Supernova! Alberto and the design system team at Maze leverage Supernova for their design system[Ariane](https://ariane.maze.co/?utm_source=supernova&utm_medium=referral&utm_campaign=ds-docs&utm_content=q&a_blog) .
