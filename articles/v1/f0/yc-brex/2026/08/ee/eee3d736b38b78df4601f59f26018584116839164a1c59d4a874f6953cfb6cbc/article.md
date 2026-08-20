---
schema_version: "1.0.0"
document_id: "eee3d736b38b78df4601f59f26018584116839164a1c59d4a874f6953cfb6cbc"
company_key: "yc-brex"
company: "Brex"
source_id: "yc-brex-news-import-5eb786253ae8"
canonical_url: "https://www.brex.com/journal/not-all-mcps-are-created-equal"
published_at: "2026-08-03T00:00:00+00:00"
first_seen_at: "2026-08-05T06:54:49.924711+00:00"
fetched_at: "2026-08-05T06:54:51.503609+00:00"
content_hash: "sha256:5fcccd5ea6dd4659543568d007593846c3cb1beb348790b83197bd7523da1edc"
---

# Not all MCPs are created equal

If you're evaluating AI for your finance stack right now, your Request For Proposal (RFP) has a row for it: “MCP support ✓”. Four vendors, four checkmarks. It looks like a solved, commoditized feature.


It isn't. That checkmark hides the single biggest decision behind whether an AI agent can actually answer your finance team's questions or just waste your time confidently. Not all MCPs are created equal, and no RFP chart will tell you that. MCP, the Model Context Protocol, is the standard way an AI agent connects to a system like your finance platform. It says nothing about what's on the other end of that connection.


Anyone armed with a coding agent can stand up a working MCP server in under a week. The quick way is to put a thin layer over your existing API so an AI can reach it. Building one that actually answers your team's questions, at your scale, without leaking data or making up numbers, takes a lot more thought than that, and both versions earn the same checkmark on the comparison chart. The real question is how you tell a good one from one built to check a box.


### **The short version**


Most finance MCPs are built one of two ways, and both break down at scale, just in different places. One dumps raw records into the model and overwhelms the agent. The other loads and queries your data on demand, which works but makes you wait minutes for an answer that's already stale by the time it lands. Brex does the heavy lifting ahead of time: the data is continuously pre-computed and pre-joined, so even a query across 100,000 expenses comes back in 2 to 3 seconds, exact, current, and scoped to what the person asking is allowed to see. The rest of this post is how we got there, and what to ask any vendor with the MCP checkmark.


### **The question that breaks most modern finance implementations**


Most of what ships today is the fast version: a thin wrapper around an existing API, dressed up so an AI can reach it. Ours isn't, and the gap shows the moment someone asks a real question. Here's that question.


Your controller is closing the books. They ask the agent a question any analyst could answer given enough hours: "Which expenses are missing receipts, and how much spend does that represent by department?"


Simple question. For a small company, it's a handful of records. For a large one, it's tens of thousands. And how a vendor's MCP server handles that question, at both ends of that range, tells you almost everything about whether the thing behind the checkmark was built for scale or just built fast.


There are two common ways to build MCPs, and both fail in ways you'll only discover after you've already bought the software.


**Approach one: the API wrapper:**


