---
schema_version: "1.0.0"
document_id: "1d305645df8443124c9f48be8acdeb66d1a276f31297ad1e9761faa49c3c830c"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/using-graphql-with-ruby-on-rails"
published_at: "2021-11-18T11:00:05+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:1c51c4da51012dcdc0d5c8086d1aa85c13cebad9b8a11f58a7e63b612893fff4"
---

# Using GraphQL with Ruby on Rails

**Objective:** Learn how to create a new project with Ruby on Rails and GraphQL while setting it up to use React and Apollo. The focus is on getting the API running and executing a query.


## Introduction


Ruby on Rails is an MVC (model, view, controller) framework that you can use to build web applications using the Ruby programming language. GraphQL is a declarative query language and server-side runtime that makes it easier for frontend developers to perform data fetching. Now, how can you use them together?


In this post, we’re going to recap the basics of Ruby on Rails, GraphQL, and then demonstrate how to build and query a Rails-GraphQL API.


## Basics of Ruby on Rails


Since the release of Ruby on Rails (RoR or simply “Rails”) in 2004, there have been hundreds of thousands of web applications using the framework. You’ve probably been using a handful of applications that are built with Ruby on Rails: GitHub, Shopify, Airbnb, Twitch, Bloomberg, ConvertKit, and Soundcloud to name a few. It is open-source software that prides itself on following “The Rails Doctrine” or “The Rails Way” when programming. The most popular being: optimize for programmer happiness and convention over configuration.


Rails is a framework written in Ruby with the goal to spend more time writing code and less time setting up config files. It unites Ruby with HTML, JavaScript, and CSS to create a web application that operates on a web server. Generally, it is a server-side web application. With its minimal syntax and many gems, it allows you to quickly create and share software projects.


However, Rails is an opinionated framework. It assumes and guides you in the “best way” of doing something. If you follow the “Rails way” you might find your development time productivity-increasing if you can adapt to its ways.


### Ruby on Rails architecture


Rails was built on the basic MVC architecture: modal, view, controller.


- **Model** : ruby classes, business logic, talks with the database and validates the data
- **View** : templates that render data from the models, and handles the presentation to a user
- **Controller** : controls the flow of an application, handles requests, initiates changes in the model (Hint: this is where GraphQL things go!)


## Basics of GraphQL


GraphQL is known to be neither the frontend nor the backend but rather the language spoken between the two to exchange information. It’s designed to make APIs fast, flexible, and developer-friendly. As an alternative to REST, GraphQL allows us to pull data from multiple data sources from only a single API call. Two ways of interaction with a database using GraphQL are with queries and mutations.


1. **Query:** this allows us to get data. It is the “read” in CRUD.
2. **Mutation** : this allows us to change information, including adding, updating, or removing data. It is where “create”, “update”, and “destroy” are in CRUD.


Since GraphQL is language-independent, we can use the Ruby gem “graphql” to create a project. This helps with a specific file structure and certain command-line tools to add GraphQL functionality in our Rails API.


