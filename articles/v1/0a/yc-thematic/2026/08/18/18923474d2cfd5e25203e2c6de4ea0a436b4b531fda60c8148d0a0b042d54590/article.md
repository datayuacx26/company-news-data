---
schema_version: "1.0.0"
document_id: "18923474d2cfd5e25203e2c6de4ea0a436b4b531fda60c8148d0a0b042d54590"
company_key: "yc-thematic"
company: "Thematic"
source_id: "yc-thematic-news-import-d60ff6e474dd"
canonical_url: "https://getthematic.com/insights/find-specific-customer-feedback"
published_at: null
first_seen_at: "2026-08-07T21:50:28.388550+00:00"
fetched_at: "2026-08-07T21:50:30.457103+00:00"
content_hash: "sha256:52e7cadbf6f270840b7d99e62095140a99f1fdc399bb56764d37b40f964fd2c2"
---

# How Do You Find Specific Customer Feedback Across Millions of Comments?

You don't need a summary. You need eleven comments.


Every mention of a named competitor. Complaints from enterprise accounts in one region, on one plan tier, during one release window. Comments that raise two problems at once, because those are the ones that explain why the score moved. The corpus is millions of comments. The search box gives you either four results or forty thousand.


The answer is to stop searching for a word and start composing a query. In Thematic, a retrieval question gets answered by layering three things: a text predicate built with boolean operators, metadata filters built from the columns you send with your feedback, and a theme filter that catches the comments whose wording you'd never have guessed. The result opens in the[Feedback tool](https://getthematic.com/analyze) as full comments you can sort, read, and export. That's retrieval. A keyword is a guess.


