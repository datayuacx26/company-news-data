---
schema_version: "1.0.0"
document_id: "7c3de64c3da4e66a4aebee041d7f0c664d71fff25ca75e49eb7b4d9f6be02973"
company_key: "cargurus-inc-class-a-common-stock"
company: "CarGurus Inc."
source_id: "cargurus-inc-class-a-common-stock-rss-ea326c83890f"
canonical_url: "https://cargurus.dev/2026/05/22/hexagonal-architecture-with-remix-2-now-react-router-framework/"
published_at: "2026-05-22T14:24:49+00:00"
first_seen_at: "2026-07-20T03:33:13.583792+00:00"
fetched_at: "2026-07-28T21:55:52.188627+00:00"
content_hash: "sha256:e465b3ecc327e904026a06228a8cbd757085a5792554cd5c114ac9dec11e6b4a"
---

# Hexagonal Architecture with Remix 2 (now React Router Framework)

If you are a software engineer perhaps one of the situations below sounds familiar.


You’ve just finished a small change to a landing page, tweaked some layout, added a new field to your article section. You run the tests… and suddenly something deep in your CMS integration is failing.


Or maybe you want to reuse a bit of business logic in another Remix route, but the function you need is buried inside a loader with HTTP-specific code you can’t untangle.


Worse, a business partner excitedly tells you they’ve finally signed the contract for that great new CMS vendor everyone’s been raving about. They want to know if it can launch sometime next quarter. Your stomach drops because you know that means touching dozens of files scattered across your app.


