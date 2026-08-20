---
schema_version: "1.0.0"
document_id: "fe349ad8cacaf0e3ab26c8f485e21af8a48ee8b623bf5d8f2be16f12ff8d6bf3"
company_key: "yc-nextmv"
company: "Nextmv"
source_id: "yc-nextmv-news-import-d2226782a65f"
canonical_url: "https://www.nextmv.io/blog/a-first-look-at-the-nextmv-mcp-server"
published_at: "2026-03-26T00:00:00+00:00"
first_seen_at: "2026-08-19T05:42:15.544580+00:00"
fetched_at: "2026-08-19T05:42:17.807879+00:00"
content_hash: "sha256:814eb29a4566b0c2c462b55f4dbda56f84d1a1aa095c106517b5da64b693a824"
---

# A first look at the Nextmv MCP Server

*Editor's note (April 7, 2026): The Nextmv MCP Server is*[now generally available](https://www.nextmv.io/docs/cli/mcp-server) *. This post has been updated to reflect the current release status.*


Last week, my colleague Sebas[hinted at an upcoming announcement](https://www.nextmv.io/blog/a-new-nextmv-cli) . This week, I’m delighted to deliver it: A preview of[the Nextmv MCP Server](https://www.nextmv.io/docs/cli/mcp-server) , our newest addition to the Nextmv platform that opens up novel ways of interacting with and managing decision apps using plain language.


If you’ve ever wanted a faster, easier way to…


- Analyze decision model run failures, solver logs, metric drift, or execution class usage
- Summarize scenario test activity and results and create new run logic on top of those learnings
- Understand decision app setup (i.e., what compute classes are configured) and audit app activity
- Create a workflow that links demand forecast predictions with optimization model decisions
- Perform model rollouts or rollbacks depending on performance and operational criteria


…then I hope you’ll explore the Nextmv MCP Server. See it in action in my video below 👇👇👇 and read on for more details.


At a high level, the Nextmv MCP Server, well, serves as the connective tissue between AI agents like Claude and Copilot and the Nextmv platform. Because the Nextmv platform functions as a decision intelligence (DI) system of record, the Nextmv MCP Server is uniquely positioned in the market to unlock an AI-powered approach to operating and managing decision apps in real-world systems.


Put another (more imaginative) way, if decision model management feels like a maze of twisty passages, Nextmv gives you a map and the Nextmv MCP Server gives you a guide with a flashlight.


While much of the DI ecosystem has focused on AI chatbots and agents for building and interacting with decision model, engine, and solver specifics, the Nextmv MCP Server provides a safe and reliable interface to a user’s Nextmv data, including all decision app run history, test results, metrics and KPIs, visualizations, errors, failures, model version registry, compute classes, and more. And it’s worth noting we’ve designed this experience with safety and reliability in mind to provide a level of deterministic control over how agents use Nextmv.


While coding agents are, of course, capable of shelling out to the` nextmv` CLI to access Nextmv, we've found that the MCP server improves speed and reliability of these agents using our systems.


I hope you’re as excited as I am to share this release with you. So[read the docs](https://www.nextmv.io/docs/cli/mcp-server) , kick the tires, and let us know what you think in[our community forum](https://discuss.nextmv.io/t/nextmv-mcp-server-preview-release/514) . This is one step toward a reliable, auditable, and observable agentic decisioning world powered by Nextmv, so stay tuned and keep DecisionOps-ing.


May your solutions be ever improving 🖖
