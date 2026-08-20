---
schema_version: "1.0.0"
document_id: "a14f942abbd080092fab3ebb2400b8148318fd74c0ab796166d571834d4dc2d5"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/cosmic-rundown-nginx-zero-day-bun-rust-rav4-privacy"
published_at: "2026-05-15T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T22:13:07.113521+00:00"
content_hash: "sha256:635c7c59212cc0514aaa94bc439158c269004d329ea83aab2fdf554e8355ba08"
---

# Cosmic Rundown: Nginx Zero-Day, Bun Goes Rust, and RAV4 Privacy Hacks

## Nginx-Rift: A New Exploit in the Wild


A new Nginx exploit called[Nginx-Rift](https://github.com/DepthFirstDisclosures/Nginx-Rift) landed on GitHub this week and immediately caught attention on[Hacker News](https://news.ycombinator.com/item?id=48138268) . The disclosure from DepthFirstDisclosures details a vulnerability that affects a significant number of Nginx deployments.


For teams running Nginx in production, this is worth immediate attention. The repository includes technical details and proof-of-concept code. If you're managing web infrastructure, check your Nginx version and monitor the official channels for patches.


## Bun's Rust Rewrite Merges, Controversy Follows


The[Bun runtime merged its Rust rewrite](https://github.com/oven-sh/bun/pull/30412) , a massive pull request that's been in the works for months. The[Hacker News discussion](https://news.ycombinator.com/item?id=48132488) hit 749 comments, split between excitement and skepticism.


The controversy didn't take long. A new[issue filed against the rewrite](https://github.com/oven-sh/bun/issues/30719) claims the codebase fails basic miri checks and allows undefined behavior in safe Rust. For a project billing itself as a faster, safer alternative to Node.js, this raises questions about the rush to ship.


The debate highlights a recurring tension in systems programming: moving fast versus moving safely. Rust's safety guarantees only work if you actually use them correctly.


## First macOS Kernel Exploit on Apple M5


Security researcher quadrige published the[first public kernel memory corruption exploit for Apple's M5 chip](https://blog.calif.io/p/first-public-kernel-memory-corruption) . The[Hacker News thread](https://news.ycombinator.com/item?id=48139219) is filled with technical discussion about the exploit chain and what it means for Apple's security posture.


This comes alongside a separate[Pixel 10 zero-click exploit](https://projectzero.google/2026/05/pixel-10-exploit.html) from Google's Project Zero team, detailed in[another active discussion](https://news.ycombinator.com/item?id=48148460) . Mobile security is getting more attention as these devices become primary computing platforms for most people.


## Removing Tracking Hardware from a RAV4


One of the week's most popular posts comes from a developer who documented[removing the modem and GPS from their 2024 RAV4 hybrid](https://arkadiyt.com/2026/05/13/removing-the-modem-and-gps-from-my-rav4/) . The[discussion](https://news.ycombinator.com/item?id=48138136) generated over 500 comments about vehicle privacy, data collection, and the right to modify hardware you own.


The post is a detailed technical walkthrough with photos. It's part of a growing movement of owners taking control of their vehicle data, especially as car manufacturers increasingly treat connectivity as a subscription service.


## Quick Hits


**Radicle** is getting attention as a[sovereign code forge built on Git](https://radicle.dev/) . The[Hacker News discussion](https://news.ycombinator.com/item?id=48147603) covers why developers are looking for GitHub alternatives that don't depend on a single company.


**OCaml in space** : A fascinating post about[using OCaml for space applications](https://gazagnaire.org/blog/2026-05-14-borealis.html) is making the rounds. The[discussion](https://news.ycombinator.com/item?id=48147058) digs into why functional programming languages are gaining traction in safety-critical systems.


**RTX 5090 on a Mac** : Someone got an[RTX 5090 working with an M4 MacBook Air via eGPU](https://scottjg.com/posts/2026-05-05-egpu-mac-gaming/) for gaming. The[thread](https://news.ycombinator.com/item?id=48137145) is a mix of amazement and questions about Apple's GPU strategy.


**Bug bounty programs dying** : Turso announced they're[retiring their bug bounty program](https://turso.tech/blog/the-wonders-of-ai) , citing AI-generated spam submissions as a major factor. The[discussion](https://news.ycombinator.com/item?id=48148391) reflects broader frustration with how AI is affecting security research.


## What This Means for Your Stack


Today's news reinforces a few themes worth considering:


1.


**Security debt is real.** Whether it's Nginx, your runtime, or your vehicle, vulnerabilities compound. Stay current on patches and monitor disclosure channels.


2.


**Rewrites carry risk.** Bun's Rust rewrite is ambitious, but shipping unsafe code in a "safe" language undermines the whole point. Test thoroughly before adopting major version changes in production.


3.


**Privacy requires action.** From cars to content management systems, data collection is the default. Choose tools that respect user privacy by design.


For teams building with a headless CMS, these security and privacy considerations matter.[Cosmic's API-first architecture](https://www.cosmicjs.com/docs/api) gives you control over where your data lives and how it's accessed, without vendor lock-in or tracking surprises.


---


*Building something with Cosmic? We'd love to hear about it.[Get started free](https://app.cosmicjs.com/signup) or[reach out to our team](https://www.cosmicjs.com/contact) with questions.*
