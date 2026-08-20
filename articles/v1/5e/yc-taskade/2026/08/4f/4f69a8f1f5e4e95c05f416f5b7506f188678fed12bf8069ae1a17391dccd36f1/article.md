---
schema_version: "1.0.0"
document_id: "4f69a8f1f5e4e95c05f416f5b7506f188678fed12bf8069ae1a17391dccd36f1"
company_key: "yc-taskade"
company: "Taskade"
source_id: "yc-taskade-rss-a662ed9a0141"
canonical_url: "https://www.taskade.com/blog/tool-use-history"
published_at: "2026-08-03T00:00:00+00:00"
first_seen_at: "2026-08-03T00:44:50.835884+00:00"
fetched_at: "2026-08-03T04:26:23.705919+00:00"
content_hash: "sha256:1ae9185ee2f49f2b7fc0f8c6b7f3866a21cb76792ada00c2df2b3ada62eb5286"
---

# How LLMs Got Hands: The History of Tool Use and Function Calling (2026)

```text
TURN 1   -> model      role: user
content: "What was our Q3 revenue?"
tools[]     [{ name: "query_db",
input_schema: { sql: string } }] TURN 2   <- model      stop_reason: tool_use             tool_call   { id: "call_01",                           name: "query_db",                           arguments: { sql: "SELECT sum(amount) ..." } }             NOTE        The model produced TEXT. Nothing has run yet.
TURN 3   -> harness    validate args -> check permission -> set timeout                         -> execute -> catch errors -> redact             tool_result { tool_use_id: "call_01",                           content: "418293.55" }             NOTE        This is the ONLY step that touches the world.
TURN 4   <- model      stop_reason: end_turn             content     "Q3 revenue was $418,293.55."
```
