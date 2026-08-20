---
schema_version: "1.0.0"
document_id: "d2b0f018cb277590c947ccc6a767f954acbbcf70afe6bdea7d840f8279ccf3f4"
company_key: "yc-zeroentropy"
company: "ZeroEntropy"
source_id: "yc-zeroentropy-news-import-2ae9b0afcf30"
canonical_url: "https://zeroentropy.dev/articles/reddit-reranker-chrome-extension/"
published_at: "2026-04-09T00:00:00+00:00"
first_seen_at: "2026-07-22T21:03:20.034441+00:00"
fetched_at: "2026-07-28T22:15:57.305389+00:00"
content_hash: "sha256:0b5f97bff46ef58037b63027b75c37ff30792414f6d077a42ec3e84d806ea1b6"
---

# Reranking Reddit: What Happens When You Sort Comments by Relevance Instead of Karma

TL;DR


- Reddit and Hacker News sort comments by upvotes. Upvotes reward being first and echoing consensus — not being informative.
- We analyzed 66 million comments across 2,147 subreddits: **the best answer has 1 upvote 32% of the time** . The first comment gets 4x more karma regardless of quality.
- **Reddit Reranker** is a free Chrome extension that re-sorts comments by relevance using zerank-2, an[instruction-following reranker](https://www.zeroentropy.dev/concepts/instruction-following-reranker/) . One click, and the most useful answers surface.
- Available now:[Chrome Web Store](https://chromewebstore.google.com/detail/reddit-reranker/jgpnceiaefjepfgleiplmoaajhmgkddj)


# Karma Is a Popularity Contest


Reddit’s ranking algorithm is older than most of its users. A comment’s position is determined by upvotes, adjusted for time — which means the first reply with broad appeal wins, every time. Quality is incidental.


This isn’t speculation. We looked at the data.


Across 66 million comments in 2,147 subreddits, we scored every comment for relevance to its parent post using zerank-2, then compared the[reranker](https://www.zeroentropy.dev/concepts/reranker/) ’s ordering to Reddit’s native karma ordering. The “best answer” in the numbers below is the comment zerank-2 ranks first — a defensible proxy for substantive relevance, not a human-labeled gold standard. On most threads, that comment is not the one at the top.


**The best answer has exactly 1 upvote 32% of the time. The first comment posted receives 4x more karma than the median reply, regardless of quality.**


The sorting is broken. Karma rewards speed, social proof, and tone-matching. It does not reward being correct, being thorough, or being relevant.


So we built a tool that does.


## Reddit Reranker


Reddit Reranker is a Chrome extension that re-sorts comments on Reddit and Hacker News using[zerank-2](https://www.zeroentropy.dev/articles/zerank-2-advanced-instruction-following-multilingual-reranker) , ZeroEntropy’s reranker. It reads the comments on the page, scores each one for relevance to the post, and reorders them. One click.


Left: default Reddit sorting. Right: the same thread after reranking. The top answer changes.


It works on:


- **old.reddit.com** — the classic interface
- **[www.reddit.com](http://www.reddit.com/)** — new Reddit
- **news.ycombinator.com** — Hacker News


Install it, paste your API key, and click Rerank. That’s it.


## How It Works


The extension extracts every comment on the page and sends them to zerank-2 as documents, with the post title and body as the query. zerank-2 returns a relevance score for each comment — a[pointwise](https://www.zeroentropy.dev/concepts/pointwise-scoring/) “how relevant is this to the post” rather than “how popular is this.”


Top-level comments get reordered by score. Threaded replies stay grouped with their parent, so conversation structure is preserved. Every comment gets a color-coded relevance badge (yellow to blue) so you can see the scores at a glance.


Every comment gets a relevance badge. Yellow is low, blue is high. You can see immediately which replies are substantive.


The whole operation takes 1–3 seconds on a typical thread.


## Six Ways to Read a Thread


Not every rerank should optimize for the same thing. Sometimes you want the most informed reply; sometimes you want the jokes; sometimes you want to see what’s controversial.


Reddit Reranker ships with six scoring modes:


Mode What it surfaces


**Relevant** Best overall responses to the post


**Informed** Expert replies with sources, citations, depth


**Joke** Humor, sarcasm, memes, meta-commentary


**Inflammatory** Controversial, combative, polarizing takes


**Actionable** Practical advice, step-by-step instructions


**Custom** Your own natural-language query — score by whatever you want


Each mode steers zerank-2’s notion of relevance by prepending an instruction to the query. This is the same[instruction-following capability](https://www.zeroentropy.dev/articles/zerank-2-advanced-instruction-following-multilingual-reranker) that makes zerank-2 useful in production[RAG](https://www.zeroentropy.dev/concepts/rag/) pipelines — applied here to a Reddit thread. For the mechanics of writing instructions that reliably shift a reranker’s behaviour, see[prompting best practices for instruction-following rerankers](https://www.zeroentropy.dev/articles/prompting-best-practices-for-instruction-following-rerankers) .


The popup UI. Select a mode, click Rerank.


## Karma vs. Relevance: The Stats Panel


After reranking, the extension shows a statistics overlay with a scatter plot of karma vs. relevance score for every comment in the thread. The x-axis is log-scaled karma; the y-axis is the zerank-2 relevance score. A trend line and[Pearson correlation](https://www.zeroentropy.dev/concepts/pearson-correlation/) coefficient (r) are computed on the fly.


Karma vs. relevance for a real thread. The correlation is weak — high-karma comments are not reliably the most relevant ones.


Across the 2,147-subreddit study, the median Pearson correlation between log-karma and zerank-2 relevance was **r =** — weakly positive on average, often near zero, and occasionally negative on threads where the top-karma reply is a one-line joke. High-karma comments are not reliably the most relevant. This is the whole point.


The panel also shows a histogram of relevance score distribution, mean and median scores, and a breakdown of high-confidence (≥ 70%) vs. low-confidence (< 30%) comments.


## Why This Matters Beyond Reddit


Reddit Reranker is a consumer demonstration of what zerank-2 does in production. The same model that re-sorts these comments is used by enterprise teams to rerank search results, filter RAG[context windows](https://www.zeroentropy.dev/concepts/context-window/) , and score document relevance across legal, medical, financial, and technical domains.


If you’ve been evaluating rerankers for your own pipeline, this is a way to see zerank-2 work on messy, real-world, adversarial text — not curated[benchmark data](https://www.zeroentropy.dev/concepts/eval-set-quality/) . Reddit comments are noisy, sarcastic, off-topic, and full of context that only humans would catch. It’s a good stress test.


## Get It


#### Install the extension


Grab it from the[Chrome Web Store](https://chromewebstore.google.com/detail/reddit-reranker/jgpnceiaefjepfgleiplmoaajhmgkddj) .


#### Get a free API key


Sign up at[dashboard.zeroentropy.dev](https://dashboard.zeroentropy.dev/api-keys) . Keys start with` ze-` .


#### Rerank something


Open any Reddit or Hacker News thread. Click the extension icon. Click Rerank.


### Try It


Reddit Reranker is free and open source.


[→ Chrome Web Store Install the extension](https://chromewebstore.google.com/detail/reddit-reranker/jgpnceiaefjepfgleiplmoaajhmgkddj)[→ Chrome Web Store Add Reddit Reranker to Chrome](https://chromewebstore.google.com/detail/reddit-reranker/jgpnceiaefjepfgleiplmoaajhmgkddj)[→ API Key Free ZeroEntropy API key](https://dashboard.zeroentropy.dev/api-keys)[→ Discord Join the community](https://discord.gg/5mkQCTnmY9)


**Documentation** :[docs.zeroentropy.dev](https://docs.zeroentropy.dev/)


**zerank-2 deep dive** :[Introducing zerank-2](https://www.zeroentropy.dev/articles/zerank-2-advanced-instruction-following-multilingual-reranker)


Built by ZeroEntropy. The reranker scores how relevant each comment actually is — not how early it was posted or how funny it is.
