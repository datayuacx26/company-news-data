---
schema_version: "1.0.0"
document_id: "73485c398fba844578c107aa949207541822b41d130aa34c84ab7160fdbae525"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/peeking-under-the-hood-with-claude-code/"
published_at: "2025-12-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T22:24:49.305726+00:00"
content_hash: "sha256:44bae82c619a0bd3e3f9b67b5ec036b45f2a1b9041c559fe71d2235f820a2904"
---

# Peeking Under the Hood with Claude Code

# **Peeking Under the Hood with Claude Code**


Claude is one of the go-to AI-native code editors for developers. Because it’s a simple chatbot interface housed inside a familiar CLI, it provides a pretty smooth path between traditional IDEs and agentic AI. But what’s actually happening behind the scenes when you ask it to write code, generate a test, or debug an issue? Who and what is it talking to behind the scenes? Can I prevent data leakage or do I need to add another layer to my tin foil hat?


To answer these questions, I used proxymock to inspect the network traffic flowing from the Claude IDE.[proxymock](https://proxymock.io/) is a network proxy that records complete API requests and responses into markdown files. It’s like Wireshark for software developers instead of packet spelunkers.


This doc is not an attempt to reverse engineer Claude but it does give some insight into how the app communicates and with which services. It’s always interesting to see how smart people build their tools and Claude is definitely on the cutting edge.


🎯 Key Takeaways


- We wrapped Claude Code with proxymock. Prompts go to api.anthropic.com/v1/messages as plain JSON (REST), not the gRPC that Cursor uses, which survives corporate firewalls better.
- Claude rewrites your prompt into a longer, more specific version before running it, and drives structured tool calls that can ask to run commands on your machine (permissioned and sandboxed).
- It sends directory listings and file contents as tool results, so what’s in your working directory is what gets shared.
- Telemetry is heavy. Command-level events go to statsig.anthropic.com (Statsig is owned by OpenAI) and detailed logs go to Datadog, tagged by model and user.


Here’s a breakdown of some interesting findings, ranked by my own paranoia:


Behind the scenes, Claude leverages the HTTP communication protocols with JSON payloads (aka REST). This is differnt than Cursor AI because that assistant uses HTTP 2.0 + gRPC because of its compactness and simplicity of development. However, REST is better supported in large enterprise environments and is not as likely to get mangled or blocked by corporate firewalls. I suspect Anthropic knows where their tool is likely to be used and has architected accordingly.


If you’d like to see how to dive into recording Claude Code on your local desktop check out our[how to video](https://youtu.be/EpBFF5PhQqc) .


Let’s take a look at the calls being made during a basic session:


## **Prompt Execution**


` api.anthropic.com/v1/messages`


Let’s get straight into the interesting stuff. The prompt you enter into the chatbot is housed in the request body JSON at path` messages\[0\].content.text` . Here is my example prompt in context:


```text
{
"model"  :   "claude-haiku-4-5-20251001"  ,
"messages"  : [
{
"role"  :   "user"  ,
"content"  : [
{
"type"  :   "text"  ,
"text"  :   "look at the code in this repository and give suggestions for how it can be improved for clarity and readability"
}
]
}
]
}
```


You also can see the model version in use and its publish date. One interesting thing if you continue looking at the user prompts is that Claude seems to re-interpret my prompt into something more intelligent and comprehensive:


```text
Explore this Go repository to understand its structure, purpose, and code organization. I need to provide suggestions for improving clarity and readability. Please:\n\n1. Understand what this project does and its main components\n2. Review the main Go files and identify the code structure\n3. Look for areas that could be improved for clarity and readability such as:\n   - Complex functions that could be simplified\n   - Missing or unclear documentation\n   - Naming conventions\n   - Code organization\n   - Error handling patterns\n   - Code duplication\n   \nProvide a comprehensive overview of the codebase and specific examples of code that could be improved.
```


That is way smarter than what I originally wrote. This example shows two things you’ll see a lot of this analysis: 1) thoughtful context and prompt re-engineering and 2) clear use of tools beyond the native LLM. It becomes clear as you go through the Claude API interaction model that the designers were thinking about the practical and targeted use of the overall product vs just model weight tuning.


## **Tools Structure**


Claude appears to have a reasonably well thought out interaction model that consists of tool commands and tools responses. You can see these messages structured throughout the` /messages` API threads. Here is an example:


