---
schema_version: "1.0.0"
document_id: "89ad0277a3f2077169aed204b9281d68180f2529c8709805a0de1941c108f806"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/the-story-behind-apollo-kotlin-3-codegen"
published_at: "2022-04-26T09:21:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:6eb01d170e9f4759b5f5c4f3eae5741cee6115a2bf638d981ddadc4343dfefd7"
---

# The story behind Apollo Kotlin 3 codegen

Apollo Kotlin 3 was released in December 2021. This is a significant release for Apollo Kotlin, and it’s fair to say it took significant effort to reach that point 🚀. Now that Apollo Kotlin 3 is in orbit 👨‍🚀, it is a good time to reflect on how we got there and the different tradeoffs involved in generating type safe models from GraphQL operations.


When we started Apollo Kotlin 3, we had two main goals:


1. Make the codebase 100% Kotlin multiplatform ([#2222](https://github.com/apollographql/apollo-kotlin/issues/2222) )
2. Implement fragments as interfaces ([#1854](https://github.com/apollographql/apollo-kotlin/issues/1854) )


Our initial gut feeling was that 1. was a huge task and 2. was not easy but had a somewhat smaller scope.


We were wrong.


While making the codebase Kotlin multiplatform was indeed a huge task, implementing fragments as interfaces proved to be even more challenging. This post is the story of how we realized that and an explanation of the different codegen options available in Apollo Kotlin 3. We hope it can be useful to get a better understanding of how the codegen works and what options are best for your project.


## Mapping GraphQL to Kotlin, a first approach


The GraphQL type system is very similar to the Kotlin one. If you’re not familiar with GraphQL, you can learn about it at[graphql.org](https://graphql.org/learn/schema/#type-system) or in[this video on the Apollo Youtube Channel.](https://www.youtube.com/watch?v=dwLI70eP3mw) In a nutshell, the GraphQL type system supports:


- Objects
- Interfaces
- Unions
- Lists
- Nullability
- Int, Double, String, Boolean


As you can see, it’s very similar to Kotlin. So it’s tempting to do a 1:1 mapping. We could generate Kotlin classes for GraphQL objects, Kotlin interfaces for GraphQL interfaces, Kotlin sealed classes for GraphQL unions, etc… This is simple…


This is also not very type safe because in such a model every field would need to be nullable.


For an example, with a schema with non-nullable fields like this:


```text
type Cat {
id: ID!
species: String!
name: String!
}
```


GraphQL makes it easy to only query the fields you are interested in. If you only want the` name` of your cat, there’s no need to query` id` and/or` species` :


```text
query GetCat {
cat {
# no need to query id/species
name
}
}
```


If we were to generate a single Kotlin class for the` Cat` type, all its fields would have to become nullable


```text
// If we were to generate Kotlin classes from schema types
// Everything would have to be nullable
class Cat(
val id: String?,
val species: String?,
val name: String?,
)
```


Writing your UI code becomes error-prone. If you have a` species` field available in your model, can you rely on it? Was it requested from your backend? Or is it absent because business logic allows it to be null in which case you’ll have to handle it? It’s hard to answer these questions without going back to the query. Also, no compile time verification is made. If you remove a field from one of the queries, the compiler cannot detect it because everything is nullable.


That’s why the decision was taken very early in Apollo Android to **generate models based on your operations, not based on the schema types** . The above` GetCat` query in Apollo Android 2 generates the following Kotlin class (details omitted for brevity):


```text
// A namespacing wrapper class for the 'GetCat' operation
class GetCatQuery {
// The operation data
// name is not nullable any more
// Also, id and species are not reachable because they were not queried
class Data(
val name: String,
)
}
```


By generating classes based on operations, you get most of the type system. If a field is accessible in your Kotlin models you can make sure **at compile time** that it will be present in any successful response.


## Fragments as interfaces: problem statement


Let’s enter fragments. For easier reuse, GraphQL defines[fragments](https://graphql.org/learn/queries/#fragments) . If you have multiple queries querying a` Cat` and all these queries require the` id` and the` name` of the` Cat` you can define a fragment like this:


```text
# Define a fragment
fragment catDetails on Cat {
id
name
}
```


And use it every time you query a` Cat` :


```text
query GetCat {
cat {
# use the fragment
...catDetails
}
}
```


Apollo Android 2.x generates a specific separate class for the` CatDetails` fragment:


```text
// A class for the fragment
class CatDetails(
val id: String,
val name: String,
)


class GetCatQuery {
class Data(
val fragments: Fragments,
)


// A synthetic wrapper class for all fragments
class Fragments(
val catDetails: CatDetails
)
}
```


You can access the data in a type safe way using the` fragments` synthetic field:


```text
// Using fragments in Apollo Android 2
val name = data.fragments.catDetails.name
```


In this type of codegen, every GraphQL fragment will get its own Kotlin model. Because of this mapping from the GraphQL operation to the Kotlin model, this type of codegen is named **operationBased** .


This works but it’s also verbose. Especially considering that the returned JSON response doesn’t know about fragments:


```text
{
"data": {
"cat": {
// No trace of fragment here
"id": "1002",
"name": "Félicette",
}
}
}
```


This verbosity generated a bunch of issues (like[#1854](https://github.com/apollographql/apollo-kotlin/issues/1854) or[#2993](https://github.com/apollographql/apollo-kotlin/issues/2993) ). It felt like we could do something better…


For the simple example above, it felt like we could do something like this:


```text
// Model the fragment as an interface
interface CatDetails(
val id: String,
val name: String,
)


class GetCatQuery {
// Make Data implement the CatDetails interface
class Data(
override val id: String,
override val name: String,
): CatDetails
}
```


This looks a lot nicer and usage is very flexible:


```text
// read it like you would read the Json response
println(data.cat.name)
// or use it like a fragment by casting it
doSomethingWithCatDetails(data.cat as CatDetails)
```


In addition to that, the fact that the Kotlin models now match 1:1 with the Json response means that it is possible to[stream the json](https://github.com/apollographql/apollo-kotlin/issues/2523) . Each JSON value is read-only once by the parser and stored only once in the models, allowing to amortize parsing during network I/O as well as storing less data in memory.


**Because this new codegen, using fragments as interfaces, matches 1:1 with the JSON response, we named it responseBased codegen.**


This sounds cool, right? Well… It is cool. But to get responseBased codegen working, we had to remove a lot of roadblocks.


## Roadblocks to responseBased codegen


### 1. Determining all possible response shapes


For simple use cases as above, determining all possible response shapes is easy, but it can get more complicated as interfaces are involved. For an example:


```text
interface Animal
interface WarmBlooded
interface Pet


query GetAnimal($id: ID!) {
animal(id: $id) {
species
... on WarmBlooded {
bloodTemperature
}
... on Pet {
name
}
}
}
```


We obviously do not want to generate a Kotlin class for each and every concrete animal as that could potentially be a lot of classes. Instead, we’re trying to find all possible shapes the Json can take. The number of possible response shapes depends on the concrete types that are available in the schema. With these types:


```text
type Cat implements Animal & WarmBlooded & Pet
type Alligator implements Animal
type Crocodile implements Animal
```


We have only 2 possible shapes:


```text
class WarmBloodedPetAnimal(species, bloodTemperature, name): Animal, WarmBlooded, Pet
class OtherAnimal(species): Animal
```


Now, if we add some more concrete types:


```text
type Cat implements Animal & WarmBlooded & Pet
type Dog implements Animal & WarmBlooded & Pet
type Lion implements Animal & WarmBlooded
type Turtle implements Animal & Pet
type Alligator implements Animal
type Crocodile implements Animal
type Caiman implements Animal
```


We now have 4 possible shapes:


```text
class PetWarmBloodedAnimal(species, bloodTemperature, name): Animal, WarmBlooded, Pet
class WarmBloodedAnimal(species, bloodTemperature): Animal, WarmBlooded
class PetAnimal(species, name): Animal, Pet
class OtherAnimal(species): Animal
```


Note how the name of the models now does not come from the name of the concrete types themselves but from the type conditions that are satisfied by each shape. Also note that fragments type conditions are not always sub-types of the parent type. It can be anything as long as the type condition possible types and the parent type possible types overlap.


This is all doable but computing all these different shapes in the general case is not as intuitive as it might appear in the simple cases.


### 2. Nested fields


Nested fields sometimes require their models to implement interfaces that are defined in a distant fragment. For an example:


```text
fragment habitatFragment on Habitat {
climate {
# this field here
name
}
}


fragment catFragment on Cat {
habitat {
...habitatFragment
}
}


fragment animalFragment on Animal {
species
...catFragment
}


query GetAnimal {
animal {
...animalFragment
... on Cat {
habitat {
# must be overriden by this model here
climate {
averagePrecipitations
}
}
}
}
}
```


This might seem far-fetched but GraphQL making it so easy to use fragments also make it very possible to end up in situations like these. Any fragment with nested fields will generate deep interfaces that will need to be implemented in the models.


### 3. Exponentially growing codegen


Because fragments are easy to use, it’s easy to make fragments that use other fragments. When doing that, the response size can grow exponentially. This is a nice feature of GraphQL. It makes it very concise to write potentially expensive queries. GraphQL is a very expressive language. When generating code, this can be an issue.


Let’s take a simple example inspired by[this issue](https://github.com/apollographql/apollo-kotlin/issues/3144) :


Schema:


```text
type Query {
cat: Cat
}


type Cat {
name: String
field1: Cat
field2: Cat
}
```


Query:


```text
query Cat {
cat {
...CatFragment1
}
}


fragment CatFragment1 on Cat {
field1 {
name
}
field2 {
name
}
}
```


The query will return a response like this:


```text
{
"data": {
"cat": {
"field1": {
"name": "Félicette1"
},
"field2": {
"name": "Félicette2"
}
}
}
}
```


We have` "name"` listed twice. If we spice things up and add one new level of fragments to the query:


```text
query Cat {
cat {
...CatFragment1
}
}


fragment CatFragment1 on Cat {
field1 {
...CatFragment2
}
field2 {
...CatFragment2
}
}
fragment CatFragment2 on Cat {
field1 {
name
}
field2 {
name
}
}
```


The query will return a response like this:


```text
{
"data": {
"cat": {
"field1": {
"field1": {
"name": "Félicette11"
},
"field2": {
"name": "Félicette12"
}
},
"field2": {
"field1": {
"name": "Félicette21"
},
"field2": {
"name": "Félicette22"
}
}
}
}
}
```


` "name"` is now listed four times. See where I’m going with this? Every time you add one level of nesting in the fragments, you multiply by 2 the response size. And remember, to generate fragments as interfaces, we need to generate classes matching the response. This is with fragments that don’t contain any interface so each fragment has only one possible shape. But when you combine that with interfaces, the generated code size grows big really fast. If you’re not nesting too many fragments, this is maybe ok but if you are, this is something to be aware of.


### 4.` @skip` and` @include` directives


We’ve seen above that nesting fragments could lead to big responses. We have also seen that using interfaces can lead to multiple response shapes, requiring multiple generated models. The same is true for` @skip` and` @include` directives on fragments:


```text
query GetAnimal($condition: Boolean!) {
cat {
...catDetails
...catHabitat @include(if: $condition)
}
}
```


There is no interface involved here, but we still need two different shapes:


```text
class Cat(...): CatDetails
class CatIfCondition(...): CatDetails, CatHabitat
```


### 5. Name clashes


Apollo Android 2.x puts instead all the models at the root level:


```text
// Fragment file
class AnimalDetails
class AnimalDetails.Habitat
class AnimalDetails.Climate


// Operation file
class WarmBloodedAnimal
class Habitat
class Climate
```


This is working well in Apollo Android 2.x because all fragments classes are reused, but it’s not working that well with fragments where every interface needs to be implemented by multiple operation models. In order to avoid name clashes, models are nested:


```text
// Fragment file
interface AnimalDetails
interface AnimalDetails.Habitat
interface AnimalDetails.Habitat.Climate


// Operation file
class WarmBloodedAnimal
class WarmBloodedAnimal.Habitat: AnimalDetails.Habitat
class WarmBloodedAnimal.Habitat.Climate: AnimalDetails.Habitat.Climate


class PetAnimal
class PetAnimal.Habitat: AnimalDetails.Habitat
class PetAnimal.Habitat.Climate: AnimalDetails.Habitat.Climate


class OtherAnimal
class OtherAnimal.Habitat: AnimalDetails.Habitat
class OtherAnimal.Habitat.Climate: AnimalDetails.Habitat.Climate
```


Or can be flattened at the price of adding suffixes:


```text
// Operation file
class WarmBloodedAnimal
class Habitat: AnimalDetails.Habitat
class Climate: AnimalDetails.Climate


class PetAnimal
class Habitat1: AnimalDetails.Habitat
class Climate1: AnimalDetails.Climate


class OtherAnimal
class Habitat2: AnimalDetails.Habitat
class Climate2: AnimalDetails.Climate
```


## Tradeoffs


That’s a lot of roadblocks and as often, tradeoffs are required.


**Solved ✅**


- 1. Determining all possible response shapes – by[iterating over all the possible types](https://github.com/apollographql/apollo-kotlin/blob/0254755bb2b0a516ca3b951d7e6a067d206f9296/apollo-compiler/src/main/kotlin/com/apollographql/apollo3/compiler/ir/shapes.kt#L14) , we can determine all the response shapes and name them according to their type conditions.
- 2. Nested fields – the codegen[assigns a unique id based on its path to each model](https://github.com/apollographql/apollo-kotlin/blob/98c228b13cc483e1dc9fb7d3db49ddf2d9c1125c/apollo-compiler/src/main/kotlin/com/apollographql/apollo3/compiler/ir/ResponseBasedModelGroupBuilder.kt#L384) so that looking up the super types is as easy as looking up in a HashMap


**Acceptable workarounds 🔧**


- ` 4.@skip` and` @include` directives on fragments are not that used and most of the time it is possible to rewrite the operation without them so we decided not to support them for now.
- 5. Name clashes are annoying, mainly due to the limitations of the MacOS filesystem. File names are limited to 256 characters meaning it’s not possible to nest classes too much. Also kapt has issues with nested interfaces. But it’s always possible to flatten the models at the price of some suffixes.


**Challenges 🟡**


- 3. Exponentially growing codegen – If your codebase is using a lot of fragments the response-based codegen will produce a lot of generated code and make your codebase very slow to compile and your classes slow to load.


For this last point, we kept a codegen inspired by 2.x that is able to reuse fragment classes. This codegen is named **operationBased** because it maps 1:1 with your GraphQL operations.


## Conclusion


While the GraphQL and Kotlin type system look very similar on the surface, generating type safe models for complex queries comes with a lot of challenges. Depending on your queries and your codebase, there is no single solution that fits all the needs.


This is why Apollo Kotlin 3 comes with configuration options so that you can choose the one that works best.


operationBased responseBased


maps to… GraphQL operation JSON response


can stream JSON 🚫 ✅


stores merged fields only once 🚫 ✅


allows polymorphism 🚫 ✅


always keeps generated code simple ✅ 🚫


By default, Apollo Kotlin 3 uses` operationBased` models because it is more compatible with 2.x and also easier to grasp. If your codebase doesn’t use too many nested fragments,` responseBased` models will offer more type information and performance.


We hope this post gave you an overview of what the two different options are and the tradeoffs involved in each one. You can read more in the[Codegen.md design document](https://github.com/apollographql/apollo-kotlin/blob/main/design-docs/Codegen.md) or[try it out directly](https://github.com/apollographql/apollo-kotlin/releases) !


What codegen are you using? Anything else you’d like to see? Feedback is very welcome, feel free to ask questions by either[opening an issue on our GitHub repo](https://github.com/apollographql/apollo-android/issues) ,[joining the community](http://community.apollographql.com/new-topic?category=Help&tags=mobile,client) or[stopping by our channel in the KotlinLang Slack](https://app.slack.com/client/T09229ZC6/C01A6KM1SBZ) (get your invite[here](https://slack.kotl.in/) ).


Written by


Martin Bonnin


[Read more by Martin Bonnin](https://www.apollographql.com/blog/author/martin)
