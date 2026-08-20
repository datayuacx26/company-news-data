---
schema_version: "1.0.0"
document_id: "87210b420db7345a3f43a8fc6694d2f4490fd4650c9d95ea6afd458b110b08a6"
company_key: "yc-taloflow"
company: "Taloflow"
source_id: "yc-taloflow-rss-cfae8c512c9e"
canonical_url: "https://www.taloflow.ai/blog/comparing-cloudflare-workers-fastly-compute-edge-akamai-edgeworkers"
published_at: "2021-12-20T13:31:00+00:00"
first_seen_at: "2026-07-26T01:23:39.915767+00:00"
fetched_at: "2026-07-28T22:26:34.525443+00:00"
content_hash: "sha256:c26d3f9f3773247d9b9d9586c4380745193d658bfd54708f5f339722c3d5b90a"
---

# Comparing Cloudflare Workers, Fastly Compute@Edge, Akamai EdgeWorkers

## My Honest Attempt At Building The Same App on Cloudflare Workers, Fastly Compute@Edge and Akamai EdgeWorkers


Building serverless applications on the “edge” is becoming a reality. The promise of fast execution and seamless deployment is near, but is it here?


**The goal:** Use an edge platform to build a proof of concept clone of[linkz.ai](https://linkz.ai/) , a tooltip generator for external links (among other things).


Demo of linkz.ai


## Project requirements


I want to create a self-contained application, all serverless, without any external tools like a managed Postgres or lambda.


**I want to write my code and deploy it to the edge. No-fuss. What are my options for deploying wicked-fast applications to the edge?**


To build my linkz.ai clone, here’s how I would structure my application—starting with a list of URLs sent from the website and ending with the data needed to make my preview.


## Disqualifying Vendors


There are a few key points here that disqualify a lot of serverless providers:


- I need a built-in persistent data store
- I need to be able to fetch and parse an arbitrary URL


Right off the bat, a lack of a native storage layer disqualifies the following:


- [Vercel’s Edge Functions](https://vercel.com/features/edge-functions)
- Stackpath’s Serverless Scripting
- Azion’s Edge Functions
- [AWS Lambda@Edge](https://aws.amazon.com/lambda/edge/)


To get around the limitations, I could use an external database like[Upstash](https://upstash.com/) , but that is beyond the scope of this article.


‍


## Building with Fastly Compute@Edge


Fastly offers four ways to build their Wasm bundles:


- [Rust](https://developer.fastly.com/learning/compute/rust/)
- [AssemblyScript](https://developer.fastly.com/learning/compute/assemblyscript/) (a subset of typescript that compiles to Wasm)
- [Javascript](https://developer.fastly.com/learning/compute/) (which they compile to a *hefty* Wasm bundle)
- [A custom/community SDK](https://developer.fastly.com/learning/compute/custom/) (for compiling something like Zig to Wasm)


Rust seems to get the most love from the Fastly team, with items like fast reload[launching with support only for Rust](https://github.com/fastly/cli/issues/476) (with the other platforms coming later).


While I prefer Typescript, AssemblyScript was a bit restrictive and limited compatibility with third party libraries that could be useful. I opted for javascript.


However, after writing some code, I found that Fastly doesn’t allow for fetch requests to URLs not pre-defined in the application’s config.


While Compute@Edge feels like a great add-on to Fastly, it doesn’t quite fit the requirements for this *full-stack* application.


‍


## Trying Akamai


The developer experience for using Akamai’s edge workers is dramatically different than Fastly or Cloudflare.


To start, to even enable them on my account, I had to head to a Slack channel to ask for access.


Trying to set up the “[Hello World](https://techdocs.akamai.com/edgeworkers/docs/create-an-edgeworker-id-1) ” example they had, I had to jump through the hoops of enabling their “advanced TLS” and creating a certificate. This process is much more involved than with Cloudflare or Fastly.


The developer experience was lacking:


- To use their workers, I needed to compress and upload the folder as a .tar.gz file. Although they do[have IDE extensions that can make this easier](https://techdocs.akamai.com/edgeworkers/docs/ide-extensions)
- Their[local development workflow relies on Jest or Mocha](https://github.com/akamai/edgeworkers-unittest) , rather than creating a test server


For someone just looking to use a workers platform, I would not recommend Akamai as a solution.


*Note: My experience may not be typical. Maybe something went wrong during onboarding. However, the point remains that Akamai’s developer experience is currently not up to par with that of Fastly or Cloudflare, and I prefer not to develop an application on their platform.*


‍


## Building on Cloudflare Workers


While the developer experience on Cloudflare Workers isn’t perfect, it does get the job done without much fuss.


Setting up workers, creating a KV store for dev and prod environments, and deploying all happen without any challenges.


Here’s a sample of the final product.


[Youtube link](https://www.youtube.com/watch?v=b2F-DItXtZs) (opens an embed window when clicked)


[Here’s a link to the repo](https://github.com/austinpena-taloflow/cooltips) . I was able to build the back end with under 200 lines of code!


### Fetching and parsing URLs


Cloudflare knows that a huge use-case for workers is working with HTML files. Their built-in[HTML rewriter](https://developers.cloudflare.com/workers/runtime-apis/html-rewriter) made it *easy* to read and pull out the information I needed from the links needing a tooltip.


### Storing persistent data with Edge KV


[Edge KV](https://developers.cloudflare.com/workers/runtime-apis/kv) is a globally distributed and eventually consistent database built for “write-once” or “write-rarely” data. For our purposes, this is great.


They also have a handy feature for[Cache TTL](https://developers.cloudflare.com/workers/runtime-apis/kv#cache-ttl) , alleviating my worry that I’ll store old data that never gets accessed.


## The verdict


The only real “workers” option for building a tool like linkz.ai is Cloudflare Workers.


Other tools, like Fastly’s Compute@Edge, Akamai’s Edge Workers, and more, are positioned to be excellent *in tandem* with an existing application.


As a self-contained application, Cloudflare Workers is the only contender.
