---
schema_version: "1.0.0"
document_id: "a6909f2c49d2f12c45ee6521f4a433ac16955183f07c7b5567581a443b101e23"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/mastering-cache-key-resolution-in-apollo-ios-with-typepolicy-and-fieldpolicy"
published_at: "2025-11-10T10:37:20+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:7e3afc489abe5874f45ce979486149ae97c19a2e30a44de5e6873cc64d748065"
---

# Mastering Cache Key Resolution in Apollo iOS with @typePolicy and @fieldPolicy

When working with Apollo iOS’s normalized cache, one of the most critical decisions you’ll make is how to identify and retrieve cached objects efficiently. The` @typePolicy` and` @fieldPolicy` directives provide a powerful declarative approach to cache key resolution, enabling your application to intelligently store and retrieve data while minimizing network requests and avoiding cache duplication.


## Overview: The Cache Key Resolution Problem


Apollo iOS uses a normalized cache to store GraphQL responses. This cache breaks down nested query responses into individual objects and stores them in a flat, key-value structure, allowing different queries to share cached data. Each object is stored as a separate cache entry identified by a unique cache key. By default, cache keys are based on the object’s path in the response, but for optimal de-duplication across queries, cache keys can be configured to use unique identifiers from the schema (like id fields), ensuring the same object is never duplicated regardless of how it’s fetched.


By default, Apollo iOS resolves cache keys using the response path of an object within a query. While this works, it can lead to data duplication when the same entity appears in different queries at different paths. For example, a` Book` object fetched through` query { book(id: "123") }` and another fetched through` query { featuredBooks }` might be stored as separate cache entries, even though they represent the same book.


The` @typePolicy` and` @fieldPolicy` directives solve this problem by allowing you to define how cache keys should be computed:


- **` @typePolicy`** : Derives cache keys from fields on the fetched object when **writing to the cache** . This ensures that objects with the same identity are stored only once, regardless of where they appear in your query structure.
- **` @fieldPolicy`** : Derives cache keys from field arguments when **reading from the cache** . This enables the cache to locate objects before making network requests, potentially eliminating unnecessary round trips to your server.


These directives work together seamlessly. Type policies ensure consistent storage of objects, while field policies enable intelligent retrieval based on query parameters. When a field policy can construct a cache key that matches one created by a type policy, the cache can serve the result immediately without network access.


## Understanding Normalized Caching


