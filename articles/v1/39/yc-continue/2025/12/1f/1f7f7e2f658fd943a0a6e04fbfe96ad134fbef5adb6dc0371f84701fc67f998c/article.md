---
schema_version: "1.0.0"
document_id: "1f7f7e2f658fd943a0a6e04fbfe96ad134fbef5adb6dc0371f84701fc67f998c"
company_key: "yc-continue"
company: "Continue"
source_id: "yc-continue-rss-8ad3fe8c275c"
canonical_url: "https://blog.continue.dev/ai-is-glue"
published_at: "2025-12-03T18:22:14+00:00"
first_seen_at: "2026-07-24T23:23:23.996128+00:00"
fetched_at: "2026-07-28T20:55:05.658267+00:00"
content_hash: "sha256:159511d0cf0278a014e9e6d2cb8ff222dab38f0036cbd9be24b218bb17d3df8a"
---

# AI is Glue

I've been thinking a lot about glue recently.


I was up at my cabin in the mountains, repairing a broken shower door. The acrylic sheet had cracked and I needed to bond it back together. I learned that to get a strong bond you need the right welding epoxy, but you also need to sand the surfaces first. Sanding gives the adhesive more bite, more texture to grab onto. The rougher the surface, the stronger the bond.


It got me thinking about glue code.


## **The Old Glue**


We used to talk a lot about glue scripts. Perl was the original["Swiss Army chainsaw"](https://www.salon.com/1998/10/13/feature_269/) of the internet, the archetypal glue language for making complex and uncooperative systems work together. Then Python took over as the glue language of choice. Shell scripts piped commands together. One tool's output became another's input. Extracting values from JSON to fill flags on the next command. Brittle, but it worked.


LLMs are the new glue. They can bond together varying surfaces: human language, CLIs, APIs, webhooks, databases. Surfaces that never fit together cleanly before.


## **The Aerolite Moment**


In the 1930s, aircraft builders used casein glue to bond wooden components. Casein is milk-based, basically hardware store wood glue. It worked for furniture. It wasn't strong enough for high-performance aircraft.


Then Norman de Bruyne developed[Aerolite](https://en.wikipedia.org/wiki/Aerolite_(adhesive)) , an adhesive that formed bonds stronger than the wood itself. The[de Havilland Mosquito](https://en.wikipedia.org/wiki/De_Havilland_Mosquito) was the result: one of the fastest aircraft of its era, built from materials that had been around for decades.


## **The Rough Surfaces**


Think about the interfaces we work with every day: webhooks that send JSON blobs, CLIs that expect specific flags, REST APIs that return nested objects, databases with their own query languages. Each one has its own modality, its own input format, its own output format. They're wildly different surfaces.


That's the sanding. That's the texture.


LLM glue works because it can get into the nooks and crannies. Read a webhook payload, extract the relevant fields, transform them into the exact format a CLI expects, parse the output, feed it into the next tool. All those rough, irregular surfaces give the glue something to grab onto.


This is exactly why it works so well. Condensing context, extracting specific values, filling in known patterns like CLI flags. These are what LLMs are good at. This isn't one-shotting thousands of lines of code into existence. This is playing to the model's strengths.


We[just announced a partnership with Snyk](https://snyk.io/blog/snyk-continue-partner-integration/) to embed AI-powered security into the developer workflow. Here's what glue looks like in practice:


1. \[Webhook\] Snyk sends a vulnerability alert
2. \[LLM\] Extract severity and affected file path
3. \[Filesystem\] Check if path is in production code
4. \[LLM\] Generate a patch based on vulnerability and code context
5. \[CLI\] Run tests
6. \[LLM\] Parse CODEOWNERS for the affected path
7. \[CLI\] gh pr create --assignee <extracted_value>


The GitHub CLI does the work. The Snyk API provides the data. The LLM is just glue, filling the gaps between tools, reducing large context down to specific values for the next deterministic step.


## **The Practice**


Every team has tools they trust. CLIs for their infrastructure, APIs for their services, test runners that catch bugs, issue trackers with established workflows.


You don't need new tools. You need glue.


Pick a practice that's been impractical. Map the tools you already use for each step. Identify where you need the LLM to extract, decide, or transform between them. The rougher the interfaces, the more the glue has to grab onto.


We couldn't build high-performance wooden aircraft until we had adhesive strong enough to bond the surfaces. The materials were always there.


---


Try it:` npx continue@latest` to build your first glue workflow or get[started in two clicks](https://blog.continue.dev/blog/introducing-tasks-and-workflows) in[Mission Control](https://hub.continue.dev/) .
