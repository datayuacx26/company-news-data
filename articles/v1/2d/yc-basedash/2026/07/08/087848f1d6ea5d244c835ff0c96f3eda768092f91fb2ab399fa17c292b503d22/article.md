---
schema_version: "1.0.0"
document_id: "087848f1d6ea5d244c835ff0c96f3eda768092f91fb2ab399fa17c292b503d22"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-a-timestamp-was-busting-our-prompt-cache/"
published_at: "2026-07-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T20:47:27.048275+00:00"
content_hash: "sha256:4d0a8411a73599ff059cee38ed7fe1c35987824c944d0cc53df70d8a4de242db"
---

# How a single timestamp broke our prompt cache

Every message you send to a Basedash AI agent re-sends a lot of text the model has already seen. The system instructions, the tool definitions, your database schema, the running chat history: all of it goes back over the wire on every turn, because that’s how stateless chat completions work.


Prompt caching is supposed to make that cheap. The providers cache the stable prefix of your request and charge a fraction of the normal input price to re-read it. Anthropic cache reads run about 10% of the base input rate, and OpenAI discounts cached input by around half automatically.


We were paying full price on almost every turn. Here’s the line that was doing it.


## What a turn actually sends


Our chat agent builds each request in the same order every time:


1. Tool definitions
2. System instructions (rules, capabilities, and the database schema)
3. Chat history (every prior user and assistant message)
4. A per-turn context message (things that change, like the current selection)
5. The new user message


Parts 1 through 3 are the big ones. On a chat with a wide schema and a dozen turns, they can be tens of thousands of tokens. They’re also stable: the tools don’t change mid-chat, and the history only grows at the end. That’s exactly the shape prompt caching wants.


## Prompt caching is an exact-prefix match


The thing to understand about prompt caching is that it matches on an exact prefix. The provider hashes your request from the very first token forward and reuses cached work up to the first byte that differs. One changed character near the front invalidates everything after it.


So the order of your request matters a lot. Stable content belongs at the front, and anything that changes per turn belongs at the back. Put one volatile token early and you’ve thrown away the cache for the entire request.


Guess where our system prompt ended.


## The one line


```text
The current date and time is ${new Date().toLocaleString()}.
```


That was the last line of the system instructions, so the model knew what “today” and “this week” meant. It’s a reasonable thing to want. It’s also a value that changes every second, sitting inside a block that’s otherwise identical across turns.


Because the timestamp lived in the system prompt, the` \[tools, system\]` prefix was different on every single request. The cache never matched, so we wrote a fresh cache entry each turn and never once read from one. We were paying the cache-write premium and getting none of the read discount, which is worse than not caching at all.


The fix was to delete the line and move the date into the per-turn context message, which sits after the stable prefix anyway:


```text
<current_date_utc>2026-07-07T15:00:00Z</current_date_utc>
```


Same information, different position. Now it changes in the part of the request that was always going to change, and the prefix in front of it stays byte-stable.


## Static first, dynamic last


Once we saw the timestamp, we went looking for everything else that was in the wrong place. The rule we landed on: static content first, dynamic content last.


The biggest offender was the database schema. It had been living in the per-turn context message, getting re-billed at full price every turn even though it’s stable for the whole chat. We moved it into the system instructions, so it’s now part of the cached prefix:


```text
// The schema is stable for the duration of a chat, so placing it in the
// system prompt keeps it inside the cached prefix instead of being
// re-processed at full input price on every turn.
```


We still rebuild it from our entities cache on each generation, so a schema change between turns gets picked up. That costs one cache invalidation, once, instead of paying for the whole schema on every turn forever.


## Advancing the cache breakpoint


OpenAI caches prefixes automatically. Anthropic makes you mark them: you set a` cache_control` breakpoint on a content block, and everything up to it gets cached. You get 4 breakpoints per request.


Our agents run in a loop. The model calls a tool, we run it, we append the result, we call the model again. Each iteration only adds messages to the end, so the whole previous request is a valid prefix of the next one. We put an “advancing” breakpoint on the last message of every Anthropic request so the next iteration reads the entire previous turn from cache:


```text
const   ANTHROPIC_CACHE_CONTROL   =   {
anthropic: { cacheControl: { type:   "ephemeral"   } },
};
```


We use 3 of the 4 breakpoints: one after the system instructions, one at the end of the chat history, and this advancing one on the final message. The history breakpoint is a domain-level flag (` cacheBoundary` ) that our Anthropic adapter translates into a real` cache_control` block, so the rest of our code doesn’t need to know anything about a specific provider’s caching API.


