---
schema_version: "1.0.0"
document_id: "84871e47190f0e3797f87fb9ee550aa24cd7bcd3a91009a53144f058efce53e0"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/introducing-ai-assistant-sherlock"
published_at: "2025-12-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:55:00.595584+00:00"
content_hash: "sha256:1f41b18fa41b5446ca92cc0b68080ed535801c1dc10f498213918939119aa4ae"
---

# Introducing Sherlock: AI assistant for Depot docs

Our AI assistant for the[Depot docs](https://depot.dev/docs) is live! Sherlock gives you a new way to search and find answers exclusively in our documentation.


We built Sherlock for readers that prefer AI search to other ways of exploring content. You can use Sherlock to retrieve and assemble information for whatever you need at the moment.


## Adopting Sherlock


Sherlock’s name and image come from co-founder Kyle’s family pet and our company mascot, Sherlock the Corgi.


One of our coveted Sherlock stickers


Sherlock the AI assistant is a Docs + Support + Co-Founder (thanks Jacob!) collaboration from our very first offsite back in October. Before the end of our one-day hackathon, we had a working chat bot for our docs. Like many proof of concept apps, only a few pieces remain in the production version. In Sherlock’s case, that’s the bulk of the system prompt.


If we’re being honest, we built Sherlock partly because we had a strong feeling that users *expect* to see an AI search option. But anecdotally we knew that some users *prefer* using AI to search a site. Any tool that helps users access the information they need to use Depot or to get unstuck is worth adopting.


We love how Sherlock can use key details from different docs to create a response that helps you get set up or solve a problem faster.


## How to use Sherlock


From the[Depot docs](https://depot.dev/docs) , click **Ask AI** at the bottom right.


Find Sherlock on the docs site


Type your question in the chat modal.


Sherlock chat modal


Refine Sherlock’s response or explore further by asking follow-up questions. If you want to start a fresh Sherlock session, refresh your browser.


You also have the option to switch to Sherlock from regular search results. Click the banner to forward your query to Sherlock.


Switch from regular search to working with Sherlock


## How Sherlock works


We built Sherlock using the[AI SDK](https://ai-sdk.dev/) , with custom tools for the following:


- ` list_pages` : list all documentation pages
- ` grep` : search documentation with grep
- ` read_page` : read the content of a specific docs page


The tools have direct access to the[markdown source](https://github.com/depot/docs) for our documentation site. This allows Sherlock to explore and understand the up-to-date documentation to answer questions. You can see Sherlock using those tools in real time as it compiles a response.


### Key features


- Reliable answers because Sherlock searches and formulates replies from only Depot content
- Links to the sources Sherlock used for the answer
- Concise, task-focused answers


## What we’re still working on


We’d love to build a completely customized docs experience, where readers can always successfully assemble and build docs exactly suited to their use case. But we also want to let users explore and learn on their own when they don't know what they need yet.


Both of these require a great set of docs as a foundation. We’re always working on that foundation: user-focused, deliberately-crafted documentation.


And of course, we’ll keep iterating Sherlock’s prompt and programming to make sure you’re getting the best possible answers.


## What’s next


Today Sherlock lives in our public documentation. Next, we want to make Sherlock available in the[Depot dashboard](https://depot.dev/orgs/) , so you can have help with the right context, right where you need it.


We’d love to hear what you think about Sherlock or anything else in the Depot docs. Email us or join our Discord Community from our[help page](https://depot.dev/help) .


## Related posts


- [Depot documentation is now open source](https://depot.dev/blog/depot-docs-now-open-source)
- [Collaborating with Claude on docs](https://depot.dev/blog/collaborate-with-claude-on-docs)
- [What technical writers actually do at startups](https://depot.dev/blog/what-technical-writers-do-at-startups)


Pedro Guerra


Enterprise Support Lead at Depot


Andrea Anderson


Technical Writer at Depot
