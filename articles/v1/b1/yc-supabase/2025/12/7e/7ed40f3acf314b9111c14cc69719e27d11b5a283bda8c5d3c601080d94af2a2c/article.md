---
schema_version: "1.0.0"
document_id: "7ed40f3acf314b9111c14cc69719e27d11b5a283bda8c5d3c601080d94af2a2c"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/building-chatgpt-apps-with-supabase"
published_at: "2025-12-17T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:55:00.595584+00:00"
content_hash: "sha256:8276b0eef3bd5f3481c3165f033f7b5da0fbf7b6ab82845bd1d61d98f3820ab7"
---

# Building ChatGPT Apps with Supabase Edge Functions and mcp-use

ChatGPT is no longer just a chat app. OpenAI recently launched the[Apps SDK](https://developers.openai.com/apps-sdk/) , which turns ChatGPT into a platform where developers can build interactive applications. These apps run inside ChatGPT and can display custom interfaces, connect to databases, and perform real actions.


In this guide, you will learn how to build a ChatGPT app that connects to your Supabase database. You will use[mcp-use](https://mcp-use.com/) , an open source SDK that makes it easy to deploy MCP servers on Supabase Edge Functions. By the end, you will have an app that lets ChatGPT users explore your database schema, view table data, and run SQL queries, all through interactive widgets.


ChatGPT Apps Marketplace Now Open


OpenAI has launched the[ChatGPT Apps Marketplace](https://chatgpt.com/apps) , where users can discover and use apps built by developers. If you build an app following this guide, you can submit it to the marketplace. Check out the[App Submission Guidelines](https://developers.openai.com/apps-sdk/app-submission-guidelines) to learn how to get your app listed.


## What are ChatGPT apps?#


ChatGPT apps extend what ChatGPT can do. Instead of just answering questions, ChatGPT can now show interactive interfaces and connect to external services.


Every ChatGPT app has two parts:


- **MCP server.** This is your backend. It defines the tools and capabilities your app provides. When a user asks ChatGPT to do something, ChatGPT calls your MCP server to get the data or perform the action.
- **Web component.** This is your frontend. It renders inside ChatGPT as an iframe. Your MCP server can return UI metadata that tells ChatGPT which component to display and what data to pass to it.


The MCP server and web component work together. ChatGPT acts as the bridge. When a user asks a question, ChatGPT figures out which tool to call. Your MCP server runs the tool and returns both data and UI instructions. ChatGPT then renders your component with the data.


This architecture is powerful because it separates what your app can do from how it looks. Your MCP server handles all the logic. Your components handle all the display. ChatGPT handles the conversation.


## What is mcp-use?#


mcp-use is an open source TypeScript SDK for building MCP servers. It solves a specific problem: the official MCP SDK uses Express, which depends on Node.js features that do not work in edge environments like Supabase Edge Functions.


mcp-use uses Hono instead of Express. Hono is a lightweight web framework designed for edge runtimes. This means your MCP server can run on Supabase Edge Functions, Cloudflare Workers, and other serverless platforms.


Here is what mcp-use gives you:


- **Edge runtime support.** Works on Deno, Cloudflare Workers, and Supabase Edge Functions.
- **OpenAI Apps SDK integration.** Built-in support for ChatGPT app widgets and UI components.
- **Supabase Auth integration.** Native support for authenticating users with Supabase Auth.
- **Full MCP compliance.** Scores 100/100 on the official MCP conformance tests.


## Why Supabase Edge Functions?#


Supabase Edge Functions are a good fit for MCP servers for several reasons.


- **Everything in one place.** Your database, authentication, and server code all live in the same Supabase project. Your MCP server can query your database directly. It can check user permissions using Supabase Auth. There is no need to manage separate services or configure connections between them.
- **Fast cold starts.** Edge Functions run on Deno and start quickly. When ChatGPT calls your MCP server, users do not wait for a container to spin up.
- **Built-in authentication.** Supabase Auth works out of the box with mcp-use. You can require users to log in before using your app. You can check their roles and permissions. You can scope data access to specific users.
- **Simple deployment.** A single command deploys your MCP server. Supabase handles scaling, HTTPS, and infrastructure.


## What you will build#


You will build a ChatGPT app that explores Supabase databases. The app provides four tools:


- **List tables.** Shows all tables in your database with an interactive schema explorer widget.
- **Show table.** Displays data from a specific table with sorting and filtering.
- **Execute SQL.** Runs read-only SQL queries and shows results with syntax highlighting.
- **Supabase status.** Shows the current status of Supabase services and recent incidents.


Each tool returns both data and a React widget. ChatGPT users see interactive interfaces, not just text responses.


## Full implementation#


This guide provides the key points for building a ChatGPT app. For the full implementation, please visit or clone the repository:[github.com/mcp-use/supabase-mcp-server](https://github.com/mcp-use/supabase-mcp-server)


## Project setup#


Start by creating a new project with mcp-use:


`
_10


npx create-mcp-use-app my-supabase-app --template apps-sdk


_10


cd my-supabase-app


_10


npm install


`


This creates a project with everything you need: the mcp-use SDK, widget templates, and build configuration for Supabase Edge Functions.


Note: the default` apps-sdk` template includes a "fruit shop" demo to show how widgets work. **You will need to modify or replace these files** to align with the Supabase explorer tools described in this guide.


Next, initialize Supabase in your project:


`
_10


supabase init


_10


supabase login


_10


supabase link --project-ref YOUR_PROJECT_ID


`


You can find your project ID in your Supabase dashboard under Project Settings.


## Building the MCP server#


Open` index.ts` and set up your MCP server:


`
_10


//index.ts


_10


import { error, MCPServer, object, text, widget } from 'mcp-use/server'


_10


_10


const server = new MCPServer({


_10


name: 'supabase-explorer',


_10


version: '1.0.0',


_10


description: 'A Supabase MCP server with Apps SDK widgets',


_10


})


`


## Adding UI widgets#


Widgets can be organized in two ways: **single-file widgets** or **folder-based widgets** . Choose the organization style that best fits your widget's complexity.


`
_10


├── index.ts # Main server file using mcp-use


_10


├── resources/ # React widget components


_10


│ ├── components/ # Reusable UI components


_10


│ ├── schema-explorer/ # Schema explorer widget (in this article)


_10


│ ├── table-viewer/ # Table viewer widget


_10


│ └── query-results/ # Query results widget


_10


│ └── supabase-status.tsx # 1 file widget


_10


└── package.json # Dependencies


`


See the widgets[implementation here](https://github.com/mcp-use/supabase-mcp-server/tree/main/resources) and the[widget docs](https://mcp-use.com/docs/typescript/server/ui-widgets) .


### Two types of widgets#


mcp-use SDK supports two widget patterns.


1. Display-only widgets
2. Display and return data widgets


### 1. Display-only widgets#


**Display-only widgets** do not fetch data on the server. They receive tool parameters as props and render UI. The supabase-status (` resources/supabase-status.tsx` ) widget works this way. It fetches status data client-side from the Supabase status API in the React component.


You don't need to register the widget as tool in` index.ts` because it doesn't need custom logic on the tool side, and mcp-use automatically registers all the **display-only widgets in**` resources/` folder as MCP tools.


`
_65


// resources/supabase-status.tsx


_65


import { McpUseProvider, useWidget, type WidgetMetadata } from "mcp-use/react";


_65


import React, { useEffect, useState } from "react";


_65


import z from "zod/v4";


_65


import "./styles.css";


_65


_65


// Tool args that will be passed to the widget props


_65


const propSchema = z.object({


_65


daysBack: z.number().default(7).describe("Number of days back to show incidents"),


_65


});


_65


_65


// Define widget metadata - auto-generates tool


_65


export const widgetMetadata: WidgetMetadata = {


_65


description: "Display Supabase service status and recent incidents from the status page",


_65


props: propSchema,


_65


exposeAsTool: true, // Important for display-only widgets, that are auto registered


_65


annotations: { readOnlyHint: true },


_65


appsSdkMetadata: {


_65


"openai/widgetCSP": {


_65


connect_domains: \["https://status.supabase.com"\],


_65


resource_domains: \["https://*.supabase.com"\],


_65


},


_65


},


_65


};


_65


_65


// Your widget component


_65


const SupabaseStatusWidget: React.FC = () => {


_65


// Get props from mcp-use hook: useWidget -> Everything you need in one hook!


_65


const { props, isPending } = useWidget<z.infer<typeof propSchema>>();


_65


const \[incidents, setIncidents\] = useState<Incident\[\]>(\[\]);


_65


_65


useEffect(() => {


_65


const fetchStatus = async () => {


_65


try {


_65


setLoading(true);


_65


const response = await fetch("https://status.supabase.com/history.rss");


_65


// ...


_65


// Fetch APIs from the widget


_65


}


_65


};


_65


fetchStatus();


_65


}, \[props.daysBack\]);


_65


_65


return (


_65


<McpUseProvider viewControls="fullscreen" autoSize>


_65


<Card className="relative p-6 rounded-3xl w-full">


_65


<div className="mb-6">


_65


<div className="flex items-center justify-between mb-2">


_65


<div>


_65


<h2 className="text-3xl font-bold text-\[hsl(var(--foreground))\] mb-1">


_65


Supabase Status


_65


</h2>


_65


<p className="text-sm text-foreground-muted">


_65


Last {props.daysBack} days


_65


</p>


_65


</div>


_65


</div>


_65


</div>


_65


{/* Full component implementation: https://github.com/mcp-use/supabase-mcp-server/blob/main/resources/supabase-status.tsx */}


_65


</Card>


_65


</McpUseProvider>


_65


);


_65


};


_65


_65


export default SupabaseStatusWidget;


`


As we defined in the widgetMetadata props` daysBack` , the MCP tool expects an argument specifying the number of days to retrieve incidents from the LLM.


### 2. Display and return data widgets#


**Display and return data widgets** fetch data server-side and pass it to both the LLM and the widget. The list-tables tool works this way. The server queries the database, returns data to ChatGPT for reasoning, and passes the same data to the widget for display.


This separation is useful. Sometimes you want to show the user more than you tell the LLM. Sometimes you want to tell the LLM more than you show the user. You control both independently.


You need to register the widget as a tool in` index.ts` . The following section goes through it.


## Adding tools#


Tools define what your app can do. Each tool has a name, parameters, and a callback function. For tools that return UI widgets, as in our case, we need to specify the` widget` argument and set the widget name from the component in` resources/` , in this case: "schema-explorer".


Here is the **list-tables** tool that returns the` schema-explorer` widget:


`
_44


// index.ts


_44


server.tool(


_44


{


_44


name: 'list-tables',


_44


description: 'List all tables in your Supabase database',


_44


schema: z.object({


_44


schemas: z.array(z.string()).optional().describe('Schemas to include (default: all)'),


_44


}),


_44


widget: {


_44


name: 'schema-explorer',


_44


invoking: 'Loading database tables...',


_44


invoked: 'Tables loaded successfully',


_44


},


_44


annotations: { readOnlyHint: true },


_44


},


_44


async ({ schemas }) => {


_44


try {


_44


const result = await supabaseClient?.callTool('list_tables', { schemas: \['public'\] })


_44


const content = result?.content\[0\]


_44


let tables: any\[\] = \[\]


_44


_44


if (content?.type === 'text') {


_44


const data = extractJsonFromResponse(content?.text ?? '')


_44


tables = Array.isArray(data) ? data : \[\]


_44


}


_44


_44


return widget({


_44


// Props passed to the React component in /resources folder


_44


props: {


_44


tables,


_44


schemas: schemas || \['public'\],


_44


},


_44


// Output returned by the MCP tool to the LLM (what the LLM sees)


_44


output: object({


_44


tables: tables.map((table) => ({


_44


name: table.name,


_44


})),


_44


}),


_44


})


_44


} catch (err) {


_44


return error(\`Error listing tables: ${err instanceof Error ? err.message : String(err)}\`)


_44


}


_44


}


_44


)


`


The tool queries your database for table information. It returns both text content for the LLM and widget metadata for the UI. ChatGPT uses the text content to understand the data. The widget renders in the chat for the user.


## Deploying to Supabase#


Supabase Edge Functions provide a great place to host your server. They are fast, scalable, and easy to manage.


For the complete step-by-step guide, see our[Supabase deployment documentation](https://mcp-use.com/docs/typescript/server/deployment-supabase#supabase) .


Prerequisites: Docker must be running before deployment


`
_10


# Verify Docker is running, otherwise install it


_10


docker info


`


mcp-use includes a deployment script that handles everything:


`
_10


curl -fsSL https://url.mcp-use.com/supabase | bash


`


This script checks your authentication, builds your application, sets environment variables, and deploys to Supabase Edge Functions.


After deployment, your MCP server is live at:


`
_10


https://<YOUR_PROJECT_ID>.supabase.co/functions/v1/<YOUR_FUNCTION_NAME>/mcp


`


## Connecting to ChatGPT#


Check out the[Official Developer Mode Guide](https://platform.openai.com/docs/guides/developer-mode) to enable it in your ChatGPT profile.


To use your app in ChatGPT:


1. Go to your ChatGPT profile settings
2. **Enable developer mode:** Go to[Settings → Apps & Connectors](https://chatgpt.com/#settings/Connectors) → **Advanced → Developer mode** .
3. Click on "Create App" to add the MCP server and enter your Edge Function URL with` /mcp` at the end.


Open a new ChatGPT App:


1. Click the "+" button to add a new connector
2. Select "More" and then select the MCP Server you just added.


Now you can start a new chat and use your tools. Ask ChatGPT to list your tables, show data from a specific table, or run a SQL query. ChatGPT will call your MCP server and display the interactive widgets.


For example, with the execute-sql tool, you can ask ChatGPT to write queries, execute them, and display the results.


## Next steps#


You now have a working ChatGPT app powered by Supabase Edge Functions. Here are some ways to extend it:


- Add more tools that interact with your specific database tables
- Build custom widgets for your data types
- Implement write operations with proper authorization checks
- Add tool chaining so ChatGPT can combine multiple operations


For more details on mcp-use, see the[mcp-use documentation](https://mcp-use.com/docs/typescript/server/deployment-supabase) or the[mcp-use GitHub repository](https://github.com/mcp-use/mcp-use) . For more on Supabase Edge Functions, see the[Edge Functions guide](https://supabase.com/docs/guides/functions) .


For the full implementation, please visit or clone the repository:[github.com/mcp-use/supabase-mcp-server](https://github.com/mcp-use/supabase-mcp-server)
