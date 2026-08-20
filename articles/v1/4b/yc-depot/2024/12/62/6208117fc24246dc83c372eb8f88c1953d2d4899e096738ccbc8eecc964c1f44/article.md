---
schema_version: "1.0.0"
document_id: "6208117fc24246dc83c372eb8f88c1953d2d4899e096738ccbc8eecc964c1f44"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/devex-dialogues-nic-pegg"
published_at: "2024-12-05T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:58:43.171296+00:00"
content_hash: "sha256:33b6d1fa9a2fc68b55b92deb6737c6a8b8be3eacf5fbaa7cd87e724ae6a69f79"
---

# Dialogues in DevEx: A Conversation with Nic Pegg

[Developer Experience](https://depot.dev/blog/devex-past-present-future) (DevEx) is at the heart of how software teams work and thrive. It encompasses everything from the tools developers use, to the processes and workflows they follow, to the culture of the teams they work within. And at its core, DevEx isn’t just about productivity. It’s about empowerment, satisfaction, and the ability to do meaningful work without unnecessary friction.


In this asynchronous interview series, we'll explore DevEx through the eyes of the people who live it every day. I’ll be speaking with software engineers, team leads, and engineering leaders to uncover how they define DevEx, the challenges they’ve faced in creating good experiences for themselves and other developers, and the creative ways they’ve sought to improve the experience of developing software in today’s modern professional landscape.


Our first interview is with[Thinkific](https://www.thinkific.com/) Staff Engineer[Nic Pegg](https://www.linkedin.com/in/nicpegg/) . We hope you enjoy the conversation!


---


**Kristen** : Thank you so much for agreeing to have this conversation with me, Nic! I sought you out specifically because of the ways in which I’ve seen you work to improve DevEx at Thinkific. I appreciate that you’ve made some of these efforts and learnings public through[your writing on LinkedIn](https://www.linkedin.com/pulse/role-empathy-retros-writing-team-support-charter-nic-pegg-ksnxe) so that others can learn from your experiences and experiments.


Before we dive in, can you tell us a little about yourself? The personal, the professional, the weird and the delightful—just whatever you’d like us to know!


**Nic** : Hi Kristen! Thank you for hosting this. I’m absolutely thrilled to be part of this dialogue series about DevEx.


I’m a British ex-pat living in Canada. I arrived in 2008 to learn to snowboard and ended up staying. My new friends switched up their boards for bikes in the summer. I thought that was a great idea, but mountain biking has a habit of putting me in the hospital. Injuries seem to be how I experience adventure. Like, when I hired a quad bike in Peru, lost control, and fell off the side of a volcano, my thumbs-up has been a bit wonky since then. More recently, I biked around Iceland (stunning doesn't come close!). This was my first time using clips, which led to a spectacularly awkward dismount and a bike cassette embedded in my calf! Iceland even inspired my daughter's name, Aevin, taken from ævintýri, meaning adventure.


Professionally, I led the Platform Team at Ritchie Bros Auctioneers from July 2018 to September 2021, which was transformative. Reflecting on that time, it felt like I’d been sleepwalking through my career before having the opportunity to scale a new team there. I learned so much across so many disciplines. Owning the stack end-to-end gave me the freedom to make the experience seamless and enjoyable for the team and our internal customers. It was here that I discovered my fascination for distributed systems and Event-Driven Architecture—bringing complex systems together in ways that feel intuitive and, hopefully, delightful for everyone involved.


At Thinkific, I found the name for something else I’d had an itch for that I was scratching in my own imperfect way: Developer Experience. Two years ago, a big push to improve processes and streamline local development drew me in. Since then, I have repeatedly dived into responses to DevEx surveys distributed amongst our teams, looking for ways to support my team and the entire engineering org.


**Kristen** : *You had a bike cassette embedded in your calf* ? There are so many questions I want to ask, but just know that I’m shuddering at the image 😆😭. (Also, I hope you’re okay!)


Why do you have this deep interest in DevEx?


**Nic** : I think my interest in DevEx comes from a few different places. I’ve lived through poor DevEx, come up with my own frustrating processes, and sometimes failed to support engineers. About six years ago, I had an aha moment: these kinds of issues are usually systemic. When I realized I was contributing to this complexity, I shifted my focus. Now, I look for those hidden friction points and try to smooth the path ahead.


**Kristen** : Is this DevEx work part of being a Staff Engineer at Thinkific, or is it something you take on by your own initiative?


**Nic** : I still find my role as a Staff Engineer somewhat nebulous. Yes, there are very concrete things like writing clean, maintainable code, mentoring other engineers and leaving helpful PR comments. But these are the things you’d expect of any engineer. There are also expectations around technical leadership and excellence. Thankfully, I find side quests and investigations to be fun. They fill in the gaps on the side of my desk so that I can find meaning in many different places and hopefully improve things in many different ways. So, that long-winded answer is me saying yes, I believe this kind of org-wide work is Staff+ work, but you need to do it as something that comes from your own initiative and drive.


I’ve also found that DevEx isn’t just about streamlined processes and easy-to-use software. There is an important human aspect, encompassing the beautiful, complex, and often unpredictable ways we experience work, that needs to be embraced. Leading with curiosity and empathy helps teams feel more connected and safe. When we acknowledge and work to improve these socio-technical dynamics holistically, people thrive.


**Kristen** : I’m not surprised that you find “side quests and investigations” to be fun – I think that’s evident just from some of the public-facing work you’ve shared. What’s your current or next-up DevEx side quest?


**Nic** : There are two things. First, I'm going through a research paper on 'developer bad days' - you know, those days where everything seems to go wrong. The paper looks at how technical hurdles and team friction don't just slow us down; they can really shake our confidence and make it harder to reach out for help. I'm curious how these insights could inform retrospectives, how we might correlate bad days with survey metrics, and what makes a 'good day.'


**Kristen** : Is this the[new research paper out by Ike Obi at Purdue and the folks at Microsoft](https://arxiv.org/pdf/2410.18379) ?


**Nic** : Yes, that’s the one!


The second side quest is a bit more technical. An ongoing passion project of mine is creating a Mocking service that generates realistic random events. The goal is to help decouple systems in local development by simulating real-world data flow without needing the full system to be up and running, hopefully making local development a bit easier.


**Kristen** : “Making local development a bit easier” – sounds like a service that strikes at the heart of DevEx. These incremental improvements can add up to big time savings for developers.


You mentioned that “DevEx isn’t just about streamlined processes and easy-to-use software.” That there’s this human aspect to it “that needs to be embraced.” I could not agree more, and in fact, my own entry into the world of DevEx was through focusing on the deeply human aspects of software development, things like developer thriving, AI skill threat, code review anxiety, etc.


How have you acknowledged and utilized human factors to improve DevEx for the teams you support?


**Nic** : An example that really stands out to me is taking a human-focused approach to retrospectives. Rather than a traditional sprint-focused approach, I like to lead retrospectives that explore deeper connections and hidden insights. I’ve come up with a few different formats that aim to uncover personal stories — things you probably wouldn’t share when writing a sticky note about how glad you are to have squashed that bug or thanking someone for unblocking you that week. One format aims to understand why writing that code felt so rewarding, or how seeing a massive PR needing your review crushed your spirits. Another format aims to uncover what support looks like for each person and how we can show up for each other.


Funnily enough, I attended one of Carol Lee's[Code Review Anxiety](https://developer-success-lab.gitbook.io/code-review-anxiety-workbook-1) sessions. I was curious about it both as someone who gets anxious about code reviews and as someone interested in making them less fraught for others. And yes, even as a Staff engineer, I absolutely get anxious about submitting PRs: Does it make sense? Is it clean enough? Will people question my role? Gosh, why did I not delete that debug comment before submitting it?


Seriously though, the anxiety is real - you want meaningful feedback on your work, but there's this emotional dance between wanting constructive dialogue and fearing criticism. I had actually given a lightning talk called 'Behind The Pull Request: The Good, The Bad & The Ugh' about making code reviews less stressful, and a colleague who'd seen it suggested that attending Carol's session would provide a valuable perspective for me. They were right - it helped me see the universality of those feelings and connect more ideas to make code reviews more constructive and supportive.


I'm really curious about your perspective on where DevEx is heading. With all the rapid changes in our industry, especially around AI, what human aspects of DevEx do you think we need to pay more attention to?


**Kristen** : The fact that you want to make code reviews less fraught and anxiety-inducing for others highlights what an incredible asset you are to an engineering organization and team, Nic!


And this is connected to a human aspect of DevEx I think many need to pay more attention to. For me, one of the hardest things about transitioning from teaching language into the software engineering space has been what I see as peoples' lack of interest in other people. Let me give an example: I once started a job where on the very first day, my manager organized a (virtual) welcome lunch for me. No one had ever done that for me before in tech, and it was just a really nice feeling to have this warm welcome gesture. However, once that lunch started, my new teammates didn’t actually make me feel welcome. They didn’t ask me a single question about myself. I left the lunch feeling like I’d done the work of keeping the conversation alive, and as a new member of the team, I don’t think this should have fallen on me. Funny enough, I’ve seen the same thing happen on “farewell” calls, where you’ve got 30 people in a virtual room to say goodbye to this person who’s leaving the company, and the vast majority sit awkwardly in silence, or with their cameras turned off, instead of actively celebrating this person who made an impact on the company and the people who work there.


I think the experience of developing software on teams could be greatly improved if folks made more of an effort to make others feel valued and like they belong. I think that feels good for individuals, but it also correlates to business outcomes. There’s empirical research supporting the positive effects of sense of belonging and sense of mattering on things like tenure at a company and even persistence in a field.


What do you think? Does that track with your own experience? Please feel free to call me out on any bullshit, there, or poke holes in my ideas.


**Nic** : Kristen, I do agree — to an extent! I have worked within teams and organizations that have felt like that. In hindsight, I can identify that these environments lacked psychological safety, which led to a lack of genuine connection. None of these ideas are new or revolutionary, but they are easily dismissed. When people feel safe — to share their ideas, struggles and passions without fear of judgement — care and connection can flourish.


I think it’s important to recognize that it’s not necessarily the fault of individuals when warmth and genuine connection are missing. Many of these same people likely have rich, supportive relationships outside of work. I believe psychological safety is driven by intentional efforts at the team and organizational level. I was reading something the other day about Google’s intense multi-year research into what is the biggest driver for thriving teams, and it was psychological safety, so obviously, Google wanted to figure out how to bottle it intentionally.


**Kristen** : That's a great point, Nic: It’s not “necessarily the fault of individuals when warmth and genuine connection are missing.” And I should know this better than anyone: I’ve felt compelled to censor myself – to basically tone down my ebullience – many times in my software career because I didn’t feel that this ebullience was welcomed or valued.


**Nic** : How awful. I’m really sorry that you’ve experienced that so often. I hope you’re now landing at companies that value you, not just your expertise.


This reminds me of Conway's Law, which suggests that organizations design systems that mirror their communication structures. I've noticed this also plays out in DevEx. Teams that are siloed or lack psychological safety repeatedly create tools and processes that reinforce those barriers. Conversely, when we build environments of trust and belonging, our technical solutions are more integrated and collaborative.


Your point about the research linking belonging to business outcomes is fascinating. I've observed this connection between human and technical factors in my own work. I’ve noticed that when teams feel psychologically safe, many good things tend to happen. People are more willing to share their code and collaborate with others across teams. They give and receive feedback in code reviews that’s not just constructive but genuinely meaningful. You also see more experimentation—people trying out new ideas without fear of failure. And, importantly, they feel comfortable admitting when they’re stuck, or when something didn’t go as planned.


I saw this play out when working on the event-driven architecture at Ritchie Bros. Teams were siloed and would throw us (the Platform Team) their data schemas, which were terrible, essentially just the database structure for a table. The technical result? Nonsensical and unusable data was available in real-time, yay. We had to drop into the teams and work with them non-judgmentally to coax out what they wanted to contribute. Building an event-streaming backbone that connects all the systems has to have core data principles that are understandable to the organization, not just the team. What started as a technical challenge was fundamentally about creating that sense of belonging and safety you mentioned. The technical solutions and team dynamics improved once teams felt they were valued contributors to a shared system rather than just data producers.


This makes me wonder - we could think of Conway's Law not just in terms of system architecture but also in terms of DevEx architecture. How we structure our teams and nurture psychological safety directly shapes the developer experience we create. My experience at Ritchie Bros showed me that when we intentionally break down those silos and create spaces for genuine connection, we see improvements in both technical outcomes and team wellbeing.


**Kristen** : I’m curious about your experience as a Platform engineer. I’m a big fan of the work that researchers Margaret-Anne Storey and Michaela Greiler have done in the DevEx space. In[one recent publication](https://dl.acm.org/doi/pdf/10.1145/3610285) , they write that “feedback loops, cognitive load, and flow state encapsulate the full range of friction types encountered by developers.”


About feedback loops in particular, they write:


> Fast feedback loops allow developers to complete their work quickly and with minimal friction. Slow feedback loops, by contrast, interrupt the development process, leading to frustration and delays as developers wait or decide to switch tasks. Slow feedback loops cause additional interruptions when feedback from systems (such as an integrated test run) or people (such as review notes) is returned and requires immediate attention.


They advise:


> To improve DevEx, organizations should shorten feedback loops by identifying areas where development tools can be accelerated (for example, build and test processes, development environment setup) or human hand-off processes improved.


As a Platform engineer, how else do you strive to improve feedback loops? Which of these feedback loops do you think are easiest to shorten, and which do you think present really hard problems for organizations to solve?


**Nic** : Honestly, I think they’re all hard problems to solve. Working with local development environments has consistently been a friction area for many engineers at Thinkific, and we’ve tried introducing improvements, but they’ve been piecemeal or targeted at very specific things. I’m starting to realize that a survey just isn’t enough. It’s a really great start to surface friction and sentiment and helps show value to senior leaders by correlating time lost with salaries. However, we need to go deeper.


What I mean by deeper is that we need meaningful conversations with everyone involved - not just engineers but also managers and anyone who touches our development processes. Each role brings a unique perspective on where the friction points are and what "good" looks like for them. These conversations often reveal interconnected issues that aren't apparent from survey data alone.


My research into Team Topologies alongside DevEx influences has really shaped my thinking on what the next steps should look like. I believe we need a plan that operates at three levels: strategic vision, tactical improvements, and pragmatic implementation. While platform teams can absolutely help reduce friction through technical improvements, I'm becoming convinced that much of what needs to be done happens at the team and individual level.


I've been particularly inspired by Team Topologies' concept of enabling teams during transition phases. Instead of platform teams trying to solve everything centrally, what if we could surface DevEx-related improvements directly in team backlogs? Teams could own and understand how to improve their local dev while having the platform team available as a support system during that transition. I'm still developing the idea, but I think this approach could lead to more sustainable improvements than centralized solutions alone.


**Kristen** : I’ve had my hands out of the code for going on a couple years now, Nic, but my gut reaction to the idea of teams owning and understanding how to improve their local development environment and processes while having the platform team available as a support system is that yes, this is how it should work.


Alright Nic - one last question for the road: Is there a developer tool you’ve started using in the last year or so that has improved your Developer Experience?


**Nic** : I used[ShadowTraffic](https://shadowtraffic.io/) recently to send realistic data across different services and test out some ideas I’ve been playing with. It’s been really helpful for exploring how systems behave under load and non-standard conditions without setting up complex test environments. The last thing I’d been playing with was sending data from ShadowTraffic to a Webhook, which saved the data into Mongo and was then CDCed to RisingWave, where I could create materialized views of the data to answer interesting questions in real-time (nothing to do with work, but a whole lot of fun).


Thank you, Kristen. I’ve really enjoyed this conversation with you and am grateful for the opportunity to reflect on and share my experiences.


**Kristen** : The feeling is so mutual, Nic! Thanks for sharing all these insightful thoughts and experiences with me and our audience!


Kristen Foster-Marks


Head of Developer Experience at Depot
