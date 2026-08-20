---
schema_version: "1.0.0"
document_id: "6d77b53d9c63317764959b70b69babb837762a4a6078f3658c751abded3a28c2"
company_key: "yc-mux"
company: "Mux"
source_id: "yc-mux-atom-4708df60f240"
canonical_url: "https://www.mux.com/blog/disciple"
published_at: "2026-07-09T15:41:32.645+00:00"
first_seen_at: "2026-07-20T23:20:16.234471+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:cc39f44d34b00f1880fb3bd7e587996dc628d6fbdf3640fc0f798dc74561f6bd"
---

# How Disciple modernized its video stack, lowered costs with Mux

Published on


July 9, 2026 (19 days ago)


# How Disciple modernized its video stack, lowered costs with Mux


By[Stacy Fernández](https://www.mux.com/team/stacy-fernandez) • 6 min read •[Customers](https://www.mux.com/blog/category/customers)


---


**TL;DR:** Disciple, a white-label community app platform serving 400+ communities and more than two million users, migrated from Cloudflare Stream to Mux. With one engineer, they moved 1.3 million minutes of video in just a few days and came out on the other side with significantly lower costs, a more innovative product, and better visibility via Mux Data.


---


For over a year, Andrei Rafai noticed a steady flow of Mux emails in his inbox announcing cost optimizations, data updates, and AI features and workflows.


As CTO of[Disciple](https://www.disciple.community/?utm_term=disciple%20media&utm_source=google&utm_medium=cpc&campaign_id=21794655061&ad_group_id=169599458538&ad_id=716374930790&keyword_id=kwd-872527886520&ppc_keyword=disciple%20media&gad_source=1&gad_campaignid=21794655061&gbraid=0AAAAA9lviCZ4qfpIyy5zbXmIsk4gzUNwb&gclid=CjwKCAjwuanRBhBSEiwAY5y6Vz_xoAnVwOUglq4ItE1e6gnCvxjK32S51_Qm_eKf0YWVWbZJ_SDzDxoCjbcQAvD_BwE) — a platform with more than 2 million users that helps creators and brands bring their content, courses, and community into their own branded, no-code app — the updates piqued his interest. Video is at the core of Disciple’s product, with many users posting videos more often than images.


When Disciple’s Cloudflare Stream contract came up for renewal, Andrei was ready to make a change. Mux’s innovative nature, plus the responsiveness from the Mux team, gave him confidence that moving over could provide a fresher video experience and better data insight without pulling significant resources from his small engineering team.


Disciple ultimately migrated over a million minutes of video in a matter of days with just one engineer. The switch brought a significant reduction in costs.


## Why Disciple left Cloudflare Stream


Cloudflare Stream was stable, but Andrei felt product updates were rare, making the platform feel stale.


“Cloudflare is making big bucks in other places, providing the whole internet with different tools and being in some ways a backbone. For a company like that, video can end up lower on the priority list,” Andrei said.


"I kept an eye on Mux and the rate of development compared to Cloudflare,” Andrei added.


Retrieving analytics was particularly frustrating. Cloudflare Stream didn't surface the metrics Andrei actually needed. For example, to get a basic usage figure like viewed minutes, he had to build a dashboard himself, connected to Cloudflare’s API.


"If I'm paying quite a lot for a product, I don't want to have to work to pull numbers," he said.


Then there was the cost of it all. Cloudflare Stream charges based on storage regardless if the videos are being actively watched. As is the nature of a social app, Disciple’s community members watch content when it’s new, but once it’s pushed down the feed, views drop off. Cloudflare Stream didn’t offer cold storage, so they were paying full price for unwatched content.


## What made Mux the right fit


"I'll be honest, the primary driver was the cost. For us, Mux is much cheaper compared to Cloudflare,” Andrei shared.


For a small company aware of their line items, it was worth the shift. Part of that cost savings comes from Mux’s[Automatic Cold Storage](https://www.mux.com/blog/automatic-cold-storage-ga) , which automatically tiers infrequently watched content to cheaper storage costs.


The developer experience and product momentum sealed it.


Mux's documentation was clear, the dashboards surfaced what he needed out of the box, and the hands-on support from his account manager and a Mux engineer instilled trust that a migration this big would be manageable.


And it was clear to Andrei that the product was moving forward in a way that would benefit his team and Disciple’s users.


“You're innovating at a rapid pace. That's one of the reasons I jumped ship. We're getting onboarded on a fresher platform with newer features," Andrei said.


## A lean migration and a cleaner stack


The migration of more than 1.3 million minutes across 400+ communities was handled by one engineer in just a few days.


Andrei was impressed by his account manager and support engineer’s responsiveness. They answered technical questions quickly as they came up, which kept the process from stalling.


"It's a testament that we were able to migrate an insane amount of data with just one engineer working for a couple of days," Andrei said.


Mux handles all of Disciple's on-demand video on their exclusive content and social media-style feeds. For playback, they went with Mux Player since it’s designed to work seamlessly with Mux infrastructure and helps them avoid potential problems that could come from maintaining a third-party player.


"Why not use the player built for that tool specifically, when you know that people are going to look after it?" Andrei said.


Live streaming also runs through Mux. Disciple supports RTMP for users who want to broadcast to multiple platforms simultaneously using tools like OBS.


Mux Data powers their internal dashboards, giving Disciple visibility into played minutes across all 400 communities. Each community is on Disciple’s tiered plan with video limits, and that data is how the team makes sure no one is exceeding them. It also doubles as a health metric: consistent video engagement is a reliable signal that a community is active.


The day-to-day experience, as Andrei describes it, is mostly silent.


"It's pretty much hands-off. Everything runs smoothly,” Andrei said.


## What's next for Disciple


Andrei has already started mapping out how[Mux Robots](https://www.mux.com/blog/mux-robots) — Mux's AI product that automates video workflows like transcription, summarization, and moderation — fits into Disciple’s roadmap.


Transcription and summarization would unlock video search. Translation would let Disciple serve multilingual communities that currently have no good solution. Chapters with descriptions would add navigation to longer videos, and moderation is also on his radar.


Andrei is still working on the rollout. The current thinking is to offer Mux Robots-powered features as an optional add-on at the community level, enabled based on each community's needs rather than switching them on platform-wide.


"Every now and then, there will be a community asking for captions or translations," he said. "Having the ability to say, ‘We can just turn this on for you is quite exciting.”


Andrei added that Mux Robots appeals to him because it’s an out-of-the-box solution that doesn’t take time away from his engineers.


"Mux has innovative cost-cutting solutions, LLM features, brilliant and responsive support. What more is there to ask for?” Andrei said.


## Written By


### [Stacy Fernández – Marketing Manager](https://www.mux.com/team/stacy-fernandez)


Journalist turned marketer who believes in storytelling, connection, and substance over fluff. My perfect day includes a park, several hours at a thrift store, and a sweet treat (ideally involving chocolate).


## Leave your wallet
where it is


No credit card required to get started.


[Sign up Sign up](https://dashboard.mux.com/signup)
