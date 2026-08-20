---
schema_version: "1.0.0"
document_id: "a866474e17107c6ae128327cd96925fc8982754dd7762a87287b852236244aa7"
company_key: "yc-wasp"
company: "Wasp"
source_id: "yc-wasp-rss-5b1984e54864"
canonical_url: "https://wasp.sh/blog/2026/06/17/made-with-wasp-vol-2"
published_at: "2026-06-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:24.634053+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:b3893f02bee813974d795790d53df4c4abc12bc7944b097ddd65a7d6ed039470"
---

# Made with Wasp, Vol. 2 - from video editing to daycare lesson planning

Welcome to Day 3 of[Launch Week #12](https://wasp.sh/blog/2026/06/05/wasp-launch-week-12-ts-spec) , a.k.a. Community Day. This is the one where we step out of the way and let the Waspeteers do the talking. 🐝


We get one question more than any other: *"okay, but what do people actually build with Wasp?"* Today's the answer.


Four apps, four verticals that have basically nothing to do with each other (video tooling, UK property tech, e-commerce, and early-childhood education), all paid, all in production, all built by people from the Wasp community. It's been a minute since[Vol. 1 back in 2023](https://wasp.sh/blog/2023/06/28/what-can-you-build-with-wasp) , and the bar has moved. Let's get into it.


## [CutCue](https://cutcue.io/) - the AI editing intern for streamers ( ***Wasp + Python workers*** )​


**Try it out:**[cutcue.io](https://cutcue.io/) · **Pricing:** €29 to €189/mo


Imagine you just streamed for six hours on Twitch and now you have to pull the three best clips for YouTube. Normally that's an evening of scrubbing the VOD with a coffee. Drop the recording into CutCue and you get back a timeline already marked up with **highlights, chapter breaks, brand mentions, moments that could lose you ad revenue (e.g. swearing), and chat peaks** , automatically.


CutCue interpreting a VOD in real time.


The cool bit: the markers don't sit in some web dashboard you have to copy-paste from. CutCue exports straight into **Premiere Pro, DaVinci Resolve, Vegas, Audacity, and Reaper** , so they show up exactly where your timeline expects them. You open your editor, and the AI's homework is already on the canvas.


Under the hood: a **Wasp app orchestrating four to five Dockerized Python workers** (audio extraction, chapter detection, fingerprinting, queue management), self-hosted on Hetzner, built stateless from day one so it can scale horizontally when it needs to. The kicker: shipped by a backend-heavy engineer who'd never written React before this project.


> "I described it usually as a baseline or a toolkit, the foundation for my project, which takes over the important stuff like session management, user payment, all the things which usually take a long time to build yourself and make stable and secure."
>
>
> *- the CutCue founder*


## [Brick](https://onbrick.co.uk/) - a website builder for UK property operators​


**Try it out:**[onbrick.co.uk](https://onbrick.co.uk/) · **Pricing:** £45 to £119/mo


Picture this: you're a UK landlord with three properties, and a prospective tenant just asked for your website. You... don't have one. Brick is the fastest way out of that conversation.


You tell it your business name and what kind of property you run (rent-to-rent, serviced accommodation, HMO, deal sourcing, development), and it spins up a full website in about five minutes: **AI-generated copy** tuned to your niche, a **chatbot that captures leads** while you're at a viewing, **GDPR pages, SSL, daily backups** , the lot. Each property niche gets its own template, so the sites don't all look the same.


Brick features a full-fledged no-code website editor! Really cool.


## [ChatBuster](https://chatbuster.com/) - the AI chat that closes the sale, live on Shopify and WordPress​


**Try it out:**[chatbuster.com](https://chatbuster.com/) · **Pricing:** from $40/mo · **Where:** Shopify App Store + WordPress.org


If you've ever shopped on a Shopify store and pinged the chat widget at midnight to ask *"do you ship to Canada?"* , ChatBuster is the thing on the other end. Except now it's AI, trained on **the merchant's actual catalog, theme, and policies** , so it answers with the right number, the right policy, and the right SKU.


Under the hood: **live catalog sync** (SKUs, prices, stock update as the merchant updates), **proactive prompts** you can trigger on scroll or schedule, and **order tracking** through the store's API. A merchant installs it from the **Shopify App Store** or **WordPress.org** (the WP plugin went live in May 2026), flips one toggle, and it's working five minutes later. No developer required.


And it's not theoretical. Real merchants are already leaving five-star reviews:


Five-star reviews from Shopify merchants.


## [PlanRelief](https://planrelief.io/) - AI lesson plans for daycare and early-years educators​


**Try it out:**[planrelief.io](https://planrelief.io/) · **Pricing:** free to try · **For:** educators of ages 0 to 12


Imagine you're a daycare teacher and you spent the afternoon watching kids in the play corner. You jot down *"Mia experimented with mixing colours."* PlanRelief takes that one-line observation, plus a theme you pick ("rainbows and seasons"), and generates **a full lesson plan** : learning objectives, materials list, assessment strategies, the activity flow, ready to hand to a supervisor.


I typed 'learn building web apps' as the topic. The AI took it seriously and came back with toddler-grade activities involving big buttons and Velcro icon boards. Respect.


The pitch line is exactly what you'd want it to be: *"paperwork done in minutes, not hours."* Free to try, no credit card.


## Build anything with Wasp, ship it, and get paying customers​


Four apps, zero overlap. Creator tools, property tech, e-commerce, classroom. All paid SaaS. All running in production. All built on the same framework you can install with one command.


That's the whole point of Community Day, really: when we say Wasp is a real fullstack framework, we mean *real* . Apps in real verticals, with real customers paying real money. The lineup looks very different from[Vol. 1](https://wasp.sh/blog/2023/06/28/what-can-you-build-with-wasp) , and it'll look very different again the next time we do this.


If you've built something on Wasp that should be in Vol. 3, come say hi on[Discord](https://discord.gg/rzdnErX) or[X/Twitter](https://twitter.com/WaspLang) . We'd love to feature you next time.


And if you haven't started yet, you're one command away:


```text
npm   i   -g   @wasp.sh/wasp-cli@latest
```


See you on Day 4. 🐝
