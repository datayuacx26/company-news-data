---
schema_version: "1.0.0"
document_id: "8e45ae1ec2cb733867484b637a4d728f181539184771db8a8855a315732c7482"
company_key: "yc-firecrawl"
company: "Firecrawl"
source_id: "yc-firecrawl-news-import-e3b10de50c72"
canonical_url: "https://www.firecrawl.dev/blog/claude-code-web-research-tokens"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-07-29T03:51:18.808011+00:00"
fetched_at: "2026-07-29T03:51:19.789559+00:00"
content_hash: "sha256:b9661148267bde5e97a1f45697f52d3dc7de26dfdd5e08fb82317f413749fa31"
---

# Claude Code Uses Too Many Tokens on Web Search - Here's the Fix

Every time Claude Code scrapes a page or runs a search mid-session, whatever comes back gets added to context, and stays there for every message after it. A single bloated search doesn't cost once, it sits in context and gets reprocessed (if uncached) on every turn for the rest of the session.


But this can be fixed using four **Firecrawl** tips that cut down what actually lands in context in the first place:


- **Asking a page a direct question** instead of scraping it whole
- **Shaping the response into JSON** instead of a full page
- **Batching multiple URLs into one call** instead of looping
- **Applying the same question format** to search results


Let's go through these tips in detail.


## Getting an API key


If you want to follow along, **scrape** and **search** , the two endpoints used throughout this post, both work with our no API key required option, so that's enough to get started.


