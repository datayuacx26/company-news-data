---
schema_version: "1.0.0"
document_id: "822ef1f060f8555eaa6945f72cfbf1f0133325028423a3fb42d7169cd322ec22"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/stream-all-the-things-with-apollo-client-4-1"
published_at: "2026-01-21T12:36:16+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:23:16.767603+00:00"
content_hash: "sha256:3eef87dc3b749770c5d25109e9c0b8781de609691d500a2a0c8431d4626e3937"
---

# @stream all the things with Apollo Client 4.1

Apollo Client 4.1 is here with significant updates to incremental delivery, fragment watching capabilities, and cache improvements. This release introduces full` @stream` directive support, compatibility with the latest GraphQL incremental delivery spec, and powerful new APIs for working with fragments.


## ` @stream` directive support


Apollo Client 4.1 introduces support for the` @stream` directive, allowing you to stream array items incrementally from your GraphQL server.` @stream` works with both the` Defer20220824Handler` and the newer` GraphQL17Alpha9Handler` .


The` @stream` directive is useful when fetching large lists where you want to display items as they become available rather than waiting for the entire response. As each item arrives, Apollo Client updates the cache and notifies your components.


```text
const QUERY = gql`
query FeedQuery {
feed @stream(initialCount: 5) {
id
contents
}
}
`
```


**Note:** The implementations of` @stream` differ between GraphQL spec versions in how incremental results are delivered. If you’re upgrading handlers, expect the timing of some incremental results to change.


## ` GraphQL17Alpha9Handler`


For servers that use the newer incremental delivery format implemented in` graphql@17.0.0-alpha.9` , Apollo Client now provides a dedicated handler:


```text
import { GraphQL17Alpha9Handler } from "@apollo/client/incremental";


const client = new ApolloClient({
// ...
incrementalHandler: new GraphQL17Alpha9Handler(),
});
```


This handler supports both` @defer` and` @stream` with the updated wire format. The handler automatically sets the appropriate` accept` header (` multipart/mixed;incrementalSpec=v0.2` ) to request the newest format.


**Note:** Your GraphQL server must implement the newer incremental delivery format for this handler to work correctly.


## Array support for useFragment


You can now pass an array to the` from` option in` useFragment` ,` useSuspenseFragment` , and` client.watchFragment` to watch multiple cache entities simultaneously:


```text
import type { FragmentType } from "@apollo/client";


interface ItemListProps {
items: Array<FragmentType<ItemFragment>>;
}


function ItemList({ items }: ItemListProps) {
const result = useFragment({
fragment: ITEM_FRAGMENT,
from: items,
});


console.log(result);
// {
//   data: [{...}, {...}, {...}, ...],
//   dataState: "complete",
//   complete: true
// }
}
```


This eliminates the need to either hoist the item fragment into the parent fragment or create a child component with` useFragment` for each array item.


## Other notable changes


### Bug fixes


- Fixed an issue where incremental payloads returning arrays with fewer items than cached items would incorrectly retain old items ([#12923](https://github.com/apollographql/apollo-client/pull/12923) )
- ` fetchMore` now rerenders incremental results when using` @defer` or` @stream` ([#12925](https://github.com/apollographql/apollo-client/pull/12925) )
- Ensured` PreloadedQueryRef` instances are unsubscribed when garbage collected ([#12884](https://github.com/apollographql/apollo-client/pull/12884) )


### Performance improvements


- Watches and observables created by` useFragment` ,` client.watchFragment` , and` cache.watchFragment` are now deduplicated for the same object, fragment, and variables. This should improve performance dramatically in cases where the same object is watched in the cache multiple times in the application. ([#12927](https://github.com/apollographql/apollo-client/pull/12927) ,[#13026](https://github.com/apollographql/apollo-client/pull/13026) ,[#13053](https://github.com/apollographql/apollo-client/pull/13053) )


### Other improvements


- The observable returned from` client.watchFragment` now includes a` getCurrentResult` function to get the current value of the fragment
- ` @client` fields with read functions now receive` undefined` for the cache value instead of` null` when a resolver is not defined ([#12927](https://github.com/apollographql/apollo-client/pull/12927) )
- Support for callback functions as the context option on mutate ([#12927](https://github.com/apollographql/apollo-client/pull/12927) )
- Support the` headers` transport for enhanced client awareness ([#13043](https://github.com/apollographql/apollo-client/pull/13043) )
- ` InMemoryCache` no longer filters out explicitly returned` undefined` items from` read` functions for array fields ([#13056](https://github.com/apollographql/apollo-client/pull/13056) )
- ` prerenderStatic` now exposes the return value to enable the use of[resumeAndPrerender](https://react.dev/reference/react-dom/static/resumeAndPrerender) with React 19.2 ([#13071](https://github.com/apollographql/apollo-client/pull/13071) )
- Add support for the` from` option used with` useFragment` to` client.readFragment` ,` cache.readFragment` ,` client.writeFragment` ,` cache.writeFragment` , and` cache.updateFragment` . ([#13038](https://github.com/apollographql/apollo-client/pull/13038) )
- Cache` merge` functions now receive` extensions` returned in the response ([#13058](https://github.com/apollographql/apollo-client/pull/13058) )


For a full list of changes, please see the[4.1 release notes](https://github.com/apollographql/apollo-client/releases/tag/%40apollo%2Fclient%404.1.0) .


Happy querying!


Written by


Jerel Miller


[Read more by Jerel Miller](https://www.apollographql.com/blog/author/jerel)
