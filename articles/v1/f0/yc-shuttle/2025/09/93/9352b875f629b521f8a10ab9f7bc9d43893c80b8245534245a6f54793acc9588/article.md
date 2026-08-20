---
schema_version: "1.0.0"
document_id: "9352b875f629b521f8a10ab9f7bc9d43893c80b8245534245a6f54793acc9588"
company_key: "yc-shuttle"
company: "Shuttle"
source_id: "yc-shuttle-rss-52efc69d7cac"
canonical_url: "https://www.shuttle.dev/blog/2025/09/11/migrate-to-shuttle"
published_at: "2025-09-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:24:10.103156+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:68a931b205f3ba73a943584e2efd0c447dcafd2606b6951673130d7143a47b83"
---

# How to Migrate to Shuttle Using Cursor and the Shuttle MCP Server

We've made migration to Shuttle very easy by leveraging our Shuttle MCP server and Cursor. The Shuttle MCP provides up to date documentation to your AI agents so that they'll be able to understand how to interact with the Shuttle platform having the latest information.


In this short tutorial, we'll walk through migrating an existing Axum todo application to Shuttle and we'll deploy the application to Shuttle, we'll do all of that in just a few minutes.


The final migrated project is available at[todo-app example](https://github.com/shuttle-hq/shuttle-examples/tree/main/axum/todo-app) .


So, let's get started.


## Prerequisites #


You'll need to have a Shuttle account, so if you don't have one, make sure to[create an account](https://console.shuttle.dev/) it takes less than a minute.


After signing up, you'll need to install the Shuttle CLI:


```text
# Install via the official installer (recommended)
curl    -sSfL   https://www.shuttle.dev/install  |    bash


shuttle  --version


```


> If you already have shuttle installed, make sure you update it by running` shuttle upgrade` .


## Login to Shuttle #


Once the CLI is installed, login to your Shuttle account:


```text
shuttle login


```


This command will redirect you to the Shuttle console in your browser. Click "Authorize" to login.


Shuttle Authorize Page


## Installing the Shuttle MCP Server in Cursor #


Installing the Shuttle MCP Server is quite easy to do, you can add it just by clicking the "Add to Cursor" button below.


If not using Cursor no problem, you can manually update your` mcp.json` file to add it to your MCP client. Example:


```text
{
"mcpServers"  :    {
"Shuttle"  :    {
"command"  :    "shuttle"  ,
"args"  :    [  "mcp"  ,    "start"  ]
}
}
}


```


Verify Installation


The green status indicates that the MCP server is working and all tools ready for your AI agent to use.


## Migrating Your Project #


Now comes the interesting part - with this approach, we're gonna make it very easy to migrate to Shuttle. We've already put together a prompt that will guide your AI agent to migrate your existing Axum project to Shuttle. Along with the MCP server, your AI agent will have every bit of information it needs to help you migrate your project.


```text
Convert this existing Rust web application to run on Shuttle using the platform's features:


##   Instructions


**  CRITICAL: Search Shuttle MCP docs before each task. Pattern: search docs → do task → search docs → do task  **


**  Always use the latest version of Shuttle dependencies  **


**  Do not modify versions of existing non-Shuttle dependencies - only add/update Shuttle-specific dependencies  **


1.    **  Analyze codebase  **    - Examine  `main.rs`  ,  `Cargo.toml`  , configs
2.    **  Update dependencies  **    - Update Cargo.toml
3.    **  Convert main function  **    - Transform to use Shuttle runtime
4.    **  Database integration  **    - If PostgreSQL is used, search "postgres" using the MCP, then migrate to Shuttle's managed Postgres
5.    **  Manage secrets  **    - If environment variables are used, create Secrets.toml with placeholder keys
6.    **  Auto migrations  **    - If database is used, ensure migrations run on startup
7.    **  Create Shuttle.toml  **    - Create if app serves static assets:


```toml
[build]
assets = ["static/ *  ", "public/  *   "]
```


8.    **  Configure tracing  **    - Remove existing tracing init (keep macros/spans)
9.    **  Add secrets macro  **    - Use  `#[shuttle_runtime::Secrets]`
10.    **  Add DB macro  **    - Use  `#[shuttle_shared_db::Postgres]`
11.    **  Update .gitignore  **    - Add:


```gitignore
/target
.shuttle*
Secrets*.toml
```


12.    **  Test locally  **    - Test if compiles with  `shuttle run`
13.    **  Create project  **    - Run  `shuttle project create --name <project-name>`   and  `shuttle project link --id <project_id>`


##   Requirements


-   Preserve all functionality and API behavior
-   Make minimal changes - focus on entry point and infrastructure only
-   Use  `shuttle_runtime::main`
-   If PostgreSQL is used: add  `shuttle_shared_db::Postgres`   and  `sqlx::migrate!()`
-   If environment variables are used: add  `shuttle_runtime::Secrets`
-   Remove tracing init (keep macros)
-   Ensure works with  `shuttle run`   and in production


##   Next Steps After Migration


After completing the migration, inform about these remaining manual steps:


1.    **  Fill in actual secret values  **    in  `Secrets.toml`   if created (only placeholders can be created)
2.    **  Review all changes  **    made during the migration for production readiness
3.    **  Deploy to production  **    - request deployment after secrets are set and changes are reviewed


```


Paste the prompt to your AI agent and let it do the work for you.


Shuttle search tool call


As you can see, the AI agent is using the Shuttle MCP server to search the documentation and get the latest information about the Shuttle platform.


## Key Migration Changes #


The main transformation involves updating your` main` function from a standard Tokio setup to Shuttle's runtime.


```text
#[shuttle_runtime::main]
async    fn    main  (
#[shuttle_shared_db::Postgres]   pool :    sqlx ::   PgPool  ,
)    ->    shuttle_axum ::   ShuttleAxum    {
...
}


```


The` shuttle_runtime::main` make the project work with Shuttle's runtime and the` shuttle_shared_db::Postgres` macro is used to provision a PostgreSQL database in production.


**Shuttle Dependencies:**


Some Shuttle dependencies are added to the` Cargo.toml` file:


```text
[  dependencies  ]
shuttle-runtime    =    "0.56.0"
shuttle-shared-db    =    {    version    =    "0.56.0"  ,    features    =    [  "postgres"  ,    "sqlx"  ]    }
shuttle-axum    =    "0.56.0"


```


- ` shuttle-runtime` - Shuttle's runtime
- ` shuttle-shared-db` - To provision a PostgreSQL database in production and Docker for development,[read more about how Shuttle shared db works](https://docs.shuttle.dev/resources/shuttle-shared-db)
- ` shuttle-axum` - Shuttle Axum dependencies, we'll use this to create the API router


**Shuttle.toml:**


```text
[  build  ]
assets    =    [  "static/*"  ]


```


A` Shuttle.toml` is required for Shuttle to know about the static assets that need to be uploaded.


## Deploying to Shuttle #


Once your project is migrated, the AI agent will attempt to create and deploy a project for you. If it didn't do it for you, you can ask it again to do it.


```text
Deploy the project to Shuttle


```


Shuttle deploy


Cursor will call the` deploy` tool to deploy the project to Shuttle and Shuttle will start building your project.


Shuttle building


After building is done, you can see the project URL in your Shuttle console and visit the website.


Shuttle console


Opening the project URL in the browser, you can see the website is working.


Todo app hosted


Perfect! 🎉 We've successfully migrated our Axum todo application to Shuttle and deployed it to production.


## Next Steps #


Shuttle has more features and we're always adding more and improving the platform, you can check out the[Shuttle documentation](https://docs.shuttle.dev/) for more information.


Clone the todo app example by running the following command and start building your own project:


```text
shuttle init  --from   shuttle-hq/shuttle-examples  --subfolder   axum/todo-app


```


#### Join the Shuttle Discord Community


Connect with other developers, learn, get help, and share your projects


[Join](https://discord.gg/shuttle)
