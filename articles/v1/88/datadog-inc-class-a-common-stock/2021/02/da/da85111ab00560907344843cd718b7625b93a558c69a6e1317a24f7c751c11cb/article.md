---
schema_version: "1.0.0"
document_id: "da85111ab00560907344843cd718b7625b93a558c69a6e1317a24f7c751c11cb"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/engineering-spotlight-mael-nison/"
published_at: "2021-02-10T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:43f810abbd837374e790c75b583f209b8ad0a2f83111bf84560137a06de11140"
---

# Engineering spotlight: Maël Nison

Daniel Maher


Maël Nison


*In this edition of the Datadog Engineering Spotlight, Daniel from the Community team sat down (virtually) with Maël Nison, Senior Software Engineer on the Frontend Platform team to talk about his journey from childhood on a remote island to the principal maintainer of Yarn, and more!*


## To weave a tale of adventure…


In 2017, a young software developer by the name of Maël Nison decided it was time for something new. Fresh out of university and with a few years of start-up experience under his belt, he was looking for new opportunities in bigger venues. When he packed his bags and left his native France for the cosmopolitan scene in London, he never thought he’d end up becoming the driving force behind one of the JavaScript world’s most important open source tools:[Yarn](https://yarnpkg.com/) .


Maël joined Facebook with no specific intention to work on a JavaScript package manager, but thanks to a “boot camp” program for new hires, he was quickly identified and brought on to the then-nascent Yarn project. Maël didn’t hesitate: “Open source on my work time,” he said, “that sounds great!” What started as a few pull requests to a relatively small project put him on a multi-year journey across code and community that he’s still a part of to this day. But it wasn’t just the ones and zeros that kept Maël interested—it was the chance to benefit a larger community, and to really push himself into uncharted territory.


The Yarn project has grown and evolved in new and interesting ways over the past couple of years. Notably, it was rewritten in[TypeScript](https://www.typescriptlang.org/) and re-architected to be more modular, but it’s also grown into a true community-driven open source project, instead of an in-house Facebook tool. This has led Maël to wear “many hats” throughout his involvement in the project. “I’ve been a developer, a product manager, a team lead, I’ve done customer support. I’ve worked on infrastructure, web design, community. I’ve even been a CEO, in a sense, working simultaneously on vision, roadmap, evangelism, outreach, and more,” he said. And while he’s no longer at Facebook—having joined Datadog during the summer of 2019—he’s still at the helm of Yarn, and working on even more new challenges as well.


But these sorts of career-building and career-defining opportunities don’t happen by accident though, so it’s worth diving into Maël’s story—to learn a bit about how he got there, and what we can take away for our own professional journey as well.


## Islands, BASIC, and the cowboy days of the internet


Maël didn’t always want to be a computer engineer. Growing up on the remote Indian Ocean island of[La Réunion](https://en.wikipedia.org/wiki/R%C3%A9union) , he was more interested in beaches and samosas—and he didn’t have access to much in the way of computers or gadgetry in any case. All of that changed when he moved with his family to continental France and began middle school in the sun-baked southern city of Toulouse, one of the world’s principal hubs for aeronautics and engineering technology. There, his local school had not only a small computer lab, but a proper club as well! Every day at noon, middle schoolers had the chance to try their hand at programming.


“I made friends,” Maël recounts, “and a few of them had started a programming club at the school. Every day at noon, we were able to go into one room of the school which had a few computers and learn by ourselves how to make games. The programming language they had picked was[DarkBASIC](https://en.wikipedia.org/wiki/The_Game_Creators#DarkBASIC) . It was a far cry from the languages I now use every day, but the basics were there!”


DarkBASIC, a variant of BASIC first released around the turn of the millennium, was designed specifically for making video games on Windows platforms. The idea was to provide a simple way to introduce programming novices to 2D and 3D development. Through the use of pre-built function libraries and provided tutorials, new developers were able to produce functional DirectX games with relative ease—especially compared to other languages and libraries available at the time. And according to Maël: it worked.


“One of my friends made a 2D game where a fish swam from one side of the screen to the other, and you could shoot bubbles at it. It was cute.” Maël was hooked: “I was able to create something and see it on the screen—and if it wasn’t right, I fixed it just by following the code. It was a very logical path from A to B. I knew that this was what I wanted to do. From that point, I never doubted that it would be my career.” This was in 2003, when 3G mobile networks were just being rolled out, the Pentium III processor was still in common usage, and JavaScript—while it had existed for some time—was still just a fun toy for client-side scripting.


During high school he was fortunate enough to be able to follow his passion. Late nights and long weekends spent hacking on any bit of kit he could get his hands on, he was already developing PHP websites—and “writing SQL vulnerabilities because I wasn’t super good at the time.” By the time he graduated, he already had experience with a variety of languages and platforms, and was active in the open source community. But the web was a different place at the turn of the 21st century; the internet had already assumed dominance as the principal communications platform on the planet, having long-since replaced things like BBSes,[Minitel](https://en.wikipedia.org/wiki/Minitel) , and other regional solutions. But while the internet itself presented a unified communications platform, the actual communications themselves were anything but. Facebook didn’t exist, Google was just a search engine, and forums were the predominant method for grouped, asynchronous communication.


> There was no GitHub at the time, so software was much harder to distribute. Usually I just went on a forum and put the source archive in a post and said, hey, download it if you want.


It was a short leap from using forum software to developing forum software, and from there, to content management systems, and to a period of discovery on a subject that would come to define his career for years to come: streamlining workflows. “All content management systems were the same. You had to click on all these buttons, then go to the administration page, then to the right section… I wanted to do something different. What if you could just click on \[a piece of\] content, and edit the post directly without having to go to an admin page? When I think about it now, it’s something that most CMSes do, but at the time it was fairly new.”


Find a problem case, develop a solution, make that solution available to everybody that might need it. That’s the spirit of open source—and a spirit that would serve Maël especially well at university and beyond.


## Paris, London, and beyond


After completing high school, he was accepted to[EPITECH](https://www.epitech.eu/) and ultimately said goodbye to Toulouse, trading pink terracotta for the wide Haussmannian boulevards of Paris. Well-known for focusing on practical, applied education, and with a laser-focus on technical curricula, EPITECH relies heavily on self-directed learning and peer assessment, putting it well outside of classic French pedagogy, which is theory-heavy and typically relies on strict, hierarchical relationships between teachers and students.


“I had a blast because that was exactly what I wanted to do,” Maël said. Shifting continents a second time, he even had a chance to do a year abroad in Québec: “I went to Canada and had a really great time. I ended up doing my \[final year\] of studies at the same time as I was doing my first full-time job, so it was a bit more challenging—but that was the end of the line, so it was okay.”


Maël wasted no time getting his career underway after graduation, and over the next few years, he’d bring his talents to a variety of organizations. “I went with very different organizational structures. Start-ups that had just raised money \[and\] fresh companies started by friends. I \[also\] worked for a super huge company, and now I’m at Datadog, which is a completely different type of place.” Different indeed. From a few friends around a folding table, to one of the largest tech companies on Earth, to a recently-IPO’d rising star, these different structures each brought their own perspectives, challenges, and opportunities.


> Start-ups, when I started my career, were the perfect fit. You could join a company and get a lot of responsibilities right away, because they don’t have a lot of employees. You get to touch everything—and everything you do is very impactful, because if you do crappy work then the company will not do well. At the same time, if you do a great job, then the company will perform well. And you will be happy to have provided this value. And since you’re put on a lot of different projects, you can grow faster than if you were just on a segment of something that you don’t entirely control.


Smaller shops provided Maël with a risk/reward structure that served him well, but after a few years, he realized that it was time to try something new. Something bigger. Something where he could have the impact he craved, but on an entirely new scale. It was the same urge that pushed him from his small southern city to the bright lights of Paris, as Maël recounts: “We were a hundred people at EPITECH Toulouse, and in Paris, I think it was a thousand. One of the reasons I wanted to go to the thousand people is because the more people there are, the more people you have to challenge you.”


That scale and those challenges were exactly what he found at his next gig, but not quite in the form that he expected. “One big reason why I joined Facebook was because it was the place to be for everything that was JavaScript—in particular because of[React](https://reactjs.org/) . But also because they had built this new package manager a few months before: Yarn.” That, of course, brings us back to where we started, but Maël certainly hasn’t slowed down since then. Like the project he came to lead, Maël has also grown and evolved, and after leaving Facebook he returned to Paris with a renewed commitment to surrounding himself with top talent and tough challenges—but now with the added twist of Yarn and open source in general.


## Enter: Datadog.


As part of an internal tools team, Maël works directly on everything that impacts developer experience—effectively the UX of development—which translates to interactions with a vast array of tools, platforms, packages, and people! But what does that mean? “For example, performance in the CLIs are important to me. Right now, I’m working on improving the time it takes to run specific commands, which is currently at 10 seconds, and I would like to bring it to two seconds.”


If that sounds like a small thing, consider this: that 10-second command might get run a thousand times a day across the organization, which means an aggregate 167 minutes of compute time lost to a pipeline. Bringing that down to two seconds would mean a savings of 11 hours over the course of a week—that’s 11 extra hours of cloud resources that could be put to good use elsewhere. Do this a few times in a row, and the impact becomes significant.


Maël has a number of open source projects on the go at any time, including improvements to VSCode, a browser extension for CI/CD pipelines, and—of course—Yarn, which is used heavily at Datadog. As he notes, “one of the good thing\[s\] of a company working on open source is that you can really benefit from the work that both internal and external contributors are doing.”


Technology-wise, it’s “almost exclusively JavaScript”, but the language is only a small part of being successful in the role. It’s also important to have the willingness and desire to learn new things and adapt to new information. But even moreso, the truly crucial element of Maël’s success has nothing to do with computers at all: it’s the ability to communicate effectively and empathetically with the people who are using these tools.


> My job is mostly to \[…\] make the right trade offs, to be aware of what the problems will be when we implement a solution and to try to mitigate them the best we can. Another large part of \[our team’s\] work is to be able to communicate well, and to be sure that we’re working on impactful things for the product teams. It’s easy to just pick something to work on because it seems interesting to you, but then you’re discussing with the product team and they tell you that it’s a workflow that \[they\] only do once every blue moon. You need to be constantly aligned on what you’re doing and whether it fixes actual problems.


And that’s ultimately the most important thing that Maël has learnt thus far: “Yes, it’s important to be up to date on all the new technologies \[…\] but what is really important is communicating with all the consumers of our tools.” In other words, computers are just tools—it’s the people that matter.


If you’d like to work on Yarn—or on any number of other interesting product and technology topics—Datadog is hiring!


-
-
-
