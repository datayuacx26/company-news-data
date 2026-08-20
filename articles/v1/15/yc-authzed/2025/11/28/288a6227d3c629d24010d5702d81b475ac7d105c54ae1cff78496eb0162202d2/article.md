---
schema_version: "1.0.0"
document_id: "288a6227d3c629d24010d5702d81b475ac7d105c54ae1cff78496eb0162202d2"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/building-a-multi-tenant-rag-with-fine-grain-authorization-using-motia-and-spicedb"
published_at: "2025-11-18T17:30:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:55:16.082008+00:00"
content_hash: "sha256:75ced0b72f21f38d0cf28bf0daeee38815cef4e4c5a350d742c8cbf9c3169bec"
---

# Build a Multi-Tenant RAG with Fine-Grain Authorization using Motia and SpiceDB

> Learn how to build a complete retrieval-augmented generation pipeline with multi-tenant authorization using Motia's event-driven framework, OpenAI embeddings, Pinecone vector search, SpiceDB permissions, and natural language querying.


If I was hard-pressed to pick my favourite computer game of all time, I'd go with Stardew Valley (sorry,[Dangerous Dave](https://en.wikipedia.org/wiki/Dangerous_Dave) ). The stats from my Nintendo Profile is all the proof you need:


Stardew Valley sits atop with 430 hours played and in second place is Mario Kart (not pictured) with ~45 hours played. That's a significant difference, and should indicate how much I adore this game.


We've been talking about the importance of Fine-Grained Authorization and RAG recently, so when I sat down to build a sample usecase for a production-grade RAG with Fine-Grained Permissions, my immediate thought went to Stardew Valley.


For those not familiar, Stardew Valley is a farm life simulation game where players manage a farm by clearing land, growing seasonal crops, and raising animals. So I thought I could build a logbook for a large farm that one could query using natural language processing. This usecase is ideal for RAG Pipelines (a technique that uses external data to improve the accuracy, relevancy, and usefulness of a LLM model’s output).


I focused on building something that was as close to production-grade as possible (and perhaps strayed from the original intent of a single farm) where an organization can own farms and data from the farms. The farms contain harvest data and users can log and query data for the farms they're part of. This provides a sticky situation for the authorization model. How does a LLM know who has access to what data?


Here's where SpiceDB and ReBAC was vital. By using metadata to indicate where the relevant embedings came from, the RAG system returned harvest data to the user only based on what data they had access to. In fact,[OpenAI uses SpiceDB](https://authzed.com/customers/openai) for their fine-grained authorization in ChatGPT Connectors using similar techniques.


