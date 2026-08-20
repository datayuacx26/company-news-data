---
schema_version: "1.0.0"
document_id: "5d97ace68ab41399368b624dcf1456504ac72fd0438c284b79dff81d6336bb77"
company_key: "yc-heroic-labs"
company: "Heroic Labs"
source_id: "yc-heroic-labs-news-import-3563bf1285ab"
canonical_url: "https://heroiclabs.com/blog/i3d-nakama-integration/"
published_at: "2025-12-16T00:00:00+00:00"
first_seen_at: "2026-07-21T22:49:46.144164+00:00"
fetched_at: "2026-07-28T22:24:57.117171+00:00"
content_hash: "sha256:19e56cd18a813c255b7c51d5bed3a78d9f992e9fc6df82c57c7f2989595d6ed4"
---

# New Integration: Connect Nakama to i3D.net's Global Server Fleet

We’re excited to announce a new[Fleet Manager](https://heroiclabs.com/docs/nakama/concepts/multiplayer/session-based/) integration between Nakama and[i3D.net](https://www.i3d.net/) , the veteran game hosting provider behind titles like *Rocket League* and *The Division* .


This open-source plugin connects Nakama’s matchmaking and social features directly to i3D.net’s ONE API, giving studios access to 60+ global points of presence across bare metal and multi-cloud infrastructure.


## How it works


When players are matched through Nakama, the integration automatically allocates game servers from i3D.net’s global fleet, waits for readiness confirmation via the[Arcus protocol](https://docs.i3d.net/game-hosting/game-integration/index/index-1) , and notifies players with connection details.


Key capabilities include:


- Automatic server allocation through i3D.net’s ONE API
- Region-based filtering for rapid matchmaking
- Server lifecycle management
- Support for hybrid bare metal and cloud scaling (AWS, GCP, Azure)


The plugin is available now on[GitHub](https://github.com/i3dnet/nakama-i3d) . For studios already using i3D.net for hosting, this integration eliminates the need to write boilerplate glue code between matchmaking and server allocation. For those exploring dedicated server options, it opens up access to proven infrastructure without building custom orchestration.


Check out the[full integration guide](https://heroiclabs.com/docs/nakama/guides/concepts/i3d-integration/) to get started.
