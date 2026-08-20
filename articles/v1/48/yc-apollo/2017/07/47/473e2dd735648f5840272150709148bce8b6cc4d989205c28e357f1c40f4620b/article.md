---
schema_version: "1.0.0"
document_id: "473e2dd735648f5840272150709148bce8b6cc4d989205c28e357f1c40f4620b"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/inspecting-your-graphql-client-in-the-browser"
published_at: "2017-07-11T18:25:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:26.939710+00:00"
content_hash: "sha256:fc6e713f50da01960c34f56ddf4c9e303be71cd419594d5c3b3b40cb1cf9643e"
---

# Inspecting your GraphQL client in the browser

Late last year,[Danielle Man](https://medium.com/u/47b3b199a712?source=post_page-----2481f8bcdd55----------------------) blogged about the[Apollo Client Developer Tools](https://dev-blog.apollodata.com/apollo-client-developer-tools-ff89181ebcf) Chrome extension, which helps you debug your Apollo-Client-based GraphQL app. The Dev Tools launched with three main features, and we’re excited to announce the first major addition today!


1. Built-in Graph *i* QL query IDE
2. Watched queries inspector
3. **Mutation log (**[new](https://github.com/apollographql/apollo-client-devtools/pull/36) **!)**
4. Store inspector


It’s been really exciting to see the reception of this tool in the last few months, and it now has over 8,000 downloads on the Chrome store. In this post, we’ll highlight the purpose of each feature and walk you through how you might use it during app development.


*If you want to follow along, get started by*[installing the Apollo Client Devtools extension](https://chrome.google.com/webstore/detail/apollo-client-developer-t/jdkknkkbebbapilgoeccciglkfbmbnfm?hl=en-US) *from the Chrome store.*


## Graph *i* QL query explorer


The main use case of Graph *i* QL is to run queries against your server to explore the data available in the API. You can easily see if your client instance is configured correctly and if your sever is returning the data you expect.


### No configuration needed


The best part of having Graph *i* QL built into the dev tools is that queries are sent from your already configured Apollo Client instance, so you don’t have to manually pass in headers and other data or manually configure a Graph *i* QL page in your app. This makes it super easy to test different queries in your app and be confident that you’re using the correct setup. To demonstrate this feature, let’s test out some queries in Graph *i* QL in[GitHunt](http://www.githunt.com/) , our live Apollo Client example app. Once you’ve navigated to[www.githunt.com](http://www.githunt.com/) , open the Apollo Client DevTools by opening the Chrome devtools console and clicking on the Apollo tab at the far right. This should take you right to Graph *i* QL. Now, we can query the server for the repository, name, and description of each element in the new repositories feed by writing the following query into the left side of the console and clicking the play button.


```text
query {
feed(type:NEW) {
repository {
name
full_name
description
}
}
}
```


If you have the dev tools installed correctly, it will look like this:


The data returned by the query will be displayed on the right side of the console.


The results returned by the query will be displayed on the right side of the console. This lets you easily test out different queries against your server’s schema while developing your frontend.


You can also try out Graph *i* QL’s auto-complete feature and see more about the schema for the GitHunt example app by typing` query { }` and then typing “ctrl+space” within the curly braces to see what fields pop up. You can also use the handy “Docs” button in the top right to see complete documentation about the schema.


## Watched queries inspector


The watched queries inspector allows us to view the watched queries and query variables on a page, as well as any errors thrown by that query. A *watched query* is a query that is currently being used by a component in the app.


Developers often split up the queries used by one page of an app into multiple files or fragments in the source code, making it cumbersome to view the entire query in one place and test out modifications. The watched queries inspector combines these fragmented queries into one query string in your developer console, saving you many roundtrips between the source code to collect the full query.


Try out the watched queries inspector on GitHunt by clicking on the “queries” button on the left panel of the extension. The left side of the new view will display a list of watched queries on the page. To view the query used to display the home page feed, click on the` Feed` query, and then open the “Query string” dropdown.


One of our favorite features is that you can then easily run the query in the GraphiQL console with the “run in GraphiQL” button.


## Mutation log


This is the newest feature of the dev tools released just a few days ago. Thanks to awesome contributors[Ryan-Neal Mes](https://github.com/ryannealmes) and[Ryan Delaney](https://github.com/rrdelaney) for contributing and reviewing this major new feature!


With the mutation log, you can see the most recent mutations done through Apollo Client, see the query and variables, and then test them out yourself via GraphiQL:


This new panel has a ton of potential. For example, in the future it might be able to help you debug your mutation` update` functions or see how optimistic UI is working. If you have an idea, please[file an issue or open a PR](https://github.com/apollographql/apollo-client-devtools) !


## Store Inspector


The store inspector allows you to view all data loaded from active queries and mutations and provides insight into how Apollo Client’s normalized caching works under the hood. Examples of these on the GitHunt site include 1) prefetching comment data into the store on comment mouse-over, and 2) the noticeable difference between temporary and permanent ID creation for comments while they are being saved to the server. Let’s take a look. Navigate back to GitHunt, open the dev tools and click on the “Store” tab in the left panel. The left side of the newly-opened tab will display the root query object and a list of all object IDs currently in the store.


Clicking on each individual object will show you the data that Apollo Client has loaded for each entry in the feed.


Navigating to another page on the site and fetching new data will put that data in our store. For example, let’s look at` Entry63` , the object in the store that represents` facebook/graphql` . If we click on “view comments” on the` facebook/graphql` repository and submit a comment, we can see that the data from the added comment has been added to the store.


### Why not Redux dev tools?


All of the data loaded with Apollo Client is actually being stored in Redux, so why not just use the Redux dev tools for this? Well, it’s actually possible to view the underlying Redux state directly, as shown below. That can be useful if you really want to dig into the implementation.


However, it can be a bit difficult to decipher the data in that format. The store feature of the Apollo Client dev tools displays the same data in a more GraphQL-specific format, making it easier to read.


### Inspecting changes during mutations


You can use the store inspector to see how the data in the store changes after mutations, as shown in the below gif. To test this out for yourself, try using the extension while doing[this tutorial](https://dev-blog.apollodata.com/react-graphql-tutorial-mutations-764d7ec23c15) on GraphQL mutations, written by[Jonas](https://medium.com/@helfer) . Here’s a quick preview:


## What’s next?


We’ve heard from many people that the developer tools are one of the reasons they love using Apollo Client to load data in their app. We agree, and our ultimate goal is to create a GraphQL experience where you can easily understand and inspect what’s going on, every step of the way.


We’re excited to put some focused effort into the Apollo Client Dev Tools this summer, and we’ll keep you updated as we fix current issues and add new features. Next up: Better performance and stability in apps with a very large amount of data, and support for FireFox!


---


*Ramya studies Computer Science at MIT and is joining us for the summer as an intern at Apollo. Be sure to follow along with her work on*[Twitter](https://twitter.com/rrramyan) *.*


*If you’re as excited about advancing GraphQL technology and enabling developers as we are,*[we’re hiring](https://www.meteor.io/jobs/) *!*


Written by


Ramya Nagarajan


[Read more by Ramya Nagarajan](https://www.apollographql.com/blog/author/rrramyan)
