---
schema_version: "1.0.0"
document_id: "b6303a17598bceaea2387537d99232eb2f5ba44c7b541af8829cb6ab91cfb6fe"
company_key: "omada-health-inc-common-stock"
company: "Omada Health Inc."
source_id: "omada-health-inc-common-stock-rss-9e1c3b1ccfca"
canonical_url: "https://medium.com/omada-health-tech/graphql-reaping-the-benefits-423daeb96be8"
published_at: "2020-05-05T16:31:01+00:00"
first_seen_at: "2026-07-20T23:18:22.449098+00:00"
fetched_at: "2026-07-28T22:26:48.304506+00:00"
content_hash: "sha256:0ef8d0380576a9ddcf43b7947c877d58cc4b7f62dffefbf33080682e4fa33257"
---

# GraphQL: Reaping the Benefits

GraphQL


API


Apollo Client


# GraphQL: Reaping the Benefits


[Omada Health Engineering](https://medium.com/@OmadaEng?source=post_page---byline--423daeb96be8---------------------------------------)


7 min read


·


May 5, 2020


--


*Written by*[Will Hastings](https://www.linkedin.com/in/willhastings4) *, Staff Engineer*


Photo by[Alina Grubnyak](https://unsplash.com/@alinnnaaaa?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on[Unsplash](https://unsplash.com/s/photos/network?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)


In my[last post on our adoption of GraphQL](https://medium.com/omada-health-tech/exploring-graphql-at-omada-f64133a50462) , I discussed the process our engineers took in evaluating and trialing GraphQL as a technology for improving how we develop APIs. Throughout that process, we confirmed that GraphQL solves or improves upon several of the shortcomings we identified with the way we traditionally developed APIs. In this post, I’ll enumerate those shortcomings and how starting to build several new APIs with GraphQL has led to numerous benefits.


## Documenting our API


We’ve always faced the challenge of documenting the structures and data types of our API’s responses and expected request formats. Lack of good documentation places a challenge on our mobile developers, who sometimes would end up reading through our backend Ruby code trying to figure out just what type a particular data point would have in a response. We tried a few different solutions, such as handwriting API documentation in Google docs or code comments and describing our structure/types using a YAML-based format.


These approaches worked for the most part, but suffered from issues like:


- Requiring manual engineering effort to stay up-to-date
- Supporting response types/structures out-of-the-box, but not requests


Since writing an API with GraphQL requires that you define a[statically typed schema](https://graphql.org/learn/schema/) , there are several great tools available to automatically generate documentation from your schema. We went with[GraphiQL](https://github.com/graphql/graphiql/tree/master/packages/graphiql#readme) since it comes out-of-the-box with the[graphql-ruby gem](https://graphql-ruby.org/) and easily mounts into a Rails app. So far, both web and mobile engineers have appreciated having complete and always up-to-date API documentation at their fingertips.


## Sending the Right Data Points in Fewer Requests


Before GraphQL, we often struggled to balance the number of our API endpoints and the size of their responses. Initially, we had dedicated endpoints for each type of resource (such as meals tracked or messages received). But this often required our client apps to make multiple requests to get the data they needed, which could easily turn into a performance problem, particularly for our mobile apps when used on cellular data networks.


The next evolution was API endpoints tailored to particular pages or screens in the app. This allowed all the data for a particular section of an app to be retrieved in one request. But inevitably discrepancies would arise between platforms, and one platform might end up with data it didn’t need. For example, we sometimes decide to initially launch new features in only one of our mobile apps or only in our web app, so that we can learn about the use and value-add of the feature before investing in it more heavily. But with page/screen-based API endpoints, it becomes harder to exclude data from the responses to apps that don’t use it, and to avoid the associated backend data loading and processing.


GraphQL helps us with both the amount of requests and the data points loaded. A client app can load several different types of resources in one GraphQL query over one HTTP request. Additionally, since GraphQL only responds with the data points, or “fields” that the client app requested in a query, we can easily avoid loading data on an app that doesn’t need it.


## Increasing Ease-of-Use for our Mobile Engineers


With our REST-based endpoints, our iOS and Android engineers have traditionally needed to write strongly-typed classes in Swift or Java/Kotlin to represent the data sent back from our API before they could start consuming data from a new endpoint. With[Apollo’s](https://www.apollographql.com/) native mobile libraries, they can instead have these classes automatically generated from our API’s schema and the GraphQL queries and mutations they’ve written. This means less time before they can start loading data, and less human-generated code for them to maintain.


## Catching Integration-level Bugs


Ensuring web and mobile client apps are making valid requests to our API has traditionally been time consuming to set up. Across both web and mobile, we’ve relied on often expensive end-to-end tests to run our apps against a running server. While these tests provide a high level of confidence that the integrations between our apps and server are sound, they often aren’t written until late in the feature development process, and existing ones aren’t usually run until an engineer pushes their work to our CI system. This increases the time from a bug being introduced in either the API or a client app to when it’s caught.


GraphQL improves on this situation due to its strongly-typed nature. Because tooling can statically analyze queries and mutations to validate them against our API schema, we can pick up on bugs more quickly than with end-to-end tests. On mobile, the automatic class generation process mentioned before will fail at compile time if they write an invalid query or mutation, or if we accidentally introduce a breaking change on the backend. We’ve also set up[eslint-plugin-graphql](https://github.com/apollographql/eslint-plugin-graphql) to do the checks for our web frontend.


## Centralizing the Definition of a Resource Type


In the past when we built page-based API endpoints, we sometimes found that the same data needed to be available in multiple endpoints for different pages (e.g. on our home page and on our Progress page), and in slightly different formats. This resulted in duplicate sources-of-truth for how a particular type of resource could be formatted in API responses, and more time writing similar code and tests.


In GraphQL, you’re encouraged to define a single type for a given resource (e.g. “meal” or “blood pressure reading”). Various fields on other types can return instances or lists of instances of that resource, but that resource is still implemented in the API by one type. That type can contain all the possible data a client might want to have access to, since GraphQL will only return the data points that the client explicitly queries for. Being defined in one place rather than several also makes it less likely that bugs will be introduced in the representation of a data type.


GraphQL provides a nice balance between the organization of RESTful APIs and the convenience of page-based APIs by allowing clients to mix-and-match the types and amount of data pulled in a given query or mutation result, according to the client’s needs.


## Simplifying Data Retrieval and Storage on Web


For our participant and health coach-facing web apps, we’ve been using React and Redux for several years now. While they’ve given us a big productivity and maintainability boost, we found that they sometimes fall short when it comes to retrieving and managing data from our API. React is not a data loading library, so it gets a free pass. Redux, out of the box at least, is not a data loading library either, but rather a predictable state manager that does a great job at managing app and UI state that various disparate components need to access.


You can leverage other libraries to add data loading capabilities to Redux. But you’re often left writing a lot of repetitive code around making API requests and managing loading, success, and error states. You also have to figure out the best way to store your data and manage the relationships between different pieces of data, as well as work together to keep everyone’s code consistent. There are many libraries out there to help with these various concerns, but no one in particular has risen above the others as the best solution, so you’re left fighting decision fatigue.


We found that[Apollo Client and its React integration](https://www.apollographql.com/docs/react/) for web bring some advantages over how we were doing data loading and management with Redux:


- Request Lifecycle: Not only does Apollo make the API request for you, it automatically re-renders your React component when the requests starts loading and upon completion, whether in success or error.
- Data Normalization: With GraphQL, data comes back from the API in a nested format, with an object of one type of data containing one or more instances of other types of data, and so on. Apollo automatically normalizes this data into a store in which each object is stored under a key based on the object’s type and ID. This makes it easy to look up and update a particular object without having to traverse a nested data structure.
- Standardization: Apollo’s API is more opinionated than Redux and its associated libraries. The patterns it provides make it easier for different developers to write more consistent code.
- Reduced state management code: While Apollo still requires some work on your part to update the client-side store in some cases (add and delete), it can be simpler than what you might do in a Redux state tree since you can pull specific objects out of its normalized store by ID or by query.


## Conclusion


Overall, using GraphQL has been a boon to development, both in building and consuming APIs. These benefits make the learning curve and cost of adoption worthwhile.


In a future post, I’ll delve into the technical details around the tips, tricks, and watchpoints we’ve discovered so far when working with GraphQL. Stay tuned!


*Thank you to the following fellow Omadans for their help with GraphQL adoption and/or this blog post: Franck Verrot, Chris Constantine, Ethan Ensler, Alexander Garbowitz, Scott Stebleton, Austin Putman, Jonathan Wrobel*
