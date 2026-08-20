---
schema_version: "1.0.0"
document_id: "32b2ac7cac63a18811c648aa6dbd3d1fa279c7738fbed37348e8931aea241a09"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/postgrest-aggregate-functions"
published_at: "2024-02-29T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T22:26:14.489943+00:00"
content_hash: "sha256:6b53d1a94d8b1e334a17bec1ba3b7f2ca5ff13457b81e86286ee98b2d4c5f7d8"
---

# PostgREST Aggregate Functions

[PostgREST v12 has been released,](https://github.com/PostgREST/postgrest/releases/tag/v12.0.0) and with it comes the release of a much-requested feature: **aggregate functions** .


As a bit of background, aggregate functions are a database feature that allow you to summarize your data by performing calculations across groups of rows. Previously, you could only use aggregate functions *indirectly* with PostgREST, for instance, by using them in a view, but with the latest release, you can now use aggregate functions on the fly, dynamically slicing-and-dicing data directly through the PostgREST API.


In this post, we’ll go through a few examples of some of the neat things you can do with this new feature. We’ll also discuss the importance of ensuring you have the appropriate safeguards in place to prevent potential performance issues that may arise when using aggregate functions.


For the most complete information, please be sure to refer to[the section on aggregate functions in the documentation](https://postgrest.org/en/v12.0/references/api/aggregate_functions.html) .


## The Basics of Aggregate Functions#


PostgREST supports a handful of the most-common aggregate functions from PostgreSQL:` avg()` ,` count()` ,` max()` ,` min()` , and` sum()` . These functions more or less do what their names suggest, but you can always take a deeper look at the[PostgreSQL documentation](https://www.postgresql.org/docs/current/functions-aggregate.html) to learn more.


Let’s take a look at an example. Imagine we have a table called` movies` that has the following columns:` name` ,` release_year` ,` genre` , and` box_office_earnings` . Let’s say that we want to grab the max and min of the` release_year` column across all of the movies in our dataset. That’s pretty simple to achieve:


cURL


`
_10


$ curl 'http://postgrest/movies?select=release_year.max(),release_year.min()'


`


supabase-js


`
_10


const { data, error } = await supabase


_10


.from('movies')


_10


.select('release_year.max(), release_year.min()')


`


`
_10


\[


_10


{


_10


"max": 2022,


_10


"min": 2018


_10


}


_10


\]


`


As you can see, to use an aggregate function, we just place the function after the column in the` select` parameter. Easy.


Now, what if we want to get a little fancier and get the max and min of the` release_year` for every` genre` in our dataset? If you’re familiar with aggregate functions in SQL, then your mind will probably go right away to` GROUP BY` . In PostgREST, there is no need to *explicitly* specify a` GROUP BY` ; instead, you can just add your grouping columns right to the` select` parameter. Any column without an aggregate function in the` select` list will be used as a grouping column:


cURL


`
_10


$ curl 'http://postgrest/movies?select=genre,release_year.max(),release_year.min()'


`


supabase-js


`
_10


const { data, error } = await supabase


_10


.from('movies')


_10


.select('genre, release_year.max(), release_year.min()')


`


`
_22


\[


_22


{


_22


"genre": "Adventure",


_22


"max": 2021,


_22


"min": 2020


_22


},


_22


{


_22


"genre": "Horror",


_22


"max": 2022,


_22


"min": 2019


_22


},


_22


{


_22


"genre": "Sci-Fi",


_22


"max": 2022,


_22


"min": 2018


_22


},


_22


{


_22


"genre": "Mystery",


_22


"max": 2019,


_22


"min": 2019


_22


}


_22


\]


`


Generally speaking, aggregate functions can be used with other PostgREST features you’re already familiar with. For instance, you can use[vertical filtering](https://postgrest.org/en/stable/references/api/tables_views.html#vertical-filtering) to apply aggregates to a slimmed down version of your dataset, like only movies released after the year 2000, or you can use[column renaming](https://postgrest.org/en/stable/references/api/tables_views.html#renaming-columns) to change the name of the aggregated column in the results, like for example changing the names of the` max` and` min` columns from the previous examples to instead be` max_release_year` and` min_release_year` .


## Aggregate Functions and Embedded Resources#


Aggregate functions also play nicely with[embedded resources](https://postgrest.org/en/stable/references/api/resource_embedding.html) , opening up a world of potential use cases.


Building on the previous examples, let’s say that we have a table called` directors` that has a **one-to-many relationship** with our` movies` table from before. We’ll be using a couple of columns from the` directors` table in this section:` name` and` country` .


Let’s say for every director, we want to get the` release_year` of their oldest and newest movies. We can do that without too much trouble:


cURL


`
_10


$ curl 'http://postgrest/directors?select=name,movies(newest_movie_year:release_year.max(),oldest_movie_year:release_year.min())'


`


supabase-js


`
_10


const { data, error } = await supabase.from('directors').select(\`name,


_10


movies(


_10


newest_movie_year:release_year.max(),


_10


oldest_movie_year:release_year.min()


_10


)\`)


`


`
_29


\[


_29


{


_29


"name": "Maria Gonzalez",


_29


"movies": \[


_29


{


_29


"newest_movie_year": 2022,


_29


"oldest_movie_year": 2018


_29


}


_29


\]


_29


},


_29


{


_29


"name": "John Smith",


_29


"movies": \[


_29


{


_29


"newest_movie_year": 2022,


_29


"oldest_movie_year": 2019


_29


}


_29


\]


_29


},


_29


{


_29


"name": "Amit Patel",


_29


"movies": \[


_29


{


_29


"newest_movie_year": 2021,


_29


"oldest_movie_year": 2019


_29


}


_29


\]


_29


}


_29


\]


`


As shown above, you can use aggregate functions *within* the context of an embedded resource: For each set of movies that belongs to a particular director, we apply the given aggregate functions, in this case applying the` min()` and` max()` functions to the` release_year` .


You can also see that we made use of a column renaming — as was briefly described earlier — to make the results a little easier to understand.


Note that we didn’t use grouping columns here, but we could use them to drill-down even further: For instance, we could grab the earliest and latest values of the` release_year` column for each director *by*` genre` by adding` genre` as a grouping column.


Let’s look at another example, but this time going the opposite way: We’ll use` movies` as our **top-level resource** and embed` directors` through a **one-to-one** relationship.


Now, we want to get the average` box_office_earnings` for our movies, grouped by the` country` of the director. To do that, we can use the following API call:


cURL


`
_10


$ curl 'http://postgrest/movies?select=avg_earnings:box_office_earnings.avg(),...directors(country)'


`


supabase-js


`
_10


const { data, error } = await supabase.from('movies').select(\`


_10


avg_earnings:box_office_earnings.avg(),


_10


...directors(country)


_10


\`)


`


`
_14


\[


_14


{


_14


"avg_earnings": 10200000.38,


_14


"country": "Spain"


_14


},


_14


{


_14


"avg_earnings": 8933333.79,


_14


"country": "India"


_14


},


_14


{


_14


"avg_earnings": 8933333.84,


_14


"country": "United States"


_14


}


_14


\]


`


In this case, we have used[the ability to spread the columns from an embedded resource](https://postgrest.org/en/stable/references/api/resource_embedding.html#spread-embedded-resource) to use the director’s` country` as a grouping column, even though the aggregate function` avg()` is being applied to a column of` movies` , not` directors` .


Because spreading columns brings them to the top-level, they are treated as columns of the top-level for purposes of aggregation and grouping. That means any aggregate functions applied to the columns of a spread resource are applied within the context of the top-level, too.


## Staying Safe with Aggregate Functions#


Now that we’ve gone through a few examples of how to use aggregate functions, it’s important to discuss how to *safely* use aggregate functions in your application. Because of the potential performance risks with aggregate functions, we have[disabled aggregate functions by default](https://postgrest.org/en/stable/references/configuration.html#db-aggregates-enabled) . Only after reviewing the risks and ensuring appropriate safeguards are in place should you enable this feature. On Supabase, you can enable it by modifying the PostgREST connection role and then reloading the server configuration, like so:


`
_10


ALTER ROLE authenticator SET pgrst.db_aggregates_enabled = 'true';


_10


NOTIFY pgrst, 'reload config';


`


Now you may be thinking, “what’s the big deal?” Aggregate functions may not seem any more likely to pose performance problems than other parts of PostgREST, but there is one key difference: Aggregate functions can operate across an effectively limitless number of rows, whereas other parts of PostgREST — thanks to pagination — can be limited to operate only across a certain number of rows.


For example, imagine our` movies` table from before has twenty million rows. If we wanted to get the max of the` release_year` for all movies and there is no index on the` release_year` column, it’s going to take a *looooong* time.


Even worse, imagine that someone with bad intentions wants to do bad things to your innocent server: It could be relatively simple for the attacker to bombard your server with expensive aggregation queries, preventing your server from having the capacity to deal with legitimate traffic, a form of denial-of-service attack.


One strategy for preventing potential performance issues is using the[pg_plan_filter_module](https://github.com/pgexperts/pg_plan_filter) . Using this extension, you can set an upper limit on the *cost* of queries that PostgREST will run.


`
_10


ALTER USER authenticator SET plan_filter.statement_cost_limit = 1e7;


`


Before PostgreSQL executes a query, it first comes up with a plan on how it will execute it, and as part of that plan, it comes up with a cost. As you might imagine, higher costs are associated with slower queries, and therefore setting an upper bound can limit your exposure to performance problems or even denial-of-service attacks.` pg_plan_filter_module` enables you to easily set this upper bound using PostgreSQL configuration.


You can even change this limit on a per-role basis, allowing more privileged roles free rein in the queries they run, while less-privileged roles — perhaps external users of your public API, for instance — could have tighter limits.


`
_11


-- anonymous users can only run cheap queries


_11


ALTER


_11


USER anon


_11


SET


_11


plan_filter.statement_cost_limit = 10000;


_11


_11


-- authenticated users can run more expensive queries


_11


ALTER


_11


USER authenticated


_11


SET


_11


plan_filter.statement_cost_limit = 1e6;


`


You can take a look at an example of using per-role configuration with PostgREST[in the documentation](https://postgrest.org/en/stable/references/transactions.html#impersonated-role-settings) .


## Summing Up#


PostgREST v12 now has aggregate functions, giving you a lot more flexibility in how you work with your data. Even better, it’s deeply integrated with other PostgREST features you already know, providing you with a powerful new abstraction that fits in frictionlessly with existing features.


While we are excited to bring aggregate functions to PostgREST, it’s important for administrators and users to understand the risks that come with them, hence why this feature comes as opt-in only. Make sure to have a strategy in place — like using` pg_plan_filter_module` — before enabling aggregate functions to ensure maximum protection.
