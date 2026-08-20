---
schema_version: "1.0.0"
document_id: "f1e40c3f8d95f279f035c6aacba7f81921181d3c380e9032f5d94e954f6f1ab6"
company_key: "yc-shuttle"
company: "Shuttle"
source_id: "yc-shuttle-rss-52efc69d7cac"
canonical_url: "https://www.shuttle.dev/blog/2025/10/01/build-rust-api-sonnet-4-5"
published_at: "2025-10-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:24:10.103156+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:3e2b0fc285c3cc54e8be47c5999ae8544da4214f29470a58061e472821801014"
---

# Building a Rust API with Claude Sonnet 4.5

Anthropic just dropped Sonnet 4.5 with a very **BOLD CLAIM** : it's the *"best coding model in the world."* That's a pretty confident statement in a crowded field of AI coding models.


Sonnet has proven itself to be very powerful and capable of handling complex tasks, but lately the community were claiming that Claude is slowly getting worse and it's not as good as it first was. This is especially after OpenAI released their leading coding model **Codex** model for their open source[Codex CLI](https://github.com/openai/codex) .


To bring back their users, Anthropic is finally back with a new model **Sonnet 4.5** and the Claude Code VSCode extension has also been updated, so you can use Sonnet 4.5 directly in VSCode or Cursor for a better experience.


Claude Code VSCode Extension


So let's see if Sonnet 4.5 is actually worth the hype and put it to the test, we as Rust developers like coding in Rust so this is what this blog post is about, we'll let Sonnet 4.5 build a Rust API for us that collects from 3 RSS feeds and serves them through an HTTP API.


## Building an RSS Aggregator API with Sonnet 4.5 #


The goal might seem simple at first, but it's not what we're interested in, building the API is easy, what matters is how well Sonnet 4.5 can handle the task and with how many prompts and iterations we can get the API to work exactly as we want.


Here is what we care about:


- **Up to date code** : Writing up to date code without out of date dependencies
- **Accuracy** : How well Sonnet 4.5 can understand the task and deliver the correct code
- **Speed** : How fast Sonnet 4.5 compared to Sonnet 4
- **MCP Capabilities** : How good is it in calling MCP tools
- **Error Prediction** : How well Sonnet 4.5 can predict and handle errors


Claude Code interface


Let's give it the first prompt:


