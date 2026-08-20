---
schema_version: "1.0.0"
document_id: "38c9ffd1badca0baf0ce3254c80dc586a33500a02f781a8cd9d1231a733d111d"
company_key: "yc-mux"
company: "Mux"
source_id: "yc-mux-atom-4708df60f240"
canonical_url: "https://www.mux.com/blog/fuertafit"
published_at: "2026-05-27T20:56:54.087+00:00"
first_seen_at: "2026-07-20T23:20:16.234471+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:79d499da90f6f8575e56d898d45b8fb5a574756cca4f0283d20699748bb0314b"
---

# How Mux helped Fuertafit make smarter product decisions

Published on


May 27, 2026 (2 months ago)


# How Mux helped Fuertafit make smarter product decisions


By[Stacy Fernández](https://www.mux.com/team/stacy-fernandez) • 5 min read •[Customers](https://www.mux.com/blog/category/customers)


---


**TL;DR:** After a two-hour outage and a billing dispute made staying with Vimeo untenable, Fuertafit's CTO switched to Mux. He found that the included Mux Data gave his two-engineer team something they'd never had: the visibility to make real product decisions from video behavior.


---


Óscar Ramírez was in a meeting in Madrid when the error messages started flooding in.


It was a weekday afternoon, when[Fuertafit](https://fuertafit.com/) ’s users across Spain finish work, change into their workout clothes, and open the app — and the worst possible time for a video outage.


The alerts told him videos weren't loading. They tried to reach Vimeo's support team, but their website was down too.


The outage lasted nearly two hours.


When Vimeo responded, days later, their question was why no ticket had been filed.


"I couldn't even go on your website," Óscar told them. "I couldn't even open a ticket."


He started looking for a new provider that week.


Fuertafit is a holistic Spanish fitness platform. Members get training plans with guided videos, nutrition guidance, and access to a community that extends offline. Around 3,000 users log in every day.


With Mux, Fuertafit’s two-person engineering team now gets reliable video infrastructure, and for the first time, the data to understand what is actually happening with their videos.


## A relationship that just wasn’t working


Fuertafit’s relationship with Vimeo had been fraying for a while.


Before the outage, Óscar had spent months trying to get costs under control. Vimeo's support couldn't help him, and their team wouldn't negotiate.


"We were going back and forth for a long time," he says. “They were not treating us well.”


## Why Mux Video was the right fit


In exploring Vimeo alternatives, Óscar knew it was not realistic for their two-person engineering team to rebuild their own video pipeline. He looked at AWS and ruled it out. Transcoding alone would eat up engineering time they didn't have.


He continued his search the way many developers do — he asked an AI research tool, and it steered him toward Mux. Unlike AWS, transcoding would be handled on Mux's end, not his. And when Óscar ran the numbers using[Mux's pricing calculator](https://www.mux.com/pricing/calculator?asset_type=ondemand&resolution=720p&quality=basic&upload=100&storage=500&delivery=1000&cold_breakdown=60%2C10%2C30&bitrate_in_mbps=3.33&static_renditions=0) he found the per-minute pricing gave him a level of predictability the bandwidth model didn’t.


“Even if we had negotiated a lower contract with Vimeo, it would still be more expensive than what we’d be paying with Mux,” he says.


## What Mux Data unlocked


"I'm in love with Mux Data," Óscar says. "It's everything that you need all in one place."


Mux Data is media-grade video analytics[included for free with Mux Video](https://www.mux.com/blog/understand-your-video-trends-introducing-mux-engagement) . Users get 100 days of data retention and can break down performance and quality by title, device, OS, and geography.


With Vimeo, analytics were too basic to drive decisions. He couldn't get the granularity he needed to connect what users were doing to anything actionable.


“Every time that I go there, it's like, 'Mmm.' This is not for professionals. This is for casual users," he says.


Fuertafit's workout videos run 25 to 30 minutes. Users rarely watch from start to finish — they skip, rewind, pause, and restart. With Mux Data, Óscar could see where attention dropped, where users looped back, and where they gave up entirely.


Decisions that used to come down to intuition — about editing style, pacing, and structure — now had data behind them. If users consistently rewound the same segment, maybe that section needed to be its own video. If a particular trainer's content held attention better, that was worth understanding.


"We can now use data for something as big as how to design a challenge or as small as how to edit a video to perform better,” he says.


The device-level breakdown was equally useful. Fuertafit ships on web, iOS, and Android. Being able to pinpoint which specific device or browser version was struggling (not just which platform) helped their two-person engineering team triage issues with precision they didn't have before.


## Migrating off Vimeo


Moving 1,500 videos took about a week. Óscar built the migration himself and ran automated servers in the background, pulling from Vimeo in 50GB chunks, re-encoding, and uploading to Mux. "I also like to sleep," he says, "so of course all of that was automated."


The trickier part was the player. Fuertafit's apps had been built around Vimeo's player and MP4 delivery. Adopting HLS meant working through compatibility issues across Safari versions, specific Android devices, and TV casting. Óscar tackled each one by watching Mux Data for patterns — which devices were failing, which regions, which network types — then shipping targeted fixes.


He credits the docs for keeping the technical transition manageable.


"I like working with an API and having documentation that is developer-friendly," he says.


Fuertafit ran Mux in parallel for about six months, testing platform by platform — web first, then mobile — before fully switching over. Since switching, Fuertafit hasn't had a significant outage.


Now Óscar's attention has shifted from infrastructure to what he can build on top of it. His team is working on a new product aimed at gym users: short videos built for checking form between sets rather than following a full workout. Mux Data will be how they decide what's working.


"Before, we were making decisions based on gut feel," Óscar says. "Now we go to the data. That changes everything."


## Written By


### [Stacy Fernández – Marketing Manager](https://www.mux.com/team/stacy-fernandez)


Journalist turned marketer who believes in storytelling, connection, and substance over fluff. My perfect day includes a park, several hours at a thrift store, and a sweet treat (ideally involving chocolate).


## Leave your wallet
where it is


No credit card required to get started.


[Sign up Sign up](https://dashboard.mux.com/signup)
