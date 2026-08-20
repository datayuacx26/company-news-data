---
schema_version: "1.0.0"
document_id: "2545ef533c034d810a98cf2be20fe1c69d3ec8300385df835c8073b004ddfb97"
company_key: "yc-sourceress"
company: "Sourceress"
source_id: "yc-sourceress-news-import-457a07c39d0c"
canonical_url: "https://imbue.com/blog/latchkey"
published_at: "2026-02-27T00:00:00+00:00"
first_seen_at: "2026-07-26T01:22:31.918155+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:df529d3a3d6a609760d7b228f07765cb402fffddc7fabe0d276eae9a5ffbec4b"
---

# Latchkey: Use plain old REST instead of MCP

AI agents are becoming increasingly autonomous. They need to integrate with the broader digital environment and it seems that this will only grow more true over time.


The[Model Context Protocol](https://modelcontextprotocol.io/) standard was developed to enable that. It allows developers to expose functionality in the form of MCP servers, and AI agents to make use of those servers to interact with various existing third-party services.


But wait, we already had good, rich, and widely adopted standards and conventions for machine-to-machine communication in the form of RESTful APIs built on top of HTTP. Why did we need another standard?


Letting AI agents directly use existing APIs had a number of disadvantages at first:


1. It’s a relatively low-level form of access which made it more challenging for agents to come up with the right invocations for a given task.
2. Agents didn’t know the details of individual APIs.
3. People rightfully didn’t want to make API keys part of their prompts.


With LLM and agent capabilities steadily getting better, #1 has empirically stopped being an issue in practice. If anything, the increased flexibility that comes with lower-level primitives plays to the strengths of today’s models.


Similarly, agents now routinely search the web, which means they can usually find the latest documentation for any public API.


This leaves us with #3. How can we let agents use HTTP APIs with minimal overhead and without worrying about authentication?


## A fresh take: Latchkey


Agents are already very good at using[curl](https://curl.se/) to access HTTP APIs. Language models have been trained on tons of examples of curl invocations and many API docs use the curl format for examples.


So what if agents simply used plain old curl calls that they know so well, and another tool took care of adding authentication on the fly?


That’s exactly what our tool[Latchkey](https://github.com/imbue-ai/latchkey) does. It allows agents to simply call APIs by prepending their` curl` invocations with the` latchkey` command:


latchkey curl -X GET https://gitlab.com/api/v4/user


Latchkey then adds the necessary credentials in the form of curl arguments, passes all the arguments to an actual curl process, and returns its output to the caller.


How does it know about the credentials? Users can store them manually in advance, using curl-like syntax, like this:


latchkey auth set gitlab -H "PRIVATE-TOKEN: <token>"


There are more options, including an experimental functionality where the agent can open a browser window and ask the user to log in first. I won’t list them all here; take a look at the[docs](https://github.com/imbue-ai/latchkey?tab=readme-ov-file#direct-usage) for the full picture.


The point is: say no to unnecessary complexity. There’s no need to go through intermediaries. Let’s keep things transparent and simple.


We continue to improve Latchkey. We invite you to try it out - it’s free and open-source (MIT-licensed). Contributions and feedback are very welcome!


## Demo


See an OpenCode agent use Latchkey to:


1. Discover an available API (AWS).
2. Confirm authentication.
3. Use the API to answer a practical question about spending.


Follow along with our builds![Subscribe to Imbue’s email newsletter](https://bit.ly/subscribe-to-imbue-emails) for product updates and events.
