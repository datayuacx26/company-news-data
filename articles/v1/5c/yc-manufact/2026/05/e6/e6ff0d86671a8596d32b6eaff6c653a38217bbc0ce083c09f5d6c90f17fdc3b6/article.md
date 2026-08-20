---
schema_version: "1.0.0"
document_id: "e6ff0d86671a8596d32b6eaff6c653a38217bbc0ce083c09f5d6c90f17fdc3b6"
company_key: "yc-manufact"
company: "Manufact (formerly mcp-use)"
source_id: "yc-manufact-news-import-914a697bc101"
canonical_url: "https://manufact.com/blog/iahorro-mcp-app-launch"
published_at: "2026-05-29T00:00:00+00:00"
first_seen_at: "2026-07-22T03:18:57.853834+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:737b0086890c1c21357d16622c8213424365ef3c535058156164ae9a30568d3e"
---

# How iAhorro shipped a mortgage simulation app on ChatGPT in 4 days

# How iAhorro shipped a mortgage simulation app on ChatGPT in days


iAhorro is Spain's leading independent mortgage advisor. They help families compare offers, simulate financing, and get expert guidance from the first question all the way to signature.


More mortgage questions are starting in ChatGPT. So iAhorro built a ChatGPT app that lets customers run a mortgage simulation in plain Spanish, with real iAhorro pricing, and get connected to a human advisor when they are ready.


The app is currently available to ChatGPT users in Spain:[open iAhorro on ChatGPT](https://chatgpt.com/apps/iahorro-hipotecas/asdk_app_69d63024d71c8191ba3666eb462f994b) .


## The starting point


When we first sat down with iAhorro, the use case was already clear:


- A conversational mortgage simulator that calls iAhorro's own pricing API. Realistic rates based on the user's amount, down payment, location, and property type.
- An early prototype. The iAhorro team had put together a first version of the MCP server in a few days. It worked end-to-end, but it was far from production-ready and did not yet meet the requirements and guidelines an MCP App has to satisfy to be published on ChatGPT.
- A clean lead handoff. Capture the right contact details at the right moment, push them to iAhorro's CRM, and let a real advisor take it from there.


With no previous experience with the ChatGPT App Store, and no bandwidth to figure out submission and post-launch monitoring from scratch, they were looking for a partner to help them get to production quality and ship the app in time.


## The collaboration


iAhorro built their MCP server using[mcp-use SDK](https://github.com/mcp-use/mcp-use) , the open-source MCP SDK. It is the most-used MCP framework on GitHub today.


It gave the team:


- A typed server with clean tool definitions for the simulator and the lead-capture flow.
- Sensible defaults for tool naming, input validation, and authentication. These are the patterns most likely to pass a ChatGPT app review.
- Drop-in support for React-based MCP UI views, so the simulation cards render correctly inside ChatGPT.


The first deployable build was running in four working days of focused effort. The rest of the timeline was about getting right some UI elements and passing ChatGPT app store review.


Once the MCP App server compiled, the team connected iAhorro's GitHub repository to Manufact Cloud. From that point, shipping meant merging to` main


`


.


Tip


Hosting


Connect GitHub, get branch previews and a production MCP endpoint with managed TLS, env vars, and deploy history.


[Learn more →](https://manufact.com/platform/hosting)


## First-pass review, on a deadline


iAhorro had a launch date that wasn't moving. The ChatGPT App Store review process is long, partly published, and partly learned the hard way — protocol conformance, tool naming, security headers, allowed domains, asset dimensions. One miss means a multi-week rejection cycle.


Before submitting, iAhorro ran Manufact's publishing checks against their production endpoint.


The checks surfaced everything that would have triggered a manual review fail. A couple of tool annotations to tighten, asset dimensions for the store listing, a domain reachability issue. The team resolved them in an afternoon. The app went into the store on the first try.


Tip


Publishing checks


Audit your MCP app against ChatGPT App Store requirements: protocol, tools, security, domains, assets, and more, directly on Manufact Cloud.[Learn more →](https://manufact.com/platform/publishing-checks)


The app went live ahead of the deadline. From day one, the iAhorro team had visibility into how customers were using it through Manufact Analytics:


- How often the simulator is being called, and at what shape of scenario.
- The drop-off and conversion at each step before` request_callback


`


fires.


The ChatGPT channel ends up evaluated the same way the rest of iAhorro's funnel is evaluated: by the quality of the conversations that end with an advisor on the phone.


## The result


A full working mortgage simulation app on ChatGPT, with the same UI and user experience as the website.


iAhorro has full ownership of the product, the data, and the customer relationship. Manufact handled the parts that aren't iAhorro's business: MCP hosting, testing, submission, and monitoring.


Before Manufact With Manufact and mcp-use


Time to first MCP app live Open-ended A few working days


QA loop Developer-blocked Self-serve via hosted Inspector


Submission risk High, opaque criteria Pre-flighted via publishing checks


Post-launch operations DIY DevOps Built-in hosting, analytics, alerting


Engineering focus DevOps and protocol plumbing The product experience


## What's next


The launch product is two tools because the launch goal was to prove the dialogic flow. With that proven, the surface area opens up to more tools. This is also our recommendation for future MCP Apps: start with few tools, pass the review, and then add more.


If you are a financial services team, or any consumer brand, thinking about how to show up inside ChatGPT, the same approach works:


- Build with[mcp-use](https://github.com/mcp-use/mcp-use) , the open-source MCP SDK.
- Deploy with[Manufact Hosting](https://manufact.com/platform/hosting) . Connect your GitHub repo and ship in one click.
- QA across clients with the[hosted Inspector](https://manufact.com/platform/cross-client-testing) .
- De-risk submission with[publishing checks](https://manufact.com/platform/publishing-checks) .
- Operate with[analytics](https://manufact.com/platform/analytics) .


[Talk to us →](https://manufact.com/book-call)


iAhorro is Spain's leading independent mortgage advisor. Learn more at[iahorro.com](https://www.iahorro.com/) .
