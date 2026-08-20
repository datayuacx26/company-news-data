---
schema_version: "1.0.0"
document_id: "5314949d1037921eb9a9285f00a8c950acb916b07d5128f3298308309a8205ca"
company_key: "yc-glide"
company: "Glide"
source_id: "yc-glide-news-import-b50025bbdd3a"
canonical_url: "https://www.glideapps.com/blog/glide-mcp"
published_at: "2026-08-08T00:34:54+00:00"
first_seen_at: "2026-08-08T09:03:34.710400+00:00"
fetched_at: "2026-08-08T09:03:36.529378+00:00"
content_hash: "sha256:be55bf9d35e4174ca8980dfd5a8b39db7f35dd909de607f899735fffe5c8cf27"
---

# Build software using your personal AI agent: GlideOS now connects to MCP clients

GlideOS now connects to[Model Context Protocol (MCP)](https://www.glideapps.com/docs/os/mcp) clients, meaning it’s now possible to build and interact with GlideOS using your personal agent, like Claude, Cursor, or ChatGPT.


AI is powerful when it doesn’t work alone.


When you connect Glide to your agent, you can create Glide apps directly from within your preferred environment and seamlessly integrate the power that Glide gives you with the agent you use every day for work. The new MCP connector lets your external agent connect to Glide and use all of its tools for understanding spreadsheets, building apps, loading data, and maintaining connectors.


Everything you can do in Glide, you can now do in the agent of your choice.


Watch Glide CEO, David Siegel, demo MCP live on his[LinkedIn](https://www.linkedin.com/posts/glideapps_coming-soon-glide-mcp-activity-7486498189181730816-ALYA) .


### Benefits of using an MCP to build in GlideOS with an agent


The main benefits you gain from using an agent to build with Glide are three things: Context, Capability, and Control.


1. **Context** from the agent's conversation history and access, e.g. your local systems
2. **Capability** from the agent's other connections and skills
3. **Control** over the models you use, which would also include efficiencies on token consumption


If you’re regularly using other AI agents, your agent knows a lot about you, and is connected to your other tools that Glide might not be connected to. By building using that agent, you can take your build further and make it more personalized. You can also build in multiple orgs simultaneously if that’s your thing. Here are some of the ways building with an MCP might benefit you:


### Use your preferred agent


With the MCP, you can get the benefits of GlideOS while continuing to work with your agent of choice. If you use ChatGPT or Claude every day, you can stay there and integrate Glide into your existing workflow with those agents. If your team or company has a business account or you have particular needs that keep you working in one specific AI agent, you can use Glide within that environment.


> > *“In my specific use case, I do a lot of research work with Perplexity, which itself wrangles various models depending on the scope of work. My Perplexity output is usually a spreadsheet. With the GlideOS MCP, I can easily have it take that spreadsheet and flip it into a new project.”*
> > - Andy Claremont, Head of Community & Ecosystem at Glide


### Experiment with design


Using different AI agents to create apps in GlideOS will give you design options that differ from building natively in Glide. Using an MCP gives you the freedom to work with different models that have different outputs, styles, and strengths.


The MCP is also a good way to pick a stronger model to build with or try out the latest AI models, such as Opus 5, as soon as they are released. These models can broaden what’s possible to design in Glide by combining strengths with new AI approaches.


### Optimize credit use across AI platforms


When you use an MCP, you can use the tokens you already paid for on your other service. You can build and edit GlideOS apps with your agent subscriptions instead of Glide credits. You can use this strategy to optimize your credit usage across AI platforms. If, for example, you have an ongoing company account with ChatGPT but have had a light usage month, you can apply those credits towards developing projects in Glide.


When you drive GlideOS from an agent like Claude or Cursor, the agent's reasoning runs on your own plan with that provider, and any tokens used are managed directly with them. Worth noting that Glide credits will still apply to the work GlideOS runs itself, including Runtime AI in your apps, workflows, and integrations.


## How to connect an MCP client in GlideOS


When you connect a Model Context Protocol (MCP) client, like Claude or Cursor, to your Glide organization, it can work with your projects the same way GlideOS does.


The MCP client signs in as you, so it will have the same features available to you. Its access depends on your organization’s plan. It can use the GlideOS built-in database and even create calculated views, which are Glide’s primitives that help customers understand and manage calculations in their software without having to write code.


To connect the MCP, click your organization's name in the Glide sidebar, then click MCP Server to get your server URL.


Once you’ve got GlideOS connected to your agent, you’ll see a list of actions. Before approving the connection, review the permissions for these actions. You can edit the permissions for each action, instructing the agent to ask you for approval, approve it every time, or never ask you to approve.


Most agentic tools can set up the connection to Glide’s MCP if you provide the URL and ask the agent to connect, but below are setup instructions for some common tools if you want to create a custom connection.


### Connect to Claude


In a[Claude](https://code.claude.com/docs/en/mcp-quickstart#add-and-verify-a-server) Team or Enterprise organization, only an owner can add a custom connector from Organization settings > Connectors. Everyone else opens Customize > Connectors and clicks Connect on the connector the owner added.


1. In GlideOS, click your organization’s name at the top of the sidebar.
2. Click MCP Server. The Glide MCP dialog opens and shows your Server URL.
3. Click Copy URL. Always copy the URL from the dialog rather than typing it.
4. Open Customize in Claude, then click Connectors.
5. Click +, then click Add custom connector.
6. Paste the Server URL into the remote MCP server URL field.
7. Click Add to save the connector.
8. Sign in to Glide when Claude sends you there, and approve the connection. If you belong to more than one organization, choose the one to connect.


### Connect to Cursor


1. In GlideOS, click your organization’s name at the top of the sidebar.
2. Click MCP Server. The Glide MCP dialog opens and shows your Server URL.
3. Click Copy URL.
4. Open Settings in Cursor and add a new MCP server.
5. Paste the Server URL into the server’s url field. Cursor saves it to .cursor/mcp.json for a single project, or to ~/.cursor/mcp.json for every project.
6. Click Needs login next to the server, then sign in to Glide and approve the connection.


### Connect to ChatGPT


1. ChatGPT treats an MCP server as a custom connector, and connectors sit behind developer mode. Turn that on first.
2. In GlideOS, click your organization’s name at the top of the sidebar.
3. Click MCP Server. The Glide MCP dialog opens and shows your Server URL.
4. Click Copy URL.
5. Open Settings in ChatGPT and click Connectors.
6. Turn on Developer mode under Advanced settings.
7. Click Create on the Connectors tab.
8. Enter a name for the connector.
9. Paste the Server URL into MCP server URL.
10. Select OAuth as the authentication method.
11. Click Create, then sign in to Glide and approve the connection.


## What is MCP or Model Context Protocol?


MCP, or[Model Context Protocol](https://modelcontextprotocol.io/docs/2026-07-28/getting-started/intro) , is an open protocol that lets AI models securely connect to external tools, data sources, and applications using a standard interface. Instead of every AI application needing a custom integration for every service (such as Google Drive, GitHub, Slack, databases, CRMs, etc.), those services can expose an MCP server that any compatible AI client can connect to.


If you have been using agents to do work in coordination with your other tools, such as Notion, Google Docs, or ClickUp, you've already been using MCPs. It’s how your AI agent connects to all the other tools that you use. With the Glide MCP, you can now use your agent to create a GlideOS project for you, just as it might create a ClickUp task.


Before MCP, if you wanted an AI to read your Slack messages or update a spreadsheet, someone had to build a custom, one-off integration for that exact combination of AI and software. Every new tool meant more custom code. MCP replaces that with one common protocol that any AI and any piece of software can both speak, so the integration only has to be built once.


MCP is becoming a common way to connect AI assistants with enterprise systems. It reduces the amount of custom integration work needed and makes it easier to build AI workflows that span multiple applications while keeping permissions and tool access under the organization's control. As more software vendors add MCP support, it is likely to become an important part of the infrastructure for AI-enabled business applications


## Integrate GlideOS into your existing work habits


More and more people are using AI agents as a home base to coordinate work across their personal tasks, teams, or even organizations. When you have Glide connected to your agent, you can instruct it to create Glide apps on the spot as a need arises.


If you realize your workload is piling up and you need a way to organize it, instruct your agent to create a to-do app in Glide, and it will begin building it for you.


If you’re coordinating an event, instruct your agent to build the app your team will use to coordinate everything seamlessly behind the scenes.


If you’re using an agent to pull data that’s been siloed in another piece of software, instruct it to build an interactive dashboard in Glide to share that data in real time with leadership instead of having it make a static slide deck.


Give your agent more power to improve your work with the Glide MCP.


Create a Glide app in five minutes, for free.


[Sign Up](https://os.glideapps.dev/?utm_source=blog&utm_campaign=Build+software+using+your+personal+AI+agent%3A+GlideOS+now+connects+to+MCP+clients)


[Cait Levin](https://www.glideapps.com/blog/author/cait-levin)


An educator for over 15 years, Cait leads all aspects of Glide’s educational experience for users, from[Glide University](https://learn.glideapps.com/) to the[Glide Docs](https://www.glideapps.com/docs) and everywhere in between. She has worked with learners of all ages across disciplines and mediums. When not thinking about how to help folks use Glide, she reads and writes on her couch in Boston, often with a snack.


### Related Articles


[Why GlideOS is the best Airtable alternative for businesses in 2026 (and you can port it in one prompt)](https://www.glideapps.com/blog/glideos-airtable-alternative)[Wren Noble](https://www.glideapps.com/blog/author/wren-noble) 8 Aug 2026


[Creating Slipstream: How we rebuilt our software engineering process for AI agents](https://www.glideapps.com/blog/creating-slipstream)[Simon Fletcher](https://www.glideapps.com/blog/author/simon-fletcher) 6 Aug 2026


[The ultimate guide to custom AI apps for your e-commerce operations](https://www.glideapps.com/blog/ai-apps-for-ecommerce)[Shivani Shah](https://www.glideapps.com/blog/author/shivani-shah) 6 Aug 2026
