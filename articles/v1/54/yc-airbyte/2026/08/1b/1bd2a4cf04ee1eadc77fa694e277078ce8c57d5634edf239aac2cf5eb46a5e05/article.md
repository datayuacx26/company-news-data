---
schema_version: "1.0.0"
document_id: "1bd2a4cf04ee1eadc77fa694e277078ce8c57d5634edf239aac2cf5eb46a5e05"
company_key: "yc-airbyte"
company: "Airbyte"
source_id: "yc-airbyte-news-import-0f166651abb1"
canonical_url: "https://airbyte.com/blog/semantic-search-google-drive-and-granola"
published_at: "2026-08-05T09:47:00+00:00"
first_seen_at: "2026-08-05T12:37:30.974140+00:00"
fetched_at: "2026-08-05T12:37:32.872151+00:00"
content_hash: "sha256:1eef4446e5cd0268796c509af00dd2706a7b308aaa2ca7c41de8ed5f7284bc44"
---

# Semantic Search Expands to Google Drive and Granola

Recently, we shipped[semantic search for Gong and Linear](https://airbyte.com/blog/semantic-search-for-gong-and-linear) .


Today, semantic search expands to **Google Drive** and **Granola** . We’re especially proud of our work bringing this to Google Drive. It’s another step up when it comes to semantic search.


Gong and Linear store structured records with long text fields, which are simpler to parse from a meaning perspective. Comparatively, Google Drive stores files, of all kinds and often in completely disorganized folders.


Semantic search now handles all of that messy, context-rich data, and is available to our users today.


## **Two kinds of unstructured text**


With Gong and Linear data, semantic search is simpler. The text is long and messy, but the container is structured. It’s clear what entity it belongs to, and the metadata is similarly easy to comprehend.


File content is different. A Google Drive folder might hold memos, images, slide decks, and quite a lot of it could be junk. There's no clear schema or entity type to guide an agent to relevant information.


The useful information is locked inside the file itself, and the only way to find it is to read the file.


Airbyte handles this as follows:


1. Extracting the text content from each file
2. Splitting it into passages (paragraphs, sections, or chunks depending on the document structure)
3. Embedding each passage as a vector, and storing it in the Context Store.
4. When your agent searches, Airbyte embeds the question the same way and returns the passages whose vectors are closest in meaning.


This gives us the same result as a Gong or Linear search, a ranked list of passages with relevance scores and surrounding context. It’s just that the context snippets represent a much wider range of data types.


All of this happens within the Context Store. Your agent doesn't crawl the Google Drive API file by file or paginate through Granola's meeting list. It searches pre-indexed content and gets back only the passages that matter.


## **Why files matter for agents**


Historically, structured data was the primary feed for enterprise analytics. Airbyte got its start moving vast amounts of these structured records for organizations. Obviously, structured data is incredibly valuable for enterprise agents, and always will be.


But a huge share of organizational knowledge doesn't live in structured records. With traditional extraction methods and pipelines, this knowledge remains effectively invisible to agents.


Not long ago, most people would dump this data into a vector database and build a RAG pipeline. That works for search, but it disconnects documents from the rest of the agent's context. The agent can find the relevant paragraph, but it can’t relate that information to your business knowledge. That’s effectively the whole point of Airbyte Agents. Our goal is to support agents that can search across the connective tissue of an entire organization’s knowledge.


We’re not so bold to claim that this challenge has been solved, of course!


But Google Drive in the Context Store alongside Gong, Linear, and the rest of Airbyte's agent connectors, our vision is one step closer to completion.


## **What Google Drive semantic search unlocks**


Connect Google Drive to Airbyte Agents and your agents can search the contents of your files by meaning. In fact, you don’t even need the right words to find what you’re looking for, as it intuits true meanings. Here are some tasks you can ask it to execute:


- "Find the sections in our product docs that describe how we handle SSO"
- "Which documents in the shared drive mention our pricing model?"
- "Find the parts of the Q2 board deck that discuss expansion revenue"


The connector surfaces only relevant results, not full file dumps that blow up your context window.


The Google Drive connector, like most Airbyte Agent connectors, also contains the full range of file operations, such as:


- listing files
- downloading content
- exporting Google Docs as PDF or Sheets as CSV
- viewing permissions
- Comments
- revision history
- creating, uploading, moving, and deleting files.


This is critical, as agents can now find meaning in unstructured data and act on that inference.


## **Use meeting notes for agent context with the Granola connector**


Granola is an AI-powered meeting notes platform. It records meetings, generates structured summaries, and stores transcripts with attendee and calendar metadata attached.


Meeting notes are among the richest and most valuable sources of context in most organizations. Every meeting contains decisions and open items that inform the smaller and larger daily tasks feeding into an organization’s strategy and success.


With semantic search, your agents can now search Granola meeting notes by meaning, not just keywords. This reduces the dependence on human interpretation, and provides autonomy to agents. If we’re approaching a world with agent coworkers, they have to be able to read and understand meeting minutes like a human can.


## **No prompt engineering, no separate tools**


You don't need to learn a new API or write different prompts to use semantic search on the Airbyte web app or the MCP server. When semantic search is available for a connector, the agent automatically chooses the right search method based on your question: semantic for meaning-based retrieval, structured for exact filters.


If you're building with the SDK or API, you can call semantic search directly with fine-grained control: how many passages to return, how much surrounding context to include, and whether to apply metadata filters alongside the meaning-based search. The[semantic search documentation](https://docs.airbyte.com/ai-agents/concepts/semantic-search) covers the full request shape.


## **Four connectors, one search layer**


Semantic search now works across four connectors: Gong (call transcripts), Linear (issue descriptions and comments), Google Drive (file contents), and Granola (meeting notes). Each represents a different category of unstructured text. Together, they cover the main places where organizational knowledge lives outside of structured records.


More connectors and fields will gain semantic search over time. If you want to see which connectors support it today, check the connector's documentation page. An entity supports semantic search when a Semantic Search action appears in its action list.


[Try Airbyte Agents free](https://airbyte.com/) or read the[semantic search docs](https://docs.airbyte.com/ai-agents/concepts/semantic-search) to get started.
