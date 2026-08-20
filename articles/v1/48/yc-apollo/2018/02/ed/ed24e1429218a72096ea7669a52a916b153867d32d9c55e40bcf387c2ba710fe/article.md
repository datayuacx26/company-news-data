---
schema_version: "1.0.0"
document_id: "ed24e1429218a72096ea7669a52a916b153867d32d9c55e40bcf387c2ba710fe"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/improved-apollo-client-devtools-eb43258f103f"
published_at: "2018-02-01T21:53:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:53.665310+00:00"
content_hash: "sha256:6c27fe741d4d5f21e92d629e2f9718bfa41d7bef6172b6b4d1e4e076f5226b69"
---

# Improved Apollo Client Devtools

One of my favorite parts of the Apollo ecosystem are the[Apollo Devtools](https://github.com/apollographql/apollo-client-devtools) . What started as a[hack week project](https://dev-blog.apollodata.com/apollo-client-developer-tools-ff89181ebcf) for the core team turned into a pretty amazing way to debug and understand what is happening in an Apollo Client-driven app. The integrated Graph *i* QL, list of active operations, and cache inspector are all invaluable tools for digging around the data your app is using. Today I’m super excited to showcase a couple new features we’ve added and give you a taste of what is to come!


## Whats new today


We’ve just released[a new version of the Devtools on the Chrome Web Store](https://chrome.google.com/webstore/detail/apollo-client-developer-t/jdkknkkbebbapilgoeccciglkfbmbnfm) with a host of important improvements:


### A new, more stable architecture


We had heard reports that in some cases, the devtools wouldn’t respond when making queries, would break after a page reload, or were too slow for large operations. In the newest version, many of these issues have been fixed. Thanks to incredible help from[Guillaume CHAU](https://medium.com/u/9bafe4e8ffdd?source=post_page-----eb43258f103f----------------------) , and drawing from examples in the Vue.js Devtools, the Apollo Devtools have a new architecture which addresses a number of stability and performance issues. If you’ve encountered these issues in the past, give them another shot!


### Improved dark mode


The dark mode for the Devtools (activated when your Chrome console is set to the dark theme) is improved. Graph *i* QL now uses the dracula theme from[CodeMirror](https://codemirror.net/) instead of the bright Graph *i* QL theme. This is especially useful for those late-night debugging sessions.


Dark mode for GraphiQL


### GraphQL subscriptions in Graph *i* QL


The new version of the Devtools uses the Observable-driven interface of Apollo Link instead of a promise-based fetcher for Graph *i* QL. This means that subscriptions are now fully supported and use the same connection that your application is using to your server!


This is just the beginning of subscription support. In the near future, I expect to see full subscription debugging features just like we currently have for queries and mutations.


Subscriptions in the Devtools 🎉


### Client-side schema support


[Peggy Rayzis](https://medium.com/u/c827782c6410?source=post_page-----eb43258f103f----------------------) has started an incredible effort to bring easy, local state management to Apollo.[Apollo Link State](https://www.apollographql.com/docs/link/links/state.html) is a great way for your team to learn one way to manage data that’s both local to the app and shared remotely. This version of the Devtools has initial support allowing you to query your client-side data and even see the schema for your local state.


This is just the beginning — with upcoming changes to Apollo Client, you will be able to fully query local and remote data side by side in your browser’s Graph *i* QL!


### All in on Apollo Link


Subscription and local state support rely on Apollo Link, the new network architecture introduced in Apollo Client 2.0. This means the Devtools no longer supports Apollo Client 1.0. If you’re still on 1.0, it’s easy to upgrade and we have a full guide[here](https://www.apollographql.com/docs/react/2.0-migration.html) ! If you need your Devtools fix right now and haven’t had the chance to upgrade, you can manually install the old Devtools version[here](https://github.com/apollographql/apollo-client-devtools/releases/tag/2.0.6) .


## What’s next


This initial set of improvements was primarily an effort to improve stability and taking advantage of more of what Apollo Client 2.0 has to offer. But we’ve also got a couple of things coming up that I’m super excited about because of the impact they’ll have on day-to-day development with GraphQL.


### Official support for more browsers


Previously, the Devtools only worked for Chrome. We are working on new versions for FireFox, Safari, a standalone Electron app, and integration with React Native Debugger! Anywhere that Apollo Client can be used, we want the Devtools to be there, too.


### More sophisticated schema exploration and usage


GraphQL is incredible due in large part to the built-in ability to introspect a server’s schema. In future versions of the Devtools, we plan to use this to show your schema’s data graph and overlay how much of it is being used by your application. As you navigate around, you’ll be able to see which types are being used, and even how data is being updated with things like mutations and direct cache writes. To put the icing on the cake, we’re going to work on time traveling debugging to play back states of your app!


### Apollo Link inspector


[Apollo Link](https://www.apollographql.com/docs/link/) is an amazing way to isolate and compose parts of your data fetching needs with Apollo Client. We want to provide a view that lets you see how data is flowing through these composed chains, where errors happen, and, with Apollo Tracing, how long different parts of the server response took to execute. You can think of these new features as a miniature version of the trace view in[Apollo Engine](https://www.apollographql.com/engine) .


### Viewing GraphQL operations in the application context


Being able to see the data in your app is so useful, but it could be even better if it’s tied into how your app actually works! In the future, we hope to build one or more views that let you do things like view operations by routes, link data directly to components on the page, and even open an operation in your editor.


---


## Our goal: Helping you make the most of GraphQL


At Apollo, our primary goal is to make GraphQL easy to get started with, then smoothly grow with you to be the most powerful way to manage data for your application. Tools like[Apollo Codegen](https://github.com/apollographql/apollo-codegen) ,[eslint-plugin-graphql](https://github.com/apollographql/eslint-plugin-graphql) , and the Apollo Devtools are just a few ways to leverage GraphQL’s power to ship features faster. Projects from the Apollo community including Apollo Client, Apollo Link, and Apollo Server do the hard work of data plumbing so you can focus on what your users need.


*If you’re as excited as we are about pushing this technology forward, we’re actively hiring for visual design, frontend developers, and open source API tooling engineers!*[DM Sashko on Twitter](https://twitter.com/stubailo) *for more info.*


Written by


James Baxley III


[Read more by James Baxley III](https://www.apollographql.com/blog/author/jbaxleyiii)
