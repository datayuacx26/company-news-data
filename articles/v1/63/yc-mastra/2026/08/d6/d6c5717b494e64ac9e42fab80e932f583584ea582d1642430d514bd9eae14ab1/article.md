---
schema_version: "1.0.0"
document_id: "d6c5717b494e64ac9e42fab80e932f583584ea582d1642430d514bd9eae14ab1"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-2c9844a44afc"
canonical_url: "https://mastra.ai/blog/introducing-managed-sandboxes-and-filesystems-for-mastra-platform"
published_at: "2026-08-03T00:00:00+00:00"
first_seen_at: "2026-08-03T21:25:28.544489+00:00"
fetched_at: "2026-08-03T21:33:45.218976+00:00"
content_hash: "sha256:4fc48003a52dfbec723f13916baaedcd9f763de5d1ab8e951919380587a5e6ae"
---

# Introducing Managed Sandboxes and Filesystems for Mastra Platform

Some of the most capable agents being built today share a common trait: they have a computer to work with. That computer gives them a place to write files, run code, and leave behind work that outlasts a single conversation. While Mastra already supports connecting to any number of hosted sandbox providers, it's always meant a good deal of setup work to get up and running.


Now, with[sandboxes and filesystems](https://mastra.ai/docs/mastra-platform/workspace) on the Mastra platform, every environment in your project can get a bucket for persistent file storage and a sandbox for running commands, provisioned for you.


Your browser does not support the video tag.


Using the new` @mastra/platform-workspace` SDK, you can pull in both the` PlatformFilesystem` and` PlatformSandbox` primitives to put your filesystem and sandbox directly on the platform. The` PlatformFilesystem` acts as your agent's persistent filesystem: files your agent writes there (generated reports, working documents, code it's iterating on, etc.) survive across conversations, deploys, and restarts, so your agent can build a durable body of work instead of starting from scratch on every run. And the` PlatformSandbox` is where your agent safely executes the code it writes: commands run in an isolated environment away from your production server, and the platform keeps the sandbox base image warmed so the first call starts quickly.


The platform handles everything around the providers. Filesystems and sandboxes are provisioned per environment, so production and staging each get their own bucket and sandbox, and your staging agent's files stay out of production's. Credentials are injected at deploy time as environment variables, which means the provider doesn’t require options on the platform, you construct them, deploy, and they connect. Billing is straightforward as well: they share your project's CPU time and data egress, with no separate infrastructure bill to manage.


You can manage them all from the Workspaces tab in your project, which shows each environment's bucket contents with upload, download, and delete actions, along with recent sandbox sessions and their command, exit code, and duration.


The same providers also work locally so you can put your environment's variables in a` .env` file and` PlatformFilesystem` and` PlatformSandbox` connect to the same bucket and sandbox pool you run in the cloud.


Sandboxes are deployed in the same service provider as your Studio and Server, so all resources are automatically colocated for better performance and cost.


## Requirements


Managed filesystems and sandboxes are enabled by default on new platform projects. For existing projects, open the Workspaces tab and select **Enable workspaces** , which provisions a bucket for every environment on the project. Your code needs the` @mastra/platform-workspace` package, which works with your existing Workspace configuration from` @mastra/core` .


## Get started in the CLI


Install the provider package:


Terminal


```text
npm   install   @mastra/platform-workspace
```


Compose the providers into a workspace:


src/mastra/workspaces/workspace.ts


```text
import   {   Workspace,   LocalFilesystem,   LocalSandbox   }   from   "  @mastra/core/workspace  "  ;
import   {   PlatformFilesystem,   PlatformSandbox   }   from   "  @mastra/platform-workspace  "  ;


// platform providers when the injected credentials are present,
// local equivalents otherwise
const   onPlatform   =   process.env.MASTRA_PLATFORM_ACCESS_TOKEN;


export   const   workspace   =   new   Workspace  ({
filesystem: onPlatform
?   new   PlatformFilesystem  ()
:   new   LocalFilesystem  ({ basePath:   "  ./workspace  "   }),
sandbox: onPlatform
?   new   PlatformSandbox  ()
:   new   LocalSandbox  ({ workingDirectory:   "  ./workspace  "   }),
});
```


Register the workspace on your main` Mastra` instance:


```text
import   {   Mastra   }   from   "  @  mastra  /core  "  ;


import   {   workspace   }   from   "  ./workspaces/workspace  "  ;


export   const     mastra   =   new   Mastra  ({   workspace });
```


Deploy your project — the workspace ships with it:


Terminal


```text
mastra   deploy
```


Your agent now has file and command tools backed by managed infrastructure. Managed filesystems and sandboxes are in alpha, so expect the surface to evolve as we learn from real workloads (and tell us what your agents need next!).


For more information and full configuration options, read our platform docs on[workspaces](https://mastra.ai/docs/mastra-platform/workspace) and[environments](https://mastra.ai/docs/mastra-platform/environments) , and the[PlatformFilesystem](https://mastra.ai/reference/workspace/platform-filesystem) and[PlatformSandbox](https://mastra.ai/reference/workspace/platform-sandbox) references.
