---
schema_version: "1.0.0"
document_id: "57947ebbadd7da493d36d0b17bcb909966e051b2ce2bfb5574569288840be4ad"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/migrating-from-fauna-to-supabase"
published_at: "2025-03-21T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:23e3f3afa6c05579e0984b17ffbf5abf45481f13f720ae1b6a2bb92c8774df72"
---

# Migrating from Fauna to Supabase

Fauna recently announced they will sunset their product by the end of May 2025, prompting engineering teams to find reliable alternatives quickly. Supabase offers a natural migration path for Fauna users, providing a robust, scalable, and open-source alternative built on Postgres.


## The implications of Fauna sunsetting#


Fauna was known for its serverless database model, offering easy scalability, flexible data modeling, and integrated GraphQL APIs. Teams depending on Fauna must now evaluate alternatives carefully, considering impacts on data modeling, querying, and backend logic.


Migrating away from Fauna requires adjustments in query logic, schema definition, and overall application architecture.


## Why Supabase is a strong replacement for Fauna#


Supabase is an open-source Postgres development platform that offers:


- **Managed Postgres database** : Stability, reliability, and strong SQL ecosystem with mature tooling.
- **Automatically generated REST APIs (via PostgREST)** : Direct analog to Fauna’s built-in API functionality.
- **Native JSONB Support:** Supabase supports JSONB data types as well as JSON functions and operators to efficiently query collection-style data, prior to full-fledged normalization.
- **Real-time database updates** , integrated authentication, secure file storage, and edge functions.
- **Row-level security** : Built directly into the database, allowing precise access control.
- **Robust TypeScript support** : Seamless integration with modern development workflows.
- **Full ACID compliance** : Stronger transactional guarantees than Fauna.
- **Predictable pricing** : Transparent, usage-based billing that avoids surprises.
- **Improved local and remote development workflow** : Efficient, predictable, and familiar to most developers.
- **Global community** . Supabase has a vibrant community of users and contributors around the world.


## Migrating from Fauna to Supabase: a step-by-step guide#


Migrating across data structures can be difficult, and normalizing large sets of unstructured or semi-structured data can take time. Given the May 30th Fauna Sunset deadline, we recommend a two-phase approach to ensure your application stays online.


**Phase 1: Export and load Into Supabase**


In this phase, your data is safely moved to Supabase before the Fauna sunset date and your applications will still function properly.


1. Export data from Fauna
2. Import JSON data into Supabase
3. Transition existing Fauna API calls to Supabase PostgREST APIs


**Phase 2: Optimize and enhance**


In this phase, with your data secured and your applications still functional, you can safely and confidently complete the transition to Supabase.


1. SQL data normalization and PostgREST update
2. Incorporation of additional Supabase features such as authentication, and object storage


## Phase 1#


Phase 1 of the Fauna to Supabase migration focuses on exporting your data from Fauna, importing into Supabase as a JSONB data type, and rewriting your data APIs to use the Supabase SDK.


### Step 1: Export data from Fauna#


Fauna allows exporting collections through their admin dashboard or CLI. Use the Fauna CLI to export your collections to Amazon S3 in JSON format:


`
_10


fauna export create s3 \\


_10


--database <database_name> \\


_10


--collection <collection_name> \\


_10


--bucket <s3_bucket_name> \\


_10


--path <s3_bucket_path> \\


_10


--format simple


`


Fauna has[also provided instructions](https://docs.fauna.com/fauna/current/migrate/?lang=javascript) using the Fauna Query Language.


### Step 2: Import JSON Data into Supabase#


Create a table in Supabase with a JSONB column to store raw Fauna documents:


`
_10


create table fauna_users_raw (


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


### Step 3: Transition Fauna API calls to Supabase PostgREST#


Once your data has been structured into tables, Supabase automatically generates REST APIs for each table via PostgREST, allowing effortless querying from your application.


Here’s a Fauna query example (using FQL) for obtaining data from a` users` table:


`
_15


import { Client, fql } from 'fauna'


_15


_15


const client = new Client({ secret: '<your-fauna-secret>' })


_15


_15


const usersQuery = fql\`


_15


users.all() {


_15


name,


_15


email


_15


}


_15


\`


_15


_15


client


_15


.query(usersQuery)


_15


.then((data) => console.log(data))


_15


.catch((error) => console.error('Error fetching users:', error))


`


And here’s the equivalent Supabase REST API call:


`
_11


import { createClient } from '@supabase/supabase-js'


_11


_11


const supabase = createClient('https://<your-project>.supabase.co', '<your-api-key>')


_11


_11


const { data, error } = await supabase.from('users').select(\`


_11


user: metadata->user


_11


\`)


_11


// the -> operator returns values as jsonb for the user collection


_11


_11


if (error) console.error(error)


_11


else console.log(data)


`


## Phase 2:#


Once you have brought your collections over to Supabase, you may find you would benefit from data normalization. As Supabase is built on top of Postgres, having normalized data will lead to significant performance benefits that cannot be matched by a set of collections stored in JSONB.


### Step 1: Normalize the data using SQL#


Once your data is imported as JSONB, leverage the powerful Postgres JSON functions to incrementally normalize and populate relational tables. In this example, we’re importing data from a rudimentary` users` table:


`
_16


-- Example normalization for users


_16


INSERT INTO users (name, email)


_16


SELECT


_16


data->'data'->'name' AS name,


_16


data->'data'->'email' AS email


_16


FROM fauna_users_raw;


_16


_16


-- Example normalization of nested orders


_16


INSERT INTO orders (user_id, product, quantity)


_16


SELECT


_16


u.id,


_16


order_data->>'product',


_16


(order_data->>'quantity')::INTEGER


_16


FROM fauna_users_raw f


_16


JOIN users u ON (f.data->'data'->>'email') = u.email,


_16


LATERAL jsonb_array_elements(f.data->'data'->'orders') AS order_data;


`


### Step 1.5: Rewrite PostgREST to query normalized data#


Here’s the PostgREST query for JSONB data from Phase 1:


`
_11


import { createClient } from '@supabase/supabase-js'


_11


_11


_11


_11


const { data, error } = await supabase.from('users').select(\`


_11


user: metadata->user


_11


\`)


_11


// the -> operator returns values as jsonb for the user collection


_11


_11


if (error) console.error(error)


_11


else console.log(data)


`


And here’s the equivalent Supabase REST API call with normalized data:


`
_10


import { createClient } from '@supabase/supabase-js'


_10


_10


_10


_10


const { data, error } = await supabase.from('users').select('name, email')


_10


_10


if (error) console.error(error)


_10


else console.log(data)


`


### Step 2: Add more Supabase features#


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


- **Schema Flexibility** : Postgres schemas require careful planning compared to Fauna’s more flexible data structures.
- **Migration Complexity** : Fauna’s nested structures necessitate normalization scripts.
- **Query Refactoring** : Fauna’s FQL/GraphQL queries must be translated to SQL queries.
- **Add indexes** : Use the Index Advisor in the Supabase Dashboard to further optimize your tables.


## Conclusion#


This is no doubt a stressful time as you transition away from Fauna. Supabase is here to help you every step of the way. Reach out to us and we can help you plan your transition and provide assistance.


Supabase is a comprehensive, scalable replacement for Fauna. Supabase is built on Postgres and offers a robust relational model, powerful security features, and predictable pricing. Supabase enables engineering teams to confidently transition away from Fauna thanks to its SQL ecosystem, more mature/better tooling, row level security, strong typescript support, and full ACID compliance. Thoughtful planning and methodical execution will ensure a seamless migration and long-term reliability.
