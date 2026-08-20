---
schema_version: "1.0.0"
document_id: "601a924cf8cbda14709292dd34c300c7b72f13b9af9124a345ee0010b879e6ff"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/full-stack-error-handling-with-graphql-apollo"
published_at: "2018-06-13T22:55:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:d425af61a1c47cc696ea1654cf0bf070e1c69b29a4c7ddc3d14c1d33c0c09b7c"
---

# Full Stack Error Handling with GraphQL and Apollo 🚀

Most GraphQL layers sit between the application frontend and a constellation of micro-services and data sources, which make them a focal point for error handling. Errors can range from bad user inputs to back-end bugs to rare network outages. Since it’s at the center of all the action, GraphQL has great potential to help us handle these errors in a systematic way.


At Apollo, we’ve heard a lot of desire from GraphQL developers for more guidance on how to do error handling. That’s why we’re baking in some of the best practices on error handling into Apollo Server 2.0 (which is[currently in beta](https://dev-blog.apollodata.com/apollo-server-2-0-30c9bbb4ab5e) ). The new enhancements will make it much easier to communicate errors to your client in an organized way, and open up new possibilities for tooling, making developing apps with GraphQL and Apollo that much easier to get started with.


On its own, the GraphQL[spec](https://facebook.github.io/graphql/October2016/#sec-Errors) itself provides little guidance on how to format error responses, requiring only a` message` field with a string description of the error. This is usually insufficient for production apps:


- As a front-end developer, I would like to display rich error feedback in the UI to empower my users to fix their own problems. My client also needs to gracefully handle partial failures to provide the best end-user experience.
- As a back-end developer, I would like to be able to determine the health of my service by classifying and monitoring errors programmatically.
- As a package author, I would like to be able to write components/middleware that could handle errors automatically in the most appropriate way.


Without standard error handling constructs such as the ones we’re including in Apollo Server 2.0, you would end up with an arbitrary combination of network errors and GraphQL errors to tell clients what went wrong, and have to write lots of custom error handling logic.


In this blog post, we will learn:


1. Different types of GraphQL errors
2. Best practices to deal with those errors
3. Practical use-cases and examples using Apollo Server 2.0


---


## A developer’s view of errors


Before we dive into different strategies for handling errors, let’s get up to speed on some terminology. Generally speaking, there are two dimensions along which we can categorize errors.


1. Is it the client or server that is at fault?
2. Where did the error occur?


### Who is at fault: Request vs Server Errors


Request Errors occur when the client is at fault. There are 3 phases to a GraphQL query and client-caused errors may occur in any of them:


- Parse Phase: Client sent malformed GraphQL request, i.e. syntax error
- Validation Phase: Client sent inputs that failed GraphQL type checking
- Execution Phase: Client sent bad user input (request out of range, resource not found, duplicate key) or had bad permissions (user is not authorized to perform some action).


On the other hand, Server Errors are execution errors where the server is at fault. This could be caused by a downstream API or database failure, or some other program bug.


### Where the error occurred: graphQLErrors vs networkError


Apollo Client also distinguishes between two kinds of errors in the GraphQL response —` graphQLErrors` and` networkError` . Both of these fields are present on the` error` field, but they each have different semantics. The best way to think about the difference between the two is to consider wherethe error occurred.


- ` networkError` : Errors that are thrown outside of your resolvers. If networkError is present in your response, it means your entire query was rejected, and therefore no data was returned. For example, the client failed to connect to your GraphQL endpoint, or some error occurred within your request middleware, or there was an error in the parse/validation phase of your query.
- ` graphQLErrors` : Any error that is thrown within your resolvers. Since we have multiple resolvers executing that are all potential sources of errors, we need an array to represent all of them. More importantly,` graphQLErrors` from failed resolvers can be returned alongside data/fields that resolved successfully.


---


## Error handling in Apollo Server 2.0


One of the new features in Apollo Server 2.0 is the[ability to add error codes](https://www.apollographql.com/docs/apollo-server/v2/features/errors.html) to your GraphQL response. Any errors exported by Apollo Server that you throw in your resolver will augment the error response with a human-readable string in the` extensions.code` field. We are rolling out this feature with a few basic errors:


- ` AuthenticationError` — for authentication failures
- ` ForbiddenError` — for authorization failures
- ` UserInputError` — for validation errors on user input
- As a fallback for uncaught failures, any other unknown errors thrown within your resolver will add the code` INTERNAL_SERVER_ERROR`


In the following examples, we are using Apollo Server 2.0 (beta), which you can install like this` npm install apollo-server@beta` .


### Throwing ApolloErrors in your resolvers


Imagine you want to prevent unauthenticated users from executing certain queries. In the resolver for the protected action, you can check for the user object in the query context, and throw an` AuthenticationError` if it is not available.


```text
import { AuthenticationError } from 'apollo-server'
const resolvers = {
Mutation: {
protectedAction(root, args , { user }) {
if (!user) {
throw new AuthenticationError('You must be logged in');
}
}
}
};
```


This is how the response will look like:


```text
{
"error": {
"graphQLErrors": [
{
"message": "forbidden",
"locations": [],
"path": [
"protectedAction"
],
"extensions": {
"code": "UNAUTHENTICATED",
}
}
],
"networkError": null,
"message": "GraphQL error: You must be logged in"
}
}
```


Having well-defined error codes in your error responses lays the groundwork to make GraphQL errors more actionable and universal. The benefits of using them consistently throughout our application stack are pretty compelling:


- Error handlers can read the error codes to distinguish between transient errors (like expired authentication tokens) vs non-transient errors (bad user input), and possibly retry the transient errors automatically.
- Packages and applications can use the standard error codes to build out an error-aware ecosystem of tooling. For example, we can filter out service-level errors that reflect the health of the API, while ignoring errors caused by bad user input.


### Automatic Re-authentication


If you are using JSON Web Tokens to authenticate the origin of a request, you will need some way to refresh the token when it expires, all without impacting the user experience (like forcing the user to authenticate again with their username and password). Having well defined error codes, we can easily use[apollo-link-error](https://github.com/apollographql/apollo-link/tree/master/packages/apollo-link-error) to alter the request flow of a GraphQL request that failed to authenticate. Here is a code example that uses[Apollo Boost](https://github.com/apollographql/apollo-client/tree/master/packages/apollo-boost) :


```text
import ApolloClient from 'apollo-boost';


const client = new ApolloClient({
uri: '<your graphql endpoint>',
// Apollo Boost allows you to specify a custom error link for your client
onError: ({ graphQLErrors, networkError, operation, forward }) => {
if (graphQLErrors) {
for (let err of graphQLErrors) {
// handle errors differently based on its error code
switch (err.extensions.code) {
case 'UNAUTHENTICATED':
// old token has expired throwing AuthenticationError,
// one way to handle is to obtain a new token and
// add it to the operation context
const headers = operation.getContext().headers
operation.setContext({
headers: {
...headers,
authorization: getNewToken(),
},
});
// Now, pass the modified operation to the next link
// in the chain. This effectively intercepts the old
// failed request, and retries it with a new token
return forward(operation);


// handle other errors
case 'ANOTHER_ERROR_CODE':
// ...
}
}
}
},
});
```


We can visualize the modified request flow with this diagram:


The great thing about this approach is that it happens entirely at the request level, intercepting and handling a failing request. Your view layer can remain blissfully unaware of what had just happened.


We can do the exact same thing for network errors, by adding an` apollo-link-retry`[link](https://github.com/apollographql/apollo-link/tree/master/packages/apollo-link-retry) to your link chain. It comes with some additional niceties like specifying a retry strategy with[exponential backoff and random delays](https://aws.amazon.com/blogs/architecture/exponential-backoff-and-jitter/) .


**Takeaway: Strive to handle recoverable/transient errors in the link layer to reduce complexity in the view layer.**


---


## Handling user level errors


At this point, hopefully we have done a good job of dealing with most service level errors nicely, and found ways to smooth over its impact on the user experience. However, users can frequently be the source of errors as well.


Even a well-meaning user or client will not always send data that your GraphQL endpoint expects, so every application needs to validate user input rigorously. In GraphQL, there are two levels of validation errors that could occur:


### GraphQL Type Validation


In GraphQL, we automatically benefit from a strongly typed schema — any time a user types in a` string` when an` int` was expected as input, an error is thrown from the validation phase within[GraphQL-JS](https://github.com/graphql/graphql-js) . Such errors prevent your GraphQL server from executing the query, resulting in the entire query being rejected. On the client, this shows up as a` networkError` with a message that looks like this:


```text
"message": "Variable \"$input\" got invalid value \"foo\"; Expected type Int; Int cannot represent non 32-bit signed integer value: foo"\
```


In general, your application should avoid sending bad queries like this to your GraphQL endpoint. First, it wastes unnecessary server resources, and second, it reduces the ability to provide quick feedback to the user on the UI. I suggest that the best way to deal with type errors is to perform strong client-side validation using packages like[this](https://github.com/chriso/validator.js) . If your schema is available on the client, you can also run the same[validation step](https://github.com/graphql/graphql-js/tree/master/src/validation) GraphQL-JS provides before sending a request.


### Custom Validation in your resolvers


However, validation is surely more than just checking whether the input types match what is expected by the schema — we need to make sure that the inputs are semantically correct. Say you would like to create an app that allows users to post events that are happening in town. The schema for an Event might look something like this.


```text
const typeDefs = gql`
type Event {
name: String!
date: Date!
capacity: Int!
zipCode: String!
}
`;
```


Notice there are already multiple ways that the user could send invalid inputs. He might enter a date in the past, use a duplicate event name, or enter an invalid URL for the display photo. Checks against these types of mistakes typically require the use of server-side data, and should live in your GraphQL resolvers.


### Throwing BadUserInputError in resolvers


Throwing` UserInputError` from Apollo Server gives us a really convenient way of adding field-level validation error information to the error response. Notice that the second argument to` UserInputError` allows you to specify these additional properties to include. A simple example is as follows:


```text
import { UserInputError } from 'apollo-server';


const resolvers = {
Query: {
events(root, { zipCode }) {


// do custom validation for user inputs
const validationErrors = {};
if (!isValidZipCode(zipCode)) {
validationErrors.zipCode = 'This is not a valid zipcode';
}
if (Object.keys(validationErrors).length > 0) {
throw new UserInputError(
'Failed to get events due to validation errors',
{ validationErrors }
);
}
// actually query events here and return successfully
return getEventsByZipcode(zipCode);
}
}
}
```


Failing validation, here is how the response will look on the client:


```text
{
"message": "Failed to get events due to validation errors",
"extensions": {
"code": "BAD_USER_INPUT",
"exception": {
"validationErrors": {
"zipCode": "This is not a valid zipcode"
}
}
}
}
```


While convenient, the weakness of this approach is that the format of the validation error messages is not captured by your schema, making it brittle to changes. Unless you maintain tight control of both server and client, you should keep the error responses as simple as possible.


For mutations, it can be worthwhile defining these validation errors as first class citizens within your schema. For more, you can look at the[schema design guides](https://www.apollographql.com/docs/guides/schema-design.html#mutation-responses) .


## Handling partial failures with nullable types


One of the great things about using GraphQL as your API gateway is the ability to stitch together data from different back-end services, allowing the client to fetch the data it needs with a single query. While this is great if you have a microservice architecture, you’ll also have to think carefully about how to handle partial failures. I strongly recommend[Sashko Stubailo](https://medium.com/u/803918030a60?source=post_page-----5c12da407210----------------------) ’s excellent post on how to use[nullability in GraphQL](https://dev-blog.apollodata.com/using-nullability-in-graphql-2254f84c4ed7) , but here is a takeaway from his discussion as it relates to error handling:


**Use nullable types for fields on which partial failure is acceptable. For critical fields in your query, specify them as non-nullable so the entire query fails when those fields fail to resolve.**


---


## Conclusion


Hopefully this blog post has given you some ideas on how to handle the variety of errors that we will encounter when using GraphQL. For easy reference, here is a flowchart that summarizes the thinking process for using each approach that we have discussed in this post.


Shoutouts:[Evans Hauser](https://medium.com/u/712ce1f56160?source=post_page-----5c12da407210----------------------) for his mentorship and work on Apollo Server 2.0,[Tim Hingston](https://medium.com/u/ed15ffea3672?source=post_page-----5c12da407210----------------------) for coming up with the taxonomy of GraphQL errors,[James Baxley III](https://medium.com/u/fd3d3dea3cd3?source=post_page-----5c12da407210----------------------) whose[talk at Apollo Day 2018](https://youtu.be/9Qw8HKatjy8?t=22m22s) provided some great thinking on this subject.


Written by


Clarence Ngoh


[Read more by Clarence Ngoh](https://www.apollographql.com/blog/author/clarence)
