---
schema_version: "1.0.0"
document_id: "d5e649904e170e03ec33ce0640ce42f288a38c769f6706ad4df949ecab5fc1a4"
company_key: "yc-retool"
company: "Retool"
source_id: "yc-retool-news-import-762698831de2"
canonical_url: "https://retool.com/blog/building-custom-apps-without-frontend-experience"
published_at: "2026-01-14T00:00:00+00:00"
first_seen_at: "2026-07-22T11:44:05.217948+00:00"
fetched_at: "2026-07-28T21:27:02.153272+00:00"
content_hash: "sha256:384a4130592f09cdb5cc10dd9c4c3074c2e3d12970ec05886ee45a4aeb295ef1"
---

# How a data analyst built a self-serve customer insights app

01 Why I built a custom app from scratch02 What I shipped (at a glance)03 What building in Retool felt like to build as a non-frontend dev04 Start with the question, not a framework05 Design with real data in the loop06 Ship the 80% that matters, then iterate07 Iterate in production-adjacent reality08 A quick note to engineers, security, and IT09 The cultural shift (and outcomes we saw)10 A reusable pattern beyond Gong11 If you want to try this


When you live in a workflow every day, you don’t need a 20-page spec to know what’s missing. You feel the friction. The fastest way I’ve found to remove it is to let the person closest to the problem assemble the first version of the solution—and give engineering all the guardrails to keep it safe.


That’s what I did with our *Gong Super App* in Retool (more details on that in a minute).


