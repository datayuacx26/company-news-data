---
schema_version: "1.0.0"
document_id: "c876116fed3de87a5ad5214881d896eec69d593b6285688c33ea45414f734586"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/remote-mcp-server"
published_at: "2025-12-09T18:53:48.536+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T22:25:03.887977+00:00"
content_hash: "sha256:b305d4066337a7c8c3319a7ce764b8c025978ba63c0c5b4dea0398ff26e73135"
---

# Now Available: Remote MCP for DigitalOcean Services

Earlier this year, DigitalOcean[introduced](https://www.digitalocean.com/blog/mcp-server-public-release) the **DigitalOcean Model Context Protocol (MCP) Server** , allowing developers to connect applications and AI-assistants like Cursor and Claude Desktop directly to their DigitalOcean cloud infrastructure. You could ask your AI assistant to deploy apps, check database status, or troubleshoot droplet issues – all conversationally, making your infrastructure “AI-readable”. Until now, this required running the MCP server locally using the *npx* binary on your computer.


Remote MCP support is now available on DigitalOcean. You can now connect your AI tools to DigitalOcean services without installing any binaries locally.


Remote MCP endpoints are live for 9 DigitalOcean services: Accounts, App Platform, Databases, DigitalOcean Kubernetes, Droplets, Insights, Marketplace, Networking, and Spaces. Each service runs as its own MCP server at a dedicated HTTPS endpoint (example:[https://apps.mcp.digitalocean.com/mcp](https://apps.mcp.digitalocean.com/mcp) for App Platform). Just update your MCP client configuration to point at our hosted endpoints, include your DigitalOcean API token and you’re ready to go with immediate, authenticated access to your infrastructure. All existing DigitalOcean MCP tutorials and videos continue to work. The only change is your mcp.json configuration.


## Why Remote MCP?


MCP is an open standard for connecting AI tools to external systems and data sources. It provides a consistent interface for exposing tools and context to LLMs. While Local MCP servers work great, Remote MCP now gives you a zero-setup production-ready experience, with the following advantages:


-


**No local dependencies:** Skip the Docker installs, Go binaries, and background processes.


-


**Modular connections:** Connect only to the services you need (e.g. just Databases, just Kubernetes, or any combination) by picking specific MCP endpoints.


-


**Always up to date:** We handle server versioning and rollouts, so your application always has access to the latest DigitalOcean API features and tools.


-


**Standardization:** Remote MCP makes it easier to standardize your MCP setup across developers in teams without needing to keep local binaries in sync.


## Endpoint mapping: one MCP server per service


Each DigitalOcean service runs as a standalone MCP server. A server only exposes tools for that specific service (App Platform vs. Databases vs. Networking, for example). From your MCP client’s perspective, you just reference the HTTPS URL:


Remote MCP Service MCP URL


App Platform[https://apps.mcp.digitalocean.com/mcp](https://apps.mcp.digitalocean.com/mcp)


Kubernetes (DOKS)[https://doks.mcp.digitalocean.com/mcp](https://doks.mcp.digitalocean.com/mcp)


Insights[https://insights.mcp.digitalocean.com/mcp](https://insights.mcp.digitalocean.com/mcp)


Spaces[https://spaces.mcp.digitalocean.com/mcp](https://spaces.mcp.digitalocean.com/mcp)


Accounts[https://accounts.mcp.digitalocean.com/mcp](https://accounts.mcp.digitalocean.com/mcp)


Networking[https://networking.mcp.digitalocean.com/mcp](https://networking.mcp.digitalocean.com/mcp)


Droplets[https://droplets.mcp.digitalocean.com/mcp](https://droplets.mcp.digitalocean.com/mcp)


Databases[https://databases.mcp.digitalocean.com/mcp](https://databases.mcp.digitalocean.com/mcp)


Marketplace[https://marketplace.mcp.digitalocean.com/mcp](https://marketplace.mcp.digitalocean.com/mcp)


*Include any subset of these in your MCP configuration, depending on the services you want your AI assistant to access.


## Example Remote MCP configuration


Like the local MCP server, Remote MCP uses your MCP client’s configuration file (ex: mcp.json or similar). Here’s an example connecting to two remote MCP servers: **apps** and **databases** .


```text


{


"mcpServers": {


"apps": {


"url": "https://apps.mcp.digitalocean.com/mcp",


"headers": {


"Authorization": "Bearer YOUR_TOKEN"


}


},


"databases": {


"url": "https://databases.mcp.digitalocean.com/mcp",


"headers": {


"Authorization": "Bearer YOUR_TOKEN"


}


}


}


}


```


To start using Remote MCP with DigitalOcean:


-


**Create or reuse a DigitalOcean API token:** Replace YOUR_TOKEN with your DigitalOcean API token from the[Applications & API page](https://cloud.digitalocean.com/account/api/tokens?i=ab895b) in the DigitalOcean console(scoped appropriately for the services you’re using (ex:App Platform, Databases, Droplets)).


-


**Add MCP endpoint:** Add one or more remote MCP servers to your mcpServers configuration, pointing to the URLs above.


-


**Use your AI assistant as usual:** From there, you can use natural language to:


-


Spin up and manage App Platform apps


-


Create or modify databases


-


Inspect logs and events


-


Query infrastructure state (apps, droplets, clusters, etc.)


**Switching from local to remote MCP**


If you’ve configured the local DigitalOcean MCP Server, your existing configuration might look like this:


### Local configuration (existing)


```text


{


"mcpServers": {


"digitalocean": {


"command": "npx",


"args": ["-y", "@digitalocean/mcp", "--services", "apps,databases"],


"env": {


"DIGITALOCEAN_API_TOKEN": "YOUR_TOKEN"


}


}


}


}


```


Here, your MCP client starts the MCP server locally (via npx or a binary), and the server reads your token from the environment. To migrate to Remote MCP, you replace the command/args definition with hosted URLs and move the token into headers, as seen in the **Example Remote MCP configuration** section above.


All previous tutorials, docs, and video guides for the DigitalOcean MCP Server remain valid. The only thing that changes is your MCP client points to a remote endpoint. Once your mcp.json references the remote URLs, your AI assistant (Claude, Cursor, VS Code, and others) will interact with your DigitalOcean resources exactly as before; just with less setup and fewer moving parts.


## Authentication and request model


Both local and remote MCP use **standard DigitalOcean API tokens** for authentication. The key difference is the transport mechanism and where you provide the token. Get your DigitalOcean API token from the[Applications & API page](https://cloud.digitalocean.com/account/api/tokens?i=ab895b) in the DigitalOcean console.


-


**Local MCP:** STDIO transport. Token passed as an environment variable when starting the local server process.


-


**Remote MCP:** Streamable HTTP transport. Token sent in the Authorization header with each HTTPS request.


**Header-based authorization:** Clients send the token in the Authorization header with each request:


```text
GET /mcp HTTP/1.1


Host: apps.mcp.digitalocean.com


Authorization: Bearer eyJhbGciOiJIUzI1NiIs...


```


**Scoped per client:** Tokens are scoped to the context of each request.


**Aligned with MCP spec:** This matches the MCP specification, which expects clients to provide credentials as part of the connection or request metadata.


All existing security guidance for the local MCP server applies here:


– Use properly scoped tokens;


– Avoid committing configuration files with secrets;


– Prefer environment-variable substitution or client-specific secret stores where possible.


## What’s next


Remote MCP is a natural evolution in our MCP investment – it makes AI-powered workflows easier to adopt, configurations easier to standardize across teams, and DigitalOcean resources easier to manage.


If you’re new to MCP:


-


Explore the official[Model Context Protocol](https://modelcontextprotocol.io/docs/getting-started/intro) documentation to understand how MCP fits into your broader AI tooling strategy.


-


Start with the DigitalOcean[MCP Server](https://docs.digitalocean.com/reference/mcp/configure-mcp/) docs for an end-to-end walkthrough.


We’re excited to see what you build next!
