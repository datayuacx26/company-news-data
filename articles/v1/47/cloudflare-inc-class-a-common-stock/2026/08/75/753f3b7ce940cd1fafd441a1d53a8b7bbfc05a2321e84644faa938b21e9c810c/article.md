---
schema_version: "1.0.0"
document_id: "753f3b7ce940cd1fafd441a1d53a8b7bbfc05a2321e84644faa938b21e9c810c"
company_key: "cloudflare-inc-class-a-common-stock"
company: "Cloudflare Inc."
source_id: "cloudflare-inc-class-a-common-stock-rss-72eb55cabac1"
canonical_url: "https://blog.cloudflare.com/ai-search-easier/"
published_at: "2026-08-06T13:00:00+00:00"
first_seen_at: "2026-08-06T13:03:06.847303+00:00"
fetched_at: "2026-08-06T13:03:08.318505+00:00"
content_hash: "sha256:b8626da26a64248ecb1a4142722ea3ebd05da792d2d033e9e16895044dec736b"
---

# Cloudflare AI Search: give your agents a search engine for your data

Today, we’re excited to announce a few developer experience improvements to[Cloudflare AI Search](https://developers.cloudflare.com/ai-search/) to make it easy to manage a search solution out of the box. Previously, you had to stitch together components of the Cloudflare primitives (Workers AI, AI Gateway, Vectorize, R2, Browser Run) but now, AI Search can do this automatically — and better. Our goal is to give your agents their own search engine, where they can easily find data to provide better answers for themselves and their humans.


We’re also sharing an early preview of pricing for customers of AI Search so you can learn how this scales. We modeled pricing in a way that makes it predictable and scalable: embedding and reranking are free when you use the default models, so no need to worry about predicting token count.


In AI Search, users can now:


- **Index a collection of data for your agent:** Make structured and unstructured data easily accessible for your agent to build with, from individual files to websites you own. (Today, it must be a zone on your Cloudflare account, but with more ways to verify ownership coming soon.)
- **Skip the sitemap for your websites:** Previously, AI Search required that websites have a sitemap to use the website integration. Now you can select the “Discover” parsing option to add a website without a sitemap as a source.
- **Get a single public endpoint for searching across a namespace:** When you enable public URLs on your namespace, you can get a` /search` and` /mcp` endpoint that can search through multiple instances or websites at once without authentication, so you can share easily with your customers.
- **Put your own custom domain over public endpoints:** You can now add your own domains over your public URLs, so you can brand your` /search` and` /mcp` endpoints (e.g.,` search.example.com/mcp` ). You can also add[Cloudflare Access](https://www.cloudflare.com/products/access/) to create private search instances.
- **Add semantic search to your sites built on EmDash with AI Search plugin:** If your site runs on[EmDash](https://github.com/emdash-cms/emdash) , our open-source CMS, the[AI Search plugin](https://docs.emdashcms.com/deployment/cloudflare/#cloudflare-ai-search) adds semantic search over your content.
- **Preview the new pricing model for AI Search:** We want pricing to be predictable and to scale with you, so we built in the cost of embedding and reranking: they’re free when you use select models from the Workers AI catalog.


Finally, we will also share examples of how AI Search is used across our own platform including[Cloudflare.com](http://cloudflare.com/) , our Developer Docs, with EmDash, in Cloudflare Dev Stack MCP — and even the blog post you’re reading right now (try cmd+K).


## AI Search in action: powering the new Cloudflare Dev Stack MCP


One of the ways we use AI Search is in our new **Cloudflare Dev Stack MCP** , which you can try today in our[AI Playground](https://playground.ai.cloudflare.com/) . It gives coding agents current, cited docs from across the Cloudflare developer ecosystem, so they build on the latest features and fixes instead of stale training data.


Here's how we built it using the features available today in AI Search:


### 1. Index each surface


We created one AI Search instance per Cloudflare-owned surface: Docs, Blog, API Docs, Community, Astro, Vite, Vitest, Hono, Replicate, OpenNext. (Each of these is Cloudflare-owned.)


They span different domains, but, because Cloudflare owns the website data, AI Search is able to treat them as a single set and ingest them all the same way. Point AI Search at a site, or set of sites, and it handles crawling, ingestion, embedding, and retrieval. Creating an instance is a single command, and for a site without a sitemap you add --parse-type discover to find pages by following links (powered by /crawl from Browser Run):


```text
npx   wrangler   ai-search   instance   create   cloudflare-community   \
--namespace   dev-stack   \
--source   https://community.cloudflare.com   \
--type   web-crawler   \
--parse-type   discover
```


### 2. Combine the instances into one search


Now the interesting part: answering a single query across all 10 instances. There are two ways to do it.


**Option A: in a Worker (what we did for Cloudflare Stack MCP)**


We bound the namespace to a Worker to create a[remote MCP server](https://developers.cloudflare.com/agents/model-context-protocol/guides/remote-mcp-server/) and made one multi-instance call across all 10 instances. We took this path because we're adding the stack search into[Cloudflare's MCP server](https://github.com/cloudflare/mcp-server-cloudflare) , so it ships as a tool alongside the Cloudflare tools agents already connect to.


The binding, in` wrangler.jsonc:`


```text
{
"ai_search_namespaces"  : [
{   "binding"  :   "AI_SEARCH"  ,   "namespace"  :   "cloudflare-stack"   }
]
}
```


Then a single tool makes one call that fans out across the instances you name:


```text
// One tool, one call that searches every surface in the namespace at once.
context.  registerTool  (
'search_dev_stack'  ,
{
description:   'Search current docs across the Cloudflare stack.'  ,
inputSchema: z.  object  ({ query: z.  string  () }),
},
async   ({   query   })   =>   {
const   res   =   await   context.env.  AI_SEARCH  .  search  ({
query,
ai_search_options: {
instance_ids: [  'developers-cloudflare-com'  ,   'astro'  ,   /* ...every surface */  ],
retrieval: { max_num_results:   10   },
reranking: { enabled:   true   },
},
})
// res.chunks come back cited and tagged with the instance they came from.
return   { content: [{ type:   'text'  , text:   format  (res.chunks) }] }
}
)
```


**Option B: flip on public endpoints (no code)**


If you'd rather not write a Worker at all, enable public URLs on the namespace. You immediately get /search and /mcp endpoints that query every instance, with no auth and nothing to deploy.


Reach for the Worker when you're folding search into an existing app or MCP server, as we are. Or reach for the public endpoint when you just want a shareable search endpoint in one click.


### 3. Brand it and lock it down


Public endpoints come with a default public URL, but you can put your own **custom domain** over them to brand the endpoint (e.g.,` search.example.com/mcp` ).


If the search should be private, add **Cloudflare Access** in front of the domain. The endpoint now requires a login, so only authorized people (or agents) can query it.


## Try it yourself: use the Dev Stack MCP


With the Cloudflare Dev Stack MCP Server, you can ask about any tool, or describe an app you want to build, and you'll get back current, cited answers on how best to build it on the Cloudflare stack.


The[AI Playground](https://playground.ai.cloudflare.com/) is worth checking out, but the real magic is wiring the MCP into your coding agent, so the stack's current docs are one tool call away. That replaces the usual fallback (web search then fetching full pages), which is slow, token-heavy, and often lands on the wrong or stale source. To use with your agent of choice, drop the Dev Stack MCP URL into your MCP configuration. For example:


```text
{
"mcpServers"  : {
"dev-stack"  : {   "url"  :   "https://stack.mcp.cloudflare.com/mcp"   }
}
}
```


## Powering search on our Blog, Developer Docs, and[Cloudflare.com](http://cloudflare.com/)


We build with AI Search the same way our customers would: Cloudflare Blog's search already runs on it, and today Developer Docs and[Cloudflare.com](http://cloudflare.com/) join it. All of it uses hybrid search, semantic and keyword together in one query, so it handles both open-ended "what does this do" questions and exact lookups of names or keywords. We recently rebuilt the Blog on EmDash, our new open-source CMS, and our new


[EmDash AI Search integration](https://docs.emdashcms.com/deployment/cloudflare/#cloudflare-ai-search) is what powers that search now. You can also add it to your own EmDash site and get the same search over your content out of the box.


## AI Search respects all bot policies


AI Search is powered by Browser Run` /crawl` in the background, but goes a step further to identify itself with its own bot identity:` Cloudflare-AI-Search` . Just like Browser Run, it follows robots.txt, identifies itself with an immutable, public user agent, and will respect whatever bot controls a site has in place.


## Preview pricing: pricing you can predict


AI Search is currently free while in beta, and billing is not yet enabled; we'll email you with plenty of notice before it starts. As we move toward general availability, here's a preview of pricing across ingestion, storage, and queries, plus embedding and reranking ( *preview prices are subject to change before billing begins)* :


**Preview usage price**


**Free monthly allotment (all Workers plans)**


**Ingestion**


Base Ingestion


$0.75 / 1M tokens


5M tokens †


Image processing (add-on)


+$0.50 / 1M tokens


5M tokens †


**Storage**


Stored data


$2.00 / GB-month


10 GB


**Query**


Semantic (hybrid and vector search)


$0.75 / 1k queries


2,000 queries ‡


Full-text


$0.10 / 1k queries


2,000 queries ‡


**Embedding and Reranking**


Ingestion and query


Free with select Workers AI models; third-party billed separately


N/A


*† A single pool of 5M ingestion tokens per month, covering any file type currently supported (e.g., text, images). ‡ A single pool of 2,000 queries per month, shared across both query types.*


Our goal is to provide pricing you can predict, starting with the models your search leans on. Embedding turns your text into the vectors that search matches on, and reranking reorders results so the most relevant come first. Both run free with AI Search defaults or when using select models from the Workers AI catalog, so the models behind indexing and every search are not a cost you have to worry about. Answer generation and query rewriting are optional steps that run on a model you choose, billed as Workers AI usage, or you can use AI Gateway credits with any model/provider.


### Example bill with preview pricing


Here's a sample monthly bill on the Workers Paid plan for creating a new AI Search instance for a 20,000-document data source (about 20M tokens of text) plus 1,000 images (assume about 1,000 tokens each), with 30,000 semantic queries a month using the default AI Search embedding and reranking model. Ingestion is chunked with roughly 10% overlap, which shows up as the × 1.1 below:


**Line item**


**Usage**


**Price**


**This month**


Base ingestion


(20M tokens of text + 1M tokens of images) × 1.1 - 5M free = 18.1M tokens


$0.75 / 1M tokens


$13.58


Image add-on


1M tokens of images × 1.1 = 1.1M tokens


$0.50 / 1M tokens


$0.55


Storage


~1.2 GB (within 10 GB free)


$2 / GB-mo


$0


Queries (semantic)


30,000 - 2,000 free = 28K


$0.75 / 1k


$21.00


Embedding (Workers AI)


Usage included with selected Workers AI model


$0


$0


Reranking (Workers AI)


Usage included with selected Workers AI model


$0


$0


**Total**


**~$35**


Images count toward base ingestion and also incur the image add-on cost. Storage assumes about 10 KB per document and 1 MB per image. Indexing is largely a one-time cost, so later months are mostly queries, closer to $21.


## Get started today


AI Search is available to enable and use today. Point it at your site, turn on hybrid search for both semantic and keyword matching, and you have a search engine for your own data, ready for your agents. Spin one up with one command:


```text
npx   wrangler   ai-search   create   my-search   \
--namespace   my-namespace   \
--source   https://my-website.com   \
--type   web-crawler   \
--hybrid-search
```


From there, query it, wire it into an agent over` /mcp` , or put a custom domain on a public` /search` endpoint to share it with your users. Check out the[AI Search docs](https://developers.cloudflare.com/ai-search/) for more information.