Before diving into the directives themselves, it’s important to understand what normalized caching is and why it matters. This[article](https://www.apollographql.com/blog/demystifying-cache-normalization) gives a good detailed explanation of cache normalization.


In a normalized cache, the server’s nested GraphQL response is transformed into a flat data structure where each object is stored with a unique cache key. This approach offers several advantages:


**Data Consistency** : When the same object appears in multiple queries, it’s stored only once. Updates to that object automatically reflect across all queries that reference it.


**Efficient Lookups** : Apollo iOS provides two cache implementations: an in-memory cache (` InMemoryNormalizedCache` ) that stores the hash map in RAM for O(1) lookup time, and a SQLite-backed cache (` SQLiteNormalizedCache` ) that persists data across app sessions while still maintaining fast indexed lookups.


**Reduced Memory Footprint** : Eliminating duplicate storage of the same entity reduces overall memory consumption.


## Deep Dive: The @typePolicy Directive


The` @typePolicy` directive is your primary tool for ensuring objects are stored consistently in the cache based on their intrinsic identity rather than their position in a query response.


### How Things Work Without @typePolicy


Without a type policy, Apollo iOS generates cache keys based on the response path. Consider this query:


```text
query GetBook {
book(id: "123") {
id
title
author
}
}
```


The cache might store this book with a key like` QUERY_ROOT.book` , which is derived from its location in the query response tree. If another query fetches the same book through a different path:


```text
query GetFeaturedBooks {
featuredBooks {
id
title
author
}
}
```


The same book would be stored again at` QUERY_ROOT.featuredBooks.0` . Now you have two copies of the same data, wasting memory and creating potential inconsistency issues.


### Using the @typePolicy Directive


To solve this, you declare a type policy in a schema extension file. Schema extension files use the` .graphqls` extension and should be included in the` schemaSearchPaths` of your code generation configuration.


For example, create a file like` schema-extensions.graphqls` and add a type extension like this:


```text
extend type Book @typePolicy(keyFields: "id")
```


With this declaration, Apollo iOS now computes cache keys for` Book` objects using their` id` field from the query response. The cache constructs keys using the format` ${TypeName}:${Identifier}` . A book with` id: "123"` in the response data will always have the cache key` Book:123` , regardless of where it appears in any query response or what input variables were used to fetch it.


The` keyFields` argument accepts a string specifying which fields to use for the cache key. These must be scalar fields (String, Int, Boolean, etc.). When the cache writes a` Book` object, it extracts the` id` field’s value and constructs the cache key` Book:123` .


**Multiple Key Fields** : Some objects require multiple fields to establish uniqueness. For example:


```text
extend type Author @typePolicy(keyFields: "firstName lastName")
```


This generates cache keys like` Author:Ray+Bradbury` , where multiple key fields are joined with a + separator. The order matters,` firstName` appears before` lastName` in the key.


**Type Policies on Interfaces** : You can also apply type policies to interface types, establishing a default key resolution strategy for all implementing types:


```text
extend interface Node @typePolicy(keyFields: "id")
```


Any type implementing the` Node` interface will inherit this type policy unless it defines its own. However, if a type implements multiple interfaces with conflicting type policies, Apollo iOS will throw a validation error during code generation.


### Programmatic Type Policy Configuration


For advanced scenarios where declarative directives aren’t sufficient, such as when you need to use non-scalar fields or implement complex logic, you can configure type policies programmatically.


Apollo iOS generates a` SchemaConfiguration.swift` file during code generation. This file is never overwritten once created, allowing you to customize it safely. You configure cache keys by implementing the` cacheKeyInfo(for:object:)` method:


```text
public enum SchemaConfiguration: ApolloAPI.SchemaConfiguration {
static func cacheKeyInfo(for type: Object, object: ObjectData) -> CacheKeyInfo? {
// Return a CacheKeyInfo for this object, or nil to fall back to declarative/default behavior
switch type {
case Objects.Book:
// Use ISBN if available, fall back to internal database ID
if let isbn = object["isbn"] as? String, !isbn.isEmpty {
return CacheKeyInfo(id: "isbn:\(isbn)")
} else if let id = object["id"] as? String {
return CacheKeyInfo(id: id)
}
return nil


case Objects.Author:
// Using a custom scalar's internal components
guard let authorUri = object["uri"] as? CustomScalars.URI else {
return nil
}
// Extract the author's unique identifier from the URI path
let pathComponents = authorUri.value.pathComponents
if let authorSlug = pathComponents.last {
return CacheKeyInfo(id: authorSlug)
}
return nil


case Objects.BookEdition:
// Composite key with conditional logic based on edition type
guard let bookId = object["bookId"] as? String else { return nil }


let editionType = object["editionType"] as? String
if editionType == "special" || editionType == "limited" {
// Special editions need unique cache entries per edition
guard let editionNumber = object["editionNumber"] as? Int else { return nil }
return CacheKeyInfo(id: "\(bookId):edition\(editionNumber)")
} else {
// Standard editions share cache entry with the base book
return CacheKeyInfo(id: bookId)
}


default:
return nil
}
}
}
```


The function receives two parameters:


- ` type` : A strongly-typed metadata representation of the object’s GraphQL type
- ` object` : The JSON response data wrapped in an ObjectData struct


**Using Default Cache ID Fields** : If most of your types share a common unique identifier field, you can implement a simple default:


```text
public enum SchemaConfiguration: ApolloAPI.SchemaConfiguration {
return try? CacheKeyInfo(jsonValue: object["id"])
}
}
```


**Working with Abstract Types** : You can configure keys based on interfaces or unions:


```text
public enum SchemaConfiguration: ApolloAPI.SchemaConfiguration {
// All types implementing the Pet interface use their "id" field
if type.implements(Interfaces.Pet) {
return try? CacheKeyInfo(jsonValue: object["id"])
}


// All types in the ClassroomPets union use their "id" field
if Unions.ClassroomPets.possibleTypes.contains(type) {
return try? CacheKeyInfo(jsonValue: object["id"])
}


return nil
}
}
```


**Grouping Cached Objects** : The` CacheKeyInfo` struct includes an optional` uniqueKeyGroup` parameter. **When cache IDs are guaranteed unique across multiple types, you can group them under a common prefix** :


```text
if type.implements(Interfaces.Animal) {
return try? CacheKeyInfo(
jsonValue: object["id"],
uniqueKeyGroup: Interfaces.Animal.name
)
}
```


This generates keys like` Animal:123` instead of` Dog:123` or` Cat:123` , allowing you to search for all cached animals with a shared prefix.


When you provide a programmatic` CacheKeyInfo` , Apollo iOS uses it instead of any` @typePolicy` directive. Returning` nil` falls back to declarative or default behavior.


## Deep Dive: The @fieldPolicy Directive


While type policies handle object storage, field policies optimize object retrieval by constructing cache keys from query arguments.


### How Things Work Without @fieldPolicy


Consider a query that fetches a specific book by ID:


```text
query GetBook($bookId: ID!) {
book(id: $bookId) {
id
title
author
}
}
```


Without a field policy, Apollo iOS must execute this query over the network even if a` Book` with that ID already exists in the cache from a previous query. The client doesn’t understand the semantic meaning of the` bookID` argument, it doesn’t know that this parameter corresponds to the identity of the returned` Book` object. A field policy tells the client “my server implementation guarantees that the` Book` in the response will have an` id` field matching the` bookID` argument value,” enabling cache lookups before making network requests.


### Using the @fieldPolicy Directive


Field policies bridge this gap by instructing the cache how to construct a cache key from field arguments. Add this to your schema extension:


```text
extend type Query @fieldPolicy(forField: "book", keyArgs: "id")
```


The` @fieldPolicy` directive requires two arguments:


- ` forField` : The name of the field to apply the policy to
- ` keyArgs` : A space-delimited string of argument names used to construct the cache key


Now when you query` book(id: "123")` , Apollo iOS constructs the cache key` Book:123` from the argument and checks if that object already exists. If it does, the field returns immediately from the cache without a network request.


**Multiple Key Arguments** :


Some fields require multiple arguments to identify an object:


```text
type Query {
animal(species: String!, habitat: String!): Animal!
}


extend type Query @fieldPolicy(forField: "animal", keyArgs: "species habitat")
```


This creates cache keys like` Animal:Lion+Savanna` . The order of arguments in the directive determines their order in the cache key, so` keyArgs: "habitat species"` would produce` Animal:Savanna+Lion` .


**List Return Types** :


Field policies support fields returning lists:


```text
type Query {
animals(species: [String!], habitat: String!): [Animal!]
}


extend type Query @fieldPolicy(forField: "animals", keyArgs: "species habitat")
```


For inputs` species: \["Lion", "Elephant"\]` and` habitat: "Savanna"` , the cache generates keys` Animal:Lion+Savanna` and` Animal:Elephant+Savanna` , attempting to retrieve each from the cache.


Important constraints for list field policies:


- If a field returns a list, it must have exactly one list input parameter
- If a field has a list input parameter, it must return a list
- Nested lists are not supported
- All cache keys must result in cache hits; partial matches fail the cache read


**Object Arguments with Dot Notation** :


When field arguments are Input Object types, use dot notation to access nested values:


```text
type Query {
animal(input: AnimalInput!): Animal!
}


input AnimalInput {
id: ID!
species: String!
}


extend type Query @fieldPolicy(forField: "animal", keyArgs: "input.species")
```


This traverses the` input` object structure to extract the` species` field, generating keys like` Animal:Lion` .


### Programmatic Field Policy Configuration


For complex scenarios beyond what the declarative directive supports, implement field policies programmatically by conforming your` SchemaConfiguration` to` FieldPolicy.Provider` :


```text
extension SchemaConfiguration: FieldPolicy.Provider {
static func cacheKey(
for field: FieldPolicy.Field,
inputData: FieldPolicy.InputData,
path: ResponsePath
) -> CacheKeyInfo? {
// For a field: hero(heroInput: HeroInput): Hero
guard let heroInput: FieldPolicy.InputData = inputData["heroInput"],
let name = heroInput["name"] as? String,
let factionID = heroInput["factionID"] as? Int else {
return nil
}


return CacheKeyInfo(id: "\(name)+\(factionID)")
}


static func cacheKeyList(
for listField: Field,
inputData: InputData,
path: ResponsePath
) -> [CacheKeyInfo]? {
// For a field: heroes(heroInput: HeroInput): [Hero]
guard let heroInput: FieldPolicy.InputData = inputData["heroInput"],
let names: FieldPolicy.InputListData = heroInput["names"],
let factionID = heroInput["factionID"] as? Int else {
return nil
}


var keys = [CacheKeyInfo]()
for i in 0..<names.count {
if let name = names[i] as? String {
keys.append(CacheKeyInfo(id: "\(name)+\(factionID)"))
}
}
return keys
}
}
```


The protocol requires two methods: one for fields that return a single cache key, and one for fields that return a list of cache keys. Each receives the field metadata, input arguments wrapped in` InputData` , and the response path.


## Bringing It All Together


The real power of Apollo iOS’s cache key resolution emerges when you use` @typePolicy` and` @fieldPolicy` together. Type policies ensure consistent object storage based on identity, while field policies enable intelligent retrieval based on arguments.


**The Complete Flow** :


1. **Writing to Cache** : When a query response arrives, Apollo iOS normalizes the response. For each object, it checks for a programmatic type policy first, then a` @typePolicy` directive, and finally falls back to response-path-based keys. Objects are stored with their computed cache keys.
2. When executing a query with arguments, Apollo iOS checks for programmatic field policies first, then` @fieldPolicy` directives. If found, it constructs cache keys from the arguments and attempts to retrieve the objects. If all required objects exist in the cache, the query resolves immediately without network access.
3. **Fallback Behavior** : At any point, returning` nil` from a programmatic configuration or omitting a directive causes Apollo iOS to fall back to the next resolution strategy in the chain.


**Example Scenario** : Consider this schema extension:


```text
extend type Book @typePolicy(keyFields: "id")
extend type Query @fieldPolicy(forField: "book", keyArgs: "id")
```


When you first execute` query { featuredBooks { id title author } }` , each book is written to the cache with keys like` Book:123` ,` Book:456` , etc., thanks to the type policy.


Later, when you execute` query { book(id: "123") { id title author } }` , the field policy constructs` Book:123` from the argument. The cache finds this key already exists and returns the cached data immediately, no network request required.


This pattern dramatically reduces network traffic, improves application responsiveness, and ensures data consistency across your entire application. By defining clear cache key resolution strategies for your types and fields, you empower Apollo iOS’s normalized cache to work at peak efficiency.


## Summary


The` @typePolicy` and` @fieldPolicy` directives provide a declarative, type-safe approach to cache key resolution in Apollo iOS. Type policies solve the write-side problem by ensuring objects are stored consistently based on their identity. Field policies solve the read-side problem by constructing cache keys from query arguments to retrieve already-cached objects.


For most applications, declarative directives in schema extension files offer the right balance of power and simplicity. When you encounter edge cases, such as needing to use non-scalar fields, implement complex key construction logic, or work with fields that don’t conform to list policy constraints, programmatic configuration provides the flexibility you need.


By mastering both approaches, you can build Apollo iOS applications with highly optimized caching behavior, reducing network overhead and delivering faster, more responsive experiences to your users.


Written by


Zach FettersMoore


[Read more by Zach FettersMoore](https://www.apollographql.com/blog/author/zach)
