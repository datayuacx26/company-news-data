---
schema_version: "1.0.0"
document_id: "6f57c50ed3205658dc0828b6e7357b1101feae258022de16b98602f302371a2e"
company_key: "yc-manufact"
company: "Manufact (formerly mcp-use)"
source_id: "yc-manufact-news-import-914a697bc101"
canonical_url: "https://manufact.com/blog/manufact-chatgpt-app"
published_at: "2026-06-11T00:00:00+00:00"
first_seen_at: "2026-07-22T03:18:57.853834+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:8803098ee10b5e5bef0e4c7711640bebf45e3140e6f59c5cef4f3c12aacfc315"
---

# Manufact is live in the ChatGPT App Store

More and more people use software through third-party agents rather than dashboards. That shift is as true for us as it is for everybody else, so we shipped a ChatGPT app.


Manufact is now an app on ChatGPT. Install it from the ChatGPT App Store and manage your MCP servers, deployments, logs, analytics, and your team directly from a conversation.


[Install Manufact on ChatGPT →](https://chatgpt.com/apps/manufact/asdk_app_69cbfd610c3881919163e080ede4d042)


When you're building an MCP server, the failures that matter happen after you deploy, like a tool call that errors against real client traffic. The Manufact app gives ChatGPT direct access to your real-time build and runtime logs. Ask it why the latest deployment failed and it pulls the build logs. Ask it why a tool is erroring and it tails the runtime logs, filtered to the requests that failed. It can also trigger a new deployment (including from a non-production branch) or stop one that's misbehaving.


For monitoring, ask for the state of your servers and the app reports deployment status, recent gateway requests with their status codes and durations, and an observability summary of events and error rates. It also renders Manufact's analytics dashboards inline in ChatGPT, like which MCP clients are connecting to your server and which tools they're calling.


Deployment analytics rendered as UI inside ChatGPT


Beyond logs and analytics, the app covers the day-to-day of running MCP servers on Manufact, like checking custom domains or inviting teammates.


## Getting started


[Install the app](https://chatgpt.com/apps/manufact/asdk_app_69cbfd610c3881919163e080ede4d042) , sign in with your Manufact account, and ask ChatGPT about your servers. If you don't have a server deployed yet, start with[Manufact Hosting](https://manufact.com/platform/hosting) : connect a GitHub repo and ship your first MCP server in one click.


The Manufact app is itself an MCP app, built with[mcp-use](https://github.com/mcp-use/mcp-use) and hosted on Manufact, the same stack we run for our customers. Before submitting it, we iterated against our own[publishing checks](https://manufact.com/platform/publishing-checks) until the app passed every ChatGPT App Store requirement, and our[evals](https://manufact.com/platform/cross-client-testing) keep running against it after every deploy.


If you want your own MCP server in front of ChatGPT users:


[Create your Manufact account →](https://manufact.com/signup?utm_source=blog&utm_medium=website&utm_campaign=chatgpt-app-launch&utm_content=manufact-chatgpt-app)
