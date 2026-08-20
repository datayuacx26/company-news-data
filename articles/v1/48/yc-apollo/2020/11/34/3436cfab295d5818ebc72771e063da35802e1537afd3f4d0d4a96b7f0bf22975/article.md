---
schema_version: "1.0.0"
document_id: "3436cfab295d5818ebc72771e063da35802e1537afd3f4d0d4a96b7f0bf22975"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/why-use-graphql"
published_at: "2020-11-02T15:50:05+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:0b7ef6523395453a45906de5ff5b561e49e02009efd42e37e70389dae7c0659b"
---

# Why use GraphQL?

Before there was GraphQL, there was REST.


In recent years, REST has become the dominant API style for building backend web services. With REST, you could signal the *type* of request we want to make (ex: GET, POST, PUT, or DELETE) and the resource we’d like to fetch or interact with (ex: /api/pets/1) using an HTTP method and a URL.


It’s a great approach (and one we initially used at StockX for several years). But REST comes with some downsides like:


- Over-fetching
- Multiple requests for multiple resources
- Waterfall network requests on nested data
- Each client need to know the location of each service


Then came GraphQL: *a server-side runtime and query language for your API* .


GraphQL lets you ask for what you want in a single query, saving bandwidth and reducing waterfall requests. It also enables clients to request their own unique data specifications.


In the summer of 2019, I wanted to make a case to migrate from our existing RESTful API architecture to GraphQL at StockX. Since then, we’ve been running GraphQL in production and enjoying the benefits around topics like documentation, performance, and developer productivity.


In this blog post, we’ll touch upon seven of the most notable benefits of GraphQL taken from my original 16-page RFC (request for comments) that we used to make the decision to switch from REST to GraphQL.


## No more versioned APIs


In a RESTful world, APIs have versions, and as APIs evolve, API version numbers increase. And as long as services keep on improving, version numbers never stop incrementing.


This leads to problems down the line.


For example, when version numbers increase indefinitely, it becomes challenging to know the latest API version capable of returning the correct data.


This also leads to the confusion of having multiple versions for each environment (ex: perhaps example is in V1, staging is in V2, and production is in V3).


