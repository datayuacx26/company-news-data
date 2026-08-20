---
schema_version: "1.0.0"
document_id: "32876af291fa269f93e6df87e6bb4a082954dbfe3f075d4de811f25f2e80c494"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/integrate-headless-cms-data-with-any-api-in-graphql"
published_at: "2025-04-09T10:03:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:883de1471b3d90c27dee2661f22c355ec269d86cf2475debdf071539c3861449"
---

# Integrate Headless CMS Data with any API in GraphQL

Headless content management systems (CMS) have been increasingly popular because they allow content to be delivered to any device or platform via their APIs. This means you can manage content in one place and display it in any of your client applications. But most client use cases need some other data that doesn’t live in our CMS that we have to orchestrate. This means we either write some code on the client side to orchestrate API calls to our CMS and other needed APIs, or we write a backend for frontend (BFF) to orchestrate those API calls so the client can make a single request. Either way, we’re writing code to orchestrate our requests and potentially duplicating a lot of code between multiple client scenarios (i.e. web vs mobile).


A common use case for headless CMS is to store your product catalog information which is going to require other services like pricing information coming from another API. What if we didn’t have to write code to orchestrate requests like this? With Apollo Connectors for REST, we can quickly bring multiple APIs together with our CMS with some simple config. Let’s walk through how we can do this with Strapi.


## Prerequisites


