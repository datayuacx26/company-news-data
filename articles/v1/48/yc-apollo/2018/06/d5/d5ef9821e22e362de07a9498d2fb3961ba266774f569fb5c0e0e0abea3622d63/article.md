---
schema_version: "1.0.0"
document_id: "d5ef9821e22e362de07a9498d2fb3961ba266774f569fb5c0e0e0abea3622d63"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/graphql-building-evolvable-schemas"
published_at: "2018-06-01T18:52:28+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:0c004c3f67e57753bb1e1d5e4f4763fcafbe5dce6ffca51a7f6321f5ca58198e"
---

# GraphQL Schema Design: Building Evolvable Schemas

*This is a guest post by Marc-Andre Giroux, who is currently working on the ecosystem API team at GitHub. He’s been writing and thinking a lot about GraphQL continuous evolution and schema design. His book,[Production Ready GraphQL](https://productionreadygraphql.com/book/) , was released in early March 2020.*


While GraphQL allows us to continuously evolve our schemas, using deprecations, for example, we should not take the decision of deprecating schema members lightly. In the end, a deprecation still requires work from all your integrators in the best case and is breaking change for anyone that has not made the change in the worst case.


While it’s possible to make these changes in an easier way for everyone with better tooling and documentation like what we’ve[built](https://developer.github.com/v4/breaking_changes/) at[GitHub](https://developer.github.com/v4/changelog/) , we must keep in mind that using deprecations should be absolute **last resort** if we want a **stable** API in which our integrators can **trust** .


The good thing is that there are ways to build our GraphQL schema in a way that could avoid serious breaking changes in the future. When building our API, we must keep in mind that things will evolve, and to set ourselves up for success when that happens. In this post, we’ll explore a few things that can help when designing our API for the future.


## 1. Prefer Object Types over simpler structures


Take for example this` CalendarEvent` type. Notice the` timeRange` field, which represents when the event starts, and ends. At a first glance, this looks alright. We’ve got a list type, which probably matches what we have internally, represented in an array with index` 0` being the start, and index` 1` being the end.


Now what if we wanted to add some more data related to the range? For example if that time range is in the past or future? Well in it’s current state, we’d have to add a field to` CalendarEvent` . We’d prefix it by` timeRange` to let our integrators know they’re related. Or worst, we’d end up deprecating` timeRange` and come up with a different design for the` timeRange` field.


This looks slightly off 🤔


The problem here is that we could not add this data in the same` timeRange` field since we’re stuck with a plain` DateTime` array. What if we had designed the` timeRange` field differently to begin with? If we used an object type instead of an array here, we could’ve end up with something like this:


This is much better for a few reasons:


1. We are free to add any additional data we want at no cost to the` TimeRange` type.
2. We have named our fields, instead of using array indexes, which is a lot funner to use for integrators.
3. Related data is found within a single type instead of using a field name prefix to do so.


Try to think if the type you’re using for your field or argument is future proof. When in doubt, use a more complex structure like an object type or an input type.


## 2. When in Doubt, Be Specific With Naming


When we start building a GraphQL Schema, the whole namespace of names is available to us. When building a new type, it’s tempting to use the simplest thing possible to describe the new entity. For example, take a` Comment` type which describes a comment on an post someone made on *SomeSocialMedia™️* .


For a while, this may work perfectly for us. However, we might eventually introduce another kind of` Comment` , and maybe this comment is actually different. This new comment class for example, might be a feedback form comment for our new app. It’s purpose is completely different, it has different fields, but also common fields.


Now we have to name that new comment something like` FeedbackFormComment` . We’re stuck with a generic` Comment` object type representing a post comment. Imagine if we wanted to come up with a` Comment` interface now that we realize we have a few types with the same behavior. Well our post comment has now stolen our generic name for the interface 😾.


Now we need to go through the hard and long problem of deprecating all fields with type` Comment` . Creating an interface with a less ideal name, removing the old fields, and finally renaming the interface to` Comment` . It’s not a fun thing to go through.


Type changes like these take 3 steps to complete:


1. Deprecate existing fields, and create new fields with a new and temporary name.
2. Remove all existing members with the desired name, and deprecate the newly created fields since we will rename them to the desired name.
3. Remove the temporary fields, and add them back with the desired name, which is now free to use.


If we initially named our type` PostComment` , it would have left us some room to clearly define what a` Comment` was.


## 3. Prefer Fields and Types Over Custom Scalars


Some structures are hard to describe using GraphQL’s type system. For example, recursive data structures can be very hard to model with GraphQL and might require us to use a custom scalar to represent correctly.


In most cases though, GraphQL’s type system gives us enough tools to model anything in our schema instead of relying on JSON scalars or any custom scalars.


There’s a few problems with over-using custom scalars:


1. We lose any introspection ability. It’s hard for our clients to know what the shape of data will be returned from a field returning one of these scalars.
2. On the server side, we have now no idea on how this data is used by our integrators. It makes it very hard to make changes to the structure of those custom scalars. We can’t use GraphQL deprecation to do so since we are not using GraphQL fields within that returned payload.


Another similar example is using strings when we could easily represent the field using an` enum` . For example, with a` status` field of type String, our clients need to guess the possible values for status. It makes it hard for us to add or remove values because they can’t possibly be handled by our clients. Using an enum allows clients to know when the field changes, or at least lets us use deprecations when needed.


## Bonus: Forget About Data, Know Your Domain!


This one is a bit less practical but I still believe it’s one of the most important things we need to care about when building any API that will last and will be great to use by our integrators. When designing the shape of your GraphQL schema, try to truly understand what you’re trying to model, and understand your domain the best you can.


With GraphQL having a type system, we see a lot of tools appearing these days that try to generate GraphQL types from databases, ActiveRecord models, or a REST API. While this is tempting to use, and definitely useful at times, by copying our data model or an existing API, we forget to that GraphQL lets us really shape the interface we want to our domain. Try to use that power instead of shaping your API using your data’s shape as inspiration (Avoid[Anemic GraphQL](https://medium.com/@__xuorig__/graphql-mutation-design-anemic-mutations-dd107ba70496) ).


By doing that, implementation details can change but your API should stay stable(r) as long as we modeled our domain correctly!


Thank for reading ❤️ If you’ve enjoyed this post, you would probably like[Production Ready GraphQL](https://book.graphqlschemadesign.com/) , a book that I worked hard on releasing.


Written by


Marc-André Giroux


[Read more by Marc-André Giroux](https://www.apollographql.com/blog/author/xuorig)
