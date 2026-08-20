---
schema_version: "1.0.0"
document_id: "d38cd1897403dce3b226d328af88c6c693e0da04e6b04766b43d534f37ade789"
company_key: "intapp-inc-common-stock"
company: "Intapp Inc."
source_id: "intapp-inc-common-stock-rss-4b1e24a9890b"
canonical_url: "https://www.intapp.com/blog/ai-governance-law-firms-buy-vs-build/"
published_at: "2026-08-06T13:00:00+00:00"
first_seen_at: "2026-08-07T13:15:36.052382+00:00"
fetched_at: "2026-08-07T13:15:37.573732+00:00"
content_hash: "sha256:1146e3555daaf97b7b593debb93e3133e46cff902f66c7b787c2a7a1b80213bc"
---

# Why building your own AI governance is the wrong bet

When AI tools started arriving in law firms, a reasonable question followed: do we need to buy a governance layer, or can we build one ourselves? Firms with strong IT teams looked at the API documentation for Harvey, Copilot, and others and concluded the integration work was manageable. For some, it still looks that way.


### The first integration is the easy part


A capable development team can write a connector that checks wall status before allowing access to a document. That’s a defined project with a beginning and an end. The lifecycle that follows is neither.


AI vendors ship on their own schedules, and APIs change with every major release. When that happens, a custom connector doesn’t send an alert. It just stops working, or works incorrectly, while the governance gap it was supposed to close quietly reopens. Your lateral screens held in your DMS. They didn’t hold in your AI tools, because nobody noticed the integration broke two releases ago. That’s not a hypothetical failure mode. It’s the ordinary consequence of maintaining bespoke integrations against software you don’t control.


Now add the second AI tool. And the third. Each new platform your firm adopts requires its own integration project, its own maintenance calendar, and its own failure surface. The IT team that built the first connector is now being asked to maintain four, keep pace with each vendor’s release cycle, and still keep everything else running.


### What you’re actually building


Governance for AI at a law firm isn’t a technical problem. It’s a legal compliance problem that requires technical execution. The policies your firm enforces — lateral hire screens, matter-level confidentiality, waiver-driven walls, MNPI controls — encode obligations that your compliance team owns and your GC has to certify. Translating them correctly into integration logic requires compliance expertise alongside the engineering work.


Consider what happens when a lateral joins from opposing counsel. Your compliance team updates the wall in your core system. With a custom integration, that change has to propagate correctly to Harvey, to Copilot, to every other AI tool the firm is running — each through a separate connector your IT team maintains. If one breaks, or lags, or was never built in the first place, a lawyer pulls up an AI workspace and sees documents she shouldn’t. The DMS screen held. The information still moved. And when the client asks for documentation of what your governance program actually covered, the answer is a custom script your IT team wrote eighteen months ago and has patched three times since.


### The maintenance burden compounds


The firms that have stayed in build mode consistently underestimate one cost above all others: the cost of staying current. Not just with API changes, but with their own policy changes. Every matter, every lateral, every new AI tool on the firm’s roadmap adds to the maintenance burden of a custom integration stack. That burden compounds while the AI footprint grows.


### What gets stuck in pilots


The most concrete cost of the build path isn’t the engineering hours. It’s the deployments that never scale because governance wasn’t in place to support them. The GC who won’t sign off on firm-wide AI deployment isn’t being obstructionist. She needs a governance layer she can certify to clients and regulators — one with audit logs, policy documentation, and a defensible answer to how ethical walls are enforced across every AI tool the firm runs. A custom-built integration your IT team maintains isn’t that layer. Every week a firm runs AI without certifiable governance, it’s accumulating access patterns that will have to be addressed retroactively. The pilot that should have become a firm-wide deployment stays a pilot.


### The real comparison


When firms weigh the build option, they typically compare the cost of Intapp Walls against the cost of the first integration build. The right comparison is total cost of ownership: integration builds across every AI tool the firm deploys, maintenance for every API change, compliance expertise to encode policies correctly, and audit infrastructure to prove it’s working.


Intapp Walls for AI was built to carry this weight in production, across hundreds of firms, against the same AI tools and the same compliance obligations your firm runs. When a vendor ships a new release, the integration is already updated. When your firm adds a new tool, your existing policies extend to it through the same layer, without a rebuild. Your compliance team sets policy once. The GC has audit logs that hold up under client scrutiny, regulatory review, and the partner-room conversation that follows a near-miss. The Innovation Officer can take an AI pilot firm-wide without a compliance fire drill in between.


Firm AI runs on a foundation of governed access. Walls is that foundation — the layer that makes firm-wide AI deployment certifiable, and keeps it that way as the tool stack grows. See how Intapp Walls for AI governs[Harvey](https://www.intapp.com/harvey-agentic-ai-partnership/) and every AI tool your firm deploys.


[Schedule a demo](https://www.intapp.com/schedule-demo/)
