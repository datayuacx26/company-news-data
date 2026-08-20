---
schema_version: "1.0.0"
document_id: "f70eb22a6a6fb276568a0eb0fd16518d279893addc615b3d9301455be522adab"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/add-python-to-your-graphql-api-with-graphos-and-strawberry-graphql"
published_at: "2022-12-13T09:04:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:4c7667537efae9b06b4204d585ae6ef6b03b67cb8fdfa3e5dc2c38bc8e01dffe"
---

# Add Python to your GraphQL API with GraphOS and Strawberry GraphQL

A supergraph helps you share responsibility for your organization’s data across multiple backend services (these are known as **subgraphs** ). Different teams can work on different subgraphs in parallel, and better yet, each subgraph can use an entirely different language and framework!


There are loads of[subgraph-compatible libraries](https://www.apollographql.com/docs/federation/building-supergraphs/supported-subgraphs?referrer=python-strawberry-subgraph) to choose from. In this post, we’ll look at how to create a subgraph in Python using the[Strawberry GraphQL](https://strawberry.rocks/) library. Python is an extremely popular language (and deservedly so), and it’s a great choice for building subgraphs.


## What’s Strawberry GraphQL?


Strawberry is a **code-first** GraphQL server library that uses Python type annotations to define GraphQL types.


💡GraphQL server libraries come in two general “flavors”: code-first and schema-first.


- With a **schema-first** library, you create a static schema file using GraphQL’s schema definition language (SDL). You then define resolvers for your fields in your code.
- With a **code-first** library (such as[Strawberry](https://strawberry.rocks/) ), you instead define your schema’s types and fields programmatically, using features of the library and language.


Let’s look at an example of generating GraphQL types with Strawberry. The left snippet below shows three object types defined using SDL. The right snippet shows those same three types defined using Strawberry:


These two snippets look pretty similar! Python’s syntax and type hints provide a code-first experience that’s very “SDL-like”.


Note the following about the Strawberry snippet:


- We annotate each type with` @strawberry.type` . This is specifically the annotation for defining *object* types. You can use` @strawberry.input` and` @strawberry.interface` for input types and interfaces, respectively.


- As mentioned above, Strawberry takes advantage of Python type hints. For example, when writing` buyer: User` , we’re specifying that this class has a field called` buye` r of type` User` . This information is then used by Strawberry to create the GraphQL type.


- Strawberry automatically converts` snake_case` field names to` camelCase` in your schema, so you can use Python casing conventions in your code.


- When defining fields in SDL, we have to explicitly mark non-nullable fields using` !` (for example,` buyer: User!` ). Python’s type system uses the opposite convention, meaning every field is non-nullable by default.To designate a field as nullable with Strawberry, we use the` |` operator with` None` , which indicates that the field’s value can also be` None` .


## Federated directives with Strawberry


If you’re familiar with Apollo Federation, you know that many of its features rely on schema directives (` @key` for[entities](https://www.apollographql.com/docs/federation/entities?referrer=python-strawberry-subgraph) ,[@shareable](https://www.apollographql.com/docs/federation/federated-types/federated-directives/#shareable?referrer=python-strawberry-subgraph) for sharing fields across subgraphs,` <a href="https://www.apollographql.com/docs/federation/federated-types/federated-directives/#shareable?referrer=python-strawberry-subgraph">@override</a>` for overriding another subgraph’s field and so on). Code-first GraphQL libraries like Strawberry need a mechanism to apply these directives, because there isn’t a static schema file to add them to.


Strawberry provides built-in support for applying schema directives. For example, the following Python code:


```text
import strawberry


from strawberry.federation.schema_directives import Key


@strawberry.type(directives=[Key(fields="id")])
class Product:
id: strawberry.ID
```


Will generate the following GraphQL type:


```text
type Product @key(fields: "id", resolvable: true) {
id: ID!
}
```


In addition to this base support,[Strawberry provides useful shortcuts for some federation-specific directives](https://strawberry.rocks/docs/guides/federation#apollo-federation-2-guide) . For example, the snippet above can be simplified like so:


```text
import strawberry


@strawberry.federation.type(keys=[“id”])
class Product:
id: strawberry.ID
```


Which is much cleaner!


Ok! Now that we have some understanding of Strawberry and Federation, let’s see what we are building!


## Extending an existing API into a supergraph


Let’s say we have an existing Node.js GraphQL server that’s deployed as a monolith. This monolith provides an API for an e-commerce store that enables clients to fetch orders, products, and the current user’s cart.


Now let’s say we want to extend this API to provide the shipping cost for each order. Our team wants to create a *separate service* that manages shipping data, and they want to implement that service in Python instead of Node.js.


Let’s see how we can do that using[Strawberry](https://strawberry.rocks/) ,[Apollo Federation](https://www.apollographql.com/docs/federation?referrer=python-strawberry-subgraph) , and[GraphOS](https://www.apollographql.com/docs/graphos?referrer=python-strawberry-subgraph) !


Historically, we’ve needed to self-host a gateway to combine multiple GraphQL APIs using Apollo Federation.


Now with GraphOS, we can set up a **cloud supergraph** that helps us jump into Federation much more quickly. There’s a lot in GraphOS, check out our[docs](https://www.apollographql.com/docs/graphos?referrer=python-strawberry-subgraph) to learn more.


Let’s look at how we can add our monolith to a **cloud supergraph** .


## Setting up a cloud supergraph


To get the most out of Apollo Federation, we usually need to make minor changes to our monolith’s schema. In this blog post we’ll be using a monolith with a schema that’s already been updated for Federation. To learn how you can update your monolith to support federation, read this tech note from the Apollo Solutions Architect team


And feel free to[join our Discord server](https://discord.gg/7sdjgwaYTK) and ask any questions you have there. We’ll be happy to guide you!


The schema for our example monolith already has the Federation directives, to make it easier to test the flow.


Our monolith is currently deployed here:[https://hack-the-supergraph-ecommerce.fly.dev/](https://hack-the-supergraph-ecommerce.fly.dev/) .


We can go to[Apollo Studio](https://studio.apollographql.com/signup?referrer=python-strawberry-subgraph) and create a new cloud supergraph that will use our monolith as its first subgraph. This will enable us to deploy our new service as the *second* subgraph.


[Walkthrough of creating a Cloud Supergraph](https://www.apollographql.com/docs/graphos/quickstart/cloud/)


Once we’ve successfully created our cloud supergraph, we should be able to query data through its GraphOS-hosted router.


Let’s try the[following query](https://studio.apollographql.com/public/patrick91-supergraph-n86wv8/explorer?variant=main) :


```text
query OrderById {
order(id: "1") {
buyer {
username
}
items {
title
price {
amount
currency
}
}
total {
amount
currency
}
}
}


```


Feel free to take some time to explore the other fields as well!


## Extending our Graph


As mentioned above, we want to add the shipping cost to the \`Order\` type. Let’s see how the type currently looks in our existing API:


```text
type Order @key(fields: "id") {
id: ID!
buyer: User!
items: [Product!]!
total: Money!
}
```


And this is what we want our updated \`Order\` type to look like:


```text
type Order @key(fields: "id") {
id: ID!
buyer: User!
items: [Product!]!
total: Money!
<strong>shippingCost: Float!</strong>
}
```


To achieve this, we need to also define the Order type in our *new* subgraph and include the new field, like so:


```text
# in our shipping service:
type Order <strong>@key(fields: "id")</strong> {
id: ID!
shippingCost: Float!
}
```


*Note: if you’re familiar with Apollo Federation 1, you might wonder about the lack of the \`extend\` keyword here This is not required anymore in Apollo Federation 2! Read more about Federation 2[here](https://www.apollographql.com/docs/federation/federation-2/new-in-federation-2?referrer=python-strawberry-subgraph) .*


Let’s go and implement this using Strawberry.


## Implementing our subgraph in Python


Creating a new subgraph always involves some boilerplate code. We need to set up our web server, install dependencies, define a basic schema, and so on.


To help with all of this boilerplate, we can use a subgraph template from the[Rover CLI](https://www.apollographql.com/docs/rover?referrer=python-strawberry-subgraph) !


In a new project directory, let’s create our subgraph using the[Strawberry GraphQL template](https://github.com/strawberry-graphql/subgraph-template-strawberry-fastapi) :


```text
rover template use --template subgraph-python-strawberry-fastapi
```


After applying our template, next we should create a virtualenv and install the dependencies:


```text
python -m venv .virtualenv
source .virtualenv/bin/activate
pip install -r requirements.txt -r requirements-dev.txt
```


We’re ready to build! Let’s head over to` api/schema.py` and start making our changes. Feel free to delete everything from that file, because we’ll implement a completely different schema.


Remember that our goal is to add a shipping cost field to our` Order` type. Let’s see how we can achieve this in our subgraph schema using Strawberry:


```text
import strawberry


def calculate_shipping_cost() -> float:
return 42.0


@strawberry.federation.type(keys=["id"])
class Order:
id: strawberry.ID
shipping_cost: float = strawberry.federation.field(resolver=calculate_shipping_cost)


schema = strawberry.federation.Schema(
enable_federation_2=True,
types=[Order],
)
```


The code above includes three definitions: a resolver, an` Order` type, and the schema itself. Let’s walk through each of those:


Resolvers in Strawberry are Python functions that are called whenever a corresponding field is requested. For simplicity, this resolver always returns 42.0 for now.


To define the` Order` type, we create a Python class with two fields:` id` and` shipping_cost` . We decorate this class with` @strawberry.federation.type` , which indicates that it represents a federated object type. We’re using the federation version of this decorator to make it easier to add the key directive. We also use` strawberry.federation.field` to attach our resolver to the` shipping_cost` field.


We create the schema itself using` strawberry.federation.Schema` . We make sure to enable Federation 2 features and pass our` Order` type to the` types` parameter.


## Trying our subgraph locally


Now that we have a complete schema we can run the API locally to make sure everything starts up successfully:


```text
uvicorn main:app --reload
```


To verify that the resolvers are functioning properly, we have two options. One is to use` rover dev` to[develop and test our supergraph locally](https://www.apollographql.com/docs/graphos/local-development?referrer=python-strawberry-subgraph) . The other is to use[Apollo Sandbox](https://studio.apollographql.com/sandbox/explorer?referrer=python-strawberry-subgraph) or the GraphiQL interface to interact directly with our subgraph.


Using[Sandbox](https://studio.apollographql.com/sandbox/explorer?referrer=python-strawberry-subgraph) , we can see we get the` Query` and` Mutation` types (standard GraphQL), as well as the special` <a href="https://www.apollographql.com/docs/federation/building-supergraphs/subgraphs-overview/#query_service?referrer=python-strawberry-subgraph" target="_blank" rel="noreferrer noopener">_service</a>` and` <a href="https://www.apollographql.com/docs/federation/building-supergraphs/subgraphs-overview/#query_entities?referrer=python-strawberry-subgraph" target="_blank" rel="noreferrer noopener">_entities</a>` fields on` Query` to support[Federation](https://www.apollographql.com/docs/federation?referrer=python-strawberry-subgraph) .


## Deploying our subgraph


Now that we have a ready GraphQL API we should deploy it and add it to our Cloud Router. To keep things simple I’ve prepared a repo with the code above that can be hosted on[Railway](https://railway.app/) in a few clicks:


[https://github.com/patrick91/strawberry-graphos-shipping](https://github.com/patrick91/strawberry-graphos-shipping)


## Adding our subgraph to the supergraph


Once we have deployed our subgraph, we can add it to the supergraph, run the following command to get the schema and add it to the supergraph:


```text
rover subgraph introspect <strong>URL</strong> | \
rover subgraph publish <strong>GRAPH_REF</strong> \
--name shipping \
--schema - \
--routing-url <strong>URL</strong>
```


Replace` URL` and` GRAPH_REF` with your deployed url and the Graph Ref from Apollo Studio.


If everything was fine, you should see a message like this:


```text
The 'shipping' subgraph in 'patrick91-supergraph-n86wv8@main' was updated
The supergraph schema for 'patrick91-supergraph-n86wv8@main' was updated, composed from the updated 'shipping' subgraph
```


Awesome! It’s time to test our supergraph, let’s update our previous query and[request the shipping cost](https://studio.apollographql.com/public/patrick91-supergraph-n86wv8/explorer?explorerURLState=N4IgzghgbgpgJgYQPYBsUwMYBcCWSB2AknCAFwgYDsAnHAEwBGcADALQDMGALBK1wBz821AIwBWOqwwQ67LuzjVKYgGzMQAGnDR4yNJlwEAoviwAnAJ7EyIeRH4wxMNnSq8umGKwgwVvfgoAZtQwgT4wtCAAvkA&referrer=operation_collections&variant=main) :


```text
query OrderWithShippingCost {
order(id: "1") {
buyer {
username
}
items {
title
price {
amount
currency
}
}
total {
amount
currency
}
<strong>shippingCost</strong>
}
}
```


After running it we should get this ouput:


```text
{
"data": {
"order": {
"buyer": {
"username": "test_1"
},
"items": [
{
"title": "Product 1",
"price": {
"amount": 1,
"currency": "USD"
}
},
{
"title": "Product 2",
"price": {
"amount": 2,
"currency": "USD"
}
}
],
"total": {
"amount": 3,
"currency": "USD"
},
<strong>      "shippingCost": 11.5
</strong>    }
}
}
```


And there it is: our shipping cost field!


## Recap


[GraphOS](https://www.apollographql.com/docs/graphos?referrer=python-strawberry-subgraph) enables us to extend and build supergraphs using your preferred language. In this blog post we have seen how to create Cloud supergraph and how to extend an existing monolith using Python and[Strawberry GraphQL](https://strawberry.rocks/) .


To learn more about GraphOS head over to the[GraphOS docs page](https://www.apollographql.com/docs/graphos?referrer=python-strawberry-subgraph) . If you want to learn more about building GraphQL APIs in Python see[Strawberry GraphQL’s website](https://strawberry.rocks/) .


Have questions about GraphOS or building GraphQL APIs? Join our[discord](https://discord.gg/7sdjgwaYTK) and chat with us!


Written by


Patrick Arminio


[Read more by Patrick Arminio](https://www.apollographql.com/blog/author/patrick)
