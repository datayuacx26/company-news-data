---
schema_version: "1.0.0"
document_id: "267c78e11c2fe6720785452ef4a8521ad217176289e547a90a7277dfaf09a13d"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/whats-new-in-apollo-client-4-2"
published_at: "2026-05-27T11:33:11+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:b821668d9036f3152b42439aeb32c7c5a7e9cbeda85f3afdb40427325af28620"
---

# What’s New in Apollo Client 4.2

We’re excited to announce the release of Apollo Client 4.2. This release brings two additional long-awaited features:


- Type-safe default options
- Event-based refetching


Let’s dive in!


## Type-safe default options


Prior to version 4.2, you had to choose between convenience and type-safety when you wanted to propagate a default option throughout your application. For example, you might set your default` errorPolicy` to` "all"` in order to render partially successful queries in your components.


This could introduce a mismatch between the runtime behavior and what TypeScript reports as the right value. Consider this` useSuspenseQuery` example:


```text
const { data } = useSuspenseQuery(QUERY);
//      ^? TData


const { data } = useSuspenseQuery(QUERY, { errorPolicy: "all" });
//      ^? TData | undefined
```


With no explicit` errorPolicy` ,` useSuspenseQuery` ‘s throws by default when an error is returned, which means \`data\` can be typed as the query data type. Passing` errorPolicy: "all"` changes the return type to include` undefined` because the server might return an error with no data.


Changing the default` errorPolicy` in` defaultOptions` was considered unsafe however because it modified the runtime behavior, but the types remained the same. This could cause crashes in your production environment that weren’t caught by TypeScript because you might access properties on` undefined` .


In 4.2, default options are now propagated through all React hooks and APIs to provide the correct type to match the runtime value. You opt in by declaring your default options using TypeScript module augmentation:


```text
// apollo.d.ts
import "@apollo/client";


declare module "@apollo/client" {
namespace ApolloClient {
namespace DeclareDefaultOptions {
interface WatchQuery {
errorPolicy: "all";
}
}
}
}
```


To make sure the runtime behavior matches the types, Apollo Client forces you to add a matching` defaultOptions` option:


```text
new ApolloClient({
// without this option, TypeScript reports an error
defaultOptions: {
watchQuery: {
errorPolicy: "all"
}
}
})
```


With that TypeScript declaration in place, the hook now reflects the runtime value:


```text
const { data } = useSuspenseQuery(QUERY);
//      ^? TData | undefined
```


This behavior extends to all query and mutation hooks and core APIs.


Learn more about declaring type-safe default options in the[TypeScript guide](https://www.apollographql.com/docs/react/data/typescript#declaring-default-options-types) .


### Deprecation of generic arguments


To achieve type-safe default options, Apollo Client requires the use of type inference. As a result, passing generic arguments to hooks and core APIs is now deprecated.


```text
// Generic arguments are no deprecated
useQuery<DataType, VariablesType>(QUERY)
```


You can still use this signature, but you won’t be able to take advantage of the new type-safety. Migrate to` TypedDocumentNode` instead:


```text
const QUERY: TypedDocumentNode<DataType, VariablesType> = gql``
```


Learn more about migrating in the[migration guide](https://www.apollographql.com/docs/react/migrating/apollo-client-4-migration#type-safety-for-default-options) .


## Event-based refetching


One of our most popular requests has been window focus refetching, a feature popularized by TanStack Query that triggers automatic refetches when the browser tab regains focus. Building your own system for handling automatic refetches resulted in a[complicated mess](https://github.com/apollographql/apollo-feature-requests/issues/247#issuecomment-1650221163) of` useEffect` or wrapper hooks to provide this sort of functionality yourself.


4.2 introduces the new` RefetchEventManager` , which handles refetches for you in response to events such as window focus or network reconnection. Pass a` RefetchEventManager` instance to the` refetchEventManager` option to opt-into automatic refetches:


```text
import { RefetchEventManager, windowFocusSource } from "@apollo/client";


const client = new ApolloClient({
// ...
refetchEventManager: new RefetchEventManager({
sources: {
windowFocus: windowFocusSource,
},
}),
});
```


Anytime a user focuses the browser tab, the client automatically refetches active queries.


Queries can also opt-out of a specific event refetch with the new` refetchOn` option:


```text
useQuery(QUERY, {
// Don't refetch this query when the windowFocus event is triggered
refetchOn: { windowFocus: false }
});
```


` RefetchEventManager` is designed for extensibility in mind. You can register your own custom events, provide customized handlers to determine which queries should be refetched, and more.


See the[event-based refetching docs](https://www.apollographql.com/docs/react/data/event-based-refetching) to learn more.


## Wrapping up


Ready to upgrade? Install Apollo Client 4.2 today:


```text
npm install @apollo/client@latest
```


For the full list of changes, check out the[release notes](https://github.com/apollographql/apollo-client/releases/tag/%40apollo%2Fclient%404.2.0) . Questions and feedback are always welcome in the[Apollo Community](https://community.apollographql.com/) .


Happy querying!


Written by


Jerel Miller


[Read more by Jerel Miller](https://www.apollographql.com/blog/author/jerel)
