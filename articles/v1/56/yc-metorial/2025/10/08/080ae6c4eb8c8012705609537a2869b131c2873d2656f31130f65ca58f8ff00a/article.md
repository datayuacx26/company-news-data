---
schema_version: "1.0.0"
document_id: "080ae6c4eb8c8012705609537a2869b131c2873d2656f31130f65ca58f8ff00a"
company_key: "yc-metorial"
company: "Metorial"
source_id: "yc-metorial-news-import-9c457a3d81d4"
canonical_url: "https://metorial.com/blog/jquery-age-of-ai"
published_at: "2025-10-26T17:17:00+00:00"
first_seen_at: "2026-07-22T04:11:44.298499+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:4f8d75a71cd8ed198fa1be1815cb9862060d81cdbd8f56a68b0e02d3709aa446"
---

# The jQuery Age of AI Agents

I have this theory: *that we are in the jQuery age of AI agents.*


If you're building with AI right now, connecting agents to tools, wiring up APIs, trying to get Claude to talk to your database, you're living through something weirdly familiar (I think). It's the same chaotic, fragmented, "why is this so hard" energy that web developers felt in the mid-2000s when every browser implemented JavaScript differently and cross-browser compatibility was a prayer, not a promise.


And just like back then, we're reaching for duct-tape solutions. We’re building pragmatic ad-hoc abstractions. Tools that don't fix the underlying mess but make it bearable enough that we can actually ship something. Why would we fix it, OpenAI is announcing a new set of APIs next week anyway.


The parallel is almost too perfect. And if history is any guide, we're about to see some dramatic changes in how AI agents work. And I’m not sure if that’s MCP or another thing.


Let me explain.


## Remember When the Web Was Chaos? (No? Me Neither.)


Here's the thing: most people reading this, myself included, weren't even alive when the browser wars started. The first browser war kicked off in 1995. If you were born around the turn of the millennium, you missed the absolute carnage of trying to build websites that worked across Netscape Navigator, Internet Explorer, and whatever else people were using.


But the stories are legendary. I imagine an intense, almost mythical sword fight between Bill Gates and Mark Andreessen … anyway.


Imagine writing the same piece of code three completely different ways because each browser decided to implement JavaScript differently. Imagine seeing "Best viewed in Internet Explorer 6" badges on websites. Not as a joke, but as a genuine disclaimer because the site literally wouldn't work in Firefox.


The keen-eyed might already see some parallels between browser and AI companies.


Microsoft and Netscape were locked in what historians now call the "browser wars," each adding proprietary features (Netscape had the` <blink>` tag, IE had` <marquee>.` Both of them were great additions to the web and we need them back) and completely ignoring any attempts at standardization. The W3C published HTML standards, but no one cared. Why would they, there’s no time for standard while you’re at war.


### Then jQuery Showed Up


In 2006, the great John Resig released jQuery at BarCamp NYC. It wasn't magic. It didn't fix the fundamental problem that browsers couldn't agree on how JavaScript should work.


What it did was brilliant in its simplicity: it abstracted away the pain. John Resig went through the painful depths of cross-browser inconsistencies so you didn’t have to.


Instead of writing different code for IE vs. Firefox vs. Safari, you wrote jQuery. One line of jQuery worked everywhere.` $('#element').hide()` just worked, regardless of which browser your user was running. By 2019, jQuery was running on 80% of the top 1 million websites. It became so dominant that developers would learn jQuery *before* learning vanilla JavaScript.


But here's what jQuery actually was: a really, really good version of dukt tape. Kinda like Flex Tape.


It didn't solve the standards problem. It didn't make browsers agree on anything. It just provided a layer that made the web less bad. And you know what? That was enough for most people.


jQuery enabled an entire generation of web applications to exist while the slower work of actual standardization happened in the background. It even inspired some web APIs that we’re used to now.` document.querySelector(’#element’)` anyone?


## Déjà Vu: The AI Agent Mess of 2025


Now let's talk about building AI agents in 2025. (BTW it’s October already, crazy, right? It’s about to be Christmas time.)


You pick a framework, maybe LangChain because it's popular, or Vercel’s AI SDK because it’s easy. You get your agent running. Great!


Then you want to connect it to Google Drive. And Slack. And your database. And suddenly you're writing custom integration code for every single service. Each one needs its own authentication logic, its own error handling, its own data transformation layer.


