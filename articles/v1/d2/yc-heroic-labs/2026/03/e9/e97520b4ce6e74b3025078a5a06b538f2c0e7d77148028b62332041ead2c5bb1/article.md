---
schema_version: "1.0.0"
document_id: "e97520b4ce6e74b3025078a5a06b538f2c0e7d77148028b62332041ead2c5bb1"
company_key: "yc-heroic-labs"
company: "Heroic Labs"
source_id: "yc-heroic-labs-news-import-3563bf1285ab"
canonical_url: "https://heroiclabs.com/blog/pixel-flow-highlight-announcement/"
published_at: "2026-03-04T00:00:00+00:00"
first_seen_at: "2026-07-21T22:49:46.144164+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:7ee1f65a42472127d27524fe9c93ad52138e61a7071b4ced4878f298eb47e844"
---

# Heroic Labs Celebrates Pixel Flow, the Breakout Hit with 10M+ Players from Loom Games

Loom Games was one month old when the copycats started appearing. That’s how fast Pixel Flow moved: launched in August 2025, it created an entirely new puzzle sub-genre and captivated audiences around the world while most studios were still trying to reverse-engineer what made it work. Today, over 10 million players log in to blast their way through pixel art puzzles on little conveyor belts, making it one of the fastest-scaling mobile games in recent memory—and Heroic Labs is proud to power the backend that keeps them running.


## Scaling fast means scaling right


With millions of daily active players comes an enormous amount of backend pressure. At its core, Pixel Flow is a social experience: two daily competitive events, Fire Quest and Pixel Arena, run on 24-hour cycles and drive players back every day to chase rewards, climb leaderboards, and knock rivals down a peg. Keeping all of that running smoothly, reliably, and at scale is where Heroic Labs comes in.


Pixel Flow perfectly demonstrates the "easy to learn, hard to master" feeling


The foundation starts with **Nakama** , which handles authentication and player accounts, including social sign-in via Google Play Games and Apple Game Center. Nakama’s Cloud Save feature keeps player data synced across devices, so no one loses their progress when they switch phones.


Pixel Flow’s competitive systems are where Nakama really earns its keep. Both Fire Quest and Pixel Arena are built on Nakama’s **Tournaments API** , **Storage Engine** , and **Storage Indexes** , with custom logic written in TypeScript using Nakama’s **server-side runtime** . The two events differ in their scheduling and reward structures, but share the same underlying infrastructure. It’s a great example of what’s possible when you build on flexible components rather than rigid, off-the-shelf systems.


Pixel Arena and Fire Quest keeps players (like yours truly) coming back every day


## What’s next for Loom Games


Scopely, one of the world’s largest mobile publishers and the studio behind Monopoly Go and Pokemon Go, recently acquired a majority stake in Loom: a clear signal that Pixel Flow isn’t a one-hit wonder, but a franchise worth betting on. Congratulations to the entire team on everything they’ve built. Watching a small, focused studio break through like this, and do it with discipline and craft, is exactly the kind of story that reminds us why we built Heroic Labs in the first place.


We’re proud to be powering Pixel Flow, and we’re excited to see where Loom takes it from here.
