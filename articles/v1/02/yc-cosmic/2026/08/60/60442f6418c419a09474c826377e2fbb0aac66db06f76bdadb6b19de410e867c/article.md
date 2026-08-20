---
schema_version: "1.0.0"
document_id: "60442f6418c419a09474c826377e2fbb0aac66db06f76bdadb6b19de410e867c"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/cosmic-rundown-phone-servers-redis-mysql-trebuchet"
published_at: "2026-08-09T00:00:00+00:00"
first_seen_at: "2026-08-09T18:59:39.419360+00:00"
fetched_at: "2026-08-09T18:59:42.490079+00:00"
content_hash: "sha256:9017dcbd15df1531299a3cd114cfec364fcc328fc504e05f5dcf8d889145a108"
---

# Cosmic Rundown: Phone Servers, Redis Exits, and Supersonic Trebuchets

## Your next server might be a phone


A developer documented how they[turned a phone into their primary server](https://seg6.space/posts/phone-server/) , and the[Hacker News discussion](https://news.ycombinator.com/item?id=49226636) went deep on the practicality. Modern phones have multi-core processors, gigabytes of RAM, built-in battery backup, and cellular failover. The power consumption is measured in single-digit watts.


The conversation surfaced real use cases: home automation hubs, personal API endpoints, lightweight databases, and edge compute nodes. For developers experimenting with self-hosting, a retired phone sitting in a drawer might be the lowest-friction entry point.


## Shopify replaced Redis with MySQL for inventory


Shopify's engineering team published how they[replaced Redis with MySQL for inventory reservations](https://shopify.engineering/scaling-inventory-reservations) and watched performance improve. The[discussion](https://news.ycombinator.com/item?id=49226536) challenged assumptions about when Redis is actually the right tool.


The key insight: Redis excels at caching and pub/sub, but for durable inventory state that needs ACID guarantees, MySQL's InnoDB already provides what you need. Adding Redis created a consistency problem that required complex synchronization. Removing it simplified the architecture and reduced failure modes.


For teams building e-commerce or inventory systems, this is a reminder to question whether that Redis layer is solving a real problem or creating new ones.


## A trebuchet that breaks the sound barrier


Tom Stanton built a[supersonic trebuchet](https://www.techeblog.com/tom-stanton-supersonic-trebuchet/) that accelerates projectiles past Mach 1 using only gravitational potential energy. The[Hacker News thread](https://news.ycombinator.com/item?id=49232110) dove into the physics of how a medieval siege weapon design can achieve velocities that seem impossible without combustion or compressed gas.


The engineering lesson: constraints force creativity. By optimizing the release timing and arm geometry, Stanton extracted performance that defies intuition about what gravity-powered machines can do.


## Silicon Valley and science fiction literacy


Historian Jill Lepore argues that[tech leaders misread science fiction](https://techcrunch.com/2026/08/09/historian-jill-lepore-says-the-tech-industry-is-led-by-bad-readers-who-are-undermining-democracy/) , treating cautionary tales as instruction manuals. The[discussion](https://news.ycombinator.com/item?id=49232221) examined how narratives shape product decisions.


The critique matters for anyone building AI-powered products. Science fiction imagines futures to interrogate them, not to provide blueprints. Reading Asimov's robot stories as a product roadmap misses the point.


## Magic hexagons exist at every order


A mathematical proof shows[magic hexagons exist for every order](https://gukov.dev/math/2026/08/02/new-magic-hexagons.html) , overturning decades of assumption that only order-3 hexagons were possible. The[thread](https://news.ycombinator.com/item?id=49229174) attracted recreational mathematicians examining the construction method.


## Windows Weather app uses 1GB of RAM


NotebookCheck reported that[Windows 11's Weather app consumes over 1GB of RAM](https://www.notebookcheck.net/Windows-11-s-built-in-Weather-app-wastes-more-than-1-GB-of-RAM.1364205.0.html) . The[discussion](https://news.ycombinator.com/item?id=49232138) became a case study in Electron bloat and the hidden costs of web-wrapped desktop apps.


## OS8088 brings Mac-like UI to vintage PCs


[OS8088](https://os8088.com/) delivers a Mac-inspired graphical environment for IBM XT, 286, and 386 machines. The[thread](https://news.ycombinator.com/item?id=49226923) explored how developers are pushing hardware that shipped 40 years ago to run interfaces that feel surprisingly modern.


## Quick hits


**Dithered QR codes** : A technique for[embedding images in QR codes](https://www.andrewt.net/dithered-qr-codes/wtf/) using dithering patterns while maintaining scannability.


**A* pathfinding heuristics* *: Red Blob Games published on[improving heuristics for A* pathfinding](https://www.redblobgames.com/pathfinding/heuristics/differential.html) , useful for game developers and robotics engineers.


**Microsoft Word 1.1a on x64** : A developer[ported Microsoft Word for Windows 1.1a to native x64](https://github.com/jmarshall23/msword) , demonstrating how legacy codebases can run on modern architectures.


**Uber's merge queue** : Uber open-sourced[SubmitQueue](https://github.com/uber/submitqueue/) , their speculative merge queue for handling high-velocity monorepo commits.


**Taxi drivers and Alzheimer's** : Research suggests[taxi drivers rarely die of Alzheimer's](https://theconversation.com/taxi-drivers-rarely-die-of-alzheimers-how-complex-mental-maps-and-spatial-reasoning-protect-your-brain-286650) , with spatial reasoning potentially providing cognitive protection.


---


Building content systems that adapt to these shifts requires infrastructure designed for change.[Cosmic's AI agents](https://www.cosmicjs.com/ai/agents) handle content generation with oversight built in. The[REST API](https://www.cosmicjs.com/docs/api) integrates with any stack.[Start building for free](https://app.cosmicjs.com/signup) and see how modern content infrastructure supports the workflows these developments enable.
