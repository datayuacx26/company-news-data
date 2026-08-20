---
schema_version: "1.0.0"
document_id: "3c9b68765ef7afab893233949dd0db66012dbca2c641036e37d8a8bd0d7d9294"
company_key: "yc-alpaca"
company: "Alpaca"
source_id: "yc-alpaca-rss-4f5e7678c8d4"
canonical_url: "https://alpaca.markets/blog/alpacas-mcp-server-for-trading-api-adds-documentation-access/"
published_at: "2026-08-10T17:00:17+00:00"
first_seen_at: "2026-08-10T19:19:39.035292+00:00"
fetched_at: "2026-08-10T19:19:40.454427+00:00"
content_hash: "sha256:e737a9368d2b4cfc2737e234f0e886275915707f55cb0e2c49d645533fa2cea4"
---

# Alpaca’s MCP Server for Trading API Adds Documentation Access

Today,[Alpaca’s Trading MCP Server](https://alpaca.markets/mcp-server?ref=alpaca.markets) now includes read-only access to Alpaca’s API documentation, giving connected assistants the context they need to help developers build trading dashboards, automation, and strategies more efficiently.


Previously, an assistant often needed API details pasted into the prompt or inferred from partial context. Now, through the same MCP server that exposes Alpaca market data, account, and trading tools, an assistant can search Alpaca’s supported API references, find the right operation for a task, and inspect its parameters, request shape, response fields, constraints, and source documentation.


This allows builders to move from intent to implementation with less need to manually search or include large portions of API documentation in a prompt. . Instead, agents can retrieve the specific API context they need when they need it.


The documentation tools cover Alpaca’s Trading API, Market Data API, and Authentication references. They are read-only and return relevant documentation context and source links for review. They do not change accounts or order states.


## **What’s New: Build With API Documentation in Context**


The new documentation tools help builders and connected assistants move from an intended workflow to the Alpaca API operation needed to implement it by providing relevant API documentation and implementation context within the connected workflow.:


- **Access API documentation in context:** Ask about operations, parameters, request shapes, and response fields without searching Alpaca’s documentation separately.


- **Example Prompt:** “I want to submit a bracket order. Show me the relevant operation, required parameters, request shape, and expected response fields.”


- **Move from intent to implementation:** Describe the workflow you want to build and surface the relevant API operation, constraints, and authentication requirements.


- **Example Prompt:** “I want to replace an existing order. Which operation should I use, what constraints apply, and what authentication is required?”


- **Give agents documentation context:** Provide source-backed API documentation that helps connected assistants reason through integrations and review implementation details against Alpaca’s documented API requirements.


- **Example Prompt:** “Review this order request against Alpaca’s API documentation, identify any missing or unsupported fields, and include the relevant source links.”


- **Keep conversations focused:** Retrieve relevant documentation context and source links relevant without navigating across separate tools or broad documentation pages.


- **Example Prompt:** “Summarize only the documentation needed to retrieve a full option chain, including the operation, required inputs, and source references.”


- **Review before acting:** Inspect API behaviour through read-only tools that do not change accounts or order states.


- **Example prompt:** “Before I implement this workflow, explain how the operation behaves, what it can change, and which constraints I should review. Do not place or modify any orders.”


## **Building with Alpaca's Trading MCP Server**


The steps for accessing documentation depend on whether you are connecting the Trading MCP Server for the first time or updating an existing installation.


### **New to Alpaca’s Trading MCP Server?**


Alpaca’s Trading MCP Server connects a supported MCP client to Alpaca’s market data, account, and trading tools. Documentation access adds read-only tools for finding supported API operations, inspecting their implementation requirements, and returning the relevant source documentation.


To get started:


1. Create or sign in to an[Alpaca Trading API account](https://app.alpaca.markets/signup?ref=alpaca.markets) and generate paper trading API keys.
2. Follow the[Trading API MCP Server documentation](https://docs.alpaca.markets/docs/alpaca-mcp-server?ref=alpaca.markets) or[GitHub repository](https://github.com/alpacahq/alpaca-mcp-server?ref=alpaca.markets) to connect the server to a supported MCP client.
3. Restart the client after saving the configuration so it can load the server and its available tools.
4. Open a new conversation and begin with a read-only documentation request:


“Which Alpaca API operation should I use to replace an existing order? Show the required parameters, constraints, response fields, and source documentation. Use documentation tools only.”


A successful response is expected to identify the relevant operation, explain the documented requirements, and include links to the underlying Alpaca documentation.


### **Already Using Alpaca’s Trading MCP Server V2?**


Documentation access is available in the Trading MCP Server version 2.2.1.


If you run the server with` uvx` , refresh the package and verify the installed version with:


```text
uvx alpaca-mcp-server@latest --version
```


After updating:


1. Restart your MCP client so it loads the current tool set.
2. Open a new conversation so the assistant receives the updated tool definitions.
3. Verify documentation access with the prompt: “Find the Alpaca API documentation for replacing an existing order. Return the operation, required parameters, constraints, response fields, and source links. Use documentation tools only.”


If the documentation tools do not appear, confirm that the installed version includes the feature, fully restart the client, and review the current troubleshooting guidance in the Trading MCP Server documentation.


Users still running Trading MCP Server V1 should follow the V1-to-V2 migration guidance before completing these steps. Review the[Trading API MCP Server Documentation](https://docs.alpaca.markets/docs/alpaca-mcp-server?ref=alpaca.markets) or[the GitHub repository](https://github.com/alpacahq/alpaca-mcp-server?ref=alpaca.markets) for complete setup instructions.


[Alpaca’s Trading MCP Server V2](https://alpaca.markets/blog/alpaca-launches-mcp-server-v2/) constructs most of its tools from Alpaca’s OpenAPI specifications, which describe operations, parameters, and response schemes. The new documentation adds context around those operations, including what they are designed to do, which inputs and constraints apply, and what their responses return.


This connects a user’s intended workflow to the relevant API operation and source documentation. That information can support an MCP tool call, a direct API request, or a broader workflow.


## **Additional Resources**


For additional AI tools, check out Alpaca’s[Skills Library for AI Agents](https://alpaca.markets/blog/alpaca-launches-skills-library-for-ai-agents/) and[CLI](https://alpaca.markets/blog/alpaca-introduces-cli-for-trading-api/) . If you’re looking to learn more about what other Alpaca users are building in the AI and agentic space, here are a few examples:


- [How to Get Started with Machine Learning in Trading](https://alpaca.markets/learn/how-to-get-started-with-machine-learning-in-trading?ref=alpaca.markets)
- [From Value Investing to Systematic Trading: Building a Multi-Strategy Backtesting Dashboard with AI and Alpaca](https://alpaca.markets/learn/from-value-investing-to-systematic-trading-building-a-multi-strategy-backtesting-dashboard-with-ai-and-alpaca?ref=alpaca.markets)
- [Building a Multi-Agent AI Trading System on Alpaca](https://alpaca.markets/learn/building-a-multi-agent-ai-trading-system-on-alpaca?ref=alpaca.markets)
- [From Quant Workflows to LLM-Assisted Trading with Alpaca](https://alpaca.markets/learn/from-quant-workflows-to-llm-assisted-trading-with-alpaca?ref=alpaca.markets)
- [Agent-M: An Autonomous Multi-Agent Trading Platform Using Alpaca](https://alpaca.markets/learn/agent-m-an-autonomous-multi-agent-trading-platform-using-alpaca?ref=alpaca.markets)


Still have questions? Contact us at[\[email protected\]](https://alpaca.markets/cdn-cgi/l/email-protection#dba8aeababb4a9af9bbab7abbab8baf5b6baa9b0beafa8) or visit our[documentation](https://docs.alpaca.markets/?ref=alpaca.markets) .


---


*The content of this article is for general informational purposes only. All examples are for educational and illustrative purposes only.*


*Information and outputs generated through Alpaca’s MCP Server, or connected AI agents are provided for educational and informational purposes only and do not constitute investment advice or a recommendation to buy, sell, or hold any security. AI-generated outputs may contain errors, omissions, or inaccuracies and should be independently reviewed and verified before use or reliance. Alpaca does not recommend any specific security, or investment strategy. Users should conduct their own due diligence before making investment decisions. All firms mentioned operate independently and are not responsible for one another's products or services.*


*Securities brokerage services are provided by Alpaca Securities LLC (dba "Alpaca Clearing"), member*[FINRA](https://www.finra.org/?ref=alpaca.markets) */*[SIPC](https://www.sipc.org/?ref=alpaca.markets) *, a wholly-owned subsidiary of AlpacaDB, Inc. Technology and services are offered by AlpacaDB, Inc.*


*This is not an offer, solicitation of an offer, or advice to buy or sell securities or open a brokerage account in any jurisdiction where Alpaca Securities is not registered or licensed, as applicable.*
