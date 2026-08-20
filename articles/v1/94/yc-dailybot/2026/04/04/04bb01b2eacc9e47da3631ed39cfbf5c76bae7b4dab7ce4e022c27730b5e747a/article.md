---
schema_version: "1.0.0"
document_id: "04bb01b2eacc9e47da3631ed39cfbf5c76bae7b4dab7ce4e022c27730b5e747a"
company_key: "yc-dailybot"
company: "DailyBot"
source_id: "yc-dailybot-rss-59e03def7d75"
canonical_url: "https://www.dailybot.com/blog/how-we-migrated-dailybot-to-astro/"
published_at: "2026-04-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:17.151433+00:00"
fetched_at: "2026-07-28T22:15:51.462388+00:00"
content_hash: "sha256:4f73dc100a367b8b424ae74e10f159612fb2feed6fd7a2a3f3f2ac2b82734a70"
---

# Rebuilding dailybot.com for the agentic era: from a visual-editor CMS to Astro

For most of the last five years, dailybot.com lived on a visual-editor CMS. By early 2026, the pace of the agentic era and the constant changes we were shipping everywhere else were starting to turn it into a bottleneck. The project that forced the decision was already on the roadmap: a full rebrand of Dailybot — new visual identity, new type ramp, new spacing, a redrawn component library — that would touch every page of the site.


The gap wasn’t performance. The gap was velocity. Every other surface in the company had started moving at agent speed — a product change proposal written Monday afternoon could be in staging Tuesday morning. The marketing site moved at CMS speed: open the editor, find the page, remember the quirks of the block you’re editing, publish, spot the typo, fix it, publish again. A design-system update that should have taken a day took a quarter. A new landing page for a feature we were shipping the same week was often impossible.


