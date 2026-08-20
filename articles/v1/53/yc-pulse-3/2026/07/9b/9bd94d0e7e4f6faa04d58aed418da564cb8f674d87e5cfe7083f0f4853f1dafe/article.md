---
schema_version: "1.0.0"
document_id: "9bd94d0e7e4f6faa04d58aed418da564cb8f674d87e5cfe7083f0f4853f1dafe"
company_key: "yc-pulse-3"
company: "Pulse"
source_id: "yc-pulse-3-news-import-f90f167021ce"
canonical_url: "https://www.runpulse.com/blog/welcome-to-the-agent-economy-agentic-payments-go-live-on-pulse"
published_at: "2026-05-11T00:00:00+00:00"
first_seen_at: "2026-07-23T21:33:19.979240+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:38e8f2e302b8f755e5fde109dba5059d22f44f40f63ae5e4421839fc362b9c93"
---

# Welcome to the Agent Economy: Agentic Payments Go Live on Pulse

Point an agent at our API with no account, no API key, and no signup, and it can pay for extraction on its own, one call at a time. Our main endpoints now settle charges via the[Machine Payments Protocol](https://mpp.dev/) (MPP), an open standard for paying an API inline over HTTP, in stablecoin, in the same request that asks for the work.


This is a bet on where software is going, because the web we have was built around a human at a checkout, with an account made in advance and a card on file, while the web that is arriving is built around a program at an API, an agent that shows up with a task, needs a tool it has never used, and has no human standing by to sign it up. Those two worlds need different payment rails, and until now the second one did not have one.


MPP runs on a status code that has sat reserved since the early days of HTTP: 402 Payment Required. It waited decades for a caller that could pay on its own, and it is the layer that lets software buy from software directly.


### How it works


An agent sends a normal extraction request. If the call needs payment, Pulse answers with 402 Payment Required and a WWW-Authenticate: Payment challenge that names exactly what to pay: the amount, the token, and a one-time deposit address minted for that request. Those are the priced terms of the specific work the agent asked for.


The agent pays in USDC, echoes the proof back on the retry, and Pulse runs the job and returns the result, all in two HTTP round trips with no invoice to reconcile and at the same per-page rate as any other Pulse surface. The charge is signed to the request, so a paid call cannot be replayed against another one, and an unpaid request is never billed for work it did not run.


### Where it fits


‍


MPP is for calls with no human in the loop, so an agent that discovers tools at runtime, finds Pulse through the MCP server, and needs to read a document it was handed thirty seconds ago can just do that, with nothing to stop for.


If you already have a Pulse account, keep using it. Your API key, SDKs, and dashboard are the right tools for a pipeline you own and run on a schedule. MPP is for the other case, where the caller is autonomous, short-lived, or one you will never see again.


### The bigger bet


The interesting part is what happens when every service works this way. An agent with a wallet and a goal assembles the tools it needs on the fly, pulling a document from one service, extracting it with Pulse, and verifying it against another while paying each per call as it goes, with no contracts, no seats, and no procurement in between. The tools compose because the payment composes with them.


That is the frontier we are building toward: an open market of services any agent can reach and pay for the instant it needs them, priced by the call and settled in real time. Machine-to-machine commerce stops being a slide in a deck and starts being how the software runs. Extraction is a natural place to start, because reading a document is often the first thing an agent has to do before it can act.


### Bottom line


MCP gave agents a way to discover and call Pulse. MPP gives them a way to pay for it, with no person between the request and the invoice. Together, an agent can find a tool it has never used, learn what it costs, pay for one call, and get back structured data on its own.


The 402 was always the missing piece, and it is live on Pulse today. Send a request, read the challenge, and let your agent pay for its own answer.
