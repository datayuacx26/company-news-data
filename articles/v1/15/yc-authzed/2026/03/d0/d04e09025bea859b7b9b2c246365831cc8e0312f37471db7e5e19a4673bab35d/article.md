---
schema_version: "1.0.0"
document_id: "d04e09025bea859b7b9b2c246365831cc8e0312f37471db7e5e19a4673bab35d"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/langchain-spicedb-integration"
published_at: "2026-03-09T12:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:53:30.609843+00:00"
content_hash: "sha256:7f670de4942c3645b900d07dd64bc7c4882ec3674a07d86d3eb2202e06d74745"
---

# Introducing the LangChain SpiceDB Integration

**Update** : The LangChain - SpiceDB library is now an[official integration](https://docs.langchain.com/oss/python/integrations/tools/spicedb) .


2025 was the year AI went mainstream - RAG pipelines, LLM-powered agents, and multi-step assistants are running in companies of all sizes. One of the most popular frameworks to build AI applications is LangChain, so we're happy to announce a LangChain-SpiceDB integration! And with this integration we hope to help you with something that can be[tricky in the AI world](https://authzed.com/blog/policy-engines-dont-work-for-ai-authorization-heres-why) - authorization.


While the fundamentals of access control remain the same for AI Agents, the surface area for data leakage explodes. For example, an AI assistant might retrieve sensitive data from a knowledge base that the requesting user doesn't have access to. Furthermore, using conventional access control methods such as Role-Based Access Control (RBAC) crumble when permissions are inherited and nested because they only check a coarse role (like` employee` ) which is mapped to broad permissions.


This is where **Relationship-Based Access Control (ReBAC)** and **SpiceDB** shine as ReBAC prevents this scenario by explicitly modeling the user-resource relationships, ensuring the check respects all inherited permissions. Popularized in Google's 2019[Zanzibar paper](https://zanzibar.tech/) , ReBAC focuses on *relationships* to determine permissions for specific resources.[SpiceDB](https://github.com/authzed/spicedb) is the most scalable open-source implementation of Zanzibar, providing the low-latency, high-throughput authorization checks required for RAG and AI applications.


The` langchain-spicedb` library brings SpiceDB's authorization model into LangChain and LangGraph workflows, so you can build RAG pipelines that respect what users are allowed to see.


## RAG pipelines minus permissions


Here's what the standard RAG flow looks like:


User asks a question → you embed it → you fetch the closest vectors → you hand those documents to an LLM → the LLM answers.


**Nowhere in this flow is there a step that asks: *does this user actually have permission to see these documents?***


When authorization does get added, it usually arrives as RBAC: "this` user` has the` employee` role, employees can read` public` documents." Simple, but inadequate for anything involving multiple users with relationships to specific resources.


SpiceDB lets you model those relationships explicitly using[schema](https://authzed.com/docs/spicedb/concepts/schema) :


zed


1


2


3


4


5


6


```text
definition    user    {  }


definition    article    {
relation    viewer  :  user
permission   view  =   viewer
}


```


zed


1


2


3


4


5


6


```text
definition    user    {  }


definition    article    {
relation    viewer  :  user
permission   view  =   viewer
}


```


Then you ask: "Does Alice have` view` permission on` article:doc123` ?" SpiceDB answers based on actual relationships in its graph, not a coarse role lookup.


## Enter langchain-spicedb


We've published[langchain-spicedb](https://pypi.org/project/langchain-spicedb/) , an authorization library for RAG pipelines that plugs SpiceDB into LangChain and LangGraph. It's vector-store agnostic which means it works with[Pinecone](https://www.pinecone.io/learn/rag-access-control/) ,[Weaviate](https://github.com/authzed/examples/tree/main/agentic-rag-authorization) , Chroma, or anything that returns documents with metadata.


shell


1


```text
pip install langchain-spicedb[all]


```


shell


1


```text
pip install langchain-spicedb[all]


```


The core pattern is **post-retrieval filtering** : retrieve the best semantic matches first, then check permissions for each document retrieved with the user's permissions by using metadata (such as a` doc_id` ) before anything reaches the LLM. This ensures that the LLM doesn't access anything that the user is not supposed to view.


The library uses SpiceDB's` CheckBulkPermissionsRequest` API under the hood, so 100 documents cost one network call, not 100.[API reference here](https://buf.build/authzed/api/docs/main:authzed.api.v1#authzed.api.v1.CheckBulkPermissionsRequest)


## Three components, one decision


### 1.` SpiceDBAuthFilter` for LangChain chains


If you're building with[LCEL](https://blog.langchain.com/langchain-expression-language/) ,` SpiceDBAuthFilter` slots between your retriever and your LLM. User identity is passed at runtime via` configurable` , so you build the chain once and reuse it across users:


py


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


```text
from   langchain_spicedb  import   SpiceDBAuthFilter
from   langchain_core.runnables  import   RunnableParallel, RunnablePassthrough
from   langchain_core.output_parsers  import   StrOutputParser


auth = SpiceDBAuthFilter(
spicedb_endpoint= "localhost:50051"  ,
spicedb_token= "sometoken"  ,
subject_type= "user"  ,
resource_type= "article"  ,
resource_id_key= "article_id"  ,
permission= "view"  ,
)


chain = (
RunnableParallel({
"context"  : retriever | auth,   # authorization happens here
"question"  : RunnablePassthrough(),
})
| prompt
| llm
| StrOutputParser()
)


# Same chain, different users
answer =  await   chain.ainvoke(
"What is our litigation approach?"  ,
config={ "configurable"  : { "subject_id"  :  "alice"  }}
)


```


py


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


```text
from   langchain_spicedb  import   SpiceDBAuthFilter
from   langchain_core.runnables  import   RunnableParallel, RunnablePassthrough
from   langchain_core.output_parsers  import   StrOutputParser


auth = SpiceDBAuthFilter(
spicedb_endpoint= "localhost:50051"  ,
spicedb_token= "sometoken"  ,
subject_type= "user"  ,
resource_type= "article"  ,
resource_id_key= "article_id"  ,
permission= "view"  ,
)


chain = (
RunnableParallel({
"context"  : retriever | auth,   # authorization happens here
"question"  : RunnablePassthrough(),
})
| prompt
| llm
| StrOutputParser()
)


# Same chain, different users
answer =  await   chain.ainvoke(
"What is our litigation approach?"  ,
config={ "configurable"  : { "subject_id"  :  "alice"  }}
)


```


If you'd rather bundle retrieval and authorization into a single` BaseRetriever` use` SpiceDBRetriever` instead. This approach is useful when dropping this into an existing pipeline that already expects a retriever.


py


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
from   langchain_spicedb  import   SpiceDBRetriever


retriever = SpiceDBRetriever(
base_retriever=vector_store.as_retriever(),
subject_id= "alice"  ,
spicedb_endpoint= "localhost:50051"  ,
spicedb_token= "sometoken"  ,
resource_type= "article"  ,
resource_id_key= "article_id"  ,
)


docs =  await   retriever.ainvoke( "query"  )


```


py


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
from   langchain_spicedb  import   SpiceDBRetriever


retriever = SpiceDBRetriever(
base_retriever=vector_store.as_retriever(),
subject_id= "alice"  ,
spicedb_endpoint= "localhost:50051"  ,
spicedb_token= "sometoken"  ,
resource_type= "article"  ,
resource_id_key= "article_id"  ,
)


docs =  await   retriever.ainvoke( "query"  )


```


### 2.` create_auth_node` for LangGraph workflows


For stateful, multi-step LangGraph workflows, authorization becomes a node in your graph.` create_auth_node()` and` RAGAuthState` handle the wiring:


py


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
from   langgraph.graph  import   StateGraph, END
from   langchain_spicedb  import   create_auth_node, RAGAuthState


graph = StateGraph(RAGAuthState)


graph.add_node( "retrieve"  , retrieve_node)
graph.add_node( "authorize"  , create_auth_node(
spicedb_endpoint= "localhost:50051"  ,
spicedb_token= "sometoken"  ,
resource_type= "article"  ,
resource_id_key= "article_id"  ,
))
graph.add_node( "generate"  , generate_node)


graph.set_entry_point( "retrieve"  )
graph.add_edge( "retrieve"  ,  "authorize"  )
graph.add_edge( "authorize"  ,  "generate"  )
graph.add_edge( "generate"  , END)


app = graph. compile  ()
result =  await   app.ainvoke({
"question"  :  "What is our litigation strategy?"  ,
"subject_id"  :  "alice"  ,
})


```


py


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
from   langgraph.graph  import   StateGraph, END
from   langchain_spicedb  import   create_auth_node, RAGAuthState


graph = StateGraph(RAGAuthState)


graph.add_node( "retrieve"  , retrieve_node)
graph.add_node( "authorize"  , create_auth_node(
spicedb_endpoint= "localhost:50051"  ,
spicedb_token= "sometoken"  ,
resource_type= "article"  ,
resource_id_key= "article_id"  ,
))
graph.add_node( "generate"  , generate_node)


graph.set_entry_point( "retrieve"  )
graph.add_edge( "retrieve"  ,  "authorize"  )
graph.add_edge( "authorize"  ,  "generate"  )
graph.add_edge( "generate"  , END)


app = graph. compile  ()
result =  await   app.ainvoke({
"question"  :  "What is our litigation strategy?"  ,
"subject_id"  :  "alice"  ,
})


```


The auth node reads` retrieved_documents` from state, runs the permission check, and writes back` authorized_documents` plus` auth_results` - a metrics dict with retrieved count, authorized count, denied IDs, and check duration.


If you need to handle multiple resource types or conditional auth paths, extend` RAGAuthState` with your own fields, or use` AuthorizationNode` directly to build reusable nodes across graphs.


[This example](https://github.com/authzed/examples/tree/main/agentic-rag-authorization) uses Weaviate as the Vector database along with a LangGraph workflow for Agentic RAG.


### 3.` SpiceDBPermissionTool` for agents


For LLM agents that need to check permissions before acting, the library ships with tools the agent can call directly:


py


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


```text
from   langchain_spicedb  import   SpiceDBPermissionTool


tool = SpiceDBPermissionTool(
spicedb_endpoint= "localhost:50051"  ,
spicedb_token= "sometoken"  ,
subject_type= "user"  ,
resource_type= "article"  ,
)


result = tool.invoke({
"subject_id"  :  "alice"  ,
"resource_id"  :  "doc123"  ,
"permission"  :  "view"
})
# Returns: "true" or "false"


```


py


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


```text
from   langchain_spicedb  import   SpiceDBPermissionTool


tool = SpiceDBPermissionTool(
spicedb_endpoint= "localhost:50051"  ,
spicedb_token= "sometoken"  ,
subject_type= "user"  ,
resource_type= "article"  ,
)


result = tool.invoke({
"subject_id"  :  "alice"  ,
"resource_id"  :  "doc123"  ,
"permission"  :  "view"
})
# Returns: "true" or "false"


```


` SpiceDBBulkPermissionTool` handles multiple resources in a single call.


One important note here is that Authorization is on the critical path so the AI agent should **always** perform an authorization check. Ensure that there's no Agent reasoning to decide *whether* an authorization check should be performed. This is an anti-pattern if the agent decides not to perform an authorization check, which could lead to data leakage.


## A quick decision guide


- Simple LCEL chain? →` SpiceDBAuthFilter`
- Existing retriever-based pipeline? →` SpiceDBRetriever`
- Stateful, multi-step LangGraph workflow? →` create_auth_node`
- Agent that checks permissions on the fly? →` SpiceDBPermissionTool`


## The bigger picture


AI agents and LLMs now handle sensitive, dynamic data so ensuring a robust authorization layer is absolutely vital. The` langchain-spicedb` integration allows you to use powerful ReBAC to build genuinely secure AI applications and RAG pipelines, effectively closing the door on data leakage that conventional systems leave wide open. This is the critical component for trust in your AI infrastructure.


` langchain-spicedb` is on[PyPI](https://pypi.org/project/langchain-spicedb/) and[GitHub](https://github.com/authzed/langchain-spicedb) . Try it, break it, and open an issue when you do.


## Next steps


The PR to make it an official LangChain integration has been merged — see the[official integration docs](https://docs.langchain.com/oss/python/integrations/tools/spicedb) .


Here's a[production-style example](https://github.com/authzed/examples/) that uses Weaviate as the vector database and the LangGraph integration for fine-grained SpiceDB authorization.


If you're attending Kubecon Amsterdam, come join[this LangChain meetup](https://luma.com/oqjo3kut) on March 26th where we'll demo this library. Swing by and say hi.


On this page


- RAG pipelines minus permissions
- Enter langchain-spicedb
- Three components, one decision
- 1. SpiceDBAuthFilter for LangChain chains
- 2. create_auth_node for LangGraph workflows
- 3. SpiceDBPermissionTool for agents
- A quick decision guide
- The bigger picture
- Next steps


## Related


[Engineering Introducing SpiceBox and spicedb-dev AI coding agents need better permissions, and so does the code they write. SpiceBox enforces fine-grained permissions on AI coding agents using SpiceDB, while spicedb-dev gives agents the authorization context they need to generate code with proper access control from the start. Both are open source. Apr 8, 2026 · 7 min](https://authzed.com/blog/spicedb-dev-and-spicebox-add-permissions-for-ai-coding-agents)[Engineering Introducing SpiceBox and spicedb-dev AI coding agents need better permissions, and so does the code they write. SpiceBox enforces fine-grained permissions on AI coding agents using SpiceDB, while spicedb-dev gives agents the authorization context they need to generate code with proper access control from the start. Both are open source. Joey Schorr · Apr 8, 2026 · 7 min](https://authzed.com/blog/spicedb-dev-and-spicebox-add-permissions-for-ai-coding-agents)


[Engineering Introducing AuthZed's MCP Servers We're launching two MCP servers to bring SpiceDB closer to your AI workflow. The AuthZed MCP Server provides instant access to documentation and examples, while the SpiceDB Dev MCP Server integrates with your development environment. Learn about our MCP journey from early prototypes to production, and discover how these tools can speed up your SpiceDB development. Sep 30, 2025 · 4 min](https://authzed.com/blog/introducing-authzeds-mcp-servers)[Engineering Introducing AuthZed's MCP Servers We're launching two MCP servers to bring SpiceDB closer to your AI workflow. The AuthZed MCP Server provides instant access to documentation and examples, while the SpiceDB Dev MCP Server integrates with your development environment. Learn about our MCP journey from early prototypes to production, and discover how these tools can speed up your SpiceDB development. Sam Kim · Sep 30, 2025 · 4 min](https://authzed.com/blog/introducing-authzeds-mcp-servers)


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)
