---
schema_version: "1.0.0"
document_id: "14330d7965ab39146055ae71a8127e7b343fe75e18280bae4e449263b550c53d"
company_key: "yc-mindsdb"
company: "MindsDB"
source_id: "yc-mindsdb-news-import-209cdb193474"
canonical_url: "https://mindshub.ai/blog/building-a-semantic-search-knowledge-base-with-mindsdb"
published_at: "2026-01-27T00:00:00+00:00"
first_seen_at: "2026-07-22T04:26:30.664731+00:00"
fetched_at: "2026-07-28T21:26:59.511520+00:00"
content_hash: "sha256:4020f1f5920889ae42f83c265dee7ee77c960a5f23b8bfa91b0f21c42a1a4d09"
---

# Building a Semantic Search Knowledge Base with MindsDB

What happens when a developer searches for “how to make async HTTP calls” but your documentation says “asynchronous network requests”? Traditional keyword search fails - even though the content is exactly what they need.


This is the fundamental limitation of keyword search: it matches words, not meaning.


In this tutorial, we’ll build a semantic search system using MindsDB that understands user intent. Using 2 million Stack Overflow posts, we’ll create knowledge bases with two different vector storage backends - **PGVector** and **FAISS** - and compare their performance.


### **What You’ll Learn:**


-


How MindsDB knowledge bases convert text into searchable vectors


-


Setting up pgvector (PostgreSQL-based) and FAISS (Facebook AI Similarity Search) storage


-


Combining semantic search with metadata filters


-


Building an AI agent that uses your knowledge base to answer questions


**Prerequisites:**


-


A MindsDB account (cloud or self-hosted)


-


PostgreSQL database with the Stack Overflow dataset


-


An OpenAI API key for embeddings


## **How Semantic Search Works**


Before we dive in, let’s understand the key difference between keyword and semantic search:


Aspect Keyword Search Semantic Search


Matching Exact words Meaning/intent


Query: “async HTTP” Misses “asynchronous requests” Finds both


Handles synonyms ❌ ✅


Understands context ❌ ✅


Semantic search works by:


1.


**Embedding** : Converting text into numerical vectors using an embedding model


2.


**Storing** : Saving these vectors in a vector database


3.


**Querying** : Converting the search query to a vector and finding the closest matches


MindsDB handles all of this through its Knowledge Base abstraction.


## **Installing Dependencies**


We need two packages:


-


**mindsdb_sdk** : Python client for interacting with MindsDB servers


-


**pandas** : For working with query results as DataFrames


```text
!  pip   install   mindsdb_sdk   pandas
```


## **3. Connecting to the MindsDB Cloud Instance**


```text
import   mindsdb_sdk


# Connect to your MindsDB instance
server   =   mindsdb_sdk.connect(
'YOUR_MINDSDB_URL'  ,    # e.g., 'https://cloud.mindsdb.com' for MindsDB Cloud
login  =  'YOUR_USERNAME'  ,
password  =  'YOUR_PASSWORD'
)
print  (  "Connected to MindsDB server"  )
```


```text
Connected to MindsDB server
```


## **4. Connecting to the Data Source**


```text
def   run_query  (sql, success_msg  =  "Query executed successfully"  ):
"""Execute a SQL query and handle 'already exists' errors gracefully."""
try  :
result   =   server.query(sql).fetch()
print  (success_msg)
return   result
except   RuntimeError   as   e:
if   "already exists"   in   str  (e).lower():
print  (  "Resource already exists - skipping"  )
else  :
raise
return   None


# Connect to your PostgreSQL database containing Stack Overflow data
run_query(  """
CREATE DATABASE pg_sample
WITH ENGINE = "postgres",
PARAMETERS = {
"user": "YOUR_PG_USER",
"password": "YOUR_PG_PASSWORD",
"host": "YOUR_PG_HOST",
"port": "5432",
"database": "sample"
}
"""  ,   "Created pg_sample database connection"  )
```


```text
Created pg_sample database connection
```


Let’s verify the connection by exploring the data. Check the dataset size:


```text
# Get total row count
count = server.query("SELECT COUNT(*) as cnt FROM pg_sample.stackoverflow_2m").fetch()
print(f"Dataset size: {count['cnt'].iloc[0]:,} rows")
```


```text
Dataset size: 2,000,000 rows
```


Show 10 records:


```text
# Test sample data
df   =   server.query(  "SELECT * FROM pg_sample.stackoverflow_2m LIMIT 10"  ).fetch()


# Display as a nice table (in Jupyter notebooks)
from   IPython.display   import   display
display(df)
```


id PostTypeId AcceptedAnswerId ParentId Score ViewCount Body Title ContentLicense FavoriteCount CreationDate LastActivityDate LastEditDate LastEditorUserId OwnerUserId Tags


0 7 2 NaN 4.0 522 NaN An explicit cast to` double` like this isn’t n… None CC BY-SA 4.0 NaN 2008-07-31T22:17:57.883 2019-10-21T14:03:54.607 2019-10-21T14:03:54.607 5496973.0 9.0 None


1 9 1 1404.0 NaN 2199 784860.0 Given a` DateTime` representing a person’s bir… How do I calculate someone’s age based on a Da… CC BY-SA 4.0 0.0 2008-07-31T23:40:59.743 2023-02-02T18:38:32.613 2022-07-27T22:34:36.320 3524942.0 1.0 c#,.net,datetime


2 11 1 1248.0 NaN 1644 197314.0 Given a specific` DateTime` value, how do I di… Calculate relative time in C# CC BY-SA 4.0 0.0 2008-07-31T23:55:37.967 2022-09-05T11:26:30.187 2022-07-10T00:19:55.237 16790137.0 1.0 c#,datetime,time,datediff,relative-time-span


