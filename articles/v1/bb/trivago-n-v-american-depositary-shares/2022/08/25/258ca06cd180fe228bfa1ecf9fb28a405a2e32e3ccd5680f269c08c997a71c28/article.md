---
schema_version: "1.0.0"
document_id: "258ca06cd180fe228bfa1ecf9fb28a405a2e32e3ccd5680f269c08c997a71c28"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2022-08-01-three-learnings-switching-to-typescript/"
published_at: "2022-08-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:b8c25d5af5511a9c52750d802d93c5d2791dde750c53acd0a38a1a9b7c6a7303"
---

# 3 Things We Learned When Switching to TypeScript

With the[rewrite of our core product web application](https://tech.trivago.com/post/2022-05-16-warp-a-web-application-rewrite-project/) , we moved from a PHP/JavaScript tech stack to a Next.js stack. One of the most significant changes for developers was the switch to TypeScript, which most of us had not had a lot of experience with, previously.


*This is the second of a series of posts about our “big rewrite” project. The[first post](https://tech.trivago.com/post/2022-05-16-warp-a-web-application-rewrite-project/) deals mainly with organisational challenges.*


Before we move to the lessons learned, here are some key facts about the migration project:


- Project: Migration of trivago.com (and more than 50 other trivago domains)
- Developers writing TypeScript: ~30
- Final` .ts` code: 200,000 lines of code in 2,600 files
- Final` .tsx` code: 115,000 lines of code in 1,500 files


From these numbers, I think it becomes clear that this is at least a mid-to-large scale project.


Let’s dive in.


# 1. Love at second sight


Even though we approached TypeScript with an open mind, and educated ourselves about it, several engineers did not like it at first. Coming from a rather functional style of writing JavaScript code, with lots of` map` ,` reduce` , and[currying](https://en.wikipedia.org/wiki/Currying) , we found that TypeScript put a lot of stones in our way, and soon got frustrated with it.


In our existing code base, we would routinely define functions such as this, where arguments were passed from one function to another regardless of their shape and type:


```text
export   const   mapLoggings   =
(  logFragments  )   =>
(  ...  args  )   =>
logFragments.  map  ((  fragment  )   =>   applyOrReturn  (fragment,   ...  args)).  join  (  ':'  );
```


With TypeScript, writing code like this is a lot harder. Either you really get your types right, and probably use a lot of[generics](https://www.typescriptlang.org/docs/handbook/2/generics.html) (type parameters) along the way - or you change your coding style to something slightly less functional.


However, once this adaption process is finished, we found that TypeScript gives you a lot of confidence in your code. When writing new code, this is already a plus. However, it is invaluable when you change existing code, especially if you have not written it yourself. Even if you don’t understand every little detail about the code, as long as you get the types right, chances are that your code is working correctly.


By now, it feels strange for many of us to work in a non-TS code base - like, how do you even reason about the code without knowing what’s in this variable, or if this parameter can ever be` null` ?


With all this praise, let us also clearly state: TypeScript does not and cannot provide the same level of type safety as, say, Java or C#. The type inference is weaker, there are fewer guarantees, and the underlying language is still JavaScript, which is known (some say: notorious) for its lenient handling of types. Nevertheless, TypeScript provides a great deal of safety compared to pure JavaScript, and frees up a lot of mental capacity to think about code structure and architecture rather than whether something can be` undefined` or not.


# 2. Rely on your (GraphQL) schema types


The previous version of our code base was already using GraphQL heavily. Now, with TypeScript being our language of choice, we were able to leverage our schema and auto-generate type definitions from it. With a simple` apollo client:codegen` command, we contact the GraphQL server and update the client-side types:


```text
apollo client:codegen packages/graphql-types/generated \
--no-addTypename \
--target=typescript \
--outputFlat \
--globalTypesFile=./packages/graphql-types/globalTypes.ts
```


Note that we write this auto-generated code to its own package` graphql-types` , to have it isolated from hand-written code and keep things conflict-free.


When you look at these auto-generated files, you quickly notice that the interface names in there are not pretty. An example:


```text
GetSearchSuggestions_getSearchSuggestions_searchSuggestions_AccommodationDetails_destinationHierarchy
```


Therefore, it is highly recommended to export them under different names, like this:


```text
export   type   {
GetSearchSuggestions   as   SearchSuggestionQuery,
GetSearchSuggestions_getSearchSuggestions_searchSuggestions_AccommodationDetails_destinationHierarchy   as   SuggestionHierarchy,
GetSearchSuggestions_getSearchSuggestions_alternativeSuggestions   as   AlternativeSearchSuggestion,
}   from   './generated/GetSearchSuggestions'  ;
```


This is a best practice that has worked very well for us. Another best practice is to connect your application type system to your automatically generated schema types.


# 3. Prefer “connected” types whenever possible


Some people love modelling, and TypeScript is a tool they have just been waiting for. So, when we started rewriting out client-side business logic in TypeScript, all sorts of types and interfaces popped up.


This is expected.


What is problematic, though, is when you start to introduce hand-written types of core entities that are also part of your schema. In our case, these core entities are` Accommodation` ,` Price` , etc. Soon, we had one hand-written` Accommodation` type and one auto-generated one.


Not good.


To make matters worse, we were simply type-casting one into the other, like this:


```text
return   accommodations   as   Accommodation  [];   // DON'T DO THIS!
```


Abracadabra, the objects retrieved from GraphQL are suddenly magically treated as our local` Accommodation` type. Moreover, because the` Accommodation` type has fields of other hand-written types, we implicitly type-cast those fields as well:


```text
export   interface   Accommodation   {
// ...
accommodationDetails  :   AccommodationDetails  ;   // hand-written
coordinates  :   Coordinates  ;   // hand-written
closestPoi  :   Nullable  <  ConceptWithDistance  >;   // hand-written
advertiserInfo  :   Nullable  <  AdvertiserInfo  []>;   // hand-written
deals  :   Deals  ;   // hand-written
// ...
}
```


For example, by type-casting an object to` Accommodation` , we also type-cast its` deals` field to our hand-written` Deals` type, even though the` deals` field coming from the server has a slightly different type.


Of course, the hand-written types were *rather similar* to the auto-generated ones. Therefore, this mostly worked. At first. Nevertheless, it’s a recipe for disaster, because you fool yourself and prevent TypeScript from spotting bugs for you.


Common problems that the type cast masked were:


- **Missing fields:** The hand-written type has a field that does not exist in the schema. Developers will trust the field is there, but it cannot be, because the server does not deliver it.
- **Null / non-null:** The hand-written type says a field is always set, while the server might deliver a` null` value.


Both problems can lead to nasty and very unexpected runtime errors. TypeScript says a certain field is there, but in reality it’s` undefined` or` null` . Automated tests are unlikely to catch such errors, because they, too, would have to circumvent type safety with a similar type cast, which is rather unlikely. If you are lucky, you will catch the bug during manual testing. If not, it will hit you in production.


# Conclusion


Moving to TypeScript from plain JavaScript is an investment. There is additional learning involved, and you will go slower at first. However, being forced to think consciously about which types of data flow through your application pays off in the long run. It sharpens your thinking, and increases your discipline. Moreover, using types really pays off when you have to modify and refactor existing code. If you get the types right, you are usually halfway there.


So, for new projects or for libraries, I would definitely recommend TypeScript. But since you can[migrate gradually](https://www.typescriptlang.org/docs/handbook/migrating-from-javascript.html) , you might want to consider it even for existing projects. The benefits will soon outweigh the initial adaptation problems.
