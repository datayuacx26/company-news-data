---
schema_version: "1.0.0"
document_id: "8a340dde6e4f7e3fa3c9d7aed617698978c96fd4c0ba654d2e305caecdd24ce1"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/fine-grained-authorization-using-spicedb-for-retrieval-augmented-generation-rag"
published_at: "2024-12-04T06:41:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:59:48.691285+00:00"
content_hash: "sha256:077bc9af96e3a11ed89eee1e2f950cfb1e3ee7a43765572d6b54d22376440cc3"
---

# Fine Grained Authorization using SpiceDB for Retrieval Augmented Generation (RAG)

## Introduction


Building "enterprise-ready AI" requires ensuring users can only augment prompts with data they're authorized to access. Relationship-based access control (ReBAC) is particularly well-suited for fine-grained authorization in Retrieval-Augmented Generation (RAG). ReBAC enables you to generate a list of resources a specific user has permission to access (e.g., documents they can view). This capability lets you pre-filter vector database queries with a list of authorized object IDs, improving both efficiency and security. Moreover, ReBAC excels at fine-grained authorization because it makes decisions based on relationships between objects, offering more precise control compared to traditional models like RBAC and ABAC.


## RAG with ReBAC Walkthrough


Let's walk through implementing fine-grained authorization for RAG using Pinecone, Langchain, OpenAI, and SpiceDB.


