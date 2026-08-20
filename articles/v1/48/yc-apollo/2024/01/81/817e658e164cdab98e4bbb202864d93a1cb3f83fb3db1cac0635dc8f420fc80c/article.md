---
schema_version: "1.0.0"
document_id: "817e658e164cdab98e4bbb202864d93a1cb3f83fb3db1cac0635dc8f420fc80c"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/whats-new-in-apollo-client-3-9"
published_at: "2024-01-31T12:27:45+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:16.511012+00:00"
content_hash: "sha256:b096174d12442758145c3cd1cde46137b557a7ae331a11c42191d1630702b6fd"
---

# What’s new in Apollo Client 3.9

With the release of Apollo Client 3.9, we’re excited to announce a host of new features and improvements, including:


- Suspense-enabled data fetching in response to user interaction with` useLoadableQuery`
- Preloading data outside of React with` createQueryPreloader` and` preloadQuery`
- Memory usage optimizations
- A new` useQueryRefHandlers` hook for accessing` refetch` and` fetchMore` for a given` queryRef`
- Multipart subscriptions network adapters for Relay and urql
- ` MockedProvider` improvements


Let’s dive in!


## Suspense-enabled data fetching in response to user interaction with` useLoadableQuery`


When it comes to Suspense-enabled data fetching in Apollo Client,` useSuspenseQuery` and` useBackgroundQuery` are useful for loading data as soon as the component calling the hook mounts. But what about loading a query in response to user interaction? For example, you may want to start loading some data when a user hovers on a link.


Introducing the` useLoadableQuery` hook 🎉 For more information, see[its documentation](https://www.apollographql.com/docs/react/data/suspense#fetching-in-response-to-user-interaction) .


## Preloading data outside of React with` createQueryPreloader` and` preloadQuery`


Starting with Apollo Client 3.9, queries can be initiated outside of React. This allows your app to begin fetching data *before* React renders your components, and can provide performance benefits.


To preload queries, you first need to create a preload function with` createQueryPreloader` .` createQueryPreloader` takes an` ApolloClient` instance as an argument and returns a function that, when called, initiates a network request.


The preload function returns a` queryRef` that is passed to[useReadQuery](https://www.apollographql.com/docs/react/api/react/hooks/#usereadquery) to read the query data and suspend the component while loading.` useReadQuery` will ensure that your component is kept up-to-date with cache updates for the preloaded query.


This feature pairs nicely with popular routers such as React Router and TanStack Router, both of which provide APIs for loading data before route components render (such as the[loader function](https://reactrouter.com/en/main/route/route#loader) from React Router). It provides the benefits of earlier fetching without sacrificing the ability to automatically re-render when future cache updates occur.


This feature is in alpha in 3.9 and may be subject to change before 3.10. If you’d like to provide feedback before it’s stabilized, please visit[#11519](https://github.com/apollographql/apollo-client/issues/11519) and add a comment.


For more information, see[its documentation](https://www.apollographql.com/docs/react/data/suspense#initiating-queries-outside-react) .


## Introducing a new` useQueryRefHandlers` hook


` useQueryRefHandlers` is a hook that returns` refetch` and` fetchMore` functions for a given` queryRef` . It’s useful for gaining access to these handlers for a` queryRef` that was created by` createQueryPreloader` , or to avoid passing these handlers down through your component tree.


For more information, see[its documentation](https://www.apollographql.com/docs/react/api/react/hooks/#usequeryrefhandlers) .


## Memory usage optimizations


Spanning over twenty memory-related pull requests, 3.9 includes a host of improvements and optimizations related to memory usage.


You can find a comprehensive list in[Lenz](https://www.apollographql.com/blog/author/lenz) ‘s December[blog post](https://www.apollographql.com/blog/apollo-3-9-beta-feature-spotlight-the-memory-story) , but at a high level this work includes many fixes and improvements and introduces **the ability to granularly configure Apollo Client’s internal memoization** .


Memoization is always a trade-off between processing time and memory, and some of our old cache limits erred on the side of consuming more memory. While the new defaults should be sufficient for most applications, we know that some will need to make different trade-offs so we made each cache size individually configurable, both before and after the Apollo Client has been initialized.


For more information, see our new[documentation page for memory management](https://www.apollographql.com/docs/react/caching/memory-management/) and share your` ApolloClient.getMemoryInternals()` values[in this issue](https://github.com/apollographql/apollo-client/issues/11444) !


## Multipart subscriptions network adapters for Relay and urql


With the launch of[federated subscriptions](https://www.youtube.com/watch?v=-4R6rLMZ9mc&list=PLpi1lPB6opQzUOqG3QroLLN06FF-Q_uhX&index=5) in the Apollo Router last year, Apollo Client added support for GraphQL[subscriptions over HTTP](https://www.apollographql.com/docs/react/data/subscriptions/#subscriptions-via-multipart-http) .


In 3.9, we’re shipping network adapters for Relay and urql to make consuming subscriptions via multipart HTTP a breeze no matter which client library you’re using. For more information, see[the documentation](https://www.apollographql.com/docs/react/data/subscriptions/#http) .


## ` MockedProvider` improvements


This release also introduces two frequently requested improvements to the testing utility,` MockedProvider` :


- **Reusing mocks:** By default, a mock is only used once. If you want to reuse a mock for multiple operations, you can now set the` maxUsageCount` field to a number indicating how many times the mock should be used.
- **Dynamic variables:** The` MockedResponse` object now takes a` variableMatcher` property that is a function that takes the variables and returns a boolean indication if this mock should match the invocation for the provided query. This can be useful when the exact value of the variables being passed are not known, or for asserting specific variables individually.


For more on these enhancements, see our complete documentation on[testing React components](https://www.apollographql.com/docs/react/development-testing/testing/#testing-the-loading-and-success-states) . Finally, to keep an eye on future testing improvements slated for 3.10, this[GitHub issue](https://github.com/apollographql/apollo-client/issues/9738) is the one to watch.


You can browse the[full changelog](https://github.com/apollographql/apollo-client/releases/tag/v3.9.0) on GitHub. Happy querying!


Written by


Alessia Bellisario


[Read more by Alessia Bellisario](https://www.apollographql.com/blog/author/alessia)
