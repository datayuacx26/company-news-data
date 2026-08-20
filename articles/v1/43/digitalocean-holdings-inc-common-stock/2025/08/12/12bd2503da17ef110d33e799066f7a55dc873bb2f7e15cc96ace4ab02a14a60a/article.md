---
schema_version: "1.0.0"
document_id: "12bd2503da17ef110d33e799066f7a55dc873bb2f7e15cc96ace4ab02a14a60a"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/mcp-server-public-release"
published_at: "2025-08-26T02:32:43.209+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T20:56:25.546971+00:00"
content_hash: "sha256:6c26c967ff7d51887f2638231a8e54da37aa50daf6fa925a5d1ef5a93776eadc"
---

# DigitalOcean MCP Server is now available

The new **DigitalOcean MCP (Model Context Protocol) Server** enables you to manage your cloud resources with simple, natural language commands through AI-powered tools like Cursor, Claude, or your own custom Large Language Models (LLM). Running locally, it connects seamlessly to 9 services – making cloud operations faster, easier, and more intuitive for developers.


**→** Try the new **DigitalOcean MCP Server** today by following the specific configuration guidelines for the MCP client from the[DigitalOcean MCP GitHub repo](https://github.com/digitalocean-labs/mcp-digitalocean)


**→** Watch our most recent MCP Server video walkthrough:


→ Explore the[Model Context Protocol (MCP) Overview](https://modelcontextprotocol.io/)


## What is Model Context Protocol?


**Model Context Protocol (MCP)** is an[open-source standard](https://modelcontextprotocol.io/docs/getting-started/intro) that streamlines how AI systems like large language models (LLMs) connect with external tools, systems, and data sources. It defines a standard and consistent way to manage and share context across machine learning components, replacing fragmented integrations. An **MCP Server** acts as a bridge between an AI application and these external resources.


**Learn more about the DigitalOcean MCP Server**


### 1. Broad Service Coverage


Now supports 9 services (and growing):[Accounts](https://docs.digitalocean.com/platform/accounts/) ,[App Platform](https://www.digitalocean.com/products/app-platform) ,[Databases](https://www.digitalocean.com/products/managed-databases) ,[DOKS](https://www.digitalocean.com/products/kubernetes) ,[Droplets](https://www.digitalocean.com/products/droplets) ,[Insights](https://docs.digitalocean.com/products/app-platform/how-to/view-insights/) ,[Marketplace](https://marketplace.digitalocean.com/) ,[Networking](https://www.digitalocean.com/products/networking) , and[Spaces Storage](https://www.digitalocean.com/products/spaces) . Instead of juggling multiple dashboards or tools, you can manage common cloud operations right inside your favorite MCP-compatible tools.


### 2. Natural Language to API Translation


Turn plain English into real API actions. No more digging through docs or writing scripts – just ask your AI assistant.


**Examples:**


-


**Deploy and Manage Apps:** Run commands like` deploy a Ruby on Rails app from my GitHub repo` or` check which apps are on my account.`


-


**Create and Manage Databases:** Easily provision a new PostgreSQL database or create a new database. For example, you could use the command` create a new PostgreSQL database named "my-project-db" with PostgreSQL version 14` and it would do so in no time.


-


**Work with Files:** Upload files from a local directory to a Spaces bucket, create a temporary access key, and get public URLs for your files. An example command could be` upload my-photos directory to my-bucket` or a more specific command could be` sync local-project-files to my-spaces-bucket in the 'assets' folder.`


-


**Check Certificates and Monitoring:** Check the status of your SSL certificates. An example command could be` check the status of my SSL certificates or check the status of the SSL certificate for \[my-website.com\](http://my-website.com)`


-


**Optimize and Understand Costs:** Get visibility on your cloud costs, drill down into your monthly app spending, or view your billing history for the last 12 months to understand reasons for higher bills in specific months. An example command could be` show me my cloud costs from December` or` break down my cloud spending from the last 6 months` .


### 3. Simple, Transparent Setup


Getting started is easy.


1.


Get your DigitalOcean API token from the[Applications & API page](https://cloud.digitalocean.com/account/api/tokens?i=ab895b) in the DigitalOcean console.


2.


Add your token when configuring the MCP client. Here’s an example mcp.json snippet:


```text


{


"mcpServers"  :    {


"digitalocean"  :    {


"command"  :    "npx"  ,


"args"  :    [  "@digitalocean/mcp"  ,    "--services apps"  ]  ,


"env"  :    {


"DIGITALOCEAN_API_TOKEN"  :    "YOUR_API_TOKEN"


}


}


}


}


```


Your credentials stay under your control, and you can start managing infrastructure in minutes.


### 4. Scoped Access for Security & Simplicity


Once you’re set up, you decide which services your AI assistant can access. Use the --services flag when starting the MCP server to restrict access to only what you need:


```text
-  -  services apps ,  databases ,  droplets


```


Scoping keeps things focused. It prevents the model from getting confused by too many options, avoids unnecessary context bloat, and limits access to only what you actually need.


### 5. Seamless Integration


Works out of the box with Claude Code, Cursor, VS Code, Windsurf, and any other MCP-compatible clients – so you can manage your infrastructure without leaving your favorite tools.


DigitalOcean MCP Server in Cursor DigitalOcean MCP Server in Claude Code


## Why This Matters


You can now efficiently integrate **cloud management directly into your AI assistants (Claude, Cursor, VSCode, Custom Assistants)** – no more context switching, manual API requests, or digging through the console for metrics. Whether you’re scaling infrastructure, troubleshooting in real-time, or deploying full applications, DigitalOcean MCP server makes the process faster, secure, and accessible.


Since going live on GitHub two weeks ago, **hundreds of developers are already using the MCP Server daily** to get their work done–provisioning infrastructure, monitoring usage, reviewing logs , and automating cloud tasks with plain English.


## How to Get Started


1. Follow the specific configuration guidelines for the MCP client (eg. Cursor, VS Code, Claude Code) from the[DigitalOcean MCP GitHub repo](https://github.com/digitalocean-labs/mcp-digitalocean) . For example, for Claude Code:


```text
claude mcp add digitalocean -  mcp \


-  e DIGITALOCEAN_API_TOKEN=YOUR_DO_API_TOKEN \


-  -   npx @digitalocean/mcp  -  -  services apps ,  databases


```


1. Watch this video to learn how to manage your DigitalOcean services with natural language commands. You’ll see end-to-end examples to build workflows like deploying a Ruby on Rails app on App Platform, creating a PostgreSQL database, uploading to Spaces, checking SSL certificates, and optimizing costs with billing insights and tips.


1. The following video demonstrates how to build and deploy an application to App Platform from Cursor.


## Important things to remember


When using an MCP server, it’s important to balance automation technology with preventative measures. Remember to include human intervention when exploring MCP Server, as a safeguard. Use secure access controls, clear audit trails, and strong error handling to help keep your system reliable. A layer of human oversight helps to ensure that critical decisions aren’t left entirely to automation. As with most new technologies, regular testing, monitoring, and maintenance are ideal best practices to adopt for a successful and resilient setup.


## Availability & Pricing


-


**Availability:**[Supported](https://docs.digitalocean.com/platform/product-lifecycle/#public-preview) in production.


-


**Pricing:** Free.


## Related Resources


- [GitHub repository](https://github.com/digitalocean-labs/mcp-digitalocean)
- [Model Context Protocol overview](https://modelcontextprotocol.io/)
- Docs coming soon


## Feedback & Next Steps


We’d love to hear what you build with MCP. Try it out, share your feedback in[Community](https://www.digitalocean.com/community) , or open an issue on[GitHub](https://github.com/digitalocean-labs/mcp-digitalocean/issues) to help shape the future of AI-powered cloud management at DigitalOcean.