3 14 1 NaN NaN 491 173083.0 What is the difference between \[Math.Floor()\](… Difference between Math.Floor() and Math.Trunc… CC BY-SA 3.0 0.0 2008-08-01T00:59:11.177 2022-04-22T08:59:43.817 2017-02-25T17:42:17.810 6495084.0 11.0 .net,math


4 6 1 31.0 NaN 319 23465.0 I have an absolutely positioned` div` containi… Why did the width collapse in the percentage w… CC BY-SA 4.0 0.0 2008-07-31T22:08:08.620 2021-01-29T18:46:45.963 2021-01-29T18:46:45.963 9134576.0 9.0 html,css,internet-explorer-7


5 12 2 NaN 11.0 347 NaN Here’s how I do it\\n\\n\`\`\`\\nvar ts = new TimeSp… None CC BY-SA 4.0 NaN 2008-07-31T23:56:41.303 2020-06-13T10:30:44.397 2020-06-13T10:30:44.397 238419.0 1.0 None


6 13 1 NaN NaN 701 277780.0 Is there a standard way for a web server to be… Determine a user’s timezone CC BY-SA 4.0 0.0 2008-08-01T00:42:38.903 2022-03-29T07:31:31.320 2020-12-03T03:37:56.313 584192.0 9.0 html,browser,timezone,user-agent,timezone-offset


7 4 1 7.0 NaN 794 70633.0 I want to assign the decimal variable “trans” … How to convert Decimal to Double in C#? CC BY-SA 4.0 0.0 2008-07-31T21:42:52.667 2022-09-08T05:07:26.033 2022-09-08T05:07:26.033 16124033.0 8.0 c#,floating-point,type-conversion,double,decimal


8 17 1 26.0 NaN 198 85547.0 How do I store binary data in \[MySQL\]([http://e](http://e/) … Binary Data in MySQL CC BY-SA 3.0 0.0 2008-08-01T05:09:55.993 2020-12-03T03:37:51.763 2020-12-03T03:37:51.763 584192.0 2.0 mysql,database,binary-data,data-storage


9 24 1 49.0 NaN 193 101180.0 If I have a trigger before the update on a tab… Throw an error preventing a table update in a … CC BY-SA 4.0 0.0 2008-08-01T12:12:19.350 2021-01-29T12:57:17.153 2021-01-29T12:57:17.153 14152908.0 22.0 mysql,database,triggers


The Stack Overflow dataset contains 2 million posts - both questions (` PostTypeId=1` ) and answers (` PostTypeId=2` ). Key columns include:


-


` Id` - Unique identifier for each post


-


` Body` - The content we’ll make semantically searchable


-


` Title` - The title of the post (questions only)


-


` Tags` - Programming language and topic tags (e.g.,` python` ,` javascript` )


-


` Score` - Community voting score - useful for prioritizing high-quality content


-


` ViewCount` - Popularity metric for filtering


-


` PostTypeId` - Type of post (1=question, 2=answer)


-


` AcceptedAnswerId` - ID of the accepted answer (for questions)


-


` CreationDate` ,` LastActivityDate` ,` LastEditDate` - Timestamps


This rich metadata allows us to combine semantic understanding with traditional filters - for example, finding Python questions about async programming with a score above 10.


## **4. Setting Up Vector Storage Backends**


MindsDB supports multiple vector storage options. We’ll set up both pgvector and a recently added FAISS and will compare how quick they are.


### **PGVector (PostgreSQL Extension)**


pgvector is a PostgreSQL extension for vector similarity search. It’s ideal when you want to keep vectors alongside your relational data.


```text
#   Create   pgvector   database   connection
run_query(  """
CREATE DATABASE pg_vector
WITH ENGINE = "  pgvector  ",
PARAMETERS = {
"  user  ": "  YOUR_PG_USER  ",
"  password  ": "  YOUR_PG_PASSWORD  ",
"  host  ": "  YOUR_PG_HOST  ",
"  port  ": "  5432  ",
"  database  ": "  vector  "
}
"""  ,   "Created pg_vector database connection"  )
```


```text
Created pg_vector database connection
```


### **FAISS (Facebook AI Similarity Search)**


FAISS is a library for efficient similarity search developed by Facebook AI Research. It’s optimized for fast similarity search on large datasets.


```text
#   Create   FAISS   database   connection
run_query(  """
CREATE DATABASE db_faiss
WITH ENGINE = 'duckdb_faiss',
PARAMETERS = {
"  persist_directory  ": "  /  home  /  ubuntu  /  faiss  "
}
"""  ,   "Created db_faiss database connection"  )
```


```text
Created db_faiss database connection
```


## **Choosing Between PGVector and FAISS**


Feature pgvector FAISS


Best for Integration with existing PostgreSQL Maximum query speed


Persistence Native PostgreSQL storage File-based


Scalability Good (PostgreSQL limits) Excellent (billions of vectors)


Setup complexity Requires PostgreSQL extension Standalone


Query speed Good (~19s for 2M vectors) Excellent (~5s for 2M vectors)


For this tutorial, we’ll implement both so you can see the performance difference firsthand.


## **5. Creating Knowledge Bases**


Now we have a table with relational data and two vector stores to keep the embedding vectors. We are ready to create knowledge bases using both storage backends.


The knowledge base will:


-


Use OpenAI’s` text-embedding-3-small` model for generating embeddings


-


Store the post` Body` as searchable content


-


Include metadata fields for filtering results


### **Knowledge Base with PGVector Storage**


```text
def   kb_exists  (kb_name):
"""Check if a knowledge base already exists."""
try  :
result   =   server.query(  "SELECT name FROM information_schema.knowledge_bases"  ).fetch()
return   kb_name   in   result[  'name'  ].values
except   Exception  :
return   False


# Create pgvector knowledge base
if   kb_exists(  "kb_stack_vector"  ):
print  (  "kb_stack_vector already exists - skipping creation"  )
else  :
run_query(  """
CREATE KNOWLEDGE_BASE kb_stack_vector
USING
storage = pg_vector.stack,
embedding_model = {
"provider": "openai",
"model_name": "text-embedding-3-small"
},
content_columns = ['Body'],
metadata_columns = [
"PostTypeId",
"AcceptedAnswerId",
"ParentId",
"Score",
"ViewCount",
"Title",
"ContentLicense",
"FavoriteCount",
"CreationDate",
"LastActivityDate",
"LastEditDate",
"LastEditorUserId",
"OwnerUserId",
"Tags"
]
"""  ,   "Created kb_stack_vector knowledge base"  )
```


```text
Created kb_stack_vector knowledge base
```


### **Knowledge Base with FAISS Storage**


```text
#   Create   FAISS knowledge base
if   kb_exists(  "kb_stack_faiss"  ):
print  (  "kb_stack_faiss already exists - skipping creation"  )
else  :
run_query(  """
CREATE KNOWLEDGE_BASE kb_stack_faiss
USING
storage = db_faiss.stack,
embedding_model = {
"  provider  ": "  openai  ",
"  model_name  ": "  text-  embedding  -  3  -  small  "
},
content_columns = ['Body'],
metadata_columns = [
"  PostTypeId  ",
"  AcceptedAnswerId  ",
"  ParentId  ",
"  Score  ",
"  ViewCount  ",
"  Title  ",
"  ContentLicense  ",
"  FavoriteCount  ",
"  CreationDate  ",
"  LastActivityDate  ",
"  LastEditDate  ",
"  LastEditorUserId  ",
"  OwnerUserId  ",
"  Tags  "
]
"""  ,   "Created kb_stack_faiss knowledge base"  )
```


```text
Created kb_stack_faiss knowledge base
```


### **Understanding the Parameters**


Parameter Description


storage Specifies the vector database connection and table name


embedding_model Configuration for the embedding model (provider and model name)


content_columns Columns to embed and make semantically searchable


metadata_columns Columns available for filtering (not embedded, but stored)


## **6. Loading Data into Knowledge Bases**


Now we’ll insert the Stack Overflow data into our knowledge bases. This process:


1.


Fetches data from the source table in batches


2.


Generates embeddings for content columns using the OpenAI API


3.


Stores vectors and metadata in the vector database


### **Loading Data into PGVector Knowledge Base**


```text
def   is_kb_empty  (kb_name):
"""Check if a knowledge base is empty (fast - only fetches 1 row)."""
result   =   server.query(  f  "SELECT id FROM   {  kb_name  }   LIMIT 1"  ).fetch()
return   len  (result)   ==   0


if   is_kb_empty(  "kb_stack_vector"  ):
print  (  "kb_stack_vector is empty - starting data insertion..."  )
server.query(  """
INSERT INTO kb_stack_vector
SELECT * FROM pg_sample.stackoverflow_2m
USING
batch_size = 1000,
track_column = id
"""  ).fetch()
print  (  "Data insertion started for kb_stack_vector"  )
else  :
print  (  "kb_stack_vector is not empty - skipping data insertion"  )
```


```text
Data insertion started for kb_stack_vector
```


### **Loading Data into FAISS Knowledge Base**


```text
if   is_kb_empty(  "kb_stack_faiss"  ):
print  (  "kb_stack_faiss is empty - starting data insertion..."  )
server  .  query  (  """
INSERT INTO kb_stack_faiss
SELECT * FROM pg_sample.stackoverflow_2m
USING
batch_size = 1000,
track_column = id
"""  ).  fetch  ()
print  (  "Data insertion started for kb_stack_faiss"  )
else  :
print  (  "kb_stack_faiss is not empty - skipping data insertion"  )
```


```text
Data insertion started for kb_stack_faiss
```


Wait until the data insertion is complete.


## **7. Querying the Knowledge Bases**


Once data is loaded, you can perform semantic searches combined with metadata filtering.


### **Basic Semantic Search**


Search for content related to “8-bit music” (finds semantically similar content):


```text
import   time


# Semantic search on pgvector KB
start   =   time.time()
results_vector   =   server.query(  """
SELECT * FROM kb_stack_vector
WHERE content = '8-bit music'
AND Tags LIKE '%python%'
LIMIT 10
"""  ).fetch()
elapsed_vector   =   time.time()   -   start
print  (  f  "pgvector query time:   {  elapsed_vector  :.2f  }   seconds"  )
display(results_vector)


# Semantic search on FAISS KB
start   =   time.time()
results_faiss   =   server.query(  """
SELECT * FROM kb_stack_faiss
WHERE content = '8-bit music'
AND Tags LIKE '%python%'
LIMIT 10
"""  ).fetch()
elapsed_faiss   =   time.time()   -   start
print  (  f  "FAISS query time:   {  elapsed_faiss  :.2f  }   seconds"  )
display(results_faiss)
```


```text
pgvector query time: 19.21 seconds
```


id chunk_id chunk_content distance relevance ContentLicense ViewCount LastEditDate Score AcceptedAnswerId OwnerUserId LastActivityDate Tags LastEditorUserId PostTypeId ParentId Title FavoriteCount CreationDate metadata


0 1118266 1118266:Body:1of2:0to971 Im trying to engineer in python a way of trans… 0.605447 0.622879 CC BY-SA 2.5 1694.0 2009-07-13T08:32:20.797 0 NaN NaN 2010-03-17T15:16:17.060 python,audio 12855.0 1 None List of values to a sound file NaN 2009-07-13T08:27:25.393 {‘Tags’: ‘python,audio’, ‘Score’: 0, ‘Title’: …


1 974071 974071:Body:1of1:0to791 I have a mosquito problem in my house. This wo… 0.615257 0.619097 CC BY-SA 2.5 55695.0 2017-05-23T12:32:21.507 44 974291.0 51197.0 2020-02-12T22:24:39.977 python,audio,mp3,frequency -1.0 1 None Python library for playing fixed-frequency sound 0.0 2009-06-10T07:05:02.037 {‘Tags’: ‘python,audio,mp3,frequency’, ‘Score’…


2 1967040 1967040:Body:1of1:0to224 I am confused because there are a lot of progr… 0.626904 0.614665 CC BY-SA 2.5 6615.0 None 7 1968691.0 237934.0 2021-08-10T10:40:59.217 python,audio NaN 1 None How can i create a melody? Is there any sound-… 0.0 2009-12-27T21:04:34.243 {‘Tags’: ‘python,audio’, ‘Score’: 7, ‘Title’: …


3 1118266 1118266:Body:2of2:972to1430 The current solution I’m thinking of involves … 0.627442 0.614461 CC BY-SA 2.5 1694.0 2009-07-13T08:32:20.797 0 NaN NaN 2010-03-17T15:16:17.060 python,audio 12855.0 1 None List of values to a sound file NaN 2009-07-13T08:27:25.393 {‘Tags’: ‘python,audio’, ‘Score’: 0, ‘Title’: …


4 1344884 1344884:Body:1of1:0to327 I want to learn how to program a music applica… 0.643957 0.608289 CC BY-SA 2.5 2205.0 2017-05-23T12:11:22.607 7 1346272.0 164623.0 2022-04-14T09:12:07.197 python,perl,waveform -1.0 1 None Programming a Self Learning Music Maker 0.0 2009-08-28T03:28:03.937 {‘Tags’: ‘python,perl,waveform’, ‘Score’: 7, ‘…


5 2376505 2376505:Body:1of2:0to968 Write a function called listenToPicture that t… 0.645214 0.607824 CC BY-SA 2.5 3058.0 2010-03-04T02:28:26.703 0 NaN 285922.0 2010-03-06T05:27:48.017 python,image,audio 34397.0 1 None How do I loop through every 4th pixel in every… NaN 2010-03-04T02:26:22.603 {‘Tags’: ‘python,image,audio’, ‘Score’: 0, ‘Ti…


6 2226853 2226853:Body:1of1:0to877 I’m trying to write a program to display PCM d… 0.654162 0.604536 CC BY-SA 2.5 12425.0 None 7 2226907.0 210920.0 2015-07-25T11:16:16.747 python,audio,pcm NaN 1 None Interpreting WAV Data 0.0 2010-02-09T05:01:25.703 {‘Tags’: ‘python,audio,pcm’, ‘Score’: 7, ‘Titl…


7 1561104 1561104:Body:1of1:0to306 Is there a way to do this? Also, I need this t… 0.668074 0.599494 CC BY-SA 2.5 1303.0 2020-06-20T09:12:55.060 1 1561314.0 151377.0 2012-01-29T00:01:18.230 python,pygame,pitch -1.0 1 None Playing sounds with python and changing their … NaN 2009-10-13T15:44:54.267 {‘Tags’: ‘python,pygame,pitch’, ‘Score’: 1, ‘T…


8 1382998 1382998:Body:4of4:2649to3382 \`\`\`\\n¼ éíñ§ÐÌëÑ » ¼ ö ® © ’\\n0 1\\n2 10\\n3 10\\n… 0.670654 0.598568 CC BY-SA 3.0 12497.0 2011-06-09T06:00:51.243 18 1383721.0 6946.0 2015-06-04T17:13:43.323 python,unicode 6946.0 1 None latin-1 to ascii 0.0 2009-09-05T10:44:40.167 {‘Tags’: ‘python,unicode’, ‘Score’: 18, ‘Title…


9 1837686 1837686:Body:1of2:0to950 I wish to take a file encoded in UTF-8 that do… 0.675999 0.596659 CC BY-SA 3.0 3016.0 2011-10-15T13:17:24.520 2 NaN NaN 2011-10-15T13:17:24.520 python,c,utf-8,compression 12113.0 1 None Compressing UTF-8(or other 8-bit encoding) to … 0.0 2009-12-03T04:43:05.963 {‘Tags’: ‘python,c,utf-8,compression’, ‘Score’…


```text
FAISS query time: 5.04 seconds
```


id distance chunk_id chunk_content relevance ContentLicense ViewCount LastEditDate Score AcceptedAnswerId OwnerUserId ParentId LastEditorUserId LastActivityDate Tags PostTypeId FavoriteCount Title CreationDate metadata


0 1118266 0.605468 1118266:Body:1of2:0to971 Im trying to engineer in python a way of trans… 0.622871 CC BY-SA 2.5 1694.0 2009-07-13T08:32:20.797 0 NaN NaN None 12855.0 2010-03-17T15:16:17.060 python,audio 1 NaN List of values to a sound file 2009-07-13T08:27:25.393 {‘ContentLicense’: ‘CC BY-SA 2.5’, ‘LastActivi…


1 974071 0.615225 974071:Body:1of1:0to791 I have a mosquito problem in my house. This wo… 0.619109 CC BY-SA 2.5 55695.0 2017-05-23T12:32:21.507 44 974291.0 51197.0 None -1.0 2020-02-12T22:24:39.977 python,audio,mp3,frequency 1 0.0 Python library for playing fixed-frequency sound 2009-06-10T07:05:02.037 {‘ContentLicense’: ‘CC BY-SA 2.5’, ‘LastActivi…


2 1967040 0.626923 1967040:Body:1of1:0to224 I am confused because there are a lot of progr… 0.614657 CC BY-SA 2.5 6615.0 None 7 1968691.0 237934.0 None NaN 2021-08-10T10:40:59.217 python,audio 1 0.0 How can i create a melody? Is there any sound-… 2009-12-27T21:04:34.243 {‘ContentLicense’: ‘CC BY-SA 2.5’, ‘LastActivi…


3 1118266 0.627461 1118266:Body:2of2:972to1430 The current solution I’m thinking of involves … 0.614454 CC BY-SA 2.5 1694.0 2009-07-13T08:32:20.797 0 NaN NaN None 12855.0 2010-03-17T15:16:17.060 python,audio 1 NaN List of values to a sound file 2009-07-13T08:27:25.393 {‘ContentLicense’: ‘CC BY-SA 2.5’, ‘LastActivi…


4 1344884 0.643955 1344884:Body:1of1:0to327 I want to learn how to program a music applica… 0.608289 CC BY-SA 2.5 2205.0 2017-05-23T12:11:22.607 7 1346272.0 164623.0 None -1.0 2022-04-14T09:12:07.197 python,perl,waveform 1 0.0 Programming a Self Learning Music Maker 2009-08-28T03:28:03.937 {‘ContentLicense’: ‘CC BY-SA 2.5’, ‘LastActivi…


5 2376505 0.645192 2376505:Body:1of2:0to968 Write a function called listenToPicture that t… 0.607832 CC BY-SA 2.5 3058.0 2010-03-04T02:28:26.703 0 NaN 285922.0 None 34397.0 2010-03-06T05:27:48.017 python,image,audio 1 NaN How do I loop through every 4th pixel in every… 2010-03-04T02:26:22.603 {‘ContentLicense’: ‘CC BY-SA 2.5’, ‘LastActivi…


6 2226853 0.654112 2226853:Body:1of1:0to877 I’m trying to write a program to display PCM d… 0.604554 CC BY-SA 2.5 12425.0 None 7 2226907.0 210920.0 None NaN 2015-07-25T11:16:16.747 python,audio,pcm 1 0.0 Interpreting WAV Data 2010-02-09T05:01:25.703 {‘ContentLicense’: ‘CC BY-SA 2.5’, ‘LastActivi…


7 1561104 0.668055 1561104:Body:1of1:0to306 Is there a way to do this? Also, I need this t… 0.599501 CC BY-SA 2.5 1303.0 2020-06-20T09:12:55.060 1 1561314.0 151377.0 None -1.0 2012-01-29T00:01:18.230 python,pygame,pitch 1 NaN Playing sounds with python and changing their … 2009-10-13T15:44:54.267 {‘ContentLicense’: ‘CC BY-SA 2.5’, ‘LastActivi…


8 1382998 0.670668 1382998:Body:4of4:2649to3382 \`\`\`\\n¼ éíñ§ÐÌëÑ » ¼ ö ® © ’\\n0 1\\n2 10\\n3 10\\n… 0.598563 CC BY-SA 3.0 12497.0 2011-06-09T06:00:51.243 18 1383721.0 6946.0 None 6946.0 2015-06-04T17:13:43.323 python,unicode 1 0.0 latin-1 to ascii 2009-09-05T10:44:40.167 {‘ContentLicense’: ‘CC BY-SA 3.0’, ‘LastActivi…


9 1837686 0.675986 1837686:Body:1of2:0to950 I wish to take a file encoded in UTF-8 that do… 0.596664 CC BY-SA 3.0 3016.0 2011-10-15T13:17:24.520 2 NaN NaN None 12113.0 2011-10-15T13:17:24.520 python,c,utf-8,compression 1 0.0 Compressing UTF-8(or other 8-bit encoding) to … 2009-12-03T04:43:05.963 {‘ContentLicense’: ‘CC BY-SA 3.0’, ‘LastActivi…


## **Analyzing the Results**


Notice how the search for “8-bit music” returned posts about:


-


Converting values to sound files


-


Playing fixed-frequency sounds


-


Creating melodies programmatically


None of these posts contain the exact phrase “8-bit music,” yet they’re all semantically relevant to chiptune/retro audio generation. This is the power of semantic search.


Also note the **4x speed improvement** with FAISS (5 seconds vs 19 seconds for pgvector). For production systems with high query volumes, this difference is significant.


### **Combined Semantic and Metadata Filtering**


Find AJAX-related posts tagged with jQuery that have high view counts:


```text
# pgvector: Semantic search   with   metadata filters
start   =   time  .  time  ()
results   =   server  .  query  (  """
SELECT * FROM kb_stack_vector
WHERE content = 'ajax'
AND Tags LIKE '%jquery%'
AND ViewCount > 1000.0
AND relevance > 0.6
LIMIT 10
"""  ).  fetch  ()
print  (f  "pgvector query time: {time.time() - start:.2f} seconds"  )
display(results)


# FAISS: Semantic search   with   metadata filters
start   =   time  .  time  ()
results   =   server  .  query  (  """
SELECT * FROM kb_stack_faiss
WHERE content = 'ajax'
AND Tags LIKE '%jquery%'
AND ViewCount > 1000.0
AND relevance > 0.6
LIMIT 10
"""  ).  fetch  ()
print  (f  "FAISS query time: {time.time() - start:.2f} seconds"  )
display(results)
```


```text
pgvector query time: 5.76 seconds
```


0 1400637 1400637:Body:28of32:25627to26627 o.ajax({type:“POST”,url:E,data:G,success:H,dat… 0.427265 0.700641 CC BY-SA 2.5 2741.0 2009-09-09T16:16:59.430 2 1400656.0 107129.0 2013-08-05T16:07:54.400 javascript,jquery 8590.0 1 None Stop reload for ajax submitted form NaN 2009-09-09T16:12:46.057 {‘Tags’: ‘javascript,jquery’, ‘Score’: 2, ‘Tit…


1 1400637 1400637:Body:30of32:27488to28356 O=false;T.onload=T.onreadystatechange=function… 0.453764 0.687870 CC BY-SA 2.5 2741.0 2009-09-09T16:16:59.430 2 1400656.0 107129.0 2013-08-05T16:07:54.400 javascript,jquery 8590.0 1 None Stop reload for ajax submitted form NaN 2009-09-09T16:12:46.057 {‘Tags’: ‘javascript,jquery’, ‘Score’: 2, ‘Tit…


2 1400637 1400637:Body:27of32:24691to25626 rn this},serialize:function(){return o.param(t… 0.454629 0.687460 CC BY-SA 2.5 2741.0 2009-09-09T16:16:59.430 2 1400656.0 107129.0 2013-08-05T16:07:54.400 javascript,jquery 8590.0 1 None Stop reload for ajax submitted form NaN 2009-09-09T16:12:46.057 {‘Tags’: ‘javascript,jquery’, ‘Score’: 2, ‘Tit…


3 1424774 1424774:Body:2of2:934to1745 var self = this;\\n $.ajax({\\n … 0.461486 0.684235 CC BY-SA 2.5 3601.0 None 1 1426940.0 173350.0 2020-06-08T10:43:45.037 jquery,loops NaN 1 None Loop with 8 times NaN 2009-09-15T02:02:58.927 {‘Tags’: ‘jquery,loops’, ‘Score’: 1, ‘Title’: …


4 1400637 1400637:Body:31of32:28357to29238 N=function(X){if(J.readyState==0){if(P){clearI… 0.462191 0.683905 CC BY-SA 2.5 2741.0 2009-09-09T16:16:59.430 2 1400656.0 107129.0 2013-08-05T16:07:54.400 javascript,jquery 8590.0 1 None Stop reload for ajax submitted form NaN 2009-09-09T16:12:46.057 {‘Tags’: ‘javascript,jquery’, ‘Score’: 2, ‘Tit…


5 546344 546344:Body:2of3:902to1764 var before = function() { $(loading).show() ;… 0.463258 0.683407 CC BY-SA 2.5 1463.0 2009-02-13T16:17:38.170 0 546642.0 2755.0 2009-02-13T16:37:59.867 javascript,jquery,ajax 2755.0 1 None Using jQuery, how can I store the result of a … 0.0 2009-02-13T15:25:00.963 {‘Tags’: ‘javascript,jquery,ajax’, ‘Score’: 0,…


6 1279625 1279625:Body:2of3:782to1754 \`\`\`\\n<!DOCTYPE html PUBLIC ”-//W3C//DTD XHTML … 0.468882 0.680790 CC BY-SA 3.0 1130.0 2016-12-03T07:00:58.213 0 1279881.0 58375.0 2016-12-03T07:00:58.213 events,jquery,getjson 6637668.0 1 None Trouble with jQuery Ajax timing NaN 2009-08-14T19:06:28.043 {‘Tags’: ‘events,jquery,getjson’, ‘Score’: 0, …


7 1400637 1400637:Body:32of32:29239to30048 L(){if(M.complete){M.complete(J,R)}if(M.global… 0.468944 0.680761 CC BY-SA 2.5 2741.0 2009-09-09T16:16:59.430 2 1400656.0 107129.0 2013-08-05T16:07:54.400 javascript,jquery 8590.0 1 None Stop reload for ajax submitted form NaN 2009-09-09T16:12:46.057 {‘Tags’: ‘javascript,jquery’, ‘Score’: 2, ‘Tit…


8 1775625 1775625:Body:5of9:3144to4049 }\\n\\n}\\n\\n\\n\\n\\n<script type=… 0.472723 0.679014 CC BY-SA 2.5 2100.0 2009-11-21T14:46:00.250 1 1776406.0 212889.0 2009-11-21T19:03:52.070 jquery,form-submit 212889.0 1 None jQuery - Multiple form submission trigger unre… 0.0 2009-11-21T14:32:41.383 {‘Tags’: ‘jquery,form-submit’, ‘Score’: 1, ‘Ti…


9 1400637 1400637:Body:26of32:23690to24690 nclick”)}o(function(){var L=document.createEle… 0.477784 0.676689 CC BY-SA 2.5 2741.0 2009-09-09T16:16:59.430 2 1400656.0 107129.0 2013-08-05T16:07:54.400 javascript,jquery 8590.0 1 None Stop reload for ajax submitted form NaN 2009-09-09T16:12:46.057 {‘Tags’: ‘javascript,jquery’, ‘Score’: 2, ‘Tit…


```text
FAISS query time: 2.50 seconds
```


0 1400637 0.427243 1400637:Body:28of32:25627to26627 o.ajax({type:“POST”,url:E,data:G,success:H,dat… 0.700651 CC BY-SA 2.5 2741.0 2009-09-09T16:16:59.430 2 1400656.0 107129.0 None 8590.0 2013-08-05T16:07:54.400 javascript,jquery 1 NaN Stop reload for ajax submitted form 2009-09-09T16:12:46.057 {‘ContentLicense’: ‘CC BY-SA 2.5’, ‘LastActivi…


1 1400637 0.453769 1400637:Body:30of32:27488to28356 O=false;T.onload=T.onreadystatechange=function… 0.687867 CC BY-SA 2.5 2741.0 2009-09-09T16:16:59.430 2 1400656.0 107129.0 None 8590.0 2013-08-05T16:07:54.400 javascript,jquery 1 NaN Stop reload for ajax submitted form 2009-09-09T16:12:46.057 {‘ContentLicense’: ‘CC BY-SA 2.5’, ‘LastActivi…


2 1400637 0.454589 1400637:Body:27of32:24691to25626 rn this},serialize:function(){return o.param(t… 0.687479 CC BY-SA 2.5 2741.0 2009-09-09T16:16:59.430 2 1400656.0 107129.0 None 8590.0 2013-08-05T16:07:54.400 javascript,jquery 1 NaN Stop reload for ajax submitted form 2009-09-09T16:12:46.057 {‘ContentLicense’: ‘CC BY-SA 2.5’, ‘LastActivi…


3 1424774 0.461469 1424774:Body:2of2:934to1745 var self = this;\\n $.ajax({\\n … 0.684243 CC BY-SA 2.5 3601.0 None 1 1426940.0 173350.0 None NaN 2020-06-08T10:43:45.037 jquery,loops 1 NaN Loop with 8 times 2009-09-15T02:02:58.927 {‘ContentLicense’: ‘CC BY-SA 2.5’, ‘LastActivi…


4 1400637 0.462233 1400637:Body:31of32:28357to29238 N=function(X){if(J.readyState==0){if(P){clearI… 0.683886 CC BY-SA 2.5 2741.0 2009-09-09T16:16:59.430 2 1400656.0 107129.0 None 8590.0 2013-08-05T16:07:54.400 javascript,jquery 1 NaN Stop reload for ajax submitted form 2009-09-09T16:12:46.057 {‘ContentLicense’: ‘CC BY-SA 2.5’, ‘LastActivi…


5 546344 0.463237 546344:Body:2of3:902to1764 var before = function() { $(loading).show() ;… 0.683416 CC BY-SA 2.5 1463.0 2009-02-13T16:17:38.170 0 546642.0 2755.0 None 2755.0 2009-02-13T16:37:59.867 javascript,jquery,ajax 1 0.0 Using jQuery, how can I store the result of a … 2009-02-13T15:25:00.963 {‘ContentLicense’: ‘CC BY-SA 2.5’, ‘LastActivi…


6 1279625 0.468854 1279625:Body:2of3:782to1754 \`\`\`\\n<!DOCTYPE html PUBLIC ”-//W3C//DTD XHTML … 0.680803 CC BY-SA 3.0 1130.0 2016-12-03T07:00:58.213 0 1279881.0 58375.0 None 6637668.0 2016-12-03T07:00:58.213 events,jquery,getjson 1 NaN Trouble with jQuery Ajax timing 2009-08-14T19:06:28.043 {‘ContentLicense’: ‘CC BY-SA 3.0’, ‘LastActivi…


7 1400637 0.468931 1400637:Body:32of32:29239to30048 L(){if(M.complete){M.complete(J,R)}if(M.global… 0.680767 CC BY-SA 2.5 2741.0 2009-09-09T16:16:59.430 2 1400656.0 107129.0 None 8590.0 2013-08-05T16:07:54.400 javascript,jquery 1 NaN Stop reload for ajax submitted form 2009-09-09T16:12:46.057 {‘ContentLicense’: ‘CC BY-SA 2.5’, ‘LastActivi…


8 1775625 0.472740 1775625:Body:5of9:3144to4049 }\\n\\n}\\n\\n\\n\\n\\n<script type=… 0.679007 CC BY-SA 2.5 2100.0 2009-11-21T14:46:00.250 1 1776406.0 212889.0 None 212889.0 2009-11-21T19:03:52.070 jquery,form-submit 1 0.0 jQuery - Multiple form submission trigger unre… 2009-11-21T14:32:41.383 {‘ContentLicense’: ‘CC BY-SA 2.5’, ‘LastActivi…


9 1400637 0.477785 1400637:Body:26of32:23690to24690 nclick”)}o(function(){var L=document.createEle… 0.676688 CC BY-SA 2.5 2741.0 2009-09-09T16:16:59.430 2 1400656.0 107129.0 None 8590.0 2013-08-05T16:07:54.400 javascript,jquery 1 NaN Stop reload for ajax submitted form 2009-09-09T16:12:46.057 {‘ContentLicense’: ‘CC BY-SA 2.5’, ‘LastActivi…


### **Understanding Query Results**


The query returns these columns:


Column Description


id Original document ID


chunk_id Identifier for the text chunk


chunk_content The actual text content


metadata JSON object with all metadata fields


distance Vector distance (lower = more similar)


relevance Relevance score (higher = more relevant, 0-1)


### **Filtering by Relevance**


Get only highly relevant results:


#### **The Power of Combined Filtering**


The query we just ran demonstrates MindsDB’s hybrid search capability:


```text
SELECT   *   FROM   kb_stack_faiss
WHERE   content   =   'ajax'                -- Semantic match
AND   Tags   LIKE   '%jquery%'          -- Metadata filter
AND   ViewCount   >   1000              -- Popularity threshold
AND   relevance   >   0  .  6               -- Quality threshold
```


This finds posts that:


1.


Are semantically similar to “ajax” (not just keyword matches)


2.


Are tagged with jQuery


3.


Have significant engagement (>1000 views)


4.


Meet a minimum relevance score


This combination is impossible with traditional search and would require complex custom code with raw vector databases.


```text
def   run_query_ignore_exists  (sql, success_msg  =  "Query executed successfully"  ):
"""Execute a query, silently ignoring 'already exists' errors."""
try  :
result   =   server.query(sql).fetch()
print  (success_msg)
return   result
except   RuntimeError   as   e:
return   None    # Silently ignore
# Create MindsDB Agent
run_query_ignore_exists(  """
drop agent stackoverflow_agent
"""  ,   "Dropped stackoverflow_agent"  )


run_query(  """
CREATE AGENT stackoverflow_agent
USING
model = {
"provider": "openai",
"model_name": "gpt-4.1"
},
data = {
"knowledge_bases": ["mindsdb.kb_stack_faiss"]
},
prompt_template = '
You are a helpful programming assistant.
mindsdb.kb_stack_faiss is a knowledge base that contains Stack Overflow questions and answers.
Use this knowledge to provide accurate, helpful responses to programming questions.
Include code examples when relevant.
You must base your answer on the Stack Overflow questions and answers extracted from mindsdb.kb_stack_faiss.
If you failed to get the results from mindsdb.kb_stack_faiss, answer I could not get the results from mindsdb.kb_stack_faiss.
Print the chunk ID for each question and answer you based your answer on.
IMPORTANT: Use a limit of 100 in your query to the knowledge base.
'
"""  ,   "Created stackoverflow_agent"  )
```


```text
Dropped stackoverflow_agent
Created stackoverflow_agent
```


```text
# Query the agent
start   =   time  .  time  ()
response   =   server  .  query  (  """
SELECT answer
FROM stackoverflow_agent
WHERE question = 'Compare JavaScript to TypeScript for building web services'
"""  ).  fetch  ()
print  (f  "Agent response time: {time.time() - start:.2f} seconds\n"  )
print  (response['answer'].iloc[0])
```


```text
Agent response time: 63.44 seconds


To compare JavaScript and TypeScript for building web services, let's look at insights from Stack Overflow posts (see chunk IDs for reference):


**JavaScript:**
- JavaScript is a dynamic, weakly typed, prototype-based language with first-class functions ([1253285:Body:1of1:0to384](https://stackoverflow.com/posts/1253285)).
- It is the default language for web development, both on the client (browser) and, with Node.js, on the server ([870980:Body:1of1:0to133](https://stackoverflow.com/posts/870980)).
- JavaScript is flexible and widely supported, but its lack of static typing can lead to runtime errors and makes large codebases harder to maintain.


**TypeScript:**
- While not directly mentioned in the top results, TypeScript is a superset of JavaScript that adds static typing and modern language features. It compiles to JavaScript, so it runs anywhere JavaScript does.
- TypeScript helps catch errors at compile time, improves code readability, and is especially beneficial for large projects or teams.


**Web Services:**
- JavaScript (with Node.js) is commonly used to build RESTful APIs and web services ([208051:Body:1of1:0to147](https://stackoverflow.com/posts/208051)).
- TypeScript is increasingly popular for the same purpose, as it provides all the benefits of JavaScript plus type safety and better tooling (e.g., autocompletion, refactoring).


**Summary Table:**


| Feature         | JavaScript                        | TypeScript                          |
|-----------------|----------------------------------|-------------------------------------|
| Typing          | Dynamic, weakly typed            | Static typing (optional)            |
| Tooling         | Good, but less type-aware        | Excellent (autocompletion, refactor)|
| Learning Curve  | Lower                            | Slightly higher (due to types)      |
| Error Checking  | Runtime                          | Compile-time + runtime              |
| Ecosystem       | Huge, universal                  | Same as JS, plus TS-specific tools  |
| Maintainability | Can be challenging in large code | Easier in large codebases           |


**Conclusion:**
- For small projects or rapid prototyping, JavaScript is sufficient and easy to start with.
- For larger projects, teams, or when maintainability and reliability are priorities, TypeScript is generally preferred.


References:
- [1253285:Body:1of1:0to384](https://stackoverflow.com/posts/1253285)
- [870980:Body:1of1:0to133](https://stackoverflow.com/posts/870980)
- [208051:Body:1of1:0to147](https://stackoverflow.com/posts/208051)


If you want more specific code examples or a deeper dive into either technology, let me know
```


## **Conclusion**


We’ve built a complete semantic search system that:


-


Processes 2 million Stack Overflow posts


-


Supports both pgvector and FAISS backends


-


Combines semantic search with metadata filtering


-


Powers an AI agent for natural language queries


### **Key Takeaways**


1.


FAISS is much faster than pgvector for pure search queries


2.


Metadata filtering lets you narrow results by tags, scores, dates


3.


Knowledge bases abstract complexity - no need to manage embeddings manually


4.


Agents can leverage knowledge bases for RAG-style applications


### **Next Steps**


-


Try different embedding models


-


Add more data sources


-


Build a chat interface


-


Explore different chunking strategies


You can watch the recording of this demo by[registering here to receive the link.](https://mindshub.ai/events/webinar-ai-knowledge-bases-at-scale)