Below: why keyword search misses more than most teams realize, what a retrieval question requires, how Thematic composes one, and why asking an AI assistant to find the comments is the wrong tool for this job. This is about finding one specific slice, not[analyzing feedback at scale](https://getthematic.com/insights/how-analyze-customer-feedback-at-scale) in aggregate.


## Why does keyword search miss so much customer feedback?


Because it misses far more than the searcher believes, and the gap has been measured for forty years.


The foundational study is[Blair and Maron's 1985 work](https://dl.acm.org/doi/10.1145/3166.3197) in *Communications of the ACM* . Skilled searchers working a corpus of roughly 350,000 pages set a target of retrieving at least 75% of the relevant documents, and believed they'd reached it. Measured against the full set, they'd retrieved under 20%. The pattern held: in the[NIST TREC 2008 Legal Track](https://trec.nist.gov/pubs/trec17/papers/LEGAL.OVERVIEW08.pdf) , the average recall of a final negotiated boolean query was 33% for highly relevant documents.


Now picture a CX analyst searching "cancel" to find churn feedback. The mechanics of the miss are ordinary:


- **Vocabulary mismatch.** Customers write "charged twice," "double billed," "took the money out twice," and "two payments came out." Searching "double billed" finds one of the four.
- **Word-form precision.** Matching in Thematic's response-contains filter is on full words, so "suit" doesn't match "suitcase." Useful, but word forms become your problem.
- **Polarity blindness.** "Cancel" matches "I tried to cancel for an hour" and "I nearly canceled, but your support team saved the account." Opposite findings, same result set.
- **No sense of what's missing.** A keyword search returns what it found. It can't show you the relevant comments it didn't match, which is why confidence outruns recall.


The problem isn't that keyword search returns nothing. It's that it returns something, and something looks like an answer.


## What does a retrieval question actually require?


Three layers, applied together. Any one alone leaks.


- **A text predicate with logic.** Not one word. Terms with explicit AND, OR, and NOT relationships, so "billing but not refunds" becomes a query instead of a manual pass over results.
- **Metadata constraints.** Who said it, when, on what plan, in which region, at what score. This turns "comments about billing" into "comments about billing from enterprise accounts in the EU last quarter."
- **Theme structure.** A layer that groups comments by what they're about rather than[which words they used](https://getthematic.com/insights/word-clouds-vs-ai-text-analysis) , so all four phrasings of "charged twice" land in one place whether or not you anticipated them.


Order matters. Filter to a defensible population first, then search within it, and the result is reproducible. Search the whole corpus and eyeball the output, and it isn't.


## Where do most feedback tools fall short?


Most ship a search box and stop. The gaps show up fast under pressure:


- One search field with no operators, so multi-term logic means running several searches and merging them by hand.
- Filters restricted to a short vendor-defined list rather than built from the metadata you actually collect.
- No way to combine text logic and metadata constraints in a single query, which pushes the real work into a spreadsheet.
- No theme layer, which puts you back to guessing vocabulary.


A useful demo test: *"Show me every comment from enterprise accounts in the EU, last quarter, mentioning billing but not refunds, tagged to the onboarding theme. Now export it, and show me how I reopen this query next month."* Watch whether that's one query or a forty-minute workaround.


## How does Thematic compose a retrieval query?


**Boolean text search.** Thematic's[response-contains filters](https://help.getthematic.com/article/128-how-to-use-filters) search response text using a simple keyword search or advanced logical queries built with the capitalized operators AND, OR, and NOT, plus brackets. AND requires both terms in the response, though not adjacent. OR requires at least one. NOT excludes. Two behaviors matter before you write a query: matching is on full words and ignores case, and evaluation groups ORs before ANDs, so` a OR b AND c` reads as` (a OR b) AND c` . Use brackets when you mean something else.


**Metadata filters.** Thematic builds filters from the additional columns supplied with each response: scores, dates, demographics, free text, and themes. Score filters work on individual values or custom buckets, such as the net promoter score (NPS) split of Detractor 0 to 6, Passive 7 to 8, Promoter 9 to 10. Free-text filters support logic groupings and numeric conditions like ">=1 touchpoints." A custom text-search filter can point at any column, including specific IDs. The ceiling is the metadata you send with your feedback, not a fixed vendor list. Adding or reordering filters is a configuration step handled with your Thematic contact.


**Theme filters.** A[themes filter](https://getthematic.com/insights/new-filters) cuts to the comments tagged to a theme regardless of wording. This is the layer that catches phrasings you didn't anticipate, and it's why[theme quality](https://getthematic.com/insights/how-to-build-a-customer-feedback-taxonomy) is a retrieval concern, not just a reporting one.


**Comparison filters.** A blue primary filter sets the population across all tools. An orange "Compare with" filter puts a second segment beside it. Both affect every visualization, summary, and export, so the comparison belongs to the query rather than a separate report.


**The Feedback tool.** This is where a composed query lands: a comment-level view showing each comment in full with its themes and metadata columns. Sort by timestamp, score, sentiment, or any metadata field. Use "Show comments about" to focus a theme, then add themes to find combinations. Users with download permissions can export what's displayed.


**Standing queries.** When the question recurs, a Comment Matching workflow alerts you as new comments match your filters and conditions, by email or Slack, with an optional CSV of the matches attached. Trigger it immediately, weekly, or monthly.


## Why is "just ask the AI" the wrong tool for exact retrieval?


Because asking and retrieving are different jobs, and Thematic is explicit about which surface does which.


Thematic Answers is strong at explanation and measurement. It interprets a natural-language question, runs up to 10 tool calls to investigate, and returns a cited, multi-part answer. Supported question types cover comparing two segments, retrieving a score, explaining what caused a score to change, and score impact.


Retrieval isn't on that list. "Show me the comments for X" is a documented unsupported question type in Answers. And in Agentic Answers, applied filters are treated as hints rather than strict limits. That's the right design for an investigative agent and the wrong guarantee for a defensible record set.


Use Thematic Answers when Compose a filter query when


You're asking why a score moved You need the exact comments behind it


You want a narrative explanation with citations You need a list you can export and hand over


Approximate scoping is fine The population must be exact and defensible


You're exploring and following a thread You need to reproduce the same set next quarter


## What does this look like at enterprise scale?


Atlassian included over 1 million community questions and comments in its feedback analysis, on top of roughly 60,000 pieces of feedback received every month across support, community posts, and in-app messages. In Atlassian's own description, teams could search, filter, and analyze feedback through dimensions like keyword, product, channel, component, domain, theme, sentiment, and date, then drill right down to the raw feedback. Atlassian selected Thematic after evaluating 36 vendors and demoing nine.


Community Health System, a not-for-profit healthcare network serving California's central San Joaquin Valley, shows the narrower version. Its annual employee engagement survey collected open-ended feedback from staff across 250 departments. The team applied a department filter and reused one structure. That produced 250 standardized one-page reports in three days, saving over 160 hours per cycle. Isolating one department's verbatims out of 250 is what made per-department reporting possible at all.


At Mitre 10, a three-person insights team handled 20,000 verbatim comments a month across 84 stores, and was regularly asked for deep dives on specific locations, often turning them around within minutes.


## A buyer's checklist for feedback retrieval


1. Does text search support AND, OR, and NOT with brackets, or only single terms and phrases?
2. How does it handle word forms and case, and is that behavior documented?
3. Are filters built from the metadata I supply, or limited to a vendor-defined list?
4. Can I combine a text predicate, metadata constraints, and a theme filter in one query?
5. Can I export the exact result set and reopen the query later without rebuilding it?
6. When I ask the AI assistant for specific comments, is that supported or best-effort?


## The short answer


To find specific customer feedback across millions of comments, compose a query instead of guessing a keyword: a boolean text predicate, metadata filters built from the columns you send with your feedback, and a theme filter for the wording you didn't anticipate. Thematic layers all three and opens the result in the Feedback tool as full comments you can sort and export.


One test on your own data. Take a question you actually get asked, express it as a single query, and see whether you can reproduce the same result set a month later.
