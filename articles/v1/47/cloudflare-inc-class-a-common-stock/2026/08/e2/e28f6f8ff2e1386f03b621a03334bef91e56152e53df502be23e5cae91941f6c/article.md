---
schema_version: "1.0.0"
document_id: "e28f6f8ff2e1386f03b621a03334bef91e56152e53df502be23e5cae91941f6c"
company_key: "cloudflare-inc-class-a-common-stock"
company: "Cloudflare Inc."
source_id: "cloudflare-inc-class-a-common-stock-rss-72eb55cabac1"
canonical_url: "https://blog.cloudflare.com/introducing-radar-researcher/"
published_at: "2026-08-07T13:00:00+00:00"
first_seen_at: "2026-08-07T15:08:08.883286+00:00"
fetched_at: "2026-08-07T15:08:09.986593+00:00"
content_hash: "sha256:3b48ca2f60be132072093969eabebbcc474f2b06cfcbbda265af8d6fcfcbd65a"
---

# Introducing Radar Researcher: An AI tool for exploring Internet data in plain language

Since launching in 2020,[Cloudflare Radar](https://radar.cloudflare.com/) has provided one-of-a-kind insight into Internet traffic on a global scale. The open data that Radar publishes from our global network is used by a variety of experts in different domains: human rights advocates, journalists, academic researchers, network operators, and more.


Today, as part of[Agents Week](https://blog.cloudflare.com/agents-week-welcome/) , Radar is beta-launching a new tool: Radar Researcher.


Now, if you want to answer a question with Radar’s data, you can just ask in plain language and get an answer back with real, interactive charts, all built on Cloudflare's developer platform. You no longer need to find the right page, choose the right filters, or read the API documentation to write a query.


## Why we built Radar Researcher


Over the past six years, the Radar team has worked to provide clear, accessible, reliable data visualizations of complex datasets. This is not without its challenges: our user base is vast and ranges from curious novices to technical network experts. Even so, we publish as much insight into the Internet as seen from our network as we can: DNS queries from our public resolver[1.1.1.1](https://one.one.one.one/) , HTTP traffic from across our global network, network quality data collected from[Cloudflare Speed Test](https://speed.cloudflare.com/) , and so much more. We also believe in the power of open, public data, which is why everything you see in Radar’s data visualizations is available via our free[API](https://developers.cloudflare.com/api/resources/radar) . Every year we make more and more of the Internet visible.


But as Radar has evolved, so has the technology for working with data. AI tools are lowering the barrier to expertise: you no longer need to know a dataset's structure or vocabulary to get useful answers from it. Subject-matter experts become collaborators rather than gatekeepers.


Radar has always wanted to do more for users who shouldn't need to master our entire catalog and its vocabulary to fully appreciate our insights. (See our recent blog post on[how the Internet responded to the 2026 FIFA World Cup](https://blog.cloudflare.com/2026-world-cup-internet-traffic/) .)


We also know that many of our users are in a hurry. For example, a journalist writing about the latest Internet outage likely does not have the time to search through each page for the perfect graph to tell their story. This is why we built Radar Researcher.


## Meet Radar Researcher


[Radar Researcher](https://radar.cloudflare.com/?showResearcher=true) is built to make accessing data as easy as describing what you’re looking for. All information is provided via Radar’s API, which ensures that the underlying LLM relies on real data, and can answer your questions quickly and in the same format as you would articulate them to a human.


It's available on every Radar page. Open the Radar Researcher panel from the header, and it docks beside whatever you're looking at, so you can keep browsing while you chat. Here are some examples of how you can use it:


- **Get an answer in plain language, with real charts** : Ask a question and get the same interactive visualizations you see across Radar, plus a short explanation.
- **Choose your depth** : Pick a concise, direct answer or a fuller, multi-topic report.
- **Keep the analysis going** : After each answer, it suggests useful follow-up questions.
- **Pick up where you left off** : Conversations are saved in a searchable history you can pin and revisit, and you can share any conversation with a link. (Shared links expire automatically after 30 days.)
- **Audit the reasoning** : Check how the LLM interpreted your question, which datasets it looked up and queried, and how it worked through the results to reach its answer.
- **Ask your way** : Type, use voice input, or launch it straight from Radar's search bar.


### Turn any chart into a conversation


Researcher doesn't only start from a blank prompt. Anywhere you see a chart on Radar, an ***Explain with AI*** action opens the assistant with that exact visualization already in hand.


Instead of asking you to describe what you're looking at, it captures the exact visualization you're viewing and starts the conversation there. As you can see below, Radar Researcher takes the visualization in context, then analyzes the data and responds with an explanation.


Under the hood, the assistant hands the model three things at once: a screenshot of the chart (a vision-capable model reads the image directly), the exact data behind it from Radar's API, and the parameters of your current view, including location, date range, and any filters you've applied. The screenshot enables Radar Researcher to see what you see. The raw data keeps every number it cites precise instead of estimated from pixels, and the filters mean the explanation is about your exact view, not a generic version of the chart.


### Use Case No. 1: What’s Internet quality like in Portugal?


Say you're curious how good the home Internet is in a given country. You can just ask:


Instead of manually calling the API, choosing parameters, or hunting through Radar's pages, Radar Researcher interprets the question and does the work for you. You can follow each step in the screenshot: it reads what you asked, queries Radar's Internet quality API, then analyzes the results and answers in plain language. And rather than a wall of numbers, it renders the same interactive charts you'd see on the[Radar Internet Quality](https://radar.cloudflare.com/quality/pt#connection-quality) section.


From here you might ask a natural follow-up, like comparing Portugal to a neighbor, and Radar Researcher fetches fresh data for each one. It even suggests useful next steps of its own, such as how Portugal stacks up against Spain or the country's most common Internet outages. By letting you find what you're looking for in plain language, we hope applying Radar's data to your own analysis is easier than ever.


### Use Case No. 2: Investigating an Internet disruption


Radar Researcher is also built with technical experts in mind. If we know one thing about engineers, researchers, and network operators, it’s that automation is key to their workflows. Looking at the same data with manual actions can be cumbersome and limiting: how do you ensure you're always looking at the most relevant data?


Consider a real event. In early 2026, Iran went through a series of government-directed Internet shutdowns, including one that lasted nearly three months before[traffic partially returned in May](https://blog.cloudflare.com/iran-internet-partially-restored-may-2026/) . Reporting a story like that means pulling several views and refreshing them as the situation develops. To investigate it, you can just ask Radar Researcher:


This time the question is open-ended, so Radar Researcher runs a short investigation. It looks up the outage events Cloudflare Radar recorded for Iran and gathers the matching traffic data, then explains the timeline in plain language: Iran's HTTP traffic index collapsed from around 0.58 on January 7 to effectively zero by January 9, began a partial recovery around January 17, and returned close to pre-shutdown levels by January 27. It presents this as an HTTP traffic index chart with the outage window annotated directly on the line, plus a table of the recorded outages that also flags a second shutdown starting February 28.


From there it suggests where to take the investigation next, such as how the traffic compared with neighboring countries.


## How we built it


Radar Researcher is built entirely on Cloudflare's own developer platform. It's a showcase of the same tools any developer can use to build an AI agent.


At its core is a Cloudflare Worker running the[Cloudflare Agents SDK](https://agents.cloudflare.com/) . Each conversation is a stateful[Durable Object](https://www.cloudflare.com/products/durable-objects/) with its own SQLite database, so your chat history, titles, and streaming responses persist. They even survive you leaving the page mid-answer, because generation continues server-side and resumes when you reconnect.
The "brain" is[Workers AI](https://www.cloudflare.com/products/workers-ai/) , running[open models](https://developers.cloudflare.com/workers-ai/models/) like Kimi K2.7. Rather than betting on a single model, we run an ordered fallback chain across three different model families. If one model is momentarily at capacity, the request transparently cascades to the next, so an incident at any single provider doesn’t take Researcher down. Every call routes through[AI Gateway](https://www.cloudflare.com/products/ai-gateway/) for logging, cost tracking, caching, and safety guardrails.


The most interesting part is how Researcher reaches Radar's data. Instead of hand-writing a tool for every one of Radar's hundreds of endpoints, we connect the agent to the unified[Cloudflare MCP server](https://github.com/cloudflare/mcp) using[Code Mode](https://blog.cloudflare.com/code-mode-mcp) . The model gets just three tools (search, execute, and docs) and writes code to query the Radar API directly. It searches the OpenAPI spec to find the right endpoint, then executes a small snippet that fetches live data. Because the full API spec lives on the MCP server, nothing about Radar's API is hard-coded into the prompt. As Radar adds datasets, Researcher can use them with no code changes.


This is visible to you, too: every answer includes an expandable trace of the model's reasoning and the exact tool calls it made to gather the data, so you can always see how it got there.


### From Markdown to real Radar charts


There was one problem to solve along the way. Language models answer in Markdown: text, tables, and bullet points. But we didn't want walls of numbers; we wanted the same rich, interactive charts you see everywhere else on Radar. Worse, when a model tries to write data directly into its answer, it tends to round, summarize, or truncate it, which is exactly what you don't want from a data tool.


Our solution keeps the data out of the model's prose entirely. When Radar Researcher fetches data, its code returns a small envelope that pairs the API path with the result. Then, instead of pasting numbers, the model emits a lightweight chart specification that simply references that same path:


```text
```radar-chart
{ "type": "speedFlower", "title": "Internet speed quality — Portugal", "dataFrom": "/radar/quality/speed/summary?location=PT" }
```
```


Radar's frontend matches the chart's dataFrom to the fetched result and renders it with the exact same visualization components used across the rest of the site. The chart is always faithful to the API, and Radar Researcher can draw on our whole visual vocabulary: time series and stacked areas, donuts, bar charts, maps, histograms, etc.


### A few small touches


Not everything needs the big model. Small, fast models handle the side tasks: one writes a short title for each new conversation, and another suggests the follow-up questions under each answer. Both run off to the side, so they never slow down your actual reply.


Radar Researcher also gets a little context about the here and now. We pass it the current date and time, plus the same high-level details about your connection that you'll find on[Radar's IP page](https://radar.cloudflare.com/ip) , such as your IP location and network. That way, when you ask something like "how's traffic here?", it can tailor the answer to where you are.


Everything reaches you through Radar's frontend (also a Worker) over a service binding, with per-IP rate limiting and shareable conversations stored in[R2](https://www.cloudflare.com/products/r2/) . Every layer, from compute to inference to gateway to storage to data, is Cloudflare.


## Powered by agents — and ready for them


Radar Researcher shows how we use an AI agent to help people explore our data. But there's another side to the agent story: what happens when the agent isn't ours, but a general-purpose assistant running in your browser, acting on your behalf?


Today, an agent that wants to use a website mostly has to scrape the page and guess at how it works, which is brittle, slow, and error-prone.[WebMCP](https://webmachinelearning.github.io/webmcp/) is an emerging web standard that fixes this: a page can register a small set of well-defined tools that any browser agent can discover and call directly, instead of reverse-engineering the DOM (Document Object Model).


We've added WebMCP support to Radar, using both flavors the standard offers:


- The **imperative API** lets us register tools in JavaScript that call straight into the same code that powers the UI, so an agent can drive Radar directly. Filter to a country, region, continent, or ASN; change the date range; search pages, sections, and entities (domains, ASNs, IPs, and more); jump to any section.
- The **declarative API** turns Radar's existing HTML forms into tools by annotating them with a couple of attributes, so an agent can scan a URL with the[URL Scanner](https://radar.cloudflare.com/scan) , look up a domain's report, or test a website's post-quantum TLS key exchange support.


Every one of these simply drives functionality already available in the user interface, and it's pure progressive enhancement: in a browser without WebMCP, the tools just aren't there and nothing changes.


It’s also a case of practicing what we preach. Radar’s own[URL Scanner](https://radar.cloudflare.com/scan) evaluates how[agent-ready](https://blog.cloudflare.com/agent-readiness/) a site is, and one of those checks looks for exactly this kind of WebMCP integration. By implementing it ourselves, Radar now passes its own[agent-readiness check](https://isitagentready.com/radar.cloudflare.com) , and, more importantly, becomes a site that both people and agents can navigate with ease.


## Try it out


Radar Researcher is available in beta today on[Cloudflare Radar](https://radar.cloudflare.com/) . Look for the Researcher button in the header on any page. It's built entirely on Cloudflare's developer platform, and it's just getting started: we'll keep expanding the datasets it understands, sharpening its analysis, and adding new ways to interact with data.


Like all Cloudflare products, Radar is built with the user in mind. We want to know what’s useful. Please let us know what you think on social media at[@CloudflareRadar](https://x.com/CloudflareRadar) (X),[noc.social/@cloudflareradar](https://noc.social/@cloudflareradar) (Mastodon), and[radar.cloudflare.com](http://radar.cloudflare.com/) (Bluesky), or by email atradar@cloudflare.com .


Tell us what works, what doesn’t, the insights you’re finding, what still feels beyond reach, and everything in between. Your feedback will help us improve and make Radar Researcher even better for our full launch. We can’t wait to see what you do with it!
