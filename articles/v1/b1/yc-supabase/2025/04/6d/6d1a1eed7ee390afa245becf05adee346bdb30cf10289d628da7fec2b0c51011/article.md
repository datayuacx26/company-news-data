---
schema_version: "1.0.0"
document_id: "6d1a1eed7ee390afa245becf05adee346bdb30cf10289d628da7fec2b0c51011"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-edge-functions-deploy-dashboard-deno-2-1"
published_at: "2025-04-01T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:4a926f6c2e5ccfd45b17ae37988fa5b39861f151010f506965c906786668cc8d"
---

# Edge Functions: Deploy from the Dashboard + Deno 2.1

Now you can create, test, edit, and deploy Edge Functions directly from the Supabase Dashboard. We're also releasing Deno 2.1 Preview today but more on that later.


## Creating Edge Functions from the Supabase Dashboard#


To write an Edge Functions previously, you had to install the Supabase CLI, spin up Docker, and then set up your editor to use Deno. Those steps are no longer necessary. The Edge Functions editor in the Dashboard has built-in syntax highlighting and type-checking for Deno and Supabase-specific APIs.


The Edge Functions editor includes templates for common use cases, such as Stripe WebHooks, OpenAI proxying, uploading files to Supabase Storage, and sending emails.


Once a Function has been deployed you can make edits directly within the Dashboard, and if you get stuck you can summon an inline AI Assistant to explain, debug or write code.


## Downloading Edge Functions#


You can download Edge Functions source code via Supabase CLI using` supabase functions download FUNCTION_NAME` or by clicking the Download button in the dashboard.


The Dashboard's Edge Function editor currently does not support versioning or rollbacks. We recommend using it only for quick testing and prototypes. When you’re ready to go to production, store Edge Functions code in a source code repository (e.g. git) and deploy it using one of the[CI integrations](https://supabase.com/docs/guides/functions/cicd-workflow) .


## Testing Edge Functions from the Supabase Dashboard#


We are introducing a built-in tool for testing your Edge Functions from the Supabase Dashboard. You can execute your Edge Function with different request payloads, headers, and query parameters. The built-in tester returns the response status, headers, and body.


With the built-in editor and tester, you have a streamlined workflow for creating, testing, and refactoring your Edge Functions without leaving the Supabase Dashboard.


## Deploying Edge Functions no longer requires Docker#


By popular request, you can now deploy Edge Functions from the Supabase CLI with the` --use-api` flag, which will not use Docker. We will make this the default behavior in future releases (with a` --use-docker` flag as a fallback option.)


`
_10


supabase functions deploy MY_FUNCTION --use-api


`


## New APIs for Deploying Edge Functions#


The ability to deploy without Docker in both the Edge Functions editor and Supabase CLI are made possible by new APIs we introduced to deploy and update Edge Functions. These APIs are publicly available for you to build custom integrations and workflows.


You can check[the Changelog announcement](https://github.com/orgs/supabase/discussions/33720) for more details and official references to these API endpoints.


## Deno 2.1 support#


Last, but not least, we have added Deno 2.1 support for Supabase Edge Runtime. With Deno 2.1, you can use built-in Deno commands to scaffold a new project, manage dependencies, run tests, and lints.


Check[our guide on how to start using Deno 2.1](https://github.com/orgs/supabase/discussions/34054) tooling for your Edge Functions.


## Conclusion#


These changes to Supabase Edge Functions make it easier and more accessible for all developers to build powerful functionality into their applications.


- Read the[Edge Functions documentation](https://supabase.com/docs/guides/functions) to learn more
- [Sign up for Supabase](https://supabase.com/dashboard/sign-up) and get started today
