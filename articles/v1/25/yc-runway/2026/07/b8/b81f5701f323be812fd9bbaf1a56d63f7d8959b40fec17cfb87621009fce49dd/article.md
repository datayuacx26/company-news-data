---
schema_version: "1.0.0"
document_id: "b81f5701f323be812fd9bbaf1a56d63f7d8959b40fec17cfb87621009fce49dd"
company_key: "yc-runway"
company: "Runway"
source_id: "yc-runway-news-import-fe61d44f24da"
canonical_url: "https://www.runway.team/blog/mobile-releases-are-broken-for-engineers-why-are-we-ok-with-this"
published_at: null
first_seen_at: "2026-07-22T12:25:56.018278+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:9bebc5d35e4ea4e4c7032c8b530feabcf51f694eea28f2125de85f27594d2cf5"
---

# Mobile releases are broken for engineers. Why are we ok with this?

There’s a reason almost every mobile engineer dreads their turn running releases. In fact, there are several reasons that they dread it. Releasing mobile apps is a broken process, barely held together by duct-taped checklists and hand-crafted scripts that bleed engineers of their time and companies of their money.


Messy, chaotic, time-devouring releases slow down work on actual features. Without the sort of infrastructure support that other software engineers usually receive, mobile engineers are left having to build bespoke solutions themselves and then spend hours every release remembering how they did things last time — troubleshooting fastlane scripts, navigating changes to App Store Connect and the Play Console, deciphering out-of-date documentation — while actively wishing they could just get back to their actual work.


Why are we ok with this?


## Mobile engineers’ time is not respected


You may not realize it, but mobile engineers are probably treated as second-class citizens within your org. This isn’t because anyone thinks less of mobile engineers, it’s just that the complex (and mundane) administrative and technical work required to get an app shipped and into the hands of customers is a huge time sink — that management knows is a huge time sink — but it’s treated as a minor task because *this is just how it works* . What can you do except write some fastlane scripts and hope for the best?


The work that goes into releasing an app — wholly apart from all the work that goes into building that app — is significant. Each release likely takes several hours of hands-on management, with additional time required to monitor the health of the app after launch and get any necessary hotfixes out.