This is the thin version from a moment ago: the MCP server wraps the existing endpoints, pulls the raw expense records, and hands them to the LLM to sort out. An AI model has a fixed amount of working memory, called its context window, measured in tokens. Showing the model 50 expenses takes roughly 10,000 tokens. In practical terms, a standard 200,000-token context window can comfortably hold around 500 expenses with room to spare; a larger 1-million-token window, 3,000 to 4,000. (The exact ceiling depends on which model you're using.)


This works for the smallest cases, an employee reviewing their own handful of expenses. But the people asking these questions are admins and controllers closing the books across the whole company, tens of thousands of expenses at a time. And at that scale, dumping data runs into two hard limits. First, it can't fit: expenses at that volume overflow any context window, at which point the model quietly stops reading and answers from the fraction it managed to load. Second: even when the data does fit, you're asking an LLM to do the arithmetic, and models are bad at arithmetic. Ask one to add up a hundred thousand transactions and it will give you an answer that looks right and isn't. In finance, a confident wrong number is worse than no number at all. Any implementation that’s relying on your LLM to do math is not the right one.


**Approach two: load, transform and query:**


This is a good improvement, and a few companies do this. Instead of dumping raw records into the model's context window, the agent pulls your data out of the API, transforms it, and loads it into a temporary in-memory database, then runs a SQL query against that. Now the database does the math, not the model, which fixes the arithmetic problem and lets the agent handle far more data than would ever fit in context. It's a genuinely better design than the API wrapper approach.


But look at what it costs. Every question has three steps: load, transform, and query. For a small company (under ~100 expenses a month), that whole round-trip might take 5 to 10 seconds. Tolerable. Two problems show up fast, though. First, the loaded copy is a snapshot: ask the same question an hour later and there are new transactions, so the agent has to load, transform, and query all over again from scratch. Second, this approach doesn't scale. For a company with thousands of expenses, "load the data" means paginating through the API 10, 20, 30 times, pulling every record across the wire before anything can be computed. Extraction and transformation alone can take four to five minutes before the query even begins, and then the query still has to join across multiple tables at runtime. Your CFO asked a question and is now watching a spinner for five minutes.


In both approaches, data has to travel to wherever the answer gets computed, on demand, every single time. That has a measurable cost to whoever asked. They wait, minutes at scale, and requests start timing out entirely on the largest accounts. The answer they finally get is a snapshot that's already stale, and the next question repeats the whole wait from scratch.


### **What we built at Brex:**


**A. The query layer:**


The question worth asking is how the data actually moves through the MCP, because that's the real tell for how it was built.


The better flow moves data continuously, ahead of time, so nothing has to move when someone actually asks a question. Underneath Brex's MCP server, a change-data-capture pipeline streams every update from the source systems as it happens and reshapes it into denormalized tables built for querying, so the expensive joins are already resolved and the data sits ready. We gave the agent real querying power over that data: it can filter, aggregate, group by, and sort across any dimension, without ever pulling the underlying records. When the agent needs an answer, it doesn't paginate through an API or load anything into a table. It sends one instruction (filter by this, group by that, sum this field) to a layer that already has the data in the right shape, the database does the full computation server-side, and only the finished answer comes back.


That changes the numbers completely. The "missing receipts and group by spend" question you saw earlier comes back in 2 to 3 seconds, and it does that even for a company with more than 100,000 expenses, because nothing is being extracted or transformed at question time; that work already happened. It costs roughly 800 to 900 tokens, because the only thing crossing into the model's context is a ten-row result, not the dataset behind it. The total is computed by a database, not guessed by a model, so it's exact. And because the pipeline is streaming, the data is effectively real-time, delayed only by the few minutes it takes to capture and denormalize a change. No snapshot to go stale, no reload on the next question.


Put it against both alternatives, at 100,000 expenses.


- Against approach one, the wrapper that loads records into context, it isn't a close call: those 100,000 expenses would run to around 20 million tokens, far past what any model can hold, so the wrapper simply can't answer the question at all. Ours crosses into context as a single small result at roughly 800 tokens. Same question, about twenty-five thousand times less data moved into the model, and unlike the wrapper, the arithmetic is done by a database instead of guessed at by the model.
- Against approach two, the load-transform-query pattern that at least gets the math right, we win on everything that made it painful: it needs minutes of pagination and transformation, three tool calls, a snapshot that goes stale on the next question, and joins computed at runtime, call it 3 to 5 minutes. Ours is 2 to 3 seconds, one call, always current, joins already resolved. That's roughly a hundred-fold speedup, from minutes to seconds, with none of the staleness. And it's permission-aware. Every query is scoped to the person asking it, so the agent only ever returns the data that the user is allowed to see.


**B. The intelligence layer:**


Some questions don't have a fixed definition. Ask "what's the most unusual spend?" and the right answer depends on the company, and even the person asking, since "unusual" means something different in each case. For questions like these, we built a second path: a conversational agent that holds a back-and-forth to work out what you're actually asking. It's schema-aware, ontology-aware, and permission-aware, and it returns answers rather than raw data. The extra inference and latency is what intelligence costs.


So each question takes the path that fits it. The query layer handles the lookups, filters, and aggregations, which are the overwhelming majority, and the conversational agent takes the ambiguous ones that need interpreting before they can be answered. In our traffic, about 90% of questions are straightforward enough for the fast query path, which leaves the slower, more expensive conversational path for the cases that genuinely need it.


### **Why we're telling you this:**


"Supports MCP" is one label for two very different philosophies. One moves your data to the answer every time you ask, whether by dumping it in a model or loading it into a scratch database, so you pay in latency, staleness, and cost on every question. The other does the heavy lifting ahead of time, so the answer is already waiting: exact, current, permission-scoped. Same words on the RFP. Completely different product, and a completely different user experience.


Building the wrapper version is easy, which is why it's everywhere. Building the version that's fast, exact, and permission-safe at scale is the hard part, and it's the part a comparison chart can't show you. It's also the part that matters most when the data is your money.


When you evaluate AI for finance, don't ask whether a vendor supports MCP. Ask:


- What happens when your controller asks about 100,000 expenses?
- What does the math, the model or a database?
- How do permissions hold up when the agent can query anything?


The answers will tell you which MCPs were built to work, and which were built to check a box.
