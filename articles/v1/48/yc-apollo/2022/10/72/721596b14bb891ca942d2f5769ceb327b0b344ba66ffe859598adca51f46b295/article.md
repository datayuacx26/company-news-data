---
schema_version: "1.0.0"
document_id: "721596b14bb891ca942d2f5769ceb327b0b344ba66ffe859598adca51f46b295"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-ios-client-1-0-with-improved-code-generation"
published_at: "2022-10-03T16:49:21+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:476aa709e8e660b185cc7a24f394923c87dd2d11165e8fe812af2fa75c3f053b"
---

# Apollo iOS Client 1.0 with improved code generation

As developers, we all know code generation can save us a ton of time, but can also be a pain point if the generated code needs to be refactored. When it works as expected code generation provides us with: productivity, simplification, portability, and consistency. When it doesn’t work as expected we experience consistency issues and code maintenance overhead.


The primary challenge with code generation is many tools end up exporting unnecessarily verbose code that you have to spend hours refactoring just to get it to where you need it to be. In some cases it’s just easier to write your code from scratch instead of relying on the generated code.


When we started to gather feedback on our now legacy[Apollo iOS client](https://www.apollographql.com/docs/ios/v0-legacy) v.51 we realized one of the biggest pain points our users had was with the generated code being provided by the Client SDK. This was leading to a lot of rework of the generated code or in some cases having to completely throw the code out and re-write it from scratch. To add to the challenges, the previous code generation tooling was written in Typescript which isn’t native for most iOS developers who use Swift as their primary development language.


At Apollo we strive to provide developer solutions and tooling that makes developers lives easier so you can join us on our journey to the **[supergraph](https://www.apollographql.com/blog/announcement/backend/the-supergraph-a-new-way-to-think-about-graphql/?referrer=blog) .** Today we are very excited to announce the new[Apollo iOS Client 1.0](https://www.apollographql.com/docs/ios/) which offers an entirely new approach to code generation.


**1.0 includes the following improvements:**


- Removal of all dependencies on Typescript and NPM
- A complete rewrite of the code generation tooling in Swift
- A greatly improved runtime performance
- Improved syntax for the use of[GraphQL Nullablilty](https://www.apollographql.com/blog/graphql/basics/using-nullability-in-graphql/)
- A reduction in overall generated code size and generated code complexity
- A reduction in the overall time to generate code
- A simpler way to construct fragment objects from the generated data objects
- Improved data validation
- Improved ease of use and overall flexibility
- Improved ability to build tests around the generated models
- Easier access to code generation – In previous versions you had to write code for your code generation configuration. Now with a simple configuration file you can get command line access to code generation.


### **Generating Code in Apollo iOS 1.0**


[Apollo iOS 1.0](https://www.apollographql.com/docs/ios/code-generation/introduction) generates operation objects for your GraphQL operations, providing a type-safe way to construct operations and provide values for input variables in your Swift code. Each generated operation contains a set of robust, strongly-typed models for its response. These generated models help you access your GraphQL response data in a type-safe and flexible format. Because generated response models are operation-specific, they include properties only for the GraphQL fields included in their corresponding operation. This means you can rely on the Swift type checker to flag data access errors at compile time.


To generate models, Apollo iOS 1.0 requires two input sources:


1. **A GraphQL Schema:** which you can obtain fro your GraphQL server or from[Apollo Studio Explorer Sandbox.](https://studio.apollographql.com/sandbox/explorer/) The schema is just a list of all of the possible queries and data types that are available to you. The schema can be thought of as a “ *code generation contract* ” of what it’s *possible* to ask for.
2. **A set of GraphQL operations and /or fragments:** A GraphQL operation defines an interaction with your schema. A GraphQL operation can be a query, a mutation, or a subscription. All operations define a set of fields from the types in your schema to fetch data for. The operation definition determines what the response your GraphQL server provides will include. Your operation input sources can also include files that define[GraphQL fragments](https://www.apollographql.com/docs/ios/fetching/fragments) These fragments can be referenced by your operation definitions.


The most basic way to think about this is the following equation:


**Schema + Operations = Generated Code**


Apollo iOS combines the type information from your schema with your operation definitions to generate models. The schema provides the information needed to generate type-safe models, and your operations define the shape and structure of the generated models.


The Apollo iOS Code Generation Engine parses your schema and operations and ensures that your operations are valid to be performed against the schema provided. It generates models that include all of the necessary information to create type-safe operations, send those operations as network requests, and parse the response data onto the type-safe response models.


### **Generated Code: Operations**


For example, given the following schema:


```text
type Query {
hero: Character!
}
interface Character {
id: String!
name: String!
friends: [Character!]
appearsIn: [Episode]!
}
type Human implements Character {
id: String!
name: String!
friends: [Character]
appearsIn: [Episode]!
height(unit: LengthUnit = METER): Float
}
type Droid implements Character {
id: String!
name: String!
friends: [Character]
appearsIn: [Episode]!
primaryFunction: String
}
```


And the following query:


```text
query HeroAndFriendsNames {
hero {
name
friends {
id
name
}
}
}
```


Apollo iOS generates a type-safe model that looks something like this (details are omitted to focus on the class structure):


```text
class HeroAndFriendsNamesQuery: GraphQLQuery {
struct Data: SelectionSet {
let hero: Hero


struct Hero: SelectionSet {
let id: String
let name: String
let friends: [Friend]?


struct Friend: SelectionSet {
let id: String
let name: String
}
}
}
}
```


Because the` HeroAndFriendsNames` query doesn’t fetch` appearsIn` , this property is not part of the returned result type and cannot be accessed here. Similarly,` id` is only accessible in` Friend` , not in` Hero` .


Because GraphQL supports nullability, you have compile-time type safety. If the request is successful, all queried data (and only this data) will be accessible. There is no need to handle null fields in UI code.


### **Generated Code: Fragments**


A named fragment is defined in a` .graphql` file, just like an operation definition. A named fragment always has a name and a “parent type”, that is, the type in the schema that it can be applied to. This fragment can be included in a selection set in an operation definition.


Apollo iOS generates separate result types for named fragments, which means they are a great way of keeping UI components or utility functions independent of specific queries.


```text
fragment HeroDetails on Character {
name
appearsIn
}
```


```text
query HeroAndFriends {
hero {
name
...HeroDetails
friends {
...HeroDetails
}
}
}
```


Apollo iOS generates a type-safe model for the` HeroDetails` fragment that looks like this (details are omitted to focus on the class structure):


```text
struct HeroDetails: MySchema.SelectionSet, Fragment {
let name: String
let appearsIn: [Episode]
}
```


The result data models generated for the` HeroAndFriendsQuery` will include the fields from the` HeroDetails` fragment and are able to be converted into the` HeroDetails` fragment model.


```text
class HeroAndFriendsQuery: GraphQLQuery {
struct Data: SelectionSet {
let hero: Hero


struct Hero: SelectionSet {
let name: String
let friends: [Friend]
let appearsIn: [Episode]


// Fragment Conversion Declaration
var fragments: Fragments
struct Fragments {
var heroDetails: HeroDetails { ... }
}


struct Friend: SelectionSet {
let name: String
let appearsIn: [Episode]


// Fragment Conversion Declaration
var fragments: Fragments
struct Fragments {
var heroDetails: HeroDetails { ... }
}
}
}
}
}
```


### The Codegen CLI


The new[Codegen CLI](https://www.apollographql.com/docs/ios/code-generation/codegen-cli) provides a command line tool that streamlines the process of running code generation. The CLI can be ran manually from the Terminal (or any other shell program) or can be called into from bash scripts. The CLI is a powerful tool that comes out of the box with our new iOS client 1.0


**The Codegen CLI has three primary commands:**


- **Initialize** : Initializes an` apollo-codegen-configuration.json` file that can be used to configure how the CLI generates code.
- **Fetch Schema** : Fetches your GraphQL schema and writes it to a file. The schema is required in order to run code generation.
- **Generate:** Runs the code generation engine using the configuration in your` apollo-codegen-configuration.json` file.


### **Conclusion**


This blog highlights just some of the great new features of the new Apollo iOS Client 1.0 and we invite you to read the complete[iOS 1.0 documentation](https://www.apollographql.com/docs/ios/) for more details! If you are using our current[Legacy 0.X version](https://www.apollographql.com/docs/ios/v0-legacy) of the Apollo iOS client we invite you to upgrade to 1.0 soon. Please note, that all existing features of 0.x are still fully supported in 1.0.


Finally, we welcome all feedback and input you might have so please let us know what you think in the[Apollo iOS Client Github Repository](https://github.com/apollographql/apollo-ios) .


Written by


John Vajda


[Read more by John Vajda](https://www.apollographql.com/blog/author/jpvajda)
