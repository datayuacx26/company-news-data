---
schema_version: "1.0.0"
document_id: "8718fb284624c01828ae4e4465d90421bd78b7c88ce0f9e27438a2bfacb71172"
company_key: "yc-onlook"
company: "Onlook"
source_id: "yc-onlook-rss-a3426f90edb2"
canonical_url: "https://onlook.substack.com/p/simple-bug-tracking"
published_at: "2024-05-21T16:50:09+00:00"
first_seen_at: "2026-07-27T04:07:26.124598+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:15423e8f2f0cfd2e1e759b745d3ca70520c2d97909d1684c7c5b4edc926998e0"
---

# The simple bug tracking system that works for our two-person team

# The simple bug tracking system that works for our two-person team


### 🦋🪲🐞🪰🦋🪲🐞🪰🦋🪲🐞🪰


[Daniel Farrell](https://substack.com/@danielfarrell)


May 21, 2024


I love scaling up systems – There’s nothing quite as satisfying as knowing something you hacked-together is working and helping the team rather than creating too much process. It’s part of the beauty of really thoughtful software. When it feels familiar, work becomes effortless.


But making familiar software takes a ton of testing.


It’s part of the reason why the best product teams like Apple and Linear take time to release things – the first draft of a software product will inevitably run into a lot of wonky edge cases that cause stuff to break, and you need a team of people to try to simulate as many of those edge cases as possible before you send it out to hundreds of thousands of people. One way you could simulate the wonky edges is to use a tool like


[Synthetic Traffic](https://synthetictraffic.com/) , but another is to just get as weird as you can with the app you’re building.


Just tracking bugs is a challenge in of itself, regardless of the tool you use. Couple that with the need to fit it into the minefield-discussion of how your team should manage tasks (Coda? Linear? Jira? Notion??), and you’re looking at a really gnarly and enraging debate. It’s the difference between sipping a nice glass of fresh squeezed orange juice and getting a vat of vodka-Red Bull-pickle juice-sriracha dumped on you – it’s aggressive, sour, spicy, sticky, and a non-ideal Tuesday night.


There are infinite combinations of how people track bugs, but I wanted to share our system we use today because it may inspire small teams to focus on what will get more things done today, rather than find the perfect system to scale before you really need to.


We originally started working in Linear, as one should when preparing for scaling, but had a realization with our workflow: As much as we love Linear, organizing all of our bugs was actually getting in the way of getting the work done.


We needed to go simpler.


Kiet and I communicate asynchronously in Slack and Google Docs, so we needed a source of truth for bugs that was super easy to understand and organize. Ideally, the source of truth wouldn’t have to be another thing we need to check or log-into or have to remember where to find.


We started in Slack with our #bugs channel, where we came up with a color system using four bug emojis that communicate what prioritize at a glance.


:butterfly: = Feature request / larger UX improvement.


:beetle: = Small improvement / smaller UX suggestion / cosmetic adjustment


:ladybug: = Medium issue <-- default if you don’t know what to rate


:fly: = Shit / broken / crashing / really bad stuff


Why did we assign each of these roles to those specific emojis? It’s mildly intuitive, and if you look closely, it’s a color spectrum from good to bad.


-


🦋 Butterfly / Blue – Good! Improvement, suggestion etc.


-


🪲 Green Beetle / Green – Small improvement / cosmetic adjustment, not too critical.


-


🐞 Ladybug / Red – Bad / needs attention.


-


🪰 Fly / Brown – Really bad / poop / critical to be addressed.


What does the bug marking system look like in practice?


For example, let’s say that Kiet put out a new release, and I came across a bug while testing. I’ll drop a new header section in our Bug Tracking Google Doc for the date (an easy proxy for the likely version we’re testing) and make a checklist of what I come across. At the beginning of each point though, I’ll add the right bug emoji to make it easy to organize at a glance.


Kiet or I will often batch bug requests into groups, because the Butterflies (feature requests) can be put off for some time while Flys (Critical issues) need to be addressed much sooner.


And because this is done in a Google Doc, we can discuss, and re-batch bugs as needed depending on how long we think they could take and how urgent they may be. The Google Doc also gives us a long record of feature ideas we’ve come up with in the past, so as we plan what to release next, we can go Butterfly hunting with CMD+F and start a new list of features we want to prioritize.


We have a #bugs channel in Slack where we originally were tracking these, and most of our conversation has shifted from the #bugs channel to the Bug Tracking Doc for asynchronous things, but the #bugs channel is a helpful place to track conversations without crowding the doc’s sidebar. Also, because Slack is great for sharing screenshots and files, it can be better for providing richer context than Google Docs.


As an additional boost to our bug tracking, I set up a Zapier automation that will send a bug to our document if someone reacts with a bug emoji on a message.


The Google Doc is our source of truth, and it’s an easy to format checklist for us to work through, while we use the #bugs Slack channel for active back-and-forth discussion on bugs. Because each bug in the Google Doc has a link to the Slack message, we can continue to reference the discussion while keeping the original checklist clean with the general idea of the bug, and none of the noise.


If you’re just starting out on a project, my biggest advice would be to go simple with your tools. Every second you spend trying to build out a fancy system is time and energy spent away from either building a product or talking to people who you’re building for.


Paul Graham wrote about the idea of “Playing house” in his essay


[Before the Startup](https://paulgraham.com/before.html) and it’s a continued reminder that systemizing things or getting too complex with how you set up your processes is just not important in the early days.


We are big fans of these tools that help teams work together effortlessly, yet until we bring on more people outside of the founders, we need to keep things lean. If you’re asking yourself the question of “where do I put this ticket?” too often because you set up some project management tool that has way too many features, you may need to go simpler.


You’ll have plenty of time to systemize your process later, but for now, try catching bugs in an easier format.


Thanks for reading Onlook! Subscribe for free to receive more posts on how we run our startup