I sit on the[data team](https://retool.com/for/data-teams) at Retool, so I’m far from a frontend developer. I spend most of my time in SQL, Python, and systems. But with Retool’s building blocks, I shipped a tool that our sales, success, and marketing teams actually use—without borrowing a sprint from product.


**Thesis:** Let business builders ship. Let engineering control the blast radius.


## Why I built a custom app from scratch


My team consistently fields a steady stream of questions like, “ *How often do customers mention \[topic/competitor\]?* ”, “ *Where did this objection come up last quarter?* ”, or “ *How are people describing us in their own words?* ” We had the data in[Gong](https://www.gong.io/) , but getting from question to answer involved a lot of context-switching and ad hoc work.


We didn’t need a new data pipeline. We needed a simple, trustworthy surface for searching, reviewing, and summarizing calls—ideally one that our go-to-market teams could use without us in the loop.


## What I shipped (at a glance)


The *Gong Super App* is straightforward on purpose. To make it usable quickly, all it needed to do was:


- **Search with context.** Pick a date range. Tap a preset for AI-related keywords or competitor names—these are just curated regex lists—and optionally flip “external only” so we only count what the customer said.
- **Review quickly.** See a table of matching calls with match counts. Filter across any column with AND/OR logic. Click a row to open a details drawer with region, opportunity type, participants and titles, and the transcript (which you can copy). Deep-link out to the Salesforce account or the call in Gong.
- **Make it actionable.** Select a handful of calls and hit “Summarize selected.” Behind the scenes, the app fetches metadata/transcripts and runs a prompt to extract use cases, unmet needs, objections, and competitors in the customer’s language. Or use “Ask AI” for ad-hoc questions across the selected set (e.g., “What do customers say about *competitor* ?” or “Which non-branded keywords do customers use to describe us?”).
- **Spot trends.** An analytics view shows mentions over time, percent of total calls, and opportunities by keyword group (e.g., competitors). Seeing a spike helps us prioritize enablement and messaging.


None of this required[frontend experience](https://retool.com/blog/frontend-dilemma) . It’s mostly wiring up components, writing queries to transform the data, and polishing the details that make an internal tool feel trustworthy.


## What building in Retool felt like to build as a non-frontend dev


I didn’t follow a traditional frontend workflow, and that turned out to be an advantage. Here’s what worked. Instead of writing React components and wrestling with build tools, I was dragging UI elements onto a canvas and wiring them to live queries. The feedback loop was instant—change something, see it update with real data, adjust based on what I learned. Here’s what worked:


## Start with the question, not a framework


My starter prompt to myself was: “How can sales/CS quickly find where customers mention X—and what should we do about it?” That shaped the UI, helping me stay focused on exactly what end users needed.


## Design with real data in the loop


With Retool, you drop inputs and tables onto a canvas and wire them to real data immediately. You’re building with actual Gong calls from the start, not mockups. When I noticed people only cared about what customers said (not our reps), for example, I added an “external only” toggle. Identifying these kinds of patterns was easier when working with live data.


## Ship the 80% that matters, then iterate


I didn’t write CSS. I used building blocks and focused on tiny quality signals. A loading skeleton instead of a blank screen. Right-aligned numerics for match counts. Monospaced timestamp chips you can copy. Small polish goes a long way.


## Iterate in production-adjacent reality


I tested with real users early and often. When someone got confused by the interface, I’d fix it immediately—whether that was better column names, clearer empty states, or keyboard shortcuts. That tight loop is what made the difference between something that worked in theory and something people actually used.


## A quick note to engineers, security, and IT


When you talk about letting non-devs build production apps, the immediate concern is security and governance. It sounds like a risk. But giving non-devs the keys doesn’t mean giving up control. Retool provides the guardrails that kept everyone comfortable:


- **SSO + RBAC.** Apps are gated behind SSO. Groups map to roles—most users are read-only; a tiny set can edit. Row-level permissions are available if you need team or region scoping.
- **Least-privilege resources.** Resources use service accounts with scoped permissions. Tokens live in Retool secrets—never exposed to the client.
- **Source control.** Track and manage changes with version control. Review and approve changes before they go live.
- **Auditing.** Every query, run, and change is logged. You can set up alerts for unusual usage patterns if needed.
- **Encryption.** Data is encrypted at rest and in transit using industry-standard protocols.


Once engineers and security configure these controls, they apply to every app you build, so everyone can build safely within those boundaries. These aren’t heroic processes—they’re boring, repeatable guardrails that make it easy to say “yes.”


## The cultural shift (and outcomes we saw)


The impact of this app wasn’t just about saving time—it changed how teams work, and made it easier for more teams to get closer to our customers. Instead of asking for help, or just dropping it altogether, they were sharing what they’d already found.


**Fewer interrupts.** Fewer “can you check X?” is always a win for a data analyst, and we certainly got that here. People can now self-serve and paste summaries into Slack instead of waiting on our team to run queries.


**Faster feedback loops.** Sales and CS teams answer questions in the moment with data instead of anecdotes. When someone asks “What did customers say about that feature?” during a standup, they can pull it up right there.


**Sharper coaching and messaging.** Summaries cluster the objections and unmet needs in the customer’s words, which makes enablement sessions more concrete. Instead of “customers have concerns,” teams are able to say “here are the three specific objections from last week’s calls.”


**Reusable pattern.** Once folks saw *filter → review → summarize → share* work here, they asked for the same flow on support tickets and product feedback. The pattern proved its value.


## A reusable pattern beyond Gong


This app gave us something we didn’t have before: a way to surface qualitative insights from a huge amount of unstructured data. Instead of reading through hundreds of calls to spot patterns, we could search, filter, and summarize across everything at once.


Now we can take that capability and replicate it across data sources and disciplines. The skeleton I built for Gong calls works for any source that captures customer voice or operational signals.


For example, we could swap Gong for:


- **Support:** Mine Intercom or Zendesk tickets for post-release themes. Summarize escalations weekly. Find patterns in how customers describe problems versus how your docs describe solutions.
- **Product feedback:** Search Notion or docs for feature requests. Group them by persona and impact. Instead of a pile of “it would be nice if” comments, surface what different user segments actually need.
- **Revenue:** Scan contracts for compliance terms. Summarize red-flag clauses for legal review. When your team asks “how often do customers negotiate X?” you’ll have an answer.
- **Quality:** Look for “bug” or “crash” language in feedback. Open deep links to issue trackers. Spot patterns before they become trends that show up in churn data.


The skeleton is always the same: *presets → filters → table → summarize selected → ask ad-hoc → trends* . Once you build it once, you can adapt it to any text corpus you need to search and understand.


## If you want to try this


You don’t need a comprehensive guide to start—just a clear question and a willingness to iterate. The steps below work whether you’re searching Gong calls, support tickets, or product feedback. Here's the 30-minute version I'd recommend to any analyst or ops lead:


- **Pick one weekly question your team asks** (e.g., “Which competitors did customers mention this month?”).
- **Define the smallest output that answers it.** A table with a match count and a deep link. A one-page summary grouped by use case/objection. Keep it minimal—you’ll add more once you see what people actually use.
- **Prototype with real data behind SSO for a small pilot group.** Five users who are willing to work through rough edges are worth more than a polished demo for fifty. They’ll tell you what’s actually missing.
- **Add one piece of polish based on the first feedback you get.** Not ten features—one thing that makes the experience feel more complete. Maybe it’s a loading state. Maybe it’s better column names. Listen for what confuses people and fix that first.
- **Deploy with confidence.** With security guardrails already built in, you’re building within a secure environment from day one, not retrofitting security later.


You’ll learn more in an hour of clicking than a week of requirements. And once people see it working, they’ll help you make it better.


This didn’t happen because I wrote a better spec. It happened because I was close to the problem and had the keys to ship, while engineering gave me guardrails that made “yes” the safe answer. Once you see that combination work, it’s hard to go back.


**[Try it today](https://login.retool.com/auth/signup?source=blog) .** With Retool’s[AppGen capabilities](https://retool.com/blog/introducing-enterprise-appgen) , building custom apps to solve your business problems is easier than ever.


Pick one high-friction question your team asks every week. Build the smallest surface that answers it. Ship in days, improve all quarter.


light Reader
