---
schema_version: "1.0.0"
document_id: "d186f2b935f97393816aaa2f1bfad69f3d65e40b9f5f0ea304e4e6e9fd529c9d"
company_key: "yc-firecrawl"
company: "Firecrawl"
source_id: "yc-firecrawl-news-import-e3b10de50c72"
canonical_url: "https://www.firecrawl.dev/blog/firecrawl-101"
published_at: "2026-05-18T00:00:00+00:00"
first_seen_at: "2026-07-21T20:06:32.273956+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:4ab47eebcc323a577f7958c69ced631dce947ea934fb2b908ea111a86230e08b"
---

# Firecrawl 101: Web Context APIs for AI Agents

## TL;DR


- **Firecrawl** is the context API to search, scrape, and interact with the web at scale, with six endpoints: Search, Scrape, Parse, Crawl, Map, and Interact
- **Search** is the front door: finds fresh sources from the live web and returns their full content in one call
- **Scrape** turns any URL into clean Markdown or structured JSON, handling JavaScript-rendered pages for you
- **Crawl and Map** give you depth across an entire site
- **Interact** keeps a live browser open so your agent can click, type, scroll, and reach data behind forms and dynamic interactions
- **Parse** converts PDFs and documents into clean, usable text


---


You built the agent. It reasons well. Then a user asks about a competitor's pricing page, and you realize your agent has no idea what's on it.


That's the problem Firecrawl solves.


