---
schema_version: "1.0.0"
document_id: "4263c1daf4fb3165db93be9e1efba41db606065f845ef5c6aaaf9cfeb269d0d7"
company_key: "yc-runway"
company: "Runway"
source_id: "yc-runway-news-import-fe61d44f24da"
canonical_url: "https://www.runway.team/blog/why-ci-cd-wont-save-your-team"
published_at: null
first_seen_at: "2026-07-22T12:25:56.018278+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:0f603fad8137309aa1ef4a01193ff88876761694e3101dbc23377784e739e44e"
---

# Why CI/CD won’t save your mobile team

When teams think about improving their mobile practice, attention most often turns to tooling, and from there quickly focuses in on CI/CD. Not only do fast builds keep engineers happy and release trains flowing, but CI/CD is also a great spot to pile on various other automations for the wider mobile dev lifecycle… or so the thinking goes. The idea that your mobile org will just hum along if you spin up a fancy bit of tooling and add some bells and whistles is definitely an alluring one, but the reality is that CI/CD is just one piece of a complex puzzle. In jumping at build pipelines and automation as a be-all and end-all solution for streamlining a mobile org and achieving[Mobile DevOps](https://www.runway.team/categories/mobile-devops) success, teams often get caught up on that one piece of the puzzle in a way that clouds the bigger, more human picture.


Automation as a “north star” used to make more sense. Not that long ago, mobile CI/CD and other tooling was much less mature, so teams needed to devote more of their own bandwidth to the automation piece. Now, that stuff is lower-hanging fruit; CI/CD is commodified and you can get most of the way there with a range of solid, out-of-the-box options. And yet, the (now disproportionate) focus on automation remains, and it can cause unintended side effects and keep teams from engaging with more complex and more foundational work. Solutions to the tough stuff — the human factor, the way team members communicate, build together, collaborate — can’t be automated away.


It can be difficult to wrap your head around this tooling vs humans question, especially because it’s been gradually evolving around us all. Pre-Runway, my co-founders and I had to take a huge step back from our mobile day-to-days to fully grasp the bigger picture — and, foreshadowing a Runway theme, we looked to the world of aviation for help. Surprising as it may seem, and while the scale and stakes might differ, it turns out a lot can be learned from aerospace about the role of automation and its impact on the humans involved: from its initial rise and specific applications, to the friction and risks its ever-growing importance created, to the kinds of solutions and measures put in place to ensure its safe and effective use.


So let’s explore a new perspective on the role of automation — and, by proxy, tooling like CI/CD — in mobile product engineering, with parallels from the world of aviation as a guide. We’ll cover some potential pitfalls and areas where automation can have unintended side effects, as well as ideas and best practices for getting the human-machine balance right and not losing sight of the big picture.


## More automation means more obfuscation


The more you automate, the more “black box” effect you need to deal with. This is especially true as you build automations on top of other automations, creating an intricate web of dependencies that exist out of plain sight. For the folks directly involved in building and maintaining these kinds of systems — say, a platform team owning CI/CD, or your resident tooling whiz teammate — this might not be a huge problem. Their familiarity and dedicated attention makes digging down and getting in the weeds to diagnose an issue or add functionality relatively easy. But for everyone else who is affected by these automations in their day to day, whether directly or indirectly, the black box effect can make life difficult in some significant ways.


Consider autopilot. In the early days of the aerospace industry’s digitization, autopilot was a straightforward system: there were a few buttons you’d use to punch in an altitude to hold, say, and the plane would keep itself there in a pretty human-understandable way. Over time, autopilot has expanded into much broader automated flight control systems, and it exerts control in a much more sophisticated way. Crucially, these automated systems are now involved in every phase of flight and even when pilots are hand-flying.


From a pilot’s perspective, the steady increase in responsibilities owned by autopilot has made their job easier and safer, but at the cost of some black box effect. There’s only so much the end user can understand about these complex systems and, in certain situations, that leads to friction and confusion — occasionally with serious consequences. The obfuscation problem is so real that it has led to fundamental — and hotly debated — differences in flight systems design philosophies[between major aircraft manufacturers](https://simpleflying.com/airbus-boeing-aircraft-technical-differences/#flight-control-laws) . On one side of that design debate, aircraft systems work “under the hood” to constrain the effects of control inputs by pilots to a safe envelope, whereas on the other side of the debate the manual inputs are somewhat more direct (but at the expense of some automated safeguards).


Back in our humble mobile context, growing reliance on CI/CD throughout the dev lifecycle creates a similar black box effect that teams need to guard against. As you cram more automation and dependencies into your build pipeline, its importance and reach within the org grows, affecting more team members and more of their day-to-day while simultaneously becoming less well understood by all but a select few experts on the team. CI/CD ends up doing a lot, with a lot of side effects, and in an opaque way that causes confusion and wastes time — especially when things go wrong. Flaky failed builds can lead engineers down rabbit holes, chasing red herrings; symbolication files gone missing can leave PMs scratching their heads as they try to triage new crashes; and distributed builds landing in the wrong place (or never landing at all) can mean work you need eyes on never gets looked at.


## More automation also means *more* human-machine interaction


If you extend the black box metaphor one notch more literally, consider how the volume of the box — the hidden-away, internal part — grows as your team adds automation to your mobile workflows (that’s the obfuscation problem we were just talking about). But as the box’s volume increases, so too does its *surface area* : that is, the number of touchpoints between machine and human worlds also grows. Imagine a CI/CD pipeline that automatically churns out more builds, in different flavors and to different destinations: there are humans on the receiving end that need to understand which build is which, what to do with any given build next, and when exactly.


The idea that more automation leads to more human touchpoints can feel counterintuitive, but it’s important to keep it in mind. Automation might reduce overall manual work, but it also increases the need for oversight and carefully-timed inputs along the way: missed handoffs and dropped balls in the gaps between humans and machines can waste time and lead to critical errors. Oversight and inputs entail human touchpoints in and of themselves, but human involvement is also required for thoughtful design and upkeep of all your human-machine interfaces.


Sticking with the autopilot analogy, increasingly sophisticated flight control systems take a lot of work off pilots’ plates, but they also multiply the ways pilots need to monitor and interact with those automated systems, and raise the bar for skills and diligence needed to do so. It’s telling that the super thorough checklists that are ever-present in aviation don’t just cover manual tasks still required of pilots, but they also govern exactly how pilots should be handling and monitoring automated systems (more on checklists later!). And, design of the very literal human-machine interfaces in aircraft cockpits has become an entire discipline unto itself, with[critical attention](https://www.sciencedirect.com/science/article/abs/pii/S1290095899800367) paid to even the tiniest button or minor flow for inputs to a flight management system.


In the CI/CD case, teams need to be thoughtful about all the new human-machine interfaces that an automation-rich pipeline creates. The most obvious interface considerations have to do with the artifacts generated: where they all land, who’s on the receiving end, and what those recipients will want to do with them next. For real continuous delivery success, you need much more than a pipeline which can seamlessly churn out and upload builds — you need an intuitive way for stakeholders to understand what builds they’re looking at, a reliable and easy way for those folks to get a particular build onto their devices, and a frictionless mechanism for feeding input back into the system (to sign off on a build, say) to advance the process. The rise of automated testing has created another interesting category of human-machine interface considerations, whereby test results must be surfaced coherently for the team and made actionable for triaging. Even seemingly trivial interactions like Slack notifications triggered by automations deserve thought. It’s easy to throw a bunch of alerts into CI/CD, but hard to do it in a way that actually serves the team and doesn’t turn into noise that gets ignored.


## And, more automation means less human-to-human communication


Quite simply, as tasks are offloaded to automation, there’s less for the team to communicate about, human to human. This isn’t inherently a bad thing — it can actually make space for more meaningful conversations in other areas, and unnecessary chatter for its own sake isn’t helpful either. But, each human touchpoint made obsolete does also represent an opportunity lost. Even in mundane exchanges there’s the possibility of important knowledge transfer, some nuance detected, some unusual pattern flagged — a bit like[rubberducking](https://www.softwaretester.blog/the-power-of-rubberducking) . In addition to the more direct impacts of this, there can also be less obvious ripple effects that change a team’s culture and way of collaborating.


In aviation, there’s always been an emphasis on both quality and quantity of communication. But especially with the rise of automation, we’ve seen a specific reckoning with its side effects in the form of a shift towards very specific frameworks for communication and bias towards overcommunication, even when automation is driving a particular workflow. Concrete examples exist in communication between air traffic control and pilots in their airspace, and in the cockpit between pilots working a flight together.


In the mobile context, the impact of automation on human communication is perhaps subtler, and the ways to address it are less intense (your team doesn’t need to sit around the proverbial CI/CD campfire narrating everything it’s doing to each other). But, you should take care to leave room for human touchpoints around the edges of automation, and create some formal framework for syncs and check-ins along the way.


## Humans are always in the picture — and they become the hard part


The discussion so far makes it pretty clear that no matter how much automation your team stands up, humans are still in the picture. Not only that, but the presence of more automation actually complicates things for those humans, especially when it comes to how they work together. Whereas collaboration in a largely manual environment can be more intuitive, automation brings with it new blind spots and modes of communication (or a lack thereof) that demand a much more deliberate and thoughtful approach to collaboration.


> The presence of more automation actually complicates things for those humans, especially when it comes to how they work together.


Humans become the hard part. While not without complexity, automation is a known quantity, something that’s generally rational and measurable. Getting a team of humans working well together is an entirely different beast — there are some best practices to follow, some frameworks to lean on, but the problem always retains an unpredictable, greenfield element. This is exactly why many teams don’t venture beyond automation — it’s more concrete, more measurable, more approachable head-on. It’s easier to stand up CI/CD and keep adding bells and whistles to a pipeline than to confront the ambiguity and nuance of the human collaboration piece, especially as automation makes the human side ever more complex. It’s a vicious cycle.


In the aerospace world, there’s an entire area of research and operational focus dedicated to this recognition that humans are the hard part. It’s called[human factors](https://www.faa.gov/data_research/research/med_humanfacs/humanfactors) , and though it’s been a thing for many decades, its significance has grown as aviation has become more automated. The technological side has both matured and expanded to the point where blame for incidents more often sits with human operators, not just due to individual human error but especially because of the confusion and opaqueness that automation can bring to collaboration between humans. Automation is fundamentally changing how pilots work together in the cockpit, and the rise of human factors is a response to that. And not just for pilots in the cockpit — human factors looks at collaboration with air traffic control, ground crew, ops teams, and other stakeholders. The most prominent application of this human-centric school of thought is[crew resource management](https://skybrary.aero/articles/crew-resource-management-crm) (CRM), an entire system for collaborating that has only become more ubiquitous and critical in pilot training as aviation has digitized.


When it comes to you and your mobile team, the takeaway should be clear: no matter how automated things get, no matter how much work you’ve offloaded to CI/CD pipelines, humans are still in the picture. And crucially, the more you automate, the *more* you need to consider those humans, not *less* . Keep human factors in mind when designing workflows and tooling, and as you layer on more automation, regularly examine possible ramifications for the human stakeholders directly and indirectly affected. This can be especially difficult because the folks building out automation are very often removed from the folks affected — imagine a platform team relative to all the distributed engineers, PMs, QA, and other stakeholders their work touches. It’s important to try to bridge those gaps, creating spaces for sharing and building empathy across the org, treating internal end users like customers and baking human factors into your processes. For example, consider adding explicit prompts for human considerations in your internal tech docs and requests for proposals for new projects or tooling, and draw inspiration from crew resource management strategies to improve communication, create a clearer sense of ownership, and better anticipate failure modes.


## Plan for manual work, instead of pretending it’s not there


However much you automate, your team will be left with manual tasks throughout the dev and release lifecycle. Plus, with more automation in the mix, that manual work becomes more fragmented and less linear: manual steps are interspersed with automated ones, and the manual steps often branch down various divergent paths depending on the outcome of automated ones. As this “swiss cheese” effect grows, it becomes increasingly difficult for the humans involved to know exactly what is needed of them, and when. In all the interstitials between humans and machines you risk wasted time, badly executed handoffs, and dropped balls. It’s tempting to feel that in moving towards a new, heavily automated world you’ve left a manual one behind, but that can create tunnel vision which not only ignores the manual work that inevitably does remain, but also makes that manual work more of a liability and a drain. Plus, automation demands a new kind of manual work — the necessary monitoring and bookkeeping that teams often ignore.


In an aircraft’s cockpit, the increasing amount of automation has simply shifted the focus of manual work from *flying* the plane, to *managing* the plane. Human operators still have a long list of steps to action throughout a flight, most of which are directly or indirectly interdependent with automated behavior. And rather than becoming an afterthought, you could argue that the manual side is demanding as much — if not more — attention: not only is aviation’s strong checklist culture holding steady, but advancements have been bringing pilots’ manual work closer to, and more integrated with, aircraft systems. Modern flight computers and aircraft avionics allow pilots to run checklists and enter necessary inputs in a tightly integrated way, and peripheral devices like[electronic flight bags](https://www.skybrary.aero/articles/electronic-flight-bag-efb) also help bridge the gap between manual and automated work. Instead of pressure to make manual steps disappear, you find a strengthening of the framework around those steps that makes them more actionable and tied into the surrounding automated context.


There are interesting parallels to draw with the mobile development world. Very often there *is* concrete evidence of manual steps in a team’s workflows, but usually in the form of some legacy checklists or runbooks in haphazard Confluence docs or Google spreadsheets. They’re often neglected and only sporadically consulted or adhered to. Part of the problem is that automation-induced tunnel vision not only makes the manual parts of the process feel less important, but it also makes maintaining the artifacts that support the remaining manual work less of a priority. It’s worth confronting this head-on, not only to ensure your team has a solid supporting framework for remaining manual tasks, but to modernize that framework so that it integrates more naturally with the automation your team also has in place. Imagine, for example, an interactive runbook for your team’s release process that can reflect the state of the outside world — RCs and test results from CI/CD, say — making it easier for human operators to know exactly when they need to intervene with some manual action, and to make better informed decisions while doing so.[Sound familiar?](https://www.runway.team/use-case/mobile-devops) ;)


## The future of Mobile DevOps is part machine, part human


If there’s one common thread to take away from the discussion up til now it’s that, far from removing humans from the picture, the rise of automation actually calls for *more* consideration of human factors. Not only are there more touchpoints between humans and machines to manage, and more areas of potential ambiguity or opaqueness to combat, but there are fundamental changes to the way your team communicates and collaborates. It can be difficult to grasp exactly how this all plays out — and how to respond — but the world of aviation sets a pretty concrete and helpful example. As planes have gotten ever more sophisticated when it comes to automation, the focus on human involvement and collaboration in the cockpit has only become sharper and standards in the human domain have become even more demanding.


Exactly what this all means for you and your mobile team on a tactical level might depend on where you’re at in your Mobile DevOps journey, but the underlying message is universal. Whether you’re scaling new heights with mature tooling and well-oiled automation, or standing up your very first CI/CD pipeline, be aware of the human factor. Instead of blindly charging down the automation-as-panacea path, consider and design for the humans involved. Not only will that improve the way your team works together, but it will make your tooling and automation that much more effective as well.
