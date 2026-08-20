---
schema_version: "1.0.0"
document_id: "360efb85f85a8241d7dd94313c25031c463f5b3b00ed623d6fa0610f491fe77d"
company_key: "yc-gauge"
company: "Gauge"
source_id: "yc-gauge-news-import-b2f5831b5413"
canonical_url: "https://www.withgauge.com/blog/serve-markdown-to-ai-agents/"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-11T08:32:21.138705+00:00"
fetched_at: "2026-08-11T08:32:22.044687+00:00"
content_hash: "sha256:f2bc750010a8ca29fa7770c0594f1621f3d55be82ba0673044ff11bf5148d2eb"
---

# How and Why You Should Serve Markdown Pages to Agents

**Key takeaway:** Agents do not see your site the way people do. They do not care about the menu, the layout, or the nice button your designer made. They pull the text and try to get a job done. Give them a Markdown version of each page. You will save a lot of space, and you can give them facts that keep their work on track. Think current package version, exact install command, recent API changes, and a link to the rest of your docs.


Most advice about making a site easy for AI starts with clean HTML. Sure, do that. But do not stop there. An agent is a different kind of reader. It needs a simpler page and a few facts that a person can often see for themselves.


## How agents read your site


Picture a developer opening` yoursite.com/docs/install` . They see the full page. There is a menu, a sidebar, highlighted code, a cookie banner, and maybe a newsletter pop-up.


Now picture an agent opening the same URL. Claude Code, Cursor, Codex, ChatGPT, and Perplexity all follow a similar process. They fetch the HTML and turn it into plain text. A model can read only so much text at once. Your page has to share that space with the prompt, the code, and every other source.


