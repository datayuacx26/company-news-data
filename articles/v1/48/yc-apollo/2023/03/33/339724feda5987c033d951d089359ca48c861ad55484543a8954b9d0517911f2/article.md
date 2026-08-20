---
schema_version: "1.0.0"
document_id: "339724feda5987c033d951d089359ca48c861ad55484543a8954b9d0517911f2"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-to-use-apollo-client-with-next-js-13"
published_at: "2023-03-14T09:56:34+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:f54c5af6bda50754f6e0fd720f7d7750624b77ff6a1cc61654881b9529244a25"
---

# How to use Apollo Client with Next.js 13

We just[released an experimental library](https://www.apollographql.com/blog/announcement/frontend/using-apollo-client-with-next-js-13-releasing-an-official-library-to-support-the-app-router/) that makes it easier to use Apollo Client with the App directory in Next.js 13.


The most interesting feature is the introduction of React Server Components. To put it simply, Server Components allow you to run data fetching on the server, removing the need for client-side fetches in most cases!


That brings us to a question: can you use Apollo Client with React Server Components? The answer is yes, but there’s nuances to that, especially when using Next.js 13!


Feel free to skip over to the **TLDR;** of this post if you want a summary on how to use Apollo with Next.js!


In this blog post we’ll learn how to use Apollo Client in server components, how Next.js cache works and when you should use Apollo Client in server components and when to use them in Client Components.


Let’s start with learning how to do data fetching inside server components. I’ve prepared a simple[GraphQL API](https://studio.apollographql.com/public/time-pav6zq?variant=main) that returns the current time. We’ll use this API to show how the caching works!


## **An example of data fetching inside server components**


Here’s a basic example on how you would an GraphQL query using fetch inside server components:


```text
export default async function Page() {
const data = await fetch(
"https://main--time-pav6zq.apollographos.net/graphql",
{
method: "POST",
body: JSON.stringify({
query: '{ now(id: "1") }',
}),
headers: {
"Content-Type": "application/json",
},
}
).then((res) => res.json());


return <main>{data.now}</main>;
}
```


One thing you might notice here is that we are creating an async component, this is enabled by[Server Components](https://github.com/reactjs/rfcs/pull/229) and it means we can await the result of our data fetching inside our component! I think this is pretty neat as it makes dealing with data fetching so much easier!


The fetch call above is a bit verbose, so let’s replace that with Apollo Client! As I mentioned above, we can still use Apollo Client in Server Components, but there’s one main gotcha. In Server Components, you’re not allowed to use any hook. So, useState, useContext and more are forbidden, this also includes ApolloProvider (which uses the context) and useQuery.


But that’s not a problem, we can still use the client directly in our components.


Let’s use Apollo’s new experimental library for this, let’s install it by doing:


```text
npm install @apollo/client@rc @apollo/experimental-nextjs-app-support
```


This library makes it easier to use Apollo Client in Server Components and Client Components when using Next.js 13


Now that we have the library installed we can use it get a function that allows to get the Apollo Client every time we need it:


```text
// lib/client.js
import { HttpLink, InMemoryCache, ApolloClient } from "@apollo/client";
import { registerApolloClient } from "@apollo/experimental-nextjs-app-support/rsc";


export const { getClient } = registerApolloClient(() => {
return new ApolloClient({
cache: new InMemoryCache(),
link: new HttpLink({
uri: "https://main--time-pav6zq.apollographos.net/graphql",
}),
});
});
```


In our previous version of this blog post, we were manually creating the` getClient` function and checking there if we needed to create a new client or not, but now this is all handled by` registerApolloClient` !


` @apollo/experimental-nextjs-app-support` makes sure that we only instance the Apollo Client once per request, since Apollo Client’s cache is designed with a single user in mind, we recommend that your Next.js server instantiates a new cache for each SSR request, rather than reusing the same long-lived instance for multiple users’ data.


Now that we have a way to fetch the client, we can use it directly in our Server Component, like so:


```text
// app/page.js
import { getClient } from "@/lib/client";


import { gql } from "@apollo/client";


const query = gql`query Now {
now(id: "1")
}`;


export default async function Page() {
const { data } = await getClient().query({ query });


return <main>{data.now}</main>;
}
```


That’s much better! By using Apollo Client we can also re-use all the tooling built around it,[like codegen for example](https://github.com/apollographql/apollo-client-nextjs/pull/23) .


Let’s see how Next.js handles http requests when using Server Components.


## **Next.js Server Components cache**


When using Server Components, Next.js overrides all the fetch requests being done there and adds caching to them.


Next.js cache is a document based cache, which is different from Apollo Client’s normalized cache. If you want to know more about how Apollo Cache works head over to this[blog post](https://www.apollographql.com/blog/apollo-client/caching/demystifying-cache-normalization/) .


Let’s go back to our initial example:


```text
export default async function Page() {
const data = await fetch(
"https://main--time-pav6zq.apollographos.net/graphql",
{
method: "POST",
body: JSON.stringify({
query: '{ now(id: "1") }',
}),
headers: {
"Content-Type": "application/json",
},
}
).then((res) => res.json());


return <main>{data.now}</main>;
}
```


If we deploy this to Vercel and hit our production page a few times, we’ll see the same value repeatedly.


This is Next.js’ default caching strategy, if we don’t pass any caching option to our fetch requests or to the page, then Next.js will cache the results when building the page.


Let’s see how we can tell next to invalidate this fetch every 10 seconds:


```text
export default async function Page() {
const data = await fetch(
"https://main--time-pav6zq.apollographos.net/graphql",
{
method: "POST",
body: JSON.stringify({
query: '{ now(id: "1") }',
}),
headers: {
"Content-Type": "application/json",
},
next: { revalidate: 10 }
}
).then((res) => res.json());


return <main>{data.now}</main>;
}
```


Here, you can see we passed` { next: { revalidate: 10 } }` to our fetch. This is an extension to the standard fetch API from Node.js, and it tells Next.js to revalidate this specific request every 10 seconds.


There are quite a few options on caching. Read this Next.js’s documentation pagte to know more about them:[https://beta.nextjs.org/docs/data-fetching/caching](https://beta.nextjs.org/docs/data-fetching/caching)


## **Next.js cache and Apollo**


Since Next.js patches all the fetch requests, this means that requests done with Apollo Client are also affected.


And, if you’re wondering, yes, it doesn’t matter whether requests are GET or POST, Next.js caches all of them!


We updated our client to avoid any caching on the server side; let’s try our request again:


```text
// app/page.tsx
import { getClient } from "@/lib/client";


import { gql } from "@apollo/client";


const query = gql`query Now {
now(id: "1")
}`;


export default async function Page() {
const { data } = await getClient().query({ query });


return <main>{data.now}</main>;
}
```


If we deploy this to Vercel and hit this page, we’ll see the same behavior as the page with the plain fetch request. The data is cached at build time!


## **Revalidating GraphQL requests**


We wouldn’t want all our GraphQL requests to be cached forever, fortunately Apollo Client allows us to pass fetch options to our queries, let’s see that in action by updating our page to revalidate that GraphQL query every 5 seconds:


```text
// app/page.tsx
import { getClient } from "@/lib/client";


import { gql } from "@apollo/client";


const query = gql`query Now {
now(id: "1")
}`;


export default async function Page() {
const { data } = await getClient().query({
query,
context: {
fetchOptions: {
next: { revalidate: 5 },
},
},
});


return <main>{data.now}</main>;
}
```


Here, we used the context argument to pass fetchOptions to our client (HTTPLink will read them and pass them to the underlying fetch call).


Additionally, we can also use[Segment Options](https://beta.nextjs.org/docs/api-reference/segment-config#revalidate) to specify the caching options for a particular page, the example above could also be done like this:


```text
// app/page.tsx
import { getClient } from "@/lib/client";


import { gql } from "@apollo/client";


//           👇
export const revalidate = 5;


const query = gql`
query Now {
now(id: "1")
}
`;


export default async function Page() {
const { data } = await getClient().query({ query });


return <main>{data.now}</main>;
}
```


Now we know how the caching works in Next.js and Apollo and how to change it based on user preferences. Before doing a recap of this post, I wanted to mention how you can still use Apollo Client with useClient and useMutation.


## **Client components**


As I mentioned before, all pages inside Next.js’ app directory are Server Components by default, so we can’t use anything that uses context, state and so on. Fortunately there’s a directive that tells React that a component is a client component, which means this component will be rendered both on server side (when using a SSR framework like Next.js) and on the client side. This directive is called` "use client"` and it works like this:


```text
"use client";


import { useSuspenseQuery } from "@apollo/experimental-nextjs-app-support/ssr";


import { gql } from "@apollo/client";


const query = gql`query Now {
now(id: "1")
}`;


export default function Page() {
const { data } = useSuspenseQuery(query);


return <main>{data.now}</main>;
}
```


In a previous version of this blog post, we were using` getClient` to get the Apollo Client and to pass it to` useQuery` . Now, we are using the new` @apollo/experimental-nextjs-app-support` to prevent having to get the client manually, and also using` useSuspenseQuery` instead of` useQuery` .


In addition to` "use client"` we are also using` useSuspenseQuery` from the new` @apollo/experimental-nextjs-app-support` library to allow for streaming SSR rendering. Similar to what we used to do before we need to use a provider to pass the client to the hooks, let’s start by creating an` ApolloWrapper` that does all of this for us:


```text
// lib/apollo-provider.js
"use client";


import { HttpLink } from "@apollo/client";
import {
NextSSRApolloClient,
ApolloNextAppProvider,
NextSSRInMemoryCache,
SSRMultipartLink,
} from "@apollo/experimental-nextjs-app-support/ssr";


function makeClient() {
const httpLink = new HttpLink({
uri: "https://main--time-pav6zq.apollographos.net/graphql",
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


If you’re curious to know how this snippet works feel free to read[the explanation here](https://www.apollographql.com/blog/announcement/frontend/using-apollo-client-with-next-js-13-releasing-an-official-library-to-support-the-app-router/#client-components-with-server-side-rendering)


Now that we have a wrapper component we can use it in our layout file, like so:


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


This snippet makes sure that all the pages using this layout will have access to the Apollo Client.


Finally, we can use the` useSuspenseQuery` hook as we have seen above:


```text
"use client";


import { useSuspenseQuery } from "@apollo/experimental-nextjs-app-support/ssr";


import { gql } from "@apollo/client";


const query = gql`query Now {
now(id: "1")
}`;


export default function Page() {
const { data } = useSuspenseQuery(query);


return <main>{data.now}</main>;
}
```


## **TLDR;**


Thanks to Apollo’s new library for Next.js, we can easily and safely use Apollo Client in both Server Components and Client Components:


Here’s a full snippet for our client setup:


```text
// lib/client.js
import { HttpLink, InMemoryCache, ApolloClient } from "@apollo/client";


export const { getClient } = registerApolloClient(() => {
return new ApolloClient({
cache: new InMemoryCache(),
link: new HttpLink({
uri: "https://main--time-pav6zq.apollographos.net/graphql",
}),
});
});
```


And this is how we can use the client inside a Server Component:


```text
// app/page.tsx
import { getClient } from "@/lib/client";


import { gql } from "@apollo/client";


export const revalidate = 5;
const query = gql`query Now {
now(id: "1")
}`;


export default async function Page() {
const client = getClient();
const { data } = await client.query({ query });


return <main>{data.now}</main>;
}
```


And this is how we can use the client inside a Client Component by first creating an` ApolloWrapper` component using the new` ApolloNextAppProvider` :


```text
// lib/apollo-provider.js
"use client";


import { ApolloLink, HttpLink } from "@apollo/client";
import {
ApolloNextAppProvider,
NextSSRInMemoryCache,
NextSSRApolloClient,
SSRMultipartLink,
} from "@apollo/experimental-nextjs-app-support/ssr";


function makeClient() {
const httpLink = new HttpLink({
uri: "https://main--time-pav6zq.apollographos.net/graphql",
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


Then using it in our layout, like this:


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


and finally using` useSuspenseQuery` in our client components


```text
"use client";


import { useSuspenseQuery } from "@apollo/experimental-nextjs-app-support/ssr";


import { gql } from "@apollo/client";


const query = gql`query Now {
now(id: "1")
}`;


export default function Page() {
const { data } = useSuspenseQuery(query);


return <main>{data.now}</main>;
}
```


If you have any question about Apollo Client or Apollo in general[join our discord](https://discord.gg/graphos) , we’ll be happy to answer your questions!


Written by


Patrick Arminio


[Read more by Patrick Arminio](https://www.apollographql.com/blog/author/patrick)