Signing up at[firecrawl.dev](https://www.firecrawl.dev/) gets you a free API key with higher rate limits and starting credits, and the code below reads it from an environment variable if one is set.


## First usage: the Node SDK


This walkthrough uses Firecrawl's[Node SDK](https://docs.firecrawl.dev/sdks/node) . A[Python SDK](https://docs.firecrawl.dev/sdks/python) ,[CLI](https://docs.firecrawl.dev/sdks/cli) , and[MCP server](https://docs.firecrawl.dev/mcp-server) all expose the same options if you're working in a different stack.


```text
npm   install   firecrawl
```


```text
import   { Firecrawl }   from   "firecrawl"  ;


const   firecrawl   =   new   Firecrawl  ({
apiKey  :   process  .  env  .  FIRECRAWL_API_KEY  ,   // optional, keyless works too
});
```


That client is reused across every example below, just swapping the method and` formats` option.


## Tip 1: don't scrape, just ask


Imagine if we wanted to pull Apple's 3-year and 5-year stock return, plus the same numbers for Sony, Amazon, and GoPro, from Yahoo Finance. Doing this with Claude Code's **default web fetch tool** then using` /context` showed the whole exchange costing **15.8k tokens** .


The exact same prompt, run through a **Firecrawl scrape skill** instead, cost **14.4k tokens** , over 1,000 tokens less. Why is the difference so large?


The big token savings come from Firecrawl's **question type** returning only the answer instead of the whole page. The difference is easier to see in code


Here's the same example using the[Claude Agent SDK](https://docs.claude.com/en/api/agent-sdk/overview) , restricting the agent to the Web fetch tool.


```text
// claude.ts


import   { query }   from   "@anthropic-ai/claude-agent-sdk"  ;


const   url   =   "https://uk.finance.yahoo.com/quote/AAPL/"  ;


const   result   =   query  ({
prompt  :   `Use the WebFetch tool on   ${  url  }   to get the current stock price.`  ,
options  :   {
allowedTools  :   [  "WebFetch"  ]  ,
settingSources  :   []  ,
}  ,
});


for   await   (  const   message   of   result) {
if   (  message  .type   ===   "user"   &&   message  .tool_use_result) {
const   output   =   message  .tool_use_result;
console  .log  (  "bytes:"  ,   output  .bytes  ,   "result:"  ,   output  .  result  ?.  length  );
}
}
```


That summarized answer came back at **295 characters** . This is because Claude will scrape the page, then use a smaller model to summarize the result before it reaches the main model.


Now the Firecrawl version using a custom MCP tool that calls scrape with the **question type** instead of a plain scrape:


```text
// claude-firecrawl.ts


import   { Firecrawl }   from   "firecrawl"  ;


const   firecrawl   =   new   Firecrawl  ({ apiKey  :   process  .  env  .  FIRECRAWL_API_KEY   });


const   url   =   "https://uk.finance.yahoo.com/quote/AAPL/"  ;


const   answer   =   await   firecrawl  .scrape  (url  ,   {
formats  :   [{ type  :   "question"  ,   question  :   "what is the current stock price of AAPL"   }]  ,
location  :   { country  :   "us"   }  ,
});


console  .log  (  "question format:"  ,   answer  .  answer  ?.  length  ,   "chars ->"  ,   answer  .answer);
```


` formats: \[{ type: "question", question }\]` tells Firecrawl to scrape the page and extract just the answer, instead of returning the whole scraped markdown.


Firecrawl still does the real scrape behind the scenes, but the agent only receives the specific fact it asked for.


That call returned **6 characters** : the price itself, nothing else.


## Tip 2: shape the data, not the page


Let's go through another example. The[Replit pricing page](https://replit.com/pricing) has a lot on it: three tiers, feature lists, an FAQ, a footer. Imagine we just need a small amount of information, like **name** , **price** , **collaborator limit** , and **parallel agent limit** , none of the surrounding page matters.


Run through Claude Code's default` WebFetch` tool with a prompt asking for exactly those fields, the result came back at **942 characters** , since the built-in summarizer still writes prose around the extracted values.


The same extraction through Firecrawl's **JSON format** returned **338 characters** , using a schema instead of a written summary:


Let's see how to do this in code.


```text
// index.ts


import   { Firecrawl }   from   "firecrawl"  ;
import   { z }   from   "zod"  ;


const   schema   =   z  .object  ({
plans  :   z  .array  (
z  .object  ({
name  :   z  .string  ()  ,
price  :   z  .string  ()  ,
collaborators  :   z  .string  ()  .describe  (  "max collaborators allowed, or 'unlimited'/'n/a'"  )  ,
agents  :   z  .string  ()  .describe  (  "how many agents can work in parallel, or 'n/a'"  )  ,
})  ,
)  ,
});


const   extracted   =   await   firecrawl  .scrape  (  "https://replit.com/pricing"  ,   {
formats  :   [
{
type  :   "json"  ,
prompt  :   "Extract every pricing plan on the page with its name, price, collaborator limit, and parallel agent limit."  ,
schema  :   z  .toJSONSchema  (schema)  ,
}  ,
]  ,
onlyMainContent  :   true  ,
});


console  .log  (  "json format:"  ,   JSON  .stringify  (  extracted  .json).  length  ,   "chars ->"  ,   extracted  .json);
```


This example uses` z.toJSONSchema(schema)` to convert a Zod object into plain[JSON Schema](https://json-schema.org/) , but Firecrawl's` json` format accepts a JSON Schema object directly too, so you can skip Zod entirely.


` onlyMainContent: true` strips nav and footer noise before the extraction step even runs. The result comes back as structured` plans` data, each with` name` ,` price` ,` collaborators` , and` agents` already parsed, no re-parsing a summary paragraph on the agent's end.


## Tip 3: batch it, don't loop it


The first two tips both worked on a single URL however, research tasks rarely stop at one page. If we asked Claude to get the current stock price for five different companies, its default tools **can't batch a fetch call** , so it makes one` WebFetch` call per URL: five separate round trips, each carrying its own tool-call framing and instructions on top of the actual page content, landing around **1,100 characters** total.


Firecrawl's` batchScrape` method sends the same five URLs as **one job** :


```text
// index.ts


import   { Firecrawl }   from   "firecrawl"  ;


const   urls   =   [
"https://uk.finance.yahoo.com/quote/AAPL/"  ,
"https://uk.finance.yahoo.com/quote/MSFT/"  ,
"https://uk.finance.yahoo.com/quote/GOOG/"  ,
"https://uk.finance.yahoo.com/quote/AMZN/"  ,
"https://uk.finance.yahoo.com/quote/TSLA/"  ,
];


const   batch   =   await   firecrawl  .batchScrape  (urls  ,   {
options  :   {
formats  :   [{ type  :   "question"  ,   question  :   "what is the current stock price"   }]  ,
location  :   { country  :   "us"   }  ,
}  ,
});
```


The same **question format** from tip 1 applies to every URL in the batch, so each result comes back as a short answer rather than a full page. One tool call, five answers, a fraction of the characters a looped equivalent produces:


The saving here isn't just the shorter answers. It's that the per-call overhead, the framing and instructions that travel with every individual tool call, only gets paid once instead of five times.


## Tip 4: ask every result, not just one page


The first three tips all assume you already have a URL, but as you can imagine research often starts with just a question.


Firecrawl's[search](https://docs.firecrawl.dev/sdks/node) endpoint handles that case, and it's where the **question format** pays off the most, because by default a search scrapes full markdown for every result it returns.


Searching for the latest stable version of Node.js with` scrapeOptions: { formats: \["markdown"\] }` set on the search call, five results come back as five full scraped pages. Passing the **question format** into that same` scrapeOptions` field instead changes what each result contains:


```text
// index.ts


import   { Firecrawl }   from   "firecrawl"  ;


const   query   =   "latest stable version of node.js"  ;


const   withQuestion   =   await   firecrawl  .search  (query  ,   {
limit  :   5  ,
scrapeOptions  :   {
formats  :   [{ type  :   "question"  ,   question  :   "what is the latest stable LTS version of node.js"   }]  ,
}  ,
});


const   answers   =   withQuestion  .  web  ?.map  ((r)   =>   r  .answer   ??   ""  )   ??   [];
console  .log  (  "search + question:"  ,   withQuestion  .  web  ?.  length  ,   "results,"  ,   answers);
```


` scrapeOptions` is the same options object` scrape()` takes, just applied per result inside a search. With the question format set, all five results return **111 characters total** , direct answers like "Node.js 24.11.0 (LTS)" instead of five full pages:


This is tip 1's trick again, just applied across an entire search instead of one page you already had the URL for. It matters most in exactly that situation: research and discovery, when you don't know which page has the answer yet and don't want to scrape blind.


Four different situations, one underlying move: decide what shape of answer you actually need before the page enters context, whether that's a single fact, a JSON object, a batch of pages, or a set of search results, so Claude reads only what it asked for.


*Check out the[Firecrawl docs](https://docs.firecrawl.dev/introduction) to go deeper.*
