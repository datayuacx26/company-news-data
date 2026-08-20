---
schema_version: "1.0.0"
document_id: "132f777a3126ed5540b9638011c260ff70a54e128fa3e2a2f4bf9d8003827996"
company_key: "yc-decisional-ai"
company: "Decisional AI"
source_id: "yc-decisional-ai-news-import-6844c8207cfa"
canonical_url: "https://www.decisional.com/blog/coding-assistant-wiped-mac"
published_at: "2025-11-08T00:00:00+00:00"
first_seen_at: "2026-07-24T04:01:29.236634+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:4f956801f251062b4870ee2d6f7ef727bf2fe9b9e11835c2bf0d5e634b07c09a"
---

# Coding Assistant Wipes Our Mac - What We Learned

An AI coding assistant accidentally deleted all files on our Mac due to a path confusion error. Without strong guardrails, convenience can outweigh safety in a single command.


## What Happened


We asked Composer to copy a file to "the correct location." It moved the file and then attempted to delete the incorrect file. However, it got confused and somehow ran a delete on the ~ path which ended up deleting everything on our Mac.


The AI coding assistant had created a config folder for MCP under /home/user/project/~/.cursor/mcp.json. When we asked it to fix the location, the cascading series of commands resulted in catastrophic data loss.


## The Path Confusion


The root cause was a path confusion error. The AI:


1. Created a config in the wrong location (nested ~ in the path)
2. When asked to correct it, attempted to clean up
3. Misinterpreted the cleanup target as the home directory
4. Executed rm -rf on what it thought was cleanup, but was actually ~


This is a failure mode that's particularly insidious because each individual step seemed reasonable.


## Lessons Learned


Despite this incident, we remain fans of AI coding assistance. But this is a reminder of critical safety considerations:


- **Always verify destructive commands** : Any rm, delete, or move operation should be inspected
- **Sandbox when possible** : Use containers or VMs for experimental AI coding sessions
- **Backup religiously** : Time Machine saved us from complete disaster
- **Review the full command** : Don't just approve the intent, read the actual command


## Guardrails Matter


Without strong guardrails, convenience can outweigh safety in a single command. The AI was trying to be helpful—it saw a mess it created and tried to clean it up. But without proper constraints on what paths can be deleted, helpfulness became destruction.


We didn't see or hear of any examples in our community of terrible things happening before this. But now we've learned firsthand that these failure modes exist and they can be severe.


## Moving Forward


We're now significantly more careful about:


1. **Path operations** : Any command touching ~ or root paths gets extra scrutiny
2. **Cleanup operations** : We manually handle cleanup rather than asking AI to do it
3. **Sandboxing** : More aggressive use of Docker and VMs for AI coding sessions
4. **Audit trails** : Logging all AI-executed commands for review


The promise of AI coding assistants is real. But so are the risks. We wanted to share this so others can learn from our experience rather than having to live through it themselves.
