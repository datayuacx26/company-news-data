---
schema_version: "1.0.0"
document_id: "3fbbbdeeba8368afdc37b40bd63a1268c7ef0ee752ec99211bd8a691cd736a4d"
company_key: "yc-infield"
company: "Infield"
source_id: "yc-infield-news-import-3dde9d4b3c10"
canonical_url: "https://www.infield.ai/post/once-a-maintainer-mike-mcquaid"
published_at: null
first_seen_at: "2026-07-25T09:33:54.576450+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:6e3b6eacde15fef1e84772f98cff57b6e682456df2387cfccbfb5452477b6056"
---

# Once a Maintainer: Mike McQuaid

Welcome to Once a Maintainer, where we interview open source maintainers and tell their story.


This week we’re talking to[Mike McQuaid](https://github.com/MikeMcQuaid) , project leader and longest tenured maintainer of[Homebrew](https://brew.sh/) , a package manager for macOS and Linux used by tens of millions of developers worldwide. After ten years at GitHub, Mike is now CTO of[Workbrew](https://workbrew.com/) , a startup for managing a fleet of machines running Homebrew. Mike spoke with us from Edinburgh, Scotland.


**What was your first exposure to computers and eventually writing software?**


As a kid my first exposure was at my school. We had BBC Micros, which is what we grew up with in the UK. This was in the early 90’s. And my dad noticed that pretty much any task, no matter how boring, if it involved computers I would happily do it. He was in finance and back before you could get online stock prices, every Saturday he would get the Financial Times and there would be the big broadsheet with the stock prices, and I had some little spreadsheet program and I would basically just go through and enter all the stock prices into his spreadsheet and spend like two hours on a Saturday afternoon doing that. And for me that was delightful. And my dad was like, OK, what's going on with this kid?


And then I just evolved my own interest through PC gaming and fiddling around trying to get the games to work on my computer. Eventually I went off to university and did a computer science and business degree and got a job as a software engineer. This was 17 years ago.


**What was the environment like at your first job?**


My first job was for BT, British Telecom. It’s sort of similar to AT&T if you’re in the States. And this was back in the days when it wasn’t that AI was going to possibly destroy your job, it was offshoring. Why would you be British and interested in writing software, because in two years all the jobs will be in India because they’re just as good for half the money, etc. etc. That was my first experience at a massive company.


I’d been dabbling with open source for a few years, and I learned pretty quickly that the open source way of doing things - it goes down well in the open source community, but at your real job you get pulled into your manager’s office, like what have you done? So that didn’t work super well with me. After that I left and was the first employee at a startup called Mendeley in London. I’d been hacking on KDE, the Linux desktop environment, for a few years at that point. Hired a bunch of people from the KDE community there, stayed there about a year, did some consulting around Qt for KDAB, a Swedish company. And then I kind of got this sense that maybe the cross-platform desktop app world was not future-proof, you know, starting to smell things. I did a bit of research. I wanted to work for a company using Ruby on Rails based in the Bay Area and pretty quickly came upon Github. I applied, and they said “Not now, you’re too fresh.” So I went to work for this company AllTrails. And then several applications later, I got a job at Github where I worked for ten years.


I guess that’s a nice segue back to the open source stuff since I left there last year and started my own company with some former Github people called Workbrew, and we’re building product solutions for bigger companies around Homebrew.


**I’m having this Zoom from a KDE desktop. So I’m curious how did you get into working on it?**


So I did Google Summer of Code the summer after I graduated, which was in 2007. Back then you could have journals, and you could post journals to your blog. And I basically built that integration so you could post journals to your blog. I built blogging support for WordPress and a couple of other blogs as my summer code project. And then I ended up sticking around and being involved with like bits and pieces on the KDE side for, you know, maybe a kind of couple years. And then I moved to Mac and I was doing some KDE porting to Mac and then I sort of gave up on it as a lost cause.


**I remember a period in the middle there where it was like you're gonna run all of your Linux apps everywhere - I think I tried to install**[Amarok](https://amarok.kde.org/) **or something on Windows, and it sort of worked. Not the way the world ended up going.**


No, it never worked quite as nicely not on Linux, unfortunately.


**What was your first exposure to Ruby?**


During university I had started dabbling with Linux and using various Linux distros, submitting patches to things and modifying things on my system or whatever. I’d seen a tiny amount of Ruby but mostly I got into KDE and playing with lower level Linux bits and pieces. Ruby didn’t come until later on when I was at the consultancy and I heard about this project called Homebrew from a friend of a friend in London. And that was the first serious Ruby that I wrote in 2009 or so, and I’ve been using it for the majority of my career ever since.


**We hear a lot about how intimidated people are to get involved with open source. So when you heard about Homebrew, what made you get over that hump and get involved?**


Hopefully without sounding too arrogant, I never felt particularly intimidated to get involved with open source projects. I think particularly in the Linux era and the pre-Github era, the barriers to entry were so high that it felt like if you managed to get through those barriers people were generally like, we're going to be your friend now. And if you're in my city, I'll take you out for dinner. I’d been to a few open source conferences by this point and everyone was very friendly and welcoming and accepting.


So with Homebrew, one of the guys I hired at Mendeley was friends with this other guy who worked at another startup in London. And he was the guy that created Homebrew,[Max Howell](https://github.com/mxcl) . I think having that connection and in the first year or so, being involved with Homebrew, meeting up and getting beers with Max and talking about our thoughts, it felt easy. What sucked me into Homebrew initially was just this idea of scratching your own itch - I’m using this, I need this, and I’m going to add a new thing because it’s easier if it’s in here and then everyone else can use it. And then over time it becomes less about me building for myself and more about, what does everyone who uses this project need and how can I help them?


**How do you manage the roadmap? We talk to some maintainers where the projects, especially the huge ones, are very formally run. Others are still very informal. How do you think about that?**


Yeah, I would say we’re on the moderately anarchistic end in terms of feature roadmaps and stuff like that. At least until you get to open source projects run entirely by corporations, maintainers have no ability to make anyone work on anything they don’t want to. I'm the longest running maintainer, I've been the project leader. But if I say like, hey Bob, I want you to ship this thing by the end of the month, then Bob can just be like, nah. And I don't really have any ability to do anything beyond that.


So in some ways it was quite similar when I was a principal engineer at GitHub, because when you're in those roles, you have no direct reports and so you don’t have twenty engineers to do what you tell them, but at the same time, you have an awful lot of cultural influence. So instead of it being like, I will tell you what to do, it becomes like, hey, I've had an idea, who wants to join me on this journey? And there's some people with within Homebrew particularly who would like to be doing more Homebrew stuff. So I guess a big change for me in the last few years is that I try to push as much of my personal Homebrew to do list into issues that are tagged ‘help wanted’. Sometimes those are done by me, and sometimes those are done by random people in the community. There are[five open issues](https://github.com/Homebrew/brew/issues?q=is%3Aissue+is%3Aopen+sort%3Aupdated-desc+label%3A%22help+wanted%22+author%3AMikeMcQuaid) in the main Homebrew repo right now and all of them are things that five years ago I would have maybe just had in my Apple notes somewhere. But I learned that there are a lot of people that might go “hey, that’s a good idea” and jump in and get involved.


**Do you guys have any kind of regular meetings among the core maintainer team?**


Once a year we try and get as many maintainers as possible to meet together in person for our AGM. We try and collocate that with FOSDEM, which is in Brussels every year in early February, to try to make a bit of a weekend of it. We're trying to do more events now. We're doing a hackathon focused on performance and security next month. And historically we’ve had some vaguely regular Zoom calls, but it’s hard to sort out the timing for those. Most of our private communication is happening in Slack. That's where we have the conversation about what do you think we should be doing? Or, there’s this issue right now. Could someone jump on and help with that?


**So this list of issues here, I see this one about Sorbet. How does that conversation, like “we should add type checking to Homebrew”, get shepherded through?**


That's a pretty good example actually. You inadvertently stumbled on one of the more amusing but contentious ones. All of our[governance documentation](https://docs.brew.sh/Homebrew-Governance) is public but if you’re not a Homebrew maintainer, it can be a bit of a struggle to get through it all. We have the project leadership committee, which is essentially managing the money and the governance structure. And historically that was also maintainers. But now two of the five people in that are not maintainers and never have been. Then we have the technical steering committee, which is managing roadmaps and deciding these types of things. And then we have a project leader, which is like an elected position as well, which is me. And I'm the only person who's ever been the project leader so far.


But the way it works in reality is that the technical steering committee exists to help resolve conflicts in technical direction I’ve been unable to resolve myself with the other maintainers. And that's what happened with the Sorbet stuff. Interestingly, a few years ago when it was initially proposed, I was not a fan of the idea. Now, however, I'm actually a big proponent of Sorbet. I think we should double down on it and use it even more.


**What convinced you?**


Well sometimes the way it can go with open source projects is that someone gets an idea, that one person is very enthusiastic. And people get excited and say yeah we want to get involved, great. But what can happen in the worst case is that none of the people who say they’re going to step up actually step up, and the person who pushed it to begin with got bored and wandered off, and now you’re left with these problems. And then it becomes someone else’s problem, often mine, to clean up the mess. So I thought that was the way it was going to go with Sorbet. But it turned out that over time actually more people got interested in it, and more people got involved. And personally over at Workbrew we started using it to solve a bunch of problems, and at GitHub a bit as well, and so now having used it to solve problems I’m like ok, this is better than I thought it was.


**So in the 15+ years that you’ve been working in open source, do you feel that the way that open source projects are run have changed? What have you noticed in terms of open source culture over the last 15 years?**


I think the really big thing in the last 15 years is where and how open source is happening. So 15 years ago I'm not sure whether node.js existed - certainly npm if it existed was pretty minor. Whereas now most engineers out there are writing JavaScript, right? If you're a new engineer coming out of a bootcamp or learning a new language, it would be a strange choice for you not to learn JavaScript and do that on the front end and the back end and then I'm full stack and yadda, yadda, yadda. So I think that has been interesting just because the JavaScript ecosystem has its own culture and way of doing things. You have millions of dependencies and often those are quite small and maintained by like a single person. Not fewer, big chunky open source projects. Like KDE is essentially a big umbrella project that probably has, I don't know how many active contributors it has, but it wouldn't surprise me if it's hundreds or thousands or whatever. And it's carved into a wide variety of pieces. Whereas I feel like open source in general has become a bit more decentralized and there's a lot more small projects of one or two people here and there.


Things have also moved away from the Linux world where it used to be. Like when I was at university, open source and using Linux were almost one to one, right? If you were a big C# Microsoft stack Windows type person, it would be relatively unusual for you to use any open source at all. But obviously over time, with GitHub and Microsoft acquiring them and Microsoft themselves getting a lot more into open source, it's like everything is open source now in some respect but what open source means has also changed. There used to be the assumption that open source projects are community run and maybe they're loosely affiliated with a company or whatever. In the earlier days of GitHub, if you looked at the repos with the most stars or whatever, Homebrew was often up there. Whereas now it's all like VSCode and next.js. Almost all the projects that are up there are ones that have major corporate backing basically. That makes open source a very different world and makes it harder to be an indie, volunteer-run project.


**Are there any other open source projects that you’re impressed by or watching?**


Ruby on Rails always continues to impress me. I'm using it again at the Workbrew startup that I'm building, and it’s the first time I've built a completely greenfield app from scratch. We’ve got a couple apps I guess, but one’s in Rails and one’s in Go and you know, Go is really good at what it does, but writing it doesn't make me happy in the same way Ruby does. There's been such an amount of time and effort and care and love that's gone into making it very pleasant for developers to use.


‍
