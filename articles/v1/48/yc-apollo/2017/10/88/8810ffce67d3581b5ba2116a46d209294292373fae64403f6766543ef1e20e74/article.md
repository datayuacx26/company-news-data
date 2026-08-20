---
schema_version: "1.0.0"
document_id: "8810ffce67d3581b5ba2116a46d209294292373fae64403f6766543ef1e20e74"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-client-2-0-5c8d0affcec7"
published_at: "2017-10-25T23:35:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:54.348132+00:00"
content_hash: "sha256:96e4ca65428c289600f6b113727002dabd7e2b2449bd598b4a3521f5e8b10bf0"
---

# Apollo Client 2.0

[Apollo Client](https://github.com/apollographql/apollo-client) is the ultra-flexible, community-driven GraphQL client for React, Vue.js, Angular, and other JavaScript platforms. You just describe your data requirements with a GraphQL query, and Apollo Client fetches and manages the data for you.


It’s used in production by companies like KLM, IBM, Intuit, and more, and is built with contributions from engineers at companies like Coursera, Convoy, and even Microsoft! With over 2 million downloads so far, Apollo Client is the most popular way to use GraphQL in your app.


## Universal GraphQL


Apollo Client brings an entire new world of data management to your application. Much like React is the cornerstone of your UI, Apollo Client can be the cornerstone of how you work with data. Apollo Client 2.0 is specially designed to enable you to bring the benefits of GraphQL to all of your data management needs.


### A small core


The core of Apollo Client is only 12kb. When you add in all of the parts you need for a complete application, we expect most apps to see around a 30kb total bundle size for all of their data needs. We are committed to keeping the library small and enabling people to get the most out of GraphQL with a minimal footprint.


### Modular and extensible


Apollo Client allows you to include only what you need for your application. This modular approach also makes it much easier to add new functionality to your data system. This gives your app room to grow in complexity, without needing to switch to a new library.


### A new way to resolve data


One of the most important features of Apollo Client 2.0 is the move to a network layer powered by observables, instead of promises. This is a subtle change that opens the door to an entire new feature set for GraphQL. Apollo Client can handle multiple results from operations, enabling use cases such as live queries.[Apollo Link](https://github.com/apollographql/apollo-link) , this new network stack, makes it possible to make practically any data source into something that can be used with GraphQL.


## The Future of GraphQL: What Apollo 2.0 will enable


Once you’ve learned GraphQL, managing data in other ways feels like a step back in both development experience and functionality. The ability to describe the data you want to read or update, and then having that just work, makes GraphQL a natural way to manage your application.


With Apollo Client 2.0, we aim to expand on those ideas and build towards a world where all of your application’s data can be managed via GraphQL. Here are some of the ideas we’ve been working on:


### Stream your data


Using[Apollo Link](https://github.com/apollographql/apollo-link) as the network layer for the new version is an opportunity to open the door to the future of GraphQL.[Lee Byron](https://medium.com/u/d9b1a61823fa?source=post_page-----5c8d0affcec7----------------------) outlined what this could look like in his[talk](https://dev-blog.apollodata.com/new-features-in-graphql-batch-defer-stream-live-and-subscribe-7585d0c28b07) at React Europe in 2016 with the introduction of things like @defer, @live, and @stream. These directives allow a new interaction with GraphQL servers and can make your application more performant and more powerful at the same time. Support for these is now possible with Apollo Link, which can accept multiple results from an operation and can handle the partial result processing on its own, keeping the core of Apollo Client small.


### Client state and REST join the fun


Oftentimes, moving to GraphQL requires coordination of multiple teams in your organization. The front-end team needs to introduce a client and refactor the previous data management to use GraphQL, while the back-end team needs to write an endpoint most frequently on top of the existing REST backend when starting out. Sometimes you may not even control the backend, or a portion of it, at all. Along with these challenges, after moving to Apollo, an application will still have the local application state it needs to manage in a totally different paradigm (like redux or mobx).


In the very near future, you will be able to install two new links which will make these problems disappear!` apollo-link-state` and` apollo-link-rest` will support using directives in your operations to redirect requests to client side data and REST endpoints respectively. Instead of using a global state library, you can write GraphQL to read and update your application state like so:


```text
const USER_QUERY = gql`
query GetUserData {
user {
firstName
deviceType @client
}
}
`;


export default graphql(USER_QUERY)(({ data }) => {
if (data.loading) return <div>loading...</div>;
return (
<h1>{data.user.firstName} is using {data.user.deviceType}</h1>
)
}
```


This will simplify the learning curve for your team and make it easier to add new features to your application!


Moving to GraphQL from REST takes time, and in some cases may not even be possible. In order to make it possible to use Apollo with your existing data endpoints,` apollo-link-rest` provides a way to use GraphQL to hit REST endpoints on the client:


```text
const USER_QUERY = gql`
query RestData($email: String!){
user(email: $email) @rest(route: '/users/email/:email', email: $email) {
id
firstName
friends @rest(route: '/users/friends/:id') @provides(vars: ['id']) {
firstName
}
}
`;


export default graphql(USER_QUERY)(({ data }) => {
if (data.loading) return <div>loading...</div>;
return (
<div>
<p>{data.user.firstName} has {data.user.friends.length} friends:</p>
<ul>
{data.user.friends.map(({ firstName }) => (
<li>{firstName}</li>
)}
</ul>
</div>
)
}
```


While not as performant as an actual GraphQL endpoint, this can make it easy to get started with Apollo immediately and to work with endpoints you may not control, while still using the data library you love. You can even mix and match REST, client, and GraphQL data in a single query.


## A call to experiment


The JavaScript community is full of incredible innovation and experimentation. Tools like Babel and libraries like React have opened the door to new ideas and are changing the way applications are built. We think GraphQL should be able to usher in a similar revolution in how data is managed and Apollo Client offers powerful ways to experiment and tune your client to the needs of your application.


### Better native and PWA support


Building a react-native app with Apollo Client 2.0 can be an incredible experience for developers and users alike. Custom links can be written to communicate across the bridge to access native stores and networking capabilities. Offline support will be much easier to manage as well with a future` apollo-link-offline` which will collect operations while the user has no network connection and replay them when the app comes back online. This will also make creating a progressive web app (PWA) with Apollo as easy as installing an npm package.


## Keeping it simple


All of these features could make Apollo Client justifiably hard to learn and use. That would be stepping away from one of the core principles of the project which is that **Apollo will always be simple to get started with** .


We have also rewritten a large portion of the documentation for using Apollo to focus around the features, best practices, and recipes you actually need in your application. With a reference index for APIs and detailed use cases in the docs, we think its the easiest way to get started and improve your knowledge of what can be done. These docs are always improving, and now each pull request to an Apollo project will have its own deployed version of the docs for review and testing.


We value the time you have spent investing in using Apollo and worked very hard to make the 2.0 as small of a breaking change as possible. All of your application code should work 100% the way it used to, but now will be faster and can grow to do more than ever before.


## Production-ready


It takes a lot for us to be comfortable with shipping a major change that can affect the users of your app. We have been using the 2.0 in our internal projects throughout the release process, as well as testing with contributors, and we’re thrilled with the results.


The[developer tools](https://chrome.google.com/webstore/detail/apollo-client-developer-t/jdkknkkbebbapilgoeccciglkfbmbnfm?hl=en) have been updated to work with the 2.0 and we plan a whole new set of features to make getting your app ready to ship easier.


And if you haven’t seen the[announcement from yesterday](https://dev-blog.apollodata.com/apollo-engine-and-graphql-error-tracking-e7dd3ce8b99d) , we’re also proud to offer[Apollo Engine](https://www.apollographql.com/engine/) , which helps you productionize your GraphQL API.


---


This release would not be possible without all of the incredible work and support from the amazing Apollo community! Thank you to each and every person who has contributed, given feedback, and pitched crazy ideas informing the future of GraphQL.


I want to give a special shout out to the Apollo team:[Evans Hauser](https://medium.com/u/712ce1f56160?source=post_page-----5c8d0affcec7----------------------) ,[Shadaj Laddad](https://medium.com/u/47e634bb4796?source=post_page-----5c8d0affcec7----------------------) ,[Peggy Rayzis](https://medium.com/u/c827782c6410?source=post_page-----5c8d0affcec7----------------------) ,[Martijn Walraven](https://medium.com/u/592c66ff70d?source=post_page-----5c8d0affcec7----------------------) ,[Thea Lamkin](https://medium.com/u/db65a320cccc?source=post_page-----5c8d0affcec7----------------------) , and[Sashko Stubailo](https://medium.com/u/803918030a60?source=post_page-----5c8d0affcec7----------------------) for the guidance, work, and support in making this release possible.


More importantly, this release would not be possible without the amazing work of the Apollo community. So many peopled helped make this a reality but I wanted to mention[Jonas Helfer](https://medium.com/u/39cbd38c57c8?source=post_page-----5c8d0affcec7----------------------) , Hagai Cohen,[Ian MacLeod](https://medium.com/u/6ca780f916b8?source=post_page-----5c8d0affcec7----------------------) ,[James Reggio](https://medium.com/u/373d9846244b?source=post_page-----5c8d0affcec7----------------------) ,[Jayden Seric](https://medium.com/u/5bd1b4d20a6d?source=post_page-----5c8d0affcec7----------------------) ,[Jon Wong](https://medium.com/u/2f97b37d43d3?source=post_page-----5c8d0affcec7----------------------) ,[Kamil Kisiela](https://medium.com/u/627fc8b15105?source=post_page-----5c8d0affcec7----------------------) ,[Guillaume CHAU](https://medium.com/u/9bafe4e8ffdd?source=post_page-----5c8d0affcec7----------------------) , specifically for their help and testing.


I hope you love using Apollo!


Written by


James Baxley III


[Read more by James Baxley III](https://www.apollographql.com/blog/author/jbaxleyiii)
