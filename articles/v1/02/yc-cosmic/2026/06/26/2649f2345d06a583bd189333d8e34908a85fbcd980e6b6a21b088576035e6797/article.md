---
schema_version: "1.0.0"
document_id: "2649f2345d06a583bd189333d8e34908a85fbcd980e6b6a21b088576035e6797"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/cosmic-rundown-html-first-macos-containers-npm-v12"
published_at: "2026-06-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:40.519323+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:8cd562e7416546ba83a27c48251d62dfe048107e0e4016e80612857ad1f4d1ba"
---

# Cosmic Rundown: HTML-First Wins, macOS Containers, npm v12 Breaking Changes

## HTML-First Development Gets Real Results


A post titled "[Building an HTML-first site doubled our users overnight](https://mohkohn.co.uk/writing/html-first/) " hit the top of Hacker News today. The[discussion](https://news.ycombinator.com/item?id=48475483) is packed with developers sharing similar experiences.


The core argument: ship HTML, minimize JavaScript, watch your metrics improve. Faster load times mean lower bounce rates. Lower bounce rates mean more conversions.


This isn't new advice, but seeing concrete numbers makes it harder to ignore. If your content-heavy site is struggling with engagement, the fix might not be more features. It might be fewer bytes.


For teams using a headless CMS like[Cosmic](https://www.cosmicjs.com/) , this pairs well with static site generators. Fetch content at build time, render to HTML, serve from a CDN. The architecture is simple and the results speak for themselves.


## Apple Brings Native Containers to macOS


Apple quietly released[container support for macOS](https://github.com/apple/container/blob/main/docs/container-machine.md) and the[Hacker News thread](https://news.ycombinator.com/item?id=48469658) is one of the most active today.


This is significant for developers who've been running Docker Desktop or Lima to get Linux containers on their Macs. Native support means better performance and tighter integration with the OS.


The documentation covers container machines, networking, and file sharing. If you're building dev environments or CI pipelines on macOS, this is worth investigating.


## npm v12 Breaking Changes


GitHub published a[changelog for npm v12](https://github.blog/changelog/2026-06-09-upcoming-breaking-changes-for-npm-v12/) that's causing some concern. The[discussion](https://news.ycombinator.com/item?id=48467705) highlights several deprecations that will affect existing workflows.


The changes focus on security and consistency, but they'll require updates to scripts and CI configurations. If you maintain packages or have complex build pipelines, review the changelog now rather than scrambling later.


## Google Now Liable for AI Overview Errors in Germany


A[German court ruling](https://the-decoder.com/landmark-german-ruling-declares-googles-ai-overviews-are-googles-own-words-and-makes-it-liable-for-false-answers/) declared that Google's AI Overviews are Google's own statements, making the company liable for false information. The[Hacker News discussion](https://news.ycombinator.com/item?id=48470248) explores the implications.


This matters for anyone building AI-powered features that synthesize information. The legal framework around AI-generated content is still forming, and this ruling sets a precedent that AI answers aren't just aggregation. They're assertions.


For content teams, this reinforces why human review gates matter. At Cosmic, our[AI Workflows](https://www.cosmicjs.com/ai/workflows) include approval checkpoints specifically so AI-generated content gets human eyes before publication.


## Chrome Dropping MV2 Extensions Permanently


Google is[permanently removing Manifest V2 extension support](https://www.neowin.net/news/google-chrome-is-killing-all-ublock-origin-bypasses-microsoft-edge-opera-to-follow/) from Chrome, and Edge and Opera will follow. The[discussion](https://news.ycombinator.com/item?id=48471970) is predictably heated.


This affects ad blockers like uBlock Origin that rely on MV2's more powerful APIs. Users who depend on these tools will need to switch browsers or accept reduced functionality.


For web developers, this is a reminder that browser APIs change. Build with fallbacks where possible.


## Quick Hits


**PgDog funding** :[PgDog announced funding](https://pgdog.dev/blog/our-funding-announcement) for their Postgres connection pooler. The[HN thread](https://news.ycombinator.com/item?id=48476466) discusses how it compares to PgBouncer.


**Apache Burr** : The Apache Foundation released[Burr](https://burr.apache.org/) , a framework for building reliable AI agents. Worth watching if you're in the agentic AI space.


**Mercedes axial flux motors** : Mercedes-Benz started[large-scale production of electric axial flux motors](https://media.mercedes-benz.com/en/article/bebac2af-acdc-465a-9538-adb0bf3d8ccf) . Smaller, lighter, more efficient. The engineering is impressive.


**Japanese train stations visualization** : Someone built an[animated map of all 9,300 Japanese train stations](https://jivx.com/eki) showing when each opened from 1872 to 2026. It's beautiful.


## What This Means for Content Teams


Today's news reinforces a few patterns:


1.


**Performance matters more than features** - The HTML-first success story is just the latest data point. Speed wins.


2.


**Platform dependencies are risks** - Whether it's Chrome extensions or npm packages, building on shifting ground requires planning.


3.


**AI liability is real** - The German ruling won't be the last. Human oversight of AI content isn't optional.


If you're building content workflows that incorporate AI, think about where your review gates are.[Cosmic's workflow system](https://www.cosmicjs.com/ai/workflows) lets you chain AI generation with human approval, so you get the speed benefits without the liability exposure.


---


*Want to see how AI agents can work within a governed content workflow?[Start building free](https://app.cosmicjs.com/signup) or[book a demo](https://calendly.com/tonyspiro/cosmic-intro) to see it in action.*
