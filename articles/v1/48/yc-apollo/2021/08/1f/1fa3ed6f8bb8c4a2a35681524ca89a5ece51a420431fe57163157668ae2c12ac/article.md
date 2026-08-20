---
schema_version: "1.0.0"
document_id: "1fa3ed6f8bb8c4a2a35681524ca89a5ece51a420431fe57163157668ae2c12ac"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/dx-audits-a-framework-for-developer-advocacy"
published_at: "2021-08-18T09:18:14+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:e951b1fa100e253fd35759849ebb8ee22563a93ae8d637d672714683afa8d4f7"
---

# DX Audits – A Framework for Developer Advocacy

If you ask a developer advocate (DA) what their day-to-day looks like, you’ll likely get a very different answer from each person you ask. Interestingly enough, I’d wager that if you asked two DAs on the same team, you’d still get two totally separate answers.


As DAs, we’re tasked with helping developers be successful, but how we do that is basically up to us. From content creation to product feedback, how we help developers varies and is generally specific to the personas of the developers we’re trying to help.


In this post, I want to present a framework we’ve been using at Apollo, which has helped us organize our day-to-day work, provides deeper insight into our contributions, and has given us a structured approach we can teach to new developer advocates as we grow. We’ve been delighted with it so far. Maybe it can help you as well.


## The Framework


Product friction is a core concept in understanding usability and improving user engagement (i.e., their experience). For us, product friction is the pain points developers experience when trying to be successful with GraphQL and the Apollo platform.


The goal of DX Audits is to document, report, and address product friction by having developer advocates experience common developer workflows, document and report their findings, and surface actionable steps to take to reduce friction. Remove the friction, and more developers will be successful!


Each audit is scoped to a particular developer persona (or, as we call them, journey stages) and consists of use cases to audit. A use case is essentially a workflow that a developer would have to go through to complete a given task (i.e., add monitoring to a GraphQL server).


Each use case is documented in a friction log. Friction logs are a detailed document explaining the steps taken to achieve the goal and how each step felt to the auditor.


Once the friction logs are completed, the auditor should record possible action items to address any friction points discovered and shared them with the appropriate people.


Finally, the DA should work to get the action items completed until the next cycle.


A diagram showcasing the flow from journey stage to action items.


The entire audit process is comprised of 4 phases. We’re currently testing a three-month cycle at Apollo. The cycle time is designed to allow for DAs to adapt quickly to changing company/org initiatives and messaging while still allowing enough time to follow up on any action items that stem from the audit.


A diagram showcasing the different phases each part of the flow belongs to.


### Discovery Phase (1 week)


The purpose of this phase is to organize feedback and input from various sources into use cases that we want to investigate or that we know are causing friction for developers.


Where you should be looking:


- **Feedback from the community and customers** is discovered from GitHub, forums, social media, events, surveys and forms, issues raised by solutions, support and customer success, etc.
- **Input from stakeholders** is information about product updates, roadmaps, engineering timelines, etc. Depending on what developer persona you are auditing, you should be reaching out to different teams regularly.
- **Intuition and personal experience** are the friction points you personally experience when developing within your product’s ecosystem.


At the end of this phase, you should have a list of use cases from your discovery and a shortlist to complete for the current audit. We store ours in Airtable, which allows us to prioritize and share them with others.


Screenshot of the Airtable UI showcasing a table titled “Use Cases” that contains friction logs grouped by the creator and filtered down to the ones in progress.


💡 When deciding on use cases to audit, consideration should be given to current company/org objectives and important KPIs!


### Building Phase (4 weeks)


The building phase is where you complete the friction logs for your use cases. Generally, this is accomplished by building projects that allow you to experience the use cases you want to audit.


What you build could be anything from example apps to working completely within a given product UI. The important part is that you complete the workflows from start to finish with as much empathy and minimal assumptions as possible.


💡 This is a great opportunity to create some content as well! For example, live streaming yourself going through these workflows and open-sourcing any projects you build can be a great resource of information for developers and an opportunity to get feedback from the community!


**Using Friction Logs Effectively**


Here are some useful best practices we’ve landed on for making the most out of friction logs!


- **Channel your empathy** – You want to approach the workflow as a developer might. Try to make no assumptions and question if the step would be more difficult for someone without your insider knowledge.
- **More detail is better than less** – The goal is to provide enough context about the steps you took to achieve your goal that anyone can read the document and understand or retrace your steps to reach the same outcome.
- **Don’t forget the good** – When you experience something delightful, make sure to document that and also be sure to include the positive experiences when summarizing a friction log!
- **Consistency is key** – Everyone should be using the same template to create friction logs. Of course, DAs should have the freedom to use whatever tools allow them to be most productive, but friction logs need to be accessible and structured similarly.


💡 Aja Hammerly’s “Introduction to friction logging” (linked in the “More Resources” section below) was a big inspiration for us.


### Reporting Phase (1 week)


