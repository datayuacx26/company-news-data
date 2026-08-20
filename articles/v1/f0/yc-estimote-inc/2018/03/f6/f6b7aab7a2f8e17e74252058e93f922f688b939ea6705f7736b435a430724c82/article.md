---
schema_version: "1.0.0"
document_id: "f6b7aab7a2f8e17e74252058e93f922f688b939ea6705f7736b435a430724c82"
company_key: "yc-estimote-inc"
company: "Estimote, Inc."
source_id: "yc-estimote-inc-rss-9f8374160089"
canonical_url: "https://blog.estimote.com/post/172115262320"
published_at: "2018-03-29T13:32:06+00:00"
first_seen_at: "2026-07-27T09:08:36.947635+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:2ded5c166d3eefb527f736e69c582d99d248ef24aa1a507a5ce2684e1c31a027"
---

# Presence verification and security is more refined than ever with Estimote Secure Monitoring

In the early days of beacons, the software was written on top native scanning solutions built into iOS and Android. One of the first questions developers would ask about them were: *is this secure? How do we make sure the user actually is next to the beacon? How do we make a deployment bulletproof?* We designed our own tools that have been tested in the field by our largest customers. But we’ve come a long way since then. Now the backbone of our software consists of custom proximity technologies like[Estimote Monitoring](http://blog.estimote.com/post/163745555265/estimote-monitoring-20-with-even-better-accuracy) and the[Proximity SDK](http://blog.estimote.com/post/166007374930/the-most-reliable-proximity-tech-now-with-a) . Our Proximity SDK’s hyper reliability, unlimited zones, and software defined range make it our core offering! And with this more sophisticated product comes the need for more sophisticated security and presence verification. That’s why today, we’re pleased to release **Estimote Secure Monitoring.**


**So, how does it work?**


Imagine you’ve built a beacon-based payment system for your train or bus, where you can automatically charge riders that are in the cab. Without security, it’s possible for somebody clever to spoof a train beacon, put it in our own Estimote lobby, and rack up fare charges every time you enter the Estimote office. Or think of it this way: automatic doors that open for you when you’re near and have a valid pass in the app. Without security, some could spoof a beacon and make the app believe you’re near the gate when you’re not, and the gate would open for a random stranger.


You need to protect the system against being gamed! You don’t want anyone to be able to clone your beacons with a Bluetooth sniffer, and then cost your business money without ever leaving their couch. This is where Estimote Secure Monitoring (ESM) comes in.


An Estimote Beacon with ESM broadcasts an encrypted sequence of bytes along with a value called the Shared Secret. Shared Secret is assigned to your beacon in Estimote Cloud and the Proximity SDK uses Estimote Cloud to resolve the encrypted broadcast. Additionally, the sequence of bytes is shuffled in constant intervals, around 10 minutes by default. We call that “packet rotation.” All of this means that only your app is able to decode what the beacon broadcasts and that the user needs to be physically present near the beacon. Even if they managed to clone it, the clone would become useless in a matter of mere minutes.


To enable Estimote Secure Monitoring, just follow the instructions[here](https://community.estimote.com/hc/en-us/articles/360002194771?flash_digest=2391354dc275a4a5331fefa182645064700b4d3b) .


If you have any doubts or questions about ESM, share them on our forums, or feel free to drop us anemail !