```text
{
"role"  :   "user"  ,
"content"  : [
{
"type"  :   "tool_result"  ,
"content"  :   "EISDIR: illegal operation on a directory, read"  ,
"is_error"  :   true  ,
"tool_use_id"  :   "toolu_01EbbbCNQPuhvEvtcunH85kv"
},
{
"tool_use_id"  :   "toolu_01X5FKs1ac3GiYhoEJkTqtSv"  ,
"type"  :   "tool_result"  ,
"content"  :   "total 56  \n  drwxr-xr-x   8 matthewleray  staff   256 Dec 29 14:56 .  \n  drwxr-xr-x  29 matthewleray  staff   928 Dec 13 17:44 ..  \n  -rw-r--r--   1 matthewleray  staff   106 Nov  3 11:27 go.mod  \n  -rw-r--r--   1 matthewleray  staff   318 Nov  3 11:27 go.sum  \n  -rw-r--r--   1 matthewleray  staff  8502 Nov  3 11:27 main.go  \n  drwxr-xr-x   4 matthewleray  staff   128 Dec 29 15:11 proxymock  \n  -rw-r--r--   1 matthewleray  staff  1851 Nov  3 11:27 README.md  \n  -rw-r--r--   1 matthewleray  staff  3325 Nov  3 11:27 test_requests.http"  ,
"is_error"  :   false
},
{
"tool_use_id"  :   "toolu_01NgTjeKhC2CjeFbA67AVp3G"  ,
"type"  :   "tool_result"  ,
"content"  :   "/Users/matthewleray/dev/demo/go-postgres/main.go  \n\n  <system-reminder>  \n  CRITICAL: This is a READ-ONLY task. You CANNOT edit, write, or create files.  \n  </system-reminder>"  ,
"is_error"  :   false
}
]
},
```


A few things to notice:


1. Claude is re-engineering the context to make your prompt more specific and useful (congrats Anthropic, making me sound smart is no small task)
2. Tool usage is explicit and structured
3. The remote LLM can ask for things to be run on your local desktop. Yes, it is protected by Claude’s permissions and sandboxing which may or may not make your security team happy. I’ve never known a happy security person but perhaps that’ll change as we get more used to these tools.
4. Claude appears to follow an internal process that includes breaking tasks into sub-tasks without the user asking that to be done (you used to need to explicitly ask). Essentially, it’s acting like a project manager.
5. Sub-tasks are broken into “tool calls” which can be parallelized.
6. UI layouts appear to be explicitly defined using a event+display based structure. Here’s an example:


```text
event: message_start
data: {  "type"  :  "message_start"  ,  "message"  :{  "model"  :  "claude-haiku-4-5-20251001"  ,  "id"  :  "msg_019GhSirXDKaEo36LoQ1q4UM"  ,  "type"  :  "message"  ,  "role"  :  "assistant"  ,  "content"  :[],  "stop_reason"  :  null  ,  "stop_sequence"  :  null  ,  "usage"  :{  "input_tokens"  :  909  ,  "cache_creation_input_tokens"  :  0  ,  "cache_read_input_tokens"  :  0  ,  "cache_creation"  :{  "ephemeral_5m_input_tokens"  :  0  ,  "ephemeral_1h_input_tokens"  :  0  },  "output_tokens"  :  2  ,  "service_tier"  :  "standard"  }}           }


event: content_block_start
data: {  "type"  :  "content_block_start"  ,  "index"  :  0  ,  "content_block"  :{  "type"  :  "text"  ,  "text"  :  ""  }  }
```


⚠️ Caution


Think critically from a security standpoint about what is in the working directory (aka context) of the Claude Code CLI. Imagine that Santa is watching but this is not jolly Santa, this is mean Santa who makes wrapping paper out of your precious source code so everyone can see.


## **Directory Structure**


Claude appears to do selectively perform a bulk upload of the directory structure of the repository.


```text
{
"tool_use_id"  :   "toolu_01X5FKs1ac3GiYhoEJkTqtSv"  ,
"type"  :   "tool_result"  ,
"is_error"  :   false
},
```


In my demo repository this is a trivial list but I ran this on a sizable monorepo with over a million lines of code and it was decidedly not trivial.


💡 Tip


