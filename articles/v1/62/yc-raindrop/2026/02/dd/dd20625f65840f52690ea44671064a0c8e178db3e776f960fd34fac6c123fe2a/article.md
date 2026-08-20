---
schema_version: "1.0.0"
document_id: "dd20625f65840f52690ea44671064a0c8e178db3e776f960fd34fac6c123fe2a"
company_key: "yc-raindrop"
company: "Raindrop"
source_id: "yc-raindrop-news-import-6a6050810bab"
canonical_url: "https://www.raindrop.ai/blog/agent-self-diagnostics/"
published_at: "2026-02-25T00:00:00+00:00"
first_seen_at: "2026-07-24T11:14:29.327591+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:1cb8c9ac8d1c416dc1770506bcfc17e7071cc2736df732f7c4de5f6d648c913c"
---

# Introducing Agent Self Diagnostics

We're excited to launch Self Diagnostics. Self Diagnostics allows agents to proactively self-report failures to their dev team.


Agents are exponentially smarter than before. They often understand their own failures better than we do. Imagine your agent keeps failing to install a package, or can't find something the user is asking for in your docs. With Self Diagnostics, it can just... tell you.


## Default signal categories


Out of the box, the agent will detect and report four categories of issues:


-


**Missing context** : Critical information, credentials, or access is missing and the user cannot provide it


-


**Repeatedly broken tool** : A tool has failed or not returned the expected response after multiple attempts


-


**Capability gap** : The task requires a tool, permission, or capability the agent doesn't have


-


**Complete task failure** : The agent was unable to accomplish the task despite making genuine attempts


You can also define your own custom categories.


Each report includes a description from the agent (e.g., "The search API returned a 503 after two retries"), so you get human-readable context, not just an error code. These reports show up in a dedicated Self Diagnostics tab in the Raindrop dashboard. You can track issue rates over time, drill into individual incidents, and get alerted when problems spike.


## How to integrate


If you're already using the Raindrop SDK with the Vercel AI SDK, it's one line:


typescript


```text
const     {     generateText  ,     streamText     }     =     raindrop  .  wrap  (  ai  ,     {
context  :     {     userId  :     "  user_123  "  ,     eventName  :     "  support-agent  "     }  ,
selfDiagnostics  :     {
enabled  :     true  ,
}  ,
}  )  ;
```


For full SDK integration details, see:


-


[Vercel AI SDK - Self Diagnostics](https://www.raindrop.ai/docs/sdk/vercel-ai-sdk#self-diagnostics-optional)


-


[Claude Agent SDK - Self Diagnostics](https://www.raindrop.ai/docs/sdk/claude-agent-sdk#self-diagnostics-optional)


## Customize it for your domain


You can define your own signal categories and add domain-specific guidance. When you provide custom signals, they


**replace** the defaults entirely:


typescript


```text
selfDiagnostics  :     {
enabled  :     true  ,
// Copy the SDK defaults, then customize as needed:
signals  :     {
missing_context  :     {
description  :
"  You cannot complete the task because critical information, credentials, or access is missing and the user cannot provide it.   "     +
"  Do NOT report this for normal clarifying questions - only when you are blocked.  "  ,
sentiment  :     "  NEGATIVE  "  ,
}  ,
repeatedly_broken_tool  :     {
description  :
"  A tool has failed on multiple distinct attempts in this conversation, preventing task completion. You are sure the tool exists, and you have tried to use it, but it has failed.   "     +
"  A single tool error is NOT enough - the tool must be persistently broken across retries.  "  ,
sentiment  :     "  NEGATIVE  "  ,
}  ,
.  .  .
}  ,
}
```


Check out the


[full documentation](https://www.raindrop.ai/docs/platform/self-diagnostics) to see everything Self Diagnostics can do.
