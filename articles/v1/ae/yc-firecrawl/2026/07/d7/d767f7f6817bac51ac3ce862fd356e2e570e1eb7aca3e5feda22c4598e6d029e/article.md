---
schema_version: "1.0.0"
document_id: "d767f7f6817bac51ac3ce862fd356e2e570e1eb7aca3e5feda22c4598e6d029e"
company_key: "yc-firecrawl"
company: "Firecrawl"
source_id: "yc-firecrawl-news-import-e3b10de50c72"
canonical_url: "https://www.firecrawl.dev/blog/scrape-every-page-firecrawl-pagination"
published_at: "2026-07-15T00:00:00+00:00"
first_seen_at: "2026-07-21T20:06:32.273956+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:e40aba8e916ca0fca0b83de2605a1fe2f9859d49f3140e9815ab766b77e7117e"
---

# How to Scrape a Paginated Website with Firecrawl

Ask an agent to scrape a **paginated** website and you'll usually get page one, thirty results, and a confident "done." The site might have forty pages behind it. The agent has no reliable way to know that.


**Firecrawl** solves this differently: it hands back **structured JSON** instead of raw HTML, so you don't need site-specific parsing logic at all. And because every page comes back in the same predictable shape, there's a universal stop condition: the last page is just the one that returns zero results. Let's go through how to implement this.


## Getting an API key


Scraping with Firecrawl works **keyless** : the` scrape` endpoint can be called without an API key from any official client (SDK, CLI, or MCP server), rate-limited per IP. Sign up on the[Firecrawl dashboard](https://www.firecrawl.dev/) to get a key if you want a higher rate limit and to unlock the other endpoints (crawl, extract, map), which do require one.


## First scrape


This walkthrough uses the **Node SDK** , but the same pagination pattern applies with the[Python SDK](https://docs.firecrawl.dev/sdks/python) , the[CLI](https://docs.firecrawl.dev/sdks/cli) , or through the[MCP server](https://docs.firecrawl.dev/mcp-server) if you're driving it from an agent interactively.


Install it, then set up a client:


```text
// npm install firecrawl


import   { Firecrawl }   from   "firecrawl"  ;


const   firecrawl   =   new   Firecrawl  ({
apiKey  :   process  .  env  .  FIRECRAWL_API_KEY  ,   // optional
});
```


The first call just proves the connection works, scraping[Hacker News](https://news.ycombinator.com/news) as plain markdown:


```text
const   result   =   await   firecrawl  .scrape  (  "https://news.ycombinator.com/news"  ,   {
formats  :   [  "markdown"  ]  ,
});


console  .log  (  result  .markdown);
```


` formats: \["markdown"\]` is spelled out here, but **markdown is actually the default** output if you omit` formats` entirely. Run this and you get every story on the page back as one wall of text, titles, points, and comments all mixed into the markdown. Useful for a quick check, not for anything you'd want to parse programmatically.


## Structured data instead of a wall of markdown


To get the individual fields out, swap the format to` json` and describe what you want, either with a prompt alone or backed by a schema for a guaranteed shape:


```text
import   { z }   from   "zod"  ;


const   schema   =   z  .object  ({
stories  :   z  .array  (
z  .object  ({
title  :   z  .string  ()  ,
url  :   z  .string  ()  ,
points  :   z  .number  ()  ,
comments  :   z  .number  ()  ,
})
)  ,
});


formats  :   [
{ type  :   "json"  ,   prompt  :   "Extract every story on the page."  ,   schema  :   z  .toJSONSchema  (schema) }  ,
]  ,
});


const   stories   =   (  result  .json   as   z  .  infer  <  typeof   schema>).stories;
console  .log  (stories);
```


The **Zod** schema here is a Node SDK convenience, converted to **JSON Schema** before it's sent, since that's what the API actually accepts on the wire. Run this and` result.json` comes back matching the schema exactly: an array of` stories` , each with a` title` ,` url` ,` points` , and` comments` . No regex, no digging through markdown for the right substring.


This still only covers page one, though. Hacker News caps each page at thirty stories, so this call always returns the same thirty no matter how many times you run it.


## Walking every page until it stops


The trick is that Hacker News pages are just a URL with an incrementing` ?p=` parameter, and once you have JSON back on every page, "is there more" becomes a one-line check: did` stories` come back empty?


```text
type   ScrapeResult   =   z  .  infer  <  typeof   schema>;
type   Story   =   ScrapeResult  [  "stories"  ][  number  ]   &   { page  :   number   };


const   stories  :   Story  []   =   [];
let   page   =   1  ;


while   (  true  ) {
const   result   =   await   firecrawl  .scrape  (
`https://news.ycombinator.com/news?p=  ${  page  }  `  ,
{
formats  :   [
]  ,
}
);


const   rows   =   (  result  .json   as   ScrapeResult  ).stories;
if   (  rows  .  length   ===   0  ) {
break  ;
}


stories  .push  (  ...  rows  .map  ((row)   =>   ({   ...  row  ,   page })));
page  ++  ;
}


await   Bun  .write  (  "stories.json"  ,   JSON  .stringify  (stories  ,   null  ,   2  ));
```


Each row gets stamped with the` page` it came from before being pushed into the` stories` array, which is handy later if you ever need to trace a story back to where it was scraped. The loop keeps incrementing` page` and re-scraping until a page's` stories` array comes back empty, at which point it breaks and writes everything collected to` stories.json` .


Running this against Hacker News walks all **61 pages** and lands just under **1,040 stories** in one file, no page-count guessing, no HTML markup to keep in sync with the site.


Not every paginated site uses a` ?p=` style URL. Plenty of older forums and directories use **offset pagination** instead, a` start` parameter that increases by the page size (` ?start=0` ,` ?start=25` ,` ?start=50` , ...). The loop is identical, just swap the increment: instead of` page++` , you'd do` start += PAGE_SIZE` , and the same "empty results means stop" check still applies.


## Try it on your own site


Swap the Hacker News URL for whatever paginated site you need, adjust the schema to match its fields, and the same loop handles the rest.


Try Firecrawl free at[firecrawl.dev](https://www.firecrawl.dev/) . For more, see the[Node SDK](https://docs.firecrawl.dev/sdks/node) and[MCP server](https://docs.firecrawl.dev/mcp-server) docs. To scrape an entire site from a root URL rather than walk a known paginated sequence, the[/crawl endpoint](https://www.firecrawl.dev/blog/mastering-the-crawl-endpoint-in-firecrawl) handles link discovery and traversal automatically. For infinite scroll or click-driven pagination where no URL changes, the[interact endpoint](https://www.firecrawl.dev/blog/firecrawl-interact-endpoint) runs a real browser session you control.