## Fetch it when you need it


Reordering fixes where things live. It doesn’t help with things that shouldn’t be in the prompt at all.


Our dashboard agent was inlining the full SQL of every chart on the dashboard into its context. Handy when the agent needs to see a query, useless the other 90% of the time, and it roughly doubled the size of that context block. So we switched it to just-in-time retrieval: list the charts by id, name, and type, and let the agent pull a specific chart’s SQL with the` get_chart` tool when it actually needs it.


```text
config  : {
includeChartIds  :   true  ,
includeSqlQueries  :   false  ,   // fetched on demand via get_chart
}
```


Smaller stable context, and the agent still gets the full query the moment it asks for one.


## Trimming old tool output


The other thing that bloats a long chat is tool results. A query that returns a few thousand rows produces a big blob of JSON, and after 10 turns you’re carrying 10 of those around. The model almost never needs the full output from many turns ago, and if it does, it can just run the query again.


So we trim them. Tool outputs older than the last 2 user turns get cut down to a head snippet plus a notice:


```text
const   DEFAULT_OPTIONS   =   {
keepRecentUserTurns:   2  ,
maxOutputLength:   2_000  ,
headLength:   500  ,
};
```


The important detail is that this is deterministic. Once a message falls outside the recent window, its trimmed bytes never change again. If we truncated based on, say, the current token budget, the older messages would shift around per turn and we’d be back to busting the cache. Stable trimming keeps the prefix cacheable while still shrinking it.


## Summarizing instead of sliding


We used to cap history with a sliding window: keep the last 20 messages, drop the rest. That’s terrible for caching. Dropping the oldest message every turn shifts the whole history by one, so the prefix changes every turn by construction. It also just forgets whatever the user said early in the chat.


We replaced it with compaction. A background job summarizes the older part of a long chat into a` ChatCompaction` row, and history loading reads only the messages after the last compaction point, with a byte-stable summary block prepended:


```text
export   const   COMPACTION_TRIGGER_MESSAGE_COUNT   =   40  ;
export   const   COMPACTION_KEEP_RECENT_MESSAGES   =   16  ;
```


Once a chat crosses 40 messages since its last compaction, we fold the older turns into a summary and keep the recent 16 verbatim. The job runs with a` singletonKey` of the chat id so a chat is never compacted twice at once, and it re-checks the threshold before doing any work. The summary is stable text, so it caches like everything else, and the model keeps the early context instead of losing it.


## Watching it work


None of this is worth much if you can’t tell whether it’s working, so we added per-provider, per-agent, per-model metrics for input tokens split into total, cache-read, and cache-write, plus a` cache_read_ratio` distribution:


```text
ai.provider.input_tokens.total
ai.provider.input_tokens.cache_read
ai.provider.input_tokens.cache_write
ai.provider.cache_read_ratio
```


The ratio is the one we watch. Before, cache reads were basically zero because of the timestamp. After, a multi-turn chat spends most of its input tokens on cache reads at a fraction of the price. It also doubles as a regression alarm: if someone adds a volatile value back into the prefix, the read ratio drops and we’ll see it.


## What we took away


A few things we’ll keep in mind next time we build an agent.


1. **Cache matching is prefix-exact.** One changing token near the front of the request invalidates everything after it. Order your prompt static-first, dynamic-last, and be ruthless about what counts as static.
2. **Timestamps are the classic trap.** A per-second clock in a system prompt looks harmless and quietly turns every request into a cache miss. If the model needs the time, put it at the end.
3. **Keep the prefix stable, not just small.** Deterministic trimming and summarization shrink context without shifting the bytes the provider already cached. Anything that resizes per turn defeats the point.
4. **Fetch on demand.** Inlining data the agent rarely needs is pure overhead. A cheap list plus a tool call beats a fat context block.
5. **Measure the cache read ratio.** It’s the single number that tells you whether your prompt is actually cacheable, and it catches regressions the moment they land.


If you liked this, we’ve written up a few more from the same stack:[how 97 Replicache subscriptions stalled our dashboard editor](https://www.basedash.com/blog/how-97-replicache-subscriptions-stalled-our-dashboard-editor) ,[what we learned running background jobs on Postgres](https://www.basedash.com/blog/what-we-learned-running-background-jobs-on-postgres) , and[what was killing our healthy Kubernetes pods](https://www.basedash.com/blog/what-was-killing-our-healthy-kubernetes-pods) .


And if you want to see what all this context engineering is in service of,[Basedash](https://www.basedash.com/) is an AI-native BI platform that turns your database into dashboards and answers without you writing SQL.
