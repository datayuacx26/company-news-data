---
schema_version: "1.0.0"
document_id: "c18be371d93d49da776c5cb66a9946ced5061d5959c2fc18022d6ce53a437e01"
company_key: "yc-nova-credit"
company: "Nova Credit"
source_id: "yc-nova-credit-rss-8f3179f5a680"
canonical_url: "https://medium.com/@nova-credit/technical-documentation-at-nova-credit-1040e467f76c"
published_at: "2021-09-28T21:26:53+00:00"
first_seen_at: "2026-07-25T16:35:22.821563+00:00"
fetched_at: "2026-07-28T22:26:35.155530+00:00"
content_hash: "sha256:8fb627f722ee511300e95711b81b79cec06b89f368793a1557094a0087bcd244"
---

# Technical Documentation at Nova Credit

Technical Documentation


Engineering


Technology


Software Development


Documentation


# Technical Documentation at Nova Credit


[Nova Credit Engineering Blog](https://medium.com/@nova-credit?source=post_page---byline--1040e467f76c---------------------------------------)


4 min read


·


Sep 28, 2021


--


by[Sophie Cooper](https://www.linkedin.com/in/sophiecooper2/)


Press enter or click to view image in full size


Photo by[Glenn Carstens-Peters](https://unsplash.com/@glenncarstenspeters?utm_source=medium&utm_medium=referral) on[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)


At[Nova Credit](https://www.novacredit.com/) , we’ve created a set of norms and procedures to facilitate information sharing and to keep historical records of how and why technical decisions were made. **Technical Requirements Documents** , or TRDs, are the technical counterpart to product documentation and should cover aspects from database changes to security implications and a high-level task breakdown. As our engineering team has grown and matured, we’ve developed strategies regarding how we handle technical documentation, so here are some tips and tricks to make the most out of writing and sharing technical documentation.


### Use a template to ensure important implications aren’t overlooked.


Rather than relying on memory to ensure that you discuss any security implications or database migrations, having a template will guarantee that any TRD covers all of the bases. While each project might vary greatly, the important considerations to share with the team can be fairly constant. For example, all our TRDs have a section regarding security implications, and a space to go through all of the[STRIDE threats](https://www.ockam.io/learn/blog/introduction_to_STRIDE_security_model) . Additionally, we have sections to discuss current unknowns, rollout and rollback strategy, and goals and non-goals. There are countless TRD templates online that can be adopted or edited to fit the needs of your team or organization.


### Embrace the 30/60/90 framework


When presenting or sharing an in-progress document, it can be helpful to share where you are in the TRD process — 30, 60, or 90% done — in order to communicate what level of feedback you are looking for. Although this was a technique that was introduced to us for design critique, it applies well to technical review as well and can be a great tool for expectation setting — whether you are asking for a more high-level discussion or help deciding on a general approach to an issue, or just need more attention on nitty gritty details of your document. Some of our most technically interesting and spirited conversations at our weekly engineering team meeting have occurred when we’ve had a 30 or 60% review to discuss and debate different approaches to the project at hand. It can also be helpful when you’d like input earlier on in the process to ensure you don’t go too far down a non-feasible path. A more in-depth explanation of the framework can be found[here](https://medium.com/swlh/using-the-30-60-90-framework-for-design-critique-960b68026b6f) .


### Discuss alternatives that weren’t chosen


It’s important to justify why your approach or strategy is the best option for the technical problem or project at hand. While it might be easier or faster to write up a quick document with just one method or solution, making sure you have thought through all possible solutions and the pros and cons of each can go a long way in convincing yourself (and others) of the right approach.


Press enter or click to view image in full size


Photo by[Kaleidico](https://unsplash.com/@kaleidico?utm_source=medium&utm_medium=referral) on[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)


### Share widely and store in one place to consolidate information.


Once the technical documentation has been written, it’s important to find the right avenue to share what decisions were made with the relevant audience. You should ensure that teammates have ample opportunities to get familiar with your topic and document — whether it’s giving everyone five minutes to read your TRD before starting discussion, opening it up for comments and questions the week before, or presenting high level details to the team. We’re still trying to find our sweet spot with the right avenue to present TRDs as our teams and engineering organization grows, but the goal is to *ensure everyone has a high-level understanding of the important decisions being made and to give everyone the opportunity to ask questions.*


After presenting, ensure all technical docs are stored in a consistent place for historical records. We’ve experimented with both a shared drive and a Github repo — any method should work as long as it’s consistent and shared throughout the team. Keeping records of all TRDs can be great for onboarding new team members and reducing information silos.


A thorough and complete TRD is a great foundation for a project. It can also help you determine what else you need to get the project started — gantt chart, project-specific slack channel for collaboration, etc. Using a document template, getting incremental feedback along the way and sharing TRDs widely has helped us level up our technical documentation at Nova Credit and ensure that all team members are aware of current projects and why technical decisions were made.


*Nova Credit is hiring! Check out our*[careers page](https://www.novacredit.com/careers) ,[current job openings](https://jobs.lever.co/neednova) *, and*[engineering values](https://www.keyvalues.com/nova-credit) *.*