This post is the story of how we moved more than 700 pages per language — English, Spanish, and Portuguese — to[Astro](https://astro.build/) , and why the combination of a static-first framework with an agent loop produced a velocity we could not have reached with either one alone.


## What we had before


Our previous site was built on[Webflow](https://webflow.com/) . It served us well for four years. We onboarded multiple marketing hires with it without a single engineering ticket; the visual editor was the fastest way for a small team to own its public surface, and the publishing story was clean enough that no one lost an afternoon to a broken deploy.


It stopped fitting when the shape of our work changed, not because the tool got worse.


## Why we couldn’t stay


Two pressures lined up by the end of 2025.


**The site had grown past the tool.** dailybot.com had become a content property: long-form blog posts, an academy section, a help center, a templates gallery, integration pages, developer docs — 16 page types, hundreds of pages per language. Every structural change multiplied across all of them. A layout tweak that was a five-minute CSS change in our product app was a multi-day exercise in the CMS.


**And the agents had arrived — but they could not edit a no-code CMS fast enough to matter.** The most consequential shift was cultural. Across the company, the rhythm had become: think out loud with an agent, draft, refine, ship. Our coordination platform exists precisely to make that rhythm work at team scale — see our[Dailybot 3 launch post](https://www.dailybot.com/blog/dailybot-v3-launch/) . But our marketing site lived in a surface where that rhythm couldn’t land. The editor was designed for a person clicking. You could train an agent to click through an editor, but that’s not where agents are productive. Teaching one to author pages by driving a browser is not a workflow; it’s pure friction.


We tried to stay. For about two weeks we rebuilt the new design system inside the existing CMS, page by page, to see if we could ship the rebrand without changing platforms. The pace was incompatible with what we needed. At that rhythm the rebrand alone would have eaten the quarter, and the rest of the site still had to keep moving.


## Why Astro, and why now


Once we ruled out staying, we evaluated two paths: move to a headless CMS — one that only manages content and exposes it over an API — with a custom frontend, or move to a static-site framework where content lives as files in the repo.


I’d already written the long case for choosing[Astro](https://astro.build/) — and for pairing it with Svelte — on my personal blog:[Astro and Svelte: the future of web development](https://xergioalex.com/blog/astro-and-svelte-the-future-of-web-development/) . That piece is the *why Astro* argument in full. This section is the short version: the four properties that mattered for this migration.


- **Static-first output.** Marketing pages are almost purely static. Astro’s default is to render HTML at build time and ship zero JavaScript unless a component asks for it. For a content site, that’s the right default.
- **Islands for the parts that need to move.** When a page genuinely needs interactivity — a live pricing calculator, an interactive comparison, a small animated demo — Astro lets us drop in a dynamic island from any modern framework: React, Vue, Svelte. We picked Svelte. We don’t pay for interactivity we don’t use.
- **Content Collections.** A blog post in Astro is a file: frontmatter, MDX, done. A Zod schema validates every entry at build time. “Does this post have a valid hero image and a published date?” becomes a type error, not a runtime surprise.
- **MDX when we need composition.** For long-form pieces — academy articles, deep-dives, changelog stories — we wanted reusable components like callouts, pull-quotes, and before/after blocks. MDX lets us compose them without leaving the article.


The decisive property was that Astro is a codebase. An agent can read it, run it, extend it, and open a pull request against it the same way a human engineer can. That property is not unique to Astro, but it was the condition our workflow needed. The hosted CMS didn’t have it.


Put differently: Astro is still a CMS for us — one where content lives as files, the schema is typed, and every edit is a diff. We did not leave the category. We changed which kind of CMS our team lives with.


## How the migration went


With that ruled out, we ran the migration test. We spent a day with the agents porting the design system base, several of the main pages, the blog, and a large chunk of the content. That was the gate: if migrating a single page was going to take a week, we weren’t shipping the project in a quarter.


All of it landed in one day.


The next day closed the first pass. One engineer,[Cursor](https://cursor.com/) and[Claude Code](https://www.anthropic.com/claude-code) , two days. Even so, there was still a lot of work ahead, but the scaffolding already lived in the codebase and the site rendered from it.


The full migration ran for six weeks. Scope by the numbers:


- 16 page types × 3 languages — about 700 pages per language, plus a markdown twin for each so agents can read the site too.
- The rebrand shipped in the same window — new identity, new type ramp, new spacing, new component library. Running it separately would have meant touching every page twice.
- Content never paused. The blog and academy kept shipping on the old site until the day we launched the new one.


With the base in place, we still had a mountain of work ahead — and most of it wasn’t engineering work. It was copy to review across hundreds of pages, design polish against the Figma file, the EN / ES / PT triplets to complete, vision calls on each section, and a content calendar that had to keep shipping.


That kind of work traditionally needs feedback and edits from the non-technical side of the team: product, design, growth. And that handoff — the moment the non-technical team has to come back to engineering to get their changes landed — is where projects like this usually stall. We solved it from the other side: we put the agents in the middle.


Between the people who own the content — our product manager, our designer, our growth lead — and the codebase. Two hours of onboarding: a short session on git basics, on how Cursor works, and on how to brief an agent so the instruction actually lands, then helping each person set up their local environment.


That environment was Cursor — and that’s where its integrated browser stopped being an engineering-tooling feature and became the literal visual editor for the site. Open the live preview next to the editor, use the inspector to point at a specific element, tell the agent what to change. The “Astro is still a CMS” line from earlier stopped being a reframe. Cursor was the visual editor; underneath every click was the actual codebase, and every edit was a diff.


By the next day all three were opening pull requests: new screens, copy edits, layout tweaks, entire new pages. Within a week they were moving faster than any of us expected. And the real surprise wasn’t the speed — it was watching them unlock skills they didn’t know they had. They didn’t learn to code. They learned to work with agents, and the agent translated each instruction into the matching diff.


A year ago, none of this would have been possible. Teaching a product manager, a designer, or a growth lead to ship through a git repo wasn’t a two-hour onboarding; it was the reason so many teams stay trapped in visual CMSes forever.


That was the actual unlock. We didn’t replace the hosted CMS with a better editor. We replaced it with a codebase that non-engineers could operate against, because an agent sat in between and did the translation. Without agents capable of that translation, we wouldn’t have attempted this migration. With them, the working surface of the whole team changed — not just engineering’s.


One call we’d make again: rebuild, don’t port. Component-for-component translation would have preserved decisions we already wanted to revisit. We kept the URLs and the content; we rebuilt everything else.


And a detail worth calling out for any team considering this path: the slow month that usually comes with a migration like this never showed up. From the first post on, writing it as markdown was faster than writing it in the old visual editor — hand the draft to the agent and it comes back as the EN / ES / PT triplet ready for review. By week five a new post was going from idea to published in English, Spanish, and Portuguese in less time than it used to take to get the first approval through the old CMS.


## The result


dailybot.com now scores a straight 100 on Lighthouse — on both desktop and mobile — across every category: performance, accessibility, best practices, and SEO. A full build runs in about 100 seconds on our CI.


The mobile 100 is the one that takes the most work to hold. If someone opens Lighthouse a month from now and sees a 95 on mobile performance, that is still a fast site — the number that matters is the floor, not the ceiling.


Lighthouse, desktop profile, April 2026. Most of the credit belongs to Astro's static-first output; we did not have to fight for this score. Lighthouse, mobile profile, April 2026. Same straight 100. The hard part on this profile isn't hitting it; it's holding it week to week.


The deployment story is part of how those scores hold. Every commit triggers an Astro build, and a few seconds later[Cloudflare Pages](https://pages.cloudflare.com/) publishes the site to its global edge. Production and every PR preview live on the same infrastructure, so anyone on the team — technical or not — can open a preview link and review a change live before it merges. That single property is what turned the non-technical workflow from “open a PR and wait for an engineer” into “open a PR and ping the reviewer.”


Underneath that workflow is the safety net that lets us trust it. Every PR runs the full pipeline before it can merge: TypeScript checks, Biome linting and formatting, the Vitest test suite, a Lighthouse CI run that holds the score floor on performance, accessibility, best practices, and SEO, and a clean production build. A broken type, a missing translation key, a regression in a content schema, or a render-blocking script slipped into a layout is caught in CI, not on the live site. That is what lets us hand the keys to non-engineers without it feeling reckless — the worst a bad PR can do is fail a check and ask for a fix.


The number we care about more is further down the stack. The time between “we should publish X” and “X is live in three languages” used to be measured in days. It is now measured in minutes for routine changes, and in one working session for new page types.


## Built for the agentic web


The second score we care about is newer. Lighthouse measures whether people and search engines can load the site. A different scorecard —[isitagentready.com](https://isitagentready.com/) — measures whether AI agents can actually work with it. It is an open proposal from Cloudflare for this new era, assembled from emerging standards the whole industry is converging on. You can read more about this in[AEO: From Invisible to Cited](https://xergioalex.com/blog/series/aeo-from-invisible-to-cited/) .


dailybot.com now scores a straight **100 / 100** there too, with every category at max — Discoverability, Content, Bot Access Control, and the API, Auth & Skill Discovery bundle. Level 5: Agent-Native.


isitagentready.com, April 2026. A second 100, for a scorecard the Lighthouse era didn't have.


Four concrete things make up that score:


-


**A public AI policy.** One line in` robots.txt` declares three explicit yeses: yes to training, yes to search indexing, yes to citations in AI answers. For a marketing site that exists to be found, anything else would be working against us.


-


**A map of the site.** A small set of public addresses points agents at our API catalog, our authentication story, and the capabilities we support. Like a hotel putting its services on a card by the door instead of hiding them in a drawer.


-


**Every page is also available as clean markdown.** Request any URL with an` Accept: text/markdown` header — or simply append` .md` to its path — and the response comes back as plain markdown instead of HTML: the format AI agents consume most readily. Two equivalent ways to retrieve this same post:


```text
curl   -H   'Accept: text/markdown'   https://www.dailybot.com/blog/how-we-migrated-dailybot-to-astro/
curl   https://www.dailybot.com/blog/how-we-migrated-dailybot-to-astro.md
```


-


**Tools they can use in their own browser.** A tiny bridge registers read-only actions — search the blog, find a help article — through an emerging open standard. Agent-enabled browsers pick them up automatically.


None of that is visible to a person browsing the site. All of it is visible to an agent. That is the point: the team moving our public surface now includes agents, and the site speaks their language.


Between the two scorecards, dailybot.com now clears every bar that matters on the modern web — performance, accessibility, SEO, content, and AEO, all green at the same time. Astro did most of that out of the box; the rest was a small set of deliberate artifacts that landed cleanly because the codebase was built to receive them. The site speaks the language the web is moving toward without giving up any of the language the web already speaks. Ready for the agent era, on a foundation that was already strong for everyone else.


## What became possible


Performance numbers are the easy part of a migration story. The part worth writing about is the work that the old cost structure used to prevent.


We ship blog posts the day we draft them. We spin up an entire category section — a new interactive asset gallery, a new academy pillar — in an afternoon and iterate on it live. We replaced static product screenshots with small Svelte-based interactive assets that a reader can actually use. The agents that help us draft, translate, and review are working against the same files our engineers are working against. There is no longer a separate “marketing site workflow” to teach them.


dailybot.com now ships in English, Spanish, and Portuguese out of a single repository. Every post, page, and help article is authored as a file per language, validated against a typed content schema, and cross-checked at build time — parity is a compile error, not a review-cycle worry. Adding a fourth language is a day of engineering and a queue of translation work; it is not a project we have to schedule around a vendor roadmap.


For a team our size — small enough to feel every inefficiency, large enough that public-surface work is constant — that is the entire point.


## What we’d tell another team


- **The editor experience is not the bottleneck. The pace is.** Decide whether your marketing site is a content property or a product, and choose a system that matches.
- **Your non-engineers can now ship to the public surface. Plan around that.** A year ago, moving the marketing site into a repo meant pushing the content work back onto engineering or building a visual editor on top. Neither is necessary anymore, if your coding tool and your design/preview surface are the same thing. We are not teaching our team to code; we are teaching them to brief an agent.
- **Rebuild, don’t port.** Component-for-component translation preserves decisions you don’t want to keep. If a migration is worth doing, spend the extra week on a real redesign in the same window.
- **Make i18n a system property, not a manual one.** Astro Content Collections with a typed schema per locale turned “are the three languages in sync?” from a spreadsheet into a build error. That alone paid back the migration cost.
- **The visual-editor CMS we had is still excellent.** But if you want to move at the pace of the new AI era, it is no longer enough.


## The real deliverable


What changed isn’t our site: it’s who can move it. Before this migration, every change to dailybot.com went through engineering. Today, our PM, our designer, and our growth lead open PRs every day. The metric we care about most isn’t Lighthouse or build time — it’s how many people on the team can push the public surface forward on a random Tuesday.


That’s the migration that matters, and the only reason we could pull it off is that the agents arrived. Without them, this story doesn’t exist: the visual editor would still be the shortest path. With them, the shortest path became a repo the whole team can operate against, because the agent translates.


If your team is where we were at the end of 2025, the decision isn’t really about Astro, or Webflow, or any specific stack. It’s about who you want moving your public surface a year from now. Our answer was: everyone on the team. That changed the shape of how we work more than any platform choice ever could.
