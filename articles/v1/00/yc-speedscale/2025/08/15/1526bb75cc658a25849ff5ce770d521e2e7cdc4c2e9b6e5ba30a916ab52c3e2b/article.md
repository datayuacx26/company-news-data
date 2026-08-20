---
schema_version: "1.0.0"
document_id: "1526bb75cc658a25849ff5ce770d521e2e7cdc4c2e9b6e5ba30a916ab52c3e2b"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/peeking-under-the-hood-of-cursor/"
published_at: "2025-08-05T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T20:56:33.291545+00:00"
content_hash: "sha256:139fbd51d708f2eae550a61071aef1ab10d4d9d701367750b57b8963d6fe84cc"
---

# Peeking Under the Hood of Cursor's API Calls

# **Peeking Under the Hood of Cursor’s API Calls**


Cursor is one of the go-to AI-native code editors for developers. Because it’s built on Visual Studio Code, it provides a pretty smooth path between traditional IDEs and agentic AI. But what’s actually happening behind the scenes when you ask it to write code, generate a test, or debug an issue? Who and what is it talking to behind the scenes? Can I prevent data leakage or do I need to add another layer to my tin foil hat?


To answer these questions, I used proxymock to inspect the network traffic flowing from the Cursor IDE.[proxymock](https://proxymock.io/) is a network proxy that records complete API requests and responses into markdown files. It’s like Wireshark for software developers instead of packet spelunkers.


This doc is not an attempt to reverse engineer Cursor but it does give some insight into how the app communicates and with which services. It’s always interesting to see how smart people build their tools and Cursor is definitely on the cutting edge.


🎯 Key Takeaways


- We wrapped Cursor with proxymock and decoded its gRPC. On a prompt it sends your workspace launch.json, selected code snippets (first 4K per file), a bulk walk of your directory structure, and your git status to api2.cursor.sh.
- launch.json is uploaded verbatim, so any secrets people leave in it (AWS_SESSION_TOKEN, API keys) go with it.
- In auto mode Cursor routes across a portfolio of models (Claude, Gemini, GPT, DeepSeek) and nudges you between them server-side.
- It generates pre-signed S3 URLs to upload per-user debugging-data zips, and sends telemetry through Sentry and Microsoft’s VS Code endpoints.


Here’s a breakdown of some interesting findings, ranked by my own paranoia:


Behind the scenes, Cursor leverages both HTTP and gRPC communication protocols. Many of the observed API calls, particularly those related to marketplace interactions and general telemetry, operate over standard HTTP. However, Cursor also extensively utilizes gRPC for its core AI and synchronization services. This is pretty typical for new software builds because you need backwards compatibility while also wanting the new hotness of gRPC for efficiency and data type synchronization. However, using gRPC means that Cursor may not work with some corporate firewalls and some calls may time out.


For a more comprehensive understanding of gRPC, you can refer to its official[documentation](https://grpc.io/docs/what-is-grpc/introduction/) or our[gRPC developer’s guide](https://speedscale.com/blog/getting-started-with-grpc-a-developers-guide/) .


Let’s take a look at the calls being made during a basic session:


### Prompt Execution


` api2.cursor.sh/aiserver.v1.ChatService/GetPromptDryRun`


Let’s get straight into the interesting stuff.


This appears to be the API that contains the prompt you type into the chat window and the context necessary to respond to the prompt. This is a gRPC call containing a rather large binary protobuf containing a few key components:


1. Details about the workspace (` launch.json` )
2. Relevant code snippets
3. A bulk upload of the directory structure and some files contained therein
4. The git status of the repository


Let’s go through each subsection of the request body, decoded into JSON:


## **Workspace**


```text
"13:LEN"  :   "a2b04e8e-c9a5-4e22-a0a9-70914b1a0838"  ,
"1:LEN"  :   "give me a summary of this codebase"  ,
```


The prompt itself is embedded within the first field of the protobuf request. This is the plain text prompt so really nothing to see here. However, this is more interesting:


```text
"1:LEN"  :   "speedctl/.vscode/launch.json"  ,
"2:LEN"  :   "{
// Use IntelliSense to learn about possible attributes.
...etc
```


**Embedded field Field 15 contains the launch.json for my workspace** . Now, I always follow best practices so I would never put environment variables, API Keys or other sensitive information into my` launch.json` . However, I have friends that like to live recklessly and so they frequently see things like` AWS_SESSION_TOKEN` and` SPEEDSCALE_API_KEY` in these messages. There’s no way any of the readers of this blog would do something dumb like this but just in case you should check out` dotenv` and` envrc` to clean this up.


## **Code snippets**


```text
"1:LEN": [
{
"13:LEN": "a2b04e8e-c9a5-4e22-a0a9-70914b1a0838",
"1:LEN": "give me a summary of this codebase",
"29:VARINT": 1,
"2:VARINT": 1,
"3:LEN": [
{
"10:VARINT": 0,
"1:LEN": "speedctl/mcp/server.go",
"2:VARINT": 1,
"3:LEN": [
"package mcp


import (
context
errors,
fmt,
net/http,
os,
strings,
sync,
time,
...
```


Hmmm, well it appears we are now uploading selective bits of the code base. Only the first 4K of each file is uploaded which makes sense given the context window and limitations of the LLM. I’m not sure how it’s choosing which files to send but it seems that in addition to the bulk code it is also uploading selected lines along with some kind of continuously incrementing offset for each line.


```text
"8:LEN": [
{
"1:LEN": "package main",
"2:I32": 1065353216
},
{
"2:I32": 1073741824
},
{
"1:LEN": "import (",
"2:I32": 1077936128
},
```


## **Directory Structure**


Cursor appears to do a bulk upload of the directory structure of the repository.


```text
{
"1:LEN"  :   "firehose"  ,
"2:LEN"  : {
"3:VARINT"  :   3  ,
"4:VARINT"  :   0  ,
"5:LEN"  : [
{
"1:LEN"  :   "queue.go"
},
{
"1:LEN"  :   "rrpairs_test.go"
},
{
"1:LEN"  :   "rrpairs.go"
}
]
}
},
```


There are hundreds of these walking the entire repository. I ran this on a sizable monorepo with over a million lines of code so it’s quite a bit.


## **Git status**


```text
"70:LEN"  :   "On branch master
Your branch is behind 'origin/master' by 2 commits, and can be fast-forwarded.
(use   \"  git pull  \"   to update your local branch)
nothing to commit, working tree clean
"  ,
```


In my opinion, this data is necessary for Cursor to do its job so this is where we talk about boring grown up stuff like the tradeoff between privacy and capability. Unless you have a stack of 5090 GPUs under your desk then you’re going to have to send this kind of info to the great NVIDIA cluster in the sky to get results. Having said that, if you are working in a corporate environment you’re going to have to turn on privacy mode. Not doing so is going to make your boss very unhappy. Also, this is where you hope Cursor never gets hacked. But let’s be honest, their security is probably better than your company’s anyway.


### AI Model Strategy Intelligence


` api2.cursor.sh/aiserver.v1.AiService/GetDefaultModelNudgeData`


Ever wonder what LLMs Cursor uses when you put it in “Auto” mode? This endpoint answers that question by revealing Cursor’s portfolio of AI models and how it decides which one to recommend to you. The traffic shows they are leveraging cutting-edge models like $claude-3.7-sonnet-thinking-max$, $gemini-2.0-flash-thinking-exp$, and $deepseek-r1$. What’s interesting is that this isn’t just a static list. It appears that Cursor is providing real-time model recommendations that nudge users towards different models based on their behavior, project context, or subscription tier. Here’s the response body detailing which models are available:


```text
{
"1:LEN"  :   "1749756077899"  ,
"3:LEN"  : [
"claude-3.7-sonnet-thinking-max"  ,
"claude-3.7-sonnet-max"  ,
"gemini-2.0-flash-thinking-exp"  ,
"gemini-2.0-flash"  ,
"gpt-4o-mini"  ,
"claude-3.5-haiku"  ,
"deepseek-v3"  ,
"deepseek-v3.1"  ,
"deepseek-r1"
]
}
```


### Usage Telemetry


` api2.cursor.sh/aiserver.v1.ClientLoggerService/GetDebuggingDataUploadUrl`


```text
{
"1:LEN"  :   "https://cursor-user-debugging-data.s3.us-east-1.amazonaws.com/github%7Cuser_01J5R1XH1AR0PQFV3HEY9RKWAJ/1753976451401-debugging-data.zip?X-Amz-Algorithm=AWS4-HMAC-SHA256  \u0026  X-Amz-Content-Sha256=UNSIGNED-PAYLOAD  \u0026  X-Amz-Credential=yadda yadda yadda"
}
```


Well now this is interesting (not the Seinfeld reference - I added that to remove an Auth token). This endpoint appears to generate pre-signed AWS S3 URLs for uploading user data. The destination is a bucket named` cursor-user-debugging-data.s3.us-east-1.amazonaws.com` . The file paths are user-specific, following a pattern like` /github%7Cuser_ID/timestamp-debugging-data.zip` . This indicates a system for comprehensive, user-specific data collection that monitors interactions, errors, and behavior patterns. Collecting this data is par for the course for most desktop apps, but the delivery mechanism is unusual. It’s common to see API systems collecting this data because it usually consumes lightweight nuggets like “user clicked here.” Having said that, pushing files to S3 is basic, effective and reliable - and also allows much higher volume uploads. Now, if you were in the mood to be irritating you could upload large bulk data to the bucket to run up their AWS bill. Cursor is presumably full of smart AI engineers so they could easily protect against this.


But I still wonder, what’s in the zip files? I wasn’t able to figure that out because I didn’t observe a zip file upload. My guess is that this data is only uploaded during an error or on some time interval but I didn’t have an observation to work from.


### AI Context Database


` api2.cursor.sh/aiserver.v1.AiService/AvailableDocs`


Request:


```text
"1:LEN"  : [
{
"1:LEN"  :   "http://accord-framework.net/docs/html/N_Accord.htm"  ,
"2:LEN"  : {
"1:LEN"  :   "http://accord-framework.net/docs/html/N_Accord.htm"  ,
"2:LEN"  :   "Accord.NET"  ,
"5:VARINT"  :   1
}
},
...
```


For an AI to be effective, it needs context. This endpoint downloads a massive dataset (over 19KB in the observed response) of documentation that fuels the AI’s knowledge base. The list is extensive, containing documentation for countless frameworks, libraries, and APIs across all major platforms. This reveals the sheer scope of Cursor’s AI training data and the specific technologies it’s targeting to provide expert-level assistance on.


### AI Session Synchronization


` api2.cursor.sh/aiserver.v1.AiService/ServerTime`


Request:


```text
{
"1:I64"  :   4790007091812002918  ,
"2:I64"  :   4790007091812003840
}
```


A simple but vital endpoint. The keen eyed observer will note that this is the protobuf timestamp[format](https://protobuf.dev/reference/protobuf/google.protobuf/#timestamp) (NANOS_FIELD_NUMBER and SECONDS_FIELD_NUMBER). Proxymock automatically converts binary Protobuf into[JSON](https://docs.speedscale.com/proxymock/guides/grpc/#body-format) so this is just two Int64 values.


This endpoint appears to provide server-side timestamps used to synchronize conversations and actions with the AI. This is essential for session management, especially in a distributed system where multiple AI processes might be collaborating to generate a response.


### Monetization Intelligence


` api2.cursor.sh/auth/full_stripe_profile`


This endpoint is all about the money. It validates a user’s subscription status and payment details through Stripe.


### Sentry Error Tracking


` metrics.cursor.sh/api/4508016051945472/envelope/`


Cursor uses Sentry for error tracking and application monitoring. This endpoint sends detailed telemetry, including a user’s machine ID and other device fingerprints. It’s used for session tracking, logging errors, and gathering usage analytics to improve the application’s stability and performance.


### Extension Marketplace


` marketplace.cursorapi.com/_apis/public/gallery/extensionquery`


Cursor has its own extension marketplace. But why can’t they just use Microsoft’s? Because Microsoft is not known for sharing their toys with mortal enemies.


### Microsoft A/B Testing


` default.exp-tas.com/vscode/ab`


This endpoint, pointing to a Microsoft service, is used for A/B testing. It allows Cursor to roll out new features and capabilities to small cohorts of users. It controls feature flags, enabling experimental functionality and giving us a sneak peek at features that may be coming soon.


### Microsoft Telemetry


` mobile.events.data.microsoft.com/OneCollector/1.0`


This is a standard Microsoft data collection endpoint, likely inherited from the VS Code base. It gathers telemetry about the development environment, including analytics on Git repository usage and other standard user actions.


## **Final Thoughts**


Cursor operates a highly sophisticated AI model recommendation engine designed to optimize the user experience and, presumably, its own operational costs. This is coupled with an extensive user behavior monitoring system. While it may not be uploading your password keychain, a few ground rules might be helpful. Here is a brief summary of how to limit data leakage and other headaches while using Cursor:


- Follow best practices and store sensitive data in safe places (ref: dotenv, envrc, secrets, etc)
- Use Privacy Mode in a corporate environment
- If you’re having connectivity issues, turn off your corporate VPN or make sure it supports HTTP2 and gRPC (not all do)
- Maybe don’t use it on code that you really need to keep secret


Hopefully this quick look into how Cursor communicates with external systems gives you some confidence. If you want to do this yourself and impress your friends, check out[proxymock.io](https://proxymock.io/) .


## Common questions


### What does Cursor send to its servers when you prompt it?


On a prompt, Cursor sends a gRPC request to` api2.cursor.sh` containing your workspace launch.json, relevant code snippets (the first 4K of each selected file), a bulk upload of your directory structure, and your repository’s git status. On a large monorepo that is a lot of directory walking.


### Can Cursor leak secrets in my project?


It can if you put them where it looks. Cursor uploads your launch.json verbatim, so anything sensitive left in it, like an AWS_SESSION_TOKEN or an API key, goes to the server with it. Keep secrets in dotenv or envrc and turn on Privacy Mode in a corporate setting.


### Which AI models does Cursor use in auto mode?


Its` GetDefaultModelNudgeData` endpoint returned a portfolio that included Claude 3.7 Sonnet (thinking and max), Gemini 2.0 Flash, GPT-4o-mini, Claude 3.5 Haiku, and DeepSeek v3 and r1. Auto mode is not a static pick, Cursor nudges you between models server-side based on context and subscription tier. (Model list as observed in August 2025.)


### How can I see what Cursor sends myself?


Wrap it with proxymock, which records complete API requests and responses and converts binary protobuf and gRPC into readable JSON. That is how we decoded these calls. It is like Wireshark for developers instead of packet spelunkers.
