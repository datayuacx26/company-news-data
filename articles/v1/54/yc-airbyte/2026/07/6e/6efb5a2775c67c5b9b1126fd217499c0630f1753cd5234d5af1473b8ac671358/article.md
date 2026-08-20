---
schema_version: "1.0.0"
document_id: "6efb5a2775c67c5b9b1126fd217499c0630f1753cd5234d5af1473b8ac671358"
company_key: "yc-airbyte"
company: "Airbyte"
source_id: "yc-airbyte-news-import-0f166651abb1"
canonical_url: "https://airbyte.com/blog/semantic-search-for-gong-and-linear"
published_at: "2026-07-15T16:17:00+00:00"
first_seen_at: "2026-07-21T23:17:10.236435+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:4d8b36c36ebca4c41988d5c6e81e6c992ad269b3eeae0c84b996b8e2c70655aa"
---

# Enable Semantic Search for Gong and Linear

Your agents can now search Gong call transcripts and Linear issues by meaning, not keywords. Ask for all calls in which a customer expressed concerns about pricing, or all issues in which users reported an OAuth failure, and the agent will find the relevant passages from those calls or issues, even if the people involved never used those exact words.


That's semantic search. It's live today in Airbyte Agents for Gong call transcripts and Linear issue descriptions and comments.


## **Why keyword search falls short**


Call transcripts and issue threads are long, messy, and unpredictable. A customer worried about cost might say, "this feels steep for what we're getting", or "I'd have to fight for budget on this." A user reporting a performance bug might write "the dashboard takes forever after the update" without ever typing "latency."


Keyword searches miss all of it.


Filters and exact matches work well for structured records, a deal stage, an issue status, an account owner. They break down when the answer is buried somewhere in 45 minutes of conversation or a 30-comment thread.


Semantic search overcomes this limitation. Airbyte embeds your question and compares it against the meaning of the transcript or issue text, then returns the most relevant passages, ranked by how closely they relate to your question, with the surrounding context attached so your agent knows who said what.


## **What you can do with it**


Connect Gong and Linear to Airbyte Agents once, and every agent you build can answer questions like:


- Which customers have pushed back on the contract terms we've quoted to them?
- Can we find moments during sales conversations when prospects mentioned that they're interested in a particular competitor?
- Which open issues describe the same login problems that customers mentioned in the calls?
- What did members of the engineering team say about the migration process for the new database in the comments on the relevant issues?


That third question is the most interesting one. Because Gong and Linear have access to the[Context Store](https://docs.airbyte.com/ai-agents/concepts/context-store) , a single agent can surface what customers said on calls alongside what engineering tracked in issues. Feedback on a sales call becomes a link to the bug that caused it.


All searches happen within the Context Store, Airbyte's pre-indexed layer of your operational data. Your agent doesn't need to crawl the Gong or Linear APIs record by record. Instead, it searches all of your calls and issues at once and pulls back only the passages that matter. In an open-source benchmark of Airbyte Agents versus the native APIs, agents querying Gong through Airbyte used up to 80% fewer tokens, and agents querying Linear used up to 75% fewer.


## **No prompt engineering required**


You don't need to engineer your prompts to use semantic search. When semantic search is available for a connector, your agent automatically chooses the search method that best fits your question: semantic search for information based on meaning, structured search for information based on exact filters.


If you're building your own agent with the SDK or API, you can also call semantic search directly. You get fine-grained control over aspects like how much surrounding context to include with each matched passage, whether you want one best passage per call or issue or several, and any filters to apply to the search. The[Context Store documentation](https://docs.airbyte.com/ai-agents/concepts/context-store) covers the full request shape.


## **More connectors on the way**


Gong transcripts and Linear issues and comments are where semantic search starts, not where it ends. More connectors and fields will gain meaning-based search over time.


[Try Airbyte Agents free](https://airbyte.com/) or read the[docs](https://docs.airbyte.com/ai-agents/concepts/context-store) to learn more.
