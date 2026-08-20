---
schema_version: "1.0.0"
document_id: "79a7623df1f7e3b7d0b131b2f9c7adecb2be360c26e225f625d661e0031b0fe9"
company_key: "yc-dayflow"
company: "Dayflow"
source_id: "yc-dayflow-rss-659ae9f20797"
canonical_url: "https://www.dayflow.so/blog/screen-recording-battery-impact/"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:40.227507+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:776104de05b07ed0b43e78982335f81f35fd3d2faf53174923e98d934c893495"
---

# How much battery does a screen-recording time tracker use?

The phrase “screen recording” sounds expensive: video encoding, spinning fans, an hour off your battery. A screen-recording *time tracker* is a different animal, because it does not record video.


## The numbers


[Dayflow](https://www.dayflow.so/) captures about one frame every 10 seconds - 6 frames a minute, versus the 3,600 a real screen recording would take. In practice on Apple silicon that costs:


- **~100MB of RAM** - about a tenth of one browser tab’s worth these days
- **Under 1% CPU** while recording
- **Battery: minutes per day, not hours** - the capture itself is a rounding error next to your display’s backlight


The honest caveat is AI analysis. Turning frames into a readable timeline takes a language model, and where that model runs decides the cost:


- **Cloud or bring-your-own-key mode:** the analysis happens elsewhere; your Mac only uploads periodic snapshots. Battery impact stays negligible.
- **Local mode (Ollama / LM Studio):** the model runs on your Mac in short bursts every few minutes. On an M-series Mac this is a brief, visible-but-small spike - the trade for nothing ever leaving your machine. On battery, expect a modest cost during those bursts, still nowhere near video-call territory.


## Why this matters when picking a tracker


Automatic trackers run all day by definition, so their idle cost is the whole ballgame. When you compare options ([the full comparison is here](https://www.dayflow.so/best-time-trackers-mac/) ), the resource question splits the field more than the feature lists do: app-usage loggers like ActivityWatch are extremely light because they only read window titles; screenshot-based tools like Dayflow are nearly as light because stills are cheap; anything doing continuous video capture or constant cloud sync is the tier to be suspicious of.


A useful rule: if you can tell from your fan or your battery menu that a passive tracker is running, it is built wrong.


## Check it yourself


Activity Monitor will settle it in thirty seconds: install[Dayflow](https://www.dayflow.so/) (free, open source, macOS 14+), let it run through a normal morning, then sort Activity Monitor by CPU and find it near the bottom of the list. The source code that keeps it that way is[public](https://github.com/JerryZLiu/Dayflow) .