In GraphQL, there’s one version of the graph. To keep track of how the graph changes and evolves over time, we register it in a[schema registry.](https://www.apollographql.com/docs/studio/schema/registry/) The registry behaves similar to that of a version control system like Git, and becomes the central place that keeps track of the current state of the graph.


## **Smaller payloads**


In RESTful API, there are no *partial* payloads — you get *everything* in every request, every time. If you don’t *need* everything, that’s unnecessary bandwidth.


In GraphQL, you specify the exact types and fields that you need; and if you don’t need much, that introduces the potential to make your API requests smaller. And for client-side applications that depend on data from APIs, reducing the amount of data transmitted could improve their performance considerably.


## **Strictly-typed interfaces**


GraphQL has strictly-typed interfaces where REST APIs have much looser ones.


Let’s say the base URL for your RESTful API is` http://www.restful.com/api/v1` . If we wanted to add posts as a resource, we could represent that with` http://www.restful.com/api/v1/posts` .


Executing a GET request on this URL *should* return an array of posts, though, of course, there’s nothing stopping the server-side developers from returning something completely different. Ultimately, this approach works well up until the point where we start to add[query params](https://en.wikipedia.org/wiki/Query_string) .


For example, if we wanted to include authors, we could adjust the route to accept another parameter to signal an additional resource type, like` api/v1/posts?include=author` .


How about the use case where we need to sort posts? The URL then becomes` api/v1/posts?include=author&sort=ASC` . Hopefully, you can see that the more we continue to add query params to improve the functionality of the endpoint, the more unmanageable it becomes.


These challenges lead to questions like “what’s the correct path to use?” and “what are the valid params I can send, and how do I send them? “. If you have front end and back end teams, these query parameters can stay undocumented for a long time.


GraphQL solves this with a single endpoint and a strictly-typed schema. By design, the schema defines the resources available for retrieval in addition to the accepted parameters when fetching that data.


## **Better client performance**


We already mentioned the ability to reduce payload size, but GraphQL also introduces the ability to minimize round trips to the server.


In a RESTful world, clients request a resource from the server, get the ID for it, then use that ID to fetch another resource (a waterfall-like series of requests to get related data).


GraphQL lets you get all the data you need in a single request; no need for follow-up requests.


## **Less time spent documenting and navigating APIs**


Traditionally, in RESTful API development, we’d use a tool like Swagger to document and navigate large APIs. I’ve found that Swagger docs are great on projects with a single service because everyone can pitch in to keep the documentation up to date. Once you get into the microservice world, however, documentation has the potential to become expensive to maintain. With Swagger docs scattered across multiple repos, it can be hard for new developers to ramp up.


GraphQL is already strictly-typed and declarative, so that’s an immediate improvement. But for what cannot be said declaratively through types and fields alone, we can also document the GraphQL schema. It looks like code comments and leads to being outwardly accessible.


```text
type Customer {
""
ID is generated by the database
""
id: ID!


""
Customer name is pulled from the CRM
""
name: String!


""
The list of orders that belongs to the customer.
""
orders: [Order]
}
```


And again, this self-documenting schema coupled with 1 URL makes onboarding developers much more straightforward than sending them multiple swagger docs.


There are also some excellent tools to explore your graph, like[Apollo Studio’s Explorer](https://www.apollographql.com/blog/introducing-the-apollo-explorer/) ,[GraphQL Playground](https://github.com/graphql/graphql-playground) , or[GraphiQL](https://github.com/graphql/graphiql) .


## **Legacy app support**


Upon releasing a RESTful API into the wild, it has to stay available for a long time. Depending on your company’s stance on legacy apps, that could be for as long as someone has that app. This means you can’t remove the V1 endpoints from your code no matter how out of date the endpoint is. It’s usually not a big deal until it is a massive problem. It can force you to keep older versions of packages, which might have security vulnerabilities or significantly increase the side of your build.


The other sizeable unexpected problem this causes is circumventing the new API. If V1 has different requirements than V2, it can wreak havoc on the backend when we add a resource with two different sets of required fields.


Since we typically only run a single GraphQL graph, when we swap out the graph’s data, that’s ok as long as the response looks the same. This allows people to swap out backends and change the source of truth on the backend with no changes from the front ends. And in addition to backends changing with no client changes, as long as the schema supports the query path, the legacy app will always be supported.


## **Better error handling**


An area of typical complexity in traditional RESTful APIs is error handling. If any part of the code on a server fails during a request, that means the entire request fails; this can lead to code bloat and complex logic on front end applications when a microservice ecosystem fails.


To illustrate the complexity, consider the scenario where you have three subsequent REST calls that happen on a page. In the client-side code, you’d have to write the logic that dictates what happens when:


- call one fails
- call one passes
- call two fails
- and calls one and two pass but call three fails.


GraphQL resolves as much data as it can before returning. It returns a top-level error object with an array of errors but still returns as much information as possible, allowing front ends to decide how to handle the partial data.


Think of a product page on[amazon.com](http://amazon.com/) . If a RESTful API backed it, how would the UI deal with partial failure, and how many partial failures would it have to deal with? If supported by GraphQL, we could check the error array to see if there is an error for the section it was about to display.


## ****Conclusion****


From performance, payload size, developer time, and built-in documentation, GraphQL is the future of APIs. This post covered seven of the tremendous benefits of GraphQL vs. REST.


If you’re just getting started with GraphQL and Apollo, check out the[full-stack tutorial](https://www.apollographql.com/docs/tutorial/introduction/) . If you’re interested in learning more about how GraphQL can help you get more done faster in your company, download the free[GraphQL at Enterprise Scale](https://www.apollographql.com/guide/) ebook by the Apollo team.


Written by


Kyle Schrade


[Read more by Kyle Schrade](https://www.apollographql.com/blog/author/kyleschrade)
