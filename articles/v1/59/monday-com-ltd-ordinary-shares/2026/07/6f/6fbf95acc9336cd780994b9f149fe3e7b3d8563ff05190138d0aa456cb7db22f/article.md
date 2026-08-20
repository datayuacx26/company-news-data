---
schema_version: "1.0.0"
document_id: "6fbf95acc9336cd780994b9f149fe3e7b3d8563ff05190138d0aa456cb7db22f"
company_key: "monday-com-ltd-ordinary-shares"
company: "monday.com Ltd."
source_id: "monday-com-ltd-ordinary-shares-rss-78444ad48313"
canonical_url: "https://engineering.monday.com/getting-more-out-of-every-ai-token/"
published_at: "2026-07-16T10:46:48+00:00"
first_seen_at: "2026-07-20T23:22:14.841151+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:52c62fbf07984db251a64c2119629311fc50fcf12a72a544c877aed5e968e97e"
---

# Getting More Out of Every AI Token

As a software engineer, together with my colleagues at monday.com and also other engineers in the industry, I bore witness to some amazing changes in the world around us and our craft. First, the industry was bewitched by the possibilities that AI-driven development offered. We started to use AI a lot, learning as we go how to actually do this effectively. And we spent a lot. According to a


[Research and Markets report](https://www.researchandmarkets.com/reports/6225896/ai-code-tools-market-report) , the AI code tools market was worth $9.46 billion in 2026. If that number is a little too abstract, it could buy us around 1.5 billion Big Macs in California.


When the bill came, executives around the globe, Monday included, realized that while AI is mighty, it’s also very much not free. Suddenly, the honeymoon period was over, and we needed to quickly learn how to not only be productive but also cost-effective. But it was never about money alone – nobody wants to work while worrying about hitting a spending limit. Fortunately, there are plenty of strategies we can adopt to improve cost efficiency and put these worries to bed.


In this article, we will focus on the boundaries between the model and the user and between the model and the tools. In other words, we will teach our agents to use fewer words and mean more.


## Short anatomy of LLM costs


LLMs are very talkative by nature. They love the fluff and flattery, and they write each response about a technical problem like they would write a recipe for a cake on a cooking blog. While reading about what a great and observant engineer I am is nice, it also doesn’t really contribute to solving my problem, unless my problem is imposter syndrome. Also, it takes time to read, and it uses expensive output tokens to produce it. Anthropic’s Opus 4.8, one of the most capable models at the time of writing, costs $5 per million input tokens and five times more – $25 per million output tokens (nearly four Big Macs, if we are not on keto).


## Cut the chatter: trim model replies with Caveman


That’s why the Caveman plugin was created, to instruct our agents to be more terse and cut out the chit-chat. We could be asking:


` My Express auth middleware is letting expired JWT tokens through. The expiry check uses Date.now() compared to the token's exp field. What's wrong, and how do I fix it?`


And a non-caveman agent would start:


` Sure! I'd be happy to help you with that. The issue you're experiencing is most likely caused by your authentication middleware not properly validating the token expiry. Let me take a look and suggest a fix \[...\]`


That’s 40 tokens already, and we haven’t even started on the real answer. I don’t want to hear about pleasantries and words of encouragement. I want a caveman:


` Bug: \`exp\` is seconds, \`Date.now()\` is milliseconds. Comparing raw = expired tokens pass. Fix: compare \`Date.now() / 1000 > exp\` (or \`exp * 1000\`).`


On average, according to its own benchmarks, Caveman cuts 65% of output tokens. Mind that it’s not only about the financial cost of a token, but also about the human cost of reading the output. This output will also stay in the context and will be sent on each conversation turn. It will be cached, and cache reads cost a tenth of normal input tokens, but with big contexts even that can compound quickly. If you have ever tried to actually use the 1-million-token context Opus offers, you know how quickly the numbers can climb.


## RTK: Compress what your commands feed the model


Agents do much more than just speak to us. They also run tools on our machines: searching through files, running tests, checking what’s installed, poking at version control, and much more. All of these tools were built for humans, long before anyone cared about tokens, so they tend to answer with long, decorated output. Consequently, they can flood our context, cost us a lot of money, and diminish the general effectiveness of the model.


That’s why the RTK proxy was created. This is what the normal \`docker ps\` command produces. Notice the nicely rendered table that makes reading the output very easy for humans:


` % docker ps
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
56e40e265f7b busybox "sleep infinity" 3 seconds ago Up 3 seconds stay-alive`


And this is the same command with RTK:


` % rtk docker ps
\[docker\] 1 containers:
56e40e265f7b stay-alive (busybox)`


It is highly opinionated and can sometimes cut out information that would be useful. But thanks to those same heuristics, it saves, on average, 60% to 90% of output tokens on supported commands. Mind that this won’t translate one-to-one into AI spend: RTK tends to compact output that would usually be cached cheaply anyway. In practice, that double-digit token compression may only buy us a single-digit cost cut, but it’s still a worthy endeavor.


## Compress everything the agent reads and leave some Headroom


Command output isn’t the only place in which we can bleed tokens. Reading code, consuming JSON, calling MCPs, sending requests, the list goes on and on. All of these can be optimized, and all of these can explode our costs if not watched.


And that’s where Headroom comes in. It’s an impressively complete context optimization layer that compresses mostly everything an AI agent reads: tool outputs, logs, files, RAG chunks and even conversation history, all before it ever reaches the model. By its own numbers, it cuts 60-95% of tokens while returning the same answers.


Where Caveman and RTK each target one boundary, Headroom tries to cover them all. The clever part is that it doesn’t compress everything the same way. It first works out what kind of content it’s looking at (code, logs, data, plain text) and then trims each in a way that fits. Code keeps the parts that describe what it does and drops the bulk of the bodies. Logs keep the failures and drop the passing noise. Data keeps the errors and the odd values and throws away the repetitive rest. On top of that, it’s careful not to disturb the parts of the conversation the model has already seen, so those stay cheap to reuse.


The other clever part is that, unlike RTK, its compression is reversible. Instead of throwing the trimmed detail away, Headroom stashes the original locally (they call it CCR, Compress-Cache-Retrieve) and hands the model a \`headroom_retrieve\` tool. So when the agent actually needs the full thing, it just asks for it. Aggressive savings, without the risk of the model going blind on the one line that mattered.


Mind that it can use RTK, and at the time of writing this article, it ships with it and uses it by default for shell-output rewriting. It can also employ some terseness steering, so it can also behave a little bit like Caveman.


## How to combine them and when to stop


The good news is that these three aren’t rivals; they stack. Caveman trims what the model writes back; RTK trims what our shell hands in; and Headroom sits over the whole conversation and compresses everything else, including RTK output. A sensible starting point: let Headroom wrap our agent, keep RTK underneath it for command output, and layer Caveman on top when we want the model itself to shut up and get to the point. Each one guards a different boundary, so the savings add rather than cancel out.


But compression isn’t free, and more of it isn’t always better. Every heuristic that drops a token is a small bet that the model didn’t need it. Most of the time, that bet pays off; occasionally, it hides the one log line or JSON field that mattered, and the agent guesses instead of knowing. This is exactly why Headroom’s reversible CCR is nice, but even that costs a round-trip when the model has to fetch the original back. So we should measure before we celebrate. Watch the token graphs, but also watch whether answers get worse. If our agent starts asking for things it used to just know, we’ve compressed past the sweet spot. The goal was never zero tokens; it was spending them only on buying us something.


## Spend Tokens Only Where They Count


Agents often work in an environment that was never designed for them. It was designed for humans, in times when text wasn’t measured in tokens, and nobody had to think twice about how much a paragraph costs. They can manage, sure, but it will take time and money, and both add up faster than we’d like. At monday.com, we strive to build environments that feel natural not only to humans, but increasingly to agents too. Whether it’s teaching them to talk like a caveman, compressing their command output with RTK, or handing everything through Headroom, the underlying idea is the same: we can, and should, make their lives easier by feeding them only the content they actually need. Our agents get faster and sharper, and our context stays clean. Last but not least, we might finally stop worrying about hitting the monthly AI spend limit and truly focus on our work.
