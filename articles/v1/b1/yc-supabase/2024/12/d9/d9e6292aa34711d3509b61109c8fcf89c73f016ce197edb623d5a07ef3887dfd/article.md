---
schema_version: "1.0.0"
document_id: "d9e6292aa34711d3509b61109c8fcf89c73f016ce197edb623d5a07ef3887dfd"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/calendars-in-postgres-using-foreign-data-wrappers"
published_at: "2024-12-20T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:a79589dad7e02cd3de4d7357f940a078ee8128be9cef03f4f4097be802f99d04"
---

# Calendars in Postgres using Foreign Data Wrappers

Today we're releasing Foreign Data Wrappers for[Cal.com](https://cal.com/) so that you can create event bookings directly from Postgres.


This is especially useful for signup forms where you create an event in your database and schedule an event simultaneously: now you can do all this in a single Postgres transaction.


## What's Cal.com?#


[Cal.com](http://cal.com/) is an open-source scheduling platform that allows individuals and businesses to book and manage appointments. It is designed to work with a variety of use cases, from personal calendars to enterprise-grade scheduling systems. They have a great[developer toolkit](https://cal.com/platform) .


## Creating event bookings with Postgres#


[Cal.com](http://cal.com/) offers various scheduling features. One of the most common scenarios for developers is creating a new event in a calendar (for example, after someone has purchased a flight).


Let's use your Supabase database to create an event in[Cal.com](http://cal.com/) , using Postgres Foreign Data Wrappers.


### Set up a Cal.com account#


- Sign up on[Cal.com](http://cal.com/)
- Visit[Settings -> Developer -> API keys](https://app.cal.com/settings/developer/api-keys) to create an API key


### Set up a Supabase account#


- Sign up on[supabase.com](http://supabase.com/)
- Create a project or open an existing project
- Go to[supabase.com/dashboard/project/_/database/extensions](https://supabase.com/dashboard/project/_/database/extensions) to enable` wrappers` extension


### Create Wasm wrapper and a foreign server#


Visit[Supabase SQL Editor](https://supabase.com/dashboard/project/_/sql/new) , use below SQL to create the Wasm foreign data wrapper:


`
_10


create foreign data wrapper wasm_wrapper


_10


handler wasm_fdw_handler


_10


validator wasm_fdw_validator;


`


And then create a foreign server for[Cal.com](http://cal.com/) connection with your API Key:


`
_10


create server cal_server


_10


foreign data wrapper wasm_wrapper


_10


options (


_10


fdw_package_url 'https://github.com/supabase/wrappers/releases/download/wasm_cal_fdw_v0.1.0/cal_fdw.wasm',


_10


fdw_package_name 'supabase:cal-fdw',


_10


fdw_package_version '0.1.0',


_10


fdw_package_checksum '4afe4fac8c51f2caa1de8483b3817d2cec3a14cd8a65a3942c8b4ff6c430f08a',


_10


api_key '<your Cal.com API key>'


_10


);


`


Find the latest version and checksum in the docs:[fdw.dev/catalog/cal](https://fdw.dev/catalog/cal/)


### Set up foreign tables#


Now let's setup the foreign tables. First of all, create a dedicate schema for the foreign tables:


`
_10


create schema if not exists cal;


`


And then create a foreign table for[Event Types](https://app.cal.com/event-types) :


`
_10


create foreign table cal.event_types (


_10


attrs jsonb


_10


)


_10


server cal_server


_10


options (


_10


object 'event-types'


_10


);


`


And another foreign table for[Bookings](https://app.cal.com/bookings/upcoming) :


`
_10


create foreign table cal.bookings (


_10


attrs jsonb


_10


)


_10


server cal_server


_10


options (


_10


object 'bookings',


_10


rowid_column 'attrs'


_10


);


`


Note the` rowid_column` option which is required to insert data into` cal.bookings` table, we will see it later.


### Query Event Types and Bookings from Cal.com#


Great, now we are all set, it's time to query some juicy data from Cal.com! Let's start query from[Event Types](https://app.cal.com/event-types) first:


`
_10


-- extract event types


_10


select


_10


etg->'profile'->>'name' as profile,


_10


et->>'id' as id,


_10


et->>'title' as title


_10


from cal.event_types t


_10


cross join json_array_elements((attrs->'eventTypeGroups')::json) etg


_10


cross join json_array_elements((etg->'eventTypes')::json) et;


`


Note all the scheduling information returned from[Cal.com](http://cal.com/) API are stored in the JSON column` attrs` , from which we can extract any fields of that object. For example, we can extract` id` ,` title` ,` name` and etc., from[Bookings](https://app.cal.com/bookings/upcoming) object:


`
_10


-- extract bookings


_10


select


_10


bk->>'id' as id,


_10


bk->>'title' as title,


_10


bk->'responses'->>'name' as name,


_10


bk->>'startTime' as start_time


_10


from cal.bookings t


_10


cross join json_array_elements((attrs->'bookings')::json) bk;


`


Oops, it looks like we haven't booked any meetings with anybody yet. Now it's the fun part, let's make a booking on[Cal.com](http://cal.com/) from Supabase!


### Make a bookings on Cal.com from Supabase#


To make a booking directly from Postgres, all we need to do is to insert a record to` cal.bookings` foreign table, with the booking details in JSON format. For example,


`
_13


-- make a 15 minutes meeting with Elon Musk


_13


insert into cal.bookings(attrs)


_13


values (


_13


'{


_13


"start": "2025-01-01T23:30:00.000Z",


_13


"eventTypeId": 1398027,


_13


"attendee": {


_13


"name": "Elon Musk",


_13


"email": "elon.musk@x.com",


_13


"timeZone": "America/New_York"


_13


}


_13


}'::jsonb


_13


);


`


Here you can see we made a 15 minutes meeting booking with Elon, just to give him a happy new year greeting 😄. Note the` eventTypeId` , “1398027”, is our` 15 Min Meeting` event type ID, you can find yours by querying the` cal.event_types` foreign table using above example SQL.


After inserting the booking record, we can verify it appears on our upcoming list in[Cal.com](http://cal.com/) .


When we query` cal.bookings` again using the previous SQL, we can see our new booking record is in the results as well.


That wraps up our tutorial! We've covered how to interact with[Cal.com](http://cal.com/) in Supabase using foreign wrapper and tables. For more information about available objects and fields, refer to the[Cal.com API v2 reference](https://cal.com/docs/api-reference/v2/introduction) and the[Wrappers Cal.com Wasm Wrapper documentation](https://fdw.dev/catalog/cal/) .


## Built with Wrappers#


The[Cal.com](http://cal.com/) FDW is built with[Wrappers](https://fdw.dev/) , a framework for Postgres Foreign Data Wrappers (FDW). Our latest release supports[Wasm (WebAssembly)](https://webassembly.org/) to simplify development for API-based services.


## Explore more#


We've built a variety of wrappers available on[fdw.dev](https://fdw.dev/) , ranging from popular tools like[Stripe](https://stripe.com/) and[Notion](https://www.notion.com/) to databases like[ClickHouse](https://clickhouse.com/) and[BigQuery](https://cloud.google.com/bigquery) . Check out the[full catalog](https://fdw.dev/catalog/) and get started with Supabase today:


[database.new](https://database.new/)