These are all symptoms of the same problem: your core business logic is tangled up with the messy details of HTTP, databases, and third-party APIs.[Hexagonal architecture](https://en.wikipedia.org/wiki/Hexagonal_architecture_(software)) (also called ports and adapters) gives you a way out. It separates your application’s engine from its bodywork, so you can upgrade one without touching the other.


*Note: This post references Remix, which now continues as React Router Framework. The patterns shown here work with both Remix 2 and React Router Framework.*


## Principles of Hexagonal Architecture


The headaches in the intro all have the same root cause: your application’s thinking is mixed in with its doing. Your business logic, the rules and decisions that make your app valuable, is tangled up with the code that talks to HTTP, databases, and third-party APIs. Change one, and you risk breaking the other.


Hexagonal architecture solves this by moving your application’s “brain” to the center, surrounded by a clear boundary of ports. Everything outside that boundary, databases, APIs, the browsers, connects through adapters. Think of the ports as clearly marked doorways into your app’s core, and the adapters as the translators who stand outside, speaking the language of the outside world but passing through only what the core actually cares about.


Let’s see this in action.


At CarGurus, our[Sell My Car landing page](https://www.cargurus.com/sell-car) needs to show recent, relevant content for users. If the CMS is slow or down, we still want the page to load quickly with fallback articles. That requirement led us to create a service function:


**A note on the code:** The examples throughout this post are simplified to highlight architectural patterns and are not direct excerpts from our production codebase.


Here’s where the ports and adapters idea comes in:


- ` fetchSellArticles` is a *port* into the application core. Other parts of the system, like a Remix[loader](https://remix.run/docs/en/main/route/loader) , call it when they need to fetch articles.
- ` getArticles` is another *port* , used by the core to talk to the CMS. Behind it sits an adapter that knows all the messy details of the CMS API.
- ` captureException` is a *port* to our error tracking service.


In this setup, the application core only knows about its own domain objects (` Article` ) and rules (use recent articles, fall back if needed). It doesn’t know about where the articles come from, what protocol they uses, or how to authenticate, that’s the adapters’ job. This enables easy reuse of the core logic in different contexts because we avoid direct coupling to the shape of a specific CMS. These properties also simplify unit testing. Let’s look at the tests now.


In Hexagonal architecture, the application core is a safe place for your business rules, protected from the churn of frameworks, protocols, and vendor APIs. Everything messy is kept outside, where adapters can handle it without contaminating your core. This makes it easy to test business rules in your application core without touching a network or database.


## **Adapters: translating between your core and the outside world**


Ports give your application core a clean, stable surface to work with, but ports don’t deal with the messy reality of interfacing with the outside world on their own. That’s where adapters come in.


Adapters live on the other side of your ports. They are the concrete implementation of the port’s interface. Their only job is to translate between your domain model and whatever shape, protocol, or authentication dance the outside world expects. They take a request from the core, make it understandable to the outside system, and return a result the core can use without knowing how it was produced.


This adapter takes care of:


- Building a CMS-specific request payload
- Handling authentication
- Unwrapping the CMS’s nested response format into a plain` Article\[\]`


From the perspective of the application core, none of that exists. It just calls` getArticles` with a simple config expressed in terms the business cares about, things like` tag` and` orderBy` , and gets back` Article` objects. The port’s interface speaks the language of the core’s domain and hides details specific to the CMS.


### **Why isolate adapters?**


Even if your CMS supports powerful query expressions, resist the urge to expose them through the port. The port should model business concepts. That keeps the application core decoupled and your tests simple; the adapter can translate domain intent into whatever syntax, payload, or authentication the CMS expects.


Keeping CMS logic in a single adapter means:


- If the CMS API changes, you update one file.
- If you switch vendors, you can rewrite the adapter without touching the core or your tests for` fetchSellArticles` .
- An API outage or schema change won’t break your business logic tests, only the adapter’s integration test might need attention.


In other words, the adapter acts as an anti-corruption layer: it absorbs the quirks, inconsistencies, and churn of external systems so your core stays clean, stable, and focused on the work that matters to the business.


### **Testing adapters**


Since adapters are the only code that knows about external systems, they’re also the only place where we need slower integration tests. Here we can integrate directly with the CMS system to ensure our systems are working as expected.


Without business logic our adapter is fairly simple. It doesn’t contain any conditional logic in its behavior. This means we can get away with just a single test to ensure its working as expected.


From the perspective of the application core, a CMS adapter is just one kind of translator, but it’s not the only one your Remix app needs. Every time your app talks to the outside world, whether it’s through HTTP, WebSockets, a queue, or a browser API, there’s an adapter doing the translation.


That includes Remix itself.


## **Remix as an Adapter**


In a hexagonal architecture, Remix’s loaders and actions are simply another kind of adapter, no different in principle from your CMS or error-tracking adapters. The only difference is what they translate: instead of converting between your domain and a vendor API, they convert between your domain and HTTP itself.


Here’s what that looks like in practice:


In the code above, the loader unwraps the HTTP request, validates the region parameter, and calls the` fetchSellArticles` port in the core. It then wraps the resulting list of` recentArticles` back into a JSON HTTP response.


The important thing: the core has no idea this request came from Remix, and Remix doesn’t know anything about how` fetchSellArticles` actually gets its data.


### **Testing the Remix Adapte** r


Testing this adapter is fairly straightforward using the same outside-in approach we used for the application core.


Just like with the CMS adapter, we mock the port (` fetchSellArticles` ) so we’re testing only this adapter’s behavior. The pattern is the same: test the translation layer in isolation, not the layers on either side. TypeScript guarantees the` fetchSellArticles` interface stays in sync and alerts us to any changes in the contract that might break our test or production code.


Since we have a conditional in this adapter we need 2 tests. The first test ensures the code flows correctly in the happy path and the second test ensures the loader adapter returns a 404 for invalid data. The 404 is an HTTP concern, so it lives in the HTTP adapter. Business rules stay in the application core and protocol rules stay at the edge.


### **Seeing the pattern**


By now, you’ve seen this boundary in action twice:


- When the outside world calls in (a Remix loader, a webhook), the adapter unwraps the request, passes a clean domain value into the core through a port, and wraps the core’s response back into the external format.
- When the core calls out (fetching from a CMS, sending an email), the adapter takes the domain request, translates it for the external system, and converts the response back into the core’s domain model.


Same rules in both directions. That’s the beauty of hexagonal architecture, the boundaries and responsibilities never change, which makes the system predictable, testable, and much easier to evolve.


### **Pro Tip: Don’t let HTTP envelopes leak into your application core**


One of the easiest ways to erode your adapter boundary in Remix is by passing raw` Request` or` FormData` objects straight into the application core. It’s tempting, they already hold the data you need, but this couples your business logic to HTTP and blinds your type checker.


From TypeScript’s perspective,` Request` and` FormData` are opaque containers. The compiler can’t tell what’s inside them, so it can’t help you catch missing fields, invalid formats, or typos in parameter names until runtime. Every time the application core reaches into one of these envelopes, you lose the static guarantees you worked so hard to get.


You may be building a web app, but your application core doesn’t need to be coupled directly to HTTP. That coupling limits reuse in CLI scripts or background jobs, and it forces you to construct HTTP objects just to run business logic unit tests.


The fix is simple:


- Unwrap and validate HTTP data in the Remix adapter.
- Convert it into explicit domain types (` Region` ,` ArticleQuery` ,` UserId` , etc.).
- Pass those domain types through your ports into the application core.


If your application core needs a` Region` , it should receive a` Region` , not a` Request` it has to dig through. This keeps HTTP concerns out of the core, keeps your ports clean, and lets TypeScript fully enforce correctness across the boundary.


## **Bringing it all together**


Those brittle tests, tangled business logic, and stomach-dropping vendor changes from the start of this post?
Hexagonal architecture is how you prevent them from taking over your life as a Remix developer.


By putting your **application core** , the rules and decisions your business cares about, in the center, and surrounding it with clearly defined **ports** , you create a safe, stable space for the logic that matters most. By pushing all framework, protocol, and vendor-specific code into **adapters** at the edges, you keep those details from leaking inward and complicating your core.


Whether the call flows into the application core (a Remix loader, a webhook) or out of it (fetching from a CMS, sending an email), the pattern is the same: unwrap external details at the edge, work in your domain language in the center, then wrap results back for the outside world.


Once you start seeing Remix loaders and actions as just another kind of adapter, you’ll stop worrying about whether a change will ripple unpredictably through your app. You’ll know exactly where to look, what to change, and what to test.


### Share this:


- [Share on X (Opens in new window) X](https://cargurus.dev/2026/05/22/hexagonal-architecture-with-remix-2-now-react-router-framework/?share=twitter)
- [Share on Facebook (Opens in new window) Facebook](https://cargurus.dev/2026/05/22/hexagonal-architecture-with-remix-2-now-react-router-framework/?share=facebook)
-


### Like this:


Like


Loading…
