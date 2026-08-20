---
schema_version: "1.0.0"
document_id: "11008dcec5f3b36bb9161b1ab6ec48a0fbf5e2e2a94078e0e400f9d5f5855b7d"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/announcing-official-apollo-federation-support-for-net-with-hot-chocolate-and-federation-2"
published_at: "2023-10-24T15:46:07+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:a0fb98206b74f39790a27c6dfa7862481b48e97f8cda1de4f5cc9b58a58edac5"
---

# Announcing official Apollo Federation support for .NET with Hot Chocolate and Federation 2!

Apollo Federation is an open specification for exposing API services as a single GraphQL access layer, known as a supergraph. In a supergraph, you connect and curate your services through smaller, modular graphs called subgraphs. There’s a vibrant ecosystem of community-led frameworks that let you write subgraphs in[every common language](https://www.apollographql.com/docs/federation/building-supergraphs/supported-subgraphs) , bolstered by official Apollo Federation support libraries for JavaScript and Java.


Today, we’re excited to announce a new[official Apollo Federation support library for Hot Chocolate](https://www.nuget.org/packages/ApolloGraphQL.HotChocolate.Federation) — a popular .NET GraphQL framework — including full[Federation 2](https://www.apollographql.com/blog/announcement/backend/announcing-federation-2/) support for field migrations, entity interfaces, and more flexible type ownership!


## A growing ecosystem of frameworks


With the help of Apollo’s official Federation support libraries, the community has added Federation support to over[30+ GraphQL frameworks](https://www.apollographql.com/docs/federation/building-supergraphs/supported-subgraphs) that you can use to build subgraphs today.


The Apollo-maintained` subgraph-js` library[enables Federation in JavaScript/TypeScript subgraphs](https://www.apollographql.com/docs/federation/building-supergraphs/supported-subgraphs/#javascript--typescript) :


- Adds Federation support to Apollo Server, Express GraphQL, NestJS, Yoga, and more.
- Builds on top of the core` graphql-js` library, to enable the entire JavaScript ecosystem.
- Apollo helps actively maintain` graphql-js` as a key reference implementation across language ecosystems.


The Apollo-maintained` federation-jvm` library[powers Federation in Java/Kotlin subgraphs](https://www.apollographql.com/docs/federation/building-supergraphs/supported-subgraphs/#java--kotlin) :


- Adds Federation support to Spring GraphQL, Netflix’s DGS Framework, and more.
- Builds on top of the core` graphql-java` library to enable the whole Java ecosystem.


### Expanding official support to .NET


With momentum building in the .NET community, more and more subgraphs are being created using community support for Federation in popular GraphQL packages like GraphQL .NET and Hot Chocolate. To better support the growing number of .NET subgraphs, we’re excited to announce official Apollo support for .NET with a new v1.0[Apollo Federation support library for Hot Chocolate](https://www.nuget.org/packages/ApolloGraphQL.HotChocolate.Federation) . It’s an open source library that adds Federation support to the main Hot Chocolate GraphQL server and is based on the previous community-led implementation for backwards compatibility. We’ve fixed a few things, overhauled the docs, and added support for Federation 2!


Official[compatibility testing for C# / .NET](https://www.apollographql.com/docs/federation/building-supergraphs/supported-subgraphs/#c--net) shows comprehensive Federation 2 support for Hot Chocolate:


## Quick start template for Hot Chocolate subgraphs


The[Rover CLI](https://www.apollographql.com/docs/rover/commands/template) now supports .NET and a new[Hot Chocolate subgraph template](https://github.com/apollographql/subgraph-csharp-hotchocolate-annotation) that includes:


- Annotation-based Federation for clean idiomatic C# code.
- GitHub actions configured for CI/CD using GraphOS schema checks and publishing.
- Railway one click deploy template.


To bootstrap a new Hot Chocolate subgraph project, run:


```text
rover template use --template subgraph-csharp-hotchocolate-annotation
```


Federation 2 is configured into your Hot Chocolate subgraph in` Program.cs`


```text
using ApolloGraphQL.HotChocolate.Federation;


var builder = WebApplication.CreateBuilder(args);


builder.Services
.AddGraphQLServer()
.AddApolloFederationV2()
// register your types and services
;


var app = builder.Build();
app.MapGraphQL();
app.Run();
```


Root query fields for your subgraph are defined in` Query.cs` , which federate into your supergraph for apps to use:


```text
public class Query
{
public Thing? Thing([ID] string id, Data repository)
=> repository.Things.FirstOrDefault(t => t.Id.Equals(id));
}
```


Entity classes like` Thing.cs` define a` Key` ,` ID` , and` Reference Resolver` to enable joining data across subgraphs:


```text
[Key("id")]
public class Thing
{
[ID]
public string Id { get; }


public string? Name { get; }


[ReferenceResolver]
public static Thing? GetThingById(
string id,
Data repository)
=> repository.Things.FirstOrDefault(t => t.Id.Equals(id));
}
```


## Upgrade an existing Hot Chocolate subgraph to Federation 2


Update your` .csproj` file:


```text
<ItemGroup>
<PackageReference Include="HotChocolate.AspNetCore" Version="13.5.1" />
<PackageReference Include="ApolloGraphQL.HotChocolate.Federation" Version="1.0.0" />
</ItemGroup>
```


Use the new official Apollo Federation support library and configure your subgraph to use Federation 2:


```text
using ApolloGraphQL.HotChocolate.Federation;


var builder = WebApplication.CreateBuilder(args);


builder.Services
.AddGraphQLServer()
.AddApolloFederationV2()
// ...
;


var app = builder.Build();
app.MapGraphQL();
app.Run();
```


## What’s next?


Starting today, Apollo will continue to maintain official Federation subgraph support for Hot Chocolate. You can expect ongoing improvements and fixes, as well as full support for the latest Apollo Federation spec changes.


We also plan to work with the community to add[Federation 2 support for other popular .NET packages like GraphQL .NET](https://www.apollographql.com/docs/federation/building-supergraphs/supported-subgraphs/#c--net) .


## Learn more


- Checkout the[updated docs](https://www.nuget.org/packages/ApolloGraphQL.HotChocolate.Federation) to learn more!
- If you have a specific question about the library or code, please start a discussion in the[Apollo community forums](https://community.apollographql.com/) or start a conversation on our[Discord server](https://discord.gg/graphos) .
- If you have improvements you’d like to see please let us know by[opening a GitHub issue](https://github.com/apollographql/federation-hotchocolate/issues/new) !


Written by


Phil Prasek


[Read more by Phil Prasek](https://www.apollographql.com/blog/author/philprasek)
