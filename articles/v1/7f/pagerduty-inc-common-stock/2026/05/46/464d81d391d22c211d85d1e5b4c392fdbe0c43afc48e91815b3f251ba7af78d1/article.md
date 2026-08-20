---
schema_version: "1.0.0"
document_id: "464d81d391d22c211d85d1e5b4c392fdbe0c43afc48e91815b3f251ba7af78d1"
company_key: "pagerduty-inc-common-stock"
company: "PagerDuty Inc."
source_id: "pagerduty-inc-common-stock-rss-b3e2d0380f72"
canonical_url: "https://www.pagerduty.com/eng/channels-are-free-why-we-gave-every-incident-its-own/"
published_at: "2026-05-21T19:35:28+00:00"
first_seen_at: "2026-07-20T23:19:31.841537+00:00"
fetched_at: "2026-07-28T20:50:31.360725+00:00"
content_hash: "sha256:cde1a067d95bf7bf7ffde8eba07cd1665576a97cfe41ce9206e07f1977dd44a1"
---

# Channels Are Free: Why We Gave Every Incident Its Own by Rich Lafferty

*“Channels are free!”*


At PagerDuty, that’s our standard response to “Should we create a Slack channel for this?” Sure, channels can multiply if not careful and require tending, but we prefer and encourage visibility and searchability over keeping the channel list short. Make a channel, make the channel public, and archive the channel if it’s no longer useful.


Why, then, were we still using a single channel, **#major-incident-room** , to coordinate response to all of our major incidents?


With the release of our Dedicated Incident Channel feature in our Slack integration, it seemed like a good time to tell the story of the switchover to our dedicated channel.


## The single-channel era


For most of the nine years I’ve been at PagerDuty, every major incident – that is, things that require response from multiple teams, customer communications, a capital-I Incident – used the same channel to coordinate the response. When I started it was called **#incident-war-room** until we got tired of war metaphors and renamed it to **#major-incident-room** . It probably even dates back to when we used Hipchat.


It worked well, we thought, and it made a lot of things easy: there was only one place to watch, leaders and adjacent teams could stay in one channel and know what was going on, bystanders would occasionally recognize a pattern and share an insight, there were no setup costs or automation to maintain, and we had a long history of what had happened when. It was optimized for simplicity and cross-incident visibility, but there had always been some issues and scale problems.


## Where it stopped scaling


The biggest and oldest issue we had with our single-channel approach was **concurrent incidents** . It turns out you can’t ask a second major incident to wait until the first one completes. We worked around this gracelessly – **#major-incident-room-2** , naturally, but on the thankfully rare occasions when we had two incidents, it was a mess. Our responder requests had **#major-incident-room** hardcoded, as had our engineers’ habits. The first incident’s incident commander would spend half the time shooing people off to the other incident. PagerDuty’s Slack integration would put all of the updates about both incidents into the main channel. But concurrent incidents were rare, so we carried on.


But even when major incidents politely avoided happening at the same time, everything in one place kept getting in the way.


**Incidents had no clear beginning or end.** Finding the start of an incident meant a lot of scrolling. Finding that particular incident you cared about meant wading through several more, doubly so if there were multiple incidents on the same day. Searching for an incident you remembered would turn up that incident, and a similar one from six years previous.


**Post-incident work had nowhere to go.** Chatting in **#major-incident-room** after an incident had been resolved was taboo, because people would see it light up and think a new incident had started. (Anyone who forgot that unwritten rule would find their message decorated with a variety of “nervous panic” emoji.) Our post-incident process asked people to create a new channel to coordinate the incident review, but that would usually happen a while after the incident ended, especially if the incident was overnight or on a weekend. As a poor stopgap, we had an **#incident-followup** channel, but that was also shared across all incidents.


**Importing Slack messages to Jeli was fraught.** When we acquired Jeli in 2023, everyone was excited to start using a great new tool for incident reviews! We then quickly found ourselves load-testing our new friends’ software by repeatedly importing a whole decade of major incident history into one incident review. Yes, Jeli offers choosing a start and end date for imports, but remembering to do that and getting the dates right was extra cognitive load.


## The dedicated-channel journey begins


The advantages of a single incident channel were clearly being overwhelmed by the challenges. But change is hard, and even with PagerDuty’s incident workflow automation right there, we resisted it longer than we should have, in hindsight. There was no single event that pushed us over the hump, but the friction kept getting in our way and the imagined costs vs. benefits had clearly flipped in favour of dedicated channels. We tried dedicated channels, with their creation automated through an Incident Workflow.


After a couple of major incidents, the situation was clear: dedicated channels worked extremely well, but it wasn’t actually about dedicated channels *versus* a single channel. They optimize for different things. **The dedicated channel becomes the container; the old channel gets demoted to a feed.**


A long-running single major incident channel **optimizes for awareness** . The company knows an incident is happening, what’s impacted, and the state of the incident. Lurkers can see the incident develop and decide if they should jump in to provide an insight people are missing. It’s obvious when the situation is all clear.


A dedicated channel **optimizes for the full incident lifecycle** . Detection, response, resolution, debriefing, and review are one full loop. Post-incident work matters as much as response does, and having it all in a single container wins. You stop asking “where is everyone talking about this now that the incident is over?” because you know where everything about the incident lives. Automation and agents that work on a single incident can easily consume what they need. And if there are multiple incidents at the same time, they all just work the same, and responders on one can completely ignore the other.


In the end, we ended up with both. Major incident response happens in dedicated channels, so we can keep talking about the incident in the same place even after the formal response has ended. The **#major-incident-room** channel lives on, but only as a feed with one message per incident: “An incident started, here’s the Slack channel”. Subsequent status changes and internal status update messages follow that message in a thread. Everyone knows that an incident is going on, and where to go for details, and the details are easily available for people who care about that particular incident.


The change was so easy that I regret not changing much, much sooner. This is just “channels are free” applied to incident response.


## Two details worth stealing


Automation was an important part of our dedicated-channel switchover. When we first rolled it out, we used Incident Workflows to make the magic happen. In doing so, we learned a couple of important lessons:


**The channel naming convention is the index.** We track incident review work in Jira, one ticket per incident with linked follow-up tasks. As a result, we refer to incidents by their Jira ticket number: “When’s the incident review for IR-49?”. At first, our automation created Slack channels using the PagerDuty incident number, and so we ended up with two naming conventions at once. The system degraded to “ask someone in DM if **#major-incident-28481** is the right one”. Now channels are named after the Jira ticket: **#major-incident-IR-49** . Pick something stable, unique, memorable, and searchable, and use that as the incident reference across all of your systems.


**Archiving is what makes this work.** “Channels are free” only holds if dead channels don’t accumulate; otherwise each channel is just a little bit more noise in the sidebar. Our policy: channels get archived by hand when the incident review is complete, or automatically 30 days after resolution through another incident workflow. If an incident takes longer than 30 days, someone will need to manually unarchive the channel. History stays searchable, and the active channel list stays clean.


## Getting started with dedicated channels


At the time, using dedicated channels in Slack meant assembling it yourself out of Incident Workflow building blocks. Powerful, but a little complicated. Now dedicated channel creation is built into PagerDuty’s Slack integration, and you can adopt dedicated channels without having to build it out of Incident Workflow actions.


If you’re still using a single incident channel, that makes it a great time to try dedicated channels per incident. Our incident processes run much more smoothly, and we wish we’d switched long ago. Incidents are stressful, and the work continues long after the incident is marked resolved.


**Channels are free. Give each incident its own.**
