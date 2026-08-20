---
schema_version: "1.0.0"
document_id: "92d1642a8b59ada28d5583fa256f7b18ee79ee2f0f89327993b9af7c45714384"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/whats-new-in-apollo-client-3-11"
published_at: "2024-07-23T11:21:52+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:e4c77f1b47731c0659be828635e838ac3a26a5f5c205f785deff45f2e44a0b69"
---

# What’s new in Apollo Client 3.11

With the release of Apollo Client 3.11, we’re excited to announce a number of features and improvements. Here is what you can expect in Apollo Client 3.11:


- A number of improvements to Apollo Client’s subscription APIs such as access to` subscribeToMore` in Suspense-enabled hooks, the ability to forward extensions in` useSubscription` , a new` ignoreResults` option for` useSubscription` , and a new` restart` function for` useSubscription` .
- Support for React 19 release candidate versions with improved React Compiler support.
- New Apollo Client Devtools options.
- Deprecation of the experimental schema testing utilities introduced in 3.10 in favor of the new[@apollo/graphql-testing-library](https://github.com/apollographql/graphql-testing-library) package.


Let’s dive in!


## Subscription API improvements


This release focuses on a number of improvements to Apollo Client’s subscription APIs.


Many have found utility in the` subscribeToMore` API that we’ve long had support for in` useQuery` .` subscribeToMore` is useful when you want to start a subscription that is expected to modify the attached query. With the release of the new Suspense-enabled hooks,` subscribeToMore` was notably missing. 3.11 now brings support for` subscribeToMore` to the` useBackgroundQuery` ,` useQueryRefHandlers` and` useLoadableQuery` hooks ([#11923](https://github.com/apollographql/apollo-client/pull/11923) ).


3.11 adds a new` ignoreResults` option to the` useSubscription` hook ([#11921](https://github.com/apollographql/apollo-client/pull/11921) ).` ignoreResults` disables component re-rendering when you get updates from the subscription. This is a useful performance optimization when you end up managing state yourself with updates via the` onData` callback.


We’ve heard many times that it is difficult to restart subscriptions after they’ve been terminated, either due to an error, or some other means. We’re making this easier in 3.11 by introducing a new` restart` function to the` useSubscription` hook that reconnects a subscription when this function is called ([#11927](https://github.com/apollographql/apollo-client/pull/11927) ).` restart` isn’t limited to just terminated subscriptions either. Calling` restart` with an active subscription terminates the connection and restarts it.


Last but not least, 3.11 adds support for an` extensions` option in useSubscription which propagates to the link chain ([#11854](https://github.com/apollographql/apollo-client/pull/11854) ). This is useful when using libraries such as[graphql-ws](https://github.com/enisdenjo/graphql-ws) which support` extensions` in its[websocket protocol](https://github.com/enisdenjo/graphql-ws/blob/master/PROTOCOL.md) . For other usages, you can access` extensions` via` operation.extensions` in the link chain.


## React 19 support


3.11 adds official support for React 19 release candidate versions. We now run our entire React test suite with React 19 to ensure compatibility and will provide bug fixes against issues with React 19. Note that 3.11.0 will not support the public release of React 19 out-of-the-box to ensure we have time to integrate with any breaking changes that may or may not be added to React between now and the public version. Once the public version of React 19 is released, we will confirm Apollo Client compatibility and release a 3.11 patch version with official support.


3.11 also includes improved compatibility with[React Compiler](https://react.dev/learn/react-compiler) . We’ve rewritten large parts of` useQuery` ([#11869](https://github.com/apollographql/apollo-client/pull/11869) ) and` useSubscription` ([#11863](https://github.com/apollographql/apollo-client/pull/11863) ) to fix[Rules of React](https://react.dev/reference/rules) violations in order take advantage of the optimizations that React Compiler provides.


## New Apollo Client Devtools options


Have you used Apollo Client Devtools lately? We’ve added a number of new capabilities in recent releases with plans for many more exciting features to come!


We are continuing this release train with a feature that’s been frequently requested: the ability to inspect and monitor multiple client instances in Apollo Client Devtools ([#1418](https://github.com/apollographql/apollo-client-devtools/pull/1418) ). This feature allows you to connect and switch the Apollo Client instance you’re inspecting to see what its active queries, mutations, and cache look like. This feature extends to the “Explorer” tab and executes its queries using the selected client.


With 3.11, we are giving you the ability to add a custom name to your client instance to make it easier to identify in Apollo Client Devtools when used in apps with multiple client instances ([#11936](https://github.com/apollographql/apollo-client/pull/11936) ). As such, we are adding a new way to configure client instances for use with Apollo Client Devtools.


Previously you would connect your client instance to Apollo Client Devtools using the` connectToDevtools` option. We’ve deprecated this option in favor of a new` devtools` option that groups all devtools-related configuration together. 3.11 specifically adds 2 properties to this option:` enabled` which replaces the` connectToDevtools` option, and` name` which allows you to provide a custom name for the client instance.


```text
new ApolloClient({
devtools: {
enabled: true,
name: "My Custom Client"
}
});
```


This change is fully backwards compatible with the` connectToDevtools` option. Using` connectToDevtools` with 3.11 will set the` devtools.enabled` property for you until you’ve migrated to the new option.


## Goodbye experimental testing utilities, hello GraphQL Testing Library


Apollo Client 3.10 introduced new[experimental schema testing utilities](https://www.apollographql.com/blog/whats-new-in-apollo-client-3-10#introducing-new-experimental-schema-based-testing-utilities) that provided the ability to mock against your server’s GraphQL schema. We are grateful for all of the feedback we’ve received and the reception has been phenomenal!


Through this process, we’ve felt these testing utilities needed to be available to a wider audience, so we have open-sourced a new testing library called[@apollo/graphql-testing-library](https://github.com/apollographql/graphql-testing-library) where we will be focusing our efforts moving forward.` @apollo/graphql-testing-library` has the added benefit that it provides client-agnostic tools which means you can use these utilities with any client that speaks GraphQL such as Relay, urql, or just plain` fetch` ! Make sure to visit the repository and give it a star!


As such, we are deprecating the experimental schema testing utilities in 3.11 and will remove these APIs in 3.12 ([#11930](https://github.com/apollographql/apollo-client/pull/11930) ). If you are using these utilities in your test suite, please migrate over to` @apollo/graphql-testing-library` instead.


Thanks to everyone that provided feedback!


## Potentially breaking fixes


### Updates to the GraphQLError TypeScript types ([#11789](https://github.com/apollographql/apollo-client/pull/11789) )


Up until Apollo Client 3.11, Apollo Client typed GraphQL error types as` GraphQLError` instances. This was incorrect as these were always plain JavaScript objects parsed directly from the server response. Using checks such as` instanceof` to detect if the error was a GraphQL error would always fail, despite TypeScript reporting this as ok.


As of 3.11, we’ve properly typed GraphQL errors as` GraphQLFormattedError` objects instead. If you relied on the` GraphQLError` type directly in your code, you will need to update this type to use` GraphQLFormattedError` instead.


This also required a change for usages of the` ExecutionResult` type. If you use this type directly in your code, we advise you to switch to the` FormattedExecutionResult` type instead to ensure proper type compatibility.


### Change to when nextFetchPolicy is called ([#11626](https://github.com/apollographql/apollo-client/pull/11626) )


` nextFetchPolicy` , when used as a function, is now reliably called when variables change. Previously this would only be called when the current` fetchPolicy` was equal to the` fetchPolicy` option or the option was not specified. If you use` nextFetchPolicy` as a function, expect to see this function called more often.


Due to this bug, this also meant that the` fetchPolicy` might be reset to the initial` fetchPolicy` , even when you specified a` nextFetchPolicy` function. If you previously relied on this behavior, you will need to update your` nextFetchPolicy` callback function to implement this resetting behavior.


As an example, if your code looked like the following:


```text
useQuery(QUERY, {
nextFetchPolicy(currentFetchPolicy, info) {
// your logic here
}
);
```


Update your function to the following to reimplement the resetting behavior:


```text
useQuery(QUERY, {
nextFetchPolicy(currentFetchPolicy, info) {
if (info.reason === 'variables-changed') {
return info.initialFetchPolicy;
}
// your logic here
}
);
```


## Other notable changes


- Added the[cause](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error/cause) property in` ApolloError` to indicate the specific original cause of the error ([#11902](https://github.com/apollographql/apollo-client/pull/11902) )
- ` useQuery` has always returned an` errors` property from the hook result as a shortcut for` errors.graphQLErrors` . Until 3.11, this was mistakenly missing from the TypeScript type,` QueryResult` , returned from` useQuery` . 3.11 adds this property to` QueryResult` and deprecates its usage. We advise using` error.graphQLErrors` instead and plan to remove this property in a future major version. ([#11954](https://github.com/apollographql/apollo-client/pull/11954) )
- 3.11 removes the deprecated` canonizeResults` option added to the new` watchFragment` API introduced in 3.10. This was introduced as a deprecated option to provide ample time to migrate away from its use. 3.11 removes support for this option entirely. ([#11949](https://github.com/apollographql/apollo-client/pull/11949) )


Written by


Jerel Miller


[Read more by Jerel Miller](https://www.apollographql.com/blog/author/jerel)
