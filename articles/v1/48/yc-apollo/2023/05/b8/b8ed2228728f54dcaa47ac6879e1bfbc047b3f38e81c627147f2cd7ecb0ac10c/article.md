---
schema_version: "1.0.0"
document_id: "b8ed2228728f54dcaa47ac6879e1bfbc047b3f38e81c627147f2cd7ecb0ac10c"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/using-apollo-client-with-next-js-13-releasing-an-official-library-to-support-the-app-router"
published_at: "2023-05-10T14:09:08+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:721933cf4ed2cc1c71616d21c16245ef937c9eb8e7ee9c8a801245f0cbccd52b"
---

# Using Apollo Client with Next.js 13: releasing an official library to support the App Router

The[latest release of Next.js 13 (13.4)](https://nextjs.org/blog/next-13-4) declared the new app directory as stable and it will now be the default for new projects.


The app directory is a major milestone of Next.js and the whole React ecosystem as it marks all components under the app directory as[React Server Components](https://nextjs.org/docs/getting-started/react-essentials#server-components) . While this is an exciting new feature, it also brought some confusion to people using them for the first time.


You might have seen my blog post about how to use[Apollo Client with Next.js 13](https://www.apollographql.com/blog/apollo-client/next-js/how-to-use-apollo-client-with-next-js-13/) , and the solution we proposed worked, but it was a bit cumbersome and might break in future.


In order to make it easier to adopt Apollo Client in Next.js we are releasing an experimental library:` @apollo/experimental-nextjs-app-support` (don’t worry the name will change once the library is stable 😊).


This library will do all the right things for you whether you’re running under Server Components or if you’re running in Client Components.


## **When should you use Client Components vs Server Components?**


Thanks to Apollo Client’s normalized cache, Client Components can automatically update all usages of an entity in your application, for example as a result of a mutation. This is not the case for Server Components and they must be manually refreshed when data changes.


So if the same data would be displayed or used in both environments, it would quickly run out of sync and result in inconsistencies.


To keep things consistent, we suggest deciding whether to render each entity in RSC or in Client Components. In the context of Apollo Client, entities refer to normalized data objects that are stored in the client cache. These entities are used to avoid duplication of (cached) data and to enable efficient updates to the cache.


When data is fetched from a GraphQL server, it is normalised into entities and stored in the cache. Subsequent queries can then retrieve data from the cache using the same entity structure, rather than refetching the data from the server. Therefore, when deciding whether to render each entity in RSC or in Client Components, it is important to consider how the data will be accessed and updated in the cache.


## **Quick start**


Let’s see how we can use this new library in both RSC and Client Components, the first thing we need to do is to install both` @apollo/client` rc and` @apollo/experimental-nextjs-app-support` :


```text
npm install @apollo/client@rc @apollo/experimental-nextjs-app-support
```


After installing both libraries, we can choose between using Server Components or Client Components, depending on our preferred approach.


## **Server components**


If you want to use Apollo Client inside Server components you need to create a new file` lib/client.js` and put the following in it:


```text
import { ApolloClient, HttpLink, InMemoryCache } from "@apollo/client";
import { registerApolloClient } from "@apollo/experimental-nextjs-app-support/rsc";


export const { getClient } = registerApolloClient(() => {
return new ApolloClient({
cache: new InMemoryCache(),
link: new HttpLink({
// https://studio.apollographql.com/public/spacex-l4uc6p/
uri: "https://main--spacex-l4uc6p.apollographos.net/graphql",
// you can disable result caching here if you want to
// (this does not work if you are rendering your page with `export const dynamic = "force-static"`)
// fetchOptions: { cache: "no-store" },
}),
});
});
```


` registerApolloClient` will return a` getClient` function that allows you to safely get an instance of the Apollo Client that’s scoped to the current request, so you won’t end up sharing the Apollo Client cache within multiple requests.


Note that the default caching mechanism in Next.js is to cache everything at build time, make sure you set[the correct options](https://www.apollographql.com/blog/apollo-client/how-to-use-apollo-client-with-next-js-13/#next-js-cache-and-apollo) based on how your data changes.


## **Client Components (with server side rendering)**


If you want to use Apollo Client using the “traditional” approach of fetching data on the client you can create a new file called` lib/apollo-wrapper.js` and put the following content:


```text
"use client";


import {
ApolloClient,
ApolloLink,
HttpLink,
} from "@apollo/client";
import {
ApolloNextAppProvider,
NextSSRInMemoryCache,
SSRMultipartLink,
} from "@apollo/experimental-nextjs-app-support/ssr";


function makeClient() {
const httpLink = new HttpLink({
// https://studio.apollographql.com/public/spacex-l4uc6p/
uri: "https://main--spacex-l4uc6p.apollographos.net/graphql",
});


return new NextSSRApolloClient({
cache: new NextSSRInMemoryCache(),
link:
typeof window === "undefined"
? ApolloLink.from([
new SSRMultipartLink({
stripDefer: true,
}),
httpLink,
])
: httpLink,
});
}


export function ApolloWrapper({ children }: React.PropsWithChildren) {
return (
<ApolloNextAppProvider makeClient={makeClient}>
{children}
</ApolloNextAppProvider>
);
}
```


This script is more complex from the previous one, so let’s break it down:


```text
"use client";
```


The` "use client"` directive tells the Next.js bundler that this file has a Client Component, this enables us to use all the APIs that aren’t available in Server Components, like` useContext` ,` useState` and so on.


In our` makeClient` we create an Apollo Http Link similar to what we did before, but if this is executed in SSR we also combine it with the` SSRMultipartLink` :


```text
return new NextSSRApolloClient({
cache: new NextSSRInMemoryCache(),
link:
typeof window === "undefined"
? ApolloLink.from([
new SSRMultipartLink({
stripDefer: true,
}),
httpLink,
])
: httpLink,
});
```


The` SSRMultipartlink` with` stripDefer: true` , will strip all fragments that are marked with the` @defer` directive from your queries during SSR – we want to get data out to the browser as fast as possible, and with a cache-policy like` cache-first` or` cache-and-network` , you can already get partial data in the browser, while the browser can start to receive the deferred interfaces.


In addition to that, when creating the Apollo Client, we also use the` NextSSRInMemoryCache` : this cache knows how to store the normalised cache in a way that can be then reused by the` ApolloNextAppProvider` to restore the cache on the browser side.


Finally we create a new React Component that combines everything like this:


```text
export function ApolloWrapper({ children }: React.PropsWithChildren) {
return (
<ApolloNextAppProvider makeClient={makeClient}>
{children}
</ApolloNextAppProvider>
);
}
```


Then we can use the wrapper like in our components like this:


```text
// app/layout.js
import { ApolloWrapper } from "/@lib/apollo-wrapper";


export default function RootLayout({
children,
}: {
children: React.ReactNode,
}) {
return (
<html lang="en">
<body>
<ApolloWrapper>{children}</ApolloWrapper>
</body>
</html>
);
}
```


Then we can use useSuspenseQuery to fetch our data:


```text
"use client";


export const dynamic = "force-dynamic";


import { useSuspenseQuery } from "@apollo/experimental-nextjs-app-support/ssr";


const query = gql`query {
launchLatest {
mission_name
}
}`


export default function PollPage() {
const { data } = useSuspenseQuery(query);


return <div>{data.launchLatest.mission_name}</div>;
};
```


In this snippet we are creating a new page that will render the latest launch from the[SpaceX API](https://studio.apollographql.com/public/spacex-l4uc6p/) , here we are also marking this page as a client component and then using` useSuspenseQuery` to fetch the query. Finally we are using export const` dynamic = "force-dynamic";` to tell[Next.js not to cache this page](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#dynamic) , so we don’t get stale data when reloading.


Note: in bigger applications you’d probably want to use useSuspenseQuery in combination with[useFragment](https://www.apollographql.com/docs/react/api/react/hooks-experimental) which which you can try out by installing the latest 3.8 rc with` npm i @apollo/client@rc` 😊


If you want to learn more about the internals of this library feel free to check out the[RFC written by Lenz](https://github.com/apollographql/apollo-client-nextjs/pull/9) , it contains a lot of details about how it works and potential pitfalls (or areas for future improvements).


I’ve also created a demo that uses both approaches to create a basic application that allows us to answer a poll about GraphQL 😊 The source code for the example is[available here](https://github.com/apollographql/apollo-client-nextjs/tree/main/examples/polls-demo) . Feel free to clone the repo and try it locally!


If you have any questions feel free to[join our discord](https://discord.gg/YBazzh8tGQ) ! We are also[planning a livestream](https://discord.gg/graphos?event=1105869286133743656) to talk about this library, show some of the code and answer any questions you might have!


Huge appreciation goes to Lenz for developing this library, and to Dan, Jerel, and Josh for their work in reviewing pull requests and code, making it all possible.


Written by


Patrick Arminio


[Read more by Patrick Arminio](https://www.apollographql.com/blog/author/patrick)
