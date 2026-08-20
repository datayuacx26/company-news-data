---
schema_version: "1.0.0"
document_id: "f4ff342fad4787af0321870822026579446cf34c3e8c859ab5a51b43344ec4ae"
company_key: "yc-runway"
company: "Runway"
source_id: "yc-runway-news-import-fe61d44f24da"
canonical_url: "https://www.runway.team/blog/do-you-know-what-your-mobile-team-is-shipping-right-now"
published_at: null
first_seen_at: "2026-07-22T12:25:56.018278+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:e677dd7b9ff3e3934cab987205f8e8a7306f8a7c167a7e6066a84b8aab559c6b"
---

# Do you know what your mobile team is shipping right now?

What’s the status of the release? How about now? Okay, how about now? Hmmm, what about now? Did you merge your PR yet? Is that fix from last week out to users now? Is that new feature live now? Did QA find any problems with the last build? Has marketing provided the new App Store metadata and screenshots you’re waiting on? What was our crash-free rate for the last release? What about the one before that? Would we be comfortable doing a faster phased rollout for the next update?


Every release requires dozens of questions to be asked and (eventually) answered. Those answers are apt to change throughout, and many of the individual questions may need to be asked multiple times by many stakeholders – engineers, QA, product, CX, leadership, marketing, you, etc. – as they try to get the details they need to move forward with their own work.


This is untenable. Do you know what your mobile team is up to right now? Does your mobile team know what they’re up to right now? Does anyone?


## The black box of mobile release management


