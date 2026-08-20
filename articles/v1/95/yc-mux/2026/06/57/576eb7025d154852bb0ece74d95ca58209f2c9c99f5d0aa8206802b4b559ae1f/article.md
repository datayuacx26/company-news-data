---
schema_version: "1.0.0"
document_id: "576eb7025d154852bb0ece74d95ca58209f2c9c99f5d0aa8206802b4b559ae1f"
company_key: "yc-mux"
company: "Mux"
source_id: "yc-mux-atom-4708df60f240"
canonical_url: "https://www.mux.com/blog/cutsio"
published_at: "2026-06-26T15:28:48.983+00:00"
first_seen_at: "2026-07-20T23:20:16.234471+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:788dda96ce4bff9a37359a1f43d991b2451d15632c8b74ac3ea6ed70f3b04ca7"
---

# How Cutsio is making massive video libraries searchable

Published on


June 26, 2026 (about 1 month ago)


# How Cutsio is making massive video libraries searchable


By[Stacy Fernández](https://www.mux.com/team/stacy-fernandez) • 6 min read •[Customers](https://www.mux.com/blog/category/customers)


---


**TL;DR:** Cutsio is a video search and delivery platform for production studios and filmmakers, built on Mux Video, Mux Robots, and Mux Player. When founder Rish Agarwal discovered Mux, it transformed Cutsio from an audio-only tool into a full video platform.


---


For production studios, footage piles up fast. No matter how organized the file system, pinpointing the exact moment you’re looking for — that establishing shot of a red house, an emotional beat from an interview, a bride's face at a specific angle — takes time. Time that would be better used editing instead of manually scrubbing through footage.


With[Cutsio](https://cutsio.com/) and Mux, hard drives become a searchable cloud library.


Type in exactly what you’re looking for, whether it’s literal like “red house” or abstract like “emotional scene,” and Cutsio pulls up relevant clips.


Rish Agarwal is an engineer turned award-winning wedding photographer and filmmaker. He created Cutsio to solve three core problems: collecting scattered videos in one place, making them searchable, and enabling easy sharing.


"Without Mux, nothing you’re seeing on Cutsio is possible,” Rish said.


## Entering the world of video


The original Cutsio only handled audio.


Rish wanted to build for video, but video is notoriously difficult at scale. To support it meant transcoding pipelines, expensive server costs, and engineering time he couldn’t absorb as a solo founder.


So he built around it, creating a platform that edited videos just by using the audio transcript to cut down, say a 20-minute video into 10 minutes.


One night, he was searching for answers to a specific technical question around HLS streaming and video codecs, when he came across a post by Dylan on the Mux blog. Rish went down a rabbit hole, eventually reading the entire Mux blog library (that’s more than 450 posts, folks) in one night.


Intrigued, he reached out to Dylan. Rish was initially skeptical, worried Mux couldn’t handle the raw camera files his users most commonly worked from. A single raw file can be 5GB per minute of footage, that’s 300GB for an hour of video. Most video platforms either can’t handle this file type or compress it to the point of affecting quality.


“Dylan told me one sentence that stuck: ‘Mux can process anything that's video’. The fact that he was so confident makes me confident as a customer,” Rish said.


Rish got to building. Within a month, Cutsio was a full video intelligence platform.


## How Mux Robots made better AI workflows


Cutsio’s workflow is simple. Users paste a link or upload a file directly and Mux ingests the asset. From there, Mux Robots takes over and automatically surfaces a summary, tags, key moments from the content, and chapters in the UI while the user keeps working.


“Mux Robots made my life 10 times easier,” Rish said.


"Mux Robots is fire and forget. I don’t have to worry about pulling stuff or waiting for jobs to complete. Mux Robots is happening in the background, and the UI is updating in real time," he adds.


Before Mux Robots, Rish had gone through two iterations of this pipeline.


First, he was feeding transcripts manually to his own OpenAI keys to generate summaries and chapters. When he discovered the[Mux AI SDK](https://www.mux.com/docs/changelog/mux-ai-release) , he started building with that. When Mux[Robots launched](https://www.mux.com/blog/mux-robots) , he quickly replaced those pipelines.


After a user uploaded explicit content Rish realized he needed to use Mux Robots' Moderate feature.


“Mux Robots already had moderation built in, so I didn't have to build it from scratch. It saved me a lot of time and energy,” Rish said.


## From raw footage to searchable moments


Once a video has a transcript, Cutsio indexes it so users can search using natural language, not just keywords. A search for "find all videos where I talked about LinkedIn" returns every moment where LinkedIn came up, directly or indirectly, across every video in the library.


Visual search goes a step further. Cutsio integrates with[Mixpeek](https://mixpeek.com/case-studies/video-editor-footage-search) to search video by what's actually happening on screen, not just what's being said. Search "two cars on a racetrack" and Cutsio returns the exact frames from hours of footage. Search "emotional moments" and Cutsio surfaces footage of crying, anger, comfort, disappointment — moments an editor might have forgotten were there.


Mux is the connective layer: Cutsio passes assets directly to Mixpeek through the Mux API, so nothing has to be downloaded or re-uploaded by the user. (Read[Mixpeek’s case study](https://mixpeek.com/case-studies/video-editor-footage-search) with Cutsio.)


## A professional viewing experience


For professional filmmakers, delivery is its own battle. A film can be technically flawless and still land badly if the client opens it in a broken video player or a cluttered Google Drive folder. The presentation is part of the work.


Cutsio's Collections feature is a shareable, theater-style page. More like a private digital screening room than just a file link.


Rish built this with Mux Player and customized it with[player.style](https://player.style/) (Sutro theme was his go-to).


"I wanted a player that looked beautiful and worked everywhere," he said. “Mux Player is :chef’s kiss:.”


He also built a mobile experience — a custom retro iPod-style scroll wheel that lets users navigate horizontal video on a vertical phone screen. Mux Player sits at the top; the scroll wheel below lets users seek through footage without switching to landscape.


## Built for filmmakers at every scale


Whether its solo wedding filmmakers or a production studio, the problems are the same: time gets eaten up in the editing process just trying to find footage. Cutsio combines searchable video libraries with a delivery experience that values beautiful design.


“Every filmmaker I talk to is facing the same problems. Mux is probably the only company able to solve it, from processing to transcription to delivery. People forget about delivery, but it's one of the hardest problems in video. Mux is the king of the whole ecosystem,” Rish said.


## Written By


### [Stacy Fernández – Marketing Manager](https://www.mux.com/team/stacy-fernandez)


Journalist turned marketer who believes in storytelling, connection, and substance over fluff. My perfect day includes a park, several hours at a thrift store, and a sweet treat (ideally involving chocolate).


## Leave your wallet
where it is


No credit card required to get started.


[Sign up Sign up](https://dashboard.mux.com/signup)
