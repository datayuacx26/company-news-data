---
schema_version: "1.0.0"
document_id: "0b6192c716ec36dafab2cec0b2e33df8e5eb962c5c4938bf692645c4240588db"
company_key: "amplitude-inc-class-a-common-stock"
company: "Amplitude Inc."
source_id: "amplitude-inc-class-a-common-stock-news-import-1333a773138e"
canonical_url: "https://amplitude.com/blog/docs-redesign"
published_at: "2026-06-02T00:00:00+00:00"
first_seen_at: "2026-07-22T23:44:16.700638+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:5746196a73c742b126f11bbc738da4b9cb2b0e4bc166c3769414694c667756db"
---

# How We Redesigned Amplitude Docs for Agents and Made Everyone an Author

In the first 18 days after we launched the new Amplitude docs, LLM crawlers requested more pages than humans did. Bots accounted for 198,843 page views. In the same window, 37,561 unique humans loaded 124,044 pages.


The bots aren’t winning a popularity contest. They’re doing a job: answering questions about Amplitude on behalf of people who never visit[amplitude.com/docs](http://amplitude.com/docs) directly. That ratio is why we rebuilt the docs. The old site treated humans as the only readers worth designing for. The new site treats agents as a first-class audience and gives them, along with the humans they work for, a much shorter loop from question to answer to merged change.


This is the story of what we built, why, and what changed when we stopped treating docs as a website and started treating it as a programmable surface.


## Why move off the old stack


Amplitude’s docs ran on a Statamic site for almost three years. It was fast, the editorial workflow was familiar, and the templates kept the design tight. Three forces eventually outgrew it:


- **Build time.** The old stack statically rendered every page during each build, so as the corpus crossed 800 documents, builds stretched past the point where contributors trusted the preview loop. A typo fix could take several minutes to render and verify, which is often longer than the fix itself took to write. A docs site that punishes iteration produces fewer iterations.
- **Localization.** The old stack assumed a deeply connected taxonomy in which every page sat inside collections and structured fields. That model can’t accommodate an automated translation pipeline that needs to mirror 800-plus English pages into Japanese, keep slugs aligned, and gracefully fall back when a translation hasn’t landed yet.
- **LLM discoverability.** The old templates produced clean HTML, but they didn’t expose structured metadata, raw Markdown, or a programmatic endpoint that an agent could reason about. The fastest-growing audience for docs is not a human in a browser. It’s an agent in a tool call. We saw that play out in the first three weeks of our redesign.


The brief for the new site was simple: Keep the docs-as-code spine, shed the deep taxonomy, optimize for fast iteration and translation, and treat LLMs as first-class consumers from the first commit.


## The 49-day rebuild


The first commit landed on March 17, 2026. The site went live on May 4. In between: 495 commits, one primary author, and a small army of agent-assisted migration tasks. The shape of the work mattered as much as the timeline.


**Day one was building the platform.** In the first afternoon, the repo gained a Next.js 15 App Router scaffold, a YAML-to-JSON navigation compiler, a content resolution library, **next-intl** for locale routing, Amplitude SDK instrumentation, an AI menu, an MCP server with **get_page** , **list_pages** , and **search_docs** tools, Pagefind search, and an on-demand revalidation endpoint wired to GitHub Actions. Architecture first, content second. Forty-nine commits in a single day.


**Weeks two and three were migration engineering.** The single biggest pull request of the project moved content, navigation, and images out of the legacy system in one shot, using a programmatic Antlers-partial-to-React component map. Smaller follow-ups fixed Vercel build paths, rewrote internal links, and regenerated the sitemap. The corpus didn’t grow because anyone retyped it; it grew because we wrote the rewriter once.


**April was the product surface.** Markdoc replaced the early MDX bootstrap. The SDK catalog collapsed into a single **/sdks** index. The API reference got a catalog template and proxy routing. Hybrid semantic search, reranking, and a tuning harness landed over the course of the month and became the default ahead of launch. SEO got JSON-LD, per-page OG images, and a generated sitemap. The public Docs MCP server shipped. A doc review gate appeared in CI early in the month and was promoted to the single required check on the last PR before launch.


**May 4 was operations.** Sixteen commits in a single day to set the **/docs** basePath, fix asset URLs, fix API URLs, redesign the FAQ, point dev and preview URLs to the new docs, and clean up the migration artifacts. Nothing glamorous. The unglamorous list is what go-live actually looks like.


## What the new stack looks like


The platform runs on Next.js 15 with the App Router and static generation, Markdoc for content, Tailwind CSS 4 for styling, and Upstash hybrid search for retrieval. Navigation lives in YAML under **nav/** and compiles to a single **docs.config.json** at build, so the sidebar is data, not a template. Locales render through **next-intl** , with English in **content/en/** and Japanese in **content/ja/** , falling back to English when a translation hasn’t landed yet. Build time dropped because static generation parallelizes cleanly, Markdoc parsing is cheaper than the old template engine, and the nav compile is a single pass rather than a per-page taxonomy lookup.


**Old stack (Statamic)**


**New stack (Next.js 16)**


Content format


Markdown + Antlers templates


Markdoc


Build model


Full rebuild on every change


Static generation, parallelized


Navigation


Deep taxonomy per page


YAML → compiled JSON, single pass


Localization


–


next-intl, English fallback


Agent access


None (HTML scraping only)


MCP server, raw Markdown API, JSON-LD


Search


Algolia DocSearch


Upstash hybrid (lexical + semantic)


## Five things that make agents first-class readers


**1. A public, read-only docs MCP server.** Every doc page is reachable through a Model Context Protocol endpoint at **amplitude.com/docs/api/mcp** . The server exposes three tools, **get_page** , **list_pages** , and **search_docs** , over Streamable HTTP. An agent connected to this server can browse the corpus, search it, and fetch the raw page for any URL without scraping HTML. In the first 18 days post-launch, the endpoint served 4,262 completed requests.


****


**2. A raw Markdown endpoint.** Each page is also available as raw Markdoc at **/api/content/\[...slug\]** . The in-product AI menu uses this endpoint to power Copy-as-Markdown, Open-in-Claude, and Open-in-ChatGPT actions, so a reader can pivot any page directly into the agent of their choice. Early usage skews exactly how you’d expect a developer audience to use it: Of the 303 AI menu selections post-launch, 234 were Copy-as-Markdown (readers grabbing the source to paste into their own agent), followed by view-raw-Markdown, then Ask Claude and Ask ChatGPT.


**3. Structured metadata on every page.** Every doc page emits schema.org JSON-LD ( **TechArticle** , **BreadcrumbList** , and **FAQPage** where appropriate) plus per-page Open Graph images generated at 1200×630 by **next/og** . Crawlers and link previews get the same structured signal that a human reader does.


**4. Bot crawl analytics.** The proxy layer logs every LLM crawler hit to Amplitude as an **LLM Bot Crawl** event. The tracked list mirrors our company-wide allowlist: GPTBot, ChatGPT-User, ClaudeBot, Claude-User, PerplexityBot, Google-Extended, Applebot-Extended, Meta-ExternalAgent, and the rest. The same instrumentation powers privacy-first MCP usage events (request completion, rejection, failure) with no raw queries, slugs, or response bodies recorded. We can see that ChatGPT-User is the dominant fetcher, that Bytespider crawls aggressively, and that Claude bots are growing, without touching what anyone actually asked.


**5. Search built for hybrid retrieval.** Search runs against Upstash Vector with both lexical and semantic ranking. A scheduled indexer reingests **content/en/**** only when source files have changed since the last successful run, and a 9-of-9 hit@1 eval gate must pass before the checkpoint advances. Result badges surface the product surface (Analytics, Admin, SDK) so agents consuming the search API get useful classification metadata, not just a URL.


## Equipping agents to contribute, not just read


Reading is half the job. The repo also documents and tools the contribution loop so that agents can safely propose changes, and so the humans steering them can move faster.


Agent-facing guidance lives in five files at the repo root: **CONTENT-AUTHORING.md** for frontmatter and Markdoc syntax, **COMPONENTS.md** for available tags, **DESIGN.md** as the design-system source of truth, **TRACKING.md** for the instrumentation contract, and **SEO.md** for canonical URLs and sitemap behavior. The site is agent-agnostic. Claude, ChatGPT/Codex, and Cursor all work because the rules are written for any reader, not embedded in a single tool’s prompt.


The skill catalog is the lever that makes this practical. **edit-doc** applies the Amplitude style guide to a document. **content-refine** rewrites prose for translatability and LLM readability: short sentences, no idioms, no ambiguous pronouns, no gendered language. **link-check** and **link-fix** find and repair broken internal links across the corpus. **feature-comparison** audits the docs against shipped features and drafts missing pages on their own branches.


The shortest path to launching one of these agents runs through Slack. Claude and Cursor are both connected to a channel called **#docs-vibe-author** . Both agents have the full repo context and the skill catalog loaded on connect, so they can produce publication-ready content in most cases without anyone opening an editor. Someone posts, “The SDK quickstart is stale, please refresh it against the current browser SDK version,” and the agent picks the right skill, opens a branch, edits the pages, runs the style pass, and opens a PR. The thread becomes the work log. The PR carries a **Doc-Reviewed-By: skill** commit trailer, signaling to CI that the style pass already ran, so low-risk English edits flow through with a single required check instead of a full human pass.


That changes who can contribute to the docs. A docs typo or a missing SDK note used to require a human to open the repo, find the file, write the edit, and push. Now it requires someone to describe the problem in a sentence in Slack. The skill catalog and the review gate take it from there. Everyone at Amplitude is a docs contributor whether they know how to write Markdoc or not.


## Compressing the GitHub loop


The shorter the loop from idea to merged change, the more often the corpus improves. Five workflow files in the repo do most of that work.


**doc-review.yml** runs on every PR. Its **Review gate** job is the single required check. Low-risk English doc edits with a **Doc-Reviewed-By: skill** trailer can merge once the gate passes. Higher-risk changes (code, nav, new docs, non-English content, prose changes over 200 words) still need human approval, but the gate is the only required check, so reviewers don’t chase stale failures. When approval lands, the workflow reruns failed jobs on the same commit and posts a single **Doc Review Ready** comment that @-mentions the author. A bi-weekly reviewer rotation routes higher-risk PRs to the on-rotation reviewer through GitHub and Linear.


**vercel-deploy.yml** posts one preview comment per PR, with the deployment URL, branch metadata, and direct preview links for up to three changed English pages. The page a reviewer wants to click is one tap away.


**index-search.yml** runs every six hours, reindexes only when content has changed, runs the 9-of-9 eval gate, and advances a **search-index-last-success** tag on success. Authors can force-dispatch the workflow when they need to bypass the checkpoint.


**sync-sdk-metadata.yml** keeps **data/sdk-metadata.json** current and only opens a PR when non-timestamp metadata changes. Code samples that reference {{sdk_versions.browser}} pick up new versions automatically.


**smoke-canonical.yml** validates canonical URLs, sitemap output, and robots behavior after every deploy, so the SEO contract documented in **SEO.md** doesn’t regress.


## What this unlocks


The most interesting outcome is that the docs-as-code surface now has two equally weighted readers, humans and agents, and the contribution loop is short enough that either one can fix a typo, refresh an SDK version, or draft a missing page without waiting on a long build or review.


Eight hundred and thirty-four English documents and roughly 820,000 words are a lot to keep accurate. The bet behind this rebuild is that the only way to keep a corpus that size honest is to make every part of it (content, navigation, search, review, and deploy) legible to the agents that increasingly help write it, and accessible to the agents that increasingly read on behalf of users.


Three weeks in, the agents have already cast their vote. They’re reading more pages than humans are.
