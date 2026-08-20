---
schema_version: "1.0.0"
document_id: "a0532bf5b876343d7c4b8fe2714e8c049f7507e82efafb8b46f6ce57854b4011"
company_key: "yc-nango"
company: "Nango"
source_id: "yc-nango-news-import-e2ee807d6673"
canonical_url: "https://nango.dev/blog/build-a-github-api-integration-for-ai-agents/"
published_at: "2026-06-17T00:00:00+00:00"
first_seen_at: "2026-07-22T05:12:58.696716+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:f9f71f8e825d98d323ecbc8b99852c6d7cac224711961c5986c717c637a3e431"
---

# Build a GitHub API integration for AI agents using Nango

This guide shows how to build a custom, customer-facing GitHub API integration that the AI agents in your product can act on, using[Nango](https://nango.dev/) and an AI coding agent (Codex, Claude Code, Cursor, or any other).


By the end of this guide, you will have:


- The ability for an AI agent in your product to create issues, comment on pull requests, and open PRs on a customer’s repositories, exposed as typed[MCP tool calls](https://nango.dev/docs/guides/functions/tool-calling) instead of raw GitHub API parameters.
- A GitHub[auth UI](https://nango.dev/docs/guides/auth/auth-guide) in your product (Nango Connect) so your customers can connect their own GitHub account or install your GitHub App.
- A durable sync that imports issues and pull requests from a customer’s repositories and keeps them up to date, so the agent always reasons over current data.
- A signature-verified webhook handler that reacts to GitHub events (pushes, pull requests, issues) in real time.


> **Get the working example:** the complete demo (frontend, backend, nango-integrations) is on GitHub at[NangoHQ/github-api-integration](https://github.com/NangoHQ/github-api-integration) . Clone it to run the whole thing end to end, or follow the guide below and let Codex generate the same functions in your own project.


## What is a GitHub API integration?


A GitHub API integration connects your application or agent to a customer’s GitHub data (repositories, issues, pull requests, and commits) through GitHub’s REST and GraphQL APIs. It authenticates as a GitHub App or an OAuth App, reads data into your app with scheduled syncs, writes back with actions like creating issues or commenting on pull requests, and reacts to repository events through webhooks.


Example use cases: an AI agent that triages a customer’s issues and opens pull requests, an AI code-review product that comments on PRs, a project management tool that mirrors a customer’s issues, or a developer analytics dashboard.


## Why is it hard to integrate the GitHub API into your app?


The GitHub REST API exposes repositories, issues, pull requests, commits, and webhooks. A production integration on top of it takes a few decisions up front, plus the infrastructure to keep them running.


The first is the auth type. For a customer-facing product, a GitHub App is usually the right call; reach for an OAuth App only when you are building a personal-developer tool that should act purely as the user.


Capability GitHub App OAuth App


Acts as A bot/installation The authorizing user


Fine-grained permissions Yes, per resource No, inherits the user's scope


Rate limit 5,000 to 12,500/hour (scales with org) 5,000/hour


Repository access Only repos granted at install Every repo the user can access


Best for Customer-facing products Personal-developer tools


See[GitHub App vs. GitHub OAuth](https://nango.dev/blog/github-app-vs-github-oauth) and GitHub’s own[comparison](https://docs.github.com/en/apps/oauth-apps/building-oauth-apps/differences-between-github-apps-and-oauth-apps) for the full breakdown. The rest of the work is infrastructure:


- **The auth flow is not standard OAuth, and tokens are short-lived:** GitHub App installation tokens expire after one hour. When “Expire user authorization tokens” is enabled (the default), user access tokens[expire after 8 hours and refresh tokens after 6 months](https://docs.github.com/en/apps/creating-github-apps/authenticating-with-a-github-app/refreshing-user-access-tokens) . A failed refresh shows up as the[BAD_REFRESH_TOKEN](https://nango.dev/blog/github-app-oauth-refresh-token-bad-refresh-token) error, usually after a rotated client secret or a revoked installation. Someone has to store, refresh, and encrypt all of this.
- **The Issues API also returns pull requests:**` GET /repos/{owner}/{repo}/issues` returns both issues and PRs, and every pull request carries a[pull_request](https://docs.github.com/en/rest/issues/issues) field. Filter on it or you double-count.
- **Webhooks need signature verification:** GitHub signs each delivery with the[X-Hub-Signature-256](https://docs.github.com/en/webhooks/using-webhooks/validating-webhook-deliveries) header (an HMAC-SHA256 of the raw body using your webhook secret). You verify it with a constant-time comparison before trusting the payload, and read the event type from` X-GitHub-Event` .
- **Pagination is header-driven:** REST responses page through the` Link` header (` rel="next"` , up to 100 items per page), and some data is only practical through GitHub’s GraphQL API, which has its own point budget.


Building all of this by hand takes weeks. With Nango and a coding agent like Codex, the same GitHub API integration ships in about an hour, the same workflow we used to[build 200+ integrations across five APIs in 15 minutes](https://nango.dev/blog/learned-building-200-api-integrations-with-opencode) .


## Why use Nango for a GitHub API integration


[Nango](https://nango.dev/) is the integration platform where coding agents build API integrations. An agent like Codex writes the integration as code in your repo, and Nango’s runtime runs it with managed auth, retries, and observability across[900+ APIs](https://nango.dev/api-integrations) .


For a GitHub integration, we will use these Nango features:


- **Managed auth for GitHub Apps and OAuth:** your product gets a customizable, white-label[auth UI](https://nango.dev/docs/guides/auth/auth-guide) where customers install your[GitHub App](https://nango.dev/docs/api-integrations/github-app) or authorize a[GitHub OAuth](https://nango.dev/docs/api-integrations/github) app, while Nango handles installation and user tokens, refresh, encryption, and revocation behind it.
- **A function builder skill for Codex:** to build your integration logic, Codex uses the Nango[function builder skill](https://nango.dev/docs/guides/functions/functions-guide#option-2-build-locally-with-the-cli) . It researches the GitHub API, writes your actions and syncs from a prompt, tests them against a real GitHub connection, and iterates on real errors until its tests pass against your connection. The same skill works with Claude Code, Cursor, Gemini CLI, and other agents.
- **Integrations infrastructure for every use case:**


- [Actions](https://nango.dev/docs/guides/functions/action-functions) : one-off operations like creating an issue, commenting on a PR, or opening a pull request.
- [Syncs](https://nango.dev/docs/guides/functions/syncs/sync-functions) : scheduled functions that keep issues, PRs, and commits flowing into your app.
- [Webhooks](https://nango.dev/docs/getting-started/use-cases/webhooks-from-external-apis) : receive GitHub events, verified, and route them to your integration in real time.
- [MCP server](https://nango.dev/docs/guides/functions/tool-calling) : exposes your deployed actions as typed tools for the AI agents in your product.


## Prerequisites


- [Sign up for Nango](https://nango.dev/) (the free tier is enough for development).
- Add GitHub as an integration. Nango supports all the various GitHub auth types. In this demo, we’ll be registering a new[GitHub App](https://nango.dev/docs/api-integrations/github/how-to-set-up-a-github-app-with-nango) (the “GitHub App OAuth” type, integration ID` github-app-oauth` ) with Nango’s callback URL` https://api.nango.dev/oauth/callback` .


- Add a test connection: on the Nango dashboard, open the integration and select Connections > Add Test Connection, then authorize a GitHub account that owns a repository with a few issues and pull requests. While Codex builds your integration, it runs the generated code against this connection, so what ships has already worked against real data.
- Give Codex a project to build in. Install the[Nango CLI](https://nango.dev/docs/reference/functions/functions-cli) and run` nango init` : it creates a` nango-integrations` folder with the Nango framework bootstrapped. Set` NANGO_SECRET_KEY_DEV` (your dev API key, from Environment Settings in the dashboard) in` nango-integrations/.env` so it can test and deploy on your behalf.
- Install the Nango skill. Run` npx skills add NangoHQ/skills -s building-nango-functions-locally` ; the installer detects Codex and copies the skill to` .agents/skills/` , where[Codex discovers it](https://developers.openai.com/codex/skills) . The same skill works with Claude Code, Cursor, and[other coding agents](https://nango.dev/docs/getting-started/coding-agent-setup) .


**Tip:** LLM training data on Nango is often stale. Add the Nango docs MCP server alongside the skill so Codex pulls current API references while it generates code:` codex mcp add nango-docs --url "https://nango.dev/docs/mcp"`


## Sync a customer’s issues and pull requests into your app


A[sync](https://nango.dev/docs/guides/functions/syncs/sync-functions) keeps a fresh copy of a customer’s GitHub data in your app. Here it imports the issues and pull requests from a connected repository, then refreshes on a schedule so new activity shows up without anyone clicking refresh.


You build it by prompting Codex with the Nango skill (type` $` to mention a skill, or run` /skills` to browse):


#####


```text
$building-nango-functions-locally Build a Nango sync for the github-app-oauth integration that imports
the issues and pull requests from the repos a customer connects and keeps them up to date,
refreshing every hour. Integrate it with my frontend.
```


With the skill loaded, Codex:


- Researches the GitHub API and the endpoints it needs.
- Writes the sync and a typed model for your records.
- Tests it against your real connection with` nango dryrun` .
- Iterates on any errors until the sync works end to end.


#####


```text
//   Codex generates this sync. You do not write it by hand.
export   default   createSync  ({
description:   '  Imports issues and pull requests from a connected repo and refreshes hourly.  '  ,
frequency:   '  every hour  '  ,
autoStart:   false  ,   //   starts once your app saves the repo metadata
models: { GithubIssue },
exec  :   async   (  nango  )   =>   {
const   {   owner  ,   repo   }   =   await   nango  .  getMetadata  ();
for   await   (  const   issues   of   nango  .  paginate  ({
endpoint:   `  /repos/  ${  owner  }  /  ${  repo  }  /issues  `  ,
params: { state:   '  all  '  , sort:   '  updated  '  , per_page:   '  100  '   },
})) {
//   GET /issues returns pull requests too; the pull_request field tells them apart.
await   nango  .  batchSave  (issues  .  map  (toRecord),   '  GithubIssue  '  );
}
},
});
```


` nango.paginate` follows GitHub’s` Link` header for you, so you do not hand-roll pagination. The full sync, with the record model, incremental updates keyed on issue activity, and the` onWebhook` handler, is in[fetch-issues-and-pull-requests.ts](https://github.com/NangoHQ/github-api-integration/blob/main/nango-integrations/github-app-oauth/syncs/fetch-issues-and-pull-requests.ts) in the demo repo.


When Codex finishes, it deploys the sync for you (approve the deploy when it asks):


#####


```text
nango   deploy   --sync   fetch-issues-and-pull-requests   dev
```


Because the prompt said to integrate the frontend, Codex also wires[Nango Connect](https://nango.dev/docs/guides/auth/auth-guide) into your app: customers install your GitHub App, pick which repositories to grant, and the sync starts on its own.


Your backend reads the synced records from Nango’s cache and serves them to your UI:


#####


```text
const   {   records   }   =   await   nango  .  listRecords  (  {
providerConfigKey  :   '  github-app-oauth  '  ,
connectionId  ,
model  :   '  GithubIssue  '  ,
}  );
```


### Run the issues sync on demand


Syncs run on a schedule (every hour in this example). When a customer wants fresh data immediately, give them a refresh button that triggers the sync on demand (behind the scenes, a` nango.triggerSync` call) instead of waiting for the next run. The refresh endpoint that triggers the sync is in[backend/server.mjs](https://github.com/NangoHQ/github-api-integration/blob/main/backend/server.mjs) in the demo repo.


## Create issues and comment on pull requests from your app


An[action](https://nango.dev/docs/guides/functions/action-functions) is a one-off operation your product or an agent triggers on demand. This is the write-back direction: an AI agent files an issue from a support ticket, posts a review summary on a pull request, or opens a fix PR. Prompt Codex to build it:


#####


```text
$building-nango-functions-locally Add an action to the github-app-oauth integration that creates an issue
in a customer's repo from a typed title, body, and labels.
```


Codex writes the action, tests it by opening a real issue in your connected test repository, and deploys it when you approve. The full action is in[create-issue.ts](https://github.com/NangoHQ/github-api-integration/blob/main/nango-integrations/github-app-oauth/actions/create-issue.ts) in the demo repo.


#####


```text
//   Codex generates this action; approve the deploy when it asks.
export   default   createAction  ({
description:   '  Create an issue in the connected repo  '  ,
retries:   0  ,   //   creating an issue is not idempotent; a blind retry could open a duplicate
exec  :   async   (  nango  ,   input  )   =>   {
const   res   =   await   nango  .  post  (  {
endpoint  :   `  /repos/  ${  input  .  owner  }  /  ${  input  .  repo  }  /issues  `  ,
data  :   {   title  :   input  .  title  ,   body  :   input  .  body  ,   labels  :   input  .  labels   },
}  );
return   { number: res  .  data  .  number  , url: res  .  data  .  html_url   };
},
});
```


On` retries: 0` : creating an issue is not idempotent, so re-running the whole action after an unknown failure could open the same issue twice. Rate-limited requests are safe because they wrote nothing, and Nango’s proxy retries those for you.


Deploy it the same way (Approve when Codex asks to deploy):


#####


```text
nango   deploy   --action   create-issue   dev
```


Commenting on a pull request uses the same shape. In GitHub, a pull request is also an issue, so a comment posts to` /repos/{owner}/{repo}/issues/{number}/comments` whether the number points at an issue or a PR. One more prompt adds an` add-issue-comment` action, and a third opens a pull request from a branch with` POST /repos/{owner}/{repo}/pulls` :


#####


```text
$building-nango-functions-locally Add actions to the github-app-oauth integration to comment on an issue
or PR, and to open a pull request from a head branch into a base branch.
```


You can test any action from your app, or run it from the Nango dashboard with the[Playground](https://nango.dev/docs/updates/changelog#playground) against your connection.


## React to GitHub events in real time with webhooks


A scheduled sync is enough for most data, but GitHub is event-rich, and a lot of products need to react the moment a pull request opens or an issue changes. GitHub Apps deliver one[webhook](https://nango.dev/docs/getting-started/use-cases/webhooks-from-external-apis) stream for the events you subscribe to (push, pull_request, issues, and more).


Nango receives those webhooks, verifies the` X-Hub-Signature-256` signature against your webhook secret, and forwards the verified event to your integration, so you do not write HMAC code. Prompt Codex to wire it up:


#####


```text
$building-nango-functions-locally Subscribe the github-app-oauth integration to pull_request and issues
webhook events and update the synced records as soon as events arrive.
```


Keep the hourly sync running as a backstop. Webhook deliveries can be missed (a transient outage, a paused delivery), and a scheduled re-sync catches anything the event stream dropped. The combination of verified webhooks for freshness and a periodic sync for completeness is the pattern most production GitHub integrations settle on.


One manual step is needed to start receiving events: point your GitHub App at Nango’s webhook URL. Copy it from your integration’s settings page in the Nango dashboard, then paste it into your GitHub App under Developer settings > GitHub Apps > your app > Webhook URL.


## Give AI agents in your product access to GitHub


This is where the previous steps pay off: the sync gives the agent current GitHub data to reason over, and the actions become the tools it acts with. Once they are deployed, the AI agents inside your product can use them. Nango exposes enabled actions as typed tool calls through a hosted[MCP server](https://nango.dev/docs/guides/functions/tool-calling) , so an agent calls` create-issue` or` add-issue-comment` with typed inputs instead of guessing raw GitHub API parameters.


Point your MCP client at Nango’s server (Streamable HTTP transport) and pass three values:


#####


```text
Authorization:        Bearer <YOUR-NANGO-SECRET-KEY>
provider-config-key:  github-app-oauth
connection-id:        <CONNECTION-ID>
```


- **Authorization:** your Nango secret key, held by your backend.
- **provider-config-key:** your GitHub integration ID (` github-app-oauth` ).
- **connection-id:** the connection for the current user. Fetch it per logged-in user rather than hardcoding it, so each user’s agent acts on their own repositories.


Codex can wire this into your product’s agent for you:


#####


```text
$building-nango-functions-locally Wire my product's AI agent to Nango's MCP server for the github-app-oauth
integration, passing the logged-in user's connection id, so it can read issues and open PRs. Add flexible actions to Nango to make the agent feature complete
```


If you do not have an agent interface in your product yet but want to try the tools, add Nango’s MCP server to Codex itself.` codex mcp add` does not support custom headers, so add it to` ~/.codex/config.toml` :


#####


```text
[mcp_servers.nango_github]
url   =   "  https://api.nango.dev/mcp  "
bearer_token_env_var   =   "  NANGO_SECRET_KEY  "
http_headers   = {   "  connection-id  "   =   "  <CONNECTION-ID>  "  ,   "  provider-config-key  "   =   "  github-app-oauth  "   }
```


Then ask Codex: “Open an issue titled ‘Flaky test in checkout’ in my connected repo using the nango github MCP.” For the patterns that make these tool calls reliable in production, see[build reliable tool calls for AI agents](https://nango.dev/blog/build-reliable-tool-calls-for-ai-agents-integrating-with-external-apis) .


**Tip:** you can view detailed action and sync logs on the Nango dashboard.


## Common issues


Issue Cause and fix


\`401 Bad credentials\` The token expired or the installation was revoked. Nango refreshes installation tokens (1 hour) and user tokens (8 hours) for you; if the customer removed the installation, they reconnect from your app.


\`BAD_REFRESH_TOKEN\` on a user token The client secret was rotated or the installation was revoked. The customer reconnects. See the full breakdown.


\`403\` or \`429\` with \`retry-after\` A primary or secondary rate limit (100 concurrent requests, 900 points per minute). Nango retries with backoff; spread bulk work and avoid firing parallel requests at one installation.


Issue sync contains pull requests \`GET /issues\` returns PRs too. Filter on the \`pull_request\` field to keep them separate.


Webhook rejected or ignored The \`X-Hub-Signature-256\` HMAC did not match. Confirm the webhook secret on your GitHub App matches the one in Nango; Nango verifies the signature for you.


\`404\` on a repo the user can see The GitHub App installation was not granted that repository. The customer adds it under the installation's repository access settings.


Endpoint returns \`Not Found\` only for a GitHub App Some endpoints work only with OAuth Apps. Check for the "Works with GitHub Apps" note on the endpoint in GitHub's docs.


\`listRecords\` is empty right after connecting The sync starts only after your app saves the repo metadata, and the first run is asynchronous. Read records once a run completes.


## FAQ


**Should I use a GitHub App or an OAuth App for a customer-facing product?**


Use a GitHub App. It installs per organization or repository with fine-grained permissions and rate limits that scale from 5,000 to 12,500 requests per hour. An OAuth App acts as the authorizing user and suits personal-developer tools.


**How do you handle GitHub API rate limits when syncing data?**


GitHub allows 5,000 to 12,500 requests per hour, plus secondary limits of 100 concurrent requests and 900 points per minute. Spread bulk work, avoid firing parallel requests at one installation, and retry on` 403` or` 429` using the` retry-after` header.


**How do you verify GitHub webhooks?**


GitHub signs each delivery with an` X-Hub-Signature-256` header, an HMAC-SHA256 of the raw request body using your webhook secret. Verify it with a constant-time comparison before trusting the payload, and read the event type from the` X-GitHub-Event` header.


**How long does it take to build a GitHub API integration?**


By hand, a production integration with auth, syncs, webhooks, and rate-limit handling takes weeks. With Nango and a coding agent like Codex, the agent writes and tests the same integration against a real connection in about an hour.


## Conclusion


A production-grade GitHub API integration for agents comes down to customer auth, incremental sync, writing safely, reacting in real time (signature-verified webhooks), and staying inside two rate-limit systems. Together they become the tools the AI agents in your product call to act on a customer’s GitHub.


With the Nango skill, Codex writes that logic as code from a prompt and tests it against a real connection, while Nango’s runtime handles managed OAuth, durable syncs, webhook verification, retries, and observability. The same workflow covers any of Nango’s[900+ supported APIs](https://nango.dev/api-integrations) , and it scales into[just-in-time integrations](https://nango.dev/blog/just-in-time-integrations) : integrations built on demand by an agent instead of pre-built for every use case.


Clone the[demo project on GitHub](https://github.com/NangoHQ/github-api-integration) to run it end to end, or build your first integration from the[quickstart](https://nango.dev/docs/getting-started/quickstart) .


**Related reading:**


- [GitHub App vs. GitHub OAuth: when to use which](https://nango.dev/blog/github-app-vs-github-oauth)
- [How to build a Google Sheets API integration with Nango and Codex](https://nango.dev/blog/how-to-build-a-google-sheets-api-integration-with-nango-and-codex)
- [How to build a Gmail API integration with Nango and Claude](https://nango.dev/blog/how-to-build-a-gmail-api-integration-with-nango-and-claude)
- [Using AI coding agents for building API integrations in 2026](https://nango.dev/blog/using-ai-coding-agents-for-building-api-integrations)
- [Introducing the Nango API integrations builder skill](https://nango.dev/blog/nango-api-integrations-builder-skill)
