---
schema_version: "1.0.0"
document_id: "7cc67ec609071459bb90a385193f68980b4d4ba3769d10fc09a4eb02d6f752c6"
company_key: "yc-slash"
company: "Slash"
source_id: "yc-slash-news-import-9ec40849fc98"
canonical_url: "https://www.slash.com/engineering/blog/new-mobile-app"
published_at: "2026-04-15T19:00:00+00:00"
first_seen_at: "2026-07-24T01:09:43.930358+00:00"
fetched_at: "2026-07-28T22:15:52.361502+00:00"
content_hash: "sha256:7174553346d43a8d3d2610be162858366a22824aa4e1eed0249af4ef8759c7de"
---

# One Engineer, Three Months, Zero Compromises

Last week we announced the launch of a brand new Slash mobile app. For a finance app where users trust us with their money, instilling confidence in the product is key - this means not only providing the core features they need to run their day-to-day business, but making sure that every interaction feels smooth and polished. Our old app was functional - it had a pretty good set of features packed in - but it felt like a web app on a phone. The UX was lacking, the design system didn’t match our current product, and there was enough tech debt in the project that we felt a ground-up rewrite was the best way to proceed.


We like to aim high at Slash so we set an aggressive target for the project - we kicked it off after the holiday break and aimed to launch at the end of Q1. This was done under the assumption that we would *probably* need to cut scope or push the deadline. The original plan was templated after last year’s[Project Facelift](https://www.slash.com/engineering/blog/facelift) , in which one person established the architecture and then we spread the work across several engineers to get it across the finish line (the Facelift took about 5 months to complete, culminating in a team-wide hack week).


As someone who’s spent almost a decade shipping UI to customers, I’d like to think I have a solid calibration for how long it takes to deliver a good product. Before this year, I would have estimated about 6 months for a small team to build and publish this app; we likely would have had to cut corners on polish or descope some features to hit deadline. In reality, it took one engineer less than three months.


## The Scope


Building a whole banking app is no small task. Here’s a non-exhaustive list of what we needed:


- A two-factor login flow with biometric re-auth (sign in with Face ID after the first time)
- Credit card management - set custom spending limits, create virtual cards, order and activate physical cards¹
- Rich transaction details with the ability to upload receipts
- Dynamic filtering UI for cards and transactions
- A multi-step wizard for managing contacts and sending payments to them
- Adding funds from a connected bank account, wire transfer, or crypto wallet
- Interactive graphs for each banking product⁴
- Dedicated homepages for employee and admin users
- Push notifications with deep linking
- The usual bells and whistles for a production app - error monitoring, a force update mechanism, etc.


So how did we ship all of this, on the timetable we planned, without cutting corners? *(Spoiler: it involves AI)*


## The Altitude Thesis


If you work in software or you’ve spent any time on the internet recently, you’ve likely been exposed to a *lot* of discourse on how generative AI is transforming the landscape of software development. We’ve gotten to the point where you can build entire applications with Claude Code, and I feel like the conversation about this often falls into one of two camps: sensationalizing how easy it is to go from idea to execution, or dismissing these “vibe coding” workflows because the output doesn’t stand up to real engineering scrutiny.


There is a concern among some - not unwarranted - that handing off meaningful work to LLMs means you have to compromise on quality or maintainability. I would argue that, used in the right workflow, these tools can now enable you to work with unprecedented velocity *without* compromising on quality. Not only that, building this app has shown me that you can *raise* the standard for quality of execution if you stay vigilant in exercising good engineering judgment.


I've found that the key to leveraging AI as a force multiplier is developing an intuition for the right altitude to work at - knowing when to get granular and when to step back and orchestrate. Of course, this is easier to articulate in hindsight than it was to discover in practice.


## Evolution of a Workflow (Or: How I Learned To Stop Worrying And Love The Prompt)


Historically, I’ve been a pretty big AI skeptic. Prior to this year I used it for tab-complete, research and ideation, and for generating reference examples, but I certainly didn’t trust it to build entire features. Most of my code pre-2026 was still written by hand - I saw these tools as a nice boost but not a game changer.


It was based largely on testimonials from my Slash coworkers that I started to get out of my comfort zone. Kevin returned to the office praising the combo of Cursor plan mode and Opus 4.5 as a huge level-up for his projects over the holiday break, and the engineering team as a group came to a consensus quickly that this felt like an inflection point ([an opinion in which we might not be alone](https://x.com/simonw/status/2007904766756880848) ) for the capabilities of these models.


During the first couple of weeks building the mobile app, I developed a better and better intuition for what Opus really excelled at, and began to trust it more with complex tasks. Most tasks that involve summarizing lots of information, building components from an established pattern, or making bulk changes are now near-trivial.


I gradually realized two things: I was spending a hefty chunk of time waiting for prompts to complete, and offloading all of this grunt work meant that I had far more mental bandwidth to focus on things that actively leveraged my engineering experience. Things like knowing when a generic solution won't suffice, how to evaluate the AI output for tradeoffs it won't consider on its own, and how to translate a static design spec into interactions that actually feel good to use. Where before I would consider a decision, make it, and then execute on it; soon I was spending most of my time just making decisions.


The insane speed with which an agent can translate your choices into code only increases the leverage of those decisions - this is how the concept of altitude came into focus for me. For key pieces that will be reused in many places, it’s important to spend time on the ground - specify in high detail; get the API right. The cost of designing the wrong abstraction or choosing the wrong core library is high when those decisions propagate to everything you build on top of them. When you already have the building blocks you need, you can afford to move up to a birds-eye view.


Approximately one day of my Cursor history - frequently jumping from 0 to 30,000 feet and back


### A Flight Plan


My development process for each new feature eventually settled into a predictable pattern. Each of these steps represents a distinct phase with one or more Cursor chat sessions:


1. **Background research** - Attempt to gather all of the information I need up front - best practices for patterns that aren’t in the codebase yet, options for third-party integrations if necessary, components that will need to be built, etc. This was critical for forcing the agent (and myself) to build broad context of the feature and be explicit about any key tradeoffs or architecture decisions. *Altitude: high.*
2. **Scoping and planning** - Define the constraints for the feature and break it down into an actual plan. Some tasks are substantial enough (like the send money feature, for example) that this step involves defining multiple sub-plans and defining context boundaries for a separate agent to tackle each one. *Altitude: medium. Attention to detail goes a long way here.*
3. **Bulk implementation** - This is where most of the code gets written. At this point, the plan should be clearly defined enough that the agent’s output is fairly predictable. For features that require building new reusable components, I would usually break this step into two stages: build just the new components and tinker with them in some sort of sandbox view, then build the actual feature once all of the building blocks are available. *Altitude: high. Most of the detail should already be included in the plan.*
4. **Testing and reshaping** - This step can sometimes bleed into the previous and next ones, but the goal here is to sculpt the model’s output back toward your original vision of the feature now that you’ve generated plenty of material to work with. I’ll usually click through all of the possible interactions in the first-draft UI, put together a large batch of changes to feed back into the prompt, and repeat this process until I get to the core behavior that I want. *Altitude: medium-low.*
5. **Tweaks, polish, bugfixes** - Making the rest of the pipeline as efficient as possible buys us headroom to really dig in here, rather than just adding a few finishing touches. This step is relatively low on lines of code but heavy on decision-making, as the finer details can make the difference between functional and delightful UX. I spent a lot of time tinkering with animations and found that Claude was pretty adept at translating detailed descriptions of timing and layering into reasonable approximations of the behavior I wanted. *Altitude: low.*


Small touches like these compound animations help communicate polish. Polish → trust → user loyalty.


### Anatomy of a Feature


To make this more concrete: the send money feature - the most complex in the app - involved a dozen sub-phases, required building new reusable components for contact management and payment details, and went through several rounds of reshaping before the flow felt right. This is roughly what that looked like day by day:


- **Day 0:** This was actually a Sunday - I just got the ball rolling on foundational research while I was doing chores. I had Claude perform an investigation of how the backend API was used in the web app and the old mobile app, map out all of the user paths and edge cases, and write up a document with all of these findings. This was a banner day for working at a high altitude; even without dedicating much attention to it, I was able to establish enough broad context to dive right into planning the following day.
- **Day 1:** I laid out a high-level plan for the whole feature with sub-phases and deliverables - Phase 1 was the scaffolding; Phase 2 enabled selecting a recipient; by the end of Phase 4 we would have the ability to send a basic payment end-to-end; and so on. I created detailed plans for the first two phases and implemented the first one, which laid out the scaffolding for the multi-step wizard.
- **Days 2-3:** I built the basic recipient UI specified in phase 2, and planning the remaining phases required for the first end-to-end demo. I spent most of my time on two things in particular: iterating on the recipient selection interaction, and fleshing out detailed requirements for submitting a payment - relatively low altitude overall.
- **Day 4:** This was the day before an all-hands demo and it produced a massive amount of code. Most of Claude’s time was spent implementing the plans from the day before; most of my time was spent designing and tweaking visual elements like the slide-to-confirm control at the end of the flow (pictured above) and making sense of the many nested forms that make up the contact management flow.
- **Days 5-6:** Picking up the pieces from the breakneck pace of the previous day - I took the opportunity to do some cleanup and refactoring while I worked on hooking up the remaining payment methods (the first iteration only included ACH transfers) and planned out the remaining phases. The contact management forms, which contain some particularly gnarly native UI involving focus management and keyboard tracking *inside of modal containers* , led me to rework a whole group of reusable components to make the pattern easier to implement. This was another case where leaning on Claude to inspect the codebase and summarize all of the different things we were doing with bottom sheet components was a huge speed boost.
- **Days 7-9:** I did a deep dive into international wire payments, which was almost a whole separate feature unto itself because of having to deal with currency conversion. Prior to this I had already created a plan covering the core logic and API requests, but needed to iterate with our design contractor to settle on an international currency UI that felt intuitive.
- **Day 10 and beyond:** The long tail of bugfixes and incremental improvements. By this point the feature was mostly done, but it had so much surface area that small items continued to shake out from testing for a while.


### Becoming a Factory


Just codifying this process and using Claude to speed up each step was already a huge productivity boost. However, when I realized that I could assembly-line these stages to develop multiple features in parallel, I unlocked a whole new gear. In fact, on every work day I listed above, at least 60% of my Cursor conversations were actually dedicated to features *other than* send money.


By week 6 of the project, this was a more complete picture of a typical day:


- Kick off research prompts for check deposit upload and transaction filtering UI - these will be the next major features to tackle.
- Write detailed plans for phases 3-7 of the send money flow - these have tangible milestones already defined but need to be specified in detail.
- Implement recipient selection (phase 2 of send money), and the UI for ordering physical cards - independent features which already had detailed plans.
- Build a new radial chart component for the treasury tab - not previously planned but simple enough to freestyle.
- Polish the virtual card creation flow.
- Fix assorted bugs on the home page and transactions page.


After a day or two, each of these features would advance to the next step of the process, and I would start researching the next items in the backlog. By the time I had fully leaned into this parallel process I was conducting upwards of 20 conversations per day across 5 or more distinct features at a time.


The assembly line at work.


## Going Solo


I mentioned earlier that the original plan was to start onboarding other engineers to the project once enough conventions had been established. By late February - around the time we planned to do this - I had started to question whether spreading the work across more developers would be worth it.


For one, onboarding other contributors to a project midway through takes time, and might pull them from other important work with a lower context-switching cost. But I was also realizing that pulling in more people might not even speed up *this* project. After all, there are only so many things that can be built in parallel before you run into conflicts, like reimplementing the same primitives or refactoring common logic to fit the needs of new features. The feature velocity enabled by the aforementioned assembly-line process felt like it was already on the level of what a small team could do. Furthermore, continuing to work solo would make it easier to maintain a unified vision.


Of course, keeping a team of one isn’t necessarily without tradeoffs. That bus factor leaves little margin for error (I was actually terribly ill for almost a week in March) and it raises questions about the scalability of the project long term. When mobile becomes a larger focus for new Slash products, do I continue to operate as an entire mobile team, or will we run into the limitations of that structure?


My mobile development battlestation at Slash HQ. Each phone was usually dedicated to testing a different feature, often configured to talk to separate backends with seeded data.


## The Fine Print


It’s also reasonable to wonder what code quality looks like at this feature velocity. A point I often hear reiterated is that the implementation speed of AI-generated code is heavily offset by the time spent cleaning up and refactoring that code later. It is true that at various points while building this app I accumulated plenty of cruft. I would counter, however, that this isn’t as big of a deal as it might seem. For one, I’ve seen plenty of hand-written codebases with a comparable amount of copy-pasting, inconsistent patterns, and redundant components. Assuming you exercise good judgment in directing it, Claude is hardly going to produce worse code than a typical engineer. More importantly, addressing that tech debt is now easier than ever. It is incredibly easy to perform refactors when AI - famously good at pattern matching - can answer broad questions about your code and make bulk changes in a fraction of the time. And you don’t have to dedicate entire chunks of time to refactoring - I found it pretty seamless to weave targeted refactors into my workflow without halting forward progress.


As quickly as I was able to build, there was still often a bottleneck in testing these features because it takes a discerning human to judge whether something *feels* smooth and easy to use. I don’t think the “feel” element is going to be easy to replace any time soon, but I do think it’s very possible to streamline the mechanical testing of end-to-end flows. This was something I chose not to focus on (instead leaning on manual testing and dogfooding), but in retrospect I do think I could have gotten good returns on investing time into automated end-to-end testing.


There are days I would get lazy about writing prompts with clear specifications, and the results reflected this immediately - sometimes I would scrap entire features and start again because I realized that my initial planning prompts were sloppy and lacking clarity.


I also won’t pretend that the path taken to deliver this app so quickly is a one-size-fits-all approach. I had the unique advantage of being able to pattern many of these features against our existing web product; this living documentation and an already battle-tested backend API were a huge reason that I was able to focus most of my energy on just building great UI.


## Looking Forward


For me, the last three months have redefined what it means to be an engineer. I can tackle more work than ever before, but it’s been more than just a productivity increase - it was a fundamental shift in how I spent my time. Looking back at my Cursor chat sessions over the course of this project, a few things jumped out:


1. I went from 3-5 conversations per day in January to as many as 49 in a single day in March. I don’t think this is a matter of working 10x harder - I think this is evidence of a strong process enabling me to keep many more things in flight at once.
2. The implementation phase for any given feature was usually between 1 and 3 days, consistently, regardless of feature size. This lends credence to the idea that AI makes the physical building time essentially constant, shifting the bottleneck of engineering time almost entirely to decision making.
3. Most features had a 1-2 week “tail” after the core implementation work was done, during which I continued to fix bugs and iterate on UX based on feedback. This was directly enabled by the assembly line process - I was able to continue giving every feature personalized attention without blocking other tasks.


A breakdown of the timeline for each feature. Note how consistently short the core implementation phases are.


I can’t see myself going back to writing code the way I did before, and that raises all sorts of questions. What happens when this kind of velocity becomes the baseline - what does it mean for project scoping and team structure in the future? Is it even feasible to work at the pace demonstrated by this project on a timetable longer than three months? It’s still very early but I do think we are entering an era where a single engineer with the right approach and a focused area of ownership can accomplish what would have previously required an entire team.


I’m not as sure, however, that this kind of leap can take place in isolation. The environment of the Slash office played a critical role in my ability to carry this through - not only was my workflow informed by the collective learnings of the team, the accountability and positive feedback created by our weekly demo culture helped sustain my energy throughout the process. Our internal background agent, Atlas, was also key for implementing backend changes to support new mobile features without interrupting my client-side workflow.


I can’t say exactly how the process of building software will continue to evolve as the available tools become even more advanced, but I feel confident that knowing *what* to build and caring who you build it for is only going to keep growing in importance. Ask me again in three months.


## Read more from us


### [Building Card Transaction Processing at Slash](https://www.slash.com/engineering/blog/card-transaction-processing)


≈ 12 minutes read


### [Building Banking-Grade Stablecoin Rails at Slash](https://www.slash.com/engineering/blog/banking-grade-stablecoin-rails)


≈ 19 minutes read


### [My Internship with Slash](https://www.slash.com/engineering/blog/intern-blog)


≈ 13 minutes read


### [The Facelift](https://www.slash.com/engineering/blog/facelift)


≈ 15 minutes read


### [Winning & Culture](https://www.slash.com/engineering/blog/winning-and-culture)


≈ 4 minutes read


### [Scaling 1M lines of TypeScript: Registries](https://www.slash.com/engineering/blog/scaling-1m-lines-of-typescript-registries)


≈ 5 minutes read


### [Health Checks](https://www.slash.com/engineering/blog/health-checks)


≈ 7 minutes read


### [Type challenge: Datadog Logs Preview](https://www.slash.com/engineering/puzzles/datadog-preview-type-challenge)


≈ 2 minutes read