Subdirectories and files take up a lot of tokens. Tokens cost money. You already needed to start organizing your code better and how you have another excuse. Check out this[blog](https://bits.logic.inc/p/ai-is-forcing-us-to-write-good-code) for more discussion.


## **User Details**


` api.anthropic.com/api/claude_cli_profile`


Curious about your billing details? Wonder no longer because you can find it at this endpoint.


```text
{
"account"  : {
"uuid"  :   "67676767-6767-6767-6767-676767676767"  ,
"full_name"  :   "Matthew LeRay"  ,
"display_name"  :   "Matthew"  ,
"email"  :   "matt@speedscale.com"  ,
"has_claude_max"  :   false  ,
"has_claude_pro"  :   false
},
"organization"  : {
"uuid"  :   "67676767-6767-6767-6767-676767676767"  ,
"name"  :   "Omni Consumer Products"  ,
"organization_type"  :   "api_team"  ,
"billing_type"  :   "prepaid"  ,
"rate_limit_tier"  :   "manual_tier_3"  ,
"has_extra_usage_enabled"  :   false
}
}
```


This is where we talk about boring grown up stuff like the tradeoff between privacy and capability. Unless you have a stack of 5090 GPUs under your desk then you’re going to have to send this kind of info to the great NVIDIA cluster in the sky to write code for you.


### **Telemetry with Statsig and Datadog**


The Anthropic team really likes to know how people use their product and when it breaks.


Like a lot, lot.


There are two main telemetry endpoints that get hit quite a bit:


- Command execution information:` statsig.anthropic.com/v1/rgstr`
- Internal logs:` http-intake.logs.us5.datadoghq.com/api/v2/logs`


The` statsig` endpoint gets hit every time the user or the API does anything mildly interesting. We can define “mildly interesting” extremely broadly because it looks like the information includes almost[OTEL](https://opentelemetry.io/docs/concepts/signals/traces/) span levels of detail.` statsig.anthropic.com` is likely a[custom proxy](https://docs.statsig.com/infrastructure/api_proxy/custom_proxy) to send telemetry info to Statsig without cross domain networking issues. Now what’s fun is Statsig was actually acquired by OpenAI, which means Anthropic may be sending detailed product usage data directly to one of their major competitors. To be clear, this is not a security vulnerability nor am I pointing out any funny business, it’s just that Anthropic clearly has a lot of faith in OpenAI’s internal data sharing policies. Anyways, here is an example and some highlights:


```text
"events"  : [
{
"eventName"  :   "tengu_api_query"  ,
"metadata"  : {
"model"  :   "claude-haiku-4-5-20251001"  ,
"messagesLength"  :   "1372"  ,
"temperature"  :   "1"  ,
"provider"  :   "firstParty"  ,
"buildAgeMins"  :   "13118"  ,
"betas"  :   "interleaved-thinking-2025-05-14,context-management-2025-06-27"  ,
"permissionMode"  :   "default"  ,
"querySource"  :   "terminal_update_title"  ,
"sessionId"  :   "1115ba25-6767-6767-6767-516f6767676767"  ,
"userType"  :   "external"  ,
"env"  :   "{  \"  platform  \"  :  \"  darwin  \"  ,  \"  arch  \"  :  \"  arm64  \"  ,  \"  nodeVersion  \"  :  \"  v24.3.0  \"  ,  \"  terminal  \"  :  \"  iTerm.app  \"  ,  \"  packageManagers  \"  :  \"  npm,yarn,pnpm  \"  ,  \"  runtimes  \"  :  \"  node  \"  ,  \"  isRunningWithBun  \"  :true,  \"  isCi  \"  :false,  \"  isClaubbit  \"  :false,  \"  isClaudeCodeRemote  \"  :false,  \"  isConductor  \"  :false,  \"  isGithubAction  \"  :false,  \"  isClaudeCodeAction  \"  :false,  \"  isClaudeAiAuth  \"  :false,  \"  version  \"  :  \"  2.0.75  \"  ,  \"  versionBase  \"  :  \"  2.0.75  \"  ,  \"  buildTime  \"  :  \"  2025-12-20T17:18:51Z  \"  ,  \"  deploymentEnvironment  \"  :  \"  unknown-darwin  \"  }"  ,
"entrypoint"  :   "cli"  ,
"isInteractive"  :   "true"  ,
"clientType"  :   "cli"  ,
"sweBenchRunId"  :   ""  ,
"sweBenchInstanceId"  :   ""  ,
"sweBenchTaskId"  :   ""
},
"user"  : {
"customIDs"  : {
"sessionId"  :   "1115ba25-6767-6767-6767-516f6767676767"  ,
"organizationUUID"  :   "45829fe4-6767-6767-6767-676767676767"  ,
"accountUUID"  :   "83a2ac9b-6767-6767-6767-676767676767"
},
"userID"  :   "ZG9uJ3QgZGVjb2RlIG15IHN0dWZm"  ,
"appVersion"  :   "2.0.75"  ,
"custom"  : {
"userType"  :   "external"  ,
"organizationUuid"  :   "45829fe4-6767-6767-6767-676767676767"  ,
"accountUuid"  :   "83a2ac9b-6767-6767-6767-676767676767"  ,
"subscriptionType"  :   ""  ,
"firstTokenTime"  :   0
},
"statsigEnvironment"  : {
"tier"  :   "production"
}
},
"time"  :   1767038213545
}
```


Some things to notice from the telemetry:


1. Session ID tracks the current interaction set (aka chat stream)
2. organizationUUID, accountUUID and userID are sticky tracking identifiers that are presumably anonymized
3. userID, device_id and other identifiers appear to be used across different endpoints and are concatenated together to form new identifiers


None of these are particularly concerning unless your tin foil hat includes a merit badge for GDPR de-anonymization across data sources. Hopefully none of you actually have this merit badge. I was told it was special when they gave it to me.


The Datadog logging agent should be well understood at this point so I won’t spend energy on it. The only thing to note is that the Datadog tags include segementation by model and user. It also has some interesting metadata additions like` is_running_with_bun` . Typically product teams will start collecting data like this when they’re interested in what ecosystem customers fit their tool into. It also helps them know what other companies they might want to purchase (just kidding). Or maybe not…[bun acquired by Anthropic](https://www.anthropic.com/news/anthropic-acquires-bun-as-claude-code-reaches-usd1b-milestone) .


```text
[
{
"ddsource": "nodejs",
"ddtags": "arch:arm64,client_type:cli,model:claude-haiku-4-5-20251001,platform:darwin,provider:firstParty,user_type:external,version:2.0.75,version_base:2.0.75",
"message": "tengu_api_success",
"service": "claude-code",
"hostname": "claude-code",
"env": "external",
"model": "claude-haiku-4-5-20251001",
"session_id": "1115ba25-6767-6767-6767-67676767676767",
"user_type": "external",
"betas": "interleaved-thinking-2025-05-14,context-management-2025-06-27",
"entrypoint": "cli",
"is_interactive": "true",
"client_type": "cli",
"agent_id": "a2263e7",
"agent_type": "subagent",
"platform": "darwin",
"arch": "arm64",
"node_version": "v24.3.0",
"terminal": "iTerm.app",
"package_managers": "npm,yarn,pnpm",
"runtimes": "node",
"is_running_with_bun": true,
"is_ci": false,
"is_claubbit": false,
"is_claude_code_remote": false,
"is_conductor": false,
"is_github_action": false,
"is_claude_code_action": false,
"is_claude_ai_auth": false,
"version": "2.0.75",
"version_base": "2.0.75",
"build_time": "2025-12-20T17:18:51Z",
"deployment_environment": "unknown-darwin",
"message_count": 1,
"message_tokens": 0,
"input_tokens": 909,
"output_tokens": 130,
"cached_input_tokens": 0,
"uncached_input_tokens": 0,
"duration_ms": 2095,
"duration_ms_including_retries": 2096,
"attempt": 1,
"ttft_ms": 738,
"build_age_mins": 13117,
"provider": "firstParty",
"request_id": "req_011CWbZqmsq1v1oxi7azJ2Gj",
"stop_reason": "end_turn",
"cost_u_s_d": 0.001559,
"did_fall_back_to_non_streaming": false,
"is_non_interactive_session": false,
"print": false,
"is_t_t_y": true,
"query_source": "agent:builtin:Explore",
"query_chain_id": "8a399dcc-9d32-4aa9-a687-fa4dcc50499b",
"query_depth": 1,
"permission_mode": "default"
}
]
```


These events appear to be aggregated and possibly sent a second time to` api.anthropic.com/api/event_logging/batch` .


## **Final Thoughts**


Claude operates a highly sophisticated AI model recommendation engine designed to optimize the user experience and, presumably, its own operational costs. This is coupled with an extensive user behavior monitoring system. Hopefully this quick look into how Claude communicates with external systems gives you some confidence. If you want to do this yourself and impress your friends, check out[proxymock.io](https://proxymock.io/) .


## Common questions


### What does Claude Code send to Anthropic when you prompt it?


Your prompt goes to` api.anthropic.com/v1/messages` as plain JSON, along with the model version and, through structured tool calls, whatever it reads from your working directory such as directory listings and file contents. It uses REST rather than the gRPC that Cursor uses, which tends to survive corporate firewalls better.


### Does Claude Code change my prompt before running it?


Yes. In our capture it rewrote a short prompt into a longer, more specific set of instructions before executing, and it broke the task into sub-tasks and parallel tool calls on its own. It behaves like a project manager, not just a raw model call.


### Can Claude Code run commands on my machine?


It can request to. The remote model issues structured tool calls that can run things on your local desktop, gated by Claude’s permissions and sandboxing. What is in your working directory is effectively context it can read, so think about what lives there.


### What telemetry does Claude Code collect?


A lot. Command-level events go to` statsig.anthropic.com` (Statsig is owned by OpenAI) and detailed logs go to Datadog, tagged by model and user, down to token counts, cost, and flags like is_running_with_bun. Sticky organization, account, and user identifiers tie the events together.
