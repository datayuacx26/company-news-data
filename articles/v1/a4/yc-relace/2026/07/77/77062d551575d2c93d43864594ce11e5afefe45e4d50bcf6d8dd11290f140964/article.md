---
schema_version: "1.0.0"
document_id: "77062d551575d2c93d43864594ce11e5afefe45e4d50bcf6d8dd11290f140964"
company_key: "yc-relace"
company: "Relace"
source_id: "yc-relace-news-import-8821aa3570c8"
canonical_url: "https://relace.ai/blog/compact"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-07-22T11:23:51.276374+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:800b99fd980c6113faa084c120690ae456994041ce54bb715774c932c5b68e4c"
---

# Relace Compact: 50k TPS for 50% cost savings

As AI costs skyrocket, token leaderboards are being replaced with daily token quotas and companies are frantically trying to reduce spend.


By compacting agent traces after periods of user inactivity, we find you can save 50% on coding workloads. Using a compaction model that runs at 50k+ tok/s allows you to do this without harming the user experience.


###### **Figure 1:** The cost over time of a real Fable 5 trace from a feature our CTO worked on in Jacq. Compacting after periods of over 5 mins of inactivity (cache-miss), would have saved 55% on this example.


Relace has always focused on small, specialized models that lower costs and improve UX for coding agents. Launching our own AI coding product, Jacq, has been a goldmine of insights on new strategies.


Our first lesson was that output tokens don't dominate spend anymore. We broke down usage statistics and found cost for long-running conversations is driven by input tokens: early tokens are read from cache hundreds of times, and gaps in usage trigger expensive cache rewrites.


Compaction on cache miss offers major savings on both fronts. Compacting regularly reduces cache-read costs from quadratic to linear, and doing it on cache miss dampens the cost of rewriting a large cache.


The only problem is latency. Standard self-summary takes 1-2 minutes, which makes frequent compaction infeasible from a product perspective. That's why we trained Relace Compact to run at well over 50k tok/s — fast and accurate enough to be completely imperceptible to users.


