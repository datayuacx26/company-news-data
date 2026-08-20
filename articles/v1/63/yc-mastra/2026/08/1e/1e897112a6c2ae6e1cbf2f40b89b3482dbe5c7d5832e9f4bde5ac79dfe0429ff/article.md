---
schema_version: "1.0.0"
document_id: "1e897112a6c2ae6e1cbf2f40b89b3482dbe5c7d5832e9f4bde5ac79dfe0429ff"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-2c9844a44afc"
canonical_url: "https://mastra.ai/blog/introducing-fine-grained-authorization"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-19T17:05:29.378927+00:00"
fetched_at: "2026-08-19T17:05:29.948532+00:00"
content_hash: "sha256:87a0eae27f93fb156d8bfae39af2765f4e87d379c761b1fe5b3f13e26887b972"
---

# Introducing Fine-Grained Authorization for Mastra

Mastra now supports[Fine-Grained Authorization](https://mastra.ai/docs/auth/fga) (FGA) to precisely manage permissions for authenticated users and the resources they can access. Configure FGA on your Mastra instance to gate every part of the runtime: HTTP routes, agent` .stream()` and` .generate()` , workflow runs, tool calls, memory reads and writes, and Mastra-hosted MCP servers.


Your browser does not support the video tag.


Mastra had role-based access control (RBAC) since March 2026 ([PR #13163](https://github.com/mastra-ai/mastra/pull/13163) ) for applying coarse permission patterns to agent calls or memory reads. But restrictions applied by role often aren't enough. Whilst RBAC handles user vs admin cleanly, it's not granular enough to fine-tune what specific users can do with specific resources.


FGA lets you refine permissions per-user and per-resource, giving you much more control over what each user can see and do.


You can enable FGA in one of two ways:


- **WorkOS** : Configure roles in the WorkOS dashboard and use the[MastraFGAWorkos](https://mastra.ai/reference/auth/workos) provider to apply them to your Mastra server.
- **Other auth providers** : Implement the[IFGAProvider](https://mastra.ai/reference/auth/fga) interface — a small vendor-agnostic surface you can point at any authorization backend.


Mastra Studio has its own separate provider config:


- **` server.fga`** : Gates your API endpoints so each user only sees their own resources.
- **` studio.fga`** : Gates Studio access separately, so only internal team members can access Studio.


For example, with WorkOS, the Studio check is typically an internal-org membership check allowing users/admins in your company's org Studio and API access, while non-org users only get API access.


FGA is part of Mastra's Enterprise Edition (EE), but you can test it out locally without a license. Production deployments require a valid EE license.[Contact sales](https://mastra.ai/contact) for more information.


## Get started


The example below uses` MastraAuthWorkos` for authentication. Regular users get API access scoped to specific resources. Admins get both API and Studio access.


Install the WorkOS auth package:


Terminal


```text
npm   install   @mastra/auth-workos
```


note


Requires` @mastra/core@1.32.0` or later, added in[PR #15410](https://github.com/mastra-ai/mastra/pull/15410) .


Add` MastraAuthWorkos` to your server's auth:


Configure` server.fga` to grant users permission to call specific agents by resource ID.` resourceMapping` treats each agent as its own WorkOS resource, and` permissionMapping` matches Mastra's` AGENTS_EXECUTE` to the WorkOS` execute` permissions.


Configure` studio.fga` for Studio access, scoped to your` organizationId` granting only org members access.


src/mastra/index.ts


```text
import   {   Mastra   }   from   "  @mastra/core  "  ;
import   {   MastraFGAPermissions   }   from   "  @mastra/core/auth/ee  "  ;
import   {   MastraAuthWorkos,   MastraFGAWorkos   }   from   "  @mastra/auth-workos  "  ;


export   const   mastra   =   new   Mastra  ({
server: {
auth:   new   MastraAuthWorkos  ({
apiKey: process.env.WORKOS_API_KEY,
clientId: process.env.WORKOS_CLIENT_ID,
redirectUri: process.env.WORKOS_REDIRECT_URI,
fetchMemberships:   true
}),
fga:   new   MastraFGAWorkos  ({
resourceMapping: {
agent: { fgaResourceType:   "  agent  "  ,   deriveId  : ({ resourceId })   =>   resourceId }
},
permissionMapping: {
[MastraFGAPermissions.AGENTS_EXECUTE]:   "  execute  "
}
})
},
studio: {
fga:   new   MastraFGAWorkos  ({
organizationId: process.env.WORKOS_ADMIN_ORG_ID
})
}
});
```


For more information and full configuration options, see:


- [Fine-grained authorization](https://mastra.ai/docs/auth/fga)
- [MastraFGAWorkos](https://mastra.ai/reference/auth/workos)
- [IFGAProvider](https://mastra.ai/reference/auth/fga)
- [WorkOS](https://workos.com/)