- [Sign up](https://studio.apollographql.com/signup?referrer=blog_connectors_headless_cms) for a free GraphOS account
- [Install Rover](https://www.apollographql.com/docs/rover/getting-started) – the CLI we’ll use to build and run things locally
- Strapi instance running locally or hosted – you can get started[here](https://docs.strapi.io/dev-docs/quick-start)


***Note** : This post will walk through a v4 instance of Strapi, but if you are using v5, everything is the same except for 1 major differences* . *The response format has been flattened, which means attributes are no longer nested in a*` data.attributes` *object and are directly accessible at the first level of the*` data` *object (e.g., a content-type’s “title” attribute is accessed with*` data.title` *).*


## Step 1: Setting up the project


In our example here, we’ll be using our headless CMS as our product catalog. If you don’t have a` Product` ** collection type defined in your[Content-Type Builder](http://localhost:1337/admin/plugins/content-type-builder) (while running` yarn develop` ), you’ll need to create one with a text` name` ** and **` description` field for the purposes of our post. The` Product` type has also been made[public](https://docs.strapi.io/dev-docs/quick-start#step-4-set-roles--permissions) in this case. Once the content type has been created, you’ll need to create at least one product in the content manager.


*We have additional fields in our Strapi instance, but you need*` name` and` description` at a minimum.


With our CMS ready, we need to setup our project locally. We’ll be using the[Connectors Community GitHub repository](https://github.com/apollographql/connectors-community) to quickly set everything up locally. Let’s first clone that project and install packages:


```text
git clone https://github.com/apollographql/connectors-community
npm install
```


Then we need to create a[GraphOS User API key](https://studio.apollographql.com/user-settings/api-keys) and add it to a` .env` file that we create where we cloned the connectors community repo:


```text
APOLLO_KEY=user:gh.michael-watson:A4MewsBUfXKfLcheB-gGow
```


The last step is to run the setup wizard from that repository using` npm start` . You’ll need to provide a location to setup the connector, create a new graph and provide a name:


Now everything is setup for us to start connecting our CMS REST API 🎉. If you open the project in VS Code, everything is already setup for the Apollo VS Code extension and you should be prompted to install it.


## Step 2: Connecting CMS API


Open up the` connector.graphql` file in your editor and modify the schema to represent exposing our products with some simple pagination:


```text
extend schema
@link(url: "https://specs.apollo.dev/federation/v2.10", import: ["@tag", "@inaccessible"])
@link(url: "https://specs.apollo.dev/connect/v0.1",import: ["@source", "@connect"])
@source(
name: "strapi"
http: {
baseURL: "http://localhost:1337"
# This is only needed if you kept your Product  collection type private
headers: [{ name: "Authorization", value: "Bearer {$config.strapi}" }]
}
)


type Product {
id: ID!
name: String
description: String
}


type Query {
products(start: Int = 0, limit: Int = 3): [Product]
}
```


The schema above also includes the defined` @source` for the connector with the` baseURL` defined for where our Strapi instance lives. If you kept your` Product` collection type private, you’ll need to provide the` Authorization` header and[configure that authentication](https://www.apollographql.com/docs/graphos/schema-design/connectors/router#authentication) in the` router.yaml` file locally.


Now let’s add the` @connect` directive to our` Query.products` field to connect that root entry point to our REST API. We’ll need to perform a` GET` request to` /api/products?pagination\[start\]={$args.start}&pagination\[limit\]={$args.limit}` where` $args` is using the input values we had defined in our schema:


```text
type Query {
products(start: Int = 0, limit: Int = 3): [Product] @connect(
source: "strapi"
http: { GET: "/api/products?pagination[start]={$args.start}&pagination[limit]={$args.limit}" }
selection: """


"""
)
}
```


The final step is to provide a selection that maps the JSON response from Strapi to the schema fields we just defined:


```text
type Query {
products(start: Int = 0, limit: Int = 3): [Product] @connect(
source: "strapi"
selection: """
$.data {
id: product_id
name
description
}
"""
)
}
```


*Note: If you are running Strapi v4, you’ll need to add in $.attributes as mentioned in the prerequisites*


This connector is just scratching the surface of what you can do when mapping your response. Apollo Connectors for REST has a rich set of transformations that you can read up more on in our[documentation](https://www.apollographql.com/docs/graphos/schema-design/connectors/responses) . Now we’re ready to test our connector our locally.


## Step 3: Testing the connector locally


The quickest way to run our connector locally is with the` rover` CLI. The` rover dev` command can be used to start up an Apollo Router instance locally and give us a development sandbox to test our connector. It also will hot reload based on our router configuration files locally. Start the` rover dev` command:


```text
rover dev --supergraph-config supergraph.yaml --router-config router.yaml
```


Once` rover dev` starts up, navigate to` http://localhost:4000` to access the sandbox for Apollo Explorer. Create your first operation and press the play button to execute it, you should see results pop up in the response like below:


If you don’t see data popping up (i.e.` products` returns` null` ), you can open the connectors debugger to see what the errors returned are.


## Step 4: Hosting the Apollo Router


With the connector working locally, it’s time to host the Apollo Router and point it at our hosted Strapi instance. The connector is pointing at our local running Strapi instance, but we’ll want to set things up where the` baseURL` is being overriden for environment-specific APIs. This can be done by adding some configuration to our` router.yaml` :


```text
connectors:
sources:
connector.strapi:
# These configurations apply to connectors with the "strapi" source in the "connector" subgraph
override_url: ${env.STRAPI_URL:-http://localhost:1337}
```


Apollo provides the router as both a binary and as images to run in your containerized deployments. We have a` router-template`[GitHub repository](https://github.com/apollographql/router-template) that contains everything you need to host the Apollo Router and is a great example to follow. In this post, we’ve hosted our Apollo Router on Railway with a simple Dockerfile:


```text
FROM ghcr.io/apollographql/router:v2.0.0


COPY router.yaml /config.yaml


CMD ["--config", "/config.yaml"]
```


While setting up our hosting environment, we also need to add our GraphOS environment variables to the hosted environment. The connectors community setup created a` .env` file in your local connectors project that contains` APOLLO_KEY` and` APOLLO_GRAPH_REF` . You’ll need to set both of these environment variables in your hosted environment.


With the Apollo Router hosted, our last step is to publish our connector to GraphOS and then our Apollo Router will hot-reload via[Apollo Uplink](https://www.apollographql.com/docs/graphos/routing/uplink) .


## Step 5: Publishing the connector to GraphOS


Publishing any schema to GraphOS will trigger a[launch](https://www.apollographql.com/docs/graphos/platform/schema-management/delivery/launch) that represents the complete process of applying a set of updates to your graph. Since the hosted Apollo Router is running the` APOLLO_GRAPH_REF` provided in the environment variables, publishing the connector to that same graph ref


```text
export APOLLO_KEY=...
rover subgraph publish {APOLLO_GRAPH_REF} --name strapi --schema ./connector.graphql
```


## Wrapping up


In this post, we discussed the need to orchestrate other APIs with our headless CMS with an example of our product catalog living in Strapi. Then we walked through how we can connect to the APIs our CMS provides and expose parameters for use cases like pagination. This pattern can be iterated on for any Content-Type you define in Strapi giving you the flexibility to express your CMS with other APIs with no code.


If you want to learn more about connectors, check out our[free tutorial](https://www.apollographql.com/tutorials/connectors-intro-rest) ,[docs](https://www.apollographql.com/docs/graphos/schema-design/connectors) , or[past connectors live episodes](https://www.youtube.com/@ApolloGraphQL/streams) .


Written by


Michael Watson


[Read more by Michael Watson](https://www.apollographql.com/blog/author/watson)
