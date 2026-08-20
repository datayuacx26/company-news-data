---
schema_version: "1.0.0"
document_id: "4bcbd927b94edeadd7e33dc35c4fd4f42d5ea43b030274f7575b94d5e590ed7e"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/cosmic-rundown-vscode-attribution-haskell-scale-ladybird-progress"
published_at: "2026-05-03T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:40.519323+00:00"
fetched_at: "2026-07-28T22:15:27.754583+00:00"
content_hash: "sha256:29bd73c94f6ca85a1a20c5f95bb103ea8678ea748ddc5632f85d734297eb1926"
---

# Cosmic Rundown: VS Code Attribution, Haskell at Scale, and Ladybird Progress

## VS Code's Copilot Attribution Controversy


A[pull request in the VS Code repository](https://github.com/microsoft/vscode/pull/310226) revealed that VS Code inserts "Co-Authored-by Copilot" into commit messages regardless of whether developers actually used AI assistance. The[Hacker News discussion](https://news.ycombinator.com/item?id=47989883) generated significant pushback.


The issue touches on attribution integrity and what it means for commit history when tools claim credit automatically. For teams that track AI usage for compliance or simply want accurate git history, this behavior creates noise. Microsoft has since responded to the feedback.


---


## Two Million Lines of Production Haskell


Mercury published a deep dive into running[a couple million lines of Haskell](https://blog.haskell.org/a-couple-million-lines-of-haskell/) in production. The[discussion](https://news.ycombinator.com/item?id=47991802) covers their engineering practices at scale.


Key takeaways:


- Strong typing catches entire categories of bugs at compile time
- Refactoring large codebases becomes tractable with the type system as a guide
- Hiring remains a challenge, but the team has built effective onboarding


For teams evaluating language choices, Mercury's experience provides real data on what functional programming looks like at scale.


---


## Ladybird Browser Update


The[April 2026 Ladybird newsletter](https://ladybird.org/newsletter/2026-04-30/) dropped with progress updates on the independent browser project. The[thread](https://news.ycombinator.com/item?id=47990318) discusses web compatibility improvements and performance work.


Ladybird represents one of the few serious attempts to build a new browser engine from scratch. For anyone invested in browser diversity, these monthly updates track meaningful progress toward that goal.


---


## Dav2d: AV1 Decoding


[Dav2d](https://code.videolan.org/videolan/dav2d) from VideoLAN provides AV1 video decoding. The[discussion](https://news.ycombinator.com/item?id=47988504) examines performance benchmarks and codec adoption.


AV1 adoption continues growing across streaming platforms. Having efficient, open-source decoders matters for the broader ecosystem.


---


## Do Not Track Gets a Refresh


[donottrack.sh](https://donottrack.sh/) offers a new approach to privacy signaling. The[Hacker News thread](https://news.ycombinator.com/item?id=47988592) debates whether any technical solution can work without regulatory backing.


The original Do Not Track header failed because sites ignored it. This project attempts to create accountability around privacy preferences.


---


## Embedded Rust vs C


An[arXiv paper](https://arxiv.org/abs/2604.25679) compares Rust and C for industrial microcontroller firmware. The[discussion](https://news.ycombinator.com/item?id=47974307) gets into practical tradeoffs.


Findings suggest Rust's safety guarantees come with modest overhead that may be acceptable depending on the use case. For teams building embedded systems, this adds to the growing body of evidence around Rust adoption.


---


## Utah Targets VPN Users


[Utah passed legislation](https://www.tomshardware.com/software/vpn/utah-becomes-first-us-state-to-target-vpn-use-with-age-verification-law) holding websites liable when users mask their location with VPNs to bypass age verification. The[discussion](https://news.ycombinator.com/item?id=47997358) examines enforcement challenges and broader implications.


This creates an interesting precedent for how location-based compliance works when users have trivial tools to circumvent it.


---


## Mercedes Brings Back Buttons


[Mercedes-Benz committed to restoring physical buttons](https://www.drive.com.au/news/mercedes-benz-commits-to-bringing-back-phycial-buttons/) in their vehicles. The[thread](https://news.ycombinator.com/item?id=47997418) celebrates the return to tactile controls.


The touchscreen-everything trend in automotive UI faced consistent criticism for safety and usability. Mercedes joining the pushback signals broader industry recognition that physical controls have value.


---


## Quick Hits


**Specsmaxxing** : A post on[writing specs in YAML](https://acai.sh/blog/specsmaxxing) to combat what the author calls "AI psychosis" gained traction. Structured specifications help AI assistants maintain context.


**Battery Innovation** : The European Patent Office reports[battery reuse and recycling patents increased seven-fold](https://www.epo.org/en/news-events/news/inventions-battery-reuse-and-recycling-increase-more-seven-fold-last-decade) over the past decade.


**WatchOS Maps** : A developer documented[six years of perfecting maps on WatchOS](https://www.david-smith.org/blog/2026/04/29/maps-on-watchos/) , showing the iteration required for constrained platforms.


**Brain Scan Research** : Stanford research shows[group averages obscure individual brain behavior](https://med.stanford.edu/news/all-news/2026/04/brain-scans-individual-versus-group.html) , with implications for personalized medicine.


---


## What This Means for Content Teams


The VS Code attribution controversy highlights how AI integration touches everything, including git history and compliance. Teams need clear policies on AI usage disclosure.[Cosmic's AI agents](https://www.cosmicjs.com/ai/agents) provide transparent activity logs so you always know what AI contributed to your content.


Mercury's Haskell story shows that unconventional technology choices can work at scale with proper investment. Similarly, choosing a headless CMS like[Cosmic](https://www.cosmicjs.com/docs/api) gives teams flexibility to use whatever frontend technology fits their needs.


---


**Start building with Cosmic**


- [Create a free account](https://app.cosmicjs.com/signup)
- [Explore the API documentation](https://www.cosmicjs.com/docs/api)
- [Learn about AI agents](https://www.cosmicjs.com/ai/agents)
