---
schema_version: "1.0.0"
document_id: "40c10bac4abbb5a48586c8e813484cdc72d094d34e4e874c07afe1dd241757c2"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/creating-a-data-component-with-apollo-link-f0719d8193ee"
published_at: "2017-10-16T23:42:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:25.846890+00:00"
content_hash: "sha256:aac43cb94169d861327a8e1e4b4da6511621daff576972d81aea20c6016c02c9"
---

# Creating a data component with Apollo Link

With the ever-nearing release of[Apollo Client 2.0,](https://dev-blog.apollodata.com/whats-coming-in-apollo-client-2-0-bcd8ea64acbd) there’s no better time to learn some of the new features that make the 2.0 the most flexible and powerful version ever.


One of the ways to leverage this new flexibility is by using the[Apollo Link library](https://github.com/apollographql/apollo-link) to customize what happens each time you send an operation to Apollo. Links are designed to be composed together to form control flow chains to manage a request, and they can be used as:


- Middleware to perform side effects, modify the operation, or even just provide developer tools like logging.
- Afterware which process the result of an operation, handle errors, or even save the data to multiple locations.
- Network fetchers including HTTP, websockets, and even across the react-native bridge to the native network stack.


This article will outline some of the core concepts to get you started. For a deeper dive, checkout the new[link docs!](https://www.apollographql.com/docs/link/)


## What is a link?


One of the authors of Apollo Link,[Evans Hauser,](https://medium.com/u/712ce1f56160?source=post_page-----f0719d8193ee----------------------) does a great job of outlining the goals of the package in his[introductory post](https://dev-blog.apollodata.com/apollo-link-the-modular-graphql-network-stack-3b6d5fcf9244) . Building on top of that knowledge, the library is designed to be a powerful way to compose actions around data handling with GraphQL. Each link represents a subset of functionality that can be composed with other links to create complex control flows of data. At a basic level, a link is a function that takes a GraphQL request and returns an observable. To learn more about observables, take a look at the proposal[here](https://github.com/tc39/proposal-observable) and read the incredible work of[Ben Lesh](https://medium.com/u/da6839d28258?source=post_page-----f0719d8193ee----------------------) such as[this article](https://medium.com/@benlesh/learning-observable-by-building-observable-d5da57405d87) .


A individual link isolate parts of the control flow of your GraphQL request into a single unit. For example, you could have a link that logs errors to your monitoring tool of choice. This error link doesn’t need to know anything else about how you are executing your request, just that it starts with an operation and eventually data will hopefully come back.


### Request


At the core of a link is the` request` method. A link’s request is called every time` execute` is run on that link chain. The request is where the operation is given to the link to return back data of some kind. Depending on where the link is in the stack, it will either use the second parameter to a link (the next link in the chain) or return back an` ExecutionResult` on its own.


The full description of a link’s request can be represented like this:


```text
type Context = { [key: string]: any };


interface Operation {
query: DocumentNode;
variables: { [key: string]: any };
operationName: string;
extensions?: { [key: string]: any };
getContext(): Context;
setContext(newContext: Context | (prevContext: Context) => Context): void;
toKey(): string;
}


type NextLink = (operation: Operation) => Observable<ExecutionResult>


type RequestHandler = (operation: Operation, forward: NextLink) => Observable<ExecutionResult>


```


### Terminating links


Since link chains have to fetch data at some point, they have the concept of a terminating link and non-terminating links. Simply enough, the terminating link is the one that doesn’t use the` forward` argument, but instead turns the operation into the result directly. Typically, this is done with a network request, but the possibilities are endless of how this can be done. The terminating link is the last link in the composed chain.


### Context


Links are meant to be composed into a single chain to handle an operation. Because of this, they need an easy way to send metadata about the request down the chain. They also need a way for the operation to send specific information to a link no matter where it was added to the chain. This is very similar to how context works in React. To accomplish this, each operation has a context object which can be accessed and manipulated by each link. For example:


```text
const link = new ApolloLink((operation, forward) => {
operation.setContext({ start: new Date() });
return forward(operation).map((data) => {
const time = new Date() - operation.getContext().start;
console.log(`operation ${operation.operationName} took ${time} to complete`);
return data;
})
})
```


Each context can be set by the operation it was started on. For example with Apollo Client:


```text
const link = new ApolloLink((operation, forward) => {
const { saveOffline } = operation.getContext();
if (saveOffline) // do offline stuff
return forward(operation);
})


const client = new ApolloClient({
cache: new InMemoryCache()
link,
});


// send context to the link
const query = client.query({
query: MY_GRAPHQL_QUERY,
context: {
saveOffline: true
}
});
```


## Building a chain of links


Since links represent small portions of how you want your GraphQL operation to be handled, in order to serve all of the needs of your app they designed to be composed with other links as needed. Composition is managed in two main ways: additive and directional. Additive composition is how you can combine multiple links into a single chain, and directional is how you can control which links are used depending on the operation.


### Additive


Apollo Link ships with two ways to compose links. The first is a method called` from` which is both exported, and is on the` ApolloLink` interface.` from` takes an array of links and combines them all into a single link. For example:


```text
import { ApolloLink } from 'apollo-link';
import { RetryLink } from 'apollo-link-retry';
import { HttpLink } from 'apollo-link-http';
import MyAuthLink from '../auth';


const link = ApolloLink.from([
new RetryLink(),
new AuthLink(),
new HttpLink({ uri: '/graphql' })
]);
```


` from` is typically used when you have many links to join together all at once. The alternative way to join links is the` concat` method which joins two links together into one.


```text
import { ApolloLink } from 'apollo-link';
import { RetryLink } from 'apollo-link-retry';
import { HttpLink } from 'apollo-link-http';


const link = ApolloLink.concat(new RetryLink(), (new HttpLink({ uri: '/graphql' }));
```


### Directional


Given that links are a way of implementing custom control flow for your GraphQL operation, Apollo Link provides and easy way to use different links depending on the operation itself (or any other global state). This is done using the` split` method which is exported as a function and is on the` ApolloLink` interface. Using the split function can be done like this:


```text
import { ApolloLink } from 'apollo-link';
import { RetryLink } from 'apollo-link-retry';
import { HttpLink } from 'apollo-link-http';


const link = new RetryLink().split(
(operation) => operation.getContext().version === 1,
new HttpLink({ uri: "/v1/graphql" }),
new HttpLink({ uri: "/v2/graphql" })
);
```


## Stateless vs Stateful


Links are typically created and shared between every request in your application. However, most links do the same thing for each request and don’t need any knowledge about other operations being performed. These links are called stateless links because they have no shared execution state between requests. Stateless links can be written as simple functions wrapped in the` ApolloLink` interface. For example:


```text
import { ApolloLink } from 'apollo-link';


const consoleLink = new ApolloLink((operation, forward) => {
console.log(`starting request for ${operation.operationName}`);
return forward(operation).map((data) => {
console.log(`ending request for ${operation.operationName}`);
return data;
})
})
```


Stateless links can also be written by extending the` ApolloLink` class and overwriting the request method. This is one way to store information about an individual link for each request.


```text
import { ApolloLink } from 'apollo-link';


class ReportErrorLink extends ApolloLink {
constructor(errorCallback) {
super();
this.errorCallback = errorCallback;
}
request(operation, forward) {
const observer = forward(operation);
// errors will be sent to the errorCallback
observer.subscribe({ error: this.errorCallback })
return observer;
}
}


const link = new ReportErrorLink(console.error);
```


### Stateful links


Sometimes a link may have different behavior based on other requests that are being executed. One such example is a link to batch requests into a single HTTP request instead of sending many at slightly different times. In order to do this, links can keep track of all operations being sent through them. The operation has a` toKey` method which will return a unique string based on the operation. Using this, you can write a link that manages application state easily:


```text
import { ApolloLink } from 'apollo-link';


class OperationTimingLink extends ApolloLink {
constructor() {
super();
this.operations = {};
}
request(operation, forward) {
const key = operation.toKey();
if (!this.operations[key]) this.operations[key] = {}
this.operations[key].start = new Date();
return forward(operation).map((data) => {
this.operations[key].elapsed = new Date() - this.operations[key].start;
return data;
});
}
}


const link = new OperationTimingLink();
```


---


The goal of Apollo Link is to provide a huge degree of flexibility to customize your GraphQL application to just what your app needs. In the future, we hope to see a number of links available on npm just like react components are today. Pick and choose what you want your app to be, compose them together, and let Apollo Client handle the rest!


Special thanks to[Evans Hauser](https://medium.com/u/712ce1f56160?source=post_page-----f0719d8193ee----------------------) ,[Peggy Rayzis](https://medium.com/u/c827782c6410?source=post_page-----f0719d8193ee----------------------) and[Sashko Stubailo](https://medium.com/u/803918030a60?source=post_page-----f0719d8193ee----------------------) for making Apollo Link such a powerful and fun library to work with!


Written by


James Baxley III


[Read more by James Baxley III](https://www.apollographql.com/blog/author/jbaxleyiii)
