---
schema_version: "1.0.0"
document_id: "88081bca3444b83c19628a1b7b4ed3fc167032cfbe2858c0000dace82176a80a"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/wait-for-it-announcing-apollo-client-3-8-with-react-suspense-integration"
published_at: "2023-08-07T12:07:43+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:3263cee3be85899b45d91c661541b86ef3df6940cf7c406c39a7bbf1fa414b96"
---

# Wait for it… Announcing Apollo Client 3.8 with React Suspense Integration

Today we’re thrilled to announce the release of Apollo Client 3.8 with long-awaited support for React 18’s Suspense features! This is our biggest minor release yet and is **jam packed** with lots of great features and updates. This release is a culmination of 16 alphas, 8 betas, and 3 release candidates.


Thank you to everyone who has tried the new features and provided us with valuable input!


Here is what to expect with this release:


- ⏲️ 3 new React hooks that integrate with React’s Suspense features
- 📑 Custom query document transforms to programmatically modify queries
- 🔗 An Apollo Link that will automatically remove` __typename` fields from variables
- 📉 A new error extraction mechanism for leaner bundles and richer production error messages
- ⏩ A type-safe way to skip queries with the new` skipToken` sentinel
- 🎬 A` @nonreactive` directive to selectively skip updates to query subtrees
- 🎓 Stability for the` useFragment` hook


Let’s explore each of these in more detail.


## ⏲️ React Suspense integration


