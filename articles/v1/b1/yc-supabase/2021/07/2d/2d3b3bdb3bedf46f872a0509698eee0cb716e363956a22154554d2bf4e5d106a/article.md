---
schema_version: "1.0.0"
document_id: "2d3b3bdb3bedf46f872a0509698eee0cb716e363956a22154554d2bf4e5d106a"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-functions-updates"
published_at: "2021-07-30T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:5a856588f9de3de8c83a6e66c17b6e4ec340063e9f137ef946196b0a7f396dc0"
---

# Updates for Supabase Functions

The question on everyone's mind - are we launching Supabase Functions? Well, it's complicated.


Today we're announcing *part* of Functions - Supabase Hooks - in Alpha, for all **new** projects.


We're also releasing support for Postgres Functions and Triggers in our Dashboard, and some timelines for the rest of Supabase Functions. Let's cover the features we're launching today before the item that everyone is waiting for: Supabase Functions.


## PostgreSQL Functions#


(Not to be confused with Supabase Functions!)


Postgres has built-in support for[SQL functions](https://www.postgresql.org/docs/current/sql-createfunction.html) . Today we're making it even easier for developers to build PostgreSQL Functions by releasing a native Functions editor. Soon we'll release some handy templates!


You can call PostgreSQL Functions with` supabase-js` using your project API \[[Docs](https://supabase.com/docs/reference/javascript/rpc) \]:


hideCopy


`
_10


const { data, error } = await supabase.rpc('best_star_wars_series', {


_10


name: 'The Prequels',


_10


})


`


## PostgreSQL Triggers#


[Triggers](https://www.postgresql.org/docs/current/trigger-definition.html) are another amazing feature of Postgres, which allows you to execute any SQL code after inserting, updating, or deleting data.


While triggers are a staple of Database Administrators, they can be a bit complex and hard to use. We plan to change that with a simple interface for building and managing PostgreSQL triggers.


## Supabase Functions#


They say building a startup is like jumping off a cliff and assembling the plane on the way down. At Supabase it's more like assembling a 747 since, although we're still in Beta, thousands of companies depend on us to power their apps and websites.


For the past few months we've been designing Supabase Functions based on our customer feedback.


### BYO Functions, zero lock-in#


A recurring request from our customers is the ability to trigger their *existing* Functions. This is especially true for our Enterprise customers, but also Jamstack developers who develop API Functions directly within their stack (like Next.js[API routes](https://nextjs.org/docs/api-routes/introduction) , or Redwood[Serverless Functions](https://redwoodjs.com/docs/serverless-functions) ).


### Timeline#


To meet these goals, we're releasing Supabase Functions in stages:


- *Stage 1:* Give developers the ability to trigger external HTTP functions - today, using Database Webhooks.
- *Stage 2:* Give developers the ability to trigger their own Serverless functions on AWS and GCP - Q4 2021.
- *Stage 3:* Release our own Serverless Functions (Supabase Functions) - Q4 for Early Preview customers.


## Database Webhooks (Alpha)#


(Note: Database Webhooks were previously called "Function Hooks")


Today we're releasing Functions Hooks in` ALPHA` . The` ALPHA` tag means that it is NOT stable, but it's available for testing and feedback. The API will change, so do not use it for anything critical. You have been warned.


Hooks? Triggers? Firestore has the concept of[Function Triggers](https://firebase.google.com/docs/functions/firestore-events) , which are very cool. Supabase Hooks are the same concept, just with a different name. Postgres already has the concept of[Triggers](https://www.postgresql.org/docs/current/triggers.html) , and we thought this would be less confusing1 .


### Hook Events#


Database Webhooks allow you to "listen" to any change in your tables to trigger an asynchronous Function. You can hook into a few different events:` INSERT` ,` UPDATE` , and` DELETE` . All events are fired **after** a database row is changed. Keen eyes will be able to spot the similarity to Postgres triggers, and that's because Database Webhooks are just a convenience wrapper around triggers.


### Hook Targets#


Supabase will support several different targets.


- HTTP/Webhooks: Send HTTP requests directly from your Postgres Database.
- AWS Lambda/Google Cloud Run: Provide Supabase with a restricted IAM role to trigger Serverless functions natively.
- Supabase Functions: We'll develop an end-to-end experience.


### Hook Payload#


If the target is a Serverless function or an HTTP` POST` request, the payload is automatically generated from the underlying table data. The format matches Supabase[Realtime](https://supabase.com/docs/reference/javascript/subscribe) , except in this case you don't a client to "listen" to the changes. This provides yet another mechanism for responding to database changes.


hideCopy


`
_21


type InsertPayload = {


_21


type: 'INSERT'


_21


table: string


_21


schema: string


_21


record: TableRecord<T>


_21


old_record: null


_21


}


_21


type UpdatePayload = {


_21


type: 'UPDATE'


_21


table: string


_21


schema: string


_21


record: TableRecord<T>


_21


old_record: TableRecord<T>


_21


}


_21


type DeletePayload = {


_21


type: 'DELETE'


_21


table: string


_21


schema: string


_21


record: null


_21


old_record: TableRecord<T>


_21


}


`


## Hooks technical design:` pg_net v0.1`#


As with most of the Supabase platform, we leverage PostgreSQL's native functionality to implement Database Webhooks (previously called "Function Hooks").


To build hooks, we've released a new PostgreSQL Extension,[pg_net](https://github.com/supabase/pg_net/) , an asynchronous networking extension with an emphasis on scalability/throughput. In its initial (unstable) release we expose:


- asynchronous HTTP` GET` requests.
- asynchronous HTTP` POST` requests with a JSON payload.


The extension is (currently) capable of >300 requests per second and is the networking layer underpinning Database Webhooks. For a complete view of capabilities, check out[the docs](https://supabase.github.io/pg_net/api/) .


### **Usage**#


` pg_net` allows you to make asynchronous HTTP requests directly within your SQL queries.


`
_11


-- Make a request


_11


select


_11


net.http_post(


_11


url:='https://httpbin.org/post',


_11


body:='{"hello": "world"}'::jsonb


_11


);


_11


_11


-- Immediately returns a response ID


_11


http_post


_11


---------


_11


1


`


After making a request, the extension will return an ID. You can use this ID to collect a response.


hideCopy


`
_14


-- Collect the response from a request


_14


select


_14


*


_14


from


_14


net.http_collect_response(1);


_14


_14


-- Results (1 row)


_14


status | message | response


_14


--------+---------+----------


_14


SUCCESS ok (


_14


status_code := 200,


_14


headers := '{"date": ...}',


_14


body := '{"args": ...}'


_14


)::net.http_response_result


`


You can cast the response to JSON within PostgreSQL:


`
_10


-- Collect the response json payload from a request


_10


select


_10


(response).body::json


_10


from


_10


net.http_collect_response(request_id:=1);


`


Result:


noCopy


`
_20


{


_20


"args": {},


_20


"data": "{\\"hello\\": \\"world\\"}",


_20


"files": {},


_20


"form": {},


_20


"headers": {


_20


"Accept": "*/*",


_20


"Content-Length": "18",


_20


"Content-Type": "application/json",


_20


"Host": "httpbin.org",


_20


"User-Agent": "pg_net/0.1",


_20


"X-Amzn-Trace-Id": "Root=1-61031a5c-7e1afeae69bffa8614d8e48e"


_20


},


_20


"json": {


_20


"hello": "world"


_20


},


_20


"origin": "135.63.38.488",


_20


"url": "https://httpbin.org/post"


_20


}


_20


(1 row)


`


### Implementation#


To build asynchronous behavior, we use a PostgreSQL[background worker](https://www.postgresql.org/docs/current/bgworker.html) with a[queue](https://github.com/supabase/pg_net/blob/3d52e7758909bb73bf7fa4586f42cea73ed239b6/sql/pg_net--0.1.sql#L11-L19) . This, coupled with the[libcurl multi interface](https://curl.se/libcurl/c/libcurl-multi.html) , enables us to do multiple simultaneous requests in the same background worker process.


Shout out to[Paul Ramsey](https://github.com/pramsey) , who gave us the implementation idea in[pgsql-http](https://github.com/pramsey/pgsql-http/#to-do) . While we originally hoped to add background workers to his extension, the implementation became too cumbersome and we decided to start with a clean slate. The advantage of being async can be seen by making some requests with both extensions:


hideCopy


`
_21


\\timing on


_21


_21


-- using pgsql-http to fetch from "supabase.io" 10 times


_21


select


_21


*


_21


from


_21


http_get('https://supabase.com')


_21


cross join


_21


generate_series(1, 10) _;


_21


_21


-- Returns in 3.5 seconds


_21


Time: 3501.935 ms


_21


_21


-- using pg_net to fetch from "supabase.io" 10 times


_21


select


_21


net.http_get('https://supabase.com')


_21


from


_21


generate_series (1,10) _;


_21


_21


-- Returns in 1.5 milliseconds


_21


Time: 1.562 ms


`


Of course, the sync version waits until each request is completed to return the result, taking around 3.5 seconds for 10 requests; while the async version returns almost immediately in 1.5 milliseconds. This is really important for Supabase hooks, which run requests for every event fired from a SQL trigger - potentially thousands of requests per second.


### Future/Roadmap#


This is only the beginning! First we'll thoroughly test it and make a stable release, then we expect to add support for


- the remaining HTTP methods (` PUT` /` PATCH` )
- synchronous HTTP
- additional protocols e.g. SMTP, FTP
- more throughput (using epoll)


## Get started today#


Database Webhooks is enabled today on all[new projects](https://supabase.com/dashboard) . Find it under Database > Alpha Preview > Database Webhooks.


## Footnotes#


1.


Postgres also has the concept of[Hooks](https://supabase.com/blog/roles-postgres-hooks) , but they're more of an internal concept.↩