Our model is already available in[Jacq](https://jacquard.dev/) , and it's accessible via[API](https://docs.relace.ai/docs/compact/quickstart) so that you can integrate it into your own agents.


### API Token Pricing Basics


The shift to agentic use cases has created a widespread inference pattern in which users send requests with an accumulating conversation prefix.


Since the attention mechanism in LLMs is causal, KV computation for a given token depends only on tokens that came before it. Instead of recomputing KV pairs for a conversation prefix after each new message, inference providers maintain a short-lived cache to reuse them.


Thus API pricing consists of three categories:


-


Cache writes on any new tokens sent by the user.


-


Cache reads on conversation prefixes the model has seen before.


-


Output tokens on new tokens the model generates.


For Claude Fable 5, the costs are $12.50/million cache-write tokens, $1/million cache-read tokens, and $50/million output tokens.


###### **Figure 2:** A visual breakdown of how messages in an agent trace are classified for billing purposes.


Note that output tokens must be written to cache after they are produced by the model, so for Fable 5 you effectively pay $62.50/million output tokens.


## Cache reads: long context blowup


Agents built on early models experienced degraded performance after 100k tokens, so users were forced to manicure context to retain performance. In those days, cost was dominated by output tokens (we developed[Fast Apply](https://relace.ai/blog/relace-apply-3) to drop cost for output code edits by 3-4x).


Today, agents remain productive well into their 1 million context windows, and users have the luxury of recklessly allowing context to grow.


###### **Figure 3:** As the conversation history grows, the cost of a request moves from output-dominated to the read-dominated regime.


If we integrate this linearly increasing input cost over the course of a coding session, we find that cache-read costs actually grow quadratically with session length.


As a toy example, consider a conversation with a series of tool/user inputs of length \\(t_{in}\\) followed by model responses of length \\(t_{out}\\). Let \\(p_{write}\\), \\(p_{read}\\), and \\(p_{out}\\) be the different API prices. Then, on the \\(i\\)th turn we incur a cost of


$$p_{read} (i-1) (t_{in} + t_{out}) + p_{write} t_{in} + p_{out} t_{out} .$$


Summing over an \\(n\\)-turn session, we get a total session cost of


$$\\begin{align*} C &= \\sum_{i=1}^{n} \\Bigl\[ p_{\\mathrm{read}}(i-1)(t_{\\mathrm{in}}+t_{\\mathrm{out}}) + p_{\\mathrm{write}}t_{\\mathrm{in}} + p_{\\mathrm{out}}t_{\\mathrm{out}} \\Bigr\] \\\\\[1em\] &= \\frac{n(n-1)}{2} p_{\\mathrm{read}}(t_{\\mathrm{in}}+t_{\\mathrm{out}}) + np_{\\mathrm{write}}t_{\\mathrm{in}} + np_{\\mathrm{out}}t_{\\mathrm{out}} \\\\\[1em\] &= \\Theta(n^{2})p_{\\mathrm{read}} + \\Theta(n)(p_{\\mathrm{write}}+p_{\\mathrm{out}}).\\end{align*}$$


So despite being almost two orders of magnitude cheaper than output tokens, cache-read tokens already dominate costs in this long context setting. Things get even worse when you consider cache rewrites, which happen a lot in practice.


## Cache (re)writes


Memory is expensive, so cached KV pairs are only stored on a provider’s server for a short period of time (TTL).


Once that window passes, any new message in the session triggers a cache write of the entire conversation prefix. For an 800k token conversation on Fable 5, a single cache miss costs $10!


###### **Figure 4:** When the TTL expires, the cached tokens go stale and need to all be rewritten on the next user message.


With engineers now multitasking on dozens of sessions in parallel, we notice it's becoming increasingly common for sessions to be left idle beyond the TTL in Jacq.


Continuing the example above, assume that every \\(k\\) turns we miss the TTL and the cache goes stale. The \\(j\\)th time this happens, we incur


$$p_{\\mathrm{write}}(jk-1)(t_{\\mathrm{in}}+t_{\\mathrm{out}})$$


Over the course of \\(n\\) total turns this adds up to a total extra cost of


$$\\begin{align*} C_{\\mathrm{extra}} = \\sum_{j=1}^{m} p_{\\mathrm{write}}(jk-1)(t_{\\mathrm{in}}+t_{\\mathrm{out}}) = \\Theta\\left(\\frac{n^{2}}{k}\\right)p_{\\mathrm{write}}. \\end{align*}$$


Asymptotically then, our cost goes as:


$$C_{\\mathrm{total}} = \\Theta(n^{2})p_{\\mathrm{read}} + \\Theta\\left(\\frac{n^{2}}{k}\\right)p_{\\mathrm{write}} + \\Theta(n)(p_{\\mathrm{write}}+p_{\\mathrm{out}}).$$


In other words, the total cost of both cache reads and cache writes grows quadratically with the number of turns, while output token costs remain roughly constant per turn and grow only linearly.


## Compact on Cache Miss


Since users don't spend time manicuring context anymore, their conversations often get bloated with stale information like repeated reads of the same file, irrelevant sections of bash outputs, etc. A natural strategy to break the quadratic input token costs is therefore to regularly compact the chat history throughout the coding session.


There are two obstacles to doing this well.


For one, editing previous messages in the conversation *ad hoc* incurs an expensive cache miss for all subsequent tokens. If you're not careful, this can easily outweigh cache read costs.


The best opportunity is therefore on **cache miss** . Once we’re past the TTL, we’ll have to rewrite the whole prefix anyway, and by compacting, we actually reduce the size of the expensive cache write.


###### **Figure 5:** Compacting on cache miss nearly halves the cost of a 50-turn trace. For best results, we recommend keeping the conversation as close to 128k tokens as possible.


The second obstacle is that deciding whether information is stale enough to prune can itself be slow and expensive. Compacting with a standard LLM takes 1-2 mins for long conversations, and if you do this right when a user sends a message, it's a horrible user experience. If you do it in the background, you may spend a lot on compacting conversations users never return to.


## Relace Compact


To solve this, we built an absurdly fast compaction model that can prune a long agent trace in seconds. We trigger it whenever a user sends a message beyond TTL, and it actually runs faster than the normal prefill time required for a cache write.


Relace Compact is a modified LLM designed for token-level classification, which means we can run it at 50k tok/s and an order of magnitude lower than cache read cost ($0.20/million input and $0.20/million output tokens). We'll share more details in a subsequent blog post.


To estimate expected savings, we broke down agent traces in Jacq and simulated the change in cost if we had compacted on cache miss (which happens on 1 out every 4 user messages). The histogram of the results is displayed below:


###### **Figure 6:** Replaying 15 days of real Jacq traces with compaction on cache-miss to 128k tokens yields an estimated aggregate savings of 56%.


Even a conservative compaction strategy down to 128k tokens saves over 56% on token costs, and after heavily testing the strategy in Jacq, we've confirmed it's imperceptible in both latency and quality.


This is a simple method for a huge win in cost savings! We're continuing to iterate to see how far we can push compaction until we begin to notice a difference in performance.


## Conclusion


Long-context agents flipped the cost structure of inference: input tokens, not output tokens, now dominate spend, and both cache reads and cache rewrites grow quadratically with session length. Compaction on cache miss attacks both terms at once — you compact exactly when you'd have to pay for a full rewrite anyway.


What made this impractical before was latency, and that's what Relace Compact solves. At 50k+ tok/s, compaction finishes faster than the prefill it replaces, making it invisible to users while cutting total spend by over 50%.


Relace Compact is live in[Jacq](https://jacquard.dev/) and available via[API](https://docs.relace.ai/docs/compact/quickstart) . There's a long tail of inference optimizations for agents, but the cheapest token is the one you never send. With compaction fast enough to be almost free, not wasting context is the obvious place to start.
