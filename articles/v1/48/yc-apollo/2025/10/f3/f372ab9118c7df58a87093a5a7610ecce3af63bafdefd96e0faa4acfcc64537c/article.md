---
schema_version: "1.0.0"
document_id: "f372ab9118c7df58a87093a5a7610ecce3af63bafdefd96e0faa4acfcc64537c"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/announcing-apollo-ios-2-0"
published_at: "2025-10-07T07:00:46+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:bc41425b53c8485225e0ab125e21791f14f0bf7a232a81c3cc82ab8bf042e5d7"
---

# Announcing Apollo iOS 2.0

Apollo iOS 2.0 is here, and it’s the biggest reimagining of our Swift client yet. Built from the ground up for Swift’s modern concurrency model, this release delivers snappier performance, stronger type guarantees, and a cleaner developer experience for teams shipping GraphQL-powered apps across Apple platforms.


## Improved APIs


Swift’s structured concurrency and the` async` /` await` syntax weren’t just sugar, they introduced an entirely new mental model for reasoning about work, isolation, and cancellation. With Swift 6 enforcing strict concurrency checks today, you need APIs that prove data is safe as it crosses actors and tasks. Apollo iOS 2.0 leans into that world: the client surface is` Sendable` -aware, network requests naturally compose with tasks, and the compiler can flag data races before they ship. Instead of juggling completion handlers and manual thread hops, your GraphQL workflows read like straightforward, sequential Swift while still benefiting from the runtime’s scheduling and cancellation semantics.


Current Apollo iOS users will notice the difference at the call site immediately. A familiar completion-handler fetch looks like this:


```text
apollo.fetch(query: HeroQuery()) { result in
switch result {
case .success(let graphQLResult):
updateUI(with: graphQLResult.data)
case .failure(let error):
handle(error)
}
}
```


In Apollo iOS 2.0, the same work becomes a single` async` expression that fits naturally inside your task flow:


```text
let response = try await apollo.fetch(query: HeroQuery())
updateUI(with: response.data)
```


The benefits extend beyond ergonomics.` ApolloClient` ,` ApolloStore` , request and response models, interceptors, and cache policies all conform to` Sendable` , so they can pass safely across concurrency boundaries. The APIs themselves are streamlined, with clearer separation between transport, cache, and request layers. Once you’ve adjusted to the concurrency-first idioms, your codebase is better prepared for the evolution of Apple’s platforms and the reliability expectations that come with them.


## Modernizations, deprecations, and WebSockets


Before you upgrade, there are a few logistical checkpoints. Deployment targets move to iOS 15, macOS 12, tvOS 15, watchOS 8, and visionOS 1. CocoaPods support is retired, so Swift Package Manager and prebuilt XCFrameworks are the supported distribution paths. If you’re using GraphQL subscriptions, note that 2.0 supports HTTP-based subscriptions only for now, but an updated WebSocket implementation is our next priority and will be released in an upcoming minor version. If your app depends on subscriptions over web sockets, you will need to hold off on upgrading until then.


## Migration path


We’ve structured the migration to de-risk the journey. Start with the breaking pieces: custom interceptors, network transports, cache implementations, and request/response models all need to adopt the new structured APIs. Once that foundation is in place, you can modernize the rest of your client code gradually. The deprecated 1.x calls now conform to` Sendable` , so your app continues to compile while you migrate queries, mutations, and subscriptions at whatever pace your release schedule allows.


## Upgrade today


Grab Apollo iOS 2.0 via SPM, update your codegen configuration, and start converting call sites to` async` /` await` . The migration guide at[https://www.apollographql.com/docs/ios/v2/migrations/2.0](https://www.apollographql.com/docs/ios/v2/migrations/2.0) walks through every change in detail, with practical examples and notes to keep you unblocked. We can’t wait to see the apps you build on top of this new foundation! Let us know what you ship and how we can keep smoothing the path forward.


Written by


Anthony Miller


[Read more by Anthony Miller](https://www.apollographql.com/blog/author/amiller)
