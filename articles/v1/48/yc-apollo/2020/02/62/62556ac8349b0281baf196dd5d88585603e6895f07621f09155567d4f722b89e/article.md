---
schema_version: "1.0.0"
document_id: "62556ac8349b0281baf196dd5d88585603e6895f07621f09155567d4f722b89e"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-to-query-on-click"
published_at: "2020-02-24T18:54:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:00be8df896a4c146e2e58e58e6d10fa13896d63ba1bf2ae92dfef2386b710925"
---

# Apollo Client [React]— How to Query on Click

Sometimes, you don’t want to fetch data right away- but instead, you’d like to fetch data manually in response to some sort of event, like a button click!


In this quick article, I’ll show you how to use the` useLazyQuery` hook to manually trigger client-side GraphQL queries.


## Project setup & demo


We’re using Apollo Client 3 with React Hooks, and[Trevor Blades](https://twitter.com/trevorblades) ’[Countries API](https://github.com/trevorblades/countries) .


You can find the entirety of the code in a Codesandbox here:[https://codesandbox.io/s/apollo-client-uselazyquery-example-6ui35](https://codesandbox.io/s/apollo-client-uselazyquery-example-6ui35)


## Fetching data in response to a click event


Here’s the relevant code sample, we import` useLazyQuery` from` @apollo/client` , write a` GET_COUNTRIES` query, and define the` getCountries` function that we use to invoke the query.


```text
// src/Countries.js
import React from "react";
import { useLazyQuery } from "@apollo/client";
import gql from "graphql-tag";


const GET_COUNTRIES = gql`
{
countries {
code
name
}
}
`;


export function DelayedCountries() {
const [
getCountries,
{ loading, data }
] = useLazyQuery(GET_COUNTRIES);


if (loading) return <p>Loading ...</p>;
if (data && data.countries) {
console.log(data.countries);
}


return (
<div>
<button onClick={() => getCountries()}>
Click me to print all countries!
</button>
{data &&
data.countries &&
data.countries.map((c, i) => <div key={i}>{c.name}</div>)
</div>
);
}
```


When the page first loads, it should look pretty empty as shown below.


A page with a button that, when pressed, will invoke a query.


But when you click the button, it should run the` GET_COUNTRIES` query and populate the` data` variable with a` countries` object containing the result of our query.


We can see that it worked because we’re presented with all the country names!


A mapped list of countries fetched after calling the GET_COUNTRIES query in response to a button click.


That’s it! That’s how to execute queries manually with` @apollo/client` .


—


For a more detailed explanation on how to use` useLazyQuery` , check out[the docs](https://www.apollographql.com/docs/react/data/queries/#executing-queries-manually) on[Executing Queries Manually](https://www.apollographql.com/docs/react/data/queries/#executing-queries-manually) .


Written by


Khalil Stemmler


[Read more by Khalil Stemmler](https://www.apollographql.com/blog/author/khalil)
