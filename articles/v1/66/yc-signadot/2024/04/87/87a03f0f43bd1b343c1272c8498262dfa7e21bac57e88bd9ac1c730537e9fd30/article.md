---
schema_version: "1.0.0"
document_id: "87a03f0f43bd1b343c1272c8498262dfa7e21bac57e88bd9ac1c730537e9fd30"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/are-you-delivering-on-developer-experience/"
published_at: "2024-04-20T22:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T22:25:56.029782+00:00"
content_hash: "sha256:a45cfdeb7e2d39610564373c0453d3f93392c97327aaf6f77eb40cb9f13ced8f"
---

# Are you Delivering on Developer Experience?

Decades ago, “[The Mythical Man-Month: Essays on Software Engineering](https://www.amazon.com/Mythical-Man-Month-Software-Engineering-Anniversary/dp/0201835959) ” introduced managers to some of the costs and friction that software developers faced that weren’t correctly encapsulated by labor and time. These included communication and coordination time:


“Somewhere, right now, a software developer is pulling open a ticket from the project backlog, excited by the prospect of working on something new. As the developer begins reading through the description of the task, their laptop is suddenly flooded with alerts from the team’s production error-tracking system, disrupting the developer’s ability to focus. Eventually, returning to the task at hand, the developer studies the requirements described in the ticket. Unfortunately, the task lacks context and clarity, so the developer asks for help, which will take days to resolve. Meanwhile, the developer checks on a previous task, which has been stuck in the queue waiting for approval for several days. The tests and builds repeatedly flake out, halting the progress of reviewers each time they attempt to verify the changes. As the developer hops from task to task, hoping finally to immerse in some deep work, they realize that today’s *experience* isn’t as good as it should be to allow for their best work.”


If “The Mythical Man-Month” illustrated distractions that prevented developers from just working on the task at hand, then research into developer experience has shown that barriers exist to developers doing their best possible work given the time they have.


A[recent article in ACM Queue](https://queue.acm.org/detail.cfm?id=3639443) encapsulates problems that modern developers face beyond those previously described.


Systems can have attributes that interfere with the best possible work on software, specifically those that delay and interfere with feedback, interrupt flow and discourage connection and collaboration. **‍**


**How Problems in DevEx Hurt Developer Productivity**


---


When things are working well for a developer, it’s pretty easy to tell: features are delivered, emergent problems are discovered before they affect users, and the team frequently finds “quick fixes” and “low hanging fruit” to deliver more than the original scope in a similar timeframe. Success often echoes all the way from better reports on[met development targets to a better user experience driving more sales](https://www.signadot.com/blog/how-brex-transformed-developer-experience-and-slashed-infrastructure-costs-with-signadot/) .


But when things aren’t working well, the evidence is often much more equivocal. Missed deadlines happen, they’re not always evidence of a problem and may just be errors in original planning. Even feature regressions that make it all the way to production might be acceptable under agile methodologies. But when all these issues are taken together, it’s easy to tell from the sentiment of the team that something is rotten in the[state of DevEx](https://www.signadot.com/blog/7-reasons-developer-experience-is-a-strategic-priority/) .


In the ACM Queue article “DevEx In Action,” the authors categorize some of the proximate causes for a poor developer experience (DevEx):


### **1. Developers Rarely Get Into ‘Flow’**


I have a bag of almonds on my desk right now. They’re coated in a flavor dust described as “soy sauce wasabi,” and I absolutely love them. That explains why I have them but not why they’re on my desk in my office, especially since a short walk through my garden will take me to my lovely kitchen should I need a snack. Why the desk almonds? The reason is flow. When I’m working on something that’s interesting, stimulating and challenging I don’t want to have to stop every 90 minutes to get a small snack. I had noticed that the last two9 hours of morning and the early afternoon were both frequently interrupted not by a need for a break but by simple hunger.


With my desk almonds I can grab a quick handful and continue humming away until it’s time for a longer break to walk the dog, smell the flowers and fix a proper lunch.


I won’t belabor the criticality of flow: It can take hours to get back into this state once broken —even a one-minute conversation can break this state — and a well-focused developer can do in a few hours of “flow state” work that might take a week otherwise.


How is flow broken? One obvious answer is the repeated interruptions of a developer’s day to answer small questions from other teams. It’s expected that a small team would communicate to collaborate, but if a developer is answering questions from project managers, support specialists, and sales; even if these requests take up a tiny percentage of developer time, those brief questions spread across the day can be enough to ensure no developer ever has two uninterrupted hours.


To mention one enemy of flow that should be almost self evident: Coders need to be comfortable, and more important than an Aeron chair is an IDE the developer knows and loves. I once worked on a team that was, largely by de facto rather than organized decision, adopting AWS Lambda for many small compute tasks. Since no one had ever really made a decision to do this migration wholesale, and since the team wasn’t well versed on CI/CD integration with AWS, this resulted in much of the actual coding work happening in the web interface for AWS Lambda. It should go without saying but having to write code in a website that the dev has never used before isn’t a recipe for flow.


One consideration when considering flow is the pain of getting an idea from the dev’s IDE to a test environment. One interviewee quoted in “[An Actionable Framework for Understanding and Improving Developer Experience](https://www.michaelagreiler.com/wp-content/uploads/2021/12/Framework-for-Understanding-and-Improving.pdf) ” said:


“The very first thing that came to my mind was around the tooling or any friction around the tooling that either makes it really painless to go from I’m working on an idea to I’m testing that idea and production, the tooling that makes it painful to go from point A to point B”


We often discuss testing’s “[shift left](https://www.signadot.com/blog/why-testing-must-shift-left-for-microservices/) ” as if it were an abstract product buzzword or, worse, a rallying cry to make hard-working devs take on even more unwanted technical responsibilities. But really shift left testing (putting more testing tools in the hands of the developers who made the changes) is about creating a state of flow where developers can try new things, experiment and see how their changes interact with the whole system before another user or a deploy process has to be involved.


### **2. Without Connection and Collaboration, Creativity Suffers**


In the example above from “DevEx in Action,” the authors start with a developer who cannot clearly identify the requirements for their next feature. This is the classical example of poor collaboration in software. Without clear requirements the work can’t really start, and by going ahead, it’s likely the work will be in the wrong direction. We should think more deeply about what real collaboration can give us and how it’s hurt by siloing.


A secondary effect of the craze for large language models is the recognition of the value of creativity in high-quality engineering. It’s been true for decades that anyone with an accurate object model for a database can use a software tool to create an API for that database. But the real connection between data stores and business needs is more than mechanically publishing every row and column as an endpoint. Creativity also requires that developers and other stakeholders collaborate in meaningful ways.


On a project for a business-to-consumer service I once spent a quarter developing an automated phone system using an early version of Trello. I implemented queuing, holds, conference calling, etc. Months into the project I was at an all-team picnic and asked the table, “Do our customers ever call us?” It was then I discovered that inbound phone calls were our No. 1 source of revenue. Had I known that earlier, my whole project would have had a different scope. I had all the requirements for my project, but the basic business needs had never been clear to me, and I had wasted weeks on outbound calling tools that were extremely low priority.


At that picnic I almost laughed at how in the dark I was about the way this company really made money. With poor collaboration no developer can feel their work is truly valued and that they’re doing their best work.


### **3. Feedback is Low-Quality and Progress Suffers**


When we say “low-quality feedback,” we don’t mean that feedback that is unclear or unhelpful, rather this refers to finding issues that are communicated either too slowly or in a way that requires extra work in interpretation. Let’s start with feedback that comes at the wrong time.


A critical concept in modern developer experience is the “inner loop” of feedback on code changes. When a developer has a quick and familiar system to get feedback on their code, it encourages multiple cycles of testing and experimentation before code is deployed to a final test environment or production. The “outer loop” of feedback involves a more formal process of proposing tests, merging changes, running integration and then end-to-end tests. When problems are found on the outer loop, the result is larger, slower deployments with developers receiving feedback hours or days after they write code.


Outer loop testing can still be[testing that is automated](https://www.signadot.com/blog/why-shift-testing-left-part-2-qa-does-more-after-devs-run-tests/) and kicked off by the original developer, but another common issue with feedback that comes later in the release cycle is that it comes from human testers or others in the release process. This often results in feedback that is symptomatic rather than identifying root causes.


When feedback isn’t clear, it’s as bad or worse than unclear requirements: Developers can’t work quickly on problems they haven’t diagnosed, and they’ve often moved on to other projects in the time between deployment and finding an issue. This out-of-band nature of solving problems found in QA also creates pressure to minimize problems; Rather than going to work on a fix, there’s a natural tendency to argue that the problem is either minor or not caused by recent changes. Finally, having someone else inform you of an issue is naturally a break in flow as the developer has to change contexts.


## **No Single Failure Characterizes Poor DevEx**


To butcher the opening of[Anna Karenina](https://www.online-literature.com/tolstoy/anna_karenina/1/) : Good development days are all alike; every day of poor productivity is poor in its own way. It’s not possible to look for one obstacle to productivity and say: “That is the one cause of our poor developer experience.” You must examine multiple contributing factors. In general, the confluence of poor flow, barriers to collaboration and untimely feedback are three common barriers to a high-quality developer experience and developer productivity.


Lest you think that the complex causes of poor developer experience make measurement difficult, it’s actually quite a bit easier to see this problem than to cure it. It is quite possible to measure a failure of DevEx. After all, DevEx encompasses how developers[feel about, think about and value their work](https://ieeexplore.ieee.org/document/9785882) . By surveying and measuring the sentiment of developers, a general issue in developer experience is easy to see.


## **Conclusions: Taking Developer Experience Seriously**


Taking developer experience seriously requires an approach that acknowledges the complexity of software development as a human-centric activity. It’s not merely about selecting the right tools or enforcing processes, but about creating an environment where developers can thrive, innovate and deliver their best work. This involves addressing the key issues identified: ensuring developers can achieve and maintain flow, fostering effective collaboration and communication and providing timely, high-quality feedback.


In my follow-up to this piece, I’ll write about how to understand and improve your developer’s experience. To aid your shift left of developer testing and feedback, consider[Signadot](https://www.signadot.com/) , which lets developers run testing and experimentation in a cluster that contains all the required services and resources. By letting developers run integration tests much sooner than before, even very large teams can enable a single developer to get a fast inner loop of feedback on their changes.
