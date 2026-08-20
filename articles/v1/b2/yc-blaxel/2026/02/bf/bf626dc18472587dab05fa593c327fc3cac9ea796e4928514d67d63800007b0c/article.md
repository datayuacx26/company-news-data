---
schema_version: "1.0.0"
document_id: "bf626dc18472587dab05fa593c327fc3cac9ea796e4928514d67d63800007b0c"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/efficiently-connect-agents-and-apis-with-code-mode-on-blaxel"
published_at: "2026-02-24T21:22:19+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T22:03:45.188546+00:00"
content_hash: "sha256:8b23aaf751e7ac2845e8d860368f79f7bb39b3457b1c1984454043b070b2a46d"
---

# Efficiently connect agents and APIs with “code mode” on Blaxel

Token efficiency is an important topic for AI developers these days.


The more tools you give an AI agent, the more information it has to deal with, and the higher the token count. This is both inefficient and expensive.


“Code mode”, first proposed in a[Cloudflare blog post](https://blog.cloudflare.com/code-mode/) , is an innovative answer to this problem. A subsequent[Anthropic blog post](https://www.anthropic.com/engineering/code-execution-with-mcp) about a similar idea, "code execution over MCP", explains why this approach offers significant token savings over traditional MCP tool calling methods.


"Code mode" is now natively supported on Blaxel for OpenAPI-compatible APIs. With this, you can expose any OpenAPI specification to your agents as an MCP server hosted on Blaxel.


## How it works


1.


Use the Blaxel CLI to deploy an MCP server in code mode. Replace the` OPENAPI_REFERENCE` variable with your OpenAPI specification URL.


bash


` bl apply -f -


<<


EOF


apiVersion: blaxel.ai/v1alpha1


kind: Function


metadata:


displayName: Petstore API Code Mode


name: petstore-code-mode


spec:


runtime:


type: mcp


image: blaxel/code-mode:latest


memory: 2048


envs:


- name: OPENAPI_REFERENCE


value: https://petstore3.swagger.io/api/v3/openapi.json


EOF


`


2.


Retrieve the MCP server's endpoint url:


bash


` bl get mcp my-api-code-mode -o json


|


jq -r


'.\[\] | .metadata.url'


`


3.


Feed this URL to your agent to have it connect to the MCP server as usual and start using the API.


The two tools available are` search` , which enables an AI agent to evaluate the OpenAPI specification, and` execute` , which enables the agent to run JavaScript code that calls the actual API endpoints needed for the requested task. All the agent-generated code runs inside an auto-scaling sandbox with a 10-minute idle TTL, ensuring complete isolation and security.


## API authentication


API authentication is fully supported. For example, if the target OpenAPI specification defines an authentication scheme called` ApiKeyAuth` , authenticate against the API by defining an environment variable in your MCP deployment schema called` AUTH_APIKEYAUTH` with the corresponding secret:


yaml


`


envs


:


-


name


:


OPENAPI_REFERENCE


value


:


https


:


//api.example.com/openapi.json


-


name


:


AUTH_APIKEYAUTH


value


:


$


{


secrets.AUTH_APIKEYAUTH


}


`


Read more about[our implementation of code mode](https://docs.blaxel.ai/Functions/Code-mode) and then try it out for yourself: we've put together a[tutorial about using a Claude SDK agent in code mode with Blaxel](https://docs.blaxel.ai/Tutorials/Claude-Agent-SDK-Code-Mode) . All you need is your[Anthropic API key](https://platform.claude.com/) , a[Blaxel account](https://blaxel.ai/) , and the[Blaxel CLI](https://docs.blaxel.ai/cli-reference/introduction) .


Feedback?[Tell us in Discord](https://discord.gg/enAfyZFWHW) .
