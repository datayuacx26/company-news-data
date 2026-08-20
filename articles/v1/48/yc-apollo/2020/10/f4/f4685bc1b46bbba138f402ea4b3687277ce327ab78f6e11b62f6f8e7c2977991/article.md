---
schema_version: "1.0.0"
document_id: "f4685bc1b46bbba138f402ea4b3687277ce327ab78f6e11b62f6f8e7c2977991"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/using-apollo-federation-with-local-schemas"
published_at: "2020-10-06T16:05:38+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:df504434b35bd22572ecc4bf7887c0f6c67656e3229edf8b0dee6d13b76ae201"
---

# Using Apollo Federation with Local Schemas

Recently, I needed to create a proxy to a third party API in our GraphQL Federation schema but didn’t want to spin up a new service just for this. This blog post explains how to use local schemas inside the Apollo Federation Gateway.


## A bit of context


At[work](https://team.pollen.co/) , we use the Prismic[CMS](https://en.wikipedia.org/wiki/Content_management_system) to power the content of our site (at least most of it). We are currently working on a new project that is using Apollo Federation as our Gateway[1](https://patrick.wtf/posts/apollo-federation-local-services#fn-1) .


We wanted to add the Prismic API to our Gateway, so as to provide a single graph to our users on the frontend (see[PrincipledGraphQL](https://principledgraphql.com/) ). Unfortunately, when a GraphQL API doesn’t provide support for Apollo Federation, things aren’t as easy as specifying the service in Apollo’s services list.


## A basic Federation example


Let’s supposed that we have one Apollo federation that’s consuming one service for the time being.


```text
const gateway = new ApolloGateway({
serviceList: [{
name: "products",
url: "http://localhost:4002"
}],
});
const server = new ApolloServer({ gateway });
server.listen();
```


As you can see, the ApolloGateway requires a list of GraphQL services as objects with a name and a URL. This is a bit unfortunate as this means that using a local schema doesn’t work by default.


**Note:** we can’t use Prismic’s GraphQL API directly as they don’t provide the necessary fields for federation. In addition to that, they also use GET requests instead of POST requests for the API call, and ApolloGateway always sends POST requests. Lastly, I wanted to keep the code simple. I’ll be happy to provide an example repo with Prismic if it’s needed. Drop me a[tweet](https://patrick.wtf/posts/(https:/countries.trevorblades.com/) in that case!


## What can we do to make this work?


What makes Federation work with multiple services is the Apollo Gateway, which basically acts as the glue between the services. It knows how to compose the schemas and how to create a query plan for executing a query.


From my understanding of the source code, Apollo Gateway seems to support local services, but, as mentioned in this[GitHub Issue](https://github.com/apollographql/apollo-server/issues/3250) it doesn’t allow to support both local and remote services at the same time by default.


Fortunately for us, we can use` buildService` to provide create a custom DataSource for our remote schema.


DataSource is what Apollo Gateway uses to fetch data from a service. That means we can create a DataSource that proxies the requests to Prismic.


We will be using[Trevor Blades’ Countries API](https://countries.trevorblades.com/) , a public GraphQL API that doesn’t support Apollo Federation.


So,` buildService` is called for each service specified in the` serviceList` configuration parameter for Apollo Gateway. Let’s go and update that to have a service for the countries service:


```text
const gateway = new ApolloGateway({
serviceList: [
{ name: 'products', url: 'http://localhost:4002' },
{ name: 'countries', url: 'http://countries' },
]
});
const server = new ApolloServer({ gateway });
server.listen();
```


The only caveat that we have is that we have need to provide a` url` for our service, even it is fake.


The next step is to provide a` buildService` function, which looks like this:


```text
const gateway = new ApolloGateway({
serviceList: [
{ name: "products", url: "http://localhost:4002" },
{ name: "countries", url: "http://countries" },
],
buildService: ({ url }) => {
if (url === "http://countries") {
return new LocalGraphQLDataSource(getCountriesSchema());
} else {
return new RemoteGraphQLDataSource({
url,
});
}
},
});
const server = new ApolloServer({ gateway });
server.listen();
```


Ok, the last step is to create the function that will return the schema for the countries service. Similar to` RemoteGraphQLDataSource` Apollo Gateway also provides a` LocalGraphQLDataSource` which accepts a` schema` as a parameter instead of a` url` . This means that we can provide our own schema!


## GraphQL Tools


Now that we know we can provide our own schema to Apollo Gateway, how can we proxy to an existing schema? We could do this manually, but it would take a bit of time, luckily for us, there’s a package called[graphql-tools](https://www.graphql-tools.com/) that allows using a[remote schema](https://www.graphql-tools.com/docs/remote-schemas) as if it were local.


In addition to that, we also need to use[graphql-transform-federation](https://github.com/0xR/graphql-transform-federation) to provide the required fields for Apollo Federation. Let’s see how our function looks like:


```text
const executor = async ({ document, variables }) => {
const query = print(document);
const fetchResult = await fetch("https://countries.trevorblades.com/", {
method: "POST",
headers: {
"Content-Type": "application/json",
},
body: JSON.stringify({ query, variables }),
});
return fetchResult.json();
};
const getCountriesSchema = async () => {
const schema = wrapSchema({
schema: await introspectSchema(executor),
executor,
});
return transformSchemaFederation(schema, {
Query: {
extend: true,
},
});
};
```


There are a few things going on here. First, we have a function called` executor` . This is our core function that proxies the calls to the` countries` service; it’s not doing anything special. It gets the document and variables and sends them to the` countries` services. For Prismic, we’d add headers for authentication and change the request to be a` GET` . But the rest would pretty much be the same.


Finally, the last piece of the puzzle is` getCountriesSchema` : it uses the utilities from` graphql-tools` and` graphql-transform-federation` to create a proxy schema with support for Federation.


` getCountriesSchema` uses` wrapSchema` to create a schema based on our Countries GraphQL API. We need to pass a schema, an executor, and some optional transforms. We do not use any transform here, but one use case is to rename the types so they don’t conflict with other services.


Finally, it calls` transformSchemaFederation` to obtain a Federation compatible schema, the only option we are passing here is telling Federation that this schema is extending the` Query` type, so all the fields provided here will be composed into the federated schema. We can also pass more options here, but at the moment we didn’t need that.


**Note:** the code in the example is a bit broken as we are not awaiting` getCountriesSchema` in our` buildServices` , the final code provided in the[git repo has the correct solution](https://github.com/patrick91/apollo-federation-local-services/blob/main/server.js#L11) , I wanted to keep the code example simple here.


## Conclusion


As you can see, you need a little bit of code to proxy a non-federated GraphQL service with Apollo Gateway, but it’s not too bad. I think it’s pretty cool we can do this as we can move the code that deals with multiple schemas from our clients to our gateway, simplifying how they fetch data.


Also,` graphql-transform-federation` allows specifying a Federation configuration. This makes it possible to use Apollo Federation features to extend types with a non-federated service, which can definitely make the schema easier to use.


To see a fully working example of this head to the[example repo I’ve written for this blog post](https://github.com/patrick91/apollo-federation-local-services) .


**Note:** all of this post might be deprecated in the future when project constellation gets released to the public. It’s a new feature that will enable consolidation of services that don’t have the (currently) required fields. In the meantime, feel free to try this approach.


Written by


Patrick Arminio


[Read more by Patrick Arminio](https://www.apollographql.com/blog/author/patrick_py)
