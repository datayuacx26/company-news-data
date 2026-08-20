---
schema_version: "1.0.0"
document_id: "2c03060c1c966173a65cb18eafa37d66ecd62355f45698545b34805b7af9a31f"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/devex-dialogues-iris-scholten"
published_at: "2025-02-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:d5ef58e8eafcb2c04e789aeeeb0658b5e55eafebfbbf8591c9012b5bcf72bb17"
---

# Dialogues in DevEx: A Conversation with Iris Scholten

​[Developer Experience](https://depot.dev/blog/devex-past-present-future) (DevEx) is at the heart of how software teams work and thrive. It encompasses everything from the tools developers use, to the processes and workflows they follow, to the culture of the teams they work within. And at its core, DevEx isn’t just about productivity. It’s about empowerment, satisfaction, and the ability to do meaningful work without unnecessary friction.


In this asynchronous interview series, we explore DevEx through the eyes of the people who live it every day. We talk to software professionals across roles and industries to uncover how they define DevEx, the challenges they’ve faced in creating good developer experiences for themselves and other developers, and the creative ways they’ve sought to improve the experience of developing software in today’s modern professional landscape.


Our fourth interview is with one of Depot’s own, Staff Software Engineer[Iris Scholten](https://www.linkedin.com/in/iris-scholten-4a5b8983/) . We hope you enjoy the conversation!


## Contents


- [About Iris](https://depot.dev/blog/devex-dialogues-iris-scholten#about-iris)
- [Onboarding to a technical product](https://depot.dev/blog/devex-dialogues-iris-scholten#technical-onboarding)
- [Impediments to getting shit done](https://depot.dev/blog/devex-dialogues-iris-scholten#productivity-impediments)
- [Improving DevEx at the team level](https://depot.dev/blog/devex-dialogues-iris-scholten#improving-devex)
- [Measuring the impact of DevEx initiatives](https://depot.dev/blog/devex-dialogues-iris-scholten#measuring-impact)
- [Developer tools Iris loves](https://depot.dev/blog/devex-dialogues-iris-scholten#developer-tools)


## The Interview


**Kristen** : I’ve been champing at the bit to do a **Dialogues in Developer Experience** interview with a Depot software engineer, Iris, so thank you for agreeing to have this conversation with me!


Before we fully dive in, can you tell the audience a little about yourself?


**Iris** : Sure! I’ve been a software engineer for more than a handful of years now, working primarily as a full-stack engineer. I started out as a front-end developer, and worked on data visualizations and various web app features. From there, I started working on features that took me deeper into the stack, and I’ve been a full-stack developer for the last few years, working on a variety of projects and constantly in a state of learning new things.


Which goes hand-in-hand with some of my personal hobbies. I’m a big fan of board games and spend a lot of time learning and playing new ones. Games in general strike my fancy, so I also play a lot of video games. For a more creative outlet, I enjoy the fiber crafts; crocheting is my go-to, but I plan to spend more time in the future learning and practicing knitting as well.


**Kristen** : I adore the term *fiber crafts* - there’s just something about it!


So, you and I share a recent experience: Onboarding to Depot while being relatively new to the **software build** space. We’ve both certainly run builds using various tooling, and we’ve both suffered the frustration of waiting for slow builds to complete, but we’ve never been members of a DevOps or Platform team and generally “responsible” for build performance.


Can you talk a little about the experience of onboarding to a very technical product that sits in a domain you’re relatively new to?


**Iris** : Depot has definitely been a very new product space for me – software builds are something I’ve not touched much in my previous roles. I’ve modified a Dockerfile here, or Docker compose there, and maybe tweaked or set up a Github Action, but mostly I’ve followed patterns set up by people who actually knew what they were doing. So, a lot of this has been a relatively new space for me to work, which can definitely be intimidating.


That said, throughout my career, I’ve often been in the position of working on something that I’m not really familiar with, and while it can be nerve-wracking, it’s always been a really good learning experience, providing me with valuable growth opportunities. And that’s been no different here as I’ve been onboarding at Depot. Over the last month, I’ve been functionally working on concepts that are very familiar to me – like contributing features to the web app – and have been able to ease into the software build products and capabilities we support.


**Kristen** : That certainly resonates. I hesitate to admit this, but when I started, I put off my “Get Depot up and running on a local project” task for about a week longer than was probably acceptable, because I had a little bit of imposter syndrome around it – I was definitely plagued with that stereotypical, new-to-the-job insecurity: *What if I can’t get it running and have to ask stupid questions to get it to work?*


**Iris** : I put it off, too! I recently made moves to go through the set up, though, and was pretty surprised and delighted by how seamless the whole experience was. I just signed up, followed the org- and project-creation instructions, and it “just worked.” I was like, “Oh – is that it?”


**Kristen** : I had the same experience! I thought I was missing something, at first, and then I went into the web app and was like, “Oh, wow - there’s my project building!” Pretty cool.


So, part of the reason that I’ve landed in the Developer Experience domain is that I sincerely care about the experience that software practitioners have when doing their work. And something I’ve observed in all the papers I’ve read and the conference talks I’ve watched and the conversations I’ve had is that at the end of the day, a great developer experience is one that enables developers to *get shit done* .


I’d love to know: In your career as a full-stack developer, what kinds of things have consistently impeded your ability to *get shit done* ?


**Iris** : When we’re talking about things that slow down actually building the thing we’re trying to build, the following come to mind:


- Lack of clarity around what to build. Sometimes, requirements are vague, and other times, there are multiple opinions pulling you in multiple directions, and you end up going around in circles trying to define and decide on what to actually build.
- Approval bottlenecks. Sometimes, we simply have to get approval before we can move forward with building. But having to go through many layers of approvals and formal processes to get the thumbs up to build something in a particular way can very much impede progress. There’s a necessary balance here, especially as a team or company grows and more coordination between stakeholders is required.
- Interruptions and a lack of focus time. More coordination often means more communication, and with this, there can be a lot of disruptions between meetings, urgent requests coming through messaging systems, noisy on-call notifications (especially unactionable pages).
- A slow development environment. I’ve experienced development workflows where just the time it takes to start up all the services I need to run locally takes long enough that I end up scrolling on my phone while waiting. And sometimes this increases the time it takes to validate a change, especially if hot module reloading isn’t set up.
- Slow build and deployment pipelines. In some cases, moving code through the pipeline into production is a quick, automated process. But not always. When the build and test run take 40 minutes in CI, for example, implementing pull request feedback can add a lot more time to the delivery process, because every additional change requires waiting for everything to go green again. And this is a problem when it makes you question whether to implement a minor change, just so that you don’t have to wait another 40 minutes to move that item through the pipeline.
- Frequent and recurrent regressions. Sometimes, and especially in a particularly complicated and complex part of a codebase that doesn’t have great test coverage, the same code breaks over and over again with even just small changes.


**Kristen** : Okay, just reading this list has made me shiver with bad memories 😆 I haven’t had my hands in the code in any serious way for a few years now, but I *remember* these things. Vague specifications. Complex-yet-brittle codebases. I was just learning how to code when hot reloading became a thing, and people were *so excited about it* , and I just couldn't understand their excitement because I hadn't lived through The Before Times 😆 But then I ended up on teams that didn’t use hot reloading, and that always turned into a, “Hey, anybody mind if I add…” conversation!


All joking aside, though, the fact that these things continue to be frustrations and impediments to *getting shit done* really speaks to the difficulty in solving them. Have you ever gotten so frustrated with one (or multiple) of those you mentioned that you were moved to try to improve them?


**Iris** : Definitely. In a lot of my past jobs, there has come a time when on-call notifications have become particularly noisy, which again, can really interrupt focus time. Auditing the monitors that we used to try to reduce noise and ensure that we were only getting paged for page worthy events usually helped. In this audit, we would ask: Is this something I should stop whatever I’m doing to mitigate – no matter if it’s in the middle of the night or the middle of the day? Is this something I want to be notified about, but isn’t really urgent? Is this something that my team can take actions to fix? For those things that were both urgent *and* my team in particular had tools to fix or mitigate the issue, we kept notifications on. And if they were the noisy ones, we would prioritize putting a fix in place for such things. If we weren’t the best team to address the incident, we’d shift those alerts to go to the correct team. For non-urgent alerts, we opted to send those to a Slack channel. And in some cases, we just removed the monitor altogether!


**Kristen** : Okay, been there, done that – who gets their monitoring and alerting “right” on the first pass? Periodic audits on our monitoring systems are so valuable, and can really improve DevEx for the teams getting those alerts.


**Iris** : Exactly.


Another example of making a positive change when I’ve seen the opportunity is that I've had my fair share of times where it seems like the same bug – or different versions of the same bug – keeps popping up, and fixing it in one part of the codebase just breaks something else in the codebase. To remedy this, I’ve started adding more “happy path” end-to-end tests, and more granular and detailed unit tests around problematic areas of the codebase. This lets us feel more confident when making changes, and provides more trust around deployments.


**Kristen** : Again, I’m shivering with the memory of a really bad experience here. The first time I managed an engineering team, I inherited a codebase that not only had variable names like *ttc* (time to close) and *ct* (cycle time), and *there was no test coverage* . Tracking down and fixing bugs was a headache that eventually prevented us from shipping. Luckily, my team and I had enough agency that we could take a couple weeks to re-write the code, and we were able to say bye-bye to that ugly, untested codebase. And we added test-driven development to our process so that we didn’t create another finicky, untested codebase in its stead.


**Iris** : I’m realizing that a lot of the challenges I identified tend to be more process-oriented. That's kind of the thing with Developer Experience. It’s not guaranteed to be good, and you have to constantly identify areas of improvement, and constantly make an effort to ensure it’s getting better. And a lot of the time, that means prioritizing those “thousand little cuts” that build up over time, and really putting in the time to fix things that are not directly going into the product, but that provide long term impact towards a team’s ability to deliver.


**Kristen** : I couldn’t agree with you more, especially around that piece of understanding if things are getting better with our changes. With any modification we make to tooling, process, environment – whatever – DevEx initiatives must be accompanied by methods for measuring their impact. And I think this is where most teams are not very well-equipped to succeed – because measurement can be *hard* . Like, part of what folks pursuing PhDs in experimental fields are learning is how to create valid, reliable, ethical measures so that when they ask a research question, they can be as confident as possible that they’re answering it with data that was properly collected, analyzed, and interpreted. It takes years of study and practice and apprenticeship to gain this knowledge, and yet in business and development contexts, we ask people daily to provide data to “prove” or “show” success, and yet so many of those folks don't know how to validly, reliably measure those things. And that’s not their fault – they’ve never been taught! Valid, reliable measurement is just not something we emphasize in business contexts, in my experience.


This has made me think that products themselves that make claims about improving *literally anything* should make measuring whether the thing actually improved easy and transparent for users and customers. I think a lot of products probably don’t do this because it’s *hard* , but also because they’d probably expose that their products don’t do what they claim they do.


How have you approached measuring whether DevEx changes you’ve made have been successful or not?


**Iris** : This can definitely be easier for some changes than others. Sometimes the tools you use can help, but like you said, others require that you keep track yourself. A lot of times, problems get brought up and prioritized based on the *perceived* disruptiveness of a particular issue, which can vary from person to person and team to team.


**Kristen** : Don’t get me started on the inherent challenges of measuring *perceptions* , *beliefs* , *attitudes* , et cetera!


**Iris** : Exactly! I really think the most important piece here is checking to see if the change you made moved the needle in a positive direction, and not just assuming that the effect was positive. On past teams, once we identified a change that was worth implementing, we’d take note of the current state of the world for this impediment – both in terms of how it felt for members of the team, as well as measuring occurrences. For something like on-call rotations, we’d count the number of pages (or clusters of pages), as well as the number of pages that required *no* action; this was frequently a self-reporting task for people on-call. For something like recurring regressions, an error tracking tool like Sentry can show how often a particular error or regression occurred, or you can look at your issue tracker and see how often you’re reopening issues. It can be easier to compare before-and-after states with speed-based problems like slow builds and CI runs, depending on what systems and tooling you use.


**Kristen** : Let me take that reference to tooling as a segue into my final question, Iris – what’s a dev tool you’ve started using in the last year or so that you absolutely love?


**Iris** : I’ve recently started using[Warp](https://www.warp.dev/) as my terminal. It feels modern, and provides some quality-of-life features like auto-suggestions, easy history searching, *and* you can move the cursor around via mouse click.


**Kristen** : *You can* *move the cursor around via mouse click?* Please excuse me while I go download Warp.


Thanks for having this conversation with me, Iris! You can go back to work now, because we know your builds finished, like, hours ago 😆


## Related posts


- [Developer Experience: Past, Present, & Future](https://depot.dev/blog/devex-past-present-future)
- [Dialogues in DevEx: A conversation with Nic Pegg](https://depot.dev/blog/devex-dialogues-nic-pegg)
- [Dialogues in DevEx: A conversation with Annabelle Thomas Taylor](https://depot.dev/blog/devex-dialogues-annabelle-thomas-taylor)


Kristen Foster-Marks


Head of Developer Experience at Depot
