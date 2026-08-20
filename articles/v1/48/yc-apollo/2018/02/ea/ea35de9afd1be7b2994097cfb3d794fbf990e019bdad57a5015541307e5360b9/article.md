---
schema_version: "1.0.0"
document_id: "ea35de9afd1be7b2994097cfb3d794fbf990e019bdad57a5015541307e5360b9"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/zero-config-graphql-state-management"
published_at: "2018-02-15T21:49:13+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:53.665310+00:00"
content_hash: "sha256:9c08ffea47a75b85d6f4c44575348e705c976c3c1b379680c90d178e307dc79c"
---

# Zero-config GraphQL state management

Recently, there have been a lot of exciting developments happening in the GraphQL client space. Some of these new clients, such as[urql](https://github.com/FormidableLabs/urql) and[micro-graphql-react](https://github.com/arackaf/micro-graphql-react) , were partially born from the idea that Apollo should be easier to get started with, especially for beginners. We’ve taken this constructive feedback to heart — that’s why today, we’re announcing` <a href="https://github.com/apollographql/apollo-client/tree/master/packages/apollo-boost" target="_blank" rel="noreferrer noopener">apollo-boost</a>` , an easy-to-use booster kit for Apollo Client with our recommended configuration. 🚀 We’re really hyped about making GraphQL easier to learn with Apollo Boost and can’t wait to hear what you all think!


As of today, here’s how simple it is to get started with Apollo Client:


```text
import ApolloClient from 'apollo-boost';const client = new ApolloClient();
```


The longer you work on an open source project, the tougher it is to put yourselves in the shoes of someone who’s discovering the project for the first time. As library authors, we want to work harder at this. Our main goal is to make GraphQL more approachable and powerful for everyone. For us, building Apollo Boost was an awesome chance for us to focus on empathy, and we’re really happy with the outcome. 🤗


I encourage all of you to take a step back and practice empathy building your software too. At the same time, continue to provide valuable feedback toward the open source projects you love.


## Back to our roots


From the beginning, the Apollo team has always strived to design tools that were simple to get started with, so you could send your first query right away. While Apollo Client 2.0’s modular architecture unlocked new possibilities for experienced users to customize their cache and network stack, it also deviated somewhat from our original goal of simplicity. First impressions are everything, and it’s important to us that beginners feel comfortable working with GraphQL. That’s why we’re returning back to our roots with` apollo-boost` !


If you’ve been with us since the 1.0 days, you’ll notice that the` ApolloClient` exported from` apollo-boost` looks familiar. With Apollo Boost, you no longer have to manually connect your cache and network stack to the client. We’ve already set up` InMemoryCache` and` HttpLink` for you by default. No heavy lifting required. 💪


## Get started


Creating a client with Apollo Boost requires zero config to get started. We’ve included some packages that we think are essential for building an Apollo app, like` apollo-link-error` and` apollo-link-state` . You can activate these features by specifying a few options on the` ApolloClient` constructor and we’ll set them up for you behind the scenes.


Let’s dive in! First, you’ll want to install the following packages. We’re going to install the new[React Apollo 2.1 beta](https://dev-blog.apollodata.com/whats-next-for-react-apollo-4d41ba12c2cb) so we can take advantage of Query components.


```text
npm i apollo-boost react-apollo@beta graphql -S
```


Next, create your client and hook it up to your app by passing it to the` ApolloProvider` exported from` react-apollo` .


```text
import React from 'react';
import { render } from 'react-dom';
import ApolloClient from 'apollo-boost';
import { ApolloProvider } from 'react-apollo';


// Pass your GraphQL endpoint to uri
const client = new ApolloClient({
uri: 'https://nx9zvp49q7.lp.gql.zone/graphql'
});


const ApolloApp = () => (
<ApolloProvider client={client}>
<App />
</ApolloProvider>
);


render(ApolloApp, document.getElementById('root'));
```


Now that our app is up and running, let’s make our first query:


```text
import React from 'react';
import { gql } from 'apollo-boost';
import { Query } from 'react-apollo';


const GET_DOG = gql`
query {
dog(breed: "bulldog") {
id
displayImage
}
}
`


export const App = () => (
<Query query={GET_DOG}>
{({ loading, error, data }) => {
if (loading) return <div>Loading...</div>;
if (error) return <div>Error :(</div>;


return <img src={data.dog.displayImage} />
}}
</Query>
)
```


That’s it! If you need more customization, there are only six optional config properties you can pass to the Apollo Boost client:


- ` uri` : Your GraphQL endpoint (defaults to` /graphql` )
- ` fetchOptions` : Static fetch options for every request
- ` request` : A function called every request that receives the operation. Here, you can implement authentication easily by dynamically setting headers on the context.
- ` onError` : An error handler function (our default logs out errors for you)
- ` clientState` : Your local state config for` <a href="https://www.apollographql.com/docs/link/links/state.html" target="_blank" rel="noreferrer noopener">apollo-link-state</a>`
- ` cacheRedirects` : Functions for redirecting queries to another entry in the cache (previously called cache resolvers)


### Advanced migration


If you’re looking to integrate custom links into your Apollo Link stack that aren’t already included in Apollo Boost, migration is simple. Apollo Boost exports some of the building blocks of Apollo Client, like` HttpLink` and` InMemoryCache` , for you so you don’t have to reinstall all of the individual packages. In the future, we’d love to have an eject feature that writes your client setup for you automatically.


```text
import { ApolloClient } from 'apollo-client';
import { HttpLink, InMemoryCache, ApolloLink } from 'apollo-boost';


const client = new ApolloClient({
link: ApolloLink.from([ myCustomLink, new HttpLink() ]),
cache: new InMemoryCache()
});
```


## 🔥 New 🔥


In addition to Apollo Boost, we’ve also made some improvements to the docs, along with a new example app on CodeSandbox.


Choose your favorite pup!


- [Pupstagram](https://codesandbox.io/s/r5qp83z0yq) **:** Our new example app 🐶 (aka what I wish my Instagram feed was). It features Apollo Boost, local state management with` apollo-link-state` , and more! For the GraphQL server, check out our[Launchpad](https://launchpad.graphql.com/nx9zvp49q7) .
- [Quick Start](https://www.apollographql.com/docs/react/essentials/get-started.html) **:** Go from zero to your first queries & mutations in less than 10 minutes with Apollo Boost.
- [Why Apollo](https://www.apollographql.com/docs/react/why-apollo.html) **:** Wondering why we normalize our cache? Need a document to convince your team to switch to Apollo Client? It’s all right here.


---


With the release of Apollo Boost, we’ve sunset the` apollo-client-preset` proof of concept which served as inspiration. For backwards compatibility, everything that was included in the old preset is included in` apollo-boost` . Simply change your import and you should be good to go! Thanks to everyone who helped test out` apollo-client-preset` .


## Inspiration


Apollo Boost was largely inspired by the simplicity of[urql](https://formidable.com/blog/2018/introducing-urql/) , the newest GraphQL client on the block. We’d like to give a big thank you to[Ken Wheeler](https://medium.com/u/e50c25098d37?source=post_page-----27b1f1b3c2c3----------------------) and the team at Formidable Labs for challenging us to rethink how we could improve the developer experience for our users. We’re grateful for the opportunity to learn from your work.


It’s super exciting to see newcomers popping up in the GraphQL ecosystem because we all get to build upon each other’s successes. I firmly believe that competition is healthy for open source projects to keep innovating. As long as more developers are getting hyped about GraphQL and how it can improve their team’s productivity, we welcome anyone entering the space.


Finally, we’d also like to thank all of you! Our team loves hearing honest feedback from the community, whether it’s positive or constructive. If you have any ideas on how we can make your experience with Apollo better, just drop us a message on Twitter or Slack.


Written by


Peggy Rayzis


[Read more by Peggy Rayzis](https://www.apollographql.com/blog/author/peggy)
