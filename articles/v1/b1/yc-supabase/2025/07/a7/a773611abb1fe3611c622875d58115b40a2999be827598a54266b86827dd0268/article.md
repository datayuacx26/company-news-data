---
schema_version: "1.0.0"
document_id: "a773611abb1fe3611c622875d58115b40a2999be827598a54266b86827dd0268"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/algolia-connector-for-supabase"
published_at: "2025-07-17T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:56:43.889340+00:00"
content_hash: "sha256:915e47bdeeb7de1f64bf2e7efe62e36ef8eb341a78908356172355515be6c18f"
---

# Algolia Connector for Supabase

Today, Algolia is launching a new Supabase Connector, making it easier than ever to index your Postgres data and power world-class search experiences without writing a single line of code.


With just a few clicks, you can connect your Supabase database to Algolia, select the tables you want to sync, and configure how often the data updates. Algolia handles the rest. You get a fast, reliable, scalable search index, and your team gets to focus on building.


## Partners Integrating with Supabase#


Supabase is more than a backend. It is a growing ecosystem of tools that work well together so developers can build faster, scale more easily, and stay focused on their product.


Partners like Algolia bring best-in-class functionality (in Algolia’s case, fast and flexible search) directly into the Supabase workflow. For developers, that means fewer workarounds, no glue code, and a smoother path from idea to production.


For partners, integrating with Supabase means more than technical compatibility. It means product visibility to tens of thousands of active projects. Supabase regularly features integrations in our docs, Launch Weeks, blog, and community programs. Developers discover and adopt your product in the context where they’re already building.


Read on to see how the Algolia Connector for Supabase works.


## How to use Algolia Connector for Supabase#


To get started with Algolia’s connector, prepare the data in your Supabase database, create Supabase as a source in Algolia’s dashboard, set up your Algolia index and configure your sync job. Here’s how you can[get started](https://www.algolia.com/doc/guides/sending-and-managing-data/send-and-update-your-data/connectors/supabase?utm_medium=referral&utm_source=supabase&utm_campaign=supabase_blog) in just a few minutes.


### 1. Prepare your data in Supabase#


Before you connect to Algolia, you will want to ensure all the fields you want to make searchable are in one place. If the fields you want to index live in more than one table, you can stitch them together in a[Postgres View](https://supabase.com/docs/guides/graphql/views) , allowing Algolia’s connector to get all the data you want to index.


For example, imagine you’re creating an app that allows you to easily find a movie to watch. You want to search across movie titles, genres, rating and actors. However, movies and actors are in two separate tables. You can create a view (e.g.,` movies_view` ) that combines the columns you need:


`
_12


create view movies_view as


_12


select


_12


m.id as objectID, -- Algolia’s unique key


_12


m.title,


_12


array_agg(distinct c.actor_name) as actor_name,


_12


m.genre,


_12


m.rating,


_12


m.vote_count


_12


from


_12


movies as m


_12


left join movie_cast as c on c.movie_id = m.id


_12


group by m.id, m.title, m.rating, m.vote_count;


`


Later in the Algolia dashboard, you will be able to pick exactly which columns you want to index.


### 2. Go to Algolia dashboard#


1. In Algolia, go to[Data Sources → Connectors](https://dashboard.algolia.com/connectors?utm_medium=referral&utm_source=supabase&utm_campaign=supabase_blog)
2. Find "Supabase" in the list and click[Connect](https://dashboard.algolia.com/connectors/supabase/create?utm_medium=referral&utm_source=supabase&utm_campaign=supabase_blog)


### 3. Configure your data source#


First, you will need to fill in your Supabase connection info. From the Supabase dashboard:


1. Click the[Connect](https://supabase.com/dashboard/project/_?showConnect=true) button found in the top of our header
2. Scroll down to **Connection Info → Transaction Pooler** and copy **host** , **port** , **database name** , and **username**
3. Paste the database credentials into the Algolia setup screen
4. Enter your Supabase database **password**
5. Select your **schema** (usually` public` )
6. Give your source a name like` supabase_movies`
7. Algolia will check the connection and confirm your credentials


### 4. Configure your destination#


Once you create Supabase as a data source, you'll need to tell Algolia where to index your data.


1. Select an existing or create a new Algolia index (e.g.` supabase_movies_index` )
2. Add Index Credentials to this destination by clicking **Create one for me**
3. Click **Create destination**


### 5. Configure your task and run your sync job#


1. Choose how often you want it to sync your data (e.g. every 6 hours)
2. Pick whether to do full syncs or partial updates
3. Select the table or view you want to index. We recommend selecting only one table or view for each index
4. Choose your[objectID](https://www.algolia.com/doc/guides/sending-and-managing-data/prepare-your-data/in-depth/what-is-in-a-record/#unique-record-identifier?utm_medium=referral&utm_source=supabase&utm_campaign=supabase_blog) (usually your primary key)


Once configured, create the task. Algolia will start syncing records from Supabase into your search index (in the YouTube demo above, 8,800+ movie records were synced in under a minute).


You can now instantly search your Supabase data using Algolia's lightning-fast API.


## No more data pipelines. Just fast search.#


With the Algolia + Supabase connector, you don’t need to build or maintain custom data pipelines. With Algolia, you don’t need to worry about scaling your own search infrastructure. With Algolia’s API clients, you just connect and go.


## Getting Started#


1. [Supabase](https://supabase.com/dashboard)
2. [Algolia](https://dashboard.algolia.com/users/sign_up?utm_medium=referral&utm_source=supabase&utm_campaign=supabase_blog)