During this step, DAs should organize their discoveries into a report to share with stakeholders and the company at large. It should include the reasoning for choosing the use cases for the audit and summarize each one individually.


This is your opportunity to discuss the friction logs and pitch any solutions (or action items) that you think would help remove that friction. If you encounter the same friction you’ve encountered in previous audits, be sure to include references to that in your report. The more information you have verifying a problem, the easier it will be to get buy-in for your proposed solutions.


It’s also a great opportunity to share what you enjoyed about the process! Not only will your coworkers appreciate the praise, but it can also identify things that are working well that could be adopted elsewhere.


💡 Make sure to discover who would own any action items proposed in the friction logs so that you can follow up on adoption and progress!


### Implementation Phase (6 weeks)


Over the next six weeks, DAs work on the action items from the audit that fall within their responsibilities and get any other action items prioritized/adopted by the appropriate teams.


This is where most of the work we associate with developer advocacy happens. DAs will create docs and content to fill gaps, maintain content channels, build new content channels, advocate for product changes, and more through action items discovered during the audit.


At the end of this phase, DAs should retro the action items, update the associated report, and provide an update about the status of each initiative from the cycle.


💡 Tracking the impact DAs have across the company through audits is a great way to get qualitative data about your team’s successes and a documented list of achievements that your team can use for performance reviews. We also reference associated KPIs to action items, which helps connect the dots from our work to quantitative metrics.


## Conclusion


Bringing structure to developer advocacy can be difficult. It’s a job that requires a lot of freedom and where the results of your work can be hard to quantify and are rarely flashy. However, by applying this framework, we’ve been able to have more productive conversations about developer issues with other teams, produce artifacts that are referenceable and sharable with the company, and create an environment of consistency that makes it easier to onboard new developer advocates.


## FAQs


**Is this all developer advocates do?**


Not at all! We have other long-lived initiatives we work on, time dedicated to education, and a lot of the things you associate with advocacy, like content creation, happen during the implementation phase of the audits.


**Does the process get in the way?**


So far, we’ve found the process (especially friction logs) to be beneficial and help us be more intentional with discovering friction! Our first friction log created discussions across multiple teams and has already surfaced many ways to improve that workflow across both open-source and the Apollo platform.


**What if I can’t complete an action item quickly?**


Not everything we do will be a quick win. Sometimes the best thing to help developers might require you to advocate for quite some time before you’re able to make the change happen, if at all! This is also why it’s important to record your friction logs and action items in a spreadsheet or database.


**What if I can’t do all the use cases I discover in my audit?**


Much like product development, there will be things you want to do that you won’t be able to! This is another reason to document and discuss the issues you discover with your team so you can reprioritize as needed. So while you won’t be able to cover them all, you can be sure you’re working on friction points that will help developers and your company be successful.


When you first begin this process, you’ll likely have a significant amount of use cases because you haven’t documented any of them yet. After that, you’ll likely discover significantly fewer use cases with each cycle.


**Should I revisit use cases?**


Absolutely! During the discovery phase of each cycle, you’ll want to identify the most pressing use cases, and it could very well be one you did recently or quite some time ago. So it really depends on where developers are feeling the most friction!


It could also be interesting to do user testing and either record their experience or ask them to!


**What if action items don’t get adopted by other teams?**


The goal is to offer possible solutions, not dictate what work should be done. Sometimes you’ll propose things that aren’t possible for some reason, and that’s okay. The key thing is that now you have something documented, and you can always advocate for it again in the future, and if it continues to be friction you experience in future audits, you’ll have more data to help make your case.


**Who should use this?**


This framework is most effective for developer advocates on a team of at least 3 and in a company that already has defined teams for marketing, product, solutions, etc. If DAs are wearing many hats and taking on responsibilities from adjacent functions, then this framework most likely won’t work! It would be most beneficial for teams where some members may be new to developer advocacy as it provides a framework that everyone uses and consistency is key to onboarding!


### Contributors


The creation of this framework was a collaboration across many teams at Apollo. Thank you to everyone who tested this and provided input and feedback as we worked on the process!


A special thank you to Peggy Rayzis for pushing me to crystalize our process on the DevRel team and for introducing me to friction logs! How we recorded our experience going through these workflows was still a mystery, and when I saw friction logs, I knew they would work wonderfully!


A special thank you to Khalil Stemmler for helping me build up developer advocacy at Apollo over the last year (and the awesome diagram in this post 😊) and to the rest of the DevRel team for improving and refining this process!


### More Resources


- An introduction to friction logging:[https://developerrelations.com/developer-experience/an-introduction-to-friction-logging](https://developerrelations.com/developer-experience/an-introduction-to-friction-logging)
- Discovering the right KPIs:[https://www.marythengvall.com/blog/2020/12/14/first-understand-the-company-goals](https://www.marythengvall.com/blog/2020/12/14/first-understand-the-company-goals)


Written by


Kurt Kemple


[Read more by Kurt Kemple](https://www.apollographql.com/blog/author/kurtkemple)
