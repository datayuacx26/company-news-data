---
schema_version: "1.0.0"
document_id: "ef9814fe73c09f77c084efdf2ebc509b495f18f0fac459b1c4286db570bb66ee"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/next-js-getting-started"
published_at: "2021-03-09T14:52:25+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:f36bd9232bc8d81917eb1bc182de81d3fe2200aaab25c8945e33dfea65391540"
---

# Getting Started With Apollo Client in Next.js

(Update May 2023) We published a library that makes it easier to use[Apollo Client with Next.js 13](https://www.apollographql.com/blog/announcement/frontend/using-apollo-client-with-next-js-13-releasing-an-official-library-to-support-the-app-router/) , this guide still remains valid if you’re using the` pages` directory.


**Wait, you want me to put my data where!?** Figuring out how to configure Apollo Client with Next.js can be a bit confusing. This confusion stems from the fact that there are three ways to fetch and render data: static, server-side, and client-side. There are benefits and tradeoffs to each strategy, but I’m happy to report that Apollo Client is compatible with all three!


In this post, we’ll briefly cover each way to fetch and render data with Next.js, along with the important considerations for each approach. We’ll wrap up with a demonstration of how to configure Apollo Client for each one.


If you’re unfamiliar with how pages are rendered in Next.js, check out[these guides on pre-rendering](https://nextjs.org/learn/basics/data-fetching/pre-rendering) and the[two types of pre-rendering](https://nextjs.org/learn/basics/data-fetching/two-forms) Next.js supports.


## A quick comparison of data fetching options in Next.js


### Static Rendering


With static rendering, your pages are generated at build time and then served as static HTML files. This method provides speedy response times because the pages don’t need to be rendered for each request. SEO also works as expected for statically rendered pages because the content is readily available for crawlers. However, statically rendered content can only be updated by deploying a new version of your app. This means that relative to other methods, your content is more likely to become stale between deploys.


### Server-side Rendering


With server-side rendering, pages are generated dynamically for each incoming request. This enables content to be updated between deploys (unlike static rendering), and SEO works as expected. However, rendering on every request does add processing time that can noticeably increase response latency.


### Client-side Rendering


With client-side rendering (the typical strategy for React apps), the browser requests the app, the page loads, then React requests the data to present to the user. This strategy keeps pages fresh and responsive, but it isn’t compatible with SEO crawlers.


## Creating a New Next.js App


Now that we understand our options let’s start setting up an app to try them out.


To create a new Next.js application, we’ll use create-next-app:


```text
npx create-next-app next-with-apollo
```


You can change` next-with-apollo` to be any project name, have fun with it, or choose the name of that project you want to build. 🙂


With the app scaffolded and dependencies installed, we can now run the application:


```text
cd next-with-apollo
npm run dev
```


Navigating to[http://localhost:3000](http://localhost:3000/) will open the running Next.js app in your browser!


## Setting Up Apollo Client


Now that the app is created, it’s time to add Apollo Client and connect it to an API. In this example, we’ll use the[Countries API](https://github.com/trevorblades/countries) from Trevor Blades to display country codes and flags for each country.


The complete code can be[found here](https://github.com/kkemple/nextjs-with-apollo) .


```text
npm install @apollo/client graphql
```


With our dependencies in place, it’s now time to set up the client in our application. Create a new file in the root of the project called` apollo-client.js` and add the following contents to it:


```text
// ./apollo-client.js


import { ApolloClient, InMemoryCache } from "@apollo/client";


const createApolloClient = () => {
return new ApolloClient({
uri: "https://countries.trevorblades.com",
cache: new InMemoryCache(),
});
};


export default createApolloClient;
```


Now we have a GraphQL client we can use within the application, and we’re ready to query for some data. However, before we can query for anything, we first need to know what data is available to us from the Countries API.


## Exploring the API


We’ll use the[Apollo Explorer](https://studio.apollographql.com/dev) , a powerful GraphQL IDE, to work with the Countries API. The Explorer connects to a graph and gives you a workspace to build, run, and save queries.


If you don’t already have an account, you can create one using your GitHub account. When you’re in, you’ll want to click the “New Graph” button at the **top right** .


Next, you’ll be prompted to give your new graph a **title** and choose a **graph type** . You’ll also want to change the **endpoint** to` https://countries.trevorblades.com` .


Once the graph is created, Apollo Studio will drop you into the Apollo Explorer.


Replace the example query in your *Operations window* with the following query.


```text
query ExampleQuery {
countries {
code
name
}
}
```


And if we run the example by clicking the **blue play button** , we should get a JSON result back like this:


```text
"data": {
"countries": [
{
"code": "AD",
"name": "Andorra"
},
{
"code": "AE",
"name": "United Arab Emirates"
},
{
"code": "AF",
"name": "Afghanistan"
},
{
"code": "AG",
"name": "Antigua and Barbuda"
},
...
]
}
```


**BOOM!** Now we’re ready to jump back into the application and start fetching data!


## Using Apollo Client for statically rendered page data


Before we can fetch data from the API, we have to set up our page. We do this for statically generated pages using the` getStaticProps` method. Next.js will use this function during the build process to get any data needed to be passed into the page component as props.


Because we’re using the methods provided by Next.js to fetch the data, we’re not going to be calling our API from within React itself. So we’ll use Apollo Client directly instead of using hooks.


Inside of` pages/index.js` import the client we created in` apollo-client.js` and` gql` from` @apollo/client` .


```text
// pages/index.js


import { gql } from "@apollo/client";
import createApolloClient from "../apollo-client";
```


Then add the following code below the React component:


```text
// pages/index.js


export async function getStaticProps() {
const client = createApolloClient();
const { data } = await client.query({
query: gql`
query Countries {
countries {
code
name
emoji
}
}
`,
});


return {
props: {
countries: data.countries.slice(0, 4),
},
};
}
```


This will fetch the **code** , **name** , and **emoji** for each country, and then we pass along the first four results to the React component via the` countries` prop. Now that we have the data available, it’s time to update the component to show the page’s data.


To use our countries data, we need to update the React component with the` countries` prop.


```text
// pages/index.js


export default function Home({ countries }) {
...
```


With the countries available in the component, we can replace the page’s grid with the following code:


```text
// pages/index.js


<div className={styles.grid}>
{countries.map((country) => (
<div key={country.code} className={styles.card}>
<h3>{country.name}</h3>
<p>
{country.code} - {country.emoji}
</p>
</div>
))}
</div>
```


The page now includes the four countries we passed in from the` getStaticProps` method! Pretty awesome!


It’s important to note that Next.js will fetch the data at build time and, therefore, be stale.


## Using Apollo Client for server side rendered page data


Fetching data for server-side generated pages is done using the` getServerSideProps` method provided by Next.js. This function will be used during each page request to get any data passed into the page component as props.


Fetching page data for server-side rendered pages isn’t very different from what we did for statically generated pages. In fact, it’s so similar we can use almost all of the work we just did.


Duplicate` ./pages/index.js` and rename it to` server-side.js` . Then change the name of the function that fetches the data from` getStaticProps` to` getServerSideProps` .


```text
// pages/server-side.js


export async function getServerSideProps() {
const client = createApolloClient();
const { data } = await client.query({
query: gql`
query Countries {
countries {
code
name
emoji
}
}
`,
});


return {
props: {
countries: data.countries.slice(0, 4),
},
};
}
```


Everything else stays the same! 🎉


The major difference here is that Next.js will fetch the country codes in this example for each page request instead of only at build time.


## Using Apollo Client for client-side data


Client-side rendering is what we typically do in React apps. The browser requests the app, the page loads, then React requests the data and presents it to the user.


Setting up Apollo Client for use on the client side takes a few additional steps. Because we’ll be in React and want to use hooks, we’ll need to use ApolloProvider. Open up ./pages/_app.js and import ApolloProvider from @apollo/client, as well as the client we created earlier.


```text
// pages/_app.js


import { ApolloProvider } from "@apollo/client";
import createApolloClient from "../apollo-client";
```


Now replace the component in _app.js with the following one:


```text
// pages/_app.js


const client = createApolloClient();
function MyApp({ Component, pageProps }) {
return (
<ApolloProvider client={client}>
<Component {...pageProps} />
</ApolloProvider>
);
}
```


With the provider wrapping our root component, we can now use hooks anywhere within our app, but we aren’t quite done yet. If we start using hooks within our app now, we’ll end up making requests to our API during page rendering (either at build time or serve time, depending on how the page is configured). This isn’t ideal because the page will be generated before the requests can return, and Next.js can pass the data to the component.


Create a new directory called` components` in the project’s root and then a file inside that folder called` ClientOnly.js` .


Copy the following code into the new file:


```text
// components/ClientOnly.js


import { useEffect, useState } from "react";


export default function ClientOnly({ children, ...delegated }) {
const [hasMounted, setHasMounted] = useState(false);


useEffect(() => {
setHasMounted(true);
}, []);


if (!hasMounted) {
return null;
}


return <div {...delegated}>{children}</div>;
}
```


To make sure we only request data from the browser, we have to ensure that the components using hooks are only rendered on the client. We can accomplish this by creating a component that only renders its children in the browser and not on the server.


For more on this pattern, check out[this post](https://www.joshwcomeau.com/react/the-perils-of-rehydration) by Josh Comeau.


Next, create another file in the` components` directory called` Countries.js` and add the following code:


```text
// components/Countries.js


import { useQuery, gql } from "@apollo/client";
import styles from "../styles/Home.module.css";


const QUERY = gql`
query Countries {
countries {
code
name
emoji
}
}
`;


export default function Countries() {
const { data, loading, error } = useQuery(QUERY);


if (loading) {
return <h2>Loading...</h2>;
}


if (error) {
console.error(error);
return null;
}


const countries = data.countries.slice(0, 4);


return (
<div className={styles.grid}>
{countries.map((country) => (
<div key={country.code} className={styles.card}>
<h3>{country.name}</h3>
<p>
{country.code} - {country.emoji}
</p>
</div>
))}
</div>
);
}
```


This component is responsible for fetching the countries and will be wrapped in the` ClientOnly` component we created previously.


Finally, create a new file in the` pages` directory called` client-side.js` and add the following code:


```text
// pages/client-side.js


import Head from "next/head";
import styles from "../styles/Home.module.css";
import ClientOnly from "../components/ClientOnly";
import Countries from "../components/Countries";


export default function ClientSide() {
return (
<div className={styles.container}>
<Head>
<title>Create Next App</title>
<link rel="icon" href="/favicon.ico" />
</Head>


<main className={styles.main}>
<h1 className={styles.title}>
Welcome to <a href="https://nextjs.org">Next.js!</a>
</h1>
<ClientOnly>
<Countries />
</ClientOnly>
</main>


<footer className={styles.footer}>
<a
href="https://vercel.com"
target="_blank"
rel="noopener noreferrer"
>
Powered by{" "}
<img src="/vercel.svg" alt="Vercel Logo" className={styles.logo} />
</a>
</footer>
</div>
);
}
﻿
```


This page is very similar to the other two pages, except that it has no` getStaticProps` or` getServerSideProps` methods because we’ll fetch the data once the page is rendered in the browser.


And that’s it! 👏


Now we have client-side data fetching in place!


## Conclusion


Next.js provides data fetching methods in both statically rendered and server-side rendered pages through the` getStaticProps` and` getServerSideProps` methods. We used Apollo Client directly to query the countries API when working in client-side rendering mode.


On the client, we wrapped the root component in` ApolloProvider` . We used a custom component to ensure we only request the countries from the client to avoid sending duplicate requests during page rendering.


In a future post, we’ll look at how we can mix these data-fetching patterns to handle more advanced use cases.


Written by


Kurt Kemple


[Read more by Kurt Kemple](https://www.apollographql.com/blog/author/kurtkemple)
