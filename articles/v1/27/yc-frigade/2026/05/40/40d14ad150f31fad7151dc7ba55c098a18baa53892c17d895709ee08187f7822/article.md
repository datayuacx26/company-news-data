---
schema_version: "1.0.0"
document_id: "40d14ad150f31fad7151dc7ba55c098a18baa53892c17d895709ee08187f7822"
company_key: "yc-frigade"
company: "Frigade"
source_id: "yc-frigade-news-import-b5bda2b6da4c"
canonical_url: "https://frigade.com/blog/from-framer-back-to-code"
published_at: "2026-05-11T00:00:00+00:00"
first_seen_at: "2026-07-21T20:57:52.164816+00:00"
fetched_at: "2026-07-28T21:55:57.170712+00:00"
content_hash: "sha256:545b91e0ecda4a8be46c2748c6fade23c83683fc38498857fff1285864ed463e"
---

# We left Framer (and others will, too)

Frigade.com has lived three lives: hand-built in code during YC, two years on Framer, and now back to code. We rebuilt the third one in 267 commits this past week, voice-only, no IDE, talking to Claude.


The speed of the rebuild matters because speed was Framer's whole pitch. Their billboard a few blocks from our office reads "Enterprise needs. Startup speeds." We just rebuilt our entire site faster than an agency could quote a merge of the two we were already running on Framer.


Framer's pitch, on the way to the airport. Exactly what we couldn't get out of the tool.


No-code tools like Framer and Webflow were built as abstractions over code for humans managing complexity. LLMs don't need those abstractions. The audience these tools were designed for has changed, and the trade-offs are about to change for almost everyone else running a marketing site on a no-code platform.


This post is the *why* part of the story. If you came for the *how* , that's in[our previous post](https://frigade.com/blog/migrating-off-framer-to-code) .


## Why we went to Framer in the first place


Frigade.com had two lives before this rebuild. We hand-built the first version on Next.js in our YC days, when there were three of us and we wanted the site to feel like a piece of product. It worked for over a year. Then it stopped working.


The code wasn't the problem. The cost of running it was, and the cost was paid in engineer time. Every change pulled someone, almost always me, off product work. With three or four people on the team, "we have ten things to ship this week and the marketing website is one of them" almost never came out in the website's favor. We'd ship a bare-bones update and get back to product. Scrolling through old versions of frigade.com from late 2023, you can tell which weeks we'd been busiest and were phoning in the site updates.


This is the dynamic Framer and Webflow were built around. Engineering time is too valuable to spend on a marketing site, so put it in a designer-friendly tool and hand it to the marketing team or a contractor. Free your engineers up for product. The whole no-code premise relies on it.


