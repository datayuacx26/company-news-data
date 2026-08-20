---
schema_version: "1.0.0"
document_id: "b964b35a710e859d43afc54d6948317ece96bab1273185660fb95ddcd2411d72"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/managing-secrets-mcp-servers"
published_at: "2025-03-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T20:58:14.920102+00:00"
content_hash: "sha256:bcc61fae41c1cb0b9d79b6410a92b032a65da83ee5ad7cec70adb7989533f1b2"
---

# Managing Secrets in MCP Servers

As MCP servers gain popularity in unifying the interactions between LLM clients and various tools/services, they're becoming a focal point in the security architecture of AI applications. At Infisical, we've been watching this trend closely, and noticed a critical aspect that's often overlooked: secrets management.


## Quick Recap: What is an MCP Server?


Model Context Protocol (MCP) servers act as “smart” middleware between Large Language Models (LLMs) like Claude or GPT and external tools or services.


Unlike traditional API servers that simply route requests, MCP servers translate the natural language requests from LLMs into structured operations on tools, and then convert the results back into a format the LLM can understand. This is done by implementing[Anthropic’s open MCP protocol](https://spec.modelcontextprotocol.io/specification/2024-11-05/) , which creates a unified interface for LLMs to access databases, call external APIs, execute code, or interact with any service—without needing custom integration for each one.


What makes MCP servers valuable is how they extend an LLM's capabilities beyond its knowledge cutoff and text generation abilities. They enable LLMs to perform real-time actions like retrieving current data, executing transactions, or accessing proprietary information—all while maintaining a consistent interaction pattern regardless of the underlying tools.


For engineers already familiar with API gateways or proxy servers, you can think of MCP servers as adding a semantic layer that understands both the LLM's intent and how to translate that into specific tool operations. And since they act as intermediaries between LLMs and external services, they need to handle sensitive credentials to function.


## The Challenge with Secrets in MCP Servers


Most MCP servers need to juggle API keys, database credentials, and other secrets to function properly. Since they act as intermediaries between LLMs and external services, they frequently handle sensitive credentials for many different systems simultaneously.


Looking at typical MCP implementations, we've noticed that secrets management isn't always addressed explicitly, which creates several potential security risks:


1. **Hardcoded secrets in server code**
2. **Long lived credentials**
3. **Lack of access control governing secrets**
4. **No audit trail for secret usage**


## Best Practices for MCP Secrets Management


### 1. Don’t Hardcode Secrets


This might seem obvious, but it still happens.


```text
// DON'T DO THIS
const API_KEY = "sk_1234567890abcdef";


// Instead, load from environment or a secrets management tooling
const API_KEY = process.env.WEATHER_API_KEY;


```


### 2. Reduce Your Exposure


One of the challenges with MCP servers is that they often serve as gateways to multiple external services—making them an target for attackers.


Whether you are building your own MCP server or running a pre-built server like` npx -y weather-mcp` , you can reduce the exposure of needed credentials by leveraging modern secrets management solutions like Infisical.


#### a. Implement Ephemeral Credentials for Custom Servers


When building a custom MCP Server, a good strategy is to implement ephemeral credentials that are short-lived and scoped to specific operations. By generating credentials on-demand and with limited lifespans, you ensure that even if credentials are somehow compromised, the window of vulnerability is minimal.


[Dynamic secrets](https://infisical.com/docs/documentation/platform/dynamic-secrets/overview) are short-lived, session-specific keys that offer the best security guarantees by minimizing the exposure window. They're ideal for scenarios like **database access** , where you might want to generate read-only credentials for queries but need elevated permissions for updates or schema changes.


However, not all integrations support dynamic credentials, which is why we also need secure methods for handling static secrets. The example below demonstrates both approaches: dynamic secrets for database access where security is paramount, and securely retrieved static API keys for services that don't support ephemeral authentication. By using the Infisical SDK, the server avoids hardcoding any secrets directly in the code.


```text
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";
import { InfisicalSDK } from "@infisical/sdk";
import { Client as PostgresClient } from "pg";


// ----- Infisical Initialization -----
const infisicalClient = new InfisicalSDK({
siteUrl: "https://app.infisical.com"
});


await infisicalClient.auth().universalAuth.login({
clientId: process.env.INFISICAL_CLIENT_ID,
clientSecret: process.env.INFISICAL_CLIENT_SECRET,
});


// ----- Create MCP Server Instance -----
const server = new McpServer({
name: "my-mcp-server",
version: "1.0.0",
});


// ----- Dynamic Secret Fetching Example -----
// Database query tool using dynamic, short-lived credentials
server.tool(
"query-database",
"Run a read-only query using dynamic credentials",
{
query: z.string().describe("SQL query string"),
},
async ({ query }) => {
// Generate dynamic credentials valid for 5 minutes
const lease = await infisicalClient.dynamicSecrets().leases.create({
dynamicSecretName: "POSTGRES_READONLY",
environmentSlug: "production",
projectSlug: "your-project-slug",
ttl: "5m",
});


const dbCreds = lease.data;
const dbClient = new PostgresClient({
host: dbCreds.host,
user: dbCreds.username,
password: dbCreds.password,
database: dbCreds.database,
});


await dbClient.connect();
const results = await dbClient.query(query);
await dbClient.end();


return {
content: [
{
type: "text",
text: JSON.stringify(results.rows, null, 2),
},
],
};
}
);


// ----- Static Secret Fetching Example -----
// Weather forecast tool using a static API key
server.tool(
"get-forecast",
"Retrieve weather forecast using a static API key",
{
latitude: z
.number()
.min(-90)
.max(90)
.describe("Latitude of the location"),
longitude: z
.number()
.min(-180)
.max(180)
.describe("Longitude of the location"),
},
async ({ latitude, longitude }) => {
// Fetch the static secret from Infisical (no hardcoding here)
const { secretValue: apiKey } =
await infisicalClient.secrets().getSecret({
secretName: "WEATHER_API_KEY",
environment: "production",
projectId: "weather-mcp-server",
});


const response = await fetch(
`https://api.weather.gov/points/${latitude},${longitude}`,
{
headers: {
"User-Agent": "weather-app/1.0",
Authorization: `Bearer ${apiKey}`,
},
}
);
const data = await response.json();


return {
content: [
{
type: "text",
text: JSON.stringify(data.properties.forecast, null, 2),
},
],
};
}
);


