---
schema_version: "1.0.0"
document_id: "b7abb3e1149c9adc1aceea388c9c3ca032cbeca45d266654264dd41d002d88db"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/adding-rust-to-your-graph"
published_at: "2023-01-04T07:04:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:73f6e3fc4726550720c16c088b47c52aaf3aad358b1d2c9311470d731271d078"
---

# Add Rust to your GraphQL API with GraphOS

There are many excellent reasons to add Rust to your GraphQL backend—like the incredible performance and[correctness](https://polyfloyd.net/post/how-rust-helps-you-prevent-bugs/) that Rust can bring. While Rust is famously good at interoperability with many languages,[GraphOS](https://www.apollographql.com/blog/announcement/platform/introducing-apollo-graphos/) provides an even easier way! There has never been a better time to start oxidizing your API.


Check out this video walking through this content in this blog post:


## Speeding up a monolith


It’s a familiar sight—a monolithic GraphQL application struggling to keep up with your ever-increasing traffic. Inevitably the Rustacean on your team echoes a familiar refrain: “why don’t we rewrite it in Rust?” While that could have been a monumental effort in the past, trying out Rust for a tiny slice of your API takes just a few steps with[Apollo Federation](https://www.apollographql.com/docs/federation/) . Importantly, **it doesn’t matter which language your monolith is implemented in!** The process of adopting Federation is the same regardless.


To start with, we need a **supergraph** . Fortunately, creating a supergraph from our monolith takes[just a few clicks](https://www.apollographql.com/docs/graphos/quickstart/cloud/) with GraphOS. Next, we can use[the Rover CLI](https://www.apollographql.com/docs/rover/) to get started with a new Rust subgraph quickly. Then, after a couple of tweaks to the monolith, our Rust subgraph can extend (and replace) the monolith’s functionality. Finally, we set up some tests and CI/CD so that our new Rust subgraph can be deployed confidently. And that’s it! Those steps are all it takes to add Rust to our monolith. Let’s get started!


We’ll use this simple e-commerce schema to represent our monolith:


```text
type Query {
product(id: ID!): Product
}


type Product {
id: ID!
name: String!
reviews: [Review!]!
averageRating: Float
}


type Review {
id: ID!
subject: String!
body: String!
rating: Int!
author: User!
}


type User {
id: ID!
username: String!
}
```


We’ve decided that the complex math used to calculate` Product.averageRating` is too slow, so we’re going to reimplement the reviews system in Rust. With Federation, we can *override* the target fields, *migrating* them to a new[subgraph](https://www.apollographql.com/docs/federation/building-supergraphs/subgraphs-overview) . The rest of the API will continue to be served from the monolith, with minimal changes required!


## Getting started quickly


[The Rover CLI](https://www.apollographql.com/docs/rover/) provides a collection of helpful templates for getting started with a GraphQL project. We’ll use the` subgraph-rust-async-graphql` template, which creates a new[subgraph](https://www.apollographql.com/docs/federation/building-supergraphs/subgraphs-overview) using Rust and` <a rel="noreferrer noopener" href="https://async-graphql.github.io/async-graphql" target="_blank">async-graphql</a>` .


*Check out all available templates using*` rover template list` *.*


We start by generating some boilerplate for the subgraph with the following command:


` rover template use --template subgraph-rust-async-graphql <PATH>`


Here,` <PATH>` is the path to the folder you want Rover to create with your new project. We’re going to create a “Reviews” subgraph, so we’ll stick it in a folder called` reviews` .


Rover templates generate fully functional GraphQL servers, so we can immediately use` cargo run` in the new` reviews` directory to start the server with[Cargo](https://doc.rust-lang.org/cargo/) . We can then use` rover dev` to[develop our supergraph locally](https://www.apollographql.com/docs/graphos/local-development) or use[Apollo Sandbox](https://studio.apollographql.com/sandbox/explorer) to interact directly with our new subgraph.


Using[Sandbox](https://studio.apollographql.com/sandbox/explorer) , we can see we get the` Query` and` Mutation` types (standard GraphQL), as well as the special` <a rel="noreferrer noopener" href="https://www.apollographql.com/docs/federation/building-supergraphs/subgraphs-overview/#query_service" data-type="URL" data-id="https://www.apollographql.com/docs/federation/building-supergraphs/subgraphs-overview/#query_service" target="_blank">_service</a>` and` <a rel="noreferrer noopener" href="https://www.apollographql.com/docs/federation/building-supergraphs/subgraphs-overview/#query_entities" target="_blank">_entities</a>` fields on` Query` to support[Federation](https://www.apollographql.com/docs/federation/) .


We now have a functional, bare-bones subgraph. We’re ready to start implementing the business logic!


## Entities and` @key`


In a supergraph, an[entity](https://www.apollographql.com/docs/federation/entities/) is an object type that can be uniquely identified and fetched from one or more subgraphs. Different subgraphs can contribute different fields to the same entity, but to a supergraph’s consumers, an entity appears the same as any other object type.


In our monolith, the` Product` type can be uniquely identified by its` id` field. Therefore, the` Product` type is an entity! To contribute to` Product` from our new Rust subgraph, we first need to mark the type as an entity in the monolith. This should be straightforward if our monolith uses one of the[Federation-compatible libraries](https://www.apollographql.com/docs/federation/building-supergraphs/supported-subgraphs) .


*Is your GraphQL server implementation not listed?[Let us know!](https://github.com/apollographql/apollo-federation-subgraph-compatibility/issues/new)*


To enable our monolith subgraph to resolve that entity, we add the` @key` directive and the appropriate code for our monolith’s library to create a “reference resolver”:


```text
type Product @key(fields: "id") {
id: ID!
# other fields omitted
}
```


Now we need to mirror that definition in our new subgraph. All templates come with an example entity, in this case` Thing` . We can use that example as a starting point for our new` Product` type:


```text
//! src/product.rs
use async_graphql::{SimpleObject, ID};


#[derive(SimpleObject)]
pub(crate) struct Product {
pub(crate) id: ID,
}
```


Note that the only field here is` id` . In each subgraph, we only need to define the entity fields that the subgraph accesses or provides. In this case,` id` is the field used to identify the entity in` @key` , so we need it. We haven’t actually *defined*` @key` yet, so let’s do that over in` src/lib.rs` .


To add` @key` to an object type with` async-graphql` , you must[define a reference resolver](https://async-graphql.github.io/async-graphql/en/apollo_federation.html#entities-and-key) —a function that GraphOS calls whenever it needs to fetch entity fields that this subgraph provides.


*Looking for a full code example? Check out[async-graphql ‘s Federation example](https://github.com/async-graphql/examples/tree/master/federation)*


The example code for` Query` in` src/lib.rs` includes both a regular resolver *and* a reference resolver for` Thing` . We’ll delete the regular resolver and update the reference resolver to return a` Product` instead:


```text
//! src/lib.rs
// ... other `use` statements omitted
use crate::product::Product;


mod product;


struct Query;


#[Object]
impl Query {
/// Add `@key(fields: "id")` to `Product`
#[graphql(entity)]
async fn product(&self, id: ID) -> Option<Product> {
Some(Product { id })
}
}
```


The generated code also includes examples for` Mutation` . We don’t need that for our use case, so we delete the` Mutation` type and replace it with` async_graphql::EmptyMutation` . Now, if we run the server with` cargo run` and check Sandbox, we can see that there is no` Mutation` type and only federation fields remain on` Query` . We can check that our product entity exists by running this query to return the complete SDL with directives:


```text
query _service {
_service {
sdl
}
}
```


This is the same query that` rover subgraph introspect` uses to fetch a subgraph’s schema. You can see a nicely formatted version of the complete SDL with the following command:


` rover subgraph introspect http://localhost:4001`


Note that this is *not* the same as standard GraphQL introspection, which does not include directives.


We can also query for a` Product` the same way that GraphOS would:


```text
query product {
_entities(representations: [{__typename: "Product", id: "1"}]) {
__typename
... on Product {
id
}
}
}
```


This doesn’t *yet* unlock any functionality for our supergraph—our new subgraph can resolve references to the` Product` entity, but the router doesn’t *need* to resolve any references because the subgraph doesn’t contribute any fields to` Product` . Let’s finally get to the fun part.


## Migrating fields with @override


The[@override](https://www.apollographql.com/docs/federation/federated-types/federated-directives#override) directive enables a subgraph to take ownership of a field away from another subgraph. In our case, the other subgraph is the existing monolith, and the fields we want to take ownership of are` Product.reviews` and` Product.averageRating` . First, let’s create the` Review` type:


```text
//! src/review.rs
use async_graphql::{SimpleObject, ID};
use crate::user::User;


#[derive(SimpleObject)]
#[graphql(shareable)]
pub(crate) struct Review {
pub(crate) id: ID,
pub(crate) subject: String,
pub(crate) body: String,
pub(crate) author: User,
pub(crate) rating: u8,
}
```


We need to define the *same*` Review` type that was defined in the monolith. By default,[this is not allowed](https://www.apollographql.com/docs/federation/federated-types/sharing-types/#sharing-object-types) to prevent naming conflicts. The @shareable directive indicates that this re-definition is intentional and that the two subgraphs are expected to *share* the type. This directive must be added to *both* definitions of the type (the monolith’s and the new subgraph’s) for composition to succeed:


```text
type Review @shareable {
id: ID!
subject: String!
body: String!
author: User!
rating: Int!
}
```


Composition enforces that the` Review` type is identical in both subgraphs, so we don’t make a mistake when recreating it here.


Note that the` Review.author` field uses a type we haven’t defined:` User` . Our new subgraph doesn’t contribute any fields to` User` , so we can create a shell entity for it (as we did originally with` Product` ):


```text
//! src/user.rs
use async_graphql::{SimpleObject, ID};


#[derive(SimpleObject)]
pub(crate) struct User {
pub(crate) id: ID,
}
```


```text
//! src/lib.rs
#[Object]
impl Query {
// ... other fields omitted


/// Add `@key(fields: "id")` to `User`
#[graphql(entity)]
async fn user(&self, id: ID) -> Option<User> {
Some(User { id })
}
}
```


We also need to make` User` an entity in the monolith:


```text
type User @key(fields: "id") {
id: ID!
# other fields omitted
}
```


This enables our subgraph to focus just on the` Review` type that it’s overriding. The router will resolve the` User` fields of` Review.author` via the monolith.


The last step is to add the fields we care about to` Product` and mark them with` #\[graphql(override_from: "monolith")\]` (` "monolith"` refers to the name of the subgraph in GraphOS) so that our subgraph can take ownership of them:


```text
//! src/product.rs
use async_graphql::{SimpleObject, ID};
use crate::review::Review;


#[derive(SimpleObject)]
pub(crate) struct Product {
pub(crate) id: ID,
#[graphql(override_from = "monolith")]
pub(crate) reviews: Vec<Review>,
#[graphql(override_from = "monolith")]
pub(crate) average_rating: Option<f64>,
}
```


Then, add those fields to the entity resolver for` Product` . Normally we would load these from a data source, but we’ll hard-code them for now for simplicity:


```text
//! src/lib.rs
#[Object]
impl Query {
/// Add `@key(fields: "id")` to `Product`
#[graphql(entity)]
async fn product(&self, id: ID) -> Option<Product> {
// TODO: Fetch these from a data source!
let reviews = vec![
Review {
id: "1".into(),
subject: "A great product!".into(),
body: "I'd definitely buy this again!".into(),
author: User { id: "1".into() },
rating: 5,
},
Review {
id: "2".into(),
subject: "Meh".into(),
body: "Honestly it was fine, but I didn't read the instructions".into(),
author: User { id: "1".into() },
rating: 2,
},
];
let average_rating = reviews
.iter()
.map(|r| r.rating as f64)
.reduce(|accum, item| accum + item)
.map(|sum| sum / reviews.len() as f64);
Some(Product {
id,
reviews,
average_rating,
})
}


// ... other fields omitted
}
```


If we restart the server and check Sandbox, we can now re-query for the` Product` entity and see that it has a` reviews` field:


```text
query product {
_entities(representations: [{__typename: "Product", id: "1"}]) {
__typename
... on Product {
id
averageRating
reviews {
id
subject
body
author {
id
}
}
}
}
}
```


## Testing the entity resolver


Now that we’ve finished our subgraph, let’s add a test to ensure it keeps working as expected. The template from Rover came with a test for` Thing` —let’s update that to test` Product` instead.


First, we’ll rename` tests/thing.graphql` to` tests/product.graphql` and update the contents to match the query we just ran:


```text
# tests/product.graphql
query product {
_entities(representations: [{__typename: "Product", id: "1"}]) {
__typename
... on Product {
id
averageRating
reviews {
id
subject
body
author {
id
}
}
}
}
}
```


Next, we can rename` tests/thing.rs` to` tests/product.rs` and update the test to use the new query and expect the same thing we got from Sandbox:


```text
//! tests/product.rs
//! Tests for the `Product` type, queries are in `product.graphql`


use serde_json::{json, Value};


mod helpers;


async fn run_graphql_query(operation: &str) -> Value {
helpers::run_graphql_query(include_str!("product.graphql"), operation).await
}


#[tokio::test]
async fn get_product_entity() {
let value = run_graphql_query("product").await;


assert_eq!(
value,
json!({
"data": {
"_entities": [
{
"__typename": "Product",
"id": "1",
"averageRating": 3.5,
"reviews": [
{
"id": "1",
"subject": "A great product!",
"body": "I'd definitely buy this again!",
"author": {
"id": "1"
}
},
{
"id": "2",
"subject": "Meh",
"body": "Honestly it was fine, but I didn't read the instructions",
"author": {
"id": "1"
}
}
]
}
]
}
})
)
}
```


Now, running` cargo test` should pass! Our implementation is complete, and it’s time to deploy this subgraph so it can start overriding the monolith.


## CI


The template code comes with GitHub Actions nearly ready to go—so let’s start by creating a GitHub repository for this code to live in. If you have the[GitHub CLI](https://cli.github.com/) you can do this with` gh repo create` . Before we push up the code, we should edit` checks.yaml` and` deploy.yaml` in` .github/workflows` to remove the` if: false` statements. These disable the jobs until we’re able to fill in the required secrets, which we’re going to do right now.


We create a new[Apollo](https://www.apollographql.com/docs/graphos/api-keys) Studio API Key and set it as the secret APOLLO_KEY (e.g., gh secret set APOLLO_KEY). Set APOLLO_GRAPH_REF to the name of the supergraph with the variant included (e.g., “my-graph@main”).


We additionally need to set PRODUCTION_URL to the URL where we’re hosting the new subgraph. We’ll use[Railway](https://railway.app/) for this because it requires no additional config, but you can use any hosting provider you like.


Last thing before we push: we need to change the name of the crate in Cargo.toml to what we want our subgraph to be called. The default GitHub Actions workflow will inspect this name when pushing to Apollo GraphOS. For this example, we’ll use` reviews` :


```text
[package]
name = "reviews"
# ... rest omitted
```


We’ll also have to change the name of the crate in` src/main.rs` and` tests/helpers/mod.rs` to match:


```text
use reviews::app;
```


Now we can push up the code and let the workflows publish the changes to GraphOS! After all the CI jobs pass, GraphOS updates to use our new subgraph instead of the monolith for` Product.reviews` ! We can verify this by checking the query plan and ensuring the data looks correct.


Querying the top-level` product` field first calls into the monolith. Then to get the` reviews` and` averageRating` fields, the router calls into our new Rust subgraph and combines the results. Finally, to fetch the` author` field in` Review` , the router requests it from the monolith.


We overrode a piece of the graph with Rust, and consumers of the graph are none the wiser!


## Conclusion


[GraphOS](https://www.apollographql.com/docs/graphos) makes building polyglot graphs easier than ever. New subgraphs can freely extend and override the graph with just a few changes to an existing monolith. This enables users to implement individual features in whichever language is best for them without worrying about interoperability. So what are you waiting for? Try out[GraphOS](https://studio.apollographql.com/signup?referral=rust-subgraph) and start adding Rust to your graph today! When you’re ready to take the plunge and start breaking up more of your monolith,[check out this guide](https://www.apollographql.com/docs/technotes/TN0013-monolith-to-federation/) .


Want to dig deeper into Rust + GraphQL?[Join our Discord](https://discord.gg/d6rTpS9Wst) to continue the discussion!


Written by


Dylan Anthony


[Read more by Dylan Anthony](https://www.apollographql.com/blog/author/dylan)
