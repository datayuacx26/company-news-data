---
schema_version: "1.0.0"
document_id: "1f1e9126208f33aeec0b2431f1cd49b30abd8c3caa6531d6dcc166f298830740"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-to-use-apollo-client-with-remix"
published_at: "2022-10-12T12:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:fa8623234246041f67d32793b0fef0455d0270b35cfb9b7915b4f1ef41fc9708"
---

# How to use Apollo Client with Remix

Remix is a newer JavaScript web framework that focuses on using native web API’s to improve the user experience by providing fast page loads and transitions. It’s both a server and browser runtime, which means you can render your pages and make requests on the server (SSR) or wait to do it in the browser.


What if you want to use SSR and Apollo Client together? Whether it’s because you like the developer experience of Apollo Client’s hooks, you don’t want to have to worry about setting up caching, or you’re incrementally migrating your app that already uses Apollo Client from one framework over to Remix, it can be helpful to know how to get them working together. So in this post, we’ll be focusing on how to set up Apollo Client with Remix to enable the use of Apollo Client’s hooks (like` useQuery` ) and caching for your SSR pages.


You can find the docs used as references for the code in this post from the[Apollo](https://www.apollographql.com/docs/react/performance/server-side-rendering/) and[Remix](https://remix.run/docs/en/v1) docs.


## Prerequisites


- An understanding of what GraphQL is
- Familiarity with the basics of Apollo Client
- Familiarity with the basics of Remix is helpful, but not necessary


## Setting up a Remix app


If you don’t already have an existing Remix app, bootstrap one by running the following command:


```text
npx create-remix@latest
```


From the[Remix docs](https://remix.run/docs/en/v1/tutorials/jokes#generating-a-new-remix-project) : “This may ask you whether you want to install` create-remix@latest` . Enter` y` . It will only be installed the first time to run the setup script.”


You’ll be prompted for a few inputs before your Remix app will be fully set up. For the purposes of this post, we’ll stick with the basics and create a Remix app in a directory called` remix-apollo` . Here’s the sequence of questions you’ll be asked, and the answers we’ll provide for our setup:


```text
? Where would you like to create your app? remix-apollo
? What type of app do you want to create? Just the basics
? Where do you want to deploy? Choose Remix App Server if you're unsure; it's easy to change deployment targets. Remix App Server
? TypeScript or JavaScript? TypeScript
? Do you want me to run `npm install`? Yes
```


From the[Remix docs](https://remix.run/docs/en/v1/tutorials/jokes#generating-a-new-remix-project) : “Remix can be deployed in a large and growing list of JavaScript environments. The “Remix App Server” is a full-featured[Node.js](https://nodejs.org/) server based on[Express](https://expressjs.com/) . It’s the simplest option and it satisfies most people’s needs, so that’s what we’re going with for this tutorial.”


While we’re setting up our app, let’s also install the required packages for Apollo Client.


```text
npm install @apollo/client graphql
```


After our Remix app is set up, we can spin up the development server on` localhost:3000` :


```text
cd remix-apollo
npm run dev
```


## Examining the file structure


When you open up the app in your code editor, the file structure should look something like this:


```text
remix-apollo
├── README.md
├── app
│   ├── entry.client.tsx
│   ├── entry.server.tsx
│   ├── root.tsx
│   └── routes
│       └── index.tsx
├── package-lock.json
├── package.json
├── public
│   └── favicon.ico
├── remix.config.js
├── remix.env.d.ts
└── tsconfig.json
```


For setting up Apollo Client, we’re concerned with two files in the` app` directory:` entry.client.tsx` and` entry.server.tsx` .


- The` entry.server.tsx` file is the first JavaScript that will be run when a request is sent to your **server** . This file is used to render your React app to a string/stream that is sent as the response to the client.
- The` entry.client.tsx` file is the first JavaScript that will be run when your app loads in the **browser** . This file is used to[hydrate](https://reactjs.org/docs/react-dom.html#hydrate) your React components.


You can read more about the[file structure](https://remix.run/docs/en/v1/tutorials/jokes#explore-the-project-structure) and the[entry files](https://remix.run/docs/en/v1/api/conventions#entry-files) in the Remix docs.


## Setting up the server


The` entry.server.tsx` file has some initial set up in a` handleRequest` function. Here, we’ll want to modify it to create an instance of` ApolloClient` and wrap the` RemixServer` component in an` ApolloProvider` .


```text
// app/entry.server.tsx


import type { EntryContext } from "@remix-run/node";
import { RemixServer } from "@remix-run/react";
import { renderToString } from "react-dom/server";
import {
ApolloProvider,
ApolloClient,
InMemoryCache,
createHttpLink,
} from "@apollo/client";


export default function handleRequest(
request: Request, // Request type from the Fetch API
responseStatusCode: number,
responseHeaders: Headers, // Headers type from the Fetch API
remixContext: EntryContext
) {
const client = new ApolloClient({
ssrMode: true,
cache: new InMemoryCache(),
link: createHttpLink({
uri: "https://flyby-gateway.herokuapp.com/", // from Apollo's Voyage tutorial series (https://www.apollographql.com/tutorials/voyage-part1/)
headers: request.headers,
credentials: request.credentials ?? "include", // or "same-origin" if your backend server is the same domain
}),
});


const App = (
<ApolloProvider client={client}>
<RemixServer context={remixContext} url={request.url} />
</ApolloProvider>
);


// TODO: update everything below this line
const markup = renderToString(App);


responseHeaders.set("Content-Type", "text/html");


return new Response("<!DOCTYPE html>" + markup, {
status: responseStatusCode,
headers: responseHeaders,
});
}
```


The[Apollo docs](https://www.apollographql.com/docs/react/performance/server-side-rendering/#initializing-apollo-client) outline a few differences from a typical client-side initialization:


- You provide` ssrMode: true` . This prevents Apollo Client from refetching queries unnecessarily, and it also enables you to use the` <a href="https://www.apollographql.com/docs/react/performance/server-side-rendering/#executing-queries-with-getdatafromtree">getDataFromTree</a>`[function](https://www.apollographql.com/docs/react/performance/server-side-rendering/#executing-queries-with-getdatafromtree) (we’ll get into that later).
- Instead of providing a` uri` option, you provide an` HttpLink` instance to the` link` option. This enables you to specify any required authentication details when sending requests to your GraphQL endpoint from the server side.


Note that you also might need to make sure your GraphQL endpoint is configured to accept GraphQL operations from your SSR server (for example, by safelisting its domain or IP).


It’s possible and valid for your GraphQL endpoint to be hosted by the *same server* that’s performing SSR, like if you have a monorepo Remix app that has both the frontend and backend code. In this case, Apollo Client doesn’t need to make network requests to execute queries. For details, see[Avoiding the network for local queries](https://www.apollographql.com/docs/react/performance/server-side-rendering/#avoiding-the-network-for-local-queries) .


When your Remix app makes a request to the server, it first initializes an instance of` ApolloClient` and creates a React tree wrapped in an` ApolloProvider` and` RemixServer` . The` RemixServer` is the component used to generate the HTML in the response from the server.


It’s important to create an *entirely new instance* of Apollo Client for each request. Otherwise, your response to a request might include sensitive cached query results from a *previous* request.


[Apollo Docs](https://www.apollographql.com/docs/react/performance/server-side-rendering/#example)


**Depending on your use case, you *might* be safe keeping the same instance of Apollo Client. You can check out this[discussion about SSR and Apollo Client on GitHub](https://github.com/apollographql/apollo-client/issues/9520) for more details.


But because we’re using Apollo Client and will have some components in the React tree that execute a GraphQL query, we need to use the` getDataFromTree` function to instruct Apollo Client to execute all of the queries required by the tree’s components.


From the[Apollo docs](https://www.apollographql.com/docs/react/performance/server-side-rendering/#executing-queries-with-getdatafromtree) : “This function walks down the entire tree and executes every required query it encounters (including nested queries). It returns a` Promise` that resolves when all result data is ready in the Apollo Client cache.”


The following code replaces everything below the` TODO` comment in the` entry.server.tsx` code above:


```text
// app/entry.server.tsx


// Add this import to the top of the file
import { getDataFromTree } from "@apollo/client/react/ssr";


return getDataFromTree(App).then(() => {
// Extract the entirety of the Apollo Client cache's current state
const initialState = client.extract();


const markup = renderToString(
<>
{App}
<script
dangerouslySetInnerHTML={{
__html: `window.__APOLLO_STATE__=${JSON.stringify(
initialState
).replace(/</g, "\\u003c")}`, // The replace call escapes the < character to prevent cross-site scripting attacks that are possible via the presence of </script> in a string literal
}}
/>
</>
);


responseHeaders.set("Content-Type", "text/html");


return new Response("<!DOCTYPE html>" + markup, {
status: responseStatusCode,
headers: responseHeaders,
});
});
```


We create the` markup` inside of the` .then` function after calling` getDataFromTree` because we need to wait for the` Promise` from that function call to resolve so that the cache is ready before we render the React tree.


Note that we’re adding a` <script>` tag in our` markup` . This script sets the value of` window.__APOLLO_STATE__` to the Apollo Client cache’s current state so that it will be available on the browser. All of this markup will be rendered in the place of the` <Outlets />` component in` app/root.tsx` , which effectively wraps all of the content on every route of our app in the` entry.server.tsx` file’s` markup` .


The` replace` call escapes the` <` character to prevent cross-site scripting attacks that are possible via the presence of` </script>` in a string literal.


With the server side of things set up, let’s move on to the client!


## Setting up the client


Moving over to the` entry.client.tsx` file, you’ll find some more basic set up from Remix that uses` ReactDOM.hydrate` to rehydrate the markup generated on the server by the` entry.server.tsx` file. Because this is the first JavaScript that will be run in the browser, this is where we want to rehydrate the client-side cache so that it’s in sync with the cache on the server.


We’ll need a client-side initialization of` ApolloClient` that consumes the same` uri` as the` entry.server.tsx` file. For the cache, we can rehydrate it (via the` restore` function) to match the cache on the server, which we set to the` window.__APOLLO_STATE__` global object in the` markup` ’s` <script>` tag. To use our new instance of` ApolloClient` , we wrap the` RemixBrowser` component in an` ApolloProvider` and pass in our` client` .


```text
// app/entry.client.tsx


import { ApolloClient, ApolloProvider, InMemoryCache } from "@apollo/client";
import { RemixBrowser } from "@remix-run/react";
import { hydrate } from "react-dom";


function Client() {
const client = new ApolloClient({
cache: new InMemoryCache().restore(window.__APOLLO_STATE__),
uri: "https://flyby-gateway.herokuapp.com/", // the same uri in our entry.server file
});


return (
<ApolloProvider client={client}>
<RemixBrowser />
</ApolloProvider>
);
}


hydrate(<Client />, document);
```


## Make TypeScript happy


To make TypeScript happy about that global object we added to the` window` , navigate to the` remix.env.d.ts` file in the root of the project and add a` Window` interface at the bottom with a type for` __APOLLO_STATE__` :


```text
// remix.env.d.ts


interface Window {
__APOLLO_STATE__: any
}
```


## Run a query


To check that we can successfully run queries now, navigate to` app/routes/index.tsx` and replace the content with the following:


```text
// app/routes/index.tsx


import { gql, useQuery } from "@apollo/client";


const LOCATIONS_QUERY = gql`
query GetLocations {
locations {
id
name
description
photo
}
}
`;


export default function Index() {
const { data } = useQuery(LOCATIONS_QUERY);


return (
<div>
{JSON.stringify(data)}
</div>
);
}
```


Now if we spin up the development server with` npm run dev` and open up` localhost:3000` , we’ll see the JSON data from the` LOCATIONS_QUERY` on the screen.


If you open up the Network panel in the browser dev tools, you’ll notice that there is no GraphQL request! This is because our request was sent on the server, not the browser. Our component and usage of the Apollo Client` useQuery` hook remains the same!


If you log out` window.__APOLLO_STATE__` in the console of your browser dev tools, you should also see the JSON output of the data because it was updated to match the value of what’s in the cache. When the client-side version of the app runs its initial queries, the data is returned instantly because it’s already in the cache!


## Conclusion


In summary, to get Apollo Client working with Remix, we need to edit two files in the` app` directory of our app:` entry.server.tsx` and` entry.client.tsx` . We’re concerned specifically about these files because` entry.server` is the first JavaScript that is run when a request is sent to the server and` entry.client` is the first JavaScript that is run when our app loads in the browser. We want our GraphQL requests and cache rehydration to be some of the first things that happen when API calls are made and pages are loaded.


1. In` entry.server.tsx` , we create an instance of` Apollo Client` . Our initialization flags the` ssrMode` option as true, creates a` cache` with` InMemoryCache` , and uses` createHttpLink` instead of` uri` to allow us to send headers and credentials with our requests. Then we wrap` RemixServer` in an` ApolloProvider` and pass in our instance of` ApolloClient` . Next, we pass our` ApolloProvider` /` RemixServer` JSX as an argument to` getDataFromTree` to tell Apollo Client to execute all of our page’s queries. After the Promise from that function call resolves and the cache is fully updated, we extract the cache value from the Apollo Client instance and set it as the value of a global window object (in our case here, we called it` __APOLLO_STATE__` ) in a` script` tag in our markup.
2. In` entry.client.tsx` , we again initialize an instance of` ApolloClient` , this time with a` uri` and` InMemoryCache` that uses the` restore` function to rehydrate the browser’s cache value with that of the server’s via the` window.__APOLLO_STATE__` global object. Then we wrap the` RemixBrowser` component in an` ApolloProvider` and pass in our` client` .
3. Finally, we can make GraphQL requests using Apollo Client’s hooks, like` useQuery` , in our Remix page` routes` and components! From here, usage of the Apollo Client hooks remains the same as any other SSR React app, with network requests being sent on the server instead of on the browser.


** **Note:** Using Apollo Client with Remix results in using Apollo Client’s hooks in lieu of Remix’s` loader` and` action` functions. Network requests are still sent on the server, but following the Apollo Client API, not the Remix APIs, because Apollo Client is a client-side library.


You can find the code used in this post on[GitHub](https://github.com/jgarrow/remix-apollo) .


Want to spin up a Remix app with Apollo Client already set up? Use the[GitHub repo](https://github.com/jgarrow/remix-apollo) for the code from this post as a[Remix stack](https://remix.run/docs/en/v1/pages/stacks) template:


```text
npx create-remix@latest --template jgarrow/remix-apollo
```


Written by


Janessa Garrow


[Read more by Janessa Garrow](https://www.apollographql.com/blog/author/janessagarrow)