While I know my way around SpiceDB and authorization, I needed help to build out the other components for a production-grade harvest logbook. So I reached out to my friend[Rohit Ghumare](https://www.linkedin.com/in/rohit-ghumare/) from Motia for his expertise.[Motia.dev](https://motia.dev/) is a backend framework that unifies APIs, background jobs, workflows, and AI Agents into a single core primitive with built-in observability and state management


Here's a photo of Rohit and myself at Kubecon Europe in 2025


What follows below is a tutorial-style post on building a Retrieval Augmented Generation system with fine-grained authorization using the Motia framework and SpiceDB. We'll use[Pinecone](https://www.pinecone.io/) as our vector database, and OpenAI as our LLM.


## What You'll Build


In this tutorial, you'll create a complete RAG system with authorization that:


- Stores harvest data and automatically generates embeddings for semantic search
- Splits text into optimized chunks with overlap for better retrieval accuracy
- Implements fine-grained authorization using SpiceDB's relationship-based access control
- Queries harvest history using natural language with AI-powered responses
- Returns contextually relevant answers with source citations from vector search
- Supports multi-tenant access where users only see data they have permission to access
- Logs all queries and responses for audit trails in CSV or Google Sheets
- Runs as an event-driven workflow orchestrated through Motia's framework


By the end of the tutorial, you'll have a complete system that combines semantic search with multi-tenant authorization.


## Prerequisites


Before starting the tutorial, ensure you have:


- [OpenAI API key](https://platform.openai.com/api-keys) for embeddings and chat
- [Pinecone account](https://app.pinecone.io/) with an index created (1536 dimensions, cosine metric)
- [Docker](https://docs.docker.com/get-docker/) installed for running SpiceDB locally


## Getting Started


### 1. Create Your Motia Project


Create a new Motia project using the CLI:


bash


1


```text
npx motia@latest create


```


bash


1


```text
npx motia@latest create


```


The installer will prompt you:


1. **Template:** Select` Base (TypeScript)`
2. **Project name:** Enter` harvest-logbook-rag`
3. **Proceed?** Type` Yes`


Navigate into your project:


bash


1


```text
cd   harvest-logbook-rag


```


bash


1


```text
cd   harvest-logbook-rag


```


Your initial project structure:


1


2


3


4


5


6


7


8


```text
harvest-logbook-rag/
├── src/
│   └── services/
│       └── pet-store/
├── steps/
│   └── petstore/
├── .env
└── package.json


```


1


2


3


4


5


6


7


8


```text
harvest-logbook-rag/
├── src/
│   └── services/
│       └── pet-store/
├── steps/
│   └── petstore/
├── .env
└── package.json


```


The default template includes a pet store example. We'll replace this with our harvest logbook system. For more on Motia basics, see the[Quick Start guide](https://www.motia.dev/docs/getting-started/quick-start) .


### 2. Install Dependencies


Install the SpiceDB client for authorization:


bash


1


```text
npm install @authzed/authzed-node


```


bash


1


```text
npm install @authzed/authzed-node


```


This is the only additional package needed.


### 3. Setup Pinecone


Pinecone will store the vector embeddings for semantic search.


#### Create a Pinecone Account


1. Go to[app.pinecone.io](https://app.pinecone.io/) and sign up
2. Create a new project


#### Create an Index


1.


Click **Create Index**


2.


Configure:


- **Name:**` harvest-logbook` (or your preference)
- **Dimensions:**` 1536` (for OpenAI embeddings)
- **Metric:**` cosine`


3.


Click **Create Index**


#### Get Your Credentials


1. Go to **API Keys** in the sidebar
2. Copy your **API Key**
3. Go back to your index
4. Click the **Connect** tab
5. Copy the **Host** (looks like:` your-index-abc123.svc.us-east-1.pinecone.io` )


Save these for the next step.


### 4. Setup SpiceDB


SpiceDB handles authorization and access control for the system.


#### Start SpiceDB with Docker


Run this command to start SpiceDB locally:


bash


1


2


3


4


5


```text
docker run -d \
--name spicedb \
-p 50051:50051 \
authzed/spicedb serve \
--grpc-preshared-key  "sometoken"


```


bash


1


2


3


4


5


```text
docker run -d \
--name spicedb \
-p 50051:50051 \
authzed/spicedb serve \
--grpc-preshared-key  "sometoken"


```


#### Verify SpiceDB is Running


Check that the container is running:


bash


1


```text
docker ps | grep spicedb


```


bash


1


```text
docker ps | grep spicedb


```


You should see output similar to:


1


```text
6316f6cb50b4   authzed/spicedb   "spicedb serve --grp…"   31 seconds ago   Up 31 seconds   0.0.0.0:50051->50051/tcp   spicedb


```


1


```text


```


SpiceDB is now running on` localhost:50051` and ready to handle authorization checks.


### 5. Configure Environment Variables


Create a` .env` file in the project root:


bash


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


```text
# OpenAI (Required for embeddings and chat)
OPENAI_API_KEY=sk-proj-xxxxxxxxxxxxx


# Pinecone (Required for vector storage)
PINECONE_API_KEY=pcsk_xxxxxxxxxxxxx
PINECONE_INDEX_HOST=your-index-abc123.svc.us-east-1.pinecone.io


# SpiceDB (Required for authorization)
SPICEDB_ENDPOINT=localhost:50051
SPICEDB_TOKEN=sometoken


# LLM Configuration (OpenAI is default)
USE_OPENAI_CHAT= true


# Logging Configuration (CSV is default)
USE_CSV_LOGGER= true


```


bash


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


```text
# OpenAI (Required for embeddings and chat)
OPENAI_API_KEY=sk-proj-xxxxxxxxxxxxx


# Pinecone (Required for vector storage)
PINECONE_API_KEY=pcsk_xxxxxxxxxxxxx
PINECONE_INDEX_HOST=your-index-abc123.svc.us-east-1.pinecone.io


# SpiceDB (Required for authorization)
SPICEDB_ENDPOINT=localhost:50051
SPICEDB_TOKEN=sometoken


# LLM Configuration (OpenAI is default)
USE_OPENAI_CHAT= true


# Logging Configuration (CSV is default)
USE_CSV_LOGGER= true


```


Replace the placeholder values with your actual credentials from the previous steps.


### 6. Initialize SpiceDB Schema


SpiceDB needs a schema that defines the authorization model for organizations, farms, and users.


#### Create the Schema File


Create` src/services/harvest-logbook/spicedb.schema` with the authorization model.[A SpiceDB schema](https://authzed.com/docs/spicedb/concepts/schema) defines the types of objects found your application, how those objects can relate to one another, and the permissions that can be computed off of those relations.


Here's a snippet of the schema that defines` user` ,` organization` and` farm` and the relations and permissions between them.


zed


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


17


18


19


20


21


22


23


```text
definition    user    {  }


definition    organization    {
relation    admin  :  user
relation    member  :  user


permission   view  =   admin  +   member
permission   edit  =   admin  +   member
permission   query  =   admin  +   member
permission   manage  =   admin
}


definition    farm    {
relation    organization  :  organization
relation    owner  :  user
relation    editor  :  user
relation    viewer  :  user


permission   view  =   viewer  +   editor  +   owner  +   organization -  >view
permission   edit  =   editor  +   owner  +   organization -  >edit
permission   query  =   viewer  +   editor  +   owner  +   organization -  >query
permission   manage  =   owner  +   organization -  >admin
}


```


zed


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


17


18


19


20


21


22


23


```text
definition    user    {  }


definition    organization    {
relation    admin  :  user
relation    member  :  user


permission   view  =   admin  +   member
permission   edit  =   admin  +   member
permission   query  =   admin  +   member
permission   manage  =   admin
}


definition    farm    {
relation    organization  :  organization
relation    owner  :  user
relation    editor  :  user
relation    viewer  :  user


permission   view  =   viewer  +   editor  +   owner  +   organization -  >view
permission   edit  =   editor  +   owner  +   organization -  >edit
permission   manage  =   owner  +   organization -  >admin
}


```


[View the complete schema on GitHub](https://github.com/MotiaDev/motia-examples/blob/main/examples/harvest-logbook-rag/src/services/harvest-logbook/spicedb.schema)


The schema establishes:


- Organizations with admins and members
- Farms with owners, editors, and viewers
- Harvest entries linked to farms
- Permission inheritance (org members can access farms in their org)


#### Create Setup Scripts


Create a` scripts/` folder and add three files:


**` scripts/setup-spicedb-schema.ts`** - Reads the schema file and writes it to SpiceDB
[View on GitHub](https://github.com/MotiaDev/motia-examples/blob/main/examples/harvest-logbook-rag/scripts/setup-spicedb-schema.ts)


**` scripts/verify-spicedb-schema.ts`** - Verifies the schema was written correctly
[View on GitHub](https://github.com/MotiaDev/motia-examples/blob/main/examples/harvest-logbook-rag/scripts/verify-spicedb-schema.ts)


**` scripts/create-sample-permissions.ts`** - Creates sample users and permissions for testing
[View on GitHub](https://github.com/Taofiqq/motia-examples/blob/main/examples/harvest-logbook-rag/scripts/create-sample-permissions.ts)


#### Install Script Runner


bash


1


```text
npm install -D tsx


```


bash


1


```text
npm install -D tsx


```


#### Add Scripts to package.json


json


1


2


3


4


5


```text
"scripts"  :    {
"spicedb:setup"  :    "tsx scripts/setup-spicedb-schema.ts"  ,
"spicedb:verify"  :    "tsx scripts/verify-spicedb-schema.ts"  ,
"spicedb:sample"  :    "tsx scripts/create-sample-permissions.ts"
}


```


json


1


2


3


4


5


```text
"scripts"  :    {
"spicedb:setup"  :    "tsx scripts/setup-spicedb-schema.ts"  ,
"spicedb:verify"  :    "tsx scripts/verify-spicedb-schema.ts"  ,
"spicedb:sample"  :    "tsx scripts/create-sample-permissions.ts"
}


```


#### Run the Setup


bash


1


2


```text
# Write schema to SpiceDB
npm run spicedb:setup


```


bash


1


2


```text
# Write schema to SpiceDB
npm run spicedb:setup


```


You should see output confirming the schema was written successfully:


**Verify it was written correctly** :


bash!


1


```text
npm run spicedb:verify


```


bash!


1


```text
npm run spicedb:verify


```


This displays the complete authorization schema showing all definitions and permissions:


The output shows:


- **farm** definition with owner/editor/viewer roles
- **harvest_entry** definition linked to farms
- **organization** definition with admin/member roles
- **query_session** definition for RAG queries
- Permission rules for each resource type


**Create sample user (user_alice as owner of farm_1):**


bash!


1


```text
npm run spicedb:sample


```


bash!


1


```text
npm run spicedb:sample


```


This creates` user_alice` as owner of` farm_1` , ready for testing.


Your authorization system is now ready.


### 7. Start Development Server


Start the Motia development server:


bash


1


```text
npm run dev


```


bash


1


```text
npm run dev


```


The server starts at` http://localhost:3000` . Open this URL in your browser to see the Motia Workbench.


You'll see the default pet store example. We'll replace this with our harvest logbook system in the next sections.


Your development environment is now ready. All services are connected:


- Motia running on` localhost:3000`
- Pinecone index created and connected
- SpiceDB running with schema loaded
- Sample permissions created (` user_alice` owns` farm_1` )


## Exploring the Project


Before we start building, let's understand the architecture we're creating.


### System Architecture


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


17


18


19


20


21


22


23


24


```text
┌─────────────────────────────────────────────────────────────┐
│  POST /harvest_logbook                                      │
│  (Store harvest data + optional query)                      │
└─────────┬───────────────────────────────────────────────────┘
│
├─→ Authorization Middleware (SpiceDB)
│   - Check user has 'edit' permission on farm
│
├─→ ReceiveHarvestData Step (API)
│   - Validate input
│   - Emit events
│
├─→ ProcessEmbeddings Step (Event)
│   - Split text into chunks (400 chars, 40 overlap)
│   - Generate embeddings (OpenAI)
│   - Store vectors (Pinecone)
│
└─→ QueryAgent Step (Event) [if query provided]
- Retrieve similar content (Pinecone)
- Generate response (OpenAI/HuggingFace)
- Emit logging event
│
└─→ LogToSheets Step (Event)
- Log query & response (CSV/Sheets)


```


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


17


18


19


20


21


22


23


24


```text
│  POST /harvest_logbook                                      │
│  (Store harvest data + optional query)                      │
│
├─→ Authorization Middleware (SpiceDB)
│   - Check user has 'edit' permission on farm
│
├─→ ReceiveHarvestData Step (API)
│   - Validate input
│   - Emit events
│
├─→ ProcessEmbeddings Step (Event)
│   - Split text into chunks (400 chars, 40 overlap)
│   - Generate embeddings (OpenAI)
│   - Store vectors (Pinecone)
│
└─→ QueryAgent Step (Event) [if query provided]
- Retrieve similar content (Pinecone)
- Generate response (OpenAI/HuggingFace)
- Emit logging event
│
└─→ LogToSheets Step (Event)
- Log query & response (CSV/Sheets)


```


### The RAG Pipeline


Our system processes harvest data through these stages:


1. **API Entry** - Receive harvest data via REST endpoint
2. **Text Chunking** - Split content into overlapping chunks (400 chars, 40 overlap)
3. **Embedding Generation** - Convert chunks to vectors using OpenAI
4. **Vector Storage** - Store embeddings in Pinecone for semantic search
5. **Query Processing** - Search vectors and generate AI responses
6. **Audit Logging** - Log all queries and responses


### Event-Driven Architecture


The system uses Motia's event-driven model:


- **API Steps** handle HTTP requests
- **Event Steps** process background tasks
- Steps communicate by emitting and subscribing to events
- Each step is independent and can be tested separately


### Authorization Layer


Every API request passes through SpiceDB authorization:


- Users have relationships with resources (owner, editor, viewer)
- Permissions are checked before processing requests
- Multi-tenant by design (users only access their farms)


### What We'll Build


We'll create five main steps:


1. **ReceiveHarvestData** - API endpoint to store harvest entries
2. **ProcessEmbeddings** - Event handler for generating and storing embeddings
3. **QueryAgent** - Event handler for AI-powered queries
4. **QueryOnly** - Separate API endpoint for querying without storing data
5. **LogToSheets** - Event handler for audit logging


Each component is a single file in the` steps/` directory. Motia automatically discovers and connects them based on the events they emit and subscribe to.


## Step 1: Create the Harvest Entry API


### What We're Building


In this step, we'll create an API endpoint that receives harvest log data and triggers the processing pipeline. This is the entry point that starts the entire RAG workflow.


### Why This Step Matters


Every workflow needs an entry point. In Motia, API steps serve as the gateway between external requests and your event-driven system. By using Motia's` api` step type, you get automatic HTTP routing, request validation, and event emission, all without writing boilerplate server code. When a farmer calls this endpoint with their harvest data, it validates the input, checks authorization, stores the entry, and emits events that trigger the embedding generation and optional query processing.


### Create the Step File


Create a new file at` steps/harvest-logbook/receive-harvest-data.step.ts` .


> The complete source code for all steps is available on GitHub. You can reference the working implementation at any time.


[View the complete Step 1 code on GitHub →](https://github.com/MotiaDev/motia-examples/blob/main/examples/harvest-logbook-rag/steps/harvest-logbook/receive-harvest-data.step.ts)


Now let's understand the key parts you'll be implementing:


### Input Validation


typescript


1


2


3


4


5


6


```text
const   bodySchema = z. object  ({
content  : z. string  (). min  ( 1  ,  "Content cannot be empty"  ),
farmId  : z. string  (). min  ( 1  ,  "Farm ID is required for authorization"  ),
metadata  : z. record  (z. any  ()). optional  (),
query  : z. string  (). optional  (),
});


```


typescript


1


2


3


4


5


6


```text
const   bodySchema = z. object  ({
content  : z. string  (). min  ( 1  ,  "Content cannot be empty"  ),
metadata  : z. record  (z. any  ()). optional  (),
query  : z. string  (). optional  (),
});


```


Zod validates that requests include the harvest content and farm ID. The` query` field is optional - if provided, the system will also answer a natural language question about the data after storing it.


### Step Configuration


typescript


1


2


3


4


5


6


7


8


9


```text
export    const    config  :  ApiRouteConfig   = {
type  :  "api"  ,
name  :  "ReceiveHarvestData"  ,
path  :  "/harvest_logbook"  ,
method  :  "POST"  ,
middleware  : [errorHandlerMiddleware, harvestEntryEditMiddleware],
emits  : [ "process-embeddings"  ,  "query-agent"  ],
bodySchema,
};


```


typescript


1


2


3


4


5


6


7


8


9


```text
export    const    config  :  ApiRouteConfig   = {
type  :  "api"  ,
name  :  "ReceiveHarvestData"  ,
path  :  "/harvest_logbook"  ,
method  :  "POST"  ,
middleware  : [errorHandlerMiddleware, harvestEntryEditMiddleware],
emits  : [ "process-embeddings"  ,  "query-agent"  ],
bodySchema,
};


```


- ` type: 'api'` makes this an HTTP endpoint
- ` middleware` runs authorization checks before the handler
- ` emits` declares this step triggers embedding processing and optional query events
- Motia handles all the routing automatically


### Authorization Check


typescript


1


```text
middleware  : [errorHandlerMiddleware, harvestEntryEditMiddleware];


```


typescript


1


```text
middleware  : [errorHandlerMiddleware, harvestEntryEditMiddleware];


```


The` harvestEntryEditMiddleware` checks SpiceDB to ensure the user has` edit` permission on the specified farm. If authorization fails, the request is rejected before reaching the handler. Authorization info is added to the request for use in the handler.


[View authorization middleware →](https://github.com/MotiaDev/motia-examples/blob/main/examples/harvest-logbook-rag/middlewares/authz.middleware.ts)


### Handler Logic


typescript


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


17


18


19


20


21


```text
export    const    handler  :  Handlers  [ "ReceiveHarvestData"  ] =  async   (
req,
{ emit, logger, state }
) => {
const   { content, farmId, metadata, query } = bodySchema. parse  (req. body  );
const   entryId =  `harvest- ${ Date  .now()}  `  ;


// Store entry data in state
await   state. set  ( "harvest-entries"  , entryId, {
content,
farmId,
metadata,
timestamp  :  new    Date  (). toISOString  (),
});


// Emit event to process embeddings
await    emit  ({
topic  :  "process-embeddings"  ,
data  : { entryId, content, metadata },
});
};


```


typescript


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


17


18


19


20


21


```text
export    const    handler  :  Handlers  [ "ReceiveHarvestData"  ] =  async   (
req,
{ emit, logger, state }
) => {
const   { content, farmId, metadata, query } = bodySchema. parse  (req. body  );
const   entryId =  `harvest- ${ Date  .now()}  `  ;


// Store entry data in state
await   state. set  ( "harvest-entries"  , entryId, {
content,
farmId,
metadata,
timestamp  :  new    Date  (). toISOString  (),
});


// Emit event to process embeddings
await    emit  ({
topic  :  "process-embeddings"  ,
data  : { entryId, content, metadata },
});
};


```


The handler generates a unique entry ID, stores the data in Motia's state management, and emits an event to trigger embedding processing. If a query was provided, it also emits a` query-agent` event.


### Event Emission


typescript


1


2


3


4


5


6


7


8


9


10


11


```text
await    emit  ({
topic  :  "process-embeddings"  ,
data  : { entryId, content,  metadata  : { ...metadata, farmId, userId } },
});


if   (query) {
await    emit  ({
topic  :  "query-agent"  ,
data  : { entryId, query },
});
}


```


typescript


1


2


3


4


5


6


7


8


9


10


11


```text
await    emit  ({
topic  :  "process-embeddings"  ,
data  : { entryId, content,  metadata  : { ...metadata, farmId, userId } },
});


if   (query) {
await    emit  ({
topic  :  "query-agent"  ,
data  : { entryId, query },
});
}


```


Events are how Motia steps communicate. The` process-embeddings` event triggers the next step to chunk the text and generate embeddings. If a query was provided, the` query-agent` event runs in parallel to answer the question using RAG.


This keeps the API response fast as it returns immediately while processing happens in the background.


### Test the Step


Open the Motia Workbench and test this endpoint:


1. Click on the` harvest-logbook` flow
2. Find` POST /harvest_logbook` in the sidebar
3. Click on it to open the request panel
4. Switch to the **Headers** tab and add:


json


1


2


3


```text
{
"x-user-id"  :    "user_alice"
}


```


json


1


2


3


```text
{
"x-user-id"  :    "user_alice"
}


```


1. Switch to the **Body** tab and add:


json


1


2


3


4


5


6


7


8


```text
{
"content"  :    "Harvested 500kg of tomatoes from field A. Weather was sunny."  ,
"farmId"  :    "farm_1"  ,
"metadata"  :    {
"field"  :    "A"  ,
"crop"  :    "tomatoes"
}
}


```


json


1


2


3


4


5


6


7


8


```text
{
"farmId"  :    "farm_1"  ,
"metadata"  :    {
"field"  :    "A"  ,
"crop"  :    "tomatoes"
}
}


```


1. Click **Send** button.


You should see a success response with the entry ID. The Workbench will show the workflow executing in real-time, with events flowing to the next steps.


## Step 2: Process Embeddings


### What We're Building


This event handler takes the harvest data from Step 1, splits it into chunks, generates vector embeddings, and stores them in Pinecone for semantic search.


### Why This Step Matters


RAG systems need to break down large text into smaller chunks for better retrieval accuracy. By chunking text with overlap and generating embeddings for each piece, we enable semantic search that finds relevant context even when queries don't match exact keywords.


This step runs in the background after the API returns, keeping the user experience fast while handling the background work of embedding generation and vector storage.


### Create the Step File


Create a new file at` steps/harvest-logbook/process-embeddings.step.ts` .


[View the complete Step 2 code on GitHub →](https://github.com/MotiaDev/motia-examples/blob/main/examples/harvest-logbook-rag/steps/harvest-logbook/process-embeddings.step.ts)


Now let's understand the key parts you'll be implementing:


### Input Schema


typescript


1


2


3


4


5


```text
const   inputSchema = z. object  ({
entryId  : z. string  (),
content  : z. string  (),
metadata  : z. record  (z. any  ()). optional  (),
});


```


typescript


1


2


3


4


5


```text
const   inputSchema = z. object  ({
entryId  : z. string  (),
content  : z. string  (),
metadata  : z. record  (z. any  ()). optional  (),
});


```


This step receives the entry ID, content, and metadata from the previous step's event emission.


### Step Configuration


typescript


1


2


3


4


5


6


7


```text
export    const    config  :  EventConfig   = {
type  :  "event"  ,
name  :  "ProcessEmbeddings"  ,
subscribes  : [ "process-embeddings"  ],
emits  : [],
input  : inputSchema,
};


```


typescript


1


2


3


4


5


6


7


```text
export    const    config  :  EventConfig   = {
type  :  "event"  ,
name  :  "ProcessEmbeddings"  ,
subscribes  : [ "process-embeddings"  ],
emits  : [],
input  : inputSchema,
};


```


- ` type: 'event'` makes this a background event handler
- ` subscribes: \['process-embeddings'\]` listens for events from Step 1
- No emits - this is the end of the embedding pipeline


### Text Chunking


typescript


1


2


3


4


5


6


```text
const   vectorIds =  await    HarvestLogbookService  . storeEntry  ({
id  : entryId,
content,
metadata,
timestamp  :  new    Date  (). toISOString  (),
});


```


typescript


1


2


3


4


5


6


```text
const   vectorIds =  await    HarvestLogbookService  . storeEntry  ({
id  : entryId,
content,
metadata,
timestamp  :  new    Date  (). toISOString  (),
});


```


The service handles text splitting (400 character chunks with 40 character overlap), embedding generation via OpenAI, and storage in Pinecone. This chunking strategy ensures semantic continuity across chunks.


[View text splitter service →](https://github.com/MotiaDev/motia-examples/blob/main/examples/harvest-logbook-rag/src/services/harvest-logbook/text-splitter.ts)


### Embedding Generation


The OpenAI service generates 1536-dimension embeddings for each text chunk using the` text-embedding-ada-002` model.


[View OpenAI service →](https://github.com/MotiaDev/motia-examples/blob/main/examples/harvest-logbook-rag/src/services/harvest-logbook/openai-service.ts)


### Vector Storage


typescript


1


2


3


4


5


```text
await   state. set  ( "harvest-vectors"  , entryId, {
vectorIds,
processedAt  :  new    Date  (). toISOString  (),
chunkCount  : vectorIds. length  ,
});


```


typescript


1


2


3


4


5


```text
await   state. set  ( "harvest-vectors"  , entryId, {
vectorIds,
processedAt  :  new    Date  (). toISOString  (),
chunkCount  : vectorIds. length  ,
});


```


After storing vectors in Pinecone, the step updates Motia's state with the vector IDs for tracking. Each chunk gets a unique ID like` harvest-123-chunk-0` ,` harvest-123-chunk-1` , etc.


[View Pinecone service →](https://github.com/MotiaDev/motia-examples/blob/main/examples/harvest-logbook-rag/src/services/harvest-logbook/pinecone-service.ts)


The embeddings are now stored and ready for semantic search when users query the system.


### Test the Step


Step 2 runs automatically when Step 1 emits the` process-embeddings` event. To test it:


1. Send a request to the` POST /harvest_logbook` endpoint (from Step 1)
2. In the Workbench, watch the workflow visualization
3. You'll see the` ProcessEmbeddings` step activate automatically
4. Check the **Logs** tab at the bottom to see:


- Text chunking progress
- Embedding generation
- Vector storage confirmation


The step completes when you see "Successfully stored embeddings" in the logs. The vectors are now in Pinecone and ready for semantic search.


## Step 3: Query Agent


### What We're Building


This event handler performs the RAG query, it searches Pinecone for relevant content, retrieves matching chunks, and uses an LLM to generate natural language responses based on the retrieved context.


### Why This Step Matters


This is where retrieval-augmented generation happens. Instead of the LLM generating responses from its training data alone, it uses actual harvest data from Pinecone as context. This ensures accurate, source-backed answers specific to the user's farm data.


The step supports both OpenAI and HuggingFace LLMs, giving you flexibility in choosing your AI provider based on cost and performance needs.


### Create the Step File


Create a new file at` steps/harvest-logbook/query-agent.step.ts` .


[View the complete Step 3 code on GitHub →](https://github.com/MotiaDev/motia-examples/blob/main/examples/harvest-logbook-rag/steps/harvest-logbook/query-agent.step.ts)


Now let's understand the key parts you'll be implementing:


### Input Schema


typescript


1


2


3


4


5


6


7


8


9


10


11


12


```text
const   inputSchema = z. object  ({
entryId  : z. string  (),
query  : z. string  (),
conversationHistory  : z
. array  (
z. object  ({
role  : z. enum  ([ "user"  ,  "assistant"  ,  "system"  ]),
content  : z. string  (),
})
)
. optional  (),
});


```


typescript


1


2


3


4


5


6


7


8


9


10


11


12


```text
const   inputSchema = z. object  ({
entryId  : z. string  (),
query  : z. string  (),
conversationHistory  : z
. array  (
z. object  ({
role  : z. enum  ([ "user"  ,  "assistant"  ,  "system"  ]),
content  : z. string  (),
})
)
. optional  (),
});


```


The step receives the query text and optional conversation history for multi-turn conversations.


### Step Configuration


typescript


1


2


3


4


5


6


7


```text
export    const    config  :  EventConfig   = {
type  :  "event"  ,
name  :  "QueryAgent"  ,
subscribes  : [ "query-agent"  ],
emits  : [ "log-to-sheets"  ],
input  : inputSchema,
};


```


typescript


1


2


3


4


5


6


7


```text
export    const    config  :  EventConfig   = {
type  :  "event"  ,
name  :  "QueryAgent"  ,
subscribes  : [ "query-agent"  ],
emits  : [ "log-to-sheets"  ],
input  : inputSchema,
};


```


- ` subscribes: \['query-agent'\]` listens for query events from Step 1
- ` emits: \['log-to-sheets'\]` triggers logging after generating response


### RAG Query Process


typescript


1


2


3


4


```text
const   agentResponse =  await    HarvestLogbookService  . queryWithAgent  ({
query,
conversationHistory,
});


```


typescript


1


2


3


4


```text
const   agentResponse =  await    HarvestLogbookService  . queryWithAgent  ({
query,
conversationHistory,
});


```


The service orchestrates the RAG pipeline: embedding the query, searching Pinecone for similar vectors, extracting context from top matches, and generating a response using the LLM.


[View RAG orchestration service →](https://github.com/MotiaDev/motia-examples/blob/main/examples/harvest-logbook-rag/src/services/harvest-logbook/index.ts)


### Vector Search


The query is embedded using OpenAI and searched against Pinecone to find the top 5 most similar chunks. Each result includes a similarity score and the original text.


[View Pinecone query implementation →](https://github.com/MotiaDev/motia-examples/blob/main/examples/harvest-logbook-rag/src/services/harvest-logbook/pinecone-service.ts)


### LLM Response Generation


typescript


1


2


3


4


5


6


```text
await   state. set  ( "agent-responses"  , entryId, {
query,
response  : agentResponse. response  ,
sources  : agentResponse. sources  ,
timestamp  : agentResponse. timestamp  ,
});


```


typescript


1


2


3


4


5


6


```text
await   state. set  ( "agent-responses"  , entryId, {
query,
response  : agentResponse. response  ,
sources  : agentResponse. sources  ,
timestamp  : agentResponse. timestamp  ,
});


```


The LLM generates a response using the retrieved context. The system supports both OpenAI (default) and HuggingFace, controlled by the` USE_OPENAI_CHAT` environment variable. The response includes source citations showing which harvest entries informed the answer.


[View OpenAI chat service →](https://github.com/MotiaDev/motia-examples/blob/main/examples/harvest-logbook-rag/src/services/harvest-logbook/openai-chat-service.ts)
[View HuggingFace service →](https://github.com/MotiaDev/motia-examples/blob/main/examples/harvest-logbook-rag/src/services/harvest-logbook/huggingface-service.ts)


### Event Emission


typescript


1


2


3


4


5


6


7


8


9


```text
await    emit  ({
topic  :  "log-to-sheets"  ,
data  : {
entryId,
query,
response  : agentResponse. response  ,
sources  : agentResponse. sources  ,
},
});


```


typescript


1


2


3


4


5


6


7


8


9


```text
await    emit  ({
topic  :  "log-to-sheets"  ,
data  : {
entryId,
query,
response  : agentResponse. response  ,
sources  : agentResponse. sources  ,
},
});


```


After generating the response, the step emits a logging event to create an audit trail of all queries and responses.


### Test the Step


Step 3 runs automatically when you include a` query` field in the Step 1 request. To test it:


1. Send a request to` POST /harvest_logbook` with a query:


json


1


2


3


4


5


```text
{
"farmId"  :    "farm_1"  ,
"query"  :    "What crops did we harvest?"
}


```


json


1


2


3


4


5


```text
{
"farmId"  :    "farm_1"  ,
"query"  :    "What crops did we harvest?"
}


```


1. In the Workbench, watch the` QueryAgent` step activate
2. Check the **Logs** tab to see:


- Query embedding generation
- Vector search in Pinecone
- LLM response generation
- Source citations


The step completes when you see the AI-generated response in the logs. The query and response are automatically logged by Step 5.


## Step 4: Query-Only Endpoint


### What We're Building


This API endpoint allows users to query their existing harvest data without storing new entries. It's a separate endpoint dedicated purely to RAG queries.


### Why This Step Matters


While Step 1 handles both storing and optionally querying data, users often need to just ask questions about their existing harvest logs. This dedicated endpoint keeps the API clean and focused - one endpoint for data entry, another for pure queries.


This separation also makes it easier to apply different rate limits or permissions between data modification and read-only operations.


### Create the Step File


Create a new file at` steps/harvest-logbook/query-only.step.ts` .


[View the complete Step 4 code on GitHub →](https://github.com/MotiaDev/motia-examples/blob/main/examples/harvest-logbook-rag/steps/harvest-logbook/query-only.step.ts)


Now let's understand the key parts you'll be implementing:


### Input Validation


typescript


1


2


3


4


5


6


7


8


9


10


11


12


```text
const   bodySchema = z. object  ({
query  : z. string  (). min  ( 1  ,  "Query cannot be empty"  ),
conversationHistory  : z
. array  (
z. object  ({
role  : z. enum  ([ "user"  ,  "assistant"  ,  "system"  ]),
content  : z. string  (),
})
)
. optional  (),
});


```


typescript


1


2


3


4


5


6


7


8


9


10


11


12


```text
const   bodySchema = z. object  ({
query  : z. string  (). min  ( 1  ,  "Query cannot be empty"  ),
conversationHistory  : z
. array  (
z. object  ({
role  : z. enum  ([ "user"  ,  "assistant"  ,  "system"  ]),
content  : z. string  (),
})
)
. optional  (),
});


```


The request requires a query and farm ID. Conversation history is optional for multi-turn conversations.


### Step Configuration


typescript


1


2


3


4


5


6


7


8


```text
export    const    config  :  ApiRouteConfig   = {
type  :  "api"  ,
name  :  "QueryHarvestLogbook"  ,
path  :  "/harvest_logbook/query"  ,
method  :  "POST"  ,
middleware  : [errorHandlerMiddleware, harvestQueryMiddleware],
emits  : [ "query-agent"  ],
};


```


typescript


1


2


3


4


5


6


7


8


```text
export    const    config  :  ApiRouteConfig   = {
type  :  "api"  ,
name  :  "QueryHarvestLogbook"  ,
path  :  "/harvest_logbook/query"  ,
method  :  "POST"  ,
middleware  : [errorHandlerMiddleware, harvestQueryMiddleware],
emits  : [ "query-agent"  ],
};


```


- ` path: '/harvest_logbook/query'` creates a dedicated query endpoint
- ` harvestQueryMiddleware` checks for` query` permission (not` edit` )
- ` emits: \['query-agent'\]` triggers the same RAG query handler as Step 3


### Authorization Middleware


typescript


1


```text
middleware  : [errorHandlerMiddleware, harvestQueryMiddleware];


```


typescript


1


```text
middleware  : [errorHandlerMiddleware, harvestQueryMiddleware];


```


The` harvestQueryMiddleware` checks SpiceDB for` query` permission. This is less restrictive than` edit` - viewers can query but cannot modify data.


### Handler Logic


typescript


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


17


```text
export    const    handler  :  Handlers  [ "QueryHarvestLogbook"  ] =  async   (
req,
{ emit, logger }
) => {
const   { query, farmId } = bodySchema. parse  (req. body  );
const   queryId =  `query- ${ Date  .now()}  `  ;


await    emit  ({
topic  :  "query-agent"  ,
data  : {  entryId  : queryId, query },
});


return   {
status  :  200  ,
body  : {  success  :  true  , queryId },
};
};


```


typescript


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


17


```text
export    const    handler  :  Handlers  [ "QueryHarvestLogbook"  ] =  async   (
req,
{ emit, logger }
) => {
const   { query, farmId } = bodySchema. parse  (req. body  );
const   queryId =  `query- ${ Date  .now()}  `  ;


await    emit  ({
topic  :  "query-agent"  ,
data  : {  entryId  : queryId, query },
});


return   {
status  :  200  ,
body  : {  success  :  true  , queryId },
};
};


```


The handler generates a unique query ID and emits the same` query-agent` event used in Step 1. This reuses the RAG pipeline from Step 3 without duplicating code.


The API returns immediately with the query ID. The actual processing happens in the background, and results are logged by Step 5.


### Test the Step


This is the dedicated query endpoint. Test it directly:


1. Click on` POST /harvest_logbook/query` in the Workbench
2. Add the header:


json


1


2


3


```text
{
"x-user-id"  :    "user_alice"
}


```


json


1


2


3


```text
{
"x-user-id"  :    "user_alice"
}


```


1. Add the body:


json


1


2


3


4


```text
{
"query"  :    "What crops did we harvest?"  ,
"farmId"  :    "farm_1"
}


```


json


1


2


3


4


```text
{
"query"  :    "What crops did we harvest?"  ,
"farmId"  :    "farm_1"
}


```


1. Click **Send**


You'll see a` 200 OK` response with the query ID. In the **Logs** tab, watch for:


- ` QueryHarvestLogbook` - Authorization and query received
- ` QueryAgent` - Querying AI agent
- ` QueryAgent` - Agent query completed


The query runs in the background and results are logged by Step 5. This endpoint is perfect for read-only query operations without storing new data.


## Step 5: Log to Sheets


### What We're Building


This event handler creates an audit trail by logging every query and its AI-generated response. It supports both local CSV files (for development) and Google Sheets (for production).


### Why This Step Matters


Audit logs are essential for understanding how users interact with your system. They help with debugging, monitoring usage patterns, and maintaining compliance. By logging queries and responses, you can track what questions users ask, identify common patterns, and improve the system over time.


The dual logging strategy (CSV/Google Sheets) gives you flexibility, use CSV locally for quick testing, then switch to Google Sheets for production without changing code.


### Create the Step File


Create a new file at` steps/harvest-logbook/log-to-sheets.step.ts` .


[View the complete Step 5 code on GitHub →](https://github.com/MotiaDev/motia-examples/blob/main/examples/harvest-logbook-rag/steps/harvest-logbook/log-to-sheets.step.ts)


Now let's understand the key parts you'll be implementing:


### Input Schema


typescript


1


2


3


4


5


6


```text
const   inputSchema = z. object  ({
entryId  : z. string  (),
query  : z. string  (),
response  : z. string  (),
sources  : z. array  (z. string  ()). optional  (),
});


```


typescript


1


2


3


4


5


6


```text
const   inputSchema = z. object  ({
entryId  : z. string  (),
query  : z. string  (),
response  : z. string  (),
sources  : z. array  (z. string  ()). optional  (),
});


```


The step receives the query, AI response, and optional source citations from Step 3.


### Step Configuration


typescript


1


2


3


4


5


6


7


```text
export    const    config  :  EventConfig   = {
type  :  "event"  ,
name  :  "LogToSheets"  ,
subscribes  : [ "log-to-sheets"  ],
emits  : [],
input  : inputSchema,
};


```


typescript


1


2


3


4


5


6


7


```text
export    const    config  :  EventConfig   = {
type  :  "event"  ,
name  :  "LogToSheets"  ,
subscribes  : [ "log-to-sheets"  ],
emits  : [],
input  : inputSchema,
};


```


- ` subscribes: \['log-to-sheets'\]` listens for logging events from Step 3
- No emits - this is the end of the workflow


### Logging Service Selection


typescript


1


2


3


4


```text
const   useCSV =
process. env  . USE_CSV_LOGGER   ===  "true"   || !process. env  . GOOGLE_SHEETS_ID  ;


await    HarvestLogbookService  . logToSheets  (query, response, sources);


```


typescript


1


2


3


4


```text
const   useCSV =


await    HarvestLogbookService  . logToSheets  (query, response, sources);


```


The service automatically chooses between CSV and Google Sheets based on environment variables. This keeps the step code simple while supporting different deployment scenarios.


[View CSV logger →](https://github.com/MotiaDev/motia-examples/blob/main/examples/harvest-logbook-rag/src/services/harvest-logbook/csv-logger.ts)
[View Google Sheets service →](https://github.com/MotiaDev/motia-examples/blob/main/examples/harvest-logbook-rag/src/services/harvest-logbook/sheets-service.ts)


### Error Handling


typescript


1


2


3


4


5


6


7


```text
try   {
await    HarvestLogbookService  . logToSheets  (query, response, sources);
logger. info  ( `Successfully logged to  ${destination}  `  );
}  catch   (error) {
logger. error  ( "Failed to log query response"  );
// Don't throw - logging failures shouldn't break the main flow
}


```


typescript


1


2


3


4


5


6


7


```text
try   {
await    HarvestLogbookService  . logToSheets  (query, response, sources);
logger. info  ( `Successfully logged to  ${destination}  `  );
}  catch   (error) {
logger. error  ( "Failed to log query response"  );
// Don't throw - logging failures shouldn't break the main flow
}


```


The step catches logging errors without throwing. This ensures that even if logging fails, the main workflow completes successfully. Users get their query results even if the audit log has issues.


### CSV Output Format


The CSV logger saves entries to` logs/harvest_logbook.csv` with these columns:


- Timestamp
- Query
- Response
- Sources (comma-separated)


Each entry is automatically escaped to handle quotes and commas in the content.


### Test the Step


Step 5 runs automatically after Step 3 completes. To verify it's working:


1. Run a query using` POST /harvest_logbook/query`
2. Check the **Logs** tab for` LogToSheets` entries
3. Verify the CSV file was created:


bash


1


```text
cat   logs/harvest_logbook.csv


```


bash


1


```text
cat   logs/harvest_logbook.csv


```


You should see your query and response logged with a timestamp. Each subsequent query appends a new row to the CSV file.


## Testing the System


Now that all steps are built, let's test the complete workflow using the Motia Workbench.


### Start the Server


bash


1


```text
npm run dev


```


bash


1


```text
npm run dev


```


Open` http://localhost:3000` in your browser to access the Workbench.


### Test 1: Store Harvest Data


1. Select the` harvest-logbook` flow from the dropdown
2. Find the` POST /harvest_logbook` endpoint in the workflow
3. Click on it to open the request panel
4. Add the authorization header:


json


1


2


3


```text
{
"x-user-id"  :    "user_alice"
}


```


json


1


2


3


```text
{
"x-user-id"  :    "user_alice"
}


```


1. Set the request body:


json


1


2


3


4


5


6


7


8


9


```text
{
"content"  :    "Harvested 500kg of tomatoes from field A. Weather was sunny, no pest damage observed."  ,
"farmId"  :    "farm_1"  ,
"metadata"  :    {
"field"  :    "A"  ,
"crop"  :    "tomatoes"  ,
"weight_kg"  :    500
}
}


```


json


1


2


3


4


5


6


7


8


9


```text
{
"farmId"  :    "farm_1"  ,
"metadata"  :    {
"field"  :    "A"  ,
"crop"  :    "tomatoes"  ,
"weight_kg"  :    500
}
}


```


1. Click **Play** Button


Watch the workflow execute in real-time. You'll see:


- Authorization check passes (user_alice has edit permission)
- Text chunked into embeddings
- Vectors stored in Pinecone
- Success response returned


### Test 2: Query the Data


1. Find the` POST /harvest_logbook/query` endpoint
2. Add the authorization header:


json


1


2


3


```text
{
"x-user-id"  :    "user_alice"
}


```


json


1


2


3


```text
{
"x-user-id"  :    "user_alice"
}


```


1. Set the request body:


json


1


2


3


4


```text
{
"farmId"  :    "farm_1"  ,
"query"  :    "What crops did we harvest recently?"
}


```


json


1


2


3


4


```text
{
"farmId"  :    "farm_1"  ,
"query"  :    "What crops did we harvest recently?"
}


```


1. Click **Send**


Watch the RAG pipeline execute:


- Query embedded via OpenAI
- Similar vectors retrieved from Pinecone
- AI generates response with context
- Query and response logged to CSV


### Test 3: Verify Authorization


Try querying as a user without permission:


1. Use the same query endpoint
2. Change the header:


json


1


2


3


```text
{
"x-user-id"  :    "user_unauthorized"
}


```


json


1


2


3


```text
{
"x-user-id"  :    "user_unauthorized"
}


```


1. Click **Send**


You'll see a 403 Forbidden response to verify if authorization works correctly. You can also create different users with different levels of access and see fine-grained authorization in action.


### View the Logs


Check the audit trail:


bash


1


```text
cat   logs/harvest_logbook.csv


```


bash


1


```text
cat   logs/harvest_logbook.csv


```


You'll see all queries and responses logged with timestamps.


The Workbench also provides trace visualization showing exactly how data flows through each step, making debugging straightforward.


## Conclusion


You've built a complete RAG system with multi-tenant authorization using Motia's event-driven framework. You learned how to:


1. Build event-driven workflows with Motia steps
2. Implement RAG with text chunking, embeddings, and vector search
3. Add fine-grained authorization using SpiceDB's relationship model
4. Handle async operations with event emission
5. Integrate multiple services (OpenAI, Pinecone, SpiceDB)


Your system now handles:


- Semantic search over harvest data with AI-powered embeddings
- Natural language querying with contextually relevant answers
- Multi-tenant access control with role-based permissions
- Event-driven processing for fast API responses
- Audit logging for compliance and debugging
- Flexible LLM options (OpenAI or HuggingFace)


Your RAG system is ready to help farmers query their harvest data naturally while keeping data secure with proper authorization.


## Final Thoughts


This was a fun exercise in tackling a complex authorization problem and also building something production-grade. I also got to play out some of my Stardew Valley fancies IRL. Maybe it's time I actually move to a cozy farm and grow my own crops (so long as the farm has a good Internet connection!)


The repository can be[found on the Motia GitHub](https://github.com/MotiaDev/motia-examples/tree/main/examples/harvest-logbook-rag/src/services/harvest-logbook) .


Feel free to reach out to us on LinkedIn or jump into the[SpiceDB Discord](https://authzed.com/discord) if you have any questions. Happy farming!


Originally published November 18, 2025: typos and new image


On this page


- What You'll Build
- Prerequisites
- Getting Started
- 1. Create Your Motia Project
- 2. Install Dependencies
- 3. Setup Pinecone
- Create a Pinecone Account
- Create an Index
- Get Your Credentials
- 4. Setup SpiceDB
- Start SpiceDB with Docker
- Verify SpiceDB is Running
- 5. Configure Environment Variables
- 6. Initialize SpiceDB Schema
- Create the Schema File
- Create Setup Scripts
- Install Script Runner
- Add Scripts to package.json
- Run the Setup
- 7. Start Development Server
- Exploring the Project
- System Architecture
- The RAG Pipeline
- Event-Driven Architecture
- Authorization Layer
- What We'll Build
- Step 1: Create the Harvest Entry API
- What We're Building
- Why This Step Matters
- Create the Step File
- Input Validation
- Step Configuration
- Authorization Check
- Handler Logic
- Event Emission
- Test the Step
- Step 2: Process Embeddings
- What We're Building
- Why This Step Matters
- Create the Step File
- Input Schema
- Step Configuration
- Text Chunking
- Embedding Generation
- Vector Storage
- Test the Step
- Step 3: Query Agent
- What We're Building
- Why This Step Matters
- Create the Step File
- Input Schema
- Step Configuration
- RAG Query Process
- Vector Search
- LLM Response Generation
- Event Emission
- Test the Step
- Step 4: Query-Only Endpoint
- What We're Building
- Why This Step Matters
- Create the Step File
- Input Validation
- Step Configuration
- Authorization Middleware
- Handler Logic
- Test the Step
- Step 5: Log to Sheets
- What We're Building
- Why This Step Matters
- Create the Step File
- Input Schema
- Step Configuration
- Logging Service Selection
- Error Handling
- CSV Output Format
- Test the Step
- Testing the System
- Start the Server
- Test 1: Store Harvest Data
- Test 2: Query the Data
- Test 3: Verify Authorization
- View the Logs
- Conclusion
- Final Thoughts


## Related


[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Cormac Foster · Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)


[AI It's Time to Decompose IAM IAM has never been a single thing—and treating it like one is holding us back. Authentication and authorization are fundamentally different disciplines, and agentic AI might finally force us to address them separately. Apr 4, 2026 · 3 min](https://authzed.com/blog/decomposing-iam)[AI It's Time to Decompose IAM IAM has never been a single thing—and treating it like one is holding us back. Authentication and authorization are fundamentally different disciplines, and agentic AI might finally force us to address them separately. Cormac Foster · Apr 4, 2026 · 3 min](https://authzed.com/blog/decomposing-iam)


[Company Agentic Security and Governance at RSAC and Kubecon Next week, RSAC and KubeCon happen at the same time. While the two shows have very different core audiences, the overlap in topic this year is striking—especially around authorization as a key driver of AI and agentic security. Mar 20, 2026 · 3 min](https://authzed.com/blog/agentic-security-and-governance-rsac-and-kubecon)[Company Agentic Security and Governance at RSAC and Kubecon Next week, RSAC and KubeCon happen at the same time. While the two shows have very different core audiences, the overlap in topic this year is striking—especially around authorization as a key driver of AI and agentic security. Cormac Foster · Mar 20, 2026 · 3 min](https://authzed.com/blog/agentic-security-and-governance-rsac-and-kubecon)
