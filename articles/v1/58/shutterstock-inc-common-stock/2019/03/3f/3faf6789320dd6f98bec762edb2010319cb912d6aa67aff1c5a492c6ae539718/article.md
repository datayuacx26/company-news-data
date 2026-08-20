---
schema_version: "1.0.0"
document_id: "3faf6789320dd6f98bec762edb2010319cb912d6aa67aff1c5a492c6ae539718"
company_key: "shutterstock-inc-common-stock"
company: "Shutterstock Inc."
source_id: "shutterstock-inc-common-stock-rss-4c794ed87dc5"
canonical_url: "https://tech.shutterstock.com/2019/03/12/shutterstock-sprint-demo"
published_at: "2019-03-12T04:00:00+00:00"
first_seen_at: "2026-07-20T23:18:13.615263+00:00"
fetched_at: "2026-07-28T22:26:51.183990+00:00"
content_hash: "sha256:9cf469fe82bc0926a30641408faedd606361ee947e19850e1db4bfeafc7d357d"
---

# The Shutterstock Sprint Demo

One of my earliest memories at Shutterstock is sitting with 60 people crammed into a 20-person meeting room and one developer presenting what they released that sprint. I’ve forgotten what that demo was about now, but clearly recall that everyone applauded at the end. In that moment, the unmistakable enthusiasm, congeniality, and love of building sold me on how cool Shutterstock was.


The sprint demo, at its core, is a series of presentations from interdisciplinary product engineering teams showing what they’ve been working on in the latest sprint. It’s a ritual that’s existed at Shutterstock since 2010 (a few months before I joined), and has given me and many others a profound view into how the company has grown over the years.


## Why Have a Sprint Demo?


The architect of the original sprint demo at Shutterstock, Wyatt Jenkins, gave two big reasons why they started it:


**Wyatt Jenkins**


> Show working code and really push ourselves to get something live to production


> Get past the powerpoint slide product manager stuff and hear raw thoughts from engineers about what it took to build something


Another early luminary at Shutterstock, Matthew Smith, said that


**Matthew Smith**


> It helps emphasize that tech is a fundamental part of the business


## The Nuts and Bolts


Shutterstock is a global company with teams across a dozen locations around the world. Today, our demos are well-oiled operations spanning continents and are centrally coordinated from our headquarters in the Empire State Building. We’ll go a bit into the details about how we get it done.


At the end of every sprint (which is universally 2-weeks in length across all teams), we gather to demo what our teams have built. As we enter the last few days of a sprint, each team asks, “what have we built that’s worthy of demo-ing?”


The team that’s in charge of the demo is comprised of a few people including an administrative coordinator, an A/V technician, and an MC to keep the show running and the audience engaged during transitions between demos. This cohort is responsible for all the logistics around demos.


On the day of the demo, the coordinator sends out an email to the product and engineering organizations with a linked spreadsheet ([see a demo sheet here](https://docs.google.com/spreadsheets/d/1WV4Cdr1Lzu8P8Jek4v0EWtfih9zIXXicqpI7YSht5NE/edit?usp=sharing) ) where every team interested in presenting can sign up. Teams can then add links to what they will be demoing (e.g. web applications, videos, etc.) as well as surfacing any special needs the coordinating team needs to accommodate.


Our HQ has a beautiful cafe space that is equipped with ceiling-mounted projectors and full A/V setup (i.e. built-in speakers and wireless microphones). The projectors are set to mirror the demo laptop on the podium.


Since over half of Shutterstock employees are located outside of NYC, we make our demos as geographically-inclusive as possible. To do this we take a two-pronged approach:


1. We run a Google Hangout running on the demo laptop that all remote presenters join; this allows us to display each presentation in our main video feed
2. That video feed is then piped in to a private uStream broadcast since Google Hangout can’t handle the hundreds of people who dial-in to watch the demo


I’m no A/V tech, though, so let’s hear the real details from Drew Taylor who heads up all the A/V work:


**Drew Taylor**


> We have an audio/video routing matrix that allows us to route and control the inputs from an iPad. The laptop that is used for the presenters is plugged into the wall ports in the cafe which run via ethernet. This connects into that A/V matrix, and it works the same way for the camera. The video and audio signals then output from the matrix to our live stream computer using an application called LiveStream Studio that broadcasts to a uStream channel. Each uStream channel has a unique URL and Stream Key; once those are entered into the corresponding fields you push the big "GO LIVE" button and the stream becomes active.


30 minutes before the start of demo, all presenters verify that their Google Chrome profiles on the demo laptop are ready to go. We use Google Chrome for its ability to manage multiple profiles whereby each team can have easy access to various internal environments and assets required for their demos. This also prevents teams from accidentally closing others’ tabs. One of these profiles is dedicated to the shared spreadsheet and serves as the jumping-off point for teams to open their profiles when the time comes. Currently, the demo laptop has about 50 different profiles corresponding to just about every engineering team at the company.


Immediately before the start of demo, the administrative coordinator sends out a company-wide email reminder. Everyone gathers in the cafe or joins the uStream, and remote presenters join the Google Hangout.


The MC comes prepared with some decent jokes and/or recent company announcements, and tries to instill a general sense of excitement before the first demo starts. Sprint demos need time limits, though, so we cap them at 4 minutes per demo to keep the meeting under an hour.


The sprint demo is a core tradition at Shutterstock and to understand why it’s become an even more essential part of our culture since 2010, Shutterstock’s Founder and CEO[Jon Oringer](https://tech.shutterstock.com/author/?Jon%20Oringer) has this to say:


**Jon Oringer**


> The sprint demo is an essential tool to share knowledge across the company, not just in the product and tech teams, but also for sales and marketing to really understand the work being done every sprint to make our customers lives easier and their experience with Shutterstock better.


And so each week, teams step-up and present demos of what they’ve been building. The whole company watches and everyone walks away with a deeper understanding of what each team is doing, how our platforms are changing, and, ultimately, how we’re improving the lives of our customers and contributors.
