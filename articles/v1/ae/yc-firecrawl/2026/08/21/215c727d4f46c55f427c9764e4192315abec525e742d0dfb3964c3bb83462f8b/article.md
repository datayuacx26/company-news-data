---
schema_version: "1.0.0"
document_id: "215c727d4f46c55f427c9764e4192315abec525e742d0dfb3964c3bb83462f8b"
company_key: "yc-firecrawl"
company: "Firecrawl"
source_id: "yc-firecrawl-news-import-e3b10de50c72"
canonical_url: "https://www.firecrawl.dev/blog/use-other-models-in-claude-code"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-05T20:36:03.741488+00:00"
fetched_at: "2026-08-05T20:36:05.816521+00:00"
content_hash: "sha256:6bb2233b572278db4606969534516bc13651ccb58a1dd97b3aba087004497125"
---

# How to Use GPT, Gemini, and Grok Models Inside Claude Code with CLI Proxy

**TL;DR:**


- CLI Proxy is a local proxy server that lets you run GPT, Gemini, or Grok models inside Claude Code by pointing` ANTHROPIC_BASE_URL` and` ANTHROPIC_AUTH_TOKEN` at` localhost:8317`
- It logs in through each provider's OAuth flow, so requests draw from your existing subscription instead of pay-as-you-go API credits
- Each Claude Code tier (` ANTHROPIC_DEFAULT_OPUS_MODEL` ,` SONNET` ,` HAIKU` ) can map to a different model from a different provider
- Skills, subagents, reasoning efforts, and plan mode all keep working because translation happens at the API layer
- The proxy stores multiple OAuth tokens per provider and rotates between them (default: round-robin) to spread usage across accounts
- Exposing Claude models to other CLIs technically violates Anthropic's ToS; using non-Claude models inside Claude Code (the direction covered here) doesn't


---


Claude Code is my pick for the best AI coding tool right now, and it expects you to use Claude models, which cost more than the competition and come with less generous subscription limits. There's a way around that:[CLI Proxy](https://help.router-for.me/) , a local proxy server that lets you run **GPT** , **Gemini** , or **Grok** models inside Claude Code, on your existing subscriptions.


It also switches between multiple accounts for the same provider automatically, so you get more out of your usage limits. Here's how the whole thing works.


## Claude Code running a GPT model


This is Claude Code, except the model in the status line is` gpt-5.6-terra` instead of a Claude model. Ask it *don't use any skills, what ai model is this and who was it built by* and it answers **GPT-5.6 Terra, built by OpenAI** .


You can swap between models, Terra, Luna, and Sol here, and change the **reasoning effort** level, without leaving Claude Code. The` API Usage Billing` notice is a red herring: the requests come out of your provider subscription rather than pay-as-you-go API credits, for reasons that make sense once you seehow the authentication works .


Everything else about Claude Code behaves normally. Giving it a real task, *search for information about Opus 5 and present it to me in an Anthropic style HTML page* , it loads a **skill** , thinks with medium effort, and runs the task with full **subagent** support.


