---
schema_version: "1.0.0"
document_id: "435d58dacfc69e4f1b3a05d1f2be8bb067ff9f8ab2447f529e98e132b87b3457"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/kysely-dialect-planetscale"
published_at: "2023-04-24T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:d255e8538f6c54e8c33effc89ff40b170b2f3debd851d886780f445d96886283"
---

# Kysely dialect for PlanetScale

We've been using[depot/kysely-planetscale](https://github.com/depot/kysely-planetscale) for several months to power Depot, so we thought we'd share why we built it and how you can use it to integrate Kysely and PlanetScale together.


## Our database architecture


Since launching in July, we've gone through several database providers and ORM frameworks. Initially we used hosted Postgres as our database and[Prisma](https://www.prisma.io/) as our ORM.


Today we use[PlanetScale](https://planetscale.com/) as our database, who provide a serverless MySQL database service based on Vitess. Postgres is an amazing database, but PlanetScale's offering was appealing to us as it allows us to focus on building our product instead of managing our database. PlanetScale also offers an incredibly powerful alternative to traditional database migrations, using[branches and safe migrations](https://planetscale.com/docs/concepts/branching) , that allows us to safely deploy database changes without downtime.


We also don't use Prisma anymore, but instead use[Kysely](https://kysely-org.github.io/kysely/) .


## Why Kysely?


We experienced various issues with Prisma over the course of many months, including slow query performance, Prisma's engine binary losing connection to the database, and the Prisma engine mysteriously using 100% CPU and crashing the application.


[Kysely](https://kysely-org.github.io/kysely/) was appealing to us because it's a type-safe SQL query builder. Like Prisma, it integrates nicely with TypeScript, so that it is aware of our database schema and can provide type safety and autocompletion when writing queries. But unlike Prisma, it doesn't abstract away the SQL, but instead offers a nice API to write it. We can use this API to produce queries that are optimized for performance.


And Kysely doesn't require a separate engine binary to communicate with the database. Instead, Kysely uses "dialects" that instruct it how to generate SQL for a specific database and how to communicate with that database. Kysely comes with dialects for common databases like MySQL, Postgres, and SQLite.


## PlanetScale serverless driver


You can connect to a PlanetScale database using the MySQL protocol over a secure connection, so we are able to use Kysely with PlanetScale in environments that support TCP connections, for instance Node.js servers, using Kysely's MySQL dialect.


However edge runtimes like Cloudflare Workers and Netlify Edge Functions don't support direct TCP connections to databases, so PlanetScale provides a[serverless driver for JavaScript](https://planetscale.com/blog/introducing-the-planetscale-serverless-driver-for-javascript) that allows you to query your PlanetScale database over HTTP, from anywhere that supports` fetch` . PlanetScale have made this highly performant using HTTP/2, so that you can achieve near native query performance from edge functions.


There wasn't a Kysely dialect for the PlanetScale serverless driver, so we decided to write one.


## ` kysely-planetscale` dialect


The[depot/kysely-planetscale](https://github.com/depot/kysely-planetscale) dialect instructs Kysely how to generate SQL that's compatible with PlanetScale's MySQL-based database, and how to communicate with it using the[@planetscale/database-js](https://github.com/planetscale/database-js) serverless driver.


This allows us to query our PlanetScale database directly from CloudFlare Workers, with all the type safety and autocompletion that Kysely provides.


### Using the dialect


To use the dialect, you need to install the` kysely-planetscale` package, along with the` kysely` and` @planetscale/database` peer dependencies:


```text
pnpm   add   kysely-planetscale   kysely   @planetscale/database
```


You can then pass a new instance of` PlanetScaleDialect` as the dialect option when creating a new Kysely instance. The dialect accepts the same options as` connect({...})` from[@planetscale/database](https://github.com/planetscale/database-js) .


```text
import   {Kysely}   from   'kysely'
import   {PlanetScaleDialect}   from   'kysely-planetscale'
import   type   {DB}   from   './schema'


const   db   =   new   Kysely  <  DB  >({
dialect:   new   PlanetScaleDialect  ({
host:   '<host>'  ,
username:   '<user>'  ,
password:   '<password>'  ,
}),
})
```


With the dialect set, you can now run queries against your PlanetScale database. Here is an example query we use to power list projects in our UI.


```text
const   db   =   new   Kysely  <  DB  >({
dialect:   new   PlanetScaleDialect  ({
host:   '<host>'  ,
username:   '<user>'  ,
password:   '<password>'  ,
}),
})


const   projects   =   db
.  selectFrom  (  'Project'  )
.  select  ([  'id'  ,   'name'  ])
.  where  (  'orgID'  ,   '='  , orgID)
.  where  (  'deletedAt'  ,   'is'  ,   null  )
.  orderBy  (  'name'  ,   'asc'  )
.  execute  ()
```


See[the README](https://github.com/depot/kysely-planetscale) for more information on how to use the dialect.


## Conclusion


We hope that[depot/kysely-planetscale](https://github.com/depot/kysely-planetscale) is helpful to other folks that are looking to leverage the power of PlanetScale while also getting the type safety and autocompletion that Kysely provides. For more on typesafe database queries from the edge, see[this excellent post by Nexxel](https://www.nexxel.dev/blog/typesafe-database) .


If you have any questions or feedback, feel free to reach out to us on[Twitter](https://twitter.com/depotdev) or[Discord](https://discord.gg/MMPqYSgDCg) .


Jacob Gillespie


CTO & Co-founder of Depot