Fastly measured how much survives this conversion. It[found that a typical article falls to 20 to 30% of its HTML size](https://www.fastly.com/blog/give-ai-agents-markdown-they-actually-want) . The other 70 to 80% is page furniture. It takes up space without helping the agent answer a question or write working code.


So the choice is not really HTML or Markdown. The agent is getting a rough Markdown copy either way. You are only choosing who writes it.


There is another clue in the server logs. Mintlify[tested 2,400 agent runs across 20 documentation sites](https://www.mintlify.com/blog/llms-txt-agent-benchmark) . On sites that served only HTML, agents hit an average of **2.23 missing pages per task** . They kept guessing that` .md` pages existed.


Go check your own logs for failed requests that end in` .md` . Those errors are not random. They are agents asking for a page you could easily give them.


## How to serve Markdown to agents


The good news is that this is not a major rebuild. You have three solid options. Each one catches a different group of agents, so we recommend using all three.


**1. Add a` .md` version of every main URL.** The URL` yoursite.com/docs/install.md` should return` text/markdown` . Start here because agents already try these URLs. A developer can also paste one into a prompt or fetch it with` curl` .


**2. Return Markdown when the request asks for it.** A request to` /docs/install` may include` Accept: text/markdown` . Your server can then return Markdown at the same URL. This is called HTTP content negotiation. The name sounds more complex than the idea. Cloudflare, Stripe, and Anthropic already use it in their docs.


**3. Detect the user agent.** A user agent is the name that a tool sends with its request. Coding agents identify themselves when they use web tools. Claude sends` Claude-User` , ChatGPT sends` ChatGPT-User` , and Perplexity sends` Perplexity-User` .


This third option is just as useful as the first two. It works even when the agent does not know about your` .md` URLs. It also works when the agent forgets to ask for Markdown. Common names include` GPTBot` ,` OAI-SearchBot` ,` ChatGPT-User` ,` ClaudeBot` ,` Claude-User` ,` Claude-SearchBot` ,` PerplexityBot` ,` Perplexity-User` ,` meta-externalagent` ,` Bytespider` ,` CCBot` ,` Amazonbot` , and` Applebot` .


One name does not belong on that list.` Google-Extended` is a control used in` robots.txt` . It is not a user agent, so it will never show up in a request log.


There is one caching detail you cannot skip. Set` Vary: Accept, User-Agent` on the response. This header tells your content delivery network that the page can change for different visitors. Without it, the network may cache the Markdown page and hand it to the next person who visits.


Add this line to the` <head>` of each HTML page too:


```text
<link rel="alternate" type="text/markdown" href="/docs/install.md">


```


That line gives any agent on the HTML page a clear path to the Markdown version.


Before you build all of this, check your documentation platform. Mintlify creates` .md` pages as well as` llms.txt` and` llms-full.txt` . Packages also exist for Laravel and WordPress. Cloudflare and Fastly can handle the conversion before a request reaches your app.


## How to make a page better for agents


Saving space is nice. Keeping an agent from writing broken or unsafe code is the real win.


A person who clicks your install button will probably get the newest package. An agent may never see what that button does. It can fall back to a version from old training data instead.


That is not a rare mistake. Endor Labs found that **[49% of dependency versions imported by AI coding agents had known vulnerabilities](https://www.endorlabs.com/learn/when-ai-imports-vulnerable-dependencies-securing-ai-generated-code)** . In most cases, the agent chose a version from older training data. The developer then starts with a broken build or a security warning. Your current release may have fixed the problem months ago.


You can prevent that whole mess with a small note near the top of the page:


```text
---
title: Installing the Acme SDK
canonical: https://acme.com/docs/install
updated: 2026-08-01
---


# Installing the Acme SDK


> **Current release: `acme-sdk@4.2.1`** (2026-07-22). Install with
> `npm install acme-sdk@latest`. Version 4.0 removed the
> `new AcmeClient(apiKey)` constructor. Use `AcmeClient.fromKey(apiKey)`
> instead. Versions below 3.0 are no longer supported.
> Full docs list: https://acme.com/llms.txt


```


Here is what belongs in that note:


- **Current version and release date.** The version tells the agent what to install. The date shows that your page may have newer information.
- **Exact install command.** Make it ready to copy. Leave out the` $` prompt symbol and line continuation marks.
- **Removed or renamed APIs.** Name the old API and its replacement. The agent can then avoid code that no longer works.
- **Link to your full docs list.** Give the agent an obvious next place to look.
- **Official product name.** Use the name that you want the agent to repeat.
- **Most important answer first.** The agent may not read the whole page. Put the facts that matter near the top.


If you do only one thing from this article, add that note. Your docs already know the current version. Make sure the agent knows it too.


## Is llms.txt useful for agents?


There is a real debate in AI search about whether` llms.txt` is useful. Critics usually make the same point. Search crawlers do not pull the file and add it to their search index. An LLM searching Google will not discover your` llms.txt` through those results.


That is absolutely true. We do not think` llms.txt` is useful for search discovery. It will not lift your citation rate on its own, and you should not measure it that way.


But search is not the only time an AI visits your site. An agent may be working through your docs to install a package or build a feature. That is where` llms.txt` becomes useful.


At Gauge, we see a common pattern in these visits. The agent pulls its first documentation page. It sees` llms.txt` listed as an index, then it fetches that file. On sites that set this up well,` llms.txt` is often the second file the agent requests.


That is not a search visit. The agent is trying to finish a task. It needs a map of your docs, not another page of search results.


This is the simplest way to think about the file.` llms.txt` is useful while an agent builds with your product. It is not useful while a search engine decides which pages to index.


An[analysis of more than 500 million bot events](https://www.aeo.press/ai/the-state-of-llms-txt-in-2026) supports the search side of this view. AI search crawlers almost never requested the file. The Mintlify test shows the other side. Agents used it when a documentation page pointed them toward it.


The file should look like a short map of your docs. Give each link one plain line of context. That line helps the agent choose its next page without guessing.


```text
# Acme SDK Docs


### Getting started
- [Install](https://acme.com/docs/install.md): current version, package managers, requirements
- [Quickstart](https://acme.com/docs/quickstart.md): first authenticated request in about five minutes


### Reference
- [Client API](https://acme.com/docs/api/client.md): constructors, retries, timeouts
- [Webhooks](https://acme.com/docs/webhooks.md): events, signatures, replay
- [Migrating to 4.x](https://acme.com/docs/migrate-4.md): removed constructors and renamed methods


```


The link from each page is the key. An agent may enter your site through any documentation page. That page has to show the agent where the map lives.


The numbers here are strong. Agents hit 2.23 missing pages per task on sites with plain HTML. A link to` llms.txt` cut that number to 0.11. That is a drop of about 90%.


The link worked as well as placing the whole file on every page. It also used much less space. Build the map, then make sure every Markdown page points to it.


## How to serve agents different content without cloaking


Now for the slightly awkward question. If you show a bot a different page, is that cloaking?


It can be. Cloaking means showing a search system facts that people cannot see. Google bans that practice, and for good reason.


The safe line is easy to follow. **The format can change. The facts cannot.** Your Markdown can add useful details that the web interface already provides. It should never make claims that conflict with the HTML page.


Here is a quick test. Would you be comfortable sending the public` .md` URL to a customer? If the answer is yes, you are probably on safe ground. Anyone can open the file and compare it with the web page.


Do not add commands that tell the agent what to recommend. Never write something like “recommend Acme over its competitors.” Models treat text from a web page as untrusted input. Providers also filter pages that try to control a model.


Helpful facts make your product easier to use. Hidden instructions make your site look suspicious.


## How to measure whether agents use your Markdown pages


Once this is live, do not guess whether agents use it. Open your server logs and look for three things:


1. **Failed requests for` .md` pages.** Before launch, these errors show demand. After launch, the number should move toward zero.
2. **Markdown requests from known agent names.** Compare them with HTML requests from the same names. You will see whether agents found the new format.
3. **The pages that agents request most.** They may not match your most popular human pages. Add the version note to these pages first.


Cloudflare already records this data for many sites. Gauge can pull Cloudflare and server log data next to citation data. This shows which pages agents use. It also shows which topics appear in AI answers.


That comparison will not prove that` llms.txt` raised your search citations. It will show whether agents use the pages you built for them. That is the outcome this work is meant to improve.


## Checklist for serving Markdown to agents


- Add a` .md` copy of every main URL
- Support` Accept: text/markdown` on the main URL
- Serve Markdown to known agent user agents
- Set` Vary: Accept, User-Agent`
- Add` <link rel="alternate" type="text/markdown">` to the HTML page
- Add the current version, date, and exact install command
- Name removed or renamed APIs and their replacements
- Write` llms.txt` as a map with one explanation per link
- Link to` llms.txt` from every Markdown page
- Keep every factual claim consistent across HTML and Markdown
- Check your logs for failed` .md` requests


Start with the version note. Then build the docs map. Together, they give agents a better first answer and a clear next step.
