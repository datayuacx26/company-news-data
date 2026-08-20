---
schema_version: "1.0.0"
document_id: "07a3503e2e9608a340d7407e65a94686b6a6d6460f8264938592aef6defacbc5"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/devex-dialogues-dan-davidson"
published_at: "2025-01-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:ac463aee1d4c67256972fe3bcf587770f6195925345277bf89867392a9e37a41"
---

# Dialogues in DevEx: A Conversation with Dan Davidson

[Developer Experience](https://depot.dev/blog/devex-past-present-future) (DevEx) is at the heart of how software teams work and thrive. It encompasses everything from the tools developers use, to the processes and workflows they follow, to the culture of the teams they work within. And at its core, DevEx isn’t just about productivity. It’s about empowerment, satisfaction, and the ability to do meaningful work without unnecessary friction.


In this asynchronous interview series, we explore DevEx through the eyes of the people who live it every day. I’ll be speaking with software professionals across roles and industries to uncover how they define DevEx, the challenges they’ve faced in creating good developer experiences for themselves and other developers, and the creative ways they’ve sought to improve the experience of developing software in today’s modern professional landscape.


Our third interview is with Senior Software Engineer[Dan Davidson](https://www.linkedin.com/in/dandd/) . We hope you enjoy the conversation!


---


**Kristen** : Thanks for agreeing to have this conversation about[Developer Experience](https://depot.dev/blog/devex-past-present-future) (DevEx) with me, Dan! I reached out to you for this interview because of some interesting conversations we’ve had around what it’s like being a software developer when you feel like you don’t fit the programmer stereotype. I’m excited to explore that sentiment in the context of DevEx.


To start, can you tell the audience a little about yourself?


**Dan** : Hey everyone! I’ve spent over 20 years in various roles within the development world. I began by building simple websites with table-based layouts and snippets of JavaScript, which quickly evolved into a business where I took on diverse projects that kept me motivated to learn and grow. Each project was unique, so I was always discovering new skills to meet my clients’ needs.


Many of these projects combined coding in WordPress with graphic design, and each presented its own challenges. This is when I realized my passion for continuous learning. The world of code is always changing, and there’s always something new to discover.


I’m a bit unconventional in several ways. While I enjoy the mental challenges of problem-solving in code, I’m also very extroverted and thrive on building relationships and collaborating with others. I also need physical activity to stay focused, so I’m a runner to balance out my time at the desk.


I’ve been married to my wonderful wife for 20 years, and we have two boys, ages 9 and 13, who are both huge fans of soccer and sports trivia.


**Kristen** : I get that need for physical activity, Dan. Too much sitting, and I just have to get up and go for a walk or run. In fact, my best work ideas always hit me on dog walks and trail runs 😆


You’ve mentioned to me previously that you feel like you’ve struggled to advance in your career at times because you don’t fit the programmer stereotype. Can you share a little more on that?


**Dan** : I’d be happy to. As an engineer, I’ve noticed that it can take me longer than some of my colleagues to solidify technically challenging concepts. This extra time has always been part of my learning process; I put in the effort to deeply understand the material. While this “double-down” approach is highly beneficial for me, it may not always align with what engineering managers typically expect. My work is thorough, clean, well thought-out, and rigorously tested, but I generally need more time to deliver the same results as some of my peers. My contributions to the codebase tend to go through multiple iterations as I refine solutions and seek feedback from others. This iterative approach might give managers the impression that I’m not as promotable as others. I believe these factors have played a role in the challenges I’ve faced advancing my career as an individual contributor in software engineering.


However, on the social side, I’m often the first to ask questions, correspond, discuss, and present ideas — even on technical topics.


**Kristen** : That’s so interesting that you feel your practice of iterating through multiple feedback cycles has made you less promotable in the eyes of your managers. In my personal experiences on software teams, iteration has been considered the “normal” way of doing things. I’m sorry that you’ve had the opposite experience.


With the rise of engineering metrics platforms and the popularization of frameworks like[DORA](https://dora.dev/research/?view=detail) and[SPACE](https://dl.acm.org/doi/10.1145/3454122.3454124) , our velocity and throughput as software engineers has never been more visible, and it's never been more scrutinized, I would argue. I think it’s no secret that some engineering managers don’t utilize these metrics in healthy ways. Do you think there’s a connection here between the rise of these frameworks and the challenges you've faced?


**Dan** : Yes! The[SPACE](https://dl.acm.org/doi/10.1145/3454122.3454124) framework provides real measures of a developer's work habits and performance, therefore helping to optimize team dynamics and keep the team in a state of flow. However, in my experience, the teams I've worked with haven’t fully embraced any framework to effectively measure and gain insights into the Developer Experience. Velocity is often the only metric that gets attention. I believe that if these teams were to adopt the[SPACE](https://dl.acm.org/doi/10.1145/3454122.3454124) framework or something similar, leaders would gain a new perspective on developer productivity — one that values a broader and more nuanced set of metrics.


**Kristen** : That adoption piece can be so challenging, can’t it? I have some theories around why people and teams haven’t leaned into using DevEx frameworks and metrics to improve the experience of developing, but I’d love to hear your thoughts. In your experience, what stands in the way of this adoption?


**Dan** : Change can be difficult, especially because people and teams often have strong, fixed opinions about how things should be. DevEx frameworks are designed to foster an environment where growth and learning can flourish. However, I believe that for some leaders, embracing this process can feel like a loss of control, since the framework’s benchmarks might not align with their own.


**Kristen** : If it were up to you, what kinds of metrics would you measure and attempt to improve upon?


**Dan** : If it were up to me, I would track metrics across all facets of the[SPACE](https://dl.acm.org/doi/10.1145/3454122.3454124) framework, which captures the various ways developers contribute and grow. I would make a point to recognize developers who excel in each of these areas, especially when they reach a level of mastery. I find it incredibly rewarding to celebrate individual successes.


**Kristen** : For the reader, I’ll just add that the[SPACE](https://dl.acm.org/doi/10.1145/3454122.3454124) framework is meant to measure developer productivity across an engineering organization by examining five dimensions:


- **S** atisfaction and well-being
- **P** erformance
- **A** ctivity
- **C** ommunication and collaboration
- **E** fficiency and flow


Maybe we can zoom in on *efficiency and flow* , which I think is inextricably connected to DevEx. The framework authors themselves state that this dimension captures:


> the ability to complete work or make progress on it with minimal interruptions or delays, whether individually or through a system


They continue:


> At the team and system level, efficiency is related to value-stream mapping, which captures the steps needed to take software from idea and creation to delivering it to the end customer.


I’m curious about your thoughts and experiences with that system-level efficiency. Over the course of your career, what tools or technologies have been introduced that have provided the greatest boosts in system-level efficiency?


**Dan** : An effective system simplifies tasks like tracking progress. While no system is perfect, having a workflow that integrates seamlessly with your task board or work-tracking tool makes it much easier to transition between tasks and make progress across multiple areas. Throughout my career, I’ve worked with a variety of systems, including Jira, LeanKit, Asana, Trello, and more that to varying degrees helped in tracking progress and scoping projects. However, one consistent truth applies to all of them: they require diligence. Regularly updating task statuses and reviewing progress daily is essential. Ultimately, these tools are only as effective as the effort you put into using them.


**Kristen** : Agreed – these project management and issue-tracking tools are only as helpful as the effort and care put into them. I tend to like them primarily for making my work and progress transparent to those who have a stake in that work.


I am asking a slightly different question, though: I’m curious about **efficiency** , rather than **effectiveness** . They’re related, of course. Some might argue that efficiency is an input to effectiveness. To ask my original question in other words: What developer tools have most helped you *get shit done* ? 😆


**Dan** : That’s an entirely different question, isn’t it? If efficiency is the goal, then perhaps we should skip the tools altogether and delegate project planning, tracking, and metrics to someone else. In my experience, every tool I’ve used tends to hinder efficient workflows under the guise of improving them. Perhaps AI will soon evolve to automate these tasks more effectively.


As a developer, my ideal workflow would involve receiving prioritized micro-tasks. I want a steady stream of small, achievable tasks that I can tackle quickly and push into the CI/CD pipeline. Small units of work, small wins every day.


Of course, this approach would require careful task management to ensure logical sequencing and avoid breaking things — but that’s exactly where AI could step in. Intelligent task assignment could make this vision a reality, enabling both efficiency and reliability.


**Kristen** : I’d love to push back on this idea that “If efficiency is the goal, then perhaps we should skip the tools altogether.”


A couple weeks ago, I had the opportunity to jump into my team’s web application and do some refactoring to make a component label dynamic to the component’s context. This was after holding roles for over two years that had my hands largely *out* of the code, and I was feeling a bit rusty.


Some of the developer tools I used were:


- Typescript (which I’d never used in an enterprise development environment before)
- Visual Studio Code (the Typescript plugin)
- ChatGPT (to get quick suggestions for syntax)
- a Typescript linter
- GitHub actions for CI/CD
- [Depot](https://depot.dev/) (the product I support – to[make the GitHub Actions run much faster](https://depot.dev/products/github-actions) than they would have otherwise)


I couldn’t believe how much *easier* development felt than it had two-and-a-half years ago, and it felt easier because of these developer tools. They each enabled me to more quickly – to more *efficiently* – complete the coding task. I mentioned this to my team during standup the next day, asking, “Has developer tooling really gotten that much better in the last two years?” which garnered furious head nodding and a great discussion on the tools we now have available to us that make our jobs a little easier. One of my teammates even mentioned how great these advances are for folks just getting into programming careers, arguing that they can help remove barriers to entry.


So, I actually wonder if we’re talking about slightly different things when we’re talking about **developer tools** . Perhaps you’re talking more about **project management** tools when you say, “Every tool I’ve used tends to hinder efficient workflows under the guise of improving them”?


**Dan** : In my broad generalization, I definitely wasn’t referring to developer tools or AI code assistants. Those tools play a huge role in improving workflows and boosting developer productivity. What feels clunky to me is the context switching between 'dev mode' and 'progress reporting mode.'


I should have clarified this earlier, because anything designed to keep you focused on building significantly enhances the developer experience.


**Kristen** : Ah, yes – “progress reporting mode” always takes some amount of time, doesn’t it? It ain’t coding, that’s for sure. Thank goodness we have some tooling to make that reporting automatic in some cases, like project management platforms that integrate with GitHub so that issue statuses get automagically updated when a PR is opened or merged to production.


I love your sentiment that “anything designed to keep you focused on building significantly enhances the developer experience” – I couldn’t agree more, and this is what I mean when I say that the *efficiency and flow* dimension of the[SPACE](https://dl.acm.org/doi/10.1145/3454122.3454124) framework is so inextricably tied up with great DevEx. It’s one of the reasons I can so wholeheartedly get behind a product like[Depot](https://depot.dev/home) – for me, few things tank my efficiency and interrupt flow more than having to wait 45 minutes for a build to complete. Especially when that build fails at minute 37, and I have to troubleshoot the issue and then run the whole thing all over again.


I still want to know: What developer tools most help you *get shit done* ?


**Dan** : I prefer to keep my development toolset lightweight, so having robust linting and typeahead features seamlessly integrated into my IDE is essential. Simple shortcuts like "CTRL+SPACE" for automatically importing the necessary dependencies save significant time. Well-configured ESLint rules aligned with the team's development practices eliminate much of the back-and-forth and nitpicking during code reviews. We are using TypeScript on my team, which has helped us catch so many coding issues early on. This, I’m certain, has saved us from accumulating an unquantifiable amount of tech debt. Our developer documentation is an area where we are working to improve. Better documentation will be helpful in knowledge-sharing and onboarding new developers.


**Kristen** : I've recently fallen head-over-heels in love with Typescript. Takes so much manual checking and guesswork out of development!


Thanks for sharing your thoughts with our reading audience here, Dan. It was a true pleasure getting to explore Developer Experience through your experiences!


## Related posts


- [Developer Experience: Past, Present, & Future](https://depot.dev/blog/devex-past-present-future)
- [Dialogues in DevEx: A conversation with Nic Pegg](https://depot.dev/blog/devex-dialogues-nic-pegg)
- [Dialogues in DevEx: A conversation with Annabelle Thomas Taylor](https://depot.dev/blog/devex-dialogues-annabelle-thomas-taylor)


Kristen Foster-Marks


Head of Developer Experience at Depot
