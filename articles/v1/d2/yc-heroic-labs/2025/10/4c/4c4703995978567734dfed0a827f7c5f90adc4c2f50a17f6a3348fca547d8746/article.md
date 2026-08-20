---
schema_version: "1.0.0"
document_id: "4c4703995978567734dfed0a827f7c5f90adc4c2f50a17f6a3348fca547d8746"
company_key: "yc-heroic-labs"
company: "Heroic Labs"
source_id: "yc-heroic-labs-news-import-3563bf1285ab"
canonical_url: "https://heroiclabs.com/blog/spatial-case-study/"
published_at: "2025-10-14T00:00:00+00:00"
first_seen_at: "2026-07-21T22:49:46.144164+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:84f6593e44896d3976f00792b4096bfb4156483e06818f3cd7773ab735633499"
---

# Animal Company: From Prototype to VR Breakout with Half-a-Million Daily Players

Animal Company, developed by Wooster Games, a subsidiary of[Spatial](http://spatial.io/) , has turned speed, timing, and community-first design into a standout success in VR’s latest growth wave. With Heroic Labs powering their backend, they’ve scaled social-first multiplayer gameplay to half-a-million daily active users while delivering weekly updates and maintaining the stability players expect from a live service.


## From a Five-Week Build to Global Traction


VR was at an inflection point when Waldo Bronchart, Head of Engineering, and his team started working on Animal Company. After years of slow adoption, Meta’s decision to open up the Quest ecosystem to a broader group of external developers resulted in “Robloxification” of VR. It created a fertile ground for fast-moving, player-driven experiences that weren’t born from careful planning but rather rapid iteration. Spatial jumped on the opportunity and set up a separate nimble entity, Wooster Games.


> "Building for VR isn't the hard part. The challenge has always been timing. We launched at the right moment, when both players and platforms were ready." - Waldo Bronchart, Head of Engineering at Wooster Games


Animal Company’s journey began with an intense five-week development sprint to prototype and launch on Quest. The initial version was barebones, but the core game loop, which was expressive, social, and perfect for TikTok, proved instantly sticky. In a market without strong ad networks or attribution pipelines, the team leaned heavily on organic virality and a cadence of weekly updates driven by community feedback.


Animal Company's vibrant VR environment featuring social gameplay


Operating as a small, autonomous studio within Spatial, they embraced a reactive development philosophy. Backend updates often shipped hours before client updates to ensure smooth data migration, allowing them to move fast without compromising stability.


## Building VR LiveOps on Nakama


For its first six weeks, Animal Company operated without a backend, leading to lost progress and limiting monetization. As the game proved its product-market-fit, Waldo and his team knew they needed a solution to manage accounts, game economy data, and authentication across platforms.


After evaluating options, Nakama stood out.


> "Transparency in pricing, open source, and the fact that our team already knew Go made the decision easy. Being able to inspect implementation details and repurpose code for custom features, such as Meta Quest authentication, allowed the team to integrate quickly and confidently." - Waldo Bronchart


**Within four weeks, Animal Company launched purchases, inventory management, and player account systems.** They also built a custom admin panel on top of Nakama’s API to manage accounts, grant items, and integrate voice moderation for player safety. Nakama’s storage engine, Friends and Groups systems, and web sockets became the backbone of their social gameplay, all critical in a VR market where multiplayer-first is the expectation.


> "Nakama lets us focus on features that matter to players instead of fighting backend limitations. It's what makes our crazy weekly update pace possible." - Waldo Bronchart


Animal Company's in-game cosmetic bundles powered by Nakama's inventory system


## Scaling Together


At Heroic Labs, it has been our privilege to work alongside Waldo and the Animal Company team during one of the most exciting moments in VR’s evolution. Helping them move from a rapid prototype to a live-service hit, while delivering the backend reliability and social features that keep players coming back, has been a collaboration we’ve deeply enjoyed.


We’re proud that Nakama has played a role in enabling Animal Company to ride VR’s new wave, and we look forward to supporting them in pushing the boundaries of what’s possible in social, multiplayer VR gaming.
