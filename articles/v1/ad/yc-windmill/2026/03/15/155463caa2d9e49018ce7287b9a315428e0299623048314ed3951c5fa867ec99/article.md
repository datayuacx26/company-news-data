---
schema_version: "1.0.0"
document_id: "155463caa2d9e49018ce7287b9a315428e0299623048314ed3951c5fa867ec99"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-12b6d71fe86e"
canonical_url: "https://www.windmill.dev/blog/launch-week-full-code-apps"
published_at: "2026-03-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:07.070377+00:00"
fetched_at: "2026-07-28T22:17:07.377837+00:00"
content_hash: "sha256:46b9dab6d3b3f8e5dcb5f9a606849f4c6c71fe4c686e833caa56d3d62b596fa0"
---

# Full-code apps: React and Svelte on Windmill

**Day 1 of[Windmill launch week](https://www.windmill.dev/launch-week-march-2026) .** You can now build complete applications with React or Svelte frontends connected to Windmill backend runnables through a typed, auto-generated API.


## The problem​


Workflow platforms typically have no UI layer. When they do, it is a low-code builder: great for dashboards and forms, but limiting when you need custom interactions, framework features like hooks and routing, or an existing component library.


Teams end up building separate frontends. That means a separate deployment pipeline, a separate auth system, REST endpoints to define and maintain, and glue code to connect it all. The orchestration layer runs your backend logic but has no opinion about how users interact with it.


For teams that need a custom frontend, we wanted them to build it where they already build the backend.


## Full-code apps: one import, typed calls​


A full-code app is a directory containing your frontend code (React or Svelte) and a` backend/` folder with scripts in any Windmill-supported language. Windmill bundles and serves the frontend. An auto-generated` wmill.ts` module provides typed functions to call your backend runnables.


Calling backend runnables from a full-code frontend with a typed API.


- React
- Svelte


```text
// App.tsx      import     {   useState  ,   useEffect   }     from     "react"  ;      import     {   backend   }     from     "./wmill.ts"  ;         export     default     function     App  (  )     {         const     [  data  ,   setData  ]     =     useState  (  null  )  ;         const     [  loading  ,   setLoading  ]     =     useState  (  true  )  ;            useEffect  (  (  )     =>     {         backend  .  get_failed_payments  (  {   days_back  :     7  ,   limit  :     100     }  )             .  then  (  setData  )             .  finally  (  (  )     =>     setLoading  (  false  )  )  ;         }  ,     [  ]  )  ;            if     (  loading  )     return     <  div  >  Loading...  </  div  >  ;            return     (           <  div  >             <  h2  >  Failed payments (  {  data  .  failed_payments_count  }  )  </  h2  >             <  p  >  Total: $  {  (  data  .  total_failed_amount     /     100  )  .  toFixed  (  2  )  }  </  p  >             <  table  >               <  thead  >                 <  tr  >  <th>ID  </  th  >  <th>Amount  </  th  >  <th>Reason  </  th  >  </  tr  >               </  thead  >               <  tbody  >                 {  data  .  failed_payments  .  map  (  (  p  )     =>     (                   <  tr     key  =  {  p  .  id  }  >                     <  td  >  {  p  .  id  }  </  td  >                     <  td  >  $  {  (  p  .  amount     /     100  )  .  toFixed  (  2  )  }  </  td  >                     <  td  >  {  p  .  failure_code  }  </  td  >                   </  tr  >                 )  )  }               </  tbody  >             </  table  >           </  div  >         )  ;      }
```


```text
<!-- App.svelte -->      <  script     lang  =  "  ts  "  >         import     {   backend   }     from     "./wmill.ts"  ;            let   data   =     $state  (  null  )  ;         let   loading   =     $state  (  true  )  ;            $effect  (  (  )     =>     {         backend  .  get_failed_payments  (  {     days_back  :     7  ,     limit  :     100     }  )             .  then  (  (  result  )     =>     {   data   =   result  ;     }  )             .  finally  (  (  )     =>     {   loading   =     false  ;     }  )  ;         }  )  ;      </  script  >        {#if loading}        <  div  >  Loading...  </  div  >     {:else}        <  div  >           <  h2  >  Failed payments ({data.failed_payments_count})  </  h2  >           <  p  >  Total: ${(data.total_failed_amount / 100).toFixed(2)}  </  p  >           <  table  >             <  thead  >               <  tr  >  <  th  >  ID  </  th  >  <  th  >  Amount  </  th  >  <  th  >  Reason  </  th  >  </  tr  >             </  thead  >             <  tbody  >             {#each data.failed_payments as payment}                <  tr  >                   <  td  >  {payment.id}  </  td  >                   <  td  >  ${(payment.amount / 100).toFixed(2)}  </  td  >                   <  td  >  {payment.failure_code}  </  td  >                 </  tr  >             {/each}            </  tbody  >           </  table  >         </  div  >     {/if}
```


## The typed backend API​


` backend.get_failed_payments` is fully typed. During development,` wmill app dev` watches your` backend/` folder and generates a` wmill.d.ts` file with typed signatures for each runnable. Given this backend script:


```text
# backend/get_failed_payments.py      def     main  (  stripe_resource  :   stripe  ,   days_back  :     int     =     7  ,   limit  :     int     =     100  )  :           # ...
```


The generated types will be:


```text
export     const   backend  :     {         get_failed_payments  :     (  v  :     {         stripe_resource  :     {   token  :     string     }  ;         days_back  ?  :     number  ;         limit  ?  :     number  ;         }  )     =>     Promise  <  FailedPaymentsResult  >  ;      }  ;
```


Your frontend gets autocomplete, type checking, and compile-time safety when calling runnables. No manual type definitions, no API contracts to maintain.


Beyond synchronous calls, the API supports async patterns for long-running tasks:


```text
import     {   backendAsync  ,   waitJob  ,   streamJob   }     from     "./wmill.ts"  ;         // Start a long-running task, get a job ID immediately      const   jobId   =     await   backendAsync  .  generate_report  (  {   query  :     "SELECT *"     }  )  ;         // Wait for completion      const   result   =     await     waitJob  (  jobId  )  ;         // Or stream results as they come      const   finalResult   =     await     streamJob  (  jobId  ,     (  update  )     =>     {         console  .  log  (  "Streaming chunk:"  ,   update  .  new_result_stream  )  ;      }  )  ;
```


## Example: AI chatbot with Windmill scripts as tools[Open in new tab ↗](https://app.windmill.dev/public/windmill-labs/928711b4b9ac223354f283212d5e7594)


- Full-code frontend
- Windmill backend runnables


- AI Agent flow
- Marketing activation
- Sales metrics


The orchestration flow that receives a user message, calls an AI model with registered tools, and streams the response back.


A script the AI agent can call to look up marketing activation live data from connected sources.


A script the AI agent can call to query live sales metrics from the database.


This app is a full-code app built entirely on Windmill. The frontend is a React chat interface. Behind it, a backend flow calls an AI model and passes Windmill scripts as callable tools.


When a user asks a question, the flow is:


1. The React frontend sends the message to a backend runnable via the typed API.
2. The backend flow calls an AI model with a set of Windmill scripts registered as tools.
3. The AI agent decides which tools to call (e.g. querying sales metrics, looking up marketing activations) and returns a response.
4. The result streams back to the frontend.


The entire app, frontend, backend logic, and AI tools, lives in a single Windmill workspace. No external API layer, no separate deployment for the chat UI.


## Build full-code apps with Claude Code and Codex​


The Windmill CLI auto-generates` AGENTS.md` and` DATATABLES.md` context files so AI coding assistants understand your project structure, backend runnables, typed API, and data schema out of the box.


With[Claude Code](https://docs.anthropic.com/en/docs/claude-code) or[Codex](https://openai.com/index/introducing-codex/) , you can prompt the app you want to build and get a working full-stack application: frontend, backend scripts, and typed connections between them. The AI agent knows how to create runnables in` backend/` , call them from your React or Svelte frontend via the typed` wmill.ts` API, and preview the result locally.


Prompting Claude Code to build a full-code app from scratch.


### Or build from the web UI​


You can also build full-code apps directly from the Windmill UI with your own AI API key. Write your frontend and backend runnables, preview instantly, and deploy without leaving the browser.


Building a full-code app from the Windmill UI with AI assistance.


## Why we built it this way​


Three design choices drove the architecture:


**Any language on the backend.** Your frontend is React or Svelte. Your backend can be anything: TypeScript, Python, SQL (PostgreSQL, MySQL, BigQuery, Snowflake, DuckDB), Go, Bash, Rust, PHP, Java, Ruby, C#, and more. Each runnable is a file in` backend/` with the language inferred from the extension. A single app can mix Python data processing with TypeScript API calls and SQL queries.


**Local development first.**` wmill app dev` starts a local dev server with hot module replacement. You use your own editor, your own tools, your own npm packages. The workflow is standard:` npm install` , write code, see changes instantly.


**No separate API layer.** The frontend calls backend runnables directly through Windmill's execution engine via WebSocket. No REST endpoints to define, no API gateway to configure, no OpenAPI specs to maintain. You write a function in` backend/` , and it appears as a typed call in your frontend.


## Example: project structure​


Here is what a typical full-code app looks like on disk:


```text
my_app.raw_app/    ├── frontend/    │   ├── App.tsx                # Main React component    │   ├── index.css    │   ├── index.tsx              # Entry point    │   ├── package.json    │   └── wmill.ts              # Auto-generated typed API to call backend runnables    └── backend/        ├── sendAiMessage          # Send message to AI agent        ├── getSalesMetrics        # Get sales metrics        └── getMarketingActivations # Get marketing activations
```


Each file in` backend/` becomes a callable runnable. The language is detected from the extension:` .ts` for TypeScript,` .py` for Python,` .pg.sql` for PostgreSQL, and so on. No YAML configuration is needed for simple runnables.


## Full-stack apps on one platform​


Full-code apps are designed to work with the rest of Windmill. Combined with[Data Tables](https://www.windmill.dev/blog/launch-week-data-tables-ducklake) and backend runnables, you get a complete full-stack setup with no external infrastructure:


- **Frontend** : React or Svelte, served by Windmill.
- **Backend** : scripts in 20+ languages, each running on isolated workers with full CPU and memory.
- **Data** :[Data Tables](https://www.windmill.dev/docs/core_concepts/persistent_storage/data_tables) for relational storage,[Ducklake](https://www.windmill.dev/docs/core_concepts/persistent_storage/ducklake) for analytics. No connection strings to manage.


Backend runnables reading and writing to Data Tables from a full-code app.


Your frontend calls backend runnables via the typed API, and those runnables read and write to Data Tables directly. Windmill handles execution, authentication, hosting, and monitoring.


## Getting started​


1. Install or update the[Windmill CLI](https://www.windmill.dev/docs/advanced/cli) .
2. Scaffold a new app:


```text
wmill app new
```


1. Install dependencies and start the dev server:


```text
cd   f/folder/my_app.raw_app     npm     install     wmill app dev
```


1. Deploy to Windmill:


```text
wmill   sync   push
```


You can also create full-code apps directly from the Windmill UI by clicking "+ App" and selecting "Full-code App".


[Full-code apps Build custom frontends with React or Svelte connected to backend runnables.](https://www.windmill.dev/docs/full_code_apps)[Full-code apps quickstart Step-by-step guide to build your first full-code app.](https://www.windmill.dev/docs/getting_started/full_code_apps_quickstart)[Internal tools use case See examples of full-stack apps built on Windmill.](https://www.windmill.dev/use-cases/internal-apps)


## What's next​


Tomorrow is Day 2: **Data Tables & Ducklake** . Store and query relational data with managed SQL and an S3-backed data lakehouse.[Follow along](https://www.windmill.dev/launch-week-march-2026) .


[Windmill](https://www.windmill.dev/) is an[open-source](https://github.com/windmill-labs/windmill) and[self-hostable](https://www.windmill.dev/docs/advanced/self_host/) developer platform to build, orchestrate, and monitor internal tools and data pipelines, combining the power of code with the velocity of low-code. We turn your scripts into internal apps and composable steps of flows that automate repetitive workflows.


You can[self-host](https://www.windmill.dev/docs/advanced/self_host/) Windmill using a` docker compose up` , or go with the[cloud app](https://app.windmill.dev/user/login) .
