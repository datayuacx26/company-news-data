---
schema_version: "1.0.0"
document_id: "200799915df641f900fe486e1b269ed81eaff634e9e0033b3257738fe9173b45"
company_key: "yc-corsair"
company: "Corsair"
source_id: "yc-corsair-news-import-5583797524f1"
canonical_url: "https://corsair.dev/blog/build-reusable-ai-agent-integrations-across-frameworks"
published_at: "2026-08-13T16:11:00+00:00"
first_seen_at: "2026-08-13T18:32:28.639378+00:00"
fetched_at: "2026-08-13T18:32:29.571414+00:00"
content_hash: "sha256:a7f00195c511b88ef7f712b20d80551b28db99b536b12346475ef892e7d2a8b9"
---

# How to Build AI Agent Integrations Once and Reuse Them Across Multiple Frameworks

Most teams do not set out to support more than one AI agent framework. It usually happens gradually. A prototype gets built in LangChain, a new product team prefers the Claude Agent SDK, another squad is experimenting with CrewAI, and somewhere along the way someone adds an MCP server for good measure. Each framework has its own way of defining tools, and each new tool means writing the Slack, Gmail, GitHub, or CRM integration all over again.


This is the quiet tax of building AI agent integrations without a plan for reuse. The API you are calling has not changed. The auth flow has not changed. The data you are fetching has not changed. Only the wrapper around it has, and that wrapper gets rebuilt every single time a new framework enters the picture. This post walks through why that happens, what specifically gets duplicated, and how to structure your integrations so you write the logic once and plug it into any framework you use today or adopt later.


## **Why Supporting Multiple AI Frameworks Can Mean Rebuilding the Same Integrations**


Every AI agent framework ships with its own concept of a "tool." LangChain has its tool decorators and BaseTool classes. CrewAI has its own Tool interface. The OpenAI Agents SDK defines function tools with a particular schema shape. The Claude Agent SDK expects tools registered in yet another format. None of these shapes are compatible with each other, even when the underlying action, say, searching Google Drive or posting to Slack, is identical.


So when a team decides to support multi-framework AI agents, the natural first instinct is to treat each framework as its own project. That means a fresh Slack client for the LangChain build, a second Slack client for the Claude Agent SDK build, and a third if a CrewAI proof of concept gets greenlit. Three codebases now share zero integration code even though they are calling the same Slack endpoints with the same permissions.


