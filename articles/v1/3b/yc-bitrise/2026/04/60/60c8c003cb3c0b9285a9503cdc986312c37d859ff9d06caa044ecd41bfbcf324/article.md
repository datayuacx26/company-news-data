---
schema_version: "1.0.0"
document_id: "60c8c003cb3c0b9285a9503cdc986312c37d859ff9d06caa044ecd41bfbcf324"
company_key: "yc-bitrise"
company: "Bitrise"
source_id: "yc-bitrise-news-import-b747fb40f1b3"
canonical_url: "https://bitrise.io/blog/post/q-a-changing-the-game-for-creators-how-substack-doubled-mobile-build-speed-with-bitrise"
published_at: "2026-04-30T00:00:00+00:00"
first_seen_at: "2026-07-24T03:43:06.304736+00:00"
fetched_at: "2026-07-28T21:25:33.541420+00:00"
content_hash: "sha256:a11249b9328ce1232ac00e9ffa8773b66ce0ba4f5f1491bf06b0ab28033d80c6"
---

# Q&A: Changing the game for creators: how Substack doubled mobile build speed with Bitrise

> Before diving into the Q&A,[read the full case study](https://bitrise.io/old/case-studies/substack) to see exactly how Substack achieved 40-50% faster builds, 75% less complexity, and 100%+ increase in test coverage for iOS and Android.


When Substack first launched in 2017, the company set out to give writers a better business model, built on subscriptions and direct relationships with readers. Since then, Substack has expanded into multi-format publishing across text, audio, and video, while building powerful tools for community and discovery, for creators, writers, and thinkers of all kinds.


The company’s app, launched in 2022, is central to Substack’s mission of building a “ *new economic engine for culture* ,” providing new ways for creators to connect with their growing communities. As the app has evolved, the infrastructure behind it has scaled too, with[Bitrise](https://bitrise.io/platform/bitrise-ci) supporting Substack’s mobile build and release workflows.


Here, we talk to Substack’s John Malatras, Engineering Manager, and Alex Sullivan, Senior Mobile Developer, about building a stronger mobile foundation with Bitrise and what it takes to deliver the experiences creators and subscribers now demand.


## \[Q1\] Substack has come a long way in the last few years. How has the launch of your mobile app changed things?


**John Malatras:** When I first joined Substack almost five years ago, it was really just a newsletter and email distribution platform. Since then, we’ve evolved into an app that helps creators and their audiences connect in new ways. You go to the app store, download it, open it up, and you’ve got that direct connection to your favourite content right there on the go. It also allows you to sample other people’s work that you might not otherwise have come across. It’s definitely a much more immersive experience for subscribers now, and for creators, it’s giving them a lot more ways to reach more people and build stronger connections. **‍**


## \[Q2\] With video and social playing a bigger role in the app, how is that shaping your mobile strategy?


‍ **John Malatras:** Ultimately, the vision for Substack is to be the place where creators can have a direct relationship with their audience, and where people pay them directly. That applies across the board, to writers, podcasters, and video creators. Everyone already consumes these types of content on other platforms, so to compete with what’s out there, the bar for our app has to be really high.


‍ **Alex Sullivan:** I think from a mobile standpoint, it means the scale of what we’re trying to do with the app is much bigger. Where it used to be mostly written text, now across the app we’re dealing with video, audio, rich images, on top of all the written content that we’re known for. So the scale of what we’re building on mobile has increased dramatically.


## \[Q3\] What challenges did you face with mobile CI before Bitrise? **‍**


**Alex Sullivan:** We were effectively juggling multiple CI solutions across iOS and Android. We built the apps on Bitrise, ran lint on Android, and used Buildkite for unit tests and formatting. That setup was pretty awkward. When something went wrong, we had to check multiple places to figure out what happened. On top of that, our Android build times kept getting longer. And as we use a merge queue system, those slow builds were blocking other developers waiting to get their changes through.


## \[Q4\] What drove the decision to consolidate everything on Bitrise?


**John Malatras:** Our setup at the time was starting to cause us some issues. On the iOS side, the release of Xcode 15 created a lot of issues for us with Xcode Cloud. We actually had to turn off our UI tests temporarily as a result. On top of that running multiple tools was expensive, so we were keen to bring costs down a bit. It was actually Bitrise who reached out to us proactively and suggested consolidating everything there. I think that definitely gave us the nudge we needed to move iOS over. And as I’d used Bitrise before, both at a previous company and at Substack, I was a pretty strong advocate for moving everything there. **‍**


**Alex Sullivan:** On the Android side, I’d already played around with Bitrise Build Cache for Gradle, and thought using Bitrise for our unit tests would be a good next step. It also felt like there was plenty of room to grow into more of the Bitrise ecosystem like Gradle caching, Android UI testing, and more along those lines. **‍**


## \[Q5\] What was the experience like migrating to Bitrise?


**John Malatras:** The experience was really positive. The team provided a complete “white glove service” which was really awesome. Ben Boral in particular, went above and beyond. As well as creating the pipeline and setting up any special conditions, he even took the time to fix and optimize all sorts of unrelated things with our setup. He was always so supportive and super proactive on Slack, it made the whole experience much easier for us.”


‍ **Alex Sullivan:** I totally agree with John, the experience was great. We were also working with a monorepo, so I was a bit worried at the start that it would make migration complicated. But the team, and Ben in particular, guided us through the whole process in a really smooth, approachable way. Everything was clear. We always knew exactly what was happening and when, and how to keep improving our setup in the future. It was just a really excellent guided process. **‍**


## \[Q6\] What were the first changes you noticed with Bitrise?


**Alex Sullivan:** When we switched over to Bitrise, we noticed an immediate improvement in Android build speed, which was great.


‍ **John Malatras:** Our build speed on iOS nearly doubled just with the change of machines. That was probably the most immediate thing everyone noticed. And because we’d had our UI tests turned off due to the Xcode Cloud issues, being able to turn those back on right away was a pretty big win too. **‍**


## \[Q7\] Are there any other standout benefits you and your team noticed?


**John Malatras** : Having a mature, mobile-focused platform was really helpful. It lets us run all of our CI/CD in one place, including custom Fastlane steps, Slack uploads, GitHub releases, and tagging. Basically everything we needed to do. We’re a pretty lean team, supporting an app used by millions, so we don’t have a lot of bandwidth to spend on CI/CD. Having a CI that just works is honestly the best thing. **‍**


**Alex Sullivan:** Looking ahead, we felt like there were a lot of areas we could grow into within the Bitrise ecosystem, things like Gradle caching and UI testing on Android. There was also the benefit that most of the mobile devs were already familiar with Bitrise and had used it before. It kind of moved things, at least on the Android side, out of generic DevOps tools to a specialist mobile solution.


## \[Q8\] How important was it to partner with a mobile-specialist CI?


**John Malatras** : Mobile can be a challenging space to work in. On a lot of broader CI/CD platforms, if mobile support is there, it’s either an afterthought or a technically challenging plugin you have to work around. But as Bitrise is focused on mobile it makes everything easier. We know if we get a new Xcode version, it might be tricky to work with, but we now have the confidence Bitrise can handle it. **‍**


## \[Q9\] You mentioned UI testing was pretty painful before Bitrise, what’s different now?


**John Malatras:** We probably added about 100% more tests overall with Bitrise. On iOS, we’d effectively started from zero after being forced to turn our unit tests off, so it was an infinite improvement.


**Alex Sullivan:** Moving to Bitrise enabled us to add UI tests in a way that would’ve been really challenging previously. For example, now when we run a hackathon I can write a bunch of tests and spend around 20 minutes on the CI setup, instead of the three weeks it sometimes took before just to get an emulator up and running. Ultimately I think Bitrise has enabled us to take that next step into UI testing with much more confidence and far less maintenance overhead than we would have had to deal with elsewhere. **‍**


## \[Q10\] As you look to the future, what’s next on the agenda for your team?


‍ **John Malatras** : I’d say we’re really just getting started. We’ve got[three or four mobile roles](http://substack.com/jobs) open right now, and I expect that to grow quickly. So having a really solid mobile foundation in place is critical, and that’s where we are today.


In terms of the relationship, it genuinely feels like Bitrise is part of our team. The team is always ahead of everything, dropping us updates in Slack, sharing new features, and just checking in. It makes us feel confident and well looked after, like things are being thought about and we’re not on our own. **‍**


## \[Q11\] What’s been the biggest personal win for you with Bitrise?


**Alex Sullivan** : It’s definitely made CI feel less like a black box. Before Bitrise, if something went wrong or I was confused, I’d usually have to ping a systems person to figure out what was happening. With Bitrise, I understand the system from start to finish, and so I can just figure things out myself.


‍ **John Malatras:** I would agree with Alex on that. I barely have to think about CI now, which is all I want. There are days where we might want to integrate or update something in Bitrise so I might think more about it then. But most days, I don’t think about it at all, which is kind of a dream.


[Read the Substack case study](https://bitrise.io/case-studies/substack)


‍
