---
schema_version: "1.0.0"
document_id: "0503a7cb74bbd88e8e286c3f30b81b4e8ea936d54d5bbe8b73b35698330becb5"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/introducing-react-apollo-2-1-c837cc23d926"
published_at: "2018-03-22T21:13:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:126bc615e768350bdfcadcca9dd0123c717409e7aa0e69c9ad5e02b0c60489af"
---

# Introducing React Apollo 2.1

The wait is over! We’re super excited to announce **React Apollo 2.1** , a huge step forward in improving how you develop React applications with GraphQL. It features a new render prop API and[much stronger TypeScript support](https://dev-blog.apollodata.com/whats-next-for-react-apollo-4d41ba12c2cb) , along with upgraded docs. It’s also 100% backwards compatible!


From the very beginning, React Apollo was designed to make building React apps with Apollo Client a delightful and intuitive experience. For a long time, this meant connecting data to your UI with higher order components. With React Apollo 2.1, using GraphQL data in your React app is now easier than ever thanks to our brand new components:` Query` ,` Mutation` , and` Subscription` .


We’ve been working with these components exclusively for the past three months and have been amazed not only by how much they’ve improved our productivity, but also by how fun they are to build. Read on to learn more! 🚀


## Data fetching with Query


Fetching data in a simple, declarative way is one of the core features of Apollo Client. We’re excited to improve upon that feature with our new` Query` component, which uses the render prop pattern to update your UI with your GraphQL data.


```text
import React from "react";
import { Query } from "react-apollo";
import gql from "graphql-tag";


const GET_TODOS = gql`
{
todos {
id
type
}
}
`;


const Todos = () => (
<Query query={GET_TODOS}>
{({ loading, error, data }) => {
if (loading) return <p>Loading...</p>;
if (error) return <p>Error :(</p>;


return data.todos.map(({ id, type }) => (
<p key={id}>{type}</p>
));
}}
</Query>
);
```


To create a` Query` component, just pass a GraphQL query to the` query` prop and provide a function as its child that returns the UI you want to render. This function, commonly referred to as a[render prop function](https://reactjs.org/docs/render-props.html) , is called with your GraphQL query’s` loading` and` error` state, in addition to your` data` when it arrives. For a full list of properties on the render prop function, check out our[updated API overview](https://www.apollographql.com/docs/react/essentials/queries.html#api) .


The render prop API allows for much greater flexibility than our old` graphql` higher order component, unlocking features such as dynamically choosing a query based on props and easily composing multiple queries. While the higher order component API is still useful, we think the new API more accurately reflects how developers are building React apps today.


## Updating data with Mutation


If you thought fetching data with the` Query` component was cool, just wait until you try updating data with the new` Mutation` component!` Mutation` also uses the render prop pattern to pass down a function to trigger a mutation from your UI, in addition to the state of your mutation and any results.


```text
import React from "react";
import { Mutation } from "react-apollo";
import gql from "graphql-tag";


const TOGGLE_TODO = gql`
mutation ToggleTodo($id: Int!) {
toggleTodo(id: $id) {
id
completed
}
}
`;


const Todo = ({ id, text }) => (
<Mutation mutation={TOGGLE_TODO} variables={{ id }}>
{(toggleTodo, { loading, error, data }) => (
<div>
<p onClick={toggleTodo}>
{text}
</p>
{loading && <p>Loading...</p>}
{error && <p>Error :( Please try again</p>}
</div>
)}
</Mutation>
);
```


In this example,` toggleTodo` is our mutate function that tells Apollo Client to fire off the mutation. We also receive information about its` loading` and` error` state, as well as any` data` returned. This is a vast improvement over creating mutation containers with the` graphql` higher order component because you no longer have to manually track the state of your mutation. We’ve also provided` onError` and` onCompleted` props to the` Mutation` component if you’d like to register any callbacks.


## Simplifying local state with ApolloConsumer


React Apollo 2.1 introduces a new component, the` ApolloConsumer` , that allows you to easily access your Apollo Client instance directly via the render prop function. You can think of the` ApolloConsumer` component as similar to the` Consumer` component from the[new React context API](https://github.com/reactjs/rfcs/blob/master/text/0002-new-version-of-context.md) .


One of the benefits of the` ApolloConsumer` component is that it drastically reduces the amount of code you need to write in order to perform simple local mutations with` apollo-link-state` . For one-off mutations like writing a string to the cache, there’s no need to set up a resolver or a GraphQL mutation! In your` ApolloConsumer` render prop function, just call` writeData` directly from your Apollo Client instance.


```text
import React from "react";
import { ApolloConsumer } from "react-apollo";


import Link from "./Link";


const FilterLink = ({ filter, children }) => (
<ApolloConsumer>
{cache => (
<Link
onClick={() => cache.writeData({ data: { visibilityFilter: filter } })}
>
{children}
</Link>
)}
</ApolloConsumer>
);
```


You can also use the` ApolloConsumer` component for[firing queries manually](https://www.apollographql.com/docs/react/essentials/queries.html#manual-query) in response to a user action, like a button click, or for prefetching data.


## New docs & tutorials


With the React Apollo 2.1 release, we’re thrilled to finally share our revamped[docs site](https://www.apollographql.com/docs/react/) , now with search and more tutorials! All of our docs improvements were specifically built with the community in mind, thanks to all the feedback you’ve shared with us on Twitter and Slack.


Not only did we refactor the code samples to` Query` and` Mutation` components, we also refactored the left-hand navigation to make core features like pagination and local state management more discoverable. In case you already know what you’re looking for, we reenabled search across all Apollo docs, thanks to[Jesse Rosenberger](https://medium.com/u/54c8047f8f10?source=post_page-----c837cc23d926----------------------) .


For this release, we wanted to ensure that the getting started experience with Apollo Client was approachable and interactive, so we built a new essentials section from the ground up. The[Apollo essentials](https://www.apollographql.com/docs/react/essentials/get-started.html) section covers everything you need to start building your first` Query` and` Mutation` components quickly without overwhelming you with information. They feature tutorials on[Apollo Boost](https://dev-blog.apollodata.com/zero-config-graphql-state-management-27b1f1b3c2c3) ,` Query` components,` Mutation` components, and[Apollo Link State](https://dev-blog.apollodata.com/the-future-of-state-management-dd410864cae2) accompanied by interactive examples on[CodeSandbox](https://codesandbox.io/) .


We know that many of you will be migrating to the new render props API incrementally, so we’ve made the[old docs](https://www.apollographql.com/docs/react/api/react-apollo.html#graphql) for the` graphql` higher order component available within the React Apollo API section.


## Contributors


We’d like to thank[Kevin Ross](https://medium.com/u/2e2136fb7ed0?source=post_page-----c837cc23d926----------------------) ,[Dirk-Jan Rutten](https://medium.com/u/b016d103ae51?source=post_page-----c837cc23d926----------------------) , and[Leonardo A Garcia C](https://medium.com/u/82263c594756?source=post_page-----c837cc23d926----------------------) , who tirelessly worked to help write, test, and improve the new render props API. We’re really happy with how flexible the` Query` ,` Mutation` , and` Subscription` components are, and we can’t wait to see what all of you build with them!


For the past two months, many of our community members were brave enough to test out the beta version of React Apollo 2.1. We’d like to give a shoutout to all our early testers, especially content creators such as[Wes Bos](https://medium.com/u/86a55cd7983b?source=post_page-----c837cc23d926----------------------) and[Sara Vieira](https://medium.com/u/b384aa92a080?source=post_page-----c837cc23d926----------------------) , for your invaluable feedback on how to refine and teach the new components.


Finally, I’d like to give a special thanks to all of the Apollo team members who were involved in shipping this release, especially[James Baxley III](https://medium.com/u/fd3d3dea3cd3?source=post_page-----c837cc23d926----------------------) for his leadership over the project. It’s been awesome to see your vision for React Apollo evolve over the past two years.


## What’s next 🚀


Our work isn’t done yet! We’re constantly looking to iterate upon the best way to build React apps with GraphQL. Here’s a sneak peek of some of the things on our mind that should be coming to React Apollo in the future:


- Today, you can manually fire queries either for prefetching or in response to a user action by creating an` ApolloConsumer` component, but you have to handle your own loading and error state. Soon, we’re looking to make **manually firing queries** a first-class feature in the` Query` component by adding a new` delay` prop and` load` action to the render prop function.
- We’re eagerly anticipating the release of React 16.3 and its awesome features like the new context API. Once it arrives, we will release a new major version of React Apollo that’s 100% compatible with **async strict mode** and all of the latest features.
- Beyond React 16.3, we can’t wait to start experimenting further with[integrating React Suspense into React Apollo](https://dev-blog.apollodata.com/a-first-look-at-async-react-apollo-10a82907b48e) . We’ve already released a first proof of concept, and are curious to see where it goes!


As always, we would love to hear your feedback about React Apollo 2.1 and the direction of the project. We hope you love it as much as we do! ❤️


Written by


Peggy Rayzis


[Read more by Peggy Rayzis](https://www.apollographql.com/blog/author/peggy)
