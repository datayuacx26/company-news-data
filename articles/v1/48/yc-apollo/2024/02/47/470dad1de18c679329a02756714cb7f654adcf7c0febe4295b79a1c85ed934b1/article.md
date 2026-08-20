---
schema_version: "1.0.0"
document_id: "470dad1de18c679329a02756714cb7f654adcf7c0febe4295b79a1c85ed934b1"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/graphql-is-for-backend-engineers"
published_at: "2024-02-05T15:35:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:16.511012+00:00"
content_hash: "sha256:d0f5f9257cda40da3f31e90f725641e840a60654c270995656d737c59a1f3691"
---

# GraphQL is for Backend Engineers

Most articles explaining the benefits of GraphQL focus on advantages for the frontend: things like preventing overfetching, reducing round trips, and iterating faster. But GraphQL provides just as many advantages for backend developers, which is why I choose it by default for new APIs and why you should consider it, too.


## Improved communication


The goal of building any API is to[enable someone to use it](https://www.postman.com/state-of-api/api-first-strategies/#measuring-success-of-public-apis) , which requires excellent communication between API producers and consumers. However, the state of the art for REST APIs doesn’t quite live up to this expectation.


On the backend, developers either need to manually document the entire API or rely on auto-generation tools that don’t fully meet their needs. Consumers face the same choice, write code by hand or workaround the bugs in their SDK generator (stated, lovingly, as the maintainer of[an OpenAPI client generator](https://github.com/openapi-generators/openapi-python-client) ). On top of this, these solutions result in inconsistent understandings of the API. Reproducing errors becomes time-consuming and frustrating, which feels like a battle instead of a collaboration. What we need is a shared language to describe how the API works—one that doesn’t add unnecessary layers of abstraction or manual work.


Meet GraphQL, where every team uses the same schema definition language (SDL) to talk about the API. SDL is designed for developers, omitting extraneous details while describing all the essential bits. This serves both as your documentation and a universal language to communicate between teams.


On the backend, developers can write code in their language of choice and have the SDL created for them. Alternatively, they can write the SDL by hand, and generate the required code. Either way, the documentation is always kept in sync with the implementation.


Consumers use this same SDL directly, so there’s no need to maintain a separate documentation website. Instead, they can get information, auto-complete names, and see potential mistakes right in their IDEs. The same request they write in their code can be run by itself for easier debugging, which makes sending snippets to the backend team much more straightforward. With clearer communication, there will be fewer open issues and less wasted time tracking down minor bugs.


## Less bikeshedding


Bikeshedding is when you spend time discussing decisions that don’t have a significant impact, and it happens quite a bit with RESTful APIs. To name a few, *I* have been in bikeshedding meetings about:


1. Which verb to use?` PUT` and` POST` could have a lot of overlap. Maybe you *should* use` GET` , but the fact that it can’t contain a body is too limiting for the amount of filters you need.
2. Should this be a query parameter, path parameter, form data, or JSON body? For path parameters, how should we serialize arrays? What about objects?
3. Do we completely normalize endpoints to reduce duplication, even though that causes slower client performance? Or include the kitchen sink even though that causes slower individual endpoint performance? Maybe a compromise with shallow nested objects containing only the most common parameters or a parameter that allows selecting fields, emulating GraphQL without the type safety.
4. Which HTTP status code is the right one for each response?
5. Should relationships be indicated with IDs or URIs?


Hopefully, these discussions will lead to a comprehensive style guide, but the meetings will continue even with those guides. While there will always be *some* debate around things like naming, GraphQL eliminates entire categories of potential style issues (including all those above) by being *simpler* . You can spend less time on the little details and more time solving more interesting problems.


## Reduce tech debt


Imagine you have a` /v1/data` endpoint called by all your frontend teams: web, iOS, and Android. You decide that the date field should return UTC instead of localized time, so how do you make that breaking change? It usually looks something like this:


1. Create a new` /v2/data` endpoint.
2. Inform all the client teams that this new endpoint exists and that they should migrate by some date.
3. Track metrics on` /v1/data` and send reminders to consumers who haven’t updated.
4. The deprecation date comes and goes, and you’re still receiving calls to the old endpoint.
5. You either pull the plug and start breaking things or, more likely, continue to support the` /v1/data` endpoint indefinitely, creating tech debt with no apparent payoff date.


Now, let’s play through the same scenario with GraphQL. First, you add` @deprecated(reason: “use utcDate”)` to your old` date` field and add the new` utcDate` field. API consumers immediately start seeing warnings in their IDEs and potentially CI/CD about this deprecation since most GraphQL tooling supports it.


As it turns out, the iOS and Android teams weren’t even using the old` date` field, so no changes are necessary for them!


Someone on the web team notices some squiggly lines in their editor and decides to fix them, following the instructions left in the` reason` field. Your backend metrics tell you that the date field is no longer in use, and you’re safe to remove it!


Creating less churn and an easier upgrade path for clients makes them more likely to upgrade. If they upgrade faster, you can remove that stale old code faster, too!


## GraphQL is for you too


One of the main features of GraphQL is its developer experience. By creating a shared language between teams, reducing complexity, and providing a smoother upgrade path, GraphQL makes building and maintaining APIs more delightful. That’s why, even if your frontend engineers aren’t pushing for it yet, you should start evaluating GraphQL for your tech stack. If you have any questions or disagree with anything I’ve said above, let me know on our[Discord server](http://discord.gg/NUaGwceDhX) ! If you want to be notified of future posts, there’s also a channel for that.


Written by


Dylan Anthony


[Read more by Dylan Anthony](https://www.apollographql.com/blog/author/dylan)
