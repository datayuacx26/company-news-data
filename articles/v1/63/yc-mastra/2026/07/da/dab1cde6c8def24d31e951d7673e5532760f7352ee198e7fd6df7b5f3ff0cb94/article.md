---
schema_version: "1.0.0"
document_id: "dab1cde6c8def24d31e951d7673e5532760f7352ee198e7fd6df7b5f3ff0cb94"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-2c9844a44afc"
canonical_url: "https://mastra.ai/blog/introducing-memory-extractors"
published_at: "2026-07-21T00:00:00+00:00"
first_seen_at: "2026-07-24T10:56:34.426085+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:47e276234e8c192c22b1d1509c16b11425af91ffa37ee7a8f7d569b4412ae9fa"
---

# Introducing Memory Extractors for Mastra Agents

You can now extract data from your agent's conversations using a[memory extractor](https://mastra.ai/docs/memory/observational-memory#extractors) . Give an` Extractor` instructions and persist defined information to send to downstream services, or surface in your app's UI.


Extractors work as part of[Observational Memory](https://mastra.ai/docs/memory/observational-memory) 's observer and reflector background agents. You can access extracted values from a[stream chunk](https://mastra.ai/docs/memory/observational-memory#read-extracted-values-from-a-stream) and can configure the output in two ways:


1. **Structured** : Zod schema for typed output. Looser instructions — model infers what to capture from schema field names.
2. **Text** : No schema, plain-text output. Explicit instructions — spell out what to capture.


Your browser does not support the video tag.


Before extractors, pulling structured data from conversations meant either:


- Using a[working-memory template](https://mastra.ai/docs/memory/working-memory#custom-templates) and a tool to write a JSON output.
- A[supervisor agent](https://mastra.ai/docs/agents/supervisor-agents) with a subagent.


Now the extractor slots into observational memory's observer pass — piggybacking on an already-primed context and prompt cache. Each extraction runs against cached tokens instead of a fresh LLM call.


## Get started


Install the latest` @mastra/memory` and a storage adapter that supports the observational memory domain.


Terminal


```text
npm   install   @mastra/memory@latest   @mastra/libsql@latest
```


note


Requires` @mastra/memory@1.22.0` or later, added in[PR #18653](https://github.com/mastra-ai/mastra/pull/18653) .


Configure a` storage` adapter on your main` Mastra` instance to persist extractions across turns and restarts.


src/mastra/index.ts


```text
import   {   Mastra   }   from   "  @mastra/core/mastra  "  ;


import   {   LibSQLStore   }   from   "  @mastra/libsql  "  ;
import   {   supportAgent   }   from   "  ./agents/support-agent  "  ;


export   const   mastra   =   new   Mastra  ({
agents: { supportAgent },
storage:   new   LibSQLStore  ({
id:   "  mastra-storage  "  ,
url:   "  file:./mastra.db  "
})
});
```


Add an` Extractor` to` observationalMemory.observation.extract` with a` name` ,` instructions` , and an optional Zod` schema` :


src/mastra/agents/support-agent.ts


```text
import   {   Agent   }   from   "  @mastra/core/agent  "  ;
import   {   Memory,   Extractor   }   from   "  @mastra/memory  "  ;
import   {   LibSQLStore   }   from   "  @mastra/libsql  "  ;
import   {   z   }   from   "  zod  "  ;


export   const   supportAgent   =   new   Agent  ({
id:   "  support-agent  "  ,
name:   "  Support Agent  "  ,
instructions:   /* ... */  ,
model:   "  anthropic/claude-opus-4-8  "  ,
memory:   new   Memory  ({
options: {
observationalMemory: {
model:   "  anthropic/claude-sonnet-5  "  ,
observation: {
extract: [
new   Extractor  ({
name:   "  Support profile  "  ,
instructions:
"  Extract the user's Mastra setup (OS, Node version, @mastra/core version), the package they're asking about, their current issue, and steps they've tried.  "  ,
schema: z.  object  ({
os: z.  string  ().  optional  (),
nodeVersion: z.  string  ().  optional  (),
mastraCoreVersion: z.  string  ().  optional  (),
packageName: z.  string  ().  optional  (),
issue: z.  string  ().  optional  (),
triedSoFar: z.  array  (z.  string  ()).  optional  ()
})
})
],
bufferOnIdle:   true  ,
messageTokens:   100  ,
bufferTokens:   false
}
}
}
})
});
```


- ` bufferOnIdle: true` : Runs observation after each turn ends.
- ` messageTokens: 100` : Triggers observation on every turn (default is 30000 tokens).
- ` bufferTokens: false` : Runs the observer synchronously inside the stream.


## Extractions


Extractor results are emitted when observational memory completes an observation or reflection. You can read` extractedValues` from the stream:


```text
const   stream   =   await   agent.  stream  (  "  ...  "  );


for   await   (  const   chunk   of   stream.fullStream)   {
if   (chunk.type   ===   "  data-om-observation-end  "   ||   chunk.type   ===   "  data-om-buffering-end  "  )   {
const   {   extractedValues   }   =   chunk.data;
console.  log  (extractedValues[  "  support-profile  "  ]);   // Derived from the extractor's name: "Support profile"
}
}
```


## Example structured extraction


When a schema is defined each field is populated with the correct extracted data:


```text
'support-profile': {
os: 'macOS Sonoma',
nodeVersion: '25.5.0',
mastraCoreVersion: '1.41.0',
packageName: '@mastra/e2b',
issue: `Whether E2B sandboxes can be used with Mastra code mode; user got error importing E2BCodeModeTransport from @mastra/e2b: "Module '@mastra/e2b' has no exported member 'E2BCodeModeTransport'"`,
triedSoFar: [ 'Attempted to import E2BCodeModeTransport from @mastra/e2b' ]
}


```


## onExtracted callback


The` onExtracted` callback fires per extraction with the` current` value and` threadId` . You can use it to forward extractions to downstream services like webhooks, a CRM, your own database:


```text
// ...
new   Extractor  ({
name:   "  Support profile  "  ,
instructions:  /* ... */  ,
schema: z.  object  ({
// ...
}),
onExtracted  :   async   (context)   =>   {
const   { current, threadId }   =   context;
console.  log  (current);
// send data to downstream services
}
});
```


For more information and full configuration options, see:


- [Extractors](https://mastra.ai/docs/memory/observational-memory#extractors)
- [Observational Memory overview](https://mastra.ai/docs/memory/observational-memory)
- [Extractor API reference](https://mastra.ai/reference/memory/observational-memory#extractor-api)
- [Memory overview](https://mastra.ai/docs/memory/overview)