In your own org, maybe one dedicated person or a small platform team takes time away from work they’d rather be doing to put the time into all your releases. Or maybe every engineer on the team[takes a turn](https://www.runway.team/blog/the-whys-and-hows-of-setting-up-a-release-manager-rotation) doing so — with their turn coming around so rarely that they never get enough reps to comfortably follow a pattern, and they have to waste enormous amounts of time getting back up to speed *every* time their turn comes up.


This isn’t just a waste of time for them, it’s a waste of time for their whole team and even their whole org. A couple hundred hours of engineering time poured into release processes every year is a couple hundred hours that would have been better spent on building features that customers care about.


Teams will naturally try to speed things up by conjuring up checklists and automations, but that doesn’t actually solve the problem because…


### Release checklists are long and usually get longer


When it is an engineer’s turn to run a release — whether it’s every other week or every three months — they have to take the time to think through and remember all the steps they need to follow. To successfully do this (or at least somewhat successfully do it), you need to write everything down in a nice list that people can follow. Taken on its own that doesn’t sound so bad, but this isn’t a simple five-step list that you can just read over and mark off — this is both a technical process and one that requires coordination with all the engineers who expect to ship code as part of the release.


Regardless of how many steps it has to start, the release checklist is guaranteed to grow alongside your org. Bigger teams mean more stakeholders who care about what happens during a release and who must give their signoff before anything can go out. A more mature org with a larger customer base means quality has to tighten up, turning every release into a high stakes event — since problems like a delayed launch or a spike in crashes can cause damage to the org’s bottom line.


> “Captains were finding it difficult to get through the full release checklist. I knew there had to be a better way to minimize risk and support the team long-term.”


Sean Pollock
Senior Engineering Manager, Wealthsimple
‍


### Mobile release toolchains are overly fragmented


It’s a running joke that every software engineer needs 30 tabs open, in four separate windows, to manage a release: eight Jira tabs, a few Confluence tabs, some CI/CD tabs, a whole window full of App Store Connect and/or Play Console tabs, some docs with release notes ready to be edited and copy-pasted, a folder with two new screenshots to upload, Slack open with a dozen channels and ten conversations needing updates, Sentry and Datadog nearby for keeping a close eye on performance. And a partridge in a pear tree.


Even the most well-written, easily followed checklist in the world cannot solve most problems when an engineer has to turn their attention up, down, and all around during a release. Jumping between a million tools is naturally going to introduce friction to the process and slow down the release manager’s work.


### There’s too much context-switching and cognitive load


Release managers often have product work to do on top of managing the release; work that may even be key to a feature that is going out as part of the new version. These require completely different tools (which means adding even more tabs and windows to the ones noted above) and different ways of thinking about the work.


Getting into the flow of either set of tasks becomes nearly impossible, upping stress and making mistakes much more likely — which then generates additional work down the line. Six hours spent jumping between release tasks and feature work can easily become an additional six hours fixing issues that were caused by your attention being split in the first place.


Most engineers recognize that good engineering needs deep focus time, something that's nearly impossible to get when you're a release manager. It[takes a toll on developer happiness](https://www.runway.team/blog/how-to-lighten-your-mobile-teams-cognitive-load) because engineers don't feel like they can do good work while simultaneously making sure the release goes out without a hitch.


If the negative impact of this kind of context switching is even considered at all from an org perspective, it’s usually only to say, “well, what can you do?” alongside maybe some very mild rebalancing of the development work the release manager is handling as part of the sprint.


## The complexity of mobile engineering is not acknowledged


Shipping an iOS or Android app is at least as complex and involved as shipping any other piece of software ([or more?](https://www.runway.team/blog/why-mobile-releases-are-a-silent-killer) ), but mobile teams often don’t receive the kind of critical infrastructure around distribution and releases that other platforms do. Mobile infrastructure and tooling is often an afterthought (or ignored entirely), even in organizations where mobile apps drive a significant portion of the business. And while some larger, mature orgs have come to recognize the need for dedicated mobile infrastructure support and formed dedicated mobile platform teams to meet this need, supporting releases isn’t always in their scope of influence.


This leaves mobile teams in the difficult position of having to personally plan, build out, and troubleshoot the infrastructure that allows them to release apps, at the same time they’re releasing those apps without any problems (how hard could it be?).


### Mobile engineers need better support for managing release infrastructure


Maybe it’s because Apple and Google make apps seem so simple (lol) by providing stores where apps are distributed, but the complex work that goes into getting the app into customers hands is treated as janitorial admin work at best.


While setting up (and later troubleshooting) fastlane scripts or creating bespoke automations from scratch may be a fun hobby for someone, these are not usually the best uses of a mobile engineer’s time. It takes them away from their area of expertise, delays their usual work, and slows down development of features that users care about.


Why is it OK that mobile engineers are left to figure all this out for themselves while other parts of the engineering team get access to actual domain experts in infrastructure and DevOps?


> “Many of our release managers were frustrated, because our release process was pretty complex. Knowledge sharing wasn’t consistent, and some people were less familiar with our tooling than others — sometimes they’d struggle with a particular step they didn’t understand. Ultimately this meant quality gates and health checks were also inconsistent.”


Guilherme Pereira
Staff Android Engineer, iFood


### Mobile engineers are either too involved or too removed from the details


Very often, engineers either take their own turn as release managers and follow a multi-part checklist that can break down if they make a small mistake, or the release process is locked away in a black box that belongs to a dedicated release team and the engineers have no idea what’s happening at all.


As we’ve now reiterated throughout this post, the idea that engineers can just context switch between development and release tasks without it being a drag on their own work and the org’s bottom line is fantastical. Tradeoffs are being made, time is being unnecessarily wasted, and they have to spend way too much time focused on mundane detail.


But the idea that the release should happen without the engineering team's involvement at all is arguably just as fantastical. The closer an engineer can be to the end result of their work (seeing when and where bugs crop up, how customers are responding, how their colleagues are contributing to releases) the more they can help shape and improve the product they’re building.


### Mobile engineers don’t feel their impact


Separating engineers from the release process — either through convoluted checklists and scripts they run through when their turn comes up every four months, or by having a release manager who handles everything — makes them one extra step removed from seeing the impact of their work.


It’s not so much that involvement in the details of the process itself is so important, but more that they have easy access to key details of what’s happening with the release of each new update and feature.


All this information is out there, it’s just spread across a dozen different places and there’s no way to easily understand the overall state of things. If the team had a single source of truth it’d be easier for every engineer (even when they’re NOT running the release) to feel ownership of reviews, post-release monitoring, and in-cycle blockers since it’s all available in one place that anyone can keep any eye on.


## Confusion and chaos is common enough that it just feels normal


We don't refer to how most companies do their mobile releases as 'confusing' or 'chaotic' because we've accepted all of the above as the price of admission, and we've moved on. That can be understandable in some cases, like in a tech startup where everyone wears several hats and you end up building workarounds that you plan to improve and fix as you go.


> “Engineers were unhappy during releases because it was easy to get confused about the status of everything. Sometimes this led to mistakes, and team leads needed to keep a close eye on things.”


Alex Branti
Android Developer, Route


The problem is that mobile releases don’t generally get simpler as companies grow, but instead become more complicated, turning into increasingly complex Rube Goldberg machines that you have to be very careful about touching because the whole thing might just explode. Companies try to solve this by giving one person control of the machine, or by writing detailed instructions on how to manage the machine. But that doesn’t solve the central issue in that you’ve built the entire infrastructure of your mobile releases around a delicate process — one that includes many components that are unnecessary, but nevertheless can’t be removed because they may break a different component somewhere along the line.


So what can mobile teams do to fix these problems? We’ll answer that question in our next post. (Surprising spoiler: That answer is not just to “use Runway”... even though using Runway is a pretty good fix for all the issues noted above.)
