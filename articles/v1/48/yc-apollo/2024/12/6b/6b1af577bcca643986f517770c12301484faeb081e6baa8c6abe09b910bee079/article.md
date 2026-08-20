---
schema_version: "1.0.0"
document_id: "6b1af577bcca643986f517770c12301484faeb081e6baa8c6abe09b910bee079"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/effortless-data-fetching-introducing-apollo-connectors-for-rest-apis"
published_at: "2024-12-05T14:00:17+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:58:43.171296+00:00"
content_hash: "sha256:9e613030ebda4890aa09443ef9d3fbbab70edf31c0e1b728cff974bc205a51c2"
---

# Effortless Data Fetching: Introducing Apollo Connectors for REST APIs

Writing resolvers for REST APIs has been a rite of passage for developers working with GraphQL, but what if there were an easier way? Meet Apollo Connectors.


As shown in Figure 1, Apollo Connectors mitigate the need to write code that connects to and returns data from REST APIs, reducing code surface area and time so you can focus on the business problem you’re trying to solve.


*Figure 1: Data flow showing Apollo Connector for REST APIs*


In this article, we will walk through a React client web application that leverages the Apollo GraphQL client to call a public[CurrencyAPI](https://currencyapi.com/) web service using the REST API Apollo Connector for REST APIs. A screenshot of the UI is seen in the following figure:


*Figure 2: Screenshot of the Currency Converter*


Under the covers, the React app uses the GraphQL Client as usual and issues the query statement. There are no changes there. On the backend, you simply define the GraphQL schema file and that’s it. There is no need to create resolvers to fetch the data from the REST API. In the following sections, you will learn how to set up and configure the Apollo Connector for REST APIs.


## Getting Started


In this article we will use a third party service called CurrencyAPI to fetch real-time currency exchange rates. At the time of this writing (Nov 2024), CurrencyAPI offers a free API key. If you wish to follow along, create an API key from[https://currencyapi.com/](https://currencyapi.com/) and copy it down, as you will need it later.


You will also need an Apollo account and the latest version of Apollo Rover CLI. See sections 1 and 2 of the following Apollo Connector tutorial for installation instructions:


[https://www.apollographql.com/tutorials/connectors-intro-rest/02-setup](https://www.apollographql.com/tutorials/connectors-intro-rest/02-setup)


Next, clone the repository.


` git clone https://github.com/RWaltersMA/CurrencyConverter.git`


## Obtain API keys and configure demo


At the time of this writing, Apollo Connectors is in preview with an anticipated general availability in early 2025. This blog will be updated as this feature evolves.


Now, we are ready to configure Apollo Studio and obtain our API Key. Navigate to[Apollo Studio](http://studio.apollographql.com/?_gl=1*tm0r8z*_gcl_aw*R0NMLjE3MzE3MDM4ODQuQ2owS0NRaUFfOXU1QmhDVUFSSXNBQmJNU1BzRm5OM1NIXzRWQWN2VHJGRFBrQkctcVltVk9ncU1iLUJjczh4YkpBOVlUZzFGQ3MyWngwZ2FBaHBfRUFMd193Y0I.*_gcl_au*MTM2NzI4NDM4Ny4xNzMwMjIwNjQ0) and click “Create a new Graph.” In the “Publish your schema using Federation” dialog, you will see toward the bottom the API Key to be used with Rover.


Below is a sample:


```text
APOLLO_KEY=service:Currency-02wxjh:xxxxxxxxxxxxxxx \
rover subgraph publish Currency-02wxjh@current \
--schema ./products-schema.graphql \
--name your-subgraph-name \
--routing-url http://products.prod.svc.cluster.local:4001/graphql
```


Copy the value for` APOLLO_KEY` . Open the` rover-dev.md file` and update the section with your personalized` APOLLO_KEY` . Copy the service name to` APOLLO_GRAPH_REF` and save the file.


Next, update the` router-config.yaml` file and replace the placeholders “<<REPLACE>>” with the API Key you obtained from currencyAPI and save this file.


```text
preview_connectors:
subgraphs:
currency:
$config:
# Replace this value with your currencyAPI key
apikey: <<REPLACE>>
```


Now, we are ready to start a local Apollo instance. Copy and paste into a terminal the` rover dev` statement that is within your` rover-dev.md` file. For example, the command for` sh` looks like the following:


```text
APOLLO_KEY="service:Currency-02wxjh:xxxxxxxxxx" \
APOLLO_GRAPH_REF="Currency-02wxjh@current" \
APOLLO_ROVER_DEV_ROUTER_VERSION=2.0.0-preview.1 \
rover dev --supergraph-config supergraph.yaml --router-config router-config.yaml
```


If successful, the console will display the following:


```text
supergraph config loaded successfully
warning: Do not run this command in production! It is intended for local development only.
==> Watching /Users/robwalters/Code/CurrencyConverter/currency.graphql for changes
WARN: Connector debugging is enabled, this may expose sensitive information.
==> your supergraph is running! head to http://localhost:4000 to query your supergraph
successfully composed with version =2.10.0-preview.0
successfully composed with version =2.10.0-preview.0 after updating the 'currency' subgraph
```


A note about` router-config.yaml` : In this example, we are launching a react client on the same machine as Rover. Since these are operating on different ports, the client on localhost:4000 and Rover on localhost:3000, the server needs to address CORS (Cross origin resource sharing). To do this, we added the following configuration to the` router-config.yaml` file:


```text
cors:
allow_any_origin: true
```


Note that setting CORS to allow any origin option is not recommended for production use unless necessary. For more information on configuration parameters in the router, check out the[online documentation](https://www.apollographql.com/docs/graphos/reference/router/configuration#yaml-config-file) .


## Starting the client


Navigate to the **client** folder and issue:


` npm install`


Then


` npm start`


Once launched, a web browser will open to localhost:3000 and display the UI shown in Figure 2.


Enter any value into the amount and select your favorite base and desired currency. Press Convert to see the most current amount in your desired currency.


Let’s take a look at what is happening under the covers. First let’s look at the client code, specifically the` convert.js` file within the **src/components** folder.


```text
// GraphQL query to fetch currencyList and listExchangeRate
const GET_currencyList = gql`
query Query {
currencyList {
symbol
name
}
listExchangeRate {
symbol
rate
}
}
`;
```


Our simple example has two queries` currencyList` **,** which returns the complete list of symbols and names for the currency, and` listExchangeRate` **,** which returns the current rate for each symbol. These data are used to populate the drop-downs in the UI. When using Apollo Connectors, the client does not need to make any changes. It is business as usual for your front-end folks. The real magic is in the back end.


## Taking a closer look at the Apollo Connector


Open the` currency.graphql` file. This file contains the configuration for the REST Apollo Connector.


```text
extend schema
@link(
url: "https://specs.apollo.dev/federation/v2.10"
import: ["@key"]
)
@link(
url: "https://specs.apollo.dev/connect/v0.1"
import: ["@source", "@connect"]
)
@source(
name: "v1"
http: {
baseURL: "https://api.currencyapi.com/v3/"
headers: [{ name: "apikey", value: "{$config.apikey}" }]
}
)


type Query {
"A list of currencies"
currencyList: [Currency]
@connect(
source: "v1"
http: { GET: "/currencies" }
selection: """
data->entries {
symbol: key
name: value.name
}
"""
)


"List exchange rates"
listExchangeRate: [ExchangeRate]
@connect(
source: "v1"
http: { GET: "/latest" }
selection: """
data->entries {
symbol: key
rate: value.value
}
"""
)
}


type Currency {
symbol: String
name: String
}


type ExchangeRate {
symbol: String
rate: Float
}
```


This is all that is needed for your Apollo Server to connect to a REST API, no extra resolver code! Looking at the GraphQL query, you’ll see an` @link` statement referencing Apollo Connector.


```text
@link(
url: "https://specs.apollo.dev/connect/v0.1"
import: ["@source", "@connect"]
)
```


Here, we are importing the` @source` and` @connect` directives. The` @connect` requires additional configuration parameters such as the details about the HTTP request, any headers, and a parameter called “selection” which filters the data from the REST API results. Here is the` currencyList` field in our example:


```text
"A list of currencies"
currencyList: [Currency]
@connect(
source: "v1"
http: { GET: "/currencies" }
selection: """
data->entries {
symbol: key
name: value.name
}
"""
)
```


In this query, the Apollo Connector will use the base url defined in the` @link` and append the string defined as part of the HTTP request type in this case is GET. Since our currencyAPI endpoint requires an authentication header, we add the **headers** field and add a key-value pair referencing the API Key and value we defined earlier in the` router-config.yaml` file. Next, we need to tell the connector what data to extract from the API results. We define this in the **selection** parameter.


```text
data->entries {
symbol: key
name: value.name
}
```


For reference, the data that returns from the CurrencyAPI is similar to the following:


```text
{
"data": {
"AED": {
"symbol": "AED",
"name": "United Arab Emirates Dirham",
"symbol_native": "د.إ",
"decimal_digits": 2,
"rounding": 0,
"code": "AED",
"name_plural": "UAE dirhams",
"type": "fiat",
"countries": [
"AE"
]
},
...
}
}
```


For our client we want to return just the symbol and name of each of the currencies. Since our return result has all our data under the “data” field, we start with data and iterate through each subdocument using the **entities** keyword. For each of these **entities,** we return the key, which in this example result is “AED” and add that value to a “symbol” field. We give the field “name” the value of the name field in the result. For a complete discussion and more examples of selection parameters, see the[online documentation](https://www.apollographql.com/docs/graphos/schema-design/connectors/selection) .


## Summary


Building resolvers for REST APIs can be a complex yet essential task for developers working with GraphQL. However, Apollo Connectors now provide a simpler, more efficient way to integrate REST APIs into your GraphQL schema, eliminating the need for custom resolver code. In this article, you learned how to setup and configure an Apollo Connector for REST APIs that query data from the CurencyAPI public API endpoint. Apollo Connectors is currently in preview, for more information, check out the[Apollo Connectors](https://www.apollographql.com/graphos/apollo-connectors) website.


## Call to action


Curious how Apollo Connectors can streamline your GraphQL workflows? Sign up for a[demo](https://www.apollographql.com/connectors-demo) or join our upcoming webinar on December 12th, 2024:[API Orchestration in 30 Minutes: A Stripe Checkout Workflow with Apollo Connectors](https://www.apollographql.com/events/api-orchestration-in-30-minutes-a-stripe-checkout-workflow-with-apollo-connectors) . After attending this webinar, you will have a solid understanding of orchestrating API calls with Apollo Connectors in the context of real-world use cases. If you are reading this after the event date, the webinar will be available on-demand.


Written by


Robert Walters


[Read more by Robert Walters](https://www.apollographql.com/blog/author/robertwalters)
