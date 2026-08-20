---
schema_version: "1.0.0"
document_id: "a7c77cb7e55e242aaa9e3194af0409714fde4e4aa2064761878ed757095d674c"
company_key: "yc-axiom-ai"
company: "Axiom.ai"
source_id: "yc-axiom-ai-news-import-c535b3e44cdf"
canonical_url: "https://axiom.ai/blog/web-scraping-in-one-line-of-code/"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-12T01:49:15.680853+00:00"
fetched_at: "2026-08-12T01:49:17.339532+00:00"
content_hash: "sha256:8ddb37818b6680af65d55a51a750ed0a4597edecb9c410f44f739af51b28282d"
---

# Web scraping in one line of code (or zero)

[← All blog posts](https://axiom.ai/blog)


# Web scraping in one line of code (or zero)


August 10, 2026


You can code a scraper. You can ask Claude to code a scraper. Writing the code is not the blocker. The challenge is everything around it: adding the waits, testing the code, even just running it. Sometimes you want the data quickly, without the fuss.


We took the power of axiom's[no-code builder](https://axiom.ai/build-with-no-code/step-by-step) steps, the ones that let you build a web scraper in just a few steps, and exposed them via an API: the[Step API](https://axiom.ai/docs/developer-hub/api/step-functions) .


One line drops a scraper into your code: it paginates on its own and stops at the result count you set. Or call the step over MCP: a URL and a selector gets you the data in seconds. Zero lines.


## An entire scraper in one line


```text
await   axiom.  scrape  (url, selector, pager, max_results, settings);


```


That's the scraper: one line, simple to use. Pass in one or several selectors for the content you want scraped, add a selector for the pager, and set max results. In your code, you'll still need to open and close the browser session around it. (Over MCP it drops to zero lines: you describe the scrape in a prompt. More on that below.) But all the fiddly bits you'd normally spend your time writing and maintaining are gone: the pagination loop, the load waits, the retry logic. When the[scraper()](https://axiom.ai/docs/developer-hub/api/step-functions/scrape) finishes, it returns a 2D array of the scraped data for you to use in your build.


## What the Step API is


The Step API is axiom’s no-code tool, available to all. It’s a set of around 20 functions, each step performs a complete browser action:[goto](https://axiom.ai/docs/developer-hub/api/step-functions/goto) ,[click](https://axiom.ai/docs/developer-hub/api/step-functions/click) ,[enterText](https://axiom.ai/docs/developer-hub/api/step-functions/enter-text) ,[scrape](https://axiom.ai/docs/developer-hub/api/step-functions/scrape) , and so on. These aren't low-level primitives like “dispatch a keypress”: each function does the whole job and returns when it’s actually done. goto() doesn’t resolve until the page has settled. scrape() pages through results on its own.


Steps wait and retry - functionality you normally write waitForSelector calls for, the step does that internally.


You can call the steps three ways, all with the same API key, against cloud or desktop browsers:


- The[Node library](https://axiom.ai/docs/developer-hub/api/step-functions) : import the client and call step functions directly from your code.
- An MCP server: expose the steps as tools so Claude or another LLM client can drive a session.
- In an existing script: connect your[Puppeteer](https://axiom.ai/build-with-code/puppeteer) or[Playwright](https://axiom.ai/build-with-code/playwright) code to an axiom.ai browser and mix in step calls.


Note: these functions come from the engine that runs axiom.ai’s no-code tool. Hundreds of thousands of users have been running that engine against real sites for years: slow pages, awkward pagination. Every failure mode they hit, we fixed. The Step API exposes that engine directly to code.


As the no-code tool evolves, we’ll be exposing more steps through the Step API.


## Scraping over MCP: zero lines


Over MCP, zero code. You don't write the scrape call; you prompt. Connect the axiom.ai MCP server to Claude and describe what you want:


"Go to[https://www.bbc.co.uk/search?d=SPORT_PS&q=Harry+Kane](https://www.bbc.co.uk/search?d=SPORT_PS&q=Harry+Kane) , scrape the titles and links of articles, and save them as a CSV"


Under the hood, the MCP server runs the steps you'd otherwise have written:


```text
const   rows   =   await   axiom.  scrape  (  "https://www.bbc.co.uk/search?d=SPORT_PS&q=Harry+Kane"  ,   "[role='list'] li a"  ,  null  ,  2  , {});


```


The run ends with listings.csv, a file anyone can open in a spreadsheet. A browser opens, scrape() collects the rows, outputs a CSV. You don't code a scraper, you describe one. Zero code.


## Adding the Step API to existing code


If you already have a Puppeteer or Playwright script, there's no need to rewrite it. Connect it to an axiom.ai browser and the script runs on our cloud or desktop infrastructure. The Step API is available in that same session, so you can drop a` scrape()` call where the extraction logic would have gone and use the rows it returns in the code you already have.


```text
import   { AxiomApi }   from   '@axiom_ai/api'  ;


const   axiom   =   new   AxiomApi  (  'eyJ1c2VyX2lkIjoyMzI5LCJ0b2tlbiI6IjIzMjk1YzQ5NjU5ZjVjOWRiYmFkIn0='  );


await   axiom.  browserOpen  ();
try   {
const   rows   =   await   axiom.  scrape  (
'https://www.bbc.co.uk/search?q=Harry+Kane&d=NEWS_PS'  ,
[
{ selector:   '[data-testid="default-promo"] p[class*="PromoHeadline"]'  , resultType:   'textContent'   },
{ selector:   '[data-testid="default-promo"] a[href]'  ,                   resultType:   'link'   },
],
'nav div:nth-child(4) a[href^="/search?q=Harry+Kane&d=NEWS_PS&page="]'  ,
50  ,
{}
);
console.  log  (rows);
}   finally   {
await   axiom.  browserClose  ();
}


```


> **Note:** the key is written into the file here only to keep the example short. Never do this in real code — an API key is a secret, and anything committed to a repo or pasted into a doc should be treated as public. Read it from an environment variable (` process.env.AXIOM_API_KEY` ) or a secrets manager instead.


Two selectors, so each row comes back with two columns — the headline text and the article link. The third argument is the pager:` scrape()` follows it until it has 50 results.


## Getting started


You'll need an API key from the[Code dashboard](https://code.axiom.ai/) or the[no-code tool](https://axiom.ai/build-with-no-code) . New accounts get 2 free hours of browser runtime, no card required. The Step API is open to everyone with an axiom.ai account, no-code and Code tool users alike.


Install the client then you can run this minimal example:


```text
npm   install   @axiom_ai/api


```


```text
import   { AxiomApi }   from   '@axiom_ai/api'  ;


const   axiom   =   new   AxiomApi  (process.env.  AXIOM_API_KEY  );


await   axiom.  browserOpen  ();
const   rows   =   await   axiom.  scrape  (  "https://example.com/products"  ,   ".product-card"  ,   null  ,   200  , {});
await   axiom.  browserClose  ();


```


Five lines of code in total, one line of scraper. Always[close the session](https://axiom.ai/docs/developer-hub/api/step-functions/close-a-session) ; open sessions consume your runtime quota.


The[quickstart](https://axiom.ai/docs/developer-hub/api/quickstart) covers setup, and the[step function reference](https://axiom.ai/docs/developer-hub/api/step-functions) lists all the step functions, including[scrapeMetadata](https://axiom.ai/docs/developer-hub/api/step-functions/scrape-metadata) for structured page fields,[integrateAI](https://axiom.ai/docs/developer-hub/api/step-functions/integrate-ai) for inline LLM calls.


## Selecting more data nd different types


## How an expert in browser automation at axiom uses the Step API


This is a relatively new feature and one I'm still discovering: the no-code steps exposed as an API to consume. Already I've used the scrape() function to build micro scrapers that extract data for reporting. Surprisingly though, the biggest impact on me so far is in no-code. With the scrape() function I fetch a page's HTML and have the LLM extract valid selectors from it. It saves me a lot of time, and it means I can build no-code axioms entirely from VS Code using Claude Code. My go-to method now is building by description in Claude, using MCP and the Step API to quickly scaffold automations that work out of the box.


## Wrapping up simple scraping


Scraping doesn't have to mean lines of code, testing and fixing. With axiom's Step API you can plug a scraper straight into your own Puppeteer or Playwright scripts and skip the time you'd spend testing and ironing out bugs. It's a battle-tested system, with many users feeding back into it.


What should you use it for? Over MCP, generate selectors as mentioned above, or run micro scrapes that fetch data through the scrape step. The steps also fit well into custom scripts, giving you a high degree of flexibility. And this is where coders and no-coders work the same way: generate your selectors over MCP, then run the scraper wherever suits you, in the no-code builder or in your own script. Scrape isn't the only function in the Step API either. Other steps such as click and enterText cover all use cases. Over the coming months we'll also roll out settings to the Step API and add options such as 1Password integration.


Grab an API key, open a cloud browser, and go scrape something. If you're a new user, the[Claude skill](https://axiom.ai/build-in-claude) will take you through setting up a free account as well.


FYI: axiom v5.2, the[next release](https://axiom.ai/docs/no-code-tool/release-notes) , ships an MCP server for the[desktop](https://axiom.ai/infrastructure/desktop) and our[cloud](https://axiom.ai/infrastructure/cloud) .


[Join discussion on r/axiom_ai](https://reddit.com/r/axiom_ai)


###### On this page


- An entire scraper in one line
- What the Step API is
- Scraping over MCP: zero lines
- Adding the Step API to existing code
- Getting started
- Selecting more data nd different types
- How an expert in browser automation at axiom uses the Step API
- Wrapping up simple scraping


By


[Alex Barlow](https://axiom.ai/authors/alex-barlow) , Co Founder


Alex spent 14 years automating repetitive tasks, before co-founding axiom.ai. He’s hands-on with users and enjoys learning from them. He creates intricate automation the…