We are thrilled to bring the long-awaited[React Suspense](https://react.dev/reference/react/Suspense) integration into Apollo Client with the release of 3.8! With this release, we are bringing 3 new React hooks to Apollo Client:` useSuspenseQuery` ,` useBackgroundQuery` , and` useReadQuery` . Let’s take a look at each of these hooks and how they integrate into your application.


### **Fetching data with` useSuspenseQuery`**


This hook initiates a GraphQL request and suspends until its data is loaded from the network. This hook is a suspenseful replacement for` useQuery` that takes full advantage of the React Suspense capabilities.


```text
function App() {
return (
<Suspense fallback={<div>Loading...</div>}>
<Greeting />
</Suspense>
);
}


function Greeting() {
const { data } = useSuspenseQuery(GREETING_QUERY);


return <div>{data.greeting}</div>;
}
```


If you look closely, you may notice that this code looks synchronous. Due to the mechanics of Suspense, your component can safely assume that whenever it renders,` data` is available, resulting in leaner components. Loading state is now handled by` <Suspense />` boundaries. No more managing loading booleans 🎉.


### Error handling


With` useQuery` , you may be used to handling errors by checking the` error` property returned from` useQuery` to determine whether to display an error state or not.` useSuspenseQuery` takes full advantage of[React Error Boundaries](https://react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary) and will instead throw errors. Errors propagate to the nearest error boundary which gives you more control over the granularity of your error fallback UI.


Of course, when you want to render partial data returned by a query that contains errors, you can change the` errorPolicy` to tell` useSuspenseQuery` not to throw errors and instead return the error next to` data` . Access the` error` property returned from` useSuspenseQuery` in these situations.


```text
const { data, error } = useSuspenseQuery(QUERY, {
errorPolicy: 'all'
});
```


### Marking updates as transitions


You may not always want to show a loading UI in response to state updates that suspend your components, such as changing variables or a` refetch` .` useSuspenseQuery` integrates with[React 18 transitions](https://react.dev/reference/react/Suspense#preventing-already-revealed-content-from-hiding) to give you control on whether to show the loading UI.


```text
import { startTransition } from 'react';


function Greeting() {
const { data, refetch } = useSuspenseQuery(GREETING_QUERY);


function handleRefetch() {
startTransition(() => {
refetch();
});
}
// ...
}
```


This marks the` refetch` as a *transition* which tells React to keep the stale UI in place until the refetched data has finished loading. This can lead to an improved user experience in cases where rendering the loading fallback is too jarring.


### Avoiding request waterfalls with` useBackgroundQuery` and` useReadQuery`


As your app grows in size and complexity, it’s not uncommon for nested components to use their own data fetching hooks. When fetching queries using` useSuspenseQuery` , this may lead to request waterfalls where nested components have to wait for their parent components to finish suspending before their request is kicked off.` useBackgroundQuery` was created to let you fetch now and suspend later. Pass the returned` queryRef` from` useBackgroundQuery` to` useReadQuery` to suspend the component while waiting for data to finish loading.


```text
function App() {
// Fetch now
const [queryRef] = useBackgroundQuery(GET_DOG, {
variables: { name: "Mozzarella" }
});


return (
<Suspense fallback={<div>Loading...</div>}>
<Dog queryRef={queryRef} />
</Suspense>
);
}


function Dog({ queryRef }) {
// Suspend later
const { data } = useReadQuery(queryRef);


return <div>Name: {data.dog.name}</div>;
}
```


As an added performance benefit, cache updates are handled by` useReadQuery` , thereby only re-rendering the component that calls` useReadQuery` .` useBackgroundQuery` is not responsible for handling data, so it will not re-render until either` variables` change, or the` refetch` and` fetchMore` functions are called.


To learn more about the Suspense integration in-depth, check out our detailed guide on[fetching with Suspense](https://www.apollographql.com/docs/react/data/suspense) .


## 📑 Programmatically modify query documents using custom document transforms


If you’ve used Apollo Client for any amount of time, you’ve likely noticed that Apollo Client will make modifications to your query before it is sent to the network, such as adding the` __typename` field or removing` @client` fields from the query. This is a technique known as a **document transform;** an advanced capability that reads and modifies the[abstract syntax tree](https://en.wikipedia.org/wiki/Abstract_syntax_tree) of a GraphQL query. With the release of 3.8, we are bringing this capability to user-space, giving you the power to customize GraphQL query documents before they are sent to the network.


While this previously could be accomplished within an[Apollo Link](https://www.apollographql.com/docs/react/api/link/introduction) , this technique had severe shortcomings, namely modifications to GraphQL documents were not seen by the cache. This limited the kinds of document transforms that could be safely made within the link chain.


This limitation is now a thing of the past! Document transforms are designed to give you full power to customize the query document to your heart’s content, such as the ability to add` @client` directives to existing fields to take advantage of[local state](https://www.apollographql.com/docs/react/local-state/local-state-management) , or adding fragments to the query document defined in the[fragment registry](https://www.apollographql.com/docs/react/data/fragments#registering-named-fragments-using-createfragmentregistry) . Document transforms are run before every request for all operations to ensure runtime conditions that alter the output from one run to the next are handled properly.


Add a document transform to Apollo Client by creating an instance of the` DocumentTransform` class and passing it as the` documentTransform` option to the` ApolloClient` constructor:


```text
const documentTransform = new DocumentTransform((document) => {
// ...make modifications to `document`
return transformedDocument;
});


const client = new ApolloClient({
documentTransform,
// ...
});
```


Document transforms are designed to be extremely composable. Combine transforms using the` concat` function, or conditionally use a transform with the` split` function.


```text
const mutationTransform = new DocumentTransform(...)
.concat(new DocumentTransform(...))
.concat(new DocumentTransform(...));


const defaultTransform = new DocumentTransform(...);


const documentTransform = DocumentTransform.split(
(document) => isMutationOperation(document),
mutationTransform,
defaultTransform
);
```


To learn more about this powerful feature, check out our guide on[document transforms.](https://www.apollographql.com/docs/react/data/document-transforms)


## 🔗 Automatically remove` __typename` from` variables` using the new` removeTypenameFromVariables` link


Opened in 2018 with 206 reactions, we were happy to close one of our most requested and longest standing[feature requests](https://github.com/apollographql/apollo-feature-requests/issues/6) ! In the several years this request has been opened, we saw no shortage of user-created solutions and determined it was time to provide first-class support for this functionality in the core library.


With this release, we are introducing a new` removeTypenameFromVariables` link that automatically strips the` __typename` field from` variables` before a request is made to your GraphQL server. This was a common problem when data requested from a previous query was used as input to another query or mutation as it meant you had to remember to strip` __typename` yourself.


For more information on this link, including how to *keep*` __typename` for JSON scalar variables when needed, check out the[link documentation](https://www.apollographql.com/docs/react/api/link/apollo-link-remove-typename) .


## 📉 Leaner Bundles with our new error extraction mechanism


With any robust JavaScript library, its important to provide helpful warnings and detailed errors when encountering issues during execution. Apollo Client is no different and does its best to provide warnings for common mistakes and detailed errors when issues arise.


This comes at a cost, however, because you need to ship each of these warning and error strings with the library to the end user, resulting in a larger bundle size. In previous versions of Apollo Client, it saved on bundle size in production builds by reducing errors down to a single message. You’ve likely seen errors that look something like this:


```text
This file is meant to help with looking up the source of errors like
"Invariant Violation: 35" and is automatically generated by the file
@apollo/client/config/processInvariants.ts for each @apollo/client
release. The numbers may change from release to release, so please
consult the @apollo/client/invariantErrorCodes.js file specific to
your @apollo/client version. This file is not meant to be imported.
```


This was unhelpful for a couple reasons:


- You needed to search your file system for the file mentioned in the error message to decipher the error.
- Dynamic information that contributed to the error was lost because the lookup file only contained static strings.


With the release 3.8, we’ve improved on both of these issues. Error messages are now, by default, *omitted* from the core bundle to reduce its bundle size. Minified error messages now contain links to an error page in our documentation to provide rich information about the conditions that triggered the error. This error is specific to the Apollo Client version you’re using to ensure accuracy. Now you’ll see something like the following when an error is encountered:


```text
Uncaught Invariant Violation: An error occured! For more details, see the full error text at https://go.apollo.dev/c/err#%7B%22version%22%3A%223.8.0-rc.1%22%2C%22message%22%3A71%2C%22args%22%3A%5B%5D%7D
```


See a[sample](https://www.apollographql.com/docs/react/errors/#%7B%22version%22%3A%223.8.0-rc.1%22%2C%22message%22%3A71%2C%22args%22%3A%5B%5D%7D) error for more information.


You can opt-in to receive the full error messages by calling the` loadErrorMessages` and` loadDevMessages` functions. We recommend only doing this in non-production environments to reduce bundle size in production builds.


```text
import { loadErrorMessages, loadDevMessages } from "@apollo/client/dev";


if (process.env.NODE_ENV !== "production") {
loadErrorMessages();
loadDevMessages();
}
```


## ⏭️ Skip query execution in a type-safe way with` skipToken`


Due to React’s[rules of hooks](https://react.dev/warnings/invalid-hook-call-warning) , there is no way to conditionally call hooks in your components. In some situations, you may find yourself needing to avoid query execution. Apollo Client provides the` skip` option to avoid query execution in these scenarios.


Imagine this common scenario: You want to skip query execution if a certain variable is not set, but required in your GraphQL query. You might be tempted to write something like this:


```text
const { data } = useSuspenseQuery(query, {
variables: { id },
skip: !id
});
```


In this case, TypeScript will complain:


```text
Type 'number | undefined' is not assignable to type 'number'.
Type 'undefined' is not assignable to type 'number'.ts(2769)
```


To get around this error, you have to tell TypeScript to ignore the fact that` id` might be` undefined` :


```text
const { data } = useSuspenseQuery(query, {
variables: { id: id! },
skip: !id
});
```


Alternatively, you might provide an obscure fallback value of the same type:


```text
const { data } = useSuspenseQuery(query, {
variables: { id: id || 0 },
skip: !id
});
```


Neither of these situations are great. in one solution, you’re lying to TypeScript by simultaneously telling it that` id` both exists and doesn’t exist, and in the other solution, you’re providing a value that might be nonsensical in the context of your application. This can be especially problematic if your skip logic becomes more complex as you might accidentally obscure bugs by not allowing TypeScript to warn you about potential issues.


To solve this issue, we are adding a` skipToken` that can be used in place of` options` for both the` useSuspenseQuery` and` useBackgroundQuery` hooks. This provides a type-safe way to skip a query without the hacky workarounds provided above.


```text
import { skipToken } from '@apollo/client';


const { data } = useSuspenseQuery(
query,
id ? { variables: { id } } : skipToken
);


const [queryRef] = useBackgroundQuery(
query,
id ? { variables: { id } } : skipToken
);
```


` skipToken` is a replacement for the` skip` option. To ease migration from` useQuery` to these new hooks, the` skip` option is provided for use in` useSuspenseQuery` and` useBackgroundQuery` , but are deprecated to encourage the use of` skipToken` .


For more detailed information, see the[documentation](https://www.apollographql.com/docs/react/api/react/hooks/#skiptoken) .


## 🎬 Stop overreacting with the` @nonreactive` directive


Apollo Client 3.8 introduces a new` @nonreactive` directive that lets you selectively skip re-rendering for data changed in specific field or fragment subtrees. This is particularly effective when rendering lists of data where you prefer the parent component avoids re-rendering for specific updates to child data. Pair this with` useFragment` to updates are targeted for child components.


For a detailed explanation, see our[@nonreactive reference](https://www.apollographql.com/docs/react/data/directives/#nonreactive) and Alessia’s[post on the Apollo blog about using @nonreactive with useFragment](https://www.apollographql.com/blog/apollo-client/introducing-apollo-clients-nonreactive-directive-and-usefragment-hook/) .


## 🎓` useFragment` is stable


We released experimental support for` useFragment` in Apollo Client 3.7 with an` _experimental` suffix. Apollo Client 3.8 stabilizes this hook and drops the` _experimental` suffix. Thank you to everyone that provided feedback for this hook during its experimental phase!


To migrate to 3.8, simply drop the` _experimental` suffix from the import.


```text
// Before
import { useFragment_experimental } from '@apollo/client';


// After
import { useFragment } from '@apollo/client';
```


## Other notable changes and improvements


- In Apollo Client versions prior to 3.7,` HttpLink` and` BatchHttpLink` created an` AbortController` internally whose signal would always abort when the request was completed. This could cause issues with Sentry Session Replay and Next.js App Router cache invalidations which replayed requests with the same options. This meant requests might be replayed with an aborted` signal` . 3.8 changes this behavior to only abort requests when unsubscribing from queries.
- User-provided` signal` s to` HttpLink` and` BatchHttpLink` now propagate` AbortErrors` back to the user. This fixes an issue where user-provided signals could get swallowed, resulting in mutations or queries that would never resolve. As a result of this change, users are now expected to handle abort errors with user-provided signals.
- 3.8 ships with improved ESM compatibility. Apollo Client isn’t fully ESM compatible yet, notably the` exports` field in package.json is still missing, but an improvement should be noticed.
- Development-only code is now bundled in a way that allows consuming bundlers to more reliably reduce bundle sizes while keeping compatibility with non-Node.js environments. See the[documentation](https://deploy-preview-11102--apollo-client-docs.netlify.app/development-testing/reducing-bundle-size) for more information on configuring your bundler to take advantage of this change.
- Better type policy inheritance with fuzzy possibleTypes: users who take advantage of this powerful abstraction were sometimes forced to write a lot of boilerplate keyFields configurations in certain scenarios. As of 3.8, users will find keyFields will behave more intuitively when used in combination with fuzzy possibleTypes. For more details, read the PR here:[https://github.com/apollographql/apollo-client/pull/10633](https://github.com/apollographql/apollo-client/pull/10633)


Of course there too many additions and changes to list. To view the full set of changes, see the[3.8 GitHub release](https://github.com/apollographql/apollo-client/releases/tag/v3.8.0) or the[changelog](https://github.com/apollographql/apollo-client/blob/main/CHANGELOG.md) .


## One more thing…


Apollo Client 3.8 just wasn’t enough for us, so we are also excited to announce the release of a[Spotify Showcase](https://github.com/apollographql/spotify-showcase) fully integrated with Apollo.


Todo apps are great, but lets face it, they don’t provide enough complexity to truly understand how multiple features work together in complex applications. The Spotify Showcase is a full-blown Spotify client that uses your Spotify account to stream your favorite tunes.


We have big plans to integrate this showcase into our daily work. We want this to be a showcase of Apollo Client best practices, a teaching tool to show off specific features of Apollo Client, and a proving ground for new and upcoming features. We understand that to create the best GraphQL client, we need to understand what it *feels* like to use it. Our users are building complex applications with Apollo Client every day and the best way to empathize with them is to use the tools and features ourselves.


This showcase isn’t just limited to Apollo Client either. If you’re curious about building apps with Apollo Server, or want to understand what an app looks like integrated with GraphOS and Apollo Federation, this showcase has it all. We encourage you to clone the repo, run the application, explore the code base, and most importantly, ask us questions about it! We want to know if this is a useful tool for you in understanding how to use Apollo Client. You can ask us questions by opening an[issue](https://github.com/apollographql/spotify-showcase/issues) on the GitHub repo or jump into our[Discord server](https://discord.gg/graphos) where we can talk live.


If you want to just checkout the backend, you can use our[public Explorer](https://studio.apollographql.com/public/Spotify-tb7du2/variant/prod-external/home) or re-create everything by using our “Try a Demo Graph” button in GraphOS. You can sign up for free[here](https://studio.apollographql.com/signup?referrer=blog) and then you’ll see that button and do this 👇


## 🌘 What’s next 🔭


We aren’t quite done with the Suspense story in Apollo Client with the release of 3.8. We plan to introduce a couple more additions to round out the Suspense functionality.


- Introduce a *lazy* version of` useBackgroundQuery` that will use a function to trigger query execution. This will provide a more robust mechanism for loading data in response to user events and preloading scenarios.
- A suspenseful version of` useFragment` that can be combined with` useBackgroundQuery` or its lazy alternative.
- Preload data *outside* of React to start loading data as early as possible. Combine this with` useReadQuery` to suspend when the data has not yet been loaded.


Outside of React, we have our eye on a few additional improvements to the core library.


- Introduce more robust testing utilities.` <MockedProvider />` has gotten a LOT of mileage throughout the years but has several shortcomings. We’d like to provide new, more robust testing utilities to improve the confidence in your Apollo Client applications that address the many shortcomings of the current solution.
- Add fine-grained metrics throughout the core library to provide powerful insights into core functionality such as cache hits/misses, network timing, etc. Please get in touch with us if you have an idea of the kind of metrics and insights you’d like to see with this feature!


### A note about our release cadence


3.8 is our biggest minor release yet chock-full of new functionality. While we are incredibly excited about all of the work that went into this release, we understand that this took some time to complete. Starting with 3.9, we will be adjusting our release cadence to bring more frequent releases. Expect to see smaller releases on a more frequent basis!


Written by


Jerel Miller


[Read more by Jerel Miller](https://www.apollographql.com/blog/author/jerel)
