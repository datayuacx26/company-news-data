---
schema_version: "1.0.0"
document_id: "40915493e17b5ae46284463e79287feda443e9c42ff2a252a944e99fe688ce2f"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/designing-your-first-graphql-schema"
published_at: "2021-12-08T12:37:53+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:7c76e884a1c164a699ca1e0d674d884980e7390cb525fa4cc830e8f78cb15894"
---

# Designing Your First GraphQL Schema

Building schemas are a key part of building a graph. Schemas are one of the major benefits of using GraphQL. The schema defines how clients can retrieve and data from your GraphQL API. With schemas, you can easily shape and evolve your data to fit your specific business, product, or project needs. If you’re new to the GraphQL world, you might be wondering how you can build flexible and evolvable schemas.


In this article, we’ll go over some principles to keep in mind when designing your own GraphQL schema. Before getting started, here are a few things you should know:


- You should know what GraphQL is. If you’re unclear on this check out this[What Is GraphQL blog post](https://www.apollographql.com/blog/graphql/basics/what-is-graphql-introduction/) .
- You know what a schema is and you’re familiar with Schema Definition Language (SDL). If you need to brush up on this, check out our[documentation on schema basics](https://www.apollographql.com/docs/apollo-server/schema/schema/) .


Once you’re more familiar with these topics, you’ll be fully prepared to dive into schema design and what you need to know to get started.


## Know domain and users well


When thinking about the design of your schema, it’s important to know your domain well. You should be familiar with your application’s UI. You should also be familiar with the behavior of your users or intended audience.


For example, if you’re building an API for an education platform, you will want to know what the needs and behaviors are for the typical user or student. What information will they want to have access to? What information will they want to change? What data is absolutely required for both the users and you and your team (if you have one)? How will this data be displayed on your UI?


You should be asking yourself these questions to make sure you know your business domain well. Knowing this information will allow you to fully take advantage of the flexibility of GraphQL and in turn design an efficient and useful schema. When your domain is also your field of expertise, you can fully customize your schema to fit your needs well. If you don’t feel like you’ve reached the “field of expertise” level yet, start by asking yourself the questions mentioned above.


## Map out a graph for your application


First off, what is a graph? One of the special things about GraphQL is that it changes how we view our data. Instead of viewing things in table form, which can be 2-dimensional, we can think of our data as a collection of objects and relationships between those objects.


We can take this a step further and call those objects **nodes** and the relationships that connect them **edges.** When you illustrate this structure either by hand or using some sort of illustration software, you’re left with a **graph.** How would this look with a real application?


Take a look at the UI for the education platform we mentioned earlier. This is the home page which will display courses along with the title, author name, and thumbnail for each course.


The UI mockup for our education platform example


We can make a direct connection between the information displayed for each course and the data we’ll need from our API.


The data we’ll need to display for each course on our education platform home page


This can be translated into a graph that would look like this:


The graph representation of the data we need and the relationships between the data


With this graph in place, you can visualize the data and the relationships they share. Then this can be reflected in your schema.


```text
type Query {
"Get tracks array for homepage grid"
tracksForHome: [Track!]!
}
"A track is a group of Modules that teaches about a specific topic"
type Track {
id: ID!
"The track's title"
title: String!
"The track's main author"
author: Author!
"The track's main illustration to display in track card or track page detail"
thumbnail: String
"The track's approximate length to complete, in minutes"
length: Int
"The number of modules this track contains"
modulesCount: Int
}
"Author of a complete Track"
type Author {
id: ID!
"Author's first and last name"
name: String!
"Author's profile picture url"
photo: String
}


```


**💡 If you would like a more in-depth explanation of how this schema was built, check out lessons 2-4 in the[Odyssey Lift Off I course](https://odyssey.apollographql.com/lift-off-part1)**


Mapping out your application’s graph is great because it helps you make the connection between your application’s UI and how users will interact with your data. These two things are very important when you’re designing your own schema because these things should be reflected in how your data is structured.


The key takeaway here is that your schema should mirror your graph. So mapping out your graph before you build your schema is a great habit to form. Try doing this when you’re building your first few schemas. It can even be helpful to draw out graphs for existing applications that you use every day. This will help you start “thinking in graph” aka thinking about data as a graph.


## Design for specific use cases


Your schema will be accessible to front-end developers who are working with different UIs: mobile, desktop, smart TVs, and sometimes even smart watches! For each of these front-end clients, data might be displayed differently. This might cause you to try to be generic and create one-size-fits-all types and fields. But is this the best way to do things?


In software development, we typically want our code to be as concise as possible and everyone knows about the DRY rule (Don’t Repeat Yourself). But when designing your schema, you should be trying to accommodate client use cases, no matter how specific they are. Does this mean you should get rid of fields and types that are generic? No! You can be both generic AND specific.


What specific behaviors and use cases should you be accounting for? Well, the answer to this question comes with knowing your domain and users well. Keep in mind, too, that the specific use cases for your clients will change over time as your application grows. You might start out with a web app but eventually grow to include a mobile client. When your application evolves like this, so will your schema. So it’s good to adopt this use case and behavior-centric mindset now.


## Be specific with naming


Naming things is hard but it’s a very important part of software development and that includes schema design. We sometimes default to using the easiest names to think of when creating types for our schema. But that can lead to confusion and breaking changes later on.


Let’s think about the education platform example mentioned earlier. Say we want to include reviews for courses and lessons. We might be inclined to create a type called` Review` . But we could be more specific. Using` CourseReview` and` LessonReview` as types instead of` Review` makes our code more clear and understandable. Going back to the previous point mentioned, it also allows us to build for specific use cases and user behaviors.


## Where to go from here


Now that you know these guiding principles for schema design, you can dive right into building your own schemas! If you want more information on building evolvable schemas that are built to be future-resistant, check out this article[GraphQL Schema Design: Building Evolvable Schemas](https://www.apollographql.com/blog/backend/schema-design/graphql-building-evolvable-schemas/) .


I also highly recommend you check out our[GraphQL course, Odyssey](https://odyssey.apollographql.com/) . The course covers building schemas and APIs in a beginner-friendly way and is a great way to take your GraphQL knowledge to the next level.


Written by


Ceora Ford


[Read more by Ceora Ford](https://www.apollographql.com/blog/author/ceora)
