---
schema_version: "1.0.0"
document_id: "ccf2281a8185a806f3196c94a4f4dbe8aadfd1448498899ce82ad03f09660174"
company_key: "yc-ekho"
company: "Ekho"
source_id: "yc-ekho-news-import-71d1330defc8"
canonical_url: "https://www.ekho.com/blog/how-we-rebuilt-ekho-com-with-ai-in-a-week"
published_at: "2026-05-06T00:00:00+00:00"
first_seen_at: "2026-07-24T05:48:23.440800+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:a441131bae3a0d218fa121f1ff59032de068674cf42d5b5a6f1822f5a3600ca3"
---

# How We Rebuilt ekho.com with AI in a Week

AI summary


We migrated our entire corporate website off Webflow in about a week using Claude Code. 150+ URLs, 26 content collections, 327+ content files. This is the story of how it went, including the part where the first attempt almost killed the project, and what it means for dealer websites.


A few weeks ago,[ekho.com](https://www.ekho.com/) lived on Webflow. Today it doesn't. We rebuilt the entire corporate site, 150+ URLs and 26 content collections, in roughly a week, on a stack we own end to end. Anyone on our team can ship a change through a prompt and a pull request.


This is the story of how it went. Including the part where the first attempt almost killed the project.


##


Why we did this


The honest answer: AI is eating software, and we wanted to feel it from the inside.


Code generation has gotten good enough that a growing number of products are now thinner than they used to be. If a model can write the code, the tool layer commoditizes fast. What's left is the outcome. The companies that win the next decade are the ones that use AI to deliver results (i.e., sell a product that actually does the work), not the ones selling AI as a feature.


That has implications for everything in the stack, including the thing you're reading this on. If a piece of infrastructure can be rebuilt in a week with AI, you have to ask why you're paying a vendor every month for it. We asked that question about our website and didn't like the answer.


Webflow was a dependency, not an asset. Every meaningful change required a contractor. We were paying for a tool *and* paying for the labor to operate it, and the turnaround was always days. For a company that ships product on a faster cadence than that, the marketing site had become the slowest thing we owned. Is this starting to sound like your legacy dealer website provider..? We now truly know how you feel.


The old ekho.com, living inside Webflow. Every change went through a contractor and took days.


Two companies in our network had already done this migration. Both moved off Webflow to a self-hosted stack, both used Claude Code, both finished in roughly two weeks, both wrote it up. Prior art existed, the path was real, and the outcome was strictly better than what we had. So we did it.


##


The first attempt almost killed it


I want to skip ahead and tell you it went smoothly. It didn't.


The instinct, once you have a coding agent that can build whole pages, is to point it at the existing site and say "rebuild this." We tried. The output was generic. The styling was off, the spacing was off, the components didn't match our design system, and the pages had that specific *AI-built marketing site* look you've seen on a hundred landing-page demos. It wasn't even close.


If we'd kept going down that path, we'd have shipped a worse version of our website faster. That's not a win.


The breakthrough was treating the first page as a training exercise instead of a deliverable. We spent a disproportionate amount of time on a single page, iterating until the components actually matched our design language. Typography, spacing, color, motion, the small details that make a site feel like a real brand instead of a template. Once we had a process that worked, we encoded it as a skill. Build a component this way. Build a section by composing components. Build a page by composing sections. Each layered skill delegating down to the one below it.


That layered approach was the unlock. The first page was slow. The second was faster. The tenth was almost trivial. By the time we were rebuilding blog templates and case study pages, the bottleneck had moved from "can the AI build this" to "have we written down what we want."


If you take one thing from this post, take that. AI doesn't replace the work of figuring out what good looks like. It collapses the work of executing it once you have. The investment is in the spec, not the code.


##


What we shipped


By the numbers:


- **150+ URLs** rebuilt and redirected
- **26 CMS collections** migrated from Webflow to typed content schemas in git
- **327+ content files** ported, sanitized, and validated
- **About a week** of work (nights and weekends)
- **Zero** ongoing platform fees beyond infrastructure


The stack itself isn't exotic. Static-first generation, edge CDN, content as files in a repo with a schema, interactive components only where they earn their keep. It's the kind of architecture that's been quietly correct for years. What's new is that you can now build it without a frontend team, by talking to a coding agent.


The site is faster than it was on Webflow. Pages are mostly static and ship effectively no JavaScript. Images convert to modern formats automatically. Fonts are subsetted. Everything is served from the edge. The Lighthouse scores moved in the right direction across the board.


The piece that mattered more than performance, though, was *who can change it* . On the old site, that list was: a contractor. On the new one, it's anyone on the team with a GitHub account. Marketing edits the homepage by writing the change and opening a pull request. Engineering reviews. It ships.


##


The fun stuff


Once the foundation worked, we built things we couldn't have built on Webflow without serious cost. Some have a lot of utility. Others have absolutely none. Let's start with the ones that don't.


### The useless (but delightful) stuff


Anyone on the team can make changes to the website. There are no limits, really, just the limits of your local browser. You can do incredibly random things. Just like this button below. Give it a try.


Or, if you're more of a gamer, here's a motorcycle that needs to get to the dealership. Space bar to jump.


Neither of those things will ever generate a lead. But both were built in approximately 45 seconds with Claude Code's fast mode on, from a single prompt, on a production website. That's the point.


### The actually useful stuff


**The Website Grader.** A full interactive tool at[/website-grader](https://www.ekho.com/website-grader) that audits any dealer site for visibility on AI search engines, traditional SEO health, and performance. It's a real product, not a marketing widget. It runs as an island of interactivity inside a static page, which is the right architecture for this kind of thing and was a pain to do well on a closed platform. Worth reading the[companion piece on why AI search is different from SEO](https://www.ekho.com/blog/why-ai-search-is-fundamentally-different-from-seo-and-why-that-matters-for-dealers) for the why behind it. Try it yourself:


**Hand-built motion.** The illustration system on the new site is custom. Every illustration renders as layered glass and color, responds to the cursor, springs in on mount, and respects reduced-motion preferences. We built it because we couldn't find a library that did what we wanted, and the cost of building it ourselves dropped to "an afternoon" once the AI workflow was working.


And the AI Sales Agent demo you see below? Also built entirely from prompts. Interactive, animated, running real conversation flows. Try hovering over it.


**Logo and partner automation.** Adding a customer or partner to the site used to be a Photoshop job followed by a CMS update. Now it's a single command. The asset pipeline fetches the logo, removes the background, color-treats it to match our palette, sizes it correctly, extracts the symbol if there is one, and opens a pull request. It can also fire automatically when partnership deals close in our CRM. Deals close, logos appear. We didn't set out to build this; it just became obvious once everything else was in code. Take a look:


**A shared design system across products.** The components on[ekho.com](https://www.ekho.com/) are the same components that power our dealer website product. When we improve a button or a section pattern on our own site, dealers using our platform get the improvement too. When we build something new for a dealer, it can flow back. One design language, two products, one codebase lineage. Here are a few of the components we're most proud of:


##


The catch: corporate sites are the simple version. Dealer sites are not.


Now the part that matters for dealers reading this and thinking "great, AI builds websites, I can just use Claude Code to rebuild my site."


A corporate marketing site is mostly static content. The data dependencies are minimal. The whole thing fits in a content folder. You can rip and replace it in a week, and we just did. Unfortunately, none of that is true for a dealer website.


A real dealer site has to:


- Pull live inventory from a DMS or OEM data feed and keep it in sync
- Display pricing, availability, and vehicle specs that change every day
- Respect OEM compliance rules and brand certification programs that vary by manufacturer
- Route leads into the right CRM with the right attribution
- Support financing pre-qualification, trade-in estimation, accessory configuration, and appointment booking
- Operate inside franchise agreements that constrain how inventory can be displayed
- Work across multiple rooftops with different brands, pricing rules, and integrations


You cannot point a coding agent at a dealer website and say "rebuild this." The website is the tip of the iceberg. Underneath it is a web of data dependencies, regulatory rules, and system integrations that no general website builder understands. This is why most dealer websites today are invisible to AI search engines. They're built on legacy platforms with bad structured data, no semantic markup, and zero consideration for how a model parses content. The site looks fine to a human and is opaque to everything else.


##


What Ekho actually is


Ekho's dealer website is not a website builder. It's a website that ships pre-connected to the things that matter: live inventory, an[AI sales agent](https://www.ekho.com/sales-agent) that handles leads on every page (plus SMS and email) around the clock, native financing and trade-in flows, accessory configuration, and the[regulatory plumbing for 50-state remote vehicle commerce](https://www.ekho.com/transaction-engine) . The website is the surface; the actual product is the system underneath.


That system took years to build, and it's the piece that AI can't shortcut. We've spent that time assembling the licensure portfolio, mapping the state-by-state workflows for titling, registration, tax, and insurance, and processing thousands of real vehicle sales across the United States. Every edge case we've encountered, every compliance rule we've encoded, every lender integration we've wired up: that's institutional knowledge earned by doing the work, not something a model can generate from a prompt.


And because we built our own site the same way, AI editing is a real piece of the dealer product too. Not "AI-generated templates." Actual AI-assisted editing of a working website connected to working systems. A layout tweak that takes a legacy support queue three days can take a dealer on Ekho about three minutes. Standing up a full dealer site on the platform can happen in a few weeks instead of months.


We realize that switching your website can be a big lift. The good news: you don't have to start there. The[AI Sales Agent](https://www.ekho.com/sales-agent) can live on any dealer website, and the[Transaction Engine](https://www.ekho.com/transaction-engine) can be hooked up to your existing site too. You get the 50-state compliance, the financing waterfall, and the 24/7 AI sales coverage without changing a pixel of your current storefront. That said, when Ekho powers the top-of-funnel experience as well, we can help drive meaningfully higher conversion. We've learned what works by optimizing our buyer flows every day based on data from thousands of transactions. It's in the dealer's best interest for us to power those surface areas, but it's never a requirement.


That's the difference between a tool and a service. A website builder gives you the wrench and wishes you luck wiring it to your DMS, your CRM, your lender waterfall, and your OEM's brand portal. Ekho can give you the working website with all of that already done. The wrench is free now. The implementation is what's worth paying for.


If that sounds like the kind of website you want for your dealership,[get in touch and join the waitlist today](https://www.ekho.com/join-the-waitlist) .


##


The takeaway


We rebuilt our own site with AI. Our sales agent is AI. Our internal tooling runs on AI agents.


We're not an AI company. We're a vehicle commerce company rebuilding the most foundational parts of vehicle retail, that happens to use AI as infrastructure. The product is the outcome: dealers sell more vehicles, buyers have a better experience, and the operational complexity of selling a titled vehicle disappears. AI is how we get there faster than anyone else. It is not the thing we sell.


The gap between companies that use AI to deliver results and companies that sell AI as a feature is going to widen fast. We know which side of that line we want to be on.


If you're a dealer thinking about your own website, the[Website Grader](https://www.ekho.com/website-grader) will tell you, in about thirty seconds, where you currently stand. If you like what we built for ourselves and want it for your own dealership,[get in touch and join the waitlist today](https://www.ekho.com/join-the-waitlist) . And if you're an engineer, an operator, or anyone who likes the idea of building software this way,[we're hiring](https://www.ekho.com/careers) .


## Frequently asked questions


About a week of focused work, end to end. The first day was the hardest. Once we had a working pattern for building components, the rest moved quickly.


Static-first generation, fast edge delivery, content as files in a repo. It's the architecture that lets a marketing site stay close to zero JavaScript and ship in milliseconds. Two other companies in our network had already validated the same approach and published their writeups. We followed prior art.


Yes. We mapped every URL, set up the redirects in advance, kept canonicals and metadata intact, and watched Search Console daily for the first few weeks. Performance scores improved across the board.


The architecture and AI editing workflow are shared. The dealer product also includes the things a corporate site doesn't need: live inventory, financing, trade-ins, accessory configuration, AI sales agent on every page, OEM compliance, and CRM integrations. That's where most of the value sits.


Yes, with a review step. Content lives in markdown and JSON files in a repo. Anyone on the team can open a pull request, often through a prompt. An engineer reviews. It ships. The bottleneck moved from "ask the contractor" to "ask for a code review."