Learn more about GraphQL in[“What is GraphQL? GraphQL introduction.”](https://www.apollographql.com/blog/graphql/basics/what-is-graphql-introduction/)


## Building the API


Here’s how we are going to do it:


1. **Set up a new Ruby on Rails project**
2. **Make some models**
3. **Add some pre-generated data**
4. **Create a GraphQL Rails endpoint**
5. **Write our first query**


### Set up a new rails project


For this project, let’s make a small Taylor Swift API or should we call it “TAY-PI” that has a few of her Red (Taylor’s Version) album songs. We will have an artist with items. We’ll dive more into that relationship on the Rails side in a bit. But first, let’s create the project.


If you don’t already have rails installed on your machine, let’s add that first by running the following command in your console.


```text
gem install rails
```


Next, we want to create a new rails project. Run the following command in your console and feel free to leave out any of the` --skip` flags, but none of them will be used for this project. Keep in mind testing is important! But we will not be going over it in this tutorial.


```text
rails new taypi -d postgresql --skip-action-mailbox --skip-action-text --skip-spring --webpack=react -T
```


Navigate or cd into your new project repo and open the project in a code editor. Next, let’s make some models. Remember, these are Ruby classes that talk with the database and validate the data. We will create models for` Artist` and` Item` . An artist (Taylor Swift) can have many items and represents the user who can manage the items. An item does not have more than one artist (for now) and it will describe an entity (the song).


To generate these two models, run the following 2 lines separately in your console.


```text
rails g model Artist first_name last_name email
```


```text
rails g model Item title description:text image_url artist:references
```


To add the relationship between the artist and the items, navigate to the **app/models/artist.rb** file and add` has_many :items, dependent: :destroy`


```text
class Artist < ApplicationRecord
has_many :items, dependent: :destroy
end
```


We will need some pre-generated data to work with and render to our page. In the **db/seeds/rb** file add the following contents and save the file.


```text
taylor = Artist.create!(
email: "taylor.swift@example.com",
first_name: "Taylor",
last_name: "Swift"
)


Item.create!(
[
{
title: "Red (Taylor's Version)",
description: "Loving him is like driving a new Maserati down a dead-end street...",
artist: taylor,
image_url: "https://static.wikia.nocookie.net/taylor-swift/images/9/93/Red_%28Taylor%27s_Version%29.jpeg/revision/latest/scale-to-width-down/1000?cb=20210618181243"
},
{
title: "All Too Well (Taylor's Version)",
description: "It was rare, I was there, I remember it all too well",
artist: taylor,
},
{
title: "We Are Never Ever Getting Back Together (Taylor's Version)",
description: "You go talk to your friends, talk to my friends, talk to me",
artist: taylor,
},
{
title: "Begin Again (Taylor's Version)",
description: "But on a Wednesday in a café, I watched it begin again",
artist: taylor,
}
]
)
```


To Initialize the database run the following command in your console:


```text
rails db:create db:migrate db:seed
```


**RECAP:** What have we done so far in our Ruby on Rails and GraphQL project? We generated our Rails API. Now let’s add GraphQL and write our first query.


### Adding GraphQL to a Ruby on Rails Project


To create our Rails-GraphQL API, let’s use a ruby gem called` graphql-ruby` . It will add many files to our project. It will add a lot of files that will help run our project. To add the gem, run the following line in your console followed by the generator.


```text
bundle add graphql
```


```text
rails generate graphql:install
```


A Rails generator is used for automating the process of creating files with boilerplate code. It creates and updates files based on templates, etc.


Let’s poke around in the files and see what we got! Check out the schema file, **taypi_schema.rb** . This is where it declares where all the queries should go and set up mutations.


```text
class TaypiSchema < GraphQL::Schema
mutation(Types::MutationType)
query(Types::QueryType)
end
```


To learn more about the similarities and differences between queries and mutations, check out this article:[“GraphQL Mutation vs Query – When to use a GraphQL Mutation”](https://www.apollographql.com/blog/graphql/basics/mutation-vs-query-when-to-use-graphql-mutation/)


Let’s get this app running. Look at the **config/routes.rb** file. The generator is very helpful here! It is mounting` graphiql::Rails::Engine` for us. This allows us to test queries and mutation using the handy web interface, GraphiQL. Think of it as building out documentation and a fun place to test out your queries on the web.


```text
Rails.application.routes.draw do
if Rails.env.development?
mount GraphiQL::Rails::Engine, at: "/graphiql", graphql_path: "/graphql"
end
post "/graphql", to: "graphql#execute"
end
```


Alternatively, you can use the[Apollo Studio Explorer](https://www.apollographql.com/docs/studio/explorer/) . It’s Apollo’s web IDE for creating, running, and managing your GraphQL operations.


### Write & execute a Rails-GraphQL query with GraphiQL


We are going to add more information to our TAY-PI so we can write our first GraphQL query in our Rails project.


We’re going to remove some of the example content and add a field called` :items` in the **query_type.rb** file, so we can get all the items returned. Notice the new` items` method added here. Each field type contains a name (items), a result type/options (` \[Types::ItemType\]` , and` :null` is required and set to` true` or` false` . The description is optional but good to have since it helps with documentation.


```text
module Types
class QueryType < Types::BaseObject
include GraphQL::Types::Relay::HasNodeField
include GraphQL::Types::Relay::HasNodesField


field :items,
[Types::ItemType],
null: false,
description: "Return a list of items"


def items
Item.all
end
end
end
```


We now want to generate the` ItemType` using the GraphQL Ruby gem. In your console, enter the following command.


```text
rails g graphql:object item
```


Now we need to update the **types/item_type.rb** file to include the fields that have a type and nullable option.


```text
module Types
class ItemType < Types::BaseObject
field :id, ID, null: false
field :title, String, null: true
field :description, String, null: true
field :image_url, String, null: true
field :artist_id, Integer, null: false
field :artist, Types::ArtistType, null: false
field :created_at, GraphQL::Types::ISO8601DateTime, null: false
field :updated_at, GraphQL::Types::ISO8601DateTime, null: false
end
end
﻿
```


You might be thinking, how does this all work? It looks for the method with the same name defined in the class time (thanks rails magic!) Now let’s do the same thing, but for the` ArtistType` .


In your console, enter the following command:


```text
rails g graphql:object artist
```


In the **artist_type.rb** file add the` full_name` method and the` full_name` field.


```text
module Types
class ArtistType < Types::BaseObject
field :id, ID, null: false
field :first_name, String, null: true
field :last_name, String, null: true
field :email, String, null: true
field :created_at, GraphQL::Types::ISO8601DateTime, null: false
field :updated_at, GraphQL::Types::ISO8601DateTime, null: false


def full_name
[object.first_name, object.last_name].compact.join("")
end
end
end
```


We now have enough code to start your rails server. Run rails s in your console and open up GraphiQL:[http://localhost:3000/graphiql](http://localhost:3000/graphiql) in your web browser. In GraphiQL run the following query. We can type in a query to run with the data we added to our db/seeds file and get a response back.


```text
{
items {
id
title
description
artist {
firstName
lastName
email
createdAt
}
}
}
```


GraphiQL query and data response


Wait, how is Rails doing all this? Let’s look at the logs in your rails server console:


The GraphQL gem created the` GraphqlController` for us. It is where requests are sent to. Within this file, you can see that the execute method/action does a lot of work for us.


```text
def execute
variables = prepare_variables(params[:variables])
query = params[:query]
operation_name = params[:operationName]
context = {
}
result = TaypiSchema.execute(query, variables: variables, context: context, operation_name: operation_name)
render json: result
rescue StandardError => e
raise e unless Rails.env.development?
handle_error_in_development(e)
end
```


We did a GraphQL query with Ruby on Rails! We are now fetching artists/users along with items.


## Conclusion


We did it! We created a new Ruby on Rails project using the graphql-ruby gem. We then configured our first GraphQL query that fetches data for our Taylor Swift API aka TAY-PI. Our TAY-PI can be expanded and is flexible enough to add more data when needed. Next, we can explore mutations with a Ruby on Rails project to make it more advanced and to take it a step further. To start your frontend work with React and Apollo check out this article,[Get Started with Apollo Client](https://www.apollographql.com/docs/react/get-started/) .


Since GraphQL is non-language dependent, it allows different types of projects to have the ability to use it, and it’s especially fun with Ruby on Rails!


### Resources


[TAYPI GitHub Repository](https://github.com/erinfox/tay-pi)


[https://evilmartians.com/chronicles/graphql-on-rails-1-from-zero-to-the-first-query](https://evilmartians.com/chronicles/graphql-on-rails-1-from-zero-to-the-first-query)


[https://dev.to/isalevine/ruby-on-rails-graphql-api-tutorial-from-rails-new-to-first-query-76h](https://dev.to/isalevine/ruby-on-rails-graphql-api-tutorial-from-rails-new-to-first-query-76h)


Written by


Erin Fox


[Read more by Erin Fox](https://www.apollographql.com/blog/author/erinfox)
