---
schema_version: "1.0.0"
document_id: "5be54be34fa65bd08a8ce5c6e9967e31d0de0b66b955d5f45eca930e4340211a"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/stir-shaken-canada/"
published_at: "2023-05-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:1676cf64713bf503ab9d93256fe1d53dbaf5296eb900857ebe8a6e18ab7383fc"
---

# STIR/SHAKEN in Canada

We’ve[written](https://www.plivo.com/docs/voice/concepts/stir-shaken/) about the FCC’s mandate for US telecom service providers (TSP) to implement STIR/SHAKEN — two technical frameworks that attempt to authenticate calling numbers and measure trust in displayed caller names. We’ve been remiss at noting that Canada too has jumped on the STIR/SHAKEN train.


The Canadian Radio-television and Telecommunications Commission (CRTC)[Decision 2021-123](https://crtc.gc.ca/eng/archive/2021/2021-123.htm) “directs telecommunications service providers (TSPs) to implement STIR/SHAKEN to authenticate and verify caller identification (ID) information for Internet Protocol (IP)-based voice calls as a condition of offering and providing telecommunications services, effective 30 November 2021.” In other words, any Canadian carrier whose calls traverse IP networks in whole or in part must implement[STIR/SHAKEN](http://plivo-webflow.webflow.io/blog/voice-calls-stir-shaken) .


On May 31, 2022, TSPs were required to file their first post-implementation STIR/SHAKEN status reports with the CRTC. Reports are filed every six months.


## An end to call spoofing?


Unfortunately, STIR/SHAKEN doesn’t guarantee that no calls will be spoofed. Even if calls are made and terminated on IP networks, if they’re interconnected via time-division multiplexing (TDM) on the public switched telephone network (PSTN), STIR/SHAKEN attribution information won’t be carried over. And calls from numbers outside the US and Canada, from countries that haven’t implemented STIR/SHAKEN, won’t carry attestation information either.


Still, it’s a start, and we expect more accurate attribution information on a higher percentage of voice calls as carriers do a better job of implementing the protocols.


While STIR/SHAKEN technology can uncover call spoofing, it doesn’t stop it or reduce the number of times it occurs. That will take additional standards that have yet to be written. For now, consumers can rely on smartphone apps like Truecaller, Hiya, and a host of others that let people identify incoming calls as possible spam and potentially automatically block them or send them to voicemail.


[Plivo](https://www.plivo.com/) has been compliant with STIR/SHAKEN regulations since they rolled out, so you can be sure that any calls you make using Plivo’s[Voice API](https://plivo-webflow.webflow.io/voice) and[phone numbers](https://plivo-webflow.webflow.io/phone-numbers) rented from Plivo will have the highest possible levels of attestation
