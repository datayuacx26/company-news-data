---
schema_version: "1.0.0"
document_id: "b1182824f9e3f364439b76dec533289546f6661b8fa94502be741cf4cb7d4266"
company_key: "yc-infield"
company: "Infield"
source_id: "yc-infield-news-import-3dde9d4b3c10"
canonical_url: "https://www.infield.ai/post/once-a-maintainer-ed-waisanen-and-nate-papes"
published_at: null
first_seen_at: "2026-07-25T09:33:54.576450+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:7f7d1ad6cff8a2af24bd0f20cdd527ebdd0266566259f02ce2d07dd6e223f2ac"
---

# Once a Maintainer: Ed Waisanen and Nate Papes

Welcome to Once a Maintainer, where we interview open source maintainers and tell their story.


This week we’re talking to[Ed Waisanen](https://www.linkedin.com/in/ed-waisanen/) and[Nate Papes](https://github.com/papes1ns) of the[Gala project](https://docs.learngala.com/) , an open source research and education platform out of the University of Michigan. Ed oversees the ongoing development of the Gala platform and advises users on instructional design, multimedia production and use, and the development of interactive data tools. Nate is a developer for[Atomic Object](https://atomicobject.com/offices/custom-software-development-ann-arbor) , a software agency based in Ann Arbor, and works closely with the Gala team.


**Ed, let’s talk a bit about your personal background and how you came to be involved with Gala.**


Ed: Sure. I was a Master’s student at the University of Michigan, at what was then called the School of Natural Resources and Environment. I was studying environmental policy but also informatics, and I got involved with this radio show called “[It’s Hot in Here](https://hotinhere.us/) ” on the student-run radio station,[WCBN](https://www.wcbn.org/) . It was a lot of PhD students and faculty, and also folks in the community who were working on the environment and sustainability. And out of that came this focus on case-based education. Essentially, using case studies as a way to get at these intersecting issues. They drew some inspiration from the business school and other disciplines where case studies are traditionally used.[Rebecca Hardin](https://hardin.seas.umich.edu/) , the faculty member who was holding down the radio station, got involved in writing an internal grant to create like 50 case studies that would become known as the[Michigan Sustainability Cases](https://www.learngala.com/catalog/libraries/michigan) .


The initial plan was to make some sort of Wordpress plugin that would house these case studies. Because traditionally, you know, a case study would be some sort of PDF that’s just a written document that gets passed around. But it didn’t feel impactful and current and like they were accomplishing their goal. And around this time a student named[Cameron Bothner](https://github.com/cbothner) , who’s now at Shopify, was very involved in the radio station and studying linguistics and computer science. He turned out to be a very gifted Rails developer.[Pearl Zhu Zeng-Yoders](https://www.linkedin.com/in/pearlzhuzeng/overlay/about-this-profile/) was another early contributor. So the idea was to build some sort of tool that would be multimedia-rich and bring the kind of stuff that was happening at the radio station into these case studies. Every case study would have a podcast episode associated with it. And that was sort of where I came in, because I had experience as a podcast producer. For example let’s say we have several conversations for a case study with somebody in charge of managing water quality somewhere. Let’s use that audio and tie it together to the writeup.


So we had this initial grant for the Michigan Sustainability Cases, which was supposed to last five years or so. That was winding down, and Cameron had left to go to Shopify. We were very lucky in having someone who was the original developer for the project be very capable but also very passionate. And that’s when we approached Atomic Object and Nate to come in.


**Nate, can you talk a little bit about your experience with Rails and how you came to be involved with the Gala project?**


Nate: So I went to Central Michigan University, and did computer science all the way through. I knew that’s what I wanted to do from a very early age. And I had a campus job where we helped the recreational staff, mostly with Python apps. That was my introduction to understanding anything about the web.


Then my first professional job out of college was at a Rails shop. They made online wellness campaigns - helping companies drive their employees to do wellness-related activities, like getting in 10k steps. I didn’t really want to work for a big company and it was a short drive, so I was like OK, I’ll do an internship there and if you want to hire me, cool. And I ended up really liking it and liking Rails a lot. I was like, why would anybody not choose this? I learned so much and got to work with really senior people.


Then Covid happened, and I needed a change, so I started doing my own consulting. And I ended up getting recruited at Atomic Object, which is an agency based in the Midwest. And one day the Managing Partner was like hey, we know you like Rails. We have a little treat for you. And he showed me Gala and said, “Would you want to work on this?” And I was amazed. I looked at the code and I was like oh my gosh, this is actually like really good code. I would love to work on this.


**What did the growth of Gala look like over this time period?**


Ed: At the beginning it was this very structured thing where students would apply to get funding for a case study, and we would essentially give them a mini-grant. They’d create their case study and I’d interface with the student teams and we’d work with Cameron to put them up. But over time we started asking for more robust tools to do the sort of authoring we wanted to do. And at a certain point, I don’t know who said it, but it was like, “Can we just make this available for anyone to create something?” And once we did that, people kept finding us and coming up with something cool to do.


One that I’d like to highlight is the[OCELOTS](https://www.learngala.com/catalog/libraries/ocelots) . They’re a group of tropical ecologists that use Gala for teaching tropical biology and conservation. It’s a research coordination network, which brings together instructors to teach them how to create modules that are similar to the goals of the Sustainability Cases, i.e. more multimedia-rich, more engaging and interesting, but still open educational resources. So now we have this relationship with several of these groups where they're doing their thing and we're kind of embedded with them and can say now Gala needs to be able to do this or that.


**Is there any kind of top down roadmap? How do you manage feature requests from the community?**


Ed: I think Nate bounces with excitement on this sort of thing. We don’t have a roadmap per se. We have started tracking issues a lot better recently. I would say it’s one of our goals because all of this is sort of managed in my head, weighing our needs versus where we have knowledgeable people, and our funding, and I’m always trying to synthesize what’s the best focus that will serve multiple needs at once.


**This is the thing with open source, right. Do you have an estimate of how much of your time you’re spending on Gala, per week or per month?**


Nate: The number one goal originally was just to keep Gala online. So one day a week I’d work on Gala for just whatever improvements needed to be made, bug fixes or whatever. And then we were behind on Heroku updates and stuff, so I started doing more work there, and eventually started doing some feature work. As I got more into it I really got into the vision, the promise of Gala and the people involved.


Ed: I balance my time with instructional design support and documentation and managing how we actually package these things up to engage more students. We have CS students that come through who work on either Gala itself or on tools that integrate with Gala, like data tools and things like that. Recently we’ve also gotten some UX design students. And they get pretty energized about it. I find we can get them pretty excited about education and they like having worked on an open source project. Many of them have used it in the classroom.


**Where do you see Gala going from here? That could be in terms of features, users, or use cases, or it could also be in terms of contributors. In other words, what do you hope for Gala?**


Nate: From my personal perspective, I’d like to see Gala grow at a sustainable pace. I’d like to see more people be aware of it. Really smart people are writing really good content and the platform is good in and of itself. I think it was ahead of its time in a way, like how you can add really rich media to the learning modules.


Ed: As you know, we’re moving our base institution to Notre Dame. And one of my hopes is that because we’ve always had people from a bunch of universities clicking around the site, you wouldn’t immediately say this is a University of Michigan product. I’d like to formalize some of the governance and be for transparency but also be set up so that more people can get involved. So students can come in and learn and also contribute. The idea is you're a learner, but you're also an author, and you're also maybe an instructor - bringing that kind of ethos to the infrastructure that's running the thing.


Another thing is we've been engaging with[SEEKCommons](https://seekcommons.org/about.html) , which is another NSF-funded network of folks who are big on open science and open metadata. We've also got some folks from[Wikidata](https://www.wikidata.org/wiki/Wikidata:Main_Page) engaged. So my hope is to become more interoperable, nicely indexed, and lean on these other open source projects to get the stuff we want to do done.


‍
