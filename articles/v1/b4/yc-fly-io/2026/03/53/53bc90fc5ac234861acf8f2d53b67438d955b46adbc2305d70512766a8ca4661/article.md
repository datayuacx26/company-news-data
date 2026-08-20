---
schema_version: "1.0.0"
document_id: "53bc90fc5ac234861acf8f2d53b67438d955b46adbc2305d70512766a8ca4661"
company_key: "yc-fly-io"
company: "Fly.io"
source_id: "yc-fly-io-news-import-54d37e81cc45"
canonical_url: "https://fly.io/blog/unfortunately-mcp/"
published_at: "2026-03-10T00:00:00+00:00"
first_seen_at: "2026-07-21T20:38:55.214316+00:00"
fetched_at: "2026-07-28T21:26:25.193690+00:00"
content_hash: "sha256:e69e01132e47738cbb69b09c4ea414611f9dcd55f5b6e94dd9e37abf268911ab"
---

# Unfortunately, Sprites Now Speak MCP

Author Name Kurt Mackey @mrkurt[@mrkurt](https://twitter.com/mrkurt) Image by


[Annie Ruygt](https://annieruygtillustration.com/)


Sprites are disposable cloud computers. They appear instantly, always include durable filesystems, and cost practically nothing when idle. They’re the best and safest place on the Internet to run agents and we want you to[create dozens of them](https://sprites.dev/) .


Sprites are a place to run agents; the first thing you should think to do with a new Sprite is to type` claude` (or` gemini` or` codex` ). We’ve put a[lot of effort](https://fly.io/blog/design-and-implementation/) into making sure coding agents feel safe and happy when they’re on Sprites, because, to (probably) quote John von Neumann, “happy agents are productive agents.”


What’s less obvious about Sprites is that they’re great tools *for* agents. Want three different versions of a new feature? A test environment? An ensemble of cooperating services? It’s super handy to be able to start your prompts, “` On a new Sprite, do…` ”.


The Sprites API is simple, discoverable, and designed for this use case. It’s just a question of how you choose to give your agent access to it. And now there’s one more way: with MCP.


## We Did This Because Your Agents Suck


This feature works well, but we’re less than enthusiastic about it. Not as product developers, mind you. It’s a good product! Just as aesthetes.


In 2026, MCP is the wrong way to extend the capabilities of an agent. The emerging Right Way to do this is command line tools and discoverable APIs.


When we plug an MCP into your agent, we’re filling its context with tool descriptions, many of which you’ll probably never use. Really, all your agent should need is a short sentence, like “` Use this skill whenever users want to create a new VM to run a task on, or to manage the VMs already available.` ” The skill should take care of the rest.


CLI-driven agent skills are efficient because they reveal capabilities progressively. You can do CLI subcommands, like` sprite checkpoint` and` sprite exec` , or with API endpoints and subpaths. Good agent harnesses are uncanny at quickly working out how to use these things.


You *are* using Playwright, right? “Make sure this web application actually works before you tell me you’re done”?


Take[Playwright, the industry-standard browser automation tool](https://playwright.dev/) . Ask` claude` to install Playwright and Chrome and there’s a coinflip chance it sets up the MCP server. But notice that when the coin comes up tails, Playwright still works.` claude` just drives it by writing little scripts. This is good! The models already know how to write little scripts without using up context.


And there’s more at stake than just efficiency. Cramming your context full of MCP tool descriptions is a way of signaling to the model that those tools are important to you. But not every Sprite command is equally important in every setting. If you’re not using network policies, you don’t need` gemini` to waste a bunch of time setting them up for you.


Skills and APIs are the best way to drive Sprites. But to make that work, you need an agent that can run shell commands for itself. So you’ll want to reach for MCP sessions when you’re stuck with an agent that can’t run commands. Thankfully, most of us aren’t using those kinds of agents anymore. In` claude` ,` gemini` , or` codex` , you should just show your agent the` sprite` CLI and let it impress you.


## sprites.dev/mcp


Plug this URL into Claude Desktop, or any other agent tool that speaks MCP. You’ll authenticate to one of your Fly.io organizations, and your agent will speak Sprites.


Then:


` On a new Sprite, take this repository and reproduce this bug from issues/913, capturing logs.`


` On a new Sprite, benchmark this function across 1000 runs and summarize the results.`


` On a new Sprite, update all the dependencies on this project to their newest versions and test that everything works.`


` On 3 new Sprites, change this service to use each of these 3 query libraries, and use HTTP to test latency.`


` On a new Sprite, run this code with bpfwatch and show me what files it touches.`


` On a new Sprite, run a load generator against this endpoint for 60 seconds and report the results.`


` On a new Sprite, download this dataset and give me a Jupyter notebook to explore it in.`


` On a new Sprite, set up a webhook receiver and render a real-time web report of all the payloads it receives.`


I don’t know. You know your projects better than we do. Whatever. Sometimes you want a clean, cheap, disposable computer (or five of them). That’s now an available feature of all your prompts. Find ways to apply it to your project, and we think you’ll end up wondering where Sprites have been all your life.


Some of you are thinking to yourself: “this feature is going to result in robots ruining my life”. We agree. So we’ve built in guardrails. When you authenticate, giving your agent access to a single specific organization on your Fly.io account, we’ll let you scope down the MCP session. You can cap the number of Sprites our MCP will create for you, and you can give them name prefixes so you can easily spot the robots and disassemble them.


## Fuck Stateless Sandboxes


We’ll keep saying this until our faces turn blue: the industry is stuck on “sandboxes” as a way of letting agents run code, and sandboxes aren’t good enough anymore. What agents want is real computers, with real filesystems, connected to real networks, and there’s no technical reason not to give them some.


[We designed Sprites so that you can fearlessly create whole bunches of them](https://fly.io/blog/code-and-let-live/) . They’re responsive enough to host web apps for your team, but they idle in a sleeping state where they cost virtually nothing. Everybody at Fly.io that uses them ends up with 20 or 30, just hanging around.


We think you’ll do better work when you can pull in as many computers as you need to solve problems. If it takes an MCP server for us to get you to do that, so be it.


Next post ↑[Building Agents that Don't Break Themselves](https://fly.io/blog/building-agents-that-dont-break-themselves/) Previous post ↓[Litestream Writable VFS](https://fly.io/blog/litestream-writable-vfs/)
