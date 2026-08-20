---
schema_version: "1.0.0"
document_id: "5d4c827267dd0c008fe0aa7780408e231e2b671ad227383e8f072b7ffbadc870"
company_key: "yc-propelauth"
company: "PropelAuth"
source_id: "yc-propelauth-news-import-52b3c4ba8ae4"
canonical_url: "https://www.propelauth.com/post/migrate-from-clerk-and-auth0-with-propelauths-mcp-server"
published_at: "2026-06-16T00:00:00+00:00"
first_seen_at: "2026-07-25T20:01:02.470883+00:00"
fetched_at: "2026-07-28T21:46:31.447870+00:00"
content_hash: "sha256:af9e59e4e937fff6c66148a41fdb009799506a19f9055736c1cb0e188040cbda"
---

# Migrate from Clerk and Auth0 with PropelAuth's MCP Server

Most teams budget a week for auth and lose a month. That's the problem the[PropelAuth Integration MCP server](https://docs.propelauth.com/getting-started/integration-mcp-server?ref=propelauth.mymidnight.blog) solves: it gives your AI agent the right context to add PropelAuth quickly, instead of guessing or working from stale docs.


Starting fresh is one case. But what happens when you already have an auth provider and you want to switch?


Migrating auth tends to be the kind of task teams keep pushing to next quarter. The work is spread across your frontend, your backend, and your user data, and a good chunk of it is reading two sets of docs at once and figuring out how the concepts in one provider map onto another. We wanted to take some of that weight off, so the Integration MCP server now includes migration support for Clerk and Auth0.


## What the migration tools do #


The server exposes a set of tools that walk your AI agent through moving off Clerk or Auth0, broken up by the part of your stack you're working on:


- ` migrate_to_propelauth_frontend` for migrating your frontend
- ` migrate_to_propelauth_backend` for migrating your backend
- ` migrate_to_propelauth_fullstack` for migrating a Next.js fullstack application
- ` migrate_to_propelauth_users` for migrating your user and org data


Each tool returns the steps and context your agent needs for that piece of the migration. That includes which PropelAuth libraries to install, how to wire them up, and how the pieces of your current provider line up with the equivalents in PropelAuth.


The user and org data tool is usually the one teams worry about most. Swapping out code is one thing, but moving existing accounts over without disrupting the people already using your product is where migrations get nerve-wracking. That tool focuses specifically on getting your users and organizations across, so it isn't left as an afterthought once the code is done.


## Setting up the Integration MCP server #


If you're already using the Integration MCP server, you have these tools now. There's nothing new to install.


If you haven't set it up yet, the following configuration works in most AI agents:


```text
{
"mcpServers"  :     {
"propelauth"  :     {
"url"  :     "https://mcp.propelauth.com/mcp"
}
}
}
```


We have specific setup steps for Cursor, Claude Desktop, the Claude CLI, and the Gemini CLI in the[installation docs](https://docs.propelauth.com/getting-started/integration-mcp-server?ref=propelauth.mymidnight.blog#installation) . For Claude Desktop, for example, you add it through the Connectors menu as a custom connector named "PropelAuth" pointing at` https://mcp.propelauth.com/mcp` .


## Migrating from Clerk or Auth0 with a prompt #


Once the server is connected, you can kick off a migration with a plain prompt describing what you're moving and where it's coming from:


- Migrate my React frontend from Clerk to PropelAuth
- Migrate my FastAPI backend from Auth0 to PropelAuth
- Migrate my users and orgs from Clerk to PropelAuth


Your agent queries the server, gets back the relevant instructions, and applies them to your project. You'll see a prompt asking for permission before it queries the Integration server, and you stay in the loop to approve the changes as they happen rather than handing the whole thing over at once.


For the data side of a migration in particular, it's still worth reading through our[migration docs](https://docs.propelauth.com/migrations?ref=propelauth.mymidnight.blog) and reaching out if you have a setup that needs extra care. The tools are there to do the heavy lifting, not to replace a quick sanity check on something as important as your user records.


## What's next for migration support #


We started with Clerk and Auth0 because those are the two providers we hear about the most from teams looking to move. If you're coming from somewhere else and want to see it supported, let us know atsupport@propelauth.com . We're paying attention to which providers come up, and that feedback is what tells us where to go next.