By default, virtually no one has full visibility into how an individual mobile release is run or even really how it’s going. Maybe a couple of mobile wizards sitting up in their mobile control towers overlooking everything have full knowledge of what’s happening, but if one of them leaves or your team grows too much or if they go insane from[all the stress](https://www.runway.team/blog/avoiding-release-anxiety-part-i-testing-automation) that managing your releases is causing them — then you’re going to have a bad time.


Regardless of what I just said about mobile wizards above, it’s incredibly unlikely that any one person — unless that one person is the only mobile developer in the entire company and they also test everything on their own while creating all the media / copy for your app store listing — ever really fully knows what’s going on with a release.


Why not? The sheer[number of tools](https://www.runway.team/blog/your-mobile-teams-knowledge-and-release-tooling-is-way-too-distributed) involved means virtually no one is likely to have access to everything. And if someone somehow does have all that access then at best they’re struggling to keep up with it all via at least 20 open tabs and 10 different Slack conversations.


To stay fully informed about the progress of a release, someone would need access to your:


- CI/CD platform
- VCS provider
- Series of fastlane scripts that have been written to automate away some of the process but that someone still needs to manage and run
- Automated testing tools
- The Slack or Teams channels where all the important discussion of each release is likely to take place
- Confluence, Notion, or wherever you’ve written the 75-step document detailing everything that needs to be done before a release can go out
- Jira, Linear, or whatever project management software your engineers and product folks use happens to be
- Asana, monday.com or whatever project management software your marketers and designers use happens to be
- Direct knowledge and updates from QA’s progress on testing (usually through tickets assigned via the above)
- Knowledge of how each developer is progressing with their contributions to an upcoming update (usually via the above, assuming each engineer is keeping Jira updated with their progress in a timely manner, which is assuming a lot)
- App Store Connect or Google Play Console
- Whatever tool you use to manage localized translations if your app is in more than one language
- Whatever tool marketing or product is using to write copy for the newest update
- Whatever tool marketing or product is using to take screenshots and create media for the newest update
- Sentry, Crashlytics, or whichever tool you use to track crashes and app health
- Datadog, New Relic, or whichever tool you use to track app performance
- Amplitude, Mixpanel, or whatever tool you use to track customer activity within the app
- App Store page, to monitor reviews for your newest version as they come in
- PagerDuty or whatever tool you use to sound the alarms when something has gone wrong


This isn’t necessarily everything, but you get the idea.


It’s highly unlikely you have access to all these tools to keep up with how things are going. It’s additionally unlikely that there is a single person in your org has a paid seat for all of these tools and even if there is someone who is worthy of having money shelled out for them to access every tool, it’s even more unlikely they know what to do with these tools when they’re in there That means they have to slow down and deal with the distraction of poking their way around in software they’re not fully familiar with any time they have to look at these tools.


So to repeat the question from the title:


## Do you know what your mobile team is doing right now?


No, you don’t. Your mobile team probably doesn’t even know what they’re doing right now (at least not as an entire group). They, as well as you, likely know what the plan is, but there’s a very good chance none of you truly have a good handle on progress at any given time.


If you find yourself asking “what’s the status of the release? What’s the status of the release? What’s the status of the release?” twenty times a week does not count as knowing what your mobile team is doing right now. Then you might have no idea what’s going on.


If mobile engineers are regularly asking the question “I know I’m late, but can I still get my update into the release?” then that mobile engineer might not know what the rest of the mobile team is doing right now. If this question ever really needs to be asked at all, then the whole team might not know what it’s up to at any given time.


If your entire mobile release cycle is full of secrets that can only be revealed via a 40-page Confluence doc or a series of scripts that were written by an engineer who left the company six months ago, then you definitely do not know what your mobile team is doing right now.


Note: I didn’t mean to write that in a Jeff Foxworthy “well, you might be a redneck!” style joke format, but now that I’ve done so I can’t change it.


No one knows what the mobile team is doing right now. And if no one has that full picture in their head, how can your releases ever be improved or any release cycle run without a ton of stress?


## These problems are compounded by the nature of mobile development


There’s a good chance that[mobile is a second class citizen within your organization](https://www.runway.team/blog/mobile-teams-are-second-class-citizens-but-not-on-purpose) , because it’s a second class citizen in many organizations.


Not because you hold any sort of ill will towards mobile software engineering or the concept of phones, but because it’s only natural that installing binaries to individual phones would end up off to the side with its own processes when most other folks in your org are (probably) working on a web app whose features mobile is separately building upon, evolving, and adapting for handheld use.


Since your mobile team is doing its own unique thing that stands kind of separate from the rest of the org, a lot of people who may have an ownership stake in how the mobile app performs have no real idea of how anything works over there. There’s a bug, why can’t you get the fix out right now? It’s good to test, but why are we testing every release so thoroughly? Why does this all take so long?


A broad toolchain with access that may be limited to only a few individuals means that not many people understand all the dark magic that goes into making an organization's mobile release cycle run, and even fewer people can peek in to see how all the individual steps are going to get information on how a release is going.


As a result of this and all the rest of the above, a team’s mobile release management process ends up in a black box that is completely disconnected from the rest of the ways the org does things.


## Open up the black box and look inside


It is often just accepted that this is how things have to function. No one thinks any of this is great, but it’s not like shipping updates to your app is supposed to be a fun day hanging out at the beach. It’s work and work means dealing with a lot of annoying little hassles. You just deal with it and move on.


But dealing with it and moving on means you slow down the rate at which you ship updates and features. It means increasing the sort of stress and frustration for your engineers and other team members that can lead to burnout. It means running your org way below anything close to peak efficiency.


To improve on this and ensure the team is always aligned on progress, you at the very least need:


**Proactive communication**


Updates on release progress need to be accessible and visible in the same place every single release cycle. How might you do this?


Create a Slack channel for each release where all discussion for that release is kept and done. This gives every stakeholder somewhere to turn first if questions arise and ensures the answers to all those questions are public so that anyone with the same question can see the answer. Creating a separate channel for each release keeps the conversation from just being one long messy history and provides an easily searchable history of what happened with each and every release.[DoorDash does this](https://doordash.engineering/2023/11/28/how-doordash-manages-mobile-releases/) for their own release trains.


On top of this, you also are ideally providing greater visibility into the process.


**Full visibility into release progress**


For proactive communication to exist, someone needs to stop and create the communication or another person needs to publicly pester them about it. That’s not ideal, as it causes delays and distractions as people are completely in the dark until they ask questions or see an update.


But how can you provide full visibility into a process that is spread across all the many, many tools mentioned at the top of this post? Well, not to toot our own horn too much, but a platform like Runway brings your entire release workflow under one roof where every step of the release process (regardless of which tool it’s happening within) can be managed by the people who need to manage it and viewed by everyone else.[DoorDash also uses](https://doordash.engineering/2023/11/28/how-doordash-manages-mobile-releases/) Runway for their release trains.


If it’s not Runway, then another mobile release management platform of some kind can provide you with a level of visibility info and control over your releases that will save your team hours of work every release cycle while making releases easy and boring.


The status quo of mobile releases being hidden away in black boxes where no one ever really has a full picture of what’s going on is not sustainable unless you want to ship fewer features, stress out developers, and generally fall behind where you want to be. And who wants that?