Part of that task was gathering current information about a model that didn't exist when the GPT model was trained, which it did through the new and improved[Firecrawl search API](https://www.firecrawl.dev/search) . **Search** is one of the endpoints that works[keyless](https://docs.firecrawl.dev/rate-limits) , so there's nothing to sign up for to try it, though[a free API key](https://www.firecrawl.dev/) raises the rate limits and adds credits.


Search is what this run needed;[/scrape](https://www.firecrawl.dev/scrape) returns clean markdown from a single URL, and[/interact](https://www.firecrawl.dev/interact) handles pages that need clicks or logins before you can extract.


The result was a full status page, built by an OpenAI model, researched with **Firecrawl** , orchestrated entirely by Claude Code. You can[see the page it generated](https://opus5-demo-page.pages.dev/) , complete with a lineup map, an evidence ledger, and a timeline. It's demo output rather than a reference, so none of it has been fact-checked.


That whole run cost 11% of the weekly subscription limit. Claude Code reported a much higher context figure than that, which is a small display bug in the setup rather than real usage.


## Installing CLI Proxy


On a Mac,[CLI Proxy](https://github.com/router-for-me/CLIProxyAPI) installs through Homebrew:


```text
brew   install   cliproxyapi
brew   services   start   cliproxyapi
```


Linux has a one-click installer script and an AUR package, Windows has a release binary plus a desktop GUI, and there's a **Docker** image if you'd rather run this on a separate VPS. All four paths are in the[quick start docs](https://help.router-for.me/introduction/quick-start.html) .


## Editing the CLI Proxy config


Where the config file lives depends on how you installed it. With Homebrew on Apple Silicon it's at` /opt/homebrew/etc/cliproxyapi.conf` . Two things in there need changing.


```text
# /opt/homebrew/etc/cliproxyapi.conf


port  :   8317


remote-management  :
# Password for the web UI. Put a plaintext value here and it gets
# hashed on startup.
secret-key  :   "something-you-will-remember"


# Authentication directory (supports ~ for home directory)
auth-dir  :   "~/.cli-proxy-api"


# API keys for authentication
api-keys  :
-   "sk-your-api-key-1"
```


The **secret-key** is the password you'll use to log into CLI Proxy's web view. It can be anything, and it gets hashed inside the config the first time you log in, so it won't sit there in plaintext.


The **api-keys** list is the more interesting one. This is the token that gets bundled with every request Claude Code sends to the proxy. Claude Code expects a token to exist, so you give it one of these, and CLI Proxy checks the incoming token against this list before doing anything real. It's what stops any other app on your machine from hitting the proxy port and burning through your usage.


You can generate a proper random value for it rather than making one up:


```text
openssl   rand   -hex   32
```


Once the config is saved, restart the service to pick it up:


```text
brew   services   restart   cliproxyapi
```


## Logging in a provider


Now go to` http://localhost:8317` and log in with the password you set as your secret key. That opens the **CLI Proxy API Management Center** , and from the dashboard, **OAuth Login** lists every provider you can authenticate with: Codex, Anthropic, Antigravity, and Kimi.


Starting a Codex login opens the normal OAuth flow: pick your account, approve it, done. The page itself doesn't change afterwards, so to confirm it worked, head to **Auth Files** , where the credential shows up with its provider, size, and health.


This is also where the **multi-account** part starts to matter. Nothing stops you logging in several times with different accounts for the same provider, and each one lands here as its own credential.


## Pointing Claude Code at the proxy


The last step is configuring your client. Claude Code reads its endpoint and model choices from environment variables, so a shell alias is enough:


```text
# config.fish


alias   ccx  =  'ANTHROPIC_BASE_URL=http://localhost:8317 \
ANTHROPIC_AUTH_TOKEN=sk-your-api-key-1 \
ANTHROPIC_DEFAULT_OPUS_MODEL=gpt-5.6-terra \
ANTHROPIC_DEFAULT_SONNET_MODEL=gpt-5.6-terra \
ANTHROPIC_DEFAULT_HAIKU_MODEL=gpt-5.6-luna \
ANTHROPIC_DEFAULT_FABLE_MODEL=gpt-5.6-sol \
CLAUDE_CODE_SUBAGENT_MODEL=gpt-5.6-terra \
claude'
```


Here's what each variable does:


- ` ANTHROPIC_BASE_URL` points Claude Code at the local proxy instead of Anthropic's servers.
- ` ANTHROPIC_AUTH_TOKEN` is one of the values from the` api-keys` list you set earlier.
- ` ANTHROPIC_DEFAULT_OPUS_MODEL` ,` ANTHROPIC_DEFAULT_SONNET_MODEL` ,` ANTHROPIC_DEFAULT_HAIKU_MODEL` , and` ANTHROPIC_DEFAULT_FABLE_MODEL` each map one Claude Code model tier onto the model you actually want running there.
- ` CLAUDE_CODE_SUBAGENT_MODEL` covers what subagents get.


They're all separate variables, so nothing stops you pointing each tier at a different provider.


You can see the result in Claude Code's own model picker, which still thinks it's offering you Claude tiers and lists a GPT model against each one:


If you're not sure what model names to put in those variables, ask the proxy. It exposes an OpenAI-compatible models endpoint, and piping it through[jq](https://jqlang.github.io/jq/) makes it readable:


```text
curl   -s   http://localhost:8317/v1/models   \
-H   "Authorization: Bearer sk-your-api-key-1"   |   jq
```


Reload your shell config, run the alias, and Claude Code starts up on a GPT model. Claude Code isn't the only option either: CLI Proxy has client configuration docs for[Codex](https://help.router-for.me/agent-client/codex.html) ,[Factory Droid](https://help.router-for.me/agent-client/droid.html) , and[OpenCode](https://help.router-for.me/agent-client/opencode.html) too.


If you have several clients pointed at the proxy, give each one a different API key from the list. That way the proxy's logs and quota tracking can tell them apart.


One thing worth flagging: this works in the other direction as well. You could sign in with **Anthropic OAuth** and use your Claude subscription inside Codex. That technically goes against Claude's terms of service though, so it's not something to recommend.


## How CLI Proxy translates between APIs


At a high level, CLI Proxy is a proxy server that lets a client like Claude Code read the **API format** of a different model.


Normally, a prompt in Claude Code goes to Anthropic's servers and comes back in Anthropic's API format, which Claude Code knows how to read. Send a prompt from Codex CLI and it goes to OpenAI's servers and comes back in OpenAI's format, which Claude Code *can't* read. Same request, incompatible envelope.


CLI Proxy sits between the client and the model provider's servers and translates in both directions: your request becomes something the provider understands, and the provider's response becomes something the client understands.


Because the translation happens at the API layer, everything built on top of the API survives the trip. That's why **reasoning efforts** , **subagents** , and Claude Code workflows all still work when the model on the other end is GPT.


## How CLI Proxy handles authentication


The more interesting half of this is how CLI Proxy handles auth.


When you log into a model provider's website to use your subscription, you get back a token that only that provider accepts. Tokens aren't interchangeable, which is normally the end of the conversation.


When you log in *through* CLI Proxy, it grabs that token and saves it in its own auth directory. So a request from Claude Code carries the **dummy token** from CLI Proxy's config, because Claude Code insists on a token existing. CLI Proxy checks that token against its list, and if it matches, makes the real request using the real provider token it's holding.


This is why the multi-account thing works. Since the proxy owns the real credentials, it can hold several accounts for the same provider and pick whichever token it likes per request. The default strategy is` round-robin` , which spreads load across accounts to get the most out of your rate limits. There's also` fill-first` if you'd rather exhaust one account before moving to the next, plus optional[session affinity](https://help.router-for.me/configuration/options.html) to bind a conversation to one credential.


## Why not just use GLM or Kimi directly


This isn't an entirely new technique. **Kimi** and **GLM** already let you use their models inside Claude Code, and[cc-mirror](https://github.com/numman-ali/cc-mirror) lets you create isolated versions of Claude Code with custom providers.


The catch with those is that you're locked into one provider. You can't make your Fable model GPT, your Opus model GLM, and your Sonnet model Kimi. You use that provider's models for everything, or you use something else.


And if you wanted to run this through Codex instead of Claude Code, some of those integrations won't work at all, because they don't support the API Codex speaks. Which is why Theo and even[Tibo from OpenAI](https://x.com/thsottiaux/status/2076119366647894371) recommend CLI Proxy for running GPT models inside Claude Code.


One local proxy, one config file, and the model behind each Claude Code tier becomes something you choose rather than something you inherit.
