---
schema_version: "1.0.0"
document_id: "dfa8783251f2f63732f1e4ac238251ecfd1f330a9e2f1c3bc6a78d17488e54b4"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/chatgpt-plugins-support-postgres"
published_at: "2023-05-25T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:8492c38514d87600d3d848c8f4091cf143658c7d729c041e3911245c89e981e8"
---

# ChatGPT plugins now support Postgres & Supabase

One of the challenges that ChatGPT faces is being able to answer questions from a private dataset. We can solve this with “retrieval plugins”, which allow ChatGPT to access information from a database.


Supabase recently contributed to the OpenAI repo with a[Postgres](https://github.com/openai/chatgpt-retrieval-plugin#postgres) and a[Supabase](https://github.com/openai/chatgpt-retrieval-plugin#supabase) implementation to help developers build plugins using pgvector.


Let’s dig into the specifics of Retrieval plugins, then we can implement an example - we’ll ingest all of the Postgres docs into a Supabase database, then get ChatGPT to answer questions. It’s a contrived example since ChatGPT already knows about Postgres, but what other data source would Supabase want to use?


## What is ChatGPT Retrieval Plugin?#


ChatGPT recently released[Plugins](https://openai.com/blog/chatgpt-plugins) which help ChatGPT access up-to-date information, run computations, or use third-party services.


A[Retrieval Plugin](https://github.com/openai/chatgpt-retrieval-plugin) is a Python project designed to inject external data into the ChatGPT. It allows ChatGPT to dynamically pull relevant information into conversations from your data sources. This could be PDF documents, Confluence, or Notion knowledge bases.


A retrieval plugin does a few things:


1. Turn documents into smaller chunks.
2. Converts chunks into embeddings using OpenAI's` text-embedding-ada-002` model.
3. Stores the embeddings into a vector database.
4. Queries the vector database for relevant documents when a question is asked.


You can choose your preferred vector database provider from a list of[supported options](https://github.com/openai/chatgpt-retrieval-plugin#choosing-a-vector-database) .


## Adding Supabase and Postgres as Datastore options for ChatGPT Retrieval Plugin#


We've implemented two vector provider options: one for Postgres and one for Supabase. The main differences are:


- The Postgres version uses the` psycopg2` python library to directly connect to the database.
- The Supabase version interacts with the database via PostgREST. This is helpful if you want to use[Row Level Security](https://supabase.com/docs/guides/auth/row-level-security) or if you are planning to use the data in the Retrieval store beyond ChatGPT.


The Postgres implementation is great to start with because there are now a[large number of providers](https://github.com/pgvector/pgvector/issues/54) supporting pgvector.


Both have the same schema so you can easily switch between them:


`
_11


create table if not exists documents (


_11


id text primary key default gen_random_uuid()::text,


_11


source text,


_11


source_id text,


_11


content text,


_11


document_id text,


_11


author text,


_11


url text,


_11


created_at timestamptz default now(),


_11


embedding vector(1536)


_11


);


`


When you create the retrieval store inside your database, a stored function is implemented to query and find relevant information for your question to ChatGPT:


`
_54


create or replace function match_page_sections(


_54


in_embedding vector(1536),


_54


in_match_count int default 3,


_54


in_document_id text default '%%',


_54


in_source_id text default '%%',


_54


in_source text default '%%',


_54


in_author text default '%%',


_54


in_start_date timestamptz default '-infinity',


_54


in_end_date timestamptz default 'infinity'


_54


)


_54


returns table (


_54


id text,


_54


source text,


_54


source_id text,


_54


document_id text,


_54


url text,


_54


created_at timestamptz,


_54


author text,


_54


content text,


_54


embedding vector(1536),


_54


similarity float


_54


)


_54


language plpgsql


_54


as $$


_54


#variable_conflict use_variable


_54


begin


_54


return query


_54


_54


select


_54


documents.id,


_54


documents.source,


_54


documents.source_id,


_54


documents.document_id,


_54


documents.url,


_54


documents.created_at,


_54


documents.author,


_54


documents.content,


_54


documents.embedding,


_54


(documents.embedding <#> in_embedding) * -1 as similarity


_54


from


_54


documents


_54


where


_54


in_start_date <= documents.created_at and


_54


documents.created_at <= in_end_date and


_54


(documents.source_id like in_source_id or documents.source_id is null) and


_54


(documents.source like in_source or documents.source is null) and


_54


(documents.author like in_author or documents.author is null) and


_54


(documents.document_id like in_document_id or documents.document_id is null)


_54


order by


_54


documents.embedding <#> in_embedding


_54


limit


_54


in_match_count;


_54


end;


_54


$$;


`


We apply filters based on the source, author, document, and date, and find the closest embeddings using the inner product distance function. This function offers the best performance when the embeddings are normalized, which is the case for OpenAI embeddings. The similarity is calculated as:` (documents.embedding <#> in_embedding) * -1 as similarity` . And that’s it, you can now seamlessly use the Retrieval Plugin with a Postgres Database underneath, eliminating the need for any manual implementation on your end.


## Example: Chat with Postgres Docs#


Let’s build an example where we can “ask ChatGPT questions” about the Postgres documentation.


This will require several steps:


1. Download all the[Postgres docs as a PDF](https://www.postgresql.org/files/documentation/pdf/15/postgresql-15-US.pdf)
2. Convert the docs into chunks of embedded text and store them in Supabase
3. Run our plugin locally so that we can ask questions about the Postgres docs.


### Step 1: Fork the ChatGPT Retrieval Plugin repository#


Fork the ChatGPT Retrieval Plugin repository to your GitHub account and clone it to your local machine. Read through the` README.md` file to understand the project structure.


### Step 2: Install dependencies#


Choose your desired datastore provider and remove unused dependencies from` pyproject.toml` . For this example, we'll use Supabase. And install dependencies with Poetry:


`
_10


poetry install


`


### Step 3: Create a Supabase project#


Create a[Supabase project](https://supabase.com/dashboard) and database by following the instructions[here](https://supabase.com/docs/guides/platform) . Export the environment variables required for the retrieval plugin to work:


`
_10


export OPENAI_API_KEY=<open_ai_api_key>


_10


export DATASTORE=supabase


_10


export SUPABASE_URL=<supabase_url>


_10


export SUPABASE_SERVICE_ROLE_KEY=<supabase_key>


`


For Postgres datastore, you'll need to export these environment variables instead:


`
_10


export OPENAI_API_KEY=<open_ai_api_key>


_10


export DATASTORE=postgres


_10


export PG_HOST=<postgres_host_url>


_10


export PG_PASSWORD=<postgres_password>


`


### Step 4: Run Postgres Locally#


To start quicker you may use Supabase CLI to spin everything up locally as it already includes pgvector from the start. Install` supabase-cli` , go to the` examples/providers` folder in the repo and run:


`
_10


supabase start


`


This will pull all docker images and run supabase stack in docker on your local machine. It will also apply all the necessary migrations to set the whole thing up. You can then use your local setup the same way, just export the environment variables and follow to the next steps.


Using` supabase-cli` is not required and you can use any other docker image or hosted version of PostgresDB that includes` pgvector` . Just make sure you run migrations from` examples/providers/supabase/migrations/20230414142107_init_pg_vector.sql` .


### Step 5: Obtain OpenAI API key#


To create embeddings Plugin uses OpenAI API and` text-embedding-ada-002` model. Each time we add some data to our datastore, or try to query relevant information from it, embedding will be created either for inserted data chunk, or for the query itself. To make it work we need to export` OPENAI_API_KEY` . If you already have an account in OpenAI, you just need to go to[User Settings - API keys](https://platform.openai.com/account/api-keys) and Create new secret key.


### Step 6: Run the plugin!#


Execute the following command to run the plugin:


`
_10


poetry run dev


_10


# output


_10


INFO: Will watch for changes in these directories: \['./chatgpt-retrieval-plugin'\]


_10


INFO: Uvicorn running on http://localhost:3333 (Press CTRL+C to quit)


_10


INFO: Started reloader process \[87843\] using WatchFiles


_10


INFO: Started server process \[87849\]


_10


INFO: Waiting for application startup.


_10


INFO: Application startup complete.


`


The plugin will start on your localhost - port :3333 by default.


### Step 6: Populating data in the datastore#


For this example, we'll upload Postgres documentation to the datastore. Download the[Postgres documentation](https://www.postgresql.org/files/documentation/pdf/15/postgresql-15-US.pdf) and use the` /upsert-file` endpoint to upload it:


`
_10


curl -X POST -F \\\\"file=@./postgresql-15-US.pdf\\\\" <http://localhost:3333/upsert-file>


`


The plugin will split your data and documents into smaller chunks automatically. You can view the chunks using the Supabase dashboard or any other SQL client you prefer. For the whole Postgres Documentation I got 7,904 records in my documents table, which is not a lot, but we can try to add index for` embedding` column to speed things up by a little. To do so, you should run the following SQL command:


`
_10


create index on documents


_10


using ivfflat (embedding vector_ip_ops)


_10


with (lists = 10);


`


This will create an index for the inner product distance function. Important to note that it is an approximate index. It will change the logic from performing the exact nearest neighbor search to the approximate nearest neighbor search.


We are using` lists = 10` , because as a general guideline, you should start looking for optimal lists constant value with the formula:` rows / 1000` when you have less than 1 million records in your table.


Now, it is time to add our plugin to ChatGPT.


### Empowering ChatGPT with Postgres knowledge#


To integrate our plugin with ChatGPT, register it in the ChatGPT dashboard. Assuming you have access to ChatGPT Plugins and plugin development, select the Plugins model in a new chat, then choose "Plugin store" and "Develop your own plugin." Enter` localhost:3333` into the domain input, and your plugin is now part of ChatGPT.


You can now ask questions about Postgres and receive answers derived from the documentation!


Let's try it out: ask ChatGPT to find out when to use` check` and when to use` using` . You will be able to see what queries were sent to our plugin and what it responded to.


And after ChatGPT receives a response from the plugin it will answer your question with the data from the documentation.


## Wrap up#


It's easy to bring any context into the datastore and utilize it with ChatGPT. Simply export your knowledge base from platforms like Notion or Confluence, upload it to the datastore, and you're good to go. You can also use any other datastore provider you prefer.


And the good news is that you’re not limited by using it with ChatGPT, you can embed it in your website or documentation, and build a Slack bot or telegram bot to answer questions about your company or product. For that, you will only need to add a single call to OpenAI API to create a summary of data retrieved from the Plugin. You can find some inspiration on how to do that in our blog post about building[Supabase Clippy assistant](https://supabase.com/blog/chatgpt-supabase-docs) .


Let us know on[Twitter](https://twitter.com/Supabase) if you are building ChatGPT Plugins. We can’t wait to see what you will build!


## More AI resources#


- [OpenAI ChatGPT Plugin docs](https://platform.openai.com/docs/plugins/introduction)
- [ChatGPT Retrieval Plugin Repo](https://github.com/openai/chatgpt-retrieval-plugin)
- [How to build ChatGPT Plugin from scratch with Supabase Edge Runtime](https://supabase.com/blog/building-chatgpt-plugins-template) .
- [Docs pgvector: Embeddings and vector similarity](https://supabase.com/docs/guides/database/extensions/pgvector)