> Build a Rust RSS aggregator API that fetches these 3 feeds:
>
>
> Hacker News:[https://news.ycombinator.com/rss](https://news.ycombinator.com/rss)
>
>
> Rust Blog:[https://blog.rust-lang.org/feed.xml](https://blog.rust-lang.org/feed.xml)
>
>
> XKCD:[https://xkcd.com/rss.xml](https://xkcd.com/rss.xml)
>
>
> Endpoints:
>
>
> GET /feeds - all items as JSON
>
>
> GET /feeds/{source} - filtered by source
>
>
> Use Axum


This is a very high level prompt, the only technical requirement is to use Axum and the endpoints should be GET /feeds and GET /feeds/{source}.


Initial implementation complete


## First Impressions #


Sonnet 4.5 got to work immediately, created the project and wrote the code for implementing all requirements.


A few things stood out right away:


**The Good:**


- It nailed the implementation. The code was straightforward, idiomatic Rust with proper async/await patterns
- Noticeably faster than Sonnet 4.0. The responses came back quicker, and it seemed more decisive in its approach
- It understood the task completely and delivered exactly what we asked for


**The Not-So-Good:**


- It used Axum 0.7 instead of the current 0.8, which has been out for almost 10 months.
- The RSS feeds we provided actually have different XML formats, Sonnet was supposed to normalize them into a unified JSON structure, but it didn't.


Looking at the code, the code compiles and runs, the API however isn't returning the feed for the Rust Blog due to lack of normalization.


## Iterating: Normalizing the Feeds #


I gave Sonnet a follow-up prompt to normalize the feeds:


> The structure of each feed is different, make sure you fetch them first and then normalize them into a unified JSON structure.


It performed exceptionally well, it first sent a GET request to each of the RSS feeds to fetch the data and understand their structure, then it normalized the data into a unified JSON structure, which is exactly what I wanted it to do.


Testing RSS feed with curl


Feed normalization complete


Testing the` /feeds` endpoint, everything was working as expected.


RSS feed output


Two prompts. That's all it took to go from initial implementation to a fully working, normalized RSS aggregator. That's really impressive, and the speed in which it did it is something I can't express enough.


## Keeping Dependencies Fresh #


There was still one thing bugging me: the outdated Axum version. Let's give another high level prompt and ask it to update the dependencies:


> Some of the crates you've used are outdated. Make sure they're all up to date and read their documentation to make sure there aren't any breaking changes.


Didn't specify which crates were outdated, let's let Sonnet figure it out itself.


There's something to note here, Axum 0.8 has a breaking change in the route syntax, the old 0.7 way was` /:id` for path parameters, but 0.8 requires` /\\{id\\}` instead. If Sonnet doesn't catch this breaking change, it will cause the application to panic at runtime.


Fetching documentation


Found outdated crates


As you might be aware by now, MCPs aren't very friendly when it comes to keeping the context clean, my first thought was "Too many MCP calls is going to blow up the context window." which is true but the important thing is that the model stays sharp and doesn't lose focus.


Updated dependencies


Axum 0.8 route update


It actually loooked up the documentation for Axum 0.8 and updated the route syntax correctly.` cargo build` works, and no runtime errors and both endpoints work perfectly.


Three prompts total with zero compilation errors so far is quite impressive.


#### Loved by developers


Join developers building with Shuttle


[Join them](https://console.shuttle.dev/)


Deployed my second service with Shuttle and I really like it! It's fast and integrates well with cargo, so I can focus on the Rust code instead of the deployment. Well done!


Matthias Endler


@matthiasendler


Rust Consultant @ Corrode


[Join them](https://console.shuttle.dev/)


## Deploying to Shuttle #


With a working API in hand, there was one more test: deployment. Claude Code has MCP (Model Context Protocol) integration with Shuttle, so I wanted to see if Sonnet could handle the entire deployment workflow.


I have the[Shuttle MCP](https://docs.shuttle.dev/integrations/mcp-server) installed in Claude, so it can interact with the Shuttle platform. To install it, you can run:


```text
claude mcp  add   Shuttle shuttle mcp start


```


For Cursor, you can add it by clicking the "Add to Cursor" button below.


Or updating your` mcp.json` file to include the following:


```text
{
"mcpServers"  :    {
"Shuttle"  :    {
"command"  :    "shuttle"  ,
"args"  :    [  "mcp"  ,    "start"  ]
}
}
}


```


You should also have the[Shuttle CLI installed](https://docs.shuttle.dev/getting-started/installation) and logged in to Shuttle, I recommend running` shuttle upgrade` if you already have it installed, you can do this by running:


```text
shuttle login


```


I gave it this prompt:


> I want you to use the Shuttle MCP to search the docs on how to convert the app to a Shuttle app and then deploy it to Shuttle.


It identified the required changes and updated the dependencies to include Shuttle runtime.


Adding Shuttle dependencies


Then converted the app to a Shuttle app.


Converting to Shuttle app


With the conversion complete, it proceeded to deploy the API.


Deploying to Shuttle


Shuttle deployment logs


Deployment success


There you go, the RSS aggregator was live and accessible. The days have changed, the entire process from prompt to production took three simple instructions and maybe five minutes of actual work.


Live API URL


## Results Summary #


Here's how Sonnet 4.5 performed on our key criteria:


Criteria Performance Notes


**Up to date code** ⚠️ Mixed Initially used Axum 0.7, but quickly updated to 0.8 when prompted and correctly handled breaking changes


**Accuracy** ✅ Excellent Nailed the implementation on first try, understood complex requirements like feed normalization and no compilation errors


**Speed** ✅ Excellent Noticeably faster than Sonnet 4.0, responses came back quicker and more decisively


**MCP Capabilities** ✅ Excellent Seamlessly used MCP tools to fetch RSS feeds, check documentation, and deploy to Shuttle


**Error Prediction** ⚠️ Good Caught Axum 0.8 breaking changes and updated route syntax correctly, but missed that RSS feeds have different structures


**Overall Score: 9/10** - Sonnet 4.5 delivered a production-ready API in just 3 prompts with zero compilation errors and successful deployment.


## Conclusion #


Sonnet 4.5 delivered on its promise. Three prompts, zero compilation errors, and a fully deployed Rust API. The speed improvements are noticeable, and the MCP integration makes deployment seamless. While it's not perfect (it did use outdated dependencies initially), it quickly corrected course when prompted.


What matters is that you always keep the context clean and write your` CLAUDE.md` files with caution, do not overfill the context window with junk and always keep updating it and iterating on it until you get the results you want.


The "best coding model" could be true. The combination of accuracy, speed, and tool integration makes it a compelling choice for Rust development.


#### Join the Shuttle Discord Community


Connect with other developers, learn, get help, and share your projects


[Join](https://discord.gg/shuttle)


Want to try building something similar? Get started quickly with:


```text
shuttle init  --from   shuttle-hq/shuttle-examples  --subfolder   axum/ai-assisted


```