So we hired our first agency and moved to Framer. They leveled up the brand and shipped a beautiful site for[Engage](https://frigade.com/engage) . The honeymoon was real. The day an agency hands a site back is the day the site is at its peak.


The first Framer site, built for Engage.


## Why Framer stopped being the right call


The tax came back, in four forms.


1. Any third-party agency sits outside the customer and product context. Every brief turned into a painstaking review cycle to pull the work back toward what we'd actually shipped.
2. The agency held the brand. We could touch copy and tweak layouts, but new illustrations, custom components, or animations needed their hand.
3. New page templates outside what they'd already built us cost a few hundred to thousands of dollars each.
4. Framer charges per seat. We didn't add one for everyone on the team, and ramping up on a separate tool for occasional edits wasn't worth it anyway. Edits queued up on me, and when I was heads-down on product, the site froze.


So the site stagnated. We stopped touching it for stretches.


Then we got ready to launch[Assistant](https://frigade.com/) , and we didn't see how Assistant fit into the existing marketing site. We hired a new agency, more on-brand for the kind of product work we wanted for that launch. They built us a slick second Framer site on a fresh domain at the time (frigade.ai) with a lot of custom Rive animation. It was beautiful.


The second Framer site, built for the Assistant launch on frigade.ai.


But now we had two Framer sites, the same four-fold tax doubled, and an incoherent customer experience. Merging them inside Framer would have meant weeks-to-months of agency work and tens of thousands of dollars. The reasons we'd left code in the first place, slowness and quality, were happening to us inside Framer.


We tried hard to stay. The simplest test: could I tell Framer's AI "build me a new on-brand case study page using our design style" and get something I could ship? The answer was seemingly no. If it had been yes, this post wouldn't exist. The tooling can do small edits well enough; what it can't do is compose components and brand language from a brief, which is the thing that actually matters.


We have a lot of respect for what Framer built. Their taste is impeccable, and the two years we spent on the platform were the right call when we made it. None of this is a knock. The trade-offs just changed.


## How fast it actually was to leave


Once I started the spike, the rebuild took less than a week. Thursday morning spike to the following Wednesday night in production. 267 commits.


I never opened an IDE. The whole rebuild was[voice-only](https://frigade.com/blog/migrating-off-framer-to-code) . I just talked to Claude, fed screenshots back, and told it what to fix. We've started calling it yap-driven development™ at Frigade.


Those 267 commits include the scaffold, every product page, every case study, every comparison page, the blog, the mobile responsive pass, the SEO work, and the modal flows. It also includes a pile of new pages and content we built but haven't even shipped yet.


For context: the agency quote to merge our two Framer sites into one cohesive brand was tens of thousands of dollars and weeks-to-months of work. We did the merge, plus a complete rebuild, plus a long list of net-new pages, in less than a week.


## What we got back beyond parity


Speed wasn't the only thing. Who can ship to the site changed too.


We're off Framer. We're off Strapi. The whole content system is markdown in the repo. Anyone on the team can now ship a blog post, an update, a case study, or a website improvement directly. There's no contractor in the loop. I'm no longer the Framer "expert" (read: bottleneck) on the team.


The cost of polish dropped to nothing. Things that would have been days of agency work got reframed as "while we're here." A custom active-tab indicator in the nav. React code blocks inside blog posts. A visual draft-and-publish flow. A new product-page chooser. Real polish on the Updates page. None of these were on the rebuild plan. All of them ended up shipping.


And new pages we couldn't have justified building in Framer started landing. Comparison pages against Pendo, Pylon, Zendesk, HubSpot. New case studies that came out better than the agency's templated versions, because we built them from components we owned instead of someone else's library.


## The thesis


What's different about coming back to code in 2026 isn't the code. It's that the cost of writing the code dropped to roughly zero, which exposes something deeper about no-code tools.


No-code tools like Framer and Webflow had one real promise: they let teams ship marketing-site work without depending on engineering. Designer-friendly UIs for non-engineers, contractor-managed templates, marketing-team self-service. These were all ways to route around the same constraint: engineering attention was the bottleneck.


LLMs collapse the bottleneck itself. A long week of talking to Claude didn't cost me a sprint of product work, because I wasn't the one writing the code. A site update that used to take an hour of engineering attention now takes a few minutes of it, from the same engineer who's still in their product flow. Engineering attention isn't the bottleneck anymore, so the tools that routed around it have less to offer.


The same pattern is playing out with animation libraries. GSAP and React Spring were abstractions over CSS and the Web Animations API for engineers who didn't want to learn the primitives. LLMs learn the primitives and write them directly. The abstraction becomes redundant.


We removed all our animation libraries from Frigade as part of this rebuild for the same reason.[Two easing curves, no animation library](https://frigade.com/blog/two-easing-curves-no-animation-library) .


So the shift isn't really about AI making code "cheap." It's about LLMs collapsing the bottleneck no-code tools were built to route around. Engineering attention was the constraint that justified the abstraction. That constraint is gone.


There's a boundary on this claim. I wouldn't ship a production SaaS product built voice-only in a weekend and trust it with customer data. A marketing site is its own category: no sessions, no API keys, no user records to corrupt, no audit log to keep clean. The thing that has to be good is the presentation. That's where the abstraction-redundancy story applies cleanly today.


## What I think comes next


The cost of writing code is going to keep dropping. The break-even point at which a team prefers code over a no-code tool is going to keep moving toward less technical teams, smaller use cases, and more types of work. Most teams currently running their marketing sites on a no-code platform are going to recompute the build/buy decision over the next year or two, and the decision will look different than it used to.


Not because no-code got worse. Because the constraint that made no-code the right choice, engineering attention being too expensive to spend on a marketing site, is gone.


If you're hitting the ceiling, paying per page, fighting the working file every time you want to ship something new, or waiting on a contractor for every meaningful change, the move back to code is shorter than it looks. We made it in a week, with a lot of help from Claude, and the result is a site we're set to keep building on for years.


The how-to is in[our previous post](https://frigade.com/blog/migrating-off-framer-to-code) .
