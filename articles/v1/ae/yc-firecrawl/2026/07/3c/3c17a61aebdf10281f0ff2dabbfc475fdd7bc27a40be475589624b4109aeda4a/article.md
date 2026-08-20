---
schema_version: "1.0.0"
document_id: "3c17a61aebdf10281f0ff2dabbfc475fdd7bc27a40be475589624b4109aeda4a"
company_key: "yc-firecrawl"
company: "Firecrawl"
source_id: "yc-firecrawl-news-import-e3b10de50c72"
canonical_url: "https://www.firecrawl.dev/blog/webmcp-headless-agents-firecrawl"
published_at: "2026-07-17T00:00:00+00:00"
first_seen_at: "2026-07-24T08:10:35.258497+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:908fe80f52c4c763acb6c777eca3cfdd28104173737970deac66b7fb8e795ad8"
---

# How to Use WebMCP with a Headless Agent (Claude Code) Using Firecrawl

**WebMCP** is an upcoming web feature that makes any site easier for an AI agent to use. Instead of the agent scraping a page's HTML to work out how it functions, the site hands it **developer-defined tools** , just like an MCP server, which saves a lot of tokens.


The catch: WebMCP only works inside a live browser tab, and most agent harnesses like **Claude Code** or a custom agentic workflow don't have one. Firecrawl's **interact** endpoint closes that gap by running a real browser in the cloud.


## What WebMCP actually is


Websites are built for humans, not agents. They're full of images, animations, and accessible markup that mean nothing if all you want is for an AI to book you a table. So engineers from Google and Microsoft put forward a[proposal for WebMCP](https://github.com/webmachinelearning/webmcp) : a developer registers tools for the actions an agent can take on a page, using JavaScript or HTML.


Each tool gets a **name** , a **description** , an **input schema** , and the **logic** that runs when it's called, exactly like a regular MCP tool. So booking a table becomes a single tool call instead of the agent finding every input, filling each one in, and clicking submit. Much less work, and far fewer tokens.


But there's a hard limitation, spelled out in the[Chrome WebMCP docs](https://developer.chrome.com/docs/ai/webmcp#limitations) : right now there's **no support for agents to call these tools in a headless browser** . The agent has to drive a live browser directly, which means the browser itself needs an onboard agent to use WebMCP.


