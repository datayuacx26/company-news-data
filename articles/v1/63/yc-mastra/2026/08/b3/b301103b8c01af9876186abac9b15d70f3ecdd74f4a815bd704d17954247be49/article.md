---
schema_version: "1.0.0"
document_id: "b301103b8c01af9876186abac9b15d70f3ecdd74f4a815bd704d17954247be49"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-2c9844a44afc"
canonical_url: "https://mastra.ai/blog/introducing-dynamic-workflows"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-12T23:46:00.370774+00:00"
fetched_at: "2026-08-12T23:46:03.089544+00:00"
content_hash: "sha256:c5c52e00c54d79a56f209de78bb1294f944c4cfeeb15256d7562ae240c385e54"
---

# Introducing Dynamic Workflows for Mastra

You can now add[dynamic workflows](https://mastra.ai/docs/workflows/dynamic-workflows) to your live Mastra server without changing or redeploying code. Users, external systems, or agents can build workflows on the fly as structured JSON objects, then add them to a running Mastra server.


Before dynamic workflows, every workflow had to be authored in TypeScript, registered with the Mastra instance, and deployed as part of the server bundle. Now, creating workflows isn't limited to engineers who can write and deploy code — agents or other team members can create them too.


Your browser does not support the video tag.


A dynamic workflow is a graph of agents, tools, and control-flow steps, persisted in storage as JSON that references registered primitives by` id` . Once added to a live server, a dynamic workflow is available to run from any HTTP client.


Workflow steps still need to receive expected inputs and pass expected outputs. You can use[mapConfig](https://mastra.ai/reference/workflows/dynamic-workflow-definition#mapping-steps) to read from` ${initData._}` (the workflow input) or` ${stepResults.<id>._}` (a prior step's output), so any agent or tool can be used at any step.


## Get started


Install @mastra/core and a storage adapter to persist workflow definitions:


```text
npm   install   @mastra/core   @mastra/libsql
```


note


Requires` @mastra/core@1.58.0` or later, added in[PR #20471](https://github.com/mastra-ai/mastra/pull/20471) .


Register the agents and tools dynamic workflows can reference. This is a standard Mastra setup — the only additional requirement is a storage adapter:


src/mastra/index.ts


```text
import   {   Mastra   }   from   "  @mastra/core/mastra  "  ;
import   {   LibSQLStore   }   from   "  @mastra/libsql  "  ;
import   {   subjectExtractor   }   from   "  ./agents/subject-extractor-agent  "  ;
import   {   summarizer   }   from   "  ./agents/summarizer-agent  "  ;
import   {   exaWebSearch   }   from   "  ./tools/exa-web-search-tool  "  ;


export   const   mastra   =   new   Mastra  ({
storage:   new   LibSQLStore  ({
id:   "  mastra-storage  "  ,
url:   "  file:./mastra.db  "
}),
agents: { subjectExtractor, summarizer },
tools: { exaWebSearch }
});
```


## Manage over HTTP


The examples below show how to manage dynamic workflows using[server routes](https://mastra.ai/reference/server/routes#dynamic-workflows) . The Mastra Client SDK's[workflows API](https://mastra.ai/reference/client-js/workflows) can also be used.


### UPSERT workflow


To update or insert a workflow definition and registered on the Mastra instance, send a` POST` request to` /api/stored/workflows` with a valid JSON body:


POST


```text
await   fetch  (  `${  MASTRA_URL  }  /api/stored/workflows  `  ,   {
method:   "  POST  "  ,
headers: {   "  Content-Type  "  :   "  application/json  "   },
body: JSON.  stringify  ({
id:   "  wf-nnxe8nah  "  ,
description:   "  A quick test workflow  "  ,
inputSchema: {},
outputSchema: {},
graph: [
{
type:   "  mapping  "  ,
id:   "  map-to-subject-extractor  "  ,
mapConfig:   '  {"prompt":{"template":"${initData.message}"}}  '
},
{
type:   "  agent  "  ,
id:   "  subject-extractor  "  ,
agentId:   "  subject-extractor  "  ,
description:   "  Extracts the main subject from a user message  "
}
]
})
});
```


### GET workflows


To list registered workflows, send a` GET` request to` /api/stored/workflows` :


GET


```text
const   res   =   await   fetch  (  `${  MASTRA_URL  }  /api/stored/workflows  `  ,   {
method:   "  GET  "
});
const   {   workflows   }   =   await   res.  json  ();
```


### DELETE workflow


To delete a registered workflow, send a DELETE request to /api/stored/workflows/:id, passing the id of the workflow you want to remove:


DELETE


```text
await   fetch  (  `${  MASTRA_URL  }  /api/stored/workflows/  ${  id  }`  ,   {
method:   "  DELETE  "
});
```


For more information and full configuration options, see:


- [Dynamic workflows](https://mastra.ai/docs/workflows/dynamic-workflows)
- [Dynamic workflow definition reference](https://mastra.ai/reference/workflows/dynamic-workflow-definition)
- [Server routes — Dynamic workflows](https://mastra.ai/reference/server/routes#dynamic-workflows)
- [Client SDK workflows API](https://mastra.ai/reference/client-js/workflows#dynamic-workflows)
