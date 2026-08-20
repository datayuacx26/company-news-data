---
schema_version: "1.0.0"
document_id: "a3af07ca9fb1fd1c2f4bce1a971c01b1e773d5b0d00ad5e2b5c5947ddd48ba63"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/introducing-the-new-superwall-com"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:0e36f28e750f3d18ff00d4d161f18c7ee0b526851e5c9acdb5b9cf321aba406a"
---

# Introducing the new superwall.com: a marketing site run by agents

## The site you're reading was mostly built by agents


Until recently, superwall.com lived inside our product monorepo. Shipping a landing page meant a designer, an engineer, and a pull request into the same codebase as the dashboard. Every marketing idea sat in the same queue as product work, and the queue always won.


That's exactly the problem Superwall exists to solve for paywalls: growth teams shouldn't wait on an engineering queue to test an idea. So we applied our own medicine to our own website.


The new site is a small, standalone repo: static pages, a design system, and a thin worker for forms and data. Every page is a file in Git. And the hands editing those files are, almost always, agents': writing pages, fixing copy, building components, opening pull requests we review in Slack.


## Where human work ends and agent work begins


The most useful thing we learned building this wasn't a tool or a framework. It was a boundary, a clean line through our growth team's work:


- **Humans:** speak to customers → understand their needs → build the product → document the product.
- **Agents:** understand the customer's voice → write feature pages → write use-case landing pages → generate and post ads, social posts, and everything downstream.


The boundary is documentation. Docs are the last artifact a human produces, and the first input an agent consumes. Which forces a rule we now live by:


> If it isn't documented, it doesn't exist.


We mean that literally. Our[changelog](https://superwall.com/changelog) is generated automatically from releases of our public docs. No one on the team writes changelog posts. If a feature ships and the docs don't mention it, the site doesn't know about it, and neither does the rest of the pipeline, because a documented feature is a trigger. One changelog entry can kick off feature pages, playbooks, ad copy, and social posts, each drafted by an agent grounded in the docs and in what customers actually say on calls.


That flips the incentive most teams have backwards. Documentation stops being the chore you do after shipping and becomes the act of shipping: the moment a feature starts marketing itself.


## Design patterns that make agents good designers


Agents are excellent at applying a system and unreliable at inventing one. So the design work on this site went into making the system explicit, small, and machine-readable. The goal is bespoke-looking output from rules an agent can actually follow.


-


**design.md.** The entire design system is one markdown file covering colors, type, spacing, and hard rules like "no shadows, ever" and "fill or border, never both", split into two registers: one for marketing surfaces and one for the product. It's public at[superwall.com/design.md](https://superwall.com/design.md) , and every agent reads the law before touching UI.


-


**A pixel icon library.** 116 icons, each a 7×7 grid of pixels encoded as a single base36 code, published as a registry at[superwall.com/icons.md](https://superwall.com/icons.md) . An agent picks an icon by name instead of hand-rolling an SVG, so icons stay consistent no matter who, or what, is designing. You can even draw your own at[superwall.com/icons](https://superwall.com/icons) .


-


**Color-profiled components.** Sections accept a named color profile (background, text, border, grid, accent) resolved from canonical tokens. Restyling a component means swapping a token name. Hardcoding a hex value isn't a convention we enforce in review; it's simply not how the components work.


-


**Dithering as a design language.** Every photo and screenshot on the site runs through a dithering shader mapped to brand color pairs. The output looks art-directed; the input can be any raw image an agent grabs. A deterministic transform that makes imagery impossible to get off-brand is the closest thing we've found to agent-proof art direction.


None of these patterns are about making the site look generated. They're about the opposite: constraining the system so tightly that a hundred different agents produce one coherent site.


## The editing stack: a CMS that is just Git


There is no CMS. Pages are files, the repo is the database, and pull requests are the editorial workflow. In practice, editing the site is 100% Claude in Slack: someone spots a typo or wants a new page, sends a message, and an agent opens a pull request with a preview link. We look, we merge, and it's live in under a minute.


The site also rebuilds itself every night, even with zero commits. The interesting part is where the data comes from: the site's directories are fed by production, not by hand. The paywall templates gallery pulls from the same public API the Superwall product uses to serve templates, so when a new template ships in the product, it shows up on the site the next morning, screenshots and all. The integrations directory works the same way, reading the canonical integrations service that powers our webhooks. Ship it in the product and the marketing page updates itself.


## The customer wall is a leaderboard


My favorite detail on the whole site is the customer wall on the homepage. Every hour we pull public App Store stats for our customers' apps, including where each one ranks in its category, and the wall reorders itself so the top-ranked apps lead. Nobody curates it. When a customer has a big week, the homepage notices before we do.


Soon that leaderboard will do more than reorder logos: when it spots a customer winning big, an agent will reach out for approval and draft the case study, with a human signing off before anything ships.


## What this changes for a growth team


Our growth team's job description got shorter and better: talk to customers, build and document the product, and review what the agents produce. Taste stays human. Production doesn't have to.


This is the same bet Superwall makes for monetization: that growth teams move fastest when testing an idea doesn't require a release train. We just extended it to our own website. If you're building something similar, or you think we're wrong about where the boundary sits, I'd genuinely love to hear it. I'm[@jakemor](https://x.com/jakemor) on X.
