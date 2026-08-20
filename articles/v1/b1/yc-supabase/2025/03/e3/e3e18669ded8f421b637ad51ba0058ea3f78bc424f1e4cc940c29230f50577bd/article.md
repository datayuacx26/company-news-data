---
schema_version: "1.0.0"
document_id: "e3e18669ded8f421b637ad51ba0058ea3f78bc424f1e4cc940c29230f50577bd"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/migrating-mongodb-data-api-with-supabase"
published_at: "2025-03-20T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:58:14.920102+00:00"
content_hash: "sha256:11c0a5ced620a3f7bdeac3e952ea343296c694620ca9981ac19d70759c845fde"
---

# Migrating from the MongoDB Data API to Supabase

MongoDB announced that their Data API and HTTPS Endpoints will reach end-of-life by September 30, 2025. This has left many engineering teams evaluating alternatives. Supabase includes[built-in support for REST APIs](https://supabase.com/features/auto-generated-rest-api) (via PostgREST), mirroring the core functionality previously provided by MongoDB's Data API.


## The implications of MongoDB's Data API deprecation#


MongoDB's Data API allowed developers to interact with their Atlas databases through straightforward RESTful endpoints, simplifying integration across frontend, backend, and serverless applications. With its removal, developers must pivot to alternative methods such as native MongoDB drivers combined with backend frameworks (Express, SpringBoot, FastAPI), or third-party solutions like RESTHeart, Hasura, or Neurelo.


This shift requires substantial refactoring for teams that rely on the simplified REST interface.


## Supabase as a MongoDB alternative#


Supabase is an open-source MongoDB alternative and offers:


- A managed Postgres database, eliminating vendor lock-in.
- Built-in REST API via open-source PostgREST, directly analogous to MongoDB's Data API.
- Real-time capabilities, authentication, file storage, and serverless functions—all tightly integrated.


Most notably, Supabase[automatically generates a RESTful API](https://supabase.com/docs/guides/api#rest-api-overview) from your database schema, powered by PostgREST, itself a stable open-source project with a strong community. This feature accelerates development by eliminating boilerplate code for basic CRUD operations. Supabase provides a near drop-in replacement for MongoDB’s Data API. The Supabase Data API also supports GraphQL and a host of mobile and web application frameworks.


In the end, you can focus on migrating your data, expose your data using Supabase’s Data API, and seamlessly integrate with your client applications.


## Migrating from MongoDB to Supabase: a step-by-step guide#


To migrate to Supabase, you will need to:


1. Export your MongoDB data
2. Import JSON data into Supabase
3. Normalize the data using SQL
4. Transition your existing MongoDB Data API calls to Supabase PostgREST
5. Add more Supabase features to round out your app


### Step 1: Export your MongoDB data#


Export MongoDB documents using mongoexport:


`
_10


mongoexport --uri="mongodb+srv://<username>:<password>@cluster.mongodb.net/<dbname>" \\


_10


--collection=users \\


_10


--jsonArray \\


_10


--out=users.json


`


### Step 2: Import JSON data into Supabase#


Create a table in Supabase with a JSONB column to store raw Mongo documents:


`
_10


create table mongo_users_raw (


_10


id uuid primary key default gen_random_uuid(),


_10


data jsonb not null


_10


);


`


Then, ingest the exported JSON data into this Supabase table using this custom script:


`
_31


import { createClient } from '@supabase/supabase-js'


_31


import fs from 'fs'


_31


_31


const supabaseUrl = 'YOUR_SUPABASE_URL'


_31


const supabaseKey = 'YOUR_SUPABASE_API_KEY'


_31


const tableName = 'YOUR_TABLE_NAME'


_31


const jsonFilePath = './filename.json'


_31


_31


const supabase = createClient(supabaseUrl, supabaseKey)


_31


_31


async function loadDocumentsToSupabase() {


_31


try {


_31


// Read JSON file


_31


const rawData = fs.readFileSync(jsonFilePath)


_31


const dataArray = JSON.parse(rawData).map((data) => ({ data }))


_31


_31


// Insert data into Supabase


_31


const { error } = await supabase.from(tableName).insert(dataArray)


_31


_31


if (error) {


_31


console.error('Error inserting data:', error)


_31


return


_31


}


_31


_31


console.log(\`Successfully inserted ${dataArray.length} records into ${tableName}\`)


_31


} catch (error) {


_31


console.error('Error in process:', error)


_31


}


_31


}


_31


_31


loadDocumentsToSupabase()


`


### Step 3: Normalize the data using SQL#


Once your data is imported as JSONB, leverage PostgreSQL’s powerful JSON functions to incrementally normalize and populate relational tables:


`
_16


-- Example to normalize user data


_16


INSERT INTO users (name, email)


_16


SELECT


_16


data->>'name' as name,


_16


data->>'email' as email


_16


FROM mongo_users_raw;


_16


_16


-- Example to normalize orders into a separate table


_16


INSERT INTO orders (user_id, product, quantity)


_16


SELECT


_16


u.id,


_16


orders->>'product',


_16


(orders.value->>'quantity')::INTEGER


_16


FROM mongo_users_raw m


_16


JOIN users u ON (m.data->>'name') = u.name,


_16


LATERAL jsonb_array_elements(m.data->'orders') AS order_data


`


### Step 4: Transition API calls to Supabase PostgREST#


Once your data has been structured into tables, Supabase automatically generates REST APIs for each table via PostgREST, allowing effortless querying from your application.


For example, if you have a users table, querying user information using Supabase’s JavaScript library would look like this:


`
_20


import { createClient } from '@supabase/supabase-js'


_20


_20


const supabase = createClient('https://<your-project>.supabase.co', '<your-api-key>')


_20


_20


// Fetch user named John along with their orders


_20


const { data, error } = await supabase


_20


.from('users')


_20


.select(


_20


\`


_20


id, name,


_20


orders (product, quantity)


_20


\`


_20


)


_20


.eq('name', 'John')


_20


_20


if (error) {


_20


console.error(error)


_20


} else {


_20


console.log(data)


_20


}


`


Using the automatically generated Supabase REST APIs offers a clear migration path from MongoDB’s deprecated Data API.


### Step 5: Add more Supabase features#


Once your data is migrated, you can start to use Supabase to its fullest:


- [Authentication](https://supabase.com/auth) . Let your users login with email, Google, Apple, GitHub, and more. Secure and trusted.
- [Role-Based Access Control (RBAC)](https://supabase.com/docs/guides/database/postgres/custom-claims-and-role-based-access-control-rbac) . Secure your data properly.
- [Storage](https://supabase.com/storage) . Affordable and fast, for all the videos and images you need in your app.
- [Edge Functions](https://supabase.com/edge-functions) . Custom backend logic when you want to dive into code.
- [Realtime](https://supabase.com/realtime) . Build immersive multi-player, collaborative experiences.
- [AI Ready](https://supabase.com/modules/vector) . When you’re ready to explore vectors and the power of AI, Supabase is there with industry-standard tools to guide you.
- [Foreign Data Wrappers (FDW)](https://supabase.com/docs/guides/database/extensions/wrappers/overview) . Pull data from Google Sheets, Airtable, MySQL, and more, as if they were part of Supabase natively.
- **Instant and secure deployment** . No need to set up servers, manage DevOps, or tweak security settings.


## Key considerations and trade-offs#


- **Schema flexibility** : PostgreSQL schemas are less flexible than MongoDB; careful upfront design mitigates this.
- **Complexity in migration** : Document structures require transformation scripts.
- **Query patterns** : Analyze existing MongoDB queries to optimize relational joins.
- **Add indexes** : Use the Index Advisor in the Supabase Dashboard to further optimize your tables.


## Conclusion#


Supabase is an ideal replacement for MongoDB, especially considering the Data API deprecation. Supabase is built on Postgres, one of the world’s most powerful and scalable databases. In addition, Supabase’s Data API directly parallels MongoDB's Data API, offering a simplified transition path.


[This blog post](https://blog.mansueli.com/migrating-from-mongodb-to-supabase-with-postgresql) provides further detail on how to transition from MongoDB. If you’re encountering difficulty, feel free to reach out to us. We’d be happy to help.


With careful planning and methodical execution, engineering teams can navigate this migration confidently, leveraging Supabase as a trusted, long-term solution.
