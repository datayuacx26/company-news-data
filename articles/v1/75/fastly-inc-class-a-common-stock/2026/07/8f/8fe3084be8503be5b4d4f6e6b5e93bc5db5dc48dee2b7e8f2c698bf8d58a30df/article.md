---
schema_version: "1.0.0"
document_id: "8fe3084be8503be5b4d4f6e6b5e93bc5db5dc48dee2b7e8f2c698bf8d58a30df"
company_key: "fastly-inc-class-a-common-stock"
company: "Fastly Inc."
source_id: "fastly-inc-class-a-common-stock-rss-83c7761b19d9"
canonical_url: "https://www.fastly.com/blog/teaching-coding-agents-to-check-their-vcl-with-fastly-fiddle/"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-07-30T16:41:29.343753+00:00"
fetched_at: "2026-07-30T16:41:30.643115+00:00"
content_hash: "sha256:d072e716b199e8463ddcf1a844d3d8baa055ba4c5be2a2be00603f4b0a37302d"
---

# Teaching Coding Agents to Check Their VCL with Fastly Fiddle

As a Solutions Architect at Fastly, I spend a bunch of time writing VCL (Fastly's Varnish Configuration Language) to provide suitable solutions for customers. I also build tools that help coding agents write VCL. Either way, I hit the same snag: the real Fastly VCL compiler lives on the edge, not on your laptop.


You can lint locally, but some things only exist on production Fastly infrastructure: geolocation data, WAF behaviour, real cache clustering, shielding, rate limiting... If you want to know whether your VCL actually works, you need to find out from Fastly itself.


[Fastly Fiddle](https://fiddle.fastly.dev/) fills this gap.


## A sandbox on Real Infrastructure


Fiddle is a web-based tool that compiles and executes VCL on real Fastly edge nodes. Not a simulation, not a local reimplementation. The actual production compiler, running on actual POPs across the globe. You give it some VCL and a set of requests, and it runs them through the full Fastly state machine:` vcl_recv` ,` vcl_fetch` ,` vcl_deliver` , the lot.


This makes it the only place outside a deployed service where you can test features that depend on edge infrastructure. Want to check how` client.geo.country_code` behaves for a specific IP? Fiddle runs on a real POP with real geo data. Want to see how shielding affects your cache hit ratio? Fiddle can enable cluster and shield on a per-request basis. Curious about rate limiting? It's all there.


## For Humans: Exploration and Sharing


When I'm helping a customer debug a caching issue, my first instinct is often to open Fiddle and reproduce the problem. I can write a few lines of VCL, fire a request at it, and see exactly what happens: which subroutines ran, what the origin saw, what the client received. If the customer needs to see the reproduction, I send them the Fiddle URL, which contains all the logic and demonstrates the behaviour.


Fiddle is particularly good for exploring VCL functions and variables you haven't used before. Fastly's VCL dialect has over 300 built-in variables (` workspace.bytes_free` ,` fastly_info.state` ,` req.digest.ratio` ...) and functions (` std.strlen` ,` regsuball` ,` digest.hash_sha256` ...). Reading the documentation tells you what they should do. Fiddle tells you what they actually do, on real traffic, with real values.


The shareable URL is quietly one of Fiddle's finest features. I've lost count of the number of support tickets where the fastest path to resolution was a fiddle link showing the exact behaviour in question. A reproducible edge execution anyone can re-run.


## For Agents: Checking Their Own Work


What if the one asking the question isn't a person?


Fastly has been building an[agent toolkit](https://github.com/fastly/fastly-agent-toolkit/) that teaches coding agents to work with Fastly services. When I started thinking about how an agent could verify VCL it generates, Fiddle fit perfectly.


The Fiddle API is undocumented, but stable. An agent can POST some JSON containing VCL and a set of test requests, and get back structured results: lint diagnostics with line numbers, pass/fail on each assertion, and expected vs actual values. Fiddle has a small test DSL that we can use to write assertions on the response:


Copied!


```text
clientFetch.status is 200
originFetches.count() is 0
clientFetch.bodyPreview includes "BadBot"
```


An agent can write and parse these without scraping a web page.


Every POST also returns compilation diagnostics immediately, without executing anything. So an agent generating VCL to, say, build a synthetic` robots.txt` or route requests by geography can check "does this even compile?" in one round trip, fix lint errors, then run the full test suite against real edge nodes. Does it produce the right status code? Does it avoid hitting origin? The agent gets a clear pass or fail from the same infrastructure that will run the code in production.


Let's look at[a small example](https://fiddle.fastly.dev/fiddle/9ce1f1f4) . I was curious what geo variables Fastly exposes, so I asked the agent (with the Fiddle skill installed) to "write a fiddle to show me some geo IP country variables." It created a fiddle with` vcl_recv` triggering a synthetic response:


Copied!


```text
# vcl_recv
error   601  ;


# vcl_error
if   (  obj.status   ==   601  ) {
set   obj.status   =   200  ;
set   obj.http.Content-Type   =   "text/plain"  ;
synthetic   "country_code: "   +   client.geo.country_code   +   LF
+   "country_name: "   +   client.geo.country_name   +   LF
+   "continent_code: "   +   client.geo.continent_code   +   LF
+   "city: "   +   client.geo.city   +   LF  ;
return  (  deliver  );
}
```


Run it and you get back real geo data from whatever POP executes the request, in my case:


Copied!


```text
country_code: GB
country_name: United Kingdom
continent_code: EU
city: lambeth
```


Fresh from Fastly, nothing deployed. You can't get that from a local linter.


## Why the Same Tool Works for Both


I had a little think about this. Fiddle wasn't designed for agents. It was designed to be a low-friction sandbox: open access, instant execution, clear feedback when something goes wrong. But those are exactly the properties an agent needs too. Rather than use the fiddle web user interface, it just POSTs JSON and gets JSON back. And the pass/fail signal is identical: a human reading "expected 200, got 503" in the browser and an agent parsing` {"pass": false, "expected": "200", "actual": "503"}` from the API are learning the same thing. The sandbox works great for humans and agents alike.


## Where Fiddle Fits


So should you throw away your local linter? Fiddle complements local tooling rather than replacing it. The[falco linter](https://github.com/ysugimoto/falco) is faster for tight iteration loops (sub-second, offline, watch mode). Our[VS Code extension](https://marketplace.visualstudio.com/items?itemName=fastly.vscode-fastly-vcl) gives you completions, diagnostics, and navigation as you type. These are the inner loop.


Fiddle is the outer loop. You reach for it when you, or your agent, need the real edge to see how something works: debugging a customer issue, exploring an unfamiliar feature, or confirming that generated VCL does what it should.


## Try it


[Fastly Fiddle](https://fiddle.fastly.dev/) is free, open to everyone, and runs your VCL on real Fastly infrastructure. Open a browser tab and tinker with something.


Want your agent to use Fiddle the same way? Have a look at the[Fastly agent toolkit](https://github.com/fastly/fastly-agent-toolkit/) and let it find out for itself. To install it for your agent, run:


Copied!


```text
npx skills add github:fastly/fastly-agent-toolkit --skill fastly-fiddle
```
