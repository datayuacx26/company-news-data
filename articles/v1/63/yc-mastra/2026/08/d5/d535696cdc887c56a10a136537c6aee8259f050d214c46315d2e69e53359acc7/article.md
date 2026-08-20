---
schema_version: "1.0.0"
document_id: "d535696cdc887c56a10a136537c6aee8259f050d214c46315d2e69e53359acc7"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-2c9844a44afc"
canonical_url: "https://mastra.ai/blog/introducing-ephemeral-sandbox-deploys"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-06T03:45:54.315491+00:00"
fetched_at: "2026-08-06T03:45:55.607657+00:00"
content_hash: "sha256:b0a8b437fa4ff1dceca0b93747b786daa5451b6231384fc97e15561580f59023"
---

# Introducing Ephemeral Sandbox Deployments for Mastra Server and Studio

You can now deploy your Mastra project (Server and Studio) to an[ephemeral sandbox](https://mastra.ai/docs/deployment/sandbox) — manually or programmatically, with configurable runtime limits. Share previews with teammates, let agents deploy autonomously to verify results, or run per-user instances isolated from your infrastructure.


A sandbox deploy gives you two public URLs to test your application. Open Studio to chat with your agents, or hit` /api` to test the endpoints directly.


We're starting today with[E2B](https://mastra.ai/reference/workspace/e2b-sandbox) ,[Daytona](https://mastra.ai/reference/workspace/daytona-sandbox) , and[Vercel Sandbox](https://mastra.ai/reference/workspace/vercel-sandbox) , with more providers to follow.


Your browser does not support the video tag.


Before sandbox deployments, hosting a Mastra project meant working through the full development lifecycle — create a PR, push a deploy, share a link. Now you can skip the pipeline and deploy straight to a sandbox. Subsequent deployments reconnect to the existing sandbox without reinstalling dependencies.


Each deployment writes a` sandbox-deployment.json` manifest with sandbox` url` ,` id` , and` expiresAt` , so CI can smoke-test the deploy.


## Get started


Install the deployer and a sandbox provider. The examples below use[E2B](https://e2b.dev/) :


Terminal


```text
npm   install   @mastra/deployer-sandbox   @mastra/e2b
```


note


Requires` @mastra/core@1.52.0` or later, added in[PR #19577](https://github.com/mastra-ai/mastra/pull/19577) .


Configure` SandboxDeployer` on the Mastra instance. The` id` identifies the sandbox — repeat deploys with the same` id` reconnect to the existing one instead of provisioning a fresh sandbox:


src/mastra/index.ts


```text
import   {   Mastra   }   from   "  @mastra/core/mastra  "  ;
import   {   SandboxDeployer   }   from   "  @mastra/deployer-sandbox  "  ;
import   {   E2BSandbox   }   from   "  @mastra/e2b  "  ;


export   const   mastra   =   new   Mastra  ({
// ...
deployer:   new   SandboxDeployer  ({
sandbox:   new   E2BSandbox  ({
id:   "  mastra-sandbox-deploy  "  ,
template:   "  base  "  ,
timeout:   3_600_000   // 1 hour
})
})
});
```


## Manually deploy


Add your E2B API key to` .env` :


.env


```text
E2B_API_KEY  =  e2b_...
```


` mastra build` doesn't auto-load` .env` , and the deployer needs your provider credentials at build time. Install` dotenv-cli` and wire up a` deploy` script that loads` .env` before running the build:


Terminal


```text
npm   install   -D   dotenv-cli
```


package.json


```text
{
"  scripts  "  :   {
"  deploy  "  :   "  dotenv -e .env -- mastra build  "
}
}
```


Then build and deploy in one command:


Terminal


```text
npm   run   deploy
```


Each deploy outputs two URLs to the terminal and writes a manifest into` .mastra/output` :


1. API endpoints:[https://4111-eijrptonwfyfew4q7cnl6.e2b.app/api](https://4111-eijrptonwfyfew4q7cnl6.e2b.app/api)
2. Studio:[https://4111-eijrptonwfyfew4q7cnl6.e2b.app](https://4111-eijrptonwfyfew4q7cnl6.e2b.app/)


.mastra/output/sandbox-deployment.json


```text
{
"  provider  "  :   "  e2b  "  ,
"  sandboxId  "  :   "  mastra-sandbox-deploy  "  ,
"  url  "  :   "  https://4111-eijrptonwfyfew4q7cnl6.e2b.app  "  ,
"  port  "  :   4111  ,
"  deployedAt  "  :   "  2026-08-04T12:46:50.675Z  "  ,
"  expiresAt  "  :   "  2026-08-04T13:46:50.675Z  "
}
```


Hit API endpoints by appending` /api` to the sandbox` url` :


Terminal


```text
curl   -s   -X   POST   <  sandbox-ur  l  >  /api/agents/weatherAgent/generate   \
-H   "  Content-Type: application/json  "   \
-d   '  {"messages":[{"role":"user","content":"Weather in London"}]}  '   \
|   jq   -r   '  .text  '
```


## Programmatically deploy


From CI, agent code, or your own scripts, use` deployToSandbox()` to deploy. By default this is API only — pass` studio: true` to include Studio:


```text
import   {   deployToSandbox   }   from   "  @mastra/deployer-sandbox  "  ;
import   {   E2BSandbox   }   from   "  @mastra/e2b  "  ;


const   deployment   =   await   deployToSandbox  ({
sandbox:   new   E2BSandbox  ({
id:   "  ci-smoke  "  ,
template:   "  base  "  ,
timeout:   3_600_000   // 1 hour
}),
dir:   "  .mastra/output  "  ,
studio:   true   // optional
});
```


Use` deployment.stop()` , and` deployment.destroy()` for further control over the sandbox lifecycle.


For more information and full configuration options, see:


- [Deploy to a sandbox](https://mastra.ai/docs/deployment/sandbox)
- [E2BSandbox reference](https://mastra.ai/reference/workspace/e2b-sandbox)
- [DaytonaSandbox reference](https://mastra.ai/reference/workspace/daytona-sandbox)
- [VercelSandbox reference](https://mastra.ai/reference/workspace/vercel-sandbox)