This is what researchers call the "N×M integration problem." If you have M different AI applications and N different tools, you need M×N different integrations. Every combination requires custom code.


Sound familiar?


## Enter MCP: jQuery for Agents


This is where the Model Context Protocol comes in.


Released by Anthropic in November 2024, MCP does for AI agent integration what jQuery did for browser compatibility: it provides a standardized abstraction layer. Instead of writing custom code for every agent-tool combination, you write one once in the MCP standard.


The architecture is straightforward: tools expose themselves as MCP servers, and agents connect as MCP clients. For the nerds: it transforms the N×M problem into M+N. Build one MCP server for Google Drive, and every MCP-compatible agent can use it. Build one MCP client in your agent, and it can connect to every MCP server.


Everyone know this is a real problem. That’s why major players like OpenAI and Google adopted it. Anthropic released pre-built servers for Google Drive, Slack, GitHub, Postgres (they aren’t good but they’re there).


### Why It Feels Like jQuery


MCP has all the hallmarks of jQuery's moment:


- **It's pragmatic, not perfect** . MCP doesn't solve all the fundamental problems with agent orchestration. It doesn't magically make security easy or resolve all the complexities of long-running workflows. What it does is make the most painful part significantly less painful.
- **It solves today's problem** . Developers don't need a five-year vision of perfect AI infrastructure. They need to ship an agent that can read emails, or connect to Salesforce, or summarize support tickets *this week* . MCP delivers that.
- **It's getting you shipping** . Just like jQuery let developers build rich web apps while browser vendors sorted out their standards mess, MCP lets you build capable agents while the industry figures out what agent standards should actually look like.


And here's the key insight: that's not a bug, it's a feature. jQuery wasn't a failure because it was temporary. It was a massive success because it enabled a generation of innovation while harder problems got solved. And even though jQuery is lost, its spirit isn’t.


## But History Doesn't Just Repeat...


There are some important differences this time around that make the AI agent standardization story potentially faster, but also messier, than the web's journey.


### Speed: Months, Not Years


The web took nearly two decades to go from fragmentation to reasonable standardization. HTML5 didn't become a formal recommendation until 2014. 19 years after the first browser war started.


AI agents? We went from the first serious agent frameworks to standardization attempts in *months* . Chat GPT launched in late 2022. MCP was announced in late 2024. That's not even two years.


This compressed timeline means either we'll see faster convergence... or more catastrophic fragmentation. Either way, we will get a solution faster.


### Real Stakes: Agents Can Do Actual Damage


When Internet Explorer crashed, you reloaded the page. Annoying, but not catastrophic.


When an AI agent with tool-calling capabilities misbehaves, it can execute financial transactions, drop production databases, send emails to your entire customer list, or control physical systems.


This isn't theoretical. The security model needs to be right from the start, not retrofitted later.


### We’re Still at War


OpenAI, Anthropic, and Google, and whoever might come next haven’t given up on that. Anthropic built a good prototype. It works good enough. It’s being used by people.


But this is war. MCP is a ceasefire at best.


MCP isn't the only player. Google's working on Agent2Agent. There's the Agent Communication Protocol. The Open Agent Schema Framework claims to reduce integration costs by 40-60% compared to custom implementations.


MCP has early momentum, but nothing is guaranteed. The web eventually converged on standards through a combination of market forces, developer pressure, and competitive cooperation between browser vendors. But also because of there sheer fact that eventually companies realized that web browsers are just carriers of information, not actual businesses.


## Let’s Embrace the Chaos


This isn’t a love letter to MCP (of to jQuery for that matter). MCP is dukt tape for AI agents. It’s not permanent. Every software engineer knows that sometimes dukt tape fixes hold up longer than expected, but never forever. The problem remains.


- **We need to connect AI agents to tools, data sources** , and whatever the new thing your PM just found and you absolutely need for your product is.
- **We need standardized ways to write those integrations.** You don’t want to be the 10.000th person writing a Google Calendar integration.
- **We need standardized ways to connect to those integrations.** Just like we have REST, GRPC, and GraphQL.
- **We need observability, and logging.** Just how we have Sentry, PagerDuty, Datadog and stdout.
- **We need security.** Because legal says so.
