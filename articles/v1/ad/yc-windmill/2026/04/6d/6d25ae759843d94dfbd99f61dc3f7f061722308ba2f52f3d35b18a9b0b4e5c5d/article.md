---
schema_version: "1.0.0"
document_id: "6d25ae759843d94dfbd99f61dc3f7f061722308ba2f52f3d35b18a9b0b4e5c5d"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-12b6d71fe86e"
canonical_url: "https://www.windmill.dev/blog/discord-bot-stripe-sandboxed-ai"
published_at: "2026-04-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:07.070377+00:00"
fetched_at: "2026-07-28T22:15:40.956663+00:00"
content_hash: "sha256:25bd884d87ea8efe534adb967772f7145c54af720cb79767b1e69376bc6cb79b"
---

# Sandboxed AI bots: give capabilities, not credentials

The[Vercel security incident](https://vercel.com/kb/bulletin/vercel-april-2026-security-incident) earlier this month leaked customer environment variables, including API keys for payment processors and databases. A month before that,[compromised LiteLLM packages on PyPI](https://docs.litellm.ai/blog/security-update-march-2026) stole AWS, GCP, and SSH credentials from developer machines. Secrets stored as env vars or in plaintext keep ending up in the wrong hands.


If you're building AI agents that call sensitive APIs, the question is: does the agent actually need to see the API key, or just the ability to call the API?


## Example: "Did Acme pay?"​


A Discord bot where your team asks billing questions and gets answers from Stripe:


The bot runs as a single Windmill script connected to Discord via a[WebSocket trigger](https://www.windmill.dev/docs/triggers/websocket_triggers) . The agent has two tools:


```text
import     {   query  ,   tool  ,   createSdkMcpServer   }     from     "@anthropic-ai/claude-agent-sdk"  ;         const   toolServer   =     createSdkMcpServer  (  {       name  :     "billing-tools"  ,       tools  :     [           tool  (             "search_invoices"  ,             "Search Stripe invoices by customer email"  ,             {   email  :   z  .  string  (  )  .  email  (  )     }  ,             async     (  args  )     =>     {               // Secret fetched server-side, the agent never sees this key               const   stripe   =     await   wmill  .  getResource  (  "f/bot/stripe"  )  ;               const   resp   =     await     fetch  (                 `  https://api.stripe.com/v1/invoices/search?query=customer_email:'  ${  encodeURIComponent  (  args  .  email  )  }  '  `  ,                 {   headers  :     {   Authorization  :     `  Bearer   ${  stripe  .  apiKey  }  `     }     }               )  ;               const   data   =     await   resp  .  json  (  )  ;               return     {   content  :     [  {   type  :     "text"  ,   text  :     JSON  .  stringify  (  data  .  data  ?.  slice  (  0  ,     5  )  ,     null  ,     2  )     }  ]     }  ;             }           )  ,           tool  (             "get_customer"  ,             "Look up a Stripe customer by email or name"  ,             {   query  :   z  .  string  (  )  .  describe  (  "Customer name or email"  )     }  ,             async     (  args  )     =>     {               const   stripe   =     await   wmill  .  getResource  (  "f/bot/stripe"  )  ;               const   resp   =     await     fetch  (                 `  https://api.stripe.com/v1/customers/search?query=name~'  ${  encodeURIComponent  (  args  .  query  )  }  ' OR email~'  ${  encodeURIComponent  (  args  .  query  )  }  '  `  ,                 {   headers  :     {   Authorization  :     `  Bearer   ${  stripe  .  apiKey  }  `     }     }               )  ;               const   data   =     await   resp  .  json  (  )  ;               return     {   content  :     [  {   type  :     "text"  ,   text  :     JSON  .  stringify  (  data  .  data  ?.  slice  (  0  ,     5  )  ,     null  ,     2  )     }  ]     }  ;             }           )  ,         ]  ,      }  )  ;
```


Someone asks "did Acme pay?" and the agent calls` get_customer` , then` search_invoices` , then responds in plain English. It decides which tools to call based on the question, but it never sees the Stripe secret key.


## How credentials stay isolated​


The tool implementation fetches secrets from[Windmill resources](https://www.windmill.dev/docs/core_concepts/resources_and_types) at runtime. The agent only sees the tool signature ("give me an email, I'll return invoice data"). The credential itself lives in Windmill's encrypted resource store and is injected inside the tool function, not passed to the agent.


In the typical setup,` STRIPE_SECRET_KEY` sits in an env var and the whole process has access to it. In Windmill,` process.env` in the agent's sandbox only contains runtime variables like workspace ID and job ID. No secrets.


## The sandbox​


Windmill's[AI sandboxes](https://www.windmill.dev/blog/launch-week-ai-sandboxes) (introduced during[launch week](https://www.windmill.dev/launch-week-march-2026) ) use[nsjail](https://www.windmill.dev/docs/advanced/security_isolation#nsjail-sandboxing) for process-level isolation:


- **Credential isolation** : API keys live in the encrypted[resource store](https://www.windmill.dev/docs/core_concepts/resources_and_types) , not in the agent's environment. Windmill supports[multiple secret storage backends](https://www.windmill.dev/docs/core_concepts/variables_and_secrets) : its own encrypted store, HashiCorp Vault, Azure Key Vault, and AWS Secrets Manager. Either way, the agent never sees them.
- **Filesystem isolation** : scripts run in an isolated mount namespace with access only to their job directory and explicitly mounted[volumes](https://www.windmill.dev/docs/core_concepts/volumes) .
- **Resource limits** : CPU, memory, and process limits per job.
- **Network isolation** (optional): outbound network access is on by default (tools need it to call APIs). For stricter setups, you can enable network namespace isolation via[unshare flags](https://www.windmill.dev/docs/advanced/security_isolation#pid-namespace-isolation) (` --net` ) to block all outbound connections.


Even without network isolation, the agent can't authenticate to Stripe or any other service because it doesn't have the key. It can only use what you explicitly wire up as tools.


## Try it​


Full setup (Discord Gateway connection, heartbeat config, handler script with tool use):


[Build an AI Discord bot Full guide: WebSocket trigger setup, heartbeat configuration, and handler script with tool use.](https://www.windmill.dev/docs/misc/guides/discord_bot)[WebSocket triggers Connect to WebSocket servers and trigger scripts on incoming messages.](https://www.windmill.dev/docs/triggers/websocket_triggers)


[AI sandbox How Windmill isolates AI agents from credentials and the host environment.](https://www.windmill.dev/docs/core_concepts/ai_sandbox)[Security and isolation nsjail, PID namespaces, and network isolation options.](https://www.windmill.dev/docs/advanced/security_isolation)


To add Stripe tools: create a` stripe` resource in your workspace with your API key, define the tools in your handler script, and deploy.


[Windmill](https://www.windmill.dev/) is an[open-source](https://github.com/windmill-labs/windmill) and[self-hostable](https://www.windmill.dev/docs/advanced/self_host/) developer platform to build, orchestrate, and monitor internal tools and data pipelines, combining the power of code with the velocity of low-code. We turn your scripts into internal apps and composable steps of flows that automate repetitive workflows.


You can[self-host](https://www.windmill.dev/docs/advanced/self_host/) Windmill using a` docker compose up` , or go with the[cloud app](https://app.windmill.dev/user/login) .
