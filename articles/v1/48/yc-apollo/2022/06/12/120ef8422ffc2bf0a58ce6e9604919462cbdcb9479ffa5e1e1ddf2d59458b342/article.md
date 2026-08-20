---
schema_version: "1.0.0"
document_id: "120ef8422ffc2bf0a58ce6e9604919462cbdcb9479ffa5e1e1ddf2d59458b342"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-to-use-apollo-sandbox-on-your-localhost"
published_at: "2022-06-14T08:53:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:6c24bdd1ee9351bdd115c56626581f7233137d0cbb5e847b024fcf144714782d"
---

# How to use Apollo Sandbox on your localhost

Two years ago we set out to build Apollo Explorer – the GraphQL IDE we felt was missing from the GraphQL community. And last summer, we made Explorer available for local development by releasing it in[Apollo Sandbox](https://www.apollographql.com/blog/announcement/platform/apollo-sandbox-an-open-graphql-ide-for-local-development/) . But Sandbox still ran on the Apollo Studio domain, and our #1 challenge has been distributing Explorer in a way that gives you enough control over the network properties of your queries. Authentication, CORS, firewalls, you name it – a GraphQL IDE exists to let people write queries, and graph admins need to be able to control the network properties of those HTTP requests, full stop.


We added a lot of controls to Explorer to help. We made a way for graph admins to set default headers on their graphs. We built a mechanism for you to define preflight scripts which can do things like automatically refresh tokens in the headers on your graph. But in the end, we have found that there’s no setting we can add to Studio that can replace what **you** can do with full control over the network requests from our IDE.


**That’s why today we are giving Sandbox to *you* .**


We are over the moon to share with you that we have made Apollo Sandbox available for you to **embed on any website** , not just on Apollo Studio. ✨🌙 ✨


## How to embed Apollo Sandbox


### Embedding Apollo Sandbox with HTML


We are now distributing the full-featured code of Apollo Sandbox over a CDN, so you can embed it on any website you’d like. All you need is a little HTML:


```text
<div id="sandbox" style="position:absolute;top:0;right:0;bottom:0;left:0"></div>
<script src="https://embeddable-sandbox.cdn.apollographql.com/_latest/embeddable-sandbox.umd.production.min.js"></script>
<script>
new window.EmbeddedSandbox({
target: "#sandbox",
// Pass through your server href if you are embedding on an endpoint.
// Otherwise, you can pass whatever endpoint you want Sandbox to start up with here.
initialEndpoint: window.location.href,
});
// advanced options: https://www.apollographql.com/docs/studio/explorer/sandbox#embedding-sandbox
</script>
```


### Embed Sandbox by default for Apollo Server


If you’re using Apollo Server, you can easily turn on embedding for Sandbox with[a new embed parameter](https://github.com/apollographql/apollo-server/blob/main/CHANGELOG.md#v380) that we released in v3.8.0. This will be the default experience in Apollo Server 4.


```text
const {
ApolloServerPluginLandingPageProductionDefault,
ApolloServerPluginLandingPageLocalDefault
} = require('apollo-server-core');


const server = new ApolloServer({
...
plugins: [
process.env.NODE_ENV === "production"
? ApolloServerPluginLandingPageProductionDefault({
embed: true,
graphRef: "plaid-gufzoj@current"
})
: ApolloServerPluginLandingPageLocalDefault({ embed: true })
]
});
```


### Embed using React (soon!)


We will be releasing a React component for embedded Sandbox that you will be able to import from our` @apollo/sandbox` package in the near future. We will update this blog post with some example code when it’s available.


## More than just an IDE


We know there are a number of GraphQL IDEs to pick from these days… and we know we’re biased, but we’re particularly excited to bring Sandbox to you because it’s so much more than just an IDE.


### Saved operations (in the cloud) ☁️


Over the years, we’ve met many developers who have told us they were scared to restart their computer or clear their browser cookies because they didn’t want to lose their queries in GraphiQL/Playground.


It’s 2022, and we figured that it’s about time for that to be a problem of the past. 🕰️


Connect your local Sandbox to your Studio account to **save operations in the cloud** and never worry about losing your work again.


### Schema diffs & checks


Have you ever worked with a schema so large it was distributed across multiple files or maybe even multiple repos? What about a GraphQL server that generates its schema from code and doesn’t even have an SDL file?


When developing graphs, sometimes all you want is a simple schema diff. But due to the implementation details of our graphs, that’s not always easy to accomplish. So we built it right into Sandbox.


You can diff your local schema against any graph in Apollo Studio, and if the changes look good, you can even run[schema checks](https://www.apollographql.com/docs/studio/schema-checks) from Sandbox to make sure they pass CI before checking them into code.


### Automatic schema reference


One of the best properties of GraphQL is that it’s self-documenting. We’ve always felt like squashing graph docs alongside a query IDE just didn’t do enough justice to the act of exploring the graph or referencing something in particular. That’s why we built a full-fledged automatic schema reference tool in Studio.


Sandbox also comes with Studio’s schema reference tool:


### Apollo Explorer


And of course, Sandbox has the Apollo Explorer and every feature we’ve built into it over the last several years.[Explorer](https://www.apollographql.com/docs/studio/explorer/explorer) is by far the most sophisticated GraphQL IDE in the ecosystem.


We love Explorer so much because it’s not just a playground for you to query graphs, it’s a playground for us as developers to build the things we always felt were missing from GraphQL tooling. As developers, there’s so much power we’ve gotten used to having in our IDEs and the GraphQL DX is still catching up in a lot of ways. We get new ideas every week about how to improve the Explorer just from using it ourselves.


Explorer has become a passion project of ours, and it is our expression of love for the GraphQL DX itself.


**That’s all for now folks!**


We’re very happy to be bringing this to you today. It’s the culmination of a couple of years of dreaming within the Apollo zoom tunnels, now turned into reality. We’d love to see what you do with Sandbox and where you take your graphs. ❤️


Tweet at us and share what you build! 🐦


As always, happy querying 👋


Written by


Danielle Man


[Read more by Danielle Man](https://www.apollographql.com/blog/author/dman)