This pattern is easy to miss early on because a single integration for a single framework does not feel expensive. It only becomes a problem once you count how much of that code is identical across frameworks. Teams that have already gone deep on one AI agent framework, like the[Claude Agent SDK integrations covered in this developer guide](https://corsair.dev/blog/claude-agent-sdk-integrations-developer-guide) , often run into this exact wall the moment a second framework enters the stack.


## **What Actually Gets Duplicated When You Build Integrations for Each AI Framework?**


If you strip away the framework specific wrapper, the actual work inside most integrations looks almost identical no matter which agent framework is calling it. Here is what typically gets rebuilt, line for line, every time:


- OAuth flows and consent screens for each connected service
- Token storage and token refresh logic so credentials do not silently expire
- Pagination handling for endpoints that return large result sets
- Retry logic and rate limit handling for APIs that throttle or fail intermittently
- Webhook signature verification and event parsing
- Data normalization so the agent gets a clean, consistent shape back
- Error handling that turns a raw API error into something the agent can reason about
- Permission and approval logic for actions that should not run without a human sign off


None of this is specific to LangChain, CrewAI, or any other framework. It is specific to the third party API. A good example of just how much of this work lives outside the framework entirely is the[OAuth walkthrough for connecting an agent to Gmail](https://corsair.dev/blog/connecting-ai-agents-to-gmail-oauth-walkthrough) , which spends most of its time on consent screens, scope selection, and long term token management rather than anything related to a specific agent framework. That complexity exists whether the agent calling Gmail is built in LangChain or the Claude Agent SDK, which is exactly why rebuilding it per framework is such an expensive habit.


## **How Framework-Specific Tooling Creates More Integration Code, Authentication Logic, and Maintenance Work**


The duplication problem gets worse once you look at how tightly most teams couple business logic to a framework's tool definition. A common pattern looks like this: the function that calls the Slack API, the function that formats the response for the agent, and the framework specific tool registration all live in the same file. That might seem convenient at first, but it means the auth handling and the framework adapter are welded together.


The result is that every new AI agent framework you adopt requires touching authentication logic again, even though nothing about the credentials themselves has changed. A team that already built a working Google Drive integration for one framework, as shown in this walkthrough on[connecting Google Drive to the OpenAI Agents SDK](https://corsair.dev/blog/connect-google-drive-to-openai-agents-sdk) , typically has to redo the OAuth scoping, service account handling, and query construction from scratch if they later want the same capability inside a different framework. The Drive API has not changed. The scopes have not changed. Only the tool registration syntax has, and that alone should not require rewriting the whole integration.


This is where maintenance cost compounds. A policy change from a provider, a deprecated API version, or a new required scope now needs to be patched in two or three places instead of one, and it is easy for one of those copies to quietly fall out of date while the others get updated.


## **What Should You Separate From Your AI Framework to Make Integrations Reusable?**


The fix is not a new framework. It is a boundary. If you want reusable AI integrations, the work needs to be split into two distinct layers that do not know about each other:


1. **The integration layer** : everything that talks to the third party API. Credential and token management, request construction, retries, rate limit handling, pagination, webhook processing, and permission checks all live here.
2. **The framework adapter layer** : a thin wrapper that takes what the integration layer already does and exposes it in whatever shape a given AI agent framework expects for a tool.


The test for whether you have drawn this line correctly is simple. If you added a brand new AI agent framework to your stack tomorrow, would you need to touch anything related to authentication, retries, or data shape? If the answer is yes, those concerns are still tangled up with the framework instead of separated from it.


Concretely, that means separating out:


- Credential storage and refresh, independent of any framework
- Request and retry logic, independent of any framework
- The data shape returned to the caller, independent of any framework
- Permission and approval rules, independent of any framework
- Webhook and event handling, independent of any framework


Once those pieces exist on their own, the framework specific code shrinks down to almost nothing: a name, a description, an input schema, and a single function call into the integration layer.


## **How to Build a Framework Agnostic Integration Layer for Your AI Agents**


In practice, a framework agnostic integration layer is just a normal, typed client library that has no idea an AI agent will ever call it. It exposes plain functions. Those functions handle auth, retries, and normalization internally, and they return clean, predictable data. Nothing about them references LangChain, CrewAI, MCP, or any other framework.


Here is a simplified example of what that looks like in TypeScript AI integrations built this way:


// integrations/github.ts


import { github } from '@corsair-dev/github';


import { createCorsair } from 'corsair/core';


const corsair = createCorsair({


plugins: \[github()\],


});


export async function getOpenPullRequests(repo: string) {


return corsair.github.api.pulls.list({ repo, state: 'open' });


}


Notice that this file never mentions an agent, a tool, or a framework. It is just a function that returns open pull requests, with auth, token refresh, and retries handled underneath it. Whether that data is fetched by way of an API sprawling across dozens of quirky endpoints like the ones described in[this breakdown of how Google APIs actually handle auth and rate limits](https://corsair.dev/blog/what-are-google-apis-and-how-do-they-work) , or a simpler service, the pattern is the same: the integration layer absorbs the complexity so nothing above it has to.


This is really what an AI integration layer, or an AI agent integration layer more specifically, is for. It sits between your agents and the outside world, and it is the single place where "how do we talk to GitHub" gets answered, once.


## **How to Reuse the Same Integrations Across AI Frameworks Without Rebuilding Your Tools**


With the integration layer in place, cross framework AI integrations become a matter of writing a small adapter, not a new integration. Each framework gets a thin file that maps its tool format onto the function you already wrote.


For LangChain:


// adapters/langchain.ts


import { tool } from '@langchain/core/tools';


import { getOpenPullRequests } from '../integrations/github';


export const githubPRTool = tool(


async ({ repo }) => JSON.stringify(await getOpenPullRequests(repo)),


{


name: 'get_open_pull_requests',


description: 'List open pull requests for a given repository',


}


);


For the Claude Agent SDK:


// adapters/claude-agent-sdk.ts


import { getOpenPullRequests } from '../integrations/github';


export const githubPRTool = {


name: 'get_open_pull_requests',


description: 'List open pull requests for a given repository',


input_schema: {


type: 'object',


properties: { repo: { type: 'string' } },


required: \['repo'\],


},


handler: async ({ repo }: { repo: string }) => getOpenPullRequests(repo),


};


Both adapters call the exact same function. Neither one contains a single line of auth handling, retry logic, or data normalization, because that work already happened once inside the integration layer. Add a third AI agent framework next quarter, and the only new code needed is another small adapter file, not another rebuild of GitHub, Slack, or Gmail from scratch.


That is the practical payoff of framework agnostic integrations. You are no longer choosing between AI agent frameworks and rebuilding your tools every time you change your mind or your product needs a second one running alongside the first. You build the integration once, keep it accurate as providers change their APIs, and let every framework you use today or add tomorrow plug into the same layer.