This is where **Firecrawl** comes in. Its[interact](https://docs.firecrawl.dev/features/interact) endpoint runs a real browser in Firecrawl's cloud that your agent controls, either by describing what you want in a **prompt** or by executing **code** directly on that browser. So a headless harness gets a live browser tab to run WebMCP tools in, without shipping one itself. Here's how to set it up.


## The demo site


To keep it concrete, this walkthrough uses a fake restaurant, **Le Petit Bistro** , where you fill in a form to book a table. It's based on Google's[French Bistro demo](https://github.com/GoogleChromeLabs/webmcp-tools/tree/main/demos/french-bistro) , which ships with **WebMCP tools already registered** . I stripped all of them out to use as a clean **before** baseline: a plain reservation form with no agent tools at all. You can clone that starter from the[webmcp-firecrawl-demo repo](https://github.com/firecrawl/webmcp-firecrawl-demo) (its` stage-1` branch adds the WebMCP tool back).


Because I removed them, the site has no WebMCP tools right now. You can confirm that by enabling **WebMCP for testing** in Chrome and installing the[WebMCP Tool Inspector](https://chromewebstore.google.com/detail/webmcp-model-context-tool/gbpdfapgefenggkahomfgkhfehlcenpd) extension, which reports what's registered on the page:


## Giving the site a WebMCP tool


WebMCP is set to ship natively in[Chrome 157](https://chromestatus.com/feature/5117755740913664) , targeted for stable release around **November 2026** . As of writing, stable Chrome is only on version 150, so native WebMCP hasn't landed yet, it currently runs behind a flag and through origin trials. To build a demo today you use the[@mcp-b/webmcp-polyfill](https://www.npmjs.com/package/@mcp-b/webmcp-polyfill) package instead.


After initializing the polyfill, you register a tool whose input schema mirrors the form's fields, then define an **execute** function that does the real work:


```text
// script.js


import   { initializeWebMCPPolyfill }   from   "@mcp-b/webmcp-polyfill"  ;


initializeWebMCPPolyfill  ();


document  .  modelContext  .registerTool  ({
name  :   "book_table_le_petit_bistro"  ,
description  :
"Initiates a dining reservation request at Le Petit Bistro. Accepts customer details, timing, and seating preferences."  ,
inputSchema  :   {
type  :   "object"  ,
properties  :   {
name  :   { type  :   "string"  ,   description  :   "Customer's full name (min 2 chars)"   }  ,
phone  :   { type  :   "string"  ,   description  :   "Customer's phone number (min 10 digits)"   }  ,
date  :   { type  :   "string"  ,   format  :   "date"  ,   description  :   "Reservation date (YYYY-MM-DD)."   }  ,
time  :   { type  :   "string"  ,   description  :   "Reservation time (HH:MM)"   }  ,
guests  :   { type  :   "string"  ,   description  :   "Number of people dining."   }  ,
seating  :   { type  :   "string"  ,   description  :   "Preferred seating area"   }  ,
requests  :   { type  :   "string"  ,   description  :   "Special requests (allergies, occasions, etc.)"   }  ,
}  ,
required  :   [  "name"  ,   "phone"  ,   "date"  ,   "time"  ,   "guests"  ]  ,
}  ,
execute  :   async   (input)   =>   {
document  .getElementById  (  "name"  ).value   =   input  .name;
document  .getElementById  (  "phone"  ).value   =   input  .phone;
document  .getElementById  (  "date"  ).value   =   input  .date;
document  .getElementById  (  "time"  ).value   =   input  .time;
document  .getElementById  (  "guests"  ).value   =   input  .guests;
if   (  input  .seating)   document  .getElementById  (  "seating"  ).value   =   input  .seating;
if   (  input  .requests)   document  .getElementById  (  "requests"  ).value   =   input  .requests;


validateForm  ();
if   (  formValidationErrors  .  length  ) {
return   { content  :   [{ type  :   "text"  ,   text  :   JSON  .stringify  (formValidationErrors) }]  ,   isError  :   true   };
}


showModal  ();
return   { content  :   [{ type  :   "text"  ,   text  :   modalDetails  .textContent }] };
}  ,
});
```


The key part is` registerTool` . The **inputSchema** is plain[JSON Schema](https://json-schema.org/) , so the agent knows exactly which fields the tool takes and which are` required` , no scraping the DOM to guess at what the form wants.


The` execute` function is what actually runs when the agent calls the tool. It receives those validated input arguments and holds the site's own logic: it drops the model's values into each form field, runs the page's existing` validateForm()` check, returns the validation errors with` isError: true` if anything's wrong, and otherwise shows the confirmation modal and hands the reservation text back to the agent.


Reload the page and the tool now shows up in the inspector, with` book_table_le_petit_bistro` selectable and its full schema visible:


With a Gemini API key you could type a prompt right into the inspector and let it fill the form. But point **Claude Code** at the same page, even with a browser MCP, and it does the slow thing: scrapes every input, fills each one, then takes a screenshot with computer use to check its work. You can imagine how many tokens that burns.


## The local baseline: agent-browser


Before bringing in Firecrawl, it helps to see the mechanism locally with[agent-browser](https://github.com/vercel-labs/agent-browser) , a CLI that runs a browser you can drive with code. A small script uses its` eval` command to run JavaScript in that browser: it looks for` document.modelContext` , and if it finds one, reads the available tools and can execute any of them by name with JSON arguments.


```text
# find-webmcp-tools.sh


# Open the page in the local browser, then read its WebMCP tools
agent-browser   open   "$URL"


agent-browser   eval   "
document.modelContext
? document.modelContext.getTools().then(t =>
t.map(({ name, description, inputSchema }) => ({ name, description, inputSchema }))
)
: 'document.modelContext is undefined (no WebMCP support on this page)'
"
```


` agent-browser open` loads the URL in a local browser and keeps the tab alive, then` agent-browser eval` runs JavaScript in that page, the same as calling` document.modelContext.getTools()` from the console. To call a tool you run a second` eval` that finds it by name and passes it to` document.modelContext.executeTool()` .


That works, and it's fast, no scraping or screenshots. But` agent-browser` needs a real browser on the machine to do it: it drives a local Chrome, downloading Chrome for Testing on first run via` agent-browser install` . If your agent runs somewhere you can't install or launch one, an online environment, a CI pipeline, a locked-down work machine, that path is out.


## The bridge: Firecrawl interact


This is where Firecrawl's[interact](https://docs.firecrawl.dev/features/interact) endpoint comes in. It gives you the same kind of browser control, but the browser lives in **Firecrawl's cloud sandbox** , so nothing is installed on your machine.


Install the[Node SDK](https://docs.firecrawl.dev/sdks/overview) first:


```text
npm   install   firecrawl
```


The flow is scrape, then interact. You scrape the page first to open a browser session and get a` scrapeId` , then keep that same session alive by calling` interact` against it:


```text
// find-webmcp-tools-firecrawl.ts


import   { Firecrawl }   from   "firecrawl"  ;


// No API key needed to get started, add one for higher rate limits.
const   firecrawl   =   new   Firecrawl  ({ apiKey  :   process  .  env  .  FIRECRAWL_API_KEY   });


// 1. Scrape to open a browser session. maxAge: 0 forces a real browser load,
// a cache-served scrape returns a scrapeId with no live session behind it.
const   scrapeResult   =   await   firecrawl  .scrape  (url  ,   { formats  :   [  "markdown"  ]  ,   maxAge  :   0   });
const   scrapeId   =   scrapeResult  .  metadata  ?.scrapeId;


// 2. Interact, discover the WebMCP tools in that same session.
const   discovery   =   await   firecrawl  .interact  (scrapeId  ,   {
code  :   `
const tools = await page.evaluate(() => {
return document.modelContext
? document.modelContext.getTools().then(t =>
)
: 'document.modelContext is undefined (no WebMCP support on this page)';
});
JSON.stringify(tools);
`  ,
language  :   "node"  ,
});
console  .log  (  discovery  .result);


// 3. Stop the session when you're done.
await   firecrawl  .stopInteraction  (scrapeId);
```


The code you pass to` interact` runs in the cloud browser, where` page` is a Playwright Page object already connected to the session. Inside` page.evaluate()` , you're running JavaScript in the actual page, so` document.modelContext.getTools()` returns exactly what the site registered. That's the whole trick: it works because it's a **real browser** , so WebMCP just works, with no special integration on Firecrawl's end.


A couple of things worth calling out. The only real difference from the` agent-browser` script is that this uses Playwright's` page.evaluate()` instead of` agent-browser eval` . Firecrawl interact lets you write[Node](https://docs.firecrawl.dev/features/interact) or[Python](https://docs.firecrawl.dev/sdks/python) Playwright, or` agent-browser` commands in bash mode, and since Node Playwright is the default code mode, writing it directly is the cleaner choice here.


To actually call a tool, you run a second` interact` against the same` scrapeId` , find the tool by name, and hand it to` document.modelContext.executeTool()` :


```text
// find-webmcp-tools-firecrawl.ts


const   invocation   =   await   firecrawl  .interact  (scrapeId  ,   {
code  :   `
const toolResult = await page.evaluate(async (argsB64) => {
const tools = await document.modelContext.getTools();
const tool = tools.find(t => t.name ===   ${  JSON  .stringify  (toolName)  }  );
if (!tool) return 'Tool not found:   ${  toolName  }  ';
const args = JSON.parse(atob(argsB64));
return document.modelContext.executeTool(tool, JSON.stringify(args));
},   ${  JSON  .stringify  (argsB64)  }  );
JSON.stringify(toolResult);
`  ,
language  :   "node"  ,
});
console  .log  (  invocation  .result);
```


The arguments are passed in as **base64** (` argsB64` ) and decoded inside the page with` atob` , which keeps quoting reliable across Firecrawl's transport. From the agent's point of view, calling` book_table_le_petit_bistro` is now one step, the same single tool call WebMCP promised, just proxied through a headless-friendly API.


## Driving it from Claude Code


Wrap those two calls in a small runner that discovers a page's tools, then calls one by name. Now you can point Claude Code straight at it. Ask it what tools a URL exposes and it finds the exact tool you registered, along with its required and optional fields:


While it runs, the[Firecrawl dashboard](https://www.firecrawl.dev/app) shows the live session under **Interact with a page** , the cloud browser temporarily spun up to load the site:


From there you just ask in plain language, "book me a table for next Thursday for two people." If you leave out required fields like the time or phone number, the agent asks or makes them up, then books the table with a single call. No local install, no scraping, no screenshots, and done in under a minute in the cloud.


## Caveats, and where this leaves you


This approach isn't perfect. Because a page has to use the polyfill today, and the polyfill runs on JavaScript, the discovery script works now. But once Chrome ships native WebMCP and sites declare tools with plain HTML tags instead, a script like this will need updating to look for those tags. The spec is also young: it recently renamed` navigator.modelContext` to` document.modelContext` , so more changes are likely before release.


Until (and if) an official headless story arrives, Firecrawl gives you a way to use **WebMCP headlessly** , from Claude Code or any agent, today.


*Check out the[Firecrawl interact docs](https://docs.firecrawl.dev/features/interact) or the[interact endpoint deep-dive](https://www.firecrawl.dev/blog/firecrawl-interact-endpoint) to go deeper.*