AI models are trained on static snapshots of the internet. They don't know what changed last week. They can't read a page they haven't seen. They can't fill in a form, log in, or scroll through a paginated result set. For a deeper look at how[AI agents](https://www.firecrawl.dev/blog/ai-agents) are architected around this web context problem, the linked guide covers agent design, tool calling, and production patterns. Firecrawl is the context API to search, scrape, and interact with the web at scale. It fills that gap, helping AI agents find sources, extract content, and get clean text or structured data they can reason over and cite.


No stitching together a SERP API, a scraper, and a PDF parser. One API family, one workflow. This Firecrawl 101 covers each endpoint, when to reach for it, and how to compose them.


## What is Firecrawl?


Firecrawl gives AI agents access to the web. It's a set of APIs that cover every step of turning a live website into usable context: finding sources, extracting content, and cleaning it into Markdown or structured JSON. The hard parts (JavaScript rendering, dynamic content, PDFs) are handled for you.


The web wasn't built for machines. Most pages require a real browser to render, block automated requests, and return content in formats no LLM can reason over directly. Firecrawl sits between your agent and the internet and handles all of that. You get clean, structured data back. Your agent gets context it can actually use.


## The API: six endpoints, one workflow


Each endpoint works on its own. You can use just` /scrape` if that's all you need, or just` /search` , or just` /interact` . They also compose: most production workflows chain two or three together.


### Search: where every workflow starts


` /search` finds relevant pages from the live web and returns their full content. Not links. The actual pages.


A standard search API hands you a ranked list of URLs. You still have to fetch and parse each one. Firecrawl's` /search` returns the URL and the cleaned page content in the same call, ready for your agent to reason over.


```text
from   firecrawl   import   Firecrawl


app   =   Firecrawl  (api_key  =  "YOUR_API_KEY"  )


results   =   app  .  search  (
query  =  "Anthropic Claude pricing 2026"  ,
limit  =  5  ,
scrape_options  =  {  "formats"  : [  "markdown"  ]}
)


for   r   in   results  .  data  :
print  (r.url)
print  (r.markdown)    # Full page content, ready for your LLM
```


That's the feed your agent needs: fresh sources with the actual text, not just pointers to it. See the[Firecrawl search endpoint guide](https://www.firecrawl.dev/blog/mastering-firecrawl-search-endpoint) for the full parameter reference.


### Scrape: one URL, clean output


` /scrape` is Firecrawl's core[web scraping API](https://www.firecrawl.dev/blog/best-web-scraping-api) . It takes a URL and returns clean Markdown, structured JSON, a screenshot, or any combination.


```text
result   =   app  .  scrape  (
"https://example.com/pricing"  ,
formats  =  [  "markdown"  , {  "type"  :   "json"  ,   "prompt"  :   "Extract all plan names and monthly prices"  }]
)


print  (result.markdown)     # Full page as clean Markdown
print  (result.json)         # {"plans": [{"name": "Starter", "price": "$19/mo"}, ...]}
```


Firecrawl handles JavaScript rendering automatically. You send a URL; you get content. The page's tech stack is not your problem. For the full parameter reference, see the[Firecrawl scrape endpoint guide](https://www.firecrawl.dev/blog/mastering-firecrawl-scrape-endpoint) .


### Crawl and Map: cover an entire site


` /crawl` follows links across an entire site and returns the content of every page. Use it when you need coverage, not just one page. For a deep dive, see the[Firecrawl crawl endpoint guide](https://www.firecrawl.dev/blog/mastering-the-crawl-endpoint-in-firecrawl) .


```text
crawl_result   =   app  .  crawl  (
"https://docs.example.com"  ,
limit  =  100  ,
scrape_options  =  {  "formats"  : [  "markdown"  ]}
)


for   page   in   crawl_result  .  data  :
print  (page.url, page.markdown[:  200  ])
```


` /map` is the lightweight version: discover all URLs on a site without scraping them. Good for understanding a site's structure before committing to a full crawl.


```text
map_result   =   app  .  map  (  "https://docs.example.com"  )
print  (map_result.links)
# ['https://docs.example.com/api', 'https://docs.example.com/guides', ...]
```


### Interact: when the page fights back


` /interact` keeps a live browser session open so your agent can click, type, scroll, and extract data that only appears after an action: a "Load more" button, a search field that filters a table, a multi-step form.


Natural language works for most tasks:


```text
firecrawl   scrape   "https://example.com/products"
firecrawl   interact   "Click the Load more button and return all results"
```


For more control, use the SDK:


```text
# Scrape a page and get a session ID
result   =   app  .  scrape  (  "https://app.example.com"  , formats  =  [  "markdown"  ])
scrape_id   =   result  .  metadata  .  scrape_id


# Interact with the live session
app  .  interact  (scrape_id, prompt  =  "Click the Load more button"  )
response   =   app  .  interact  (scrape_id, prompt  =  "Return all visible results"  )
print  (response.output)


# Always stop the session when done
app  .  stop_interaction  (scrape_id)
```


Sessions stay alive for up to 10 minutes. You can chain as many interactions as you need within that window. For the full guide, see[mastering the Firecrawl interact endpoint](https://www.firecrawl.dev/blog/firecrawl-interact-endpoint) .


### Parse: turn documents into usable text


` /parse` converts PDFs, Word docs, and similar files into clean, usable text: the same Markdown or structured JSON you get from Scrape.


```text
result   =   app  .  scrape  (
"https://example.com/annual-report.pdf"  ,
formats  =  [  "markdown"  ]
)
print  (result.markdown)
```


PDF content is notoriously messy: multi-column layouts, footnotes mid-sentence, scanned pages that are technically images. Firecrawl parses the structure and returns text your LLM can work with.


## Three patterns most teams ship


**Research agent.** Search for sources, scrape the full content of the top results, pass the Markdown to your LLM for synthesis. For a complete worked example of this pattern using LangGraph and Firecrawl, see the[documentation agent tutorial](https://www.firecrawl.dev/blog/build-documentation-agent-langgraph-firecrawl) .


```text
results   =   app  .  search  (  "Stripe pricing changes 2026"  , limit  =  5  )


pages   =   [
app  .  scrape  (r.url, formats  =  [  "markdown"  ])
for   r   in   results  .  data
]


context   =   "\n\n---\n\n"  .  join  (p.markdown   for   p   in   pages)
# Pass context to your LLM
```


Good for competitive monitoring, market research, or any agent that needs to cite sources. When the context grows large,[RAG chunking strategies](https://www.firecrawl.dev/blog/best-chunking-strategies-rag) help you inject only the most relevant sections rather than entire pages.


**Enrichment pipeline.** You have a list of company URLs. Scrape each one, extract structured fields, write the results to your CRM. No custom code per site.


```text
companies   =   [  "https://acme.com"  ,   "https://globex.com"  ]


results   =   app  .  batch_scrape  (
companies,
formats  =  [{  "type"  :   "json"  ,   "prompt"  :   "Extract company name, founding year, and main product"  }]
)


for   r   in   results  .  data  :
print  (r.url, r.json)
```


**Live assistant.** A user asks a question. The agent searches for the answer, scrapes the top sources, and cites them in the response. Users get grounded answers backed by the current web, not a model's memory of it.


```text
query   =   "What's the current S3 pricing for us-east-1?"


results   =   app  .  search  (query, limit  =  3  , scrape_options  =  {  "formats"  : [  "markdown"  ]})
context   =   "\n\n"  .  join  (r.markdown   for   r   in   results.data)


# Feed context to your LLM with instructions to cite sources
```


These three are just the patterns we see most often. Honestly, every week we talk to Firecrawl users who are doing something we hadn't thought of — job boards feeding hiring agents, docs sites powering internal search, product pages keeping pricing models current. The web is big and the use cases keep coming.


## Start building in two minutes


Sign up at[firecrawl.dev](https://firecrawl.dev/) for 1,000 free credits.


### Python SDK


```text
pip   install   firecrawl-py
```


```text
from   firecrawl   import   Firecrawl


app   =   Firecrawl  (api_key  =  "YOUR_API_KEY"  )
result   =   app  .  scrape  (  "https://firecrawl.dev"  , formats  =  [  "markdown"  ])
print  (result.markdown)
```


### CLI


Every endpoint is also available from the terminal. Install the CLI with npm:


```text
npm   install   -g   firecrawl-cli
firecrawl   login
```


Then use any endpoint directly:


```text
firecrawl   scrape   https://firecrawl.dev
firecrawl   search   "web scraping api for AI agents"
firecrawl   map   https://firecrawl.dev
```


If you're using Claude Code, Cursor, Codex, or another AI coding agent, run this once and the CLI installs itself as a skill your agent can call:


```text
npx   -y   firecrawl-cli@latest   init   --all   --browser
```


### MCP


Firecrawl also runs as an[MCP server](https://docs.firecrawl.dev/mcp-server) , so any MCP-compatible host (Claude, Cursor, Windsurf) can call Search, Scrape, Crawl, and Map directly. For a broader guide to the[best MCP servers for developers](https://www.firecrawl.dev/blog/best-mcp-servers-for-developers) — covering design tools, browser automation, and code execution alongside web scraping — that post shows how Firecrawl fits into a full MCP toolchain. No code required. Add the remote hosted URL to your MCP config:


```text
https://mcp.firecrawl.dev/{FIRECRAWL_API_KEY}/v2/mcp


```


Your AI assistant can start pulling live web data from any conversation.


[Get started with Firecrawl](https://firecrawl.dev/) ·[Read the docs](https://docs.firecrawl.dev/)