```


The database query example creates temporary read-only credentials valid for just 5 minutes, executes the query with limited permissions, and automatically terminates access after use. Meanwhile, the weather forecast example demonstrates secure API key retrieval from Infisical using environment-specific parameters, allowing authenticated requests without exposing credentials in the code.


#### b. Securely Inject Secrets into Pre-Built Servers


Most pre-built MCP servers you can find in repositories like[https://github.com/punkpeye/awesome-mcp-servers](https://github.com/punkpeye/awesome-mcp-servers?%5D(https://github.com/punkpeye/awesome-mcp-servers?tab=readme-ov-file)) require you to expose secrets as environment variables. Rather than storing these values in files which could easily be leaked, it’s safer and more practical to use the **Infisical CLI** to inject API keys or other secrets in the subcommand on-the-fly.


Instead of running the command like this:


```text
env FIRECRAWL_API_KEY=fc-YOUR_API_KEY npx -y weather-mcp-server


```


You should inject your secrets into your local environment like this:


```text
infisical run -- npx -y weather-mcp-server


```


Note that for this command to succeed, the Infisical CLI must be authenticated. This can be achieved either manually with the[infisical login](https://infisical.com/docs/cli/commands/run#infisical-run:tags) command or automatically through several methods, most notably[machine identity authentication](https://infisical.com/docs/cli/commands/login#machine-identity-authentication-quick-start) .


[Read more about the Infisical CLI](https://infisical.com/docs/cli/usage) .


### 3. Isolate Access Between Servers


In larger deployments, where servers handle isolated functions, not all MCP servers require access to every secret. By organizing your secrets based on the specific integrations or services each server manages, you can enforce granular access control policies. This isolation ensures that each MCP server only has access to the credentials it needs, which minimizes the overall attack surface and simplifies auditing.


For example, you might define access governance policies in Infisical that look like this:


```text
// Define access policies for different MCP servers
const policies = [
{
name: "weather-server-policy",
permissions: [
{
subject: "secrets",
inverted: false,
action: "read",
conditions: {
environment: "weather-server",
secretPath: "secrets/WEATHER_API_KEY"
}
}
]
},
{
name: "database-server-policy",
permissions: [
{
subject: "secrets",
inverted: false,
action: "read",
conditions: {
environment: "database-server",
secretPath: "dynamic-secrets/POSTGRES_READONLY"
}
}
]
}
];


```


By granting access via policies you achieve:


- **Access Isolation:** Only the relevant MCP server accesses its designated secrets.
- **Enhanced Security:** Reduced exposure limits the impact if one server is compromised.
- **Simplified Auditing:** Tracking secret access by server becomes straightforward, making it easy to triage incident response.
- **Streamlined Management:** Clear organization of secrets makes both development and productions deployments consistent with less room for hiccups.


## Conclusion


Proper secrets management is a critical yet often overlooked component of secure MCP server implementation. As these servers become the central nervous system connecting LLMs to external tools and services, they represent a high-value target for potential attackers. By avoiding hardcoded credentials, implementing ephemeral secrets where possible, securely injecting secrets into pre-built servers, and isolating access between different MCP instances, you can significantly reduce your security exposure.


Remember that a single compromised API key or database credential could potentially give attackers access to sensitive data or services across your entire infrastructure.


Infisical offers a developer-friendly solution that works seamlessly with your existing workflows, available both self-hosted or as a fully-managed cloud service.[Get started in under 5 minutes](https://app.infisical.com/signup) and ensure your AI infrastructure's security foundation is as advanced as the LLMs it supports.