[Download a Jupyter Notebook file](https://github.com/authzed/examples/blob/main/notebooks/rag.ipynb) for this tutorial


### Prerequisites


- Access to a[SpiceDB](https://authzed.com/spicedb) instance and API key. (This example uses insecure connections to a locally running SpiceDB instance. You can find examples of using a secure client[here](https://github.com/authzed/authzed-py?tab=readme-ov-file#basic-usage) .)
- A[Pinecone](https://www.pinecone.io/) account and API key.
- An[OpenAI](https://openai.com/) account and API key.


#### Installing Libraries


Run the following command to install the required libraries for this example:


` pip install python-dotenv authzed pinecone pinecone\[grpc\] langchain_pinecone langchain langchain_openai langchain_core`


#### Running SpiceDB


Follow the[install guide](https://authzed.com/docs/spicedb/getting-started/installing-spicedb) for your platform and run a local instance of SpiceDB.


` spicedb serve --grpc-preshared-key rag-rebac-walkthrough`


### Secrets


This walkthrough requires the following environment variables. The easiest way to set these is with a .env file in your working directory:


- ` OPENAI_API_KEY=<add OpenAI key>`
- ` PINECONE_API_KEY=<add Pinecone key>`
- ` SPICEDB_API_KEY=rag-rebac-walkthrough`
- ` SPICEDB_ADDR=localhost:50051`


### Setup


python


1


2


3


4


```text
from   dotenv  import   load_dotenv


#load secrets from .env file
load_dotenv()


```


python


1


2


3


4


```text
from   dotenv  import   load_dotenv


#load secrets from .env file
load_dotenv()


```


1


```text
True


```


1


```text
True


```


In this scenario, we'll authorize access to view blog articles.


First, we'll define the authorization logic for our example by writing a[schema](https://authzed.com/docs/spicedb/concepts/schema) to SpiceDB. The schema below defines two object types,` user` and` article` . Users can relate to a document as a` viewer` , and any user who is related to a document as a` viewer` can` view` the document.


python


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


```text
from   authzed.api.v1  import   (
Client,
WriteSchemaRequest,
)


import   os


#change to bearer_token_credentials if you are using tls
from   grpcutil  import   insecure_bearer_token_credentials


SCHEMA =  """definition user {}


definition article {
relation viewer: user


permission view = viewer
}"""


client = Client(os.getenv( 'SPICEDB_ADDR'  ), insecure_bearer_token_credentials(os.getenv( 'SPICEDB_API_KEY'  )))
resp = client.WriteSchema(WriteSchemaRequest(schema=SCHEMA))


```


python


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


```text
from   authzed.api.v1  import   (
Client,
WriteSchemaRequest,
)


import   os


#change to bearer_token_credentials if you are using tls
from   grpcutil  import   insecure_bearer_token_credentials


SCHEMA =  """definition user {}


definition article {
relation viewer: user


permission view = viewer
}"""


resp = client.WriteSchema(WriteSchemaRequest(schema=SCHEMA))


```


Now, we'll write relationships to SpiceDB specifying that Tim is a viewer of documents 123 and 456.


After writing these relationships, any permission checks to SpiceDB will reflect that Tim can view documents 123 and 456.


python


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


25


26


27


28


29


30


31


32


33


34


35


36


37


38


39


40


```text
from   authzed.api.v1  import   (
ObjectReference,
Relationship,
RelationshipUpdate,
SubjectReference,
WriteRelationshipsRequest,
)


client.WriteRelationships(
WriteRelationshipsRequest(
updates=[
RelationshipUpdate(
operation=RelationshipUpdate.Operation.OPERATION_CREATE,
relationship=Relationship(
resource=ObjectReference(object_type= "article"  , object_id= "123"  ),
relation= "viewer"  ,
subject=SubjectReference(
object  =ObjectReference(
object_type= "user"  ,
object_id= "tim"  ,
)
),
),
),
RelationshipUpdate(
operation=RelationshipUpdate.Operation.OPERATION_CREATE,
relationship=Relationship(
resource=ObjectReference(object_type= "article"  , object_id= "456"  ),
relation= "viewer"  ,
subject=SubjectReference(
object  =ObjectReference(
object_type= "user"  ,
object_id= "tim"  ,
)
),
),
),
]
)
)


```


python


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


25


26


27


28


29


30


31


32


33


34


35


36


37


38


39


40


```text
from   authzed.api.v1  import   (
ObjectReference,
Relationship,
RelationshipUpdate,
SubjectReference,
WriteRelationshipsRequest,
)


client.WriteRelationships(
WriteRelationshipsRequest(
updates=[
RelationshipUpdate(
operation=RelationshipUpdate.Operation.OPERATION_CREATE,
relationship=Relationship(
resource=ObjectReference(object_type= "article"  , object_id= "123"  ),
relation= "viewer"  ,
subject=SubjectReference(
object  =ObjectReference(
object_type= "user"  ,
object_id= "tim"  ,
)
),
),
),
RelationshipUpdate(
operation=RelationshipUpdate.Operation.OPERATION_CREATE,
relationship=Relationship(
resource=ObjectReference(object_type= "article"  , object_id= "456"  ),
relation= "viewer"  ,
subject=SubjectReference(
object  =ObjectReference(
object_type= "user"  ,
object_id= "tim"  ,
)
),
),
),
]
)
)


```


1


```text
<_AioCall object>


```


1


```text
<_AioCall object>


```


We'll now define a Pinecone serverless index.


Pinecone is a specialized database for handling vector-based data. Their serverless product simplifies getting started with a vector DB.


python


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
from   pinecone.grpc  import   PineconeGRPC  as   Pinecone
from   pinecone  import   ServerlessSpec
import   os


pc = Pinecone(api_key=os.getenv( "PINECONE_API_KEY"  ))


index_name =  "oscars"


pc.create_index(
name=index_name,
dimension= 1024  ,
metric= "cosine"  ,
spec=ServerlessSpec(
cloud= "aws"  ,
region= "us-east-1"
)
)


```


python


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
from   pinecone.grpc  import   PineconeGRPC  as   Pinecone
from   pinecone  import   ServerlessSpec
import   os


pc = Pinecone(api_key=os.getenv( "PINECONE_API_KEY"  ))


index_name =  "oscars"


pc.create_index(
name=index_name,
dimension= 1024  ,
metric= "cosine"  ,
spec=ServerlessSpec(
cloud= "aws"  ,
region= "us-east-1"
)
)


```


We are simulating a real-world RAG (retrieval-augmented generation) scenario by embedding a completely fictional string: "Peyton Manning won the 2023 Oscar for best football movie.". Since LLMs don't "know" this fact (as it's made up), we mimic a typical RAG case where private or unknown data augments prompts.


In this example, we also specify metadata like article_id to track which article the string comes from. The article_id is important for linking embeddings to objects that users are authorized on.


python


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


25


26


27


28


29


30


```text
from   langchain_pinecone  import   PineconeEmbeddings
from   langchain_pinecone  import   PineconeVectorStore


from   langchain.schema  import   Document
import   os


# Create a Document object that specifies our made up article and specifies the document_id as metadata.
text =  "Peyton Manning won the 2023 Oscar for best football movie"
metadata = {
"article_id"  :  "123"
}
document = Document(page_content=text,metadata=metadata)


# Initialize a LangChain embedding object.
model_name =  "multilingual-e5-large"
embeddings = PineconeEmbeddings(
model=model_name,
pinecone_api_key=os.environ.get( "PINECONE_API_KEY"  )
)


namespace_name =  "oscar"


# Upsert the embedding into your Pinecone index.
docsearch = PineconeVectorStore.from_documents(
documents=[document],
index_name=index_name,
embedding=embeddings,
namespace=namespace_name
)


```


python


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


25


26


27


28


29


30


```text
from   langchain_pinecone  import   PineconeEmbeddings
from   langchain_pinecone  import   PineconeVectorStore


from   langchain.schema  import   Document
import   os


text =  "Peyton Manning won the 2023 Oscar for best football movie"
metadata = {
"article_id"  :  "123"
}
document = Document(page_content=text,metadata=metadata)


# Initialize a LangChain embedding object.
model_name =  "multilingual-e5-large"
embeddings = PineconeEmbeddings(
model=model_name,
pinecone_api_key=os.environ.get( "PINECONE_API_KEY"  )
)


namespace_name =  "oscar"


# Upsert the embedding into your Pinecone index.
docsearch = PineconeVectorStore.from_documents(
documents=[document],
index_name=index_name,
embedding=embeddings,
namespace=namespace_name
)


```


1


2


```text
WARNING: All log messages before absl::InitializeLog() is called are written to STDERR
I0000 00:00:1730934436.344860 32369463 fork_posix.cc:77] Other threads are currently calling into gRPC, skipping fork() handlers


```


1


2


```text


```


### Making a request when the user is authorized to view the necessary contextual data


Next, we will query SpiceDB for a list of documents that Tim is allowed to view. Here, we use the[LookupResources API](https://buf.build/authzed/api/docs/main:authzed.api.v1#authzed.api.v1.LookupResourcesRequest) to obtain a list of articles that` tim` has` view` permission on.


python


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


25


26


27


28


29


30


31


```text
from   authzed.api.v1  import   (
LookupResourcesRequest,
ObjectReference,
SubjectReference,
)


subject = SubjectReference(
object  =ObjectReference(
object_type= "user"  ,
object_id= "tim"  ,
)
)


def    lookupArticles  ():
return   client.LookupResources(
LookupResourcesRequest(
subject=subject,
permission= "view"  ,
resource_object_type= "article"  ,
)
)


resp = lookupArticles()


authorized_articles = []


async    for   response  in   resp:
authorized_articles.append(response.resource_object_id)


print  ( "Article IDs that Tim is authorized to view:"  )
print  (authorized_articles)


```


python


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


25


26


27


28


29


30


31


```text
from   authzed.api.v1  import   (
LookupResourcesRequest,
ObjectReference,
SubjectReference,
)


subject = SubjectReference(
object  =ObjectReference(
object_type= "user"  ,
object_id= "tim"  ,
)
)


def    lookupArticles  ():
return   client.LookupResources(
LookupResourcesRequest(
subject=subject,
permission= "view"  ,
resource_object_type= "article"  ,
)
)


resp = lookupArticles()


authorized_articles = []


async    for   response  in   resp:
authorized_articles.append(response.resource_object_id)


print  ( "Article IDs that Tim is authorized to view:"  )
print  (authorized_articles)


```


1


2


```text
Article IDs that Tim is authorized to view:
['123', '456']


```


1


2


```text
Article IDs that Tim is authorized to view:
['123', '456']


```


We can now issue a prompt to GPT-3.5, enhanced with relevant data that the user is authorized to access. This ensures that the response is based on information the user is permitted to view.


python


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


25


26


27


28


29


30


31


32


33


34


35


36


37


38


39


40


41


42


43


44


45


46


47


48


49


50


51


52


53


54


55


56


57


58


59


60


61


62


63


```text
from   langchain_openai  import   ChatOpenAI, OpenAIEmbeddings
from   langchain_pinecone  import   PineconeVectorStore
from   langchain_core.output_parsers  import   StrOutputParser
from   langchain.prompts  import   ChatPromptTemplate
from   langchain.schema.output_parser  import   StrOutputParser
from   langchain_core.runnables  import   (
RunnableParallel,
RunnablePassthrough
)


# Define the ask function
def    ask  ():
# Initialize a LangChain object for an OpenAI chat model.
llm = ChatOpenAI(
openai_api_key=os.environ.get( "OPENAI_API_KEY"  ),
model_name= "gpt-3.5-turbo"  ,
temperature= 0.0
)


# Initialize a LangChain object for a Pinecone index with an OpenAI embeddings model.
knowledge = PineconeVectorStore.from_existing_index(
index_name=index_name,
namespace=namespace_name,
embedding=OpenAIEmbeddings(
openai_api_key=os.environ[ "OPENAI_API_KEY"  ],
dimensions= 1024  ,
model= "text-embedding-3-large"
)
)


# Initialize a retriever with a filter that restricts the search to authorized documents.
retriever=knowledge.as_retriever(
search_kwargs={
"filter"  : {
"article_id"  :
{ "$in"  : authorized_articles},
},
}
)


# Initialize a string prompt template that let's us add context and a question.
prompt = ChatPromptTemplate.from_template( """Answer the question below using the context:


Context: {context}


Question: {question}


Answer: """  )


retrieval =  RunnableParallel(
{ "context"  : retriever,  "question"  : RunnablePassthrough()}
)


chain = retrieval | prompt | llm | StrOutputParser()


question =  """Who won the 2023 Oscar for best football movie?"""


print  ( "Prompt: \n"  )
print  (question)
print  (chain.invoke(question))


#invoke the ask function
ask()


```


python


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


25


26


27


28


29


30


31


32


33


34


35


36


37


38


39


40


41


42


43


44


45


46


47


48


49


50


51


52


53


54


55


56


57


58


59


60


61


62


63


```text
from   langchain_openai  import   ChatOpenAI, OpenAIEmbeddings
from   langchain_pinecone  import   PineconeVectorStore
from   langchain_core.output_parsers  import   StrOutputParser
from   langchain.prompts  import   ChatPromptTemplate
from   langchain.schema.output_parser  import   StrOutputParser
from   langchain_core.runnables  import   (
RunnableParallel,
RunnablePassthrough
)


# Define the ask function
def    ask  ():
# Initialize a LangChain object for an OpenAI chat model.
llm = ChatOpenAI(
openai_api_key=os.environ.get( "OPENAI_API_KEY"  ),
model_name= "gpt-3.5-turbo"  ,
temperature= 0.0
)


knowledge = PineconeVectorStore.from_existing_index(
index_name=index_name,
namespace=namespace_name,
embedding=OpenAIEmbeddings(
openai_api_key=os.environ[ "OPENAI_API_KEY"  ],
dimensions= 1024  ,
model= "text-embedding-3-large"
)
)


retriever=knowledge.as_retriever(
search_kwargs={
"filter"  : {
"article_id"  :
{ "$in"  : authorized_articles},
},
}
)


# Initialize a string prompt template that let's us add context and a question.


Context: {context}


Question: {question}


Answer: """  )


retrieval =  RunnableParallel(
{ "context"  : retriever,  "question"  : RunnablePassthrough()}
)


chain = retrieval | prompt | llm | StrOutputParser()


question =  """Who won the 2023 Oscar for best football movie?"""


print  ( "Prompt: \n"  )
print  (question)
print  (chain.invoke(question))


#invoke the ask function
ask()


```


1


2


3


4


```text
Prompt:


Who won the 2023 Oscar for best football movie?
Peyton Manning


```


1


2


3


4


```text
Prompt:


Who won the 2023 Oscar for best football movie?
Peyton Manning


```


### Making a request when the user is not authorized to view the necessary contextual data


Now, let's see what happens when Tim is not authorized to view the document.


First, we will delete the relationship that related Tim as a viewer to document 123.


python


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


```text
client.WriteRelationships(
WriteRelationshipsRequest(
updates=[
RelationshipUpdate(
operation=RelationshipUpdate.Operation.OPERATION_DELETE,
relationship=Relationship(
resource=ObjectReference(object_type= "article"  , object_id= "123"  ),
relation= "viewer"  ,
subject=SubjectReference(
object  =ObjectReference(
object_type= "user"  ,
object_id= "tim"  ,
)
),
),
),
]
)
)


```


python


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


```text
client.WriteRelationships(
WriteRelationshipsRequest(
updates=[
RelationshipUpdate(
operation=RelationshipUpdate.Operation.OPERATION_DELETE,
relationship=Relationship(
resource=ObjectReference(object_type= "article"  , object_id= "123"  ),
relation= "viewer"  ,
subject=SubjectReference(
object  =ObjectReference(
object_type= "user"  ,
object_id= "tim"  ,
)
),
),
),
]
)
)


```


1


```text
<_AioCall object>


```


1


```text
<_AioCall object>


```


Next, we will update the list of documents that Tim is authorized to view.


python


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


```text
#this function was defined above
resp = lookupArticles()


authorized_articles = []


async    for   response  in   resp:
authorized_articles.append(response.resource_object_id)


print  ( "Documents that Tim can view:"  )
print  (authorized_articles)


```


python


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


```text
#this function was defined above
resp = lookupArticles()


authorized_articles = []


async    for   response  in   resp:
authorized_articles.append(response.resource_object_id)


print  ( "Documents that Tim can view:"  )
print  (authorized_articles)


```


1


2


```text
Documents that Tim can view:
['456']


```


1


2


```text
Documents that Tim can view:
['456']


```


Now, we can run our query again.


Note that we no longer recieve a completion that answers our question because Tim is no longer authorized to view the document that contains the context required to answer the question.


python


1


2


```text
#this function was defined above
ask()


```


python


1


2


```text
#this function was defined above
ask()


```


1


2


3


4


```text
Prompt:


Who won the 2023 Oscar for best football movie?
There is no information provided in the context to determine who won the 2023 Oscar for best football movie.


```


1


2


3


4


```text
Prompt:


Who won the 2023 Oscar for best football movie?


```


### Clean Up


You can now delete your Pinceone index if you'd like to.


python


1


```text
pc.delete_index(index_name)


```


python


1


```text
pc.delete_index(index_name)


```


## Building a RAG?


AuthZed can help you add fine-grained access control to your RAG pipeline.[Learn more here](https://authzed.com/use-cases/ai-retrieval-augmented-generation) and schedule a call with us.


On this page


- Introduction
- RAG with ReBAC Walkthrough
- Prerequisites
- Installing Libraries
- Running SpiceDB
- Secrets
- Setup
- Making a request when the user is authorized to view the necessary contextual data
- Making a request when the user is not authorized to view the necessary contextual data
- Clean Up
- Building a RAG?


## Related


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Engineering Build a production-grade Agentic RAG system using AuthZed Cloud Learn how to build a production-grade Agentic RAG system on AuthZed Cloud, where authorization is hardcoded into the LangGraph pipeline—not a prompt instruction the agent can skip. Apr 15, 2026 · 7 min](https://authzed.com/blog/build-production-grade-agentic-rag-authzed-cloud)[Engineering Build a production-grade Agentic RAG system using AuthZed Cloud Learn how to build a production-grade Agentic RAG system on AuthZed Cloud, where authorization is hardcoded into the LangGraph pipeline—not a prompt instruction the agent can skip. Sohan Maheshwar · Apr 15, 2026 · 7 min](https://authzed.com/blog/build-production-grade-agentic-rag-authzed-cloud)


[Engineering Introducing SpiceBox and spicedb-dev AI coding agents need better permissions, and so does the code they write. SpiceBox enforces fine-grained permissions on AI coding agents using SpiceDB, while spicedb-dev gives agents the authorization context they need to generate code with proper access control from the start. Both are open source. Apr 8, 2026 · 7 min](https://authzed.com/blog/spicedb-dev-and-spicebox-add-permissions-for-ai-coding-agents)[Engineering Introducing SpiceBox and spicedb-dev AI coding agents need better permissions, and so does the code they write. SpiceBox enforces fine-grained permissions on AI coding agents using SpiceDB, while spicedb-dev gives agents the authorization context they need to generate code with proper access control from the start. Both are open source. Joey Schorr · Apr 8, 2026 · 7 min](https://authzed.com/blog/spicedb-dev-and-spicebox-add-permissions-for-ai-coding-agents)
